# Tema 10.- El SGBDR Oracle. Opción "Partitioning" y Enterprise Manager.

## 1. Introducción

En entornos departamentales modestos, las tablas de una base de datos suelen contener unos pocos miles o decenas de miles de registros, lo que facilita su administración y consulta. Sin embargo, en el ámbito de las Administraciones Públicas de gran escala —sistemas tributarios nacionales, registros de la Seguridad Social, historiales clínicos de un Sistema Nacional de Salud— las tablas pueden alcanzar cientos de millones o incluso miles de millones de filas, configurando lo que Oracle denomina **VLDB (Very Large DataBases)**.

Una tabla monolítica de un terabyte de "Declaraciones de la Renta" presenta cuellos de botella críticos: las consultas recorren innecesariamente millones de registros históricos, las operaciones de mantenimiento (reconstrucción de índices, purga de datos prescritos) requieren tiempos de ejecución inaceptables, y las copias de seguridad se prolongan durante horas.

Para abordar estos desafíos, Oracle ofrece la opción propietaria **Oracle Partitioning**, que permite descomponer tablas e índices de gran tamaño en fragmentos más pequeños y manejables sin modificar el código de las aplicaciones. Complementariamente, **Oracle Enterprise Manager (OEM)** proporciona la plataforma centralizada de administración y monitorización que permite gestionar eficientemente toda la infraestructura Oracle.

## 2. Oracle Partitioning: Concepto y Principio de Transparencia

### 2.1. Definición

El **Particionamiento** consiste en dividir físicamente una tabla o un índice de gran tamaño en múltiples segmentos independientes denominados **particiones**. Cada partición almacena un subconjunto de las filas de la tabla, determinado por un criterio de distribución definido por el DBA (fecha, región, rango numérico, etc.).

### 2.2. Principio de transparencia

La característica fundamental del particionamiento en Oracle es su **transparencia lógica**: aunque la tabla esté físicamente dividida en múltiples particiones, lógicamente sigue comportándose como una única tabla. Las aplicaciones y los usuarios ejecutan sus sentencias SQL habituales (`SELECT`, `INSERT`, `UPDATE`, `DELETE`) sin necesidad de conocer ni referenciar las particiones subyacentes.

**Ejemplo:** Una tabla `MULTAS_TRAFICO` con 1.000 millones de registros correspondientes a diez años puede particionarse por año. Un programador ejecuta:

```sql
SELECT * FROM MULTAS_TRAFICO WHERE fecha_multa BETWEEN '01/01/2025' AND '31/12/2025';
```

El código no hace referencia a ninguna partición. Oracle determina automáticamente qué partición contiene los datos de 2025 y accede exclusivamente a esa partición.

## 3. Beneficios del Particionamiento

### 3.1. Partition Pruning (Poda de particiones)

Es la mejora de rendimiento más significativa. El optimizador de consultas de Oracle (Cost-Based Optimizer - CBO) analiza las condiciones `WHERE` de la consulta y **elimina de la búsqueda todas las particiones que no contienen datos relevantes**. Si la tabla tiene 120 particiones mensuales de 10 años y la consulta solicita datos de marzo de 2025, Oracle accede únicamente a esa partición, ignorando las 119 restantes. El rendimiento de la consulta mejora proporcionalmente al número de particiones eliminadas.

### 3.2. Mantenimiento independiente por partición

Las operaciones de administración y mantenimiento pueden ejecutarse sobre particiones individuales sin afectar a las demás:

*   **`ALTER TABLE ... TRUNCATE PARTITION`:** Vacía instantáneamente una partición histórica (por ejemplo, datos de 1990) sin bloquear las particiones activas.
*   **`ALTER TABLE ... DROP PARTITION`:** Elimina una partición completa.
*   **`ALTER TABLE ... ADD PARTITION`:** Añade una nueva partición (por ejemplo, para el nuevo ejercicio fiscal).
*   **`ALTER INDEX ... REBUILD PARTITION`:** Reconstruye el índice de una sola partición.

Mientras se ejecuta el mantenimiento de una partición, las aplicaciones continúan operando normalmente sobre las demás particiones.

### 3.3. Estrategia de almacenamiento diferenciado (ILM - Information Lifecycle Management)

Las particiones pueden ubicarse en tablespaces diferentes con distintas características de almacenamiento. Los datos recientes ("calientes") se almacenan en tablespaces sobre discos SSD de alto rendimiento, mientras que los datos históricos ("fríos") se migran a tablespaces sobre discos mecánicos de menor coste y mayor capacidad. Esta estrategia optimiza simultáneamente el rendimiento y los costes de almacenamiento.

### 3.4. Mejora de la disponibilidad

Si un disco que contiene una partición específica falla, el resto de la tabla permanece disponible para consultas y actualizaciones. Solo se ve afectada la partición ubicada en el disco dañado.

## 4. Tipologías de Particionamiento

El DBA debe definir una **clave de partición (Partition Key)**: la columna o columnas que determinan en qué partición se almacena cada fila.

### 4.1. Range Partitioning (Particionado por rango)

Es la tipología más común. Las filas se distribuyen en particiones según rangos de valores de la clave de partición, típicamente fechas o números secuenciales.

```sql
CREATE TABLE MULTAS_TRAFICO (
    id_multa    NUMBER,
    fecha_multa DATE,
    importe     NUMBER(10,2),
    matricula   VARCHAR2(10)
)
PARTITION BY RANGE (fecha_multa) (
    PARTITION p_2023 VALUES LESS THAN (DATE '2024-01-01') TABLESPACE ts_historico,
    PARTITION p_2024 VALUES LESS THAN (DATE '2025-01-01') TABLESPACE ts_historico,
    PARTITION p_2025 VALUES LESS THAN (DATE '2026-01-01') TABLESPACE ts_actual,
    PARTITION p_max  VALUES LESS THAN (MAXVALUE)          TABLESPACE ts_actual
);
```

### 4.2. List Partitioning (Particionado por lista)

Las filas se distribuyen según valores discretos de la clave de partición. Ideal para columnas con un conjunto finito de valores categóricos.

```sql
CREATE TABLE EXPEDIENTES (
    id_expediente NUMBER,
    provincia     VARCHAR2(50),
    tipo          VARCHAR2(20)
)
PARTITION BY LIST (provincia) (
    PARTITION p_alicante VALUES ('ALICANTE'),
    PARTITION p_valencia VALUES ('VALENCIA'),
    PARTITION p_castellon VALUES ('CASTELLON'),
    PARTITION p_otros    VALUES (DEFAULT)
);
```

### 4.3. Hash Partitioning (Particionado por hash)

Cuando la clave de partición no sigue un patrón natural (rangos o categorías), Oracle aplica una función hash interna para distribuir las filas de forma uniforme entre las particiones. Es ideal para columnas con valores pseudoaleatorios como identificadores UUID o números de referencia.

```sql
CREATE TABLE TRANSACCIONES (
    id_transaccion NUMBER,
    detalle        VARCHAR2(200)
)
PARTITION BY HASH (id_transaccion)
PARTITIONS 8;
```

### 4.4. Composite Partitioning (Particionado compuesto)

Combina dos niveles de particionamiento para un control más granular. La tabla se particiona primero por un criterio y luego cada partición se subparticiona por un segundo criterio.

```sql
CREATE TABLE VENTAS (
    id_venta     NUMBER,
    fecha_venta  DATE,
    region       VARCHAR2(50),
    importe      NUMBER(10,2)
)
PARTITION BY RANGE (fecha_venta)
SUBPARTITION BY LIST (region) (
    PARTITION p_2025 VALUES LESS THAN (DATE '2026-01-01') (
        SUBPARTITION p_2025_alicante VALUES ('ALICANTE'),
        SUBPARTITION p_2025_valencia VALUES ('VALENCIA'),
        SUBPARTITION p_2025_otros    VALUES (DEFAULT)
    ),
    PARTITION p_2026 VALUES LESS THAN (DATE '2027-01-01') (
        SUBPARTITION p_2026_alicante VALUES ('ALICANTE'),
        SUBPARTITION p_2026_valencia VALUES ('VALENCIA'),
        SUBPARTITION p_2026_otros    VALUES (DEFAULT)
    )
);
```

Las combinaciones más habituales son Range-List, Range-Hash y Range-Range.

### 4.5. Interval Partitioning

Extensión del Range Partitioning que crea automáticamente nuevas particiones a medida que se insertan datos que exceden el rango de las particiones existentes, eliminando la necesidad de que el DBA añada particiones manualmente para cada nuevo periodo.

```sql
CREATE TABLE RECIBOS (
    id_recibo   NUMBER,
    fecha       DATE,
    importe     NUMBER(10,2)
)
PARTITION BY RANGE (fecha)
INTERVAL (NUMTOYMINTERVAL(1,'MONTH'))
(PARTITION p_inicio VALUES LESS THAN (DATE '2025-01-01'));
```

## 5. Oracle Enterprise Manager (OEM)

### 5.1. Concepto y necesidad

La complejidad de la infraestructura Oracle —estructuras de memoria SGA/PGA, procesos de fondo, tablespaces, entornos RAC multiservidor, configuraciones Data Guard y tablas particionadas de petabytes— hace inviable la administración exclusivamente mediante línea de comandos SQL*Plus. **Oracle Enterprise Manager (OEM)** es la plataforma oficial de administración centralizada que proporciona una interfaz gráfica web (dashboard) para monitorizar, configurar y administrar toda la infraestructura Oracle desde un único punto de acceso.

### 5.2. Versiones de Oracle Enterprise Manager

#### 5.2.1. Oracle Enterprise Manager Database Express (EM Express)

*   **Alcance:** Versión ligera integrada directamente en la instalación de la base de datos (a partir de Oracle 12c).
*   **Arquitectura:** Se accede a través de un navegador web conectándose al puerto HTTPS configurado en la instancia (típicamente el puerto 5500).
*   **Capacidades:** Monitorización de rendimiento en tiempo real, visualización de la actividad de las sesiones (Active Session History - ASH), gestión básica de tablespaces, usuarios y parámetros de la instancia.
*   **Limitación:** Solo administra la base de datos individual donde está instalado. No soporta monitorización multibases ni multi-host.

#### 5.2.2. Oracle Enterprise Manager Cloud Control (EMCC)

*   **Alcance:** Plataforma de administración empresarial para infraestructuras complejas.
*   **Arquitectura:** Se despliega como una aplicación independiente en un servidor dedicado (Oracle Management Server - OMS), con agentes (Oracle Management Agent) instalados en cada servidor gestionado, y un repositorio de datos almacenado en una base de datos Oracle dedicada.
*   **Capacidades:**
    *   Administración centralizada de cientos de bases de datos, clústeres RAC, configuraciones Data Guard y servidores de aplicaciones desde una única consola.
    *   **Performance Hub:** Análisis detallado del rendimiento con AWR (Automatic Workload Repository), ASH y ADDM (Automatic Database Diagnostic Monitor).
    *   **SQL Tuning Advisor:** Analiza automáticamente las consultas SQL más costosas y recomienda la creación de índices, la reescritura de consultas o la recolección de estadísticas.
    *   **Programación de backups RMAN:** Permite definir y programar políticas de backup mediante interfaz gráfica.
    *   **Gestión de parches:** Automatiza la descarga, verificación y aplicación de parches de seguridad y actualizaciones.
    *   **Alertas y notificaciones:** Configura umbrales de alerta (espacio en tablespace, consumo de CPU, bloqueos) y envía notificaciones por correo electrónico o integración con sistemas de ticketing.
    *   **Compliance y seguridad:** Evalúa la conformidad de las bases de datos con estándares de seguridad (CIS Benchmarks, STIG/DISA).
    *   **Provisionamiento:** Automatiza la creación, clonación y migración de bases de datos.

### 5.3. Componentes clave del panel de OEM Cloud Control

| Componente | Función |
|------------|---------|
| **Targets** | Inventario de todos los recursos gestionados (bases de datos, hosts, listeners, clústeres) |
| **Performance Home** | Gráficos en tiempo real de actividad de la CPU, E/S y esperas del sistema |
| **AWR Reports** | Informes históricos de rendimiento para análisis de tendencias y diagnóstico |
| **SQL Monitor** | Seguimiento en tiempo real de la ejecución de sentencias SQL individuales |
| **Job System** | Programación y ejecución de tareas automatizadas (backups, scripts, mantenimiento) |
| **Incident Manager** | Gestión centralizada de alertas, problemas detectados y acciones correctivas |

## 6. Conclusión

Oracle Partitioning y Oracle Enterprise Manager representan dos pilares complementarios de la gestión de bases de datos Oracle en entornos de gran escala. El particionamiento transforma tablas monolíticas de cientos de millones de registros en estructuras manejables, mejorando drásticamente el rendimiento de las consultas mediante Partition Pruning, habilitando el mantenimiento independiente por partición y permitiendo estrategias de almacenamiento diferenciado que optimizan costes y rendimiento.

Oracle Enterprise Manager, por su parte, proporciona la capa de administración visual indispensable para gestionar infraestructuras Oracle complejas —con múltiples bases de datos, clústeres RAC, configuraciones Data Guard y tablespaces particionados— desde una única consola centralizada, incorporando capacidades de monitorización proactiva, diagnóstico automatizado de rendimiento y gestión de la seguridad.

En el contexto de las Administraciones Públicas, donde la escalabilidad, el rendimiento y la capacidad de administración eficiente son requisitos críticos, el dominio de estas tecnologías resulta esencial para garantizar la operatividad de los sistemas de información que sustentan los servicios al ciudadano.
