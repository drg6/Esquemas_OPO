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
