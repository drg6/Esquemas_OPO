# Tema 10.- El SGBDR Oracle: Opción "Partitioning" y Enterprise Manager.

## 1. Introducción
* **Desafío VLDB (*Very Large DataBases*) en AAPP:** Tablas históricas masivas (recaudación, padrón, registro electrónico, sanidad) con cientos de millones de filas donde una estructura monolítica degrada las consultas, bloquea el mantenimiento y penaliza los backups.
* **Solución dual en Oracle:**
  * **Oracle Partitioning:** Estrategia de *"divide y vencerás"* a nivel físico manteniendo una única entidad lógica.
  * **Oracle Enterprise Manager (OEM):** Plataforma centralizada de gobierno, monitorización y diagnóstico proactivo.

## 2. Oracle Partitioning: Concepto y Principio de Transparencia
* **Definición:** División física de una tabla o índice en segmentos más pequeños e independientes (**particiones**), gobernados por una o varias columnas denominadas **Clave de Partición (*Partition Key*)**.
* **Transparencia Lógica:** Es 100% transparente para las aplicaciones. El código ejecuta sentencias SQL estándar (`SELECT`, `INSERT`, `UPDATE`, `DELETE`) contra el nombre de la tabla sin referenciar particiones; el motor de Oracle enruta la operación internamente.

## 3. Beneficios Arquitectónicos del Particionamiento
* **3.1. Rendimiento — *Partition Pruning* (Poda de particiones):**
  * El optimizador de costes (**CBO**) analiza el predicado `WHERE` y **excluye de la lectura física todas las particiones irrelevantes** (ej. leer solo la partición de marzo de 2026 ignorando otros 10 años de histórico).
* **3.2. Mantenimiento Independiente (Operaciones DDL granulares):**
  * Permite operar sobre una partición sin bloquear el resto de la tabla activa: `ALTER TABLE ... TRUNCATE PARTITION`, `DROP PARTITION`, `ADD PARTITION` o `ALTER INDEX ... REBUILD PARTITION`.
* **3.3. Gestión del Ciclo de Vida del Dato (ILM - *Information Lifecycle Management*):**
  * Permite alojar cada partición en un **Tablespace distinto**: particiones actuales ("datos calientes") en almacenamiento NVMe/SSD de alto rendimiento y particiones históricas ("datos fríos") en discos económicos de alta capacidad.
* **3.4. Alta Disponibilidad parcial:**
  * Si falla el datafile/disco de una partición histórica, el resto de particiones de la tabla continúan operativas online.

## 4. Tipologías de Particionamiento y Sintaxis Clave
* **4.1. Range Partitioning (Por Rango):**
  * Distribución por intervalos continuos (fechas, ejercicios fiscales). Usa la cláusula `VALUES LESS THAN` y el comodín de cierre `MAXVALUE`.
  * *Sintaxis:* `PARTITION BY RANGE (fecha) (PARTITION p_2025 VALUES LESS THAN (DATE '2026-01-01') TABLESPACE ts_actual, PARTITION p_max VALUES LESS THAN (MAXVALUE));`
* **4.2. List Partitioning (Por Lista):**
  * Valores discretos/categóricos cerrados (provincias, estados de expediente). Admite partición colectora `DEFAULT`.
  * *Sintaxis:* `PARTITION BY LIST (provincia) (PARTITION p_alc VALUES ('ALICANTE'), PARTITION p_otros VALUES (DEFAULT));`
* **4.3. Hash Partitioning (Por Función Hash):**
  * Aplica un algoritmo hash sobre la clave (ej. ID o UUID) para repartir filas homogéneamente entre $N$ particiones y balancear la E/S cuando no existen rangos naturales.
  * *Sintaxis:* `PARTITION BY HASH (id_transaccion) PARTITIONS 8;`
* **4.4. Composite Partitioning (Compuesto):**
  * Anida dos criterios (`PARTITION BY ... SUBPARTITION BY ...`), combinando habitualmente *Range-List*, *Range-Hash* o *Range-Range* (ej. particionar por año y subparticionar por provincia).
* **4.5. Interval Partitioning (Automático por Intervalos):**
  * Extensión de *Range* que automatiza la administración: Oracle **crea dinámicamente nuevas particiones** cuando entra un registro que supera el límite superior existente.
  * *Sintaxis:* `PARTITION BY RANGE (fecha) INTERVAL (NUMTOYMINTERVAL(1,'MONTH')) (PARTITION p_ini VALUES LESS THAN (DATE '2025-01-01'));`

## 5. Oracle Enterprise Manager (OEM)
* **5.1. Propósito:** Consola web centralizada para administrar infraestructuras complejas (instancias, RAC, Data Guard, almacenamiento ASM) sin depender exclusivamente de CLI (`SQL*Plus`).
* **5.2. Ediciones y Arquitecturas:**
  * **EM Database Express (EM Express):** Consola web ligera integrada de serie en la propia instancia (desde 12c, puerto HTTPS típico `5500`). Sin agentes externos; **solo gestiona una base de datos individual** (sesiones ASH, tablespaces, parámetros).
  * **EM Cloud Control (EMCC):** Plataforma corporativa multi-sistema basada en arquitectura de 3 capas:
    1. **OMS (*Oracle Management Server*):** Servidor de aplicaciones central.
    2. **OMA (*Oracle Management Agent*):** Agente desplegado en cada host monitorizado.
    3. **Management Repository:** Base de datos Oracle dedicada que almacena métricas e inventario.
* **5.3. Módulos y Capacidades Clave de Cloud Control:**
  * **Targets & Incident Manager:** Inventario global (BDs, listeners, nodos) y gestión de alertas por umbrales con notificación/ticketing.
  * **Performance Hub (Diagnóstico):** Integra **AWR** (*Automatic Workload Repository*, histórico de rendimiento), **ASH** (*Active Session History*, actividad en tiempo real) y **ADDM** (motor de diagnóstico automático).
  * **SQL Tuning Advisor & SQL Monitor:** Seguimiento de queries pesadas en vivo y recomendaciones automáticas de índices, perfiles SQL o estadísticas.
  * **Job System, Parches y RMAN:** Planificación visual de backups RMAN, despliegue automatizado de parches de seguridad y aprovisionamiento/clonación de BDs.
  * **Compliance:** Auditoría continua de seguridad frente a estándares (CIS, STIG y controles aplicables al ENS).

## 6. Conclusión
Oracle Partitioning y Enterprise Manager son herramientas indispensables para el escalado sostenible en la Administración Pública. Mientras Partitioning resuelve el cuello de botella físico de las bases de datos masivas (VLDB) mediante *Partition Pruning*, mantenimientos no disruptivos y políticas de ahorro ILM, OEM Cloud Control dota a los equipos de sistemas de observabilidad integral (AWR/ADDM) y automatización centralizada, garantizando el rendimiento y la disponibilidad exigidos por los servicios públicos digitales.

-------------------

# Tema 10.- El SGBDR Oracle. Opción "Partitioning" y Enterprise Manager.

## 1. Introducción

**VLDB (Very Large DataBases)** -> tablas con muchos registros (sistemas tributarios nacionales, registros de la Seguridad Social, ...), soluciona consultas y backup lentas
**Oracle Partitioning**, descompone tablas e índices en fragmentos más pequeño. 
**Oracle Enterprise Manager (OEM)** plataforma administración y monitorización centralizada.

## 2. Oracle Partitioning: Concepto y Principio de Transparencia

**Particionamiento** dividir físicamente una tabla o un índice de gran tamaño en **particiones** (subconjunto de filas) mediante un criterio de distribución (fecha, región, rango numérico, etc.).
**Transparencia lógica**: Lógicamente es una única tabla. 

## 3. Beneficios del Particionamiento

### 3.1. Partition Pruning (Poda de particiones)

Mejora el rendimiento, el optimizador de consultas de Oracle (Cost-Based Optimizer - CBO) analiza las condiciones `WHERE` y **elimina de la búsqueda todas las particiones que no contienen datos relevantes**. 

### 3.2. Mantenimiento independiente por partición

Las operaciones de administración y mantenimiento sobre particiones individuales: **`ALTER TABLE ... TRUNCATE PARTITION`:** , **`ALTER TABLE ... DROP PARTITION`:**, **`ALTER TABLE ... ADD PARTITION`:** , **`ALTER INDEX ... REBUILD PARTITION`:** 
Se continúa operando normalmente sobre las demás particiones.

### 3.3. Estrategia de almacenamiento diferenciado (ILM - Information Lifecycle Management)

Las particiones con **datos recientes** ("calientes") 0en tablespaces sobre discos SSD de alto rendimiento, mientras que **datos históricos** ("fríos") sobre discos mecánicos de menor coste y mayor capacidad. 

### 3.4. Mejora de la disponibilidad

Partición falla, el resto tabla perrmanece disponible. 

## 4. Tipologías de Particionamiento

**Clave de partición (Partition Key)**: columna o columnas que determinan en qué partición se almacena cada fila. **PARTITION BY RANGE (fecha_multa) ( PARTITION p2023 VALUES ...)**

*   **Range Partitioning (Particionado por rango)**: Por rangos continuos (Ej: VALUES LESS THAN fecha).
*   **List**: Valores categóricos discretos (Ej: Provincias, pais).
*   **Hash**: Valores sin patrón, distribución uniforme mediante algoritmo interno (Ej: UUID, DNI).  **PARTITION BY HASH (id_transaccion) PARTITIONS 8;**
*   **Composite**: Combina dos métodos (Subparticiones. Ej: Range-List, Range-Hash y Range-Range). **PARTITION BY RANGE (fecha_venta) SUBPARTITION BY LIST (region) ()...**
*   **Interval**: Extensión de Range, auto-crea nuevas particiones (Ej: Cada mes). Auto-crea la partición futura al vuelo si no existe (Evita el error ORA-14400). **PARTITION BY RANGE (fecha) INTERVAL (NUMTOYMINTERVAL(1,'MONTH'))**

## 5. Oracle Enterprise Manager (OEM)

### 5.1. Concepto y necesidad

Plataforma oficial de administración centralizada (dashboard) para monitorizar, configurar y administrar infraestructura Oracle.

### 5.2. Versiones de Oracle Enterprise Manager

#### 5.2.1. Oracle Enterprise Manager Database Express (EM Express)

Out of the Box (integrada por defecto sin instalación extra)
*   **Capacidades:** Monitorización de rendimiento, actividad (Active Session History - ASH), tablespaces, usuarios.
*   **Limitación:** Solo administra BD individual, no multibases ni multi-host.

#### 5.2.2. Oracle Enterprise Manager Cloud Control (EMCC)

*   Consola Web (Interfaz UI).
*   OMS (Oracle Management Server): El motor intermedio.
*   OMR (Oracle Management Repository): La BD dedicada que guarda todas las métricas históricas.
*   Agentes: Instalados en cada host objetivo (Targets).

*   **Capacidades:**
    *   Administración centralizada de BDs, clústeres RAC, Data Guard y servidores de aplicaciones.
    *   **Performance Hub:** Análisis rendimiento con AWR (Automatic Workload Repository), ASH y ADDM (Automatic Database Diagnostic Monitor).
    *   **SQL Tuning Advisor:** Análisis consultas SQL. Recomienda creación de índices, recolección de estadísticas. SQL Monitor
    *   **Programación de backups RMAN:** 
    *   **Gestión de parches:** 
    *   **Alertas y notificaciones:** Incident Manager
    *   **Compliance y seguridad:**
    *   **Provisionamiento:** Automatiza la creación, clonación y migración de bases de datos.
    *   **Job System**

## 6. Conclusión

Partitioning = Soluciona el problema físico del volumen de datos (Rendimiento/ILM).
OEM = Soluciona el problema lógico de la complejidad operativa (Administración centralizada).

Implementacion en AP -> esencial para garantizar la operatividad y gobernar grandes volúmenes de información ciudadana
