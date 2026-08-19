# Tema 6.- El SGBDR Oracle. Arquitectura, modos de funcionamiento y administración.

## 1. Introducción

Oracle Database (Larry Ellison, 1977) es un SGBDR líder en entornos empresariales de misión crítica (Tier 1). No es un simple almacén, sino un ecosistema integral que soporta cargas OLTP y OLAP, garantizando rendimiento, alta disponibilidad y cumplimiento de las propiedades ACID. Su arquitectura se basa en la separación estricta entre dos conceptos: la Base de Datos (física) y la Instancia (lógica).

## 2. Dicotomía Fundamental: Instancia vs. Base de Datos

### 2.1. La Base de Datos (The Database)

La Base de Datos componente **persistente y no volátil**. Archivos. Preservar la información de forma duradera, sobreviviendo a reinicios del sistema, cortes de energía y fallos de hardware.

1.  **Archivos de Datos (Datafiles - `.dbf` o `.ora`):** Datos reales (tablas, índices). Cada datafile pertenece a un único Tablespace.

2.  **Archivos de Control (Control Files):** Almacenan metadatos estructurales críticos (estado, ubicaciones, checkpoints). Sin control files, BD no arranca.

3.  **Archivos de Redo Log en línea (Online Redo Log Files):** egistran cronológicamente todo cambio antes de escribirse en los datafiles mediante la técnica Write-Ahead Logging (WAL). Permiten reconstruir transacciones ante caídas.

**Archivos adicionales relevantes:**
*   **Archived Redo Logs:** Copias archivadas redo logs. Recuperación ante desastres y Oracle Data Guard.
*   **Parameter File (PFILE/SPFILE):** Parámetros configuración instancia (tamaño de memoria, procesos máximos, rutas de archivos).
*   **Password File:** Contraseñas de usuarios con privilegios de administración (SYSDBA, SYSOPER).

### 2.2. La Instancia de Oracle (The Oracle Instance)

La Instancia es el componente **volátil y dinámico**. En memoria RAM del servidor, se crea al arrancar.

La Instancia compuesta por:
- **Estructuras de memoria:** SGA (System Global Area) y PGA (Program Global Area).
- **Procesos de fondo (Background Processes):** Procesos del SO tareas de mantenimiento y sincronización.

**Relación Instancia-Base de Datos:**
En Single Instance la relación es 1:1 con la BD. En Oracle RAC (alta disponibilidad), múltiples instancias atacan una misma BD compartida.

## 3. Estructuras de Memoria: SGA y PGA

Oracle reserva memoria RAM del servidor para datos y las instrucciones más utilizados. Más rápido.

### 3.1. SGA (System Global Area)

Area de memoria **compartida** entre usuarios. Contiene:

1.  **Database Buffer Cache (Caché de Bloques de Datos):** Mayor impacto rendimiento. Leer datos -> busca bloque en Buffer Cache. Lo encuentra (cache hit), evita lectura. No encuentra (cache miss), lee bloque del datafile, almacena en Buffer Cache y lo sirve. Los bloques modificados pero no escritos en disco -> **dirty blocks** (bloques sucios)

2.  **Shared Pool (Pool Compartido):** Datos compartidas entre sesiones:
    *   **Library Cache:** Almacena planes de ejecución SQL ya parseadas (c*soft parse* vs. *hard parse*).
    *   **Data Dictionary Cache (Row Cache):** Metadatos del sistema.

3.  **Redo Log Buffer:** Buffer circular que retiene temporalmente los cambios antes de volcarlos al Redo Log en disco, agilizando transacciones.

4.  **Large Pool (opcional):** Memoria para operaciones de backup/recovery (RMAN), sesiones de servidor compartido (Shared Server) y operaciones de E/S paralelas.

5.  **Java Pool (opcional):** Memoria para la máquina virtual Java.

### 3.2. PGA (Program Global Area)

Memoria privada exclusiva de cada sesión de usuario (gestionada por PGA_AGGREGATE_TARGET).

*   **Área de ordenación (Sort Area):** Para operaciones ORDER BY y GROUP BY.
*   **Área de hash:** Para operaciones de join.
*   **Variables de sesión:** 

## 4. Procesos de Fondo (Background Processes)

Sincronizan la Instancia (memoria) con la Base de Datos (disco).

1.  **DBWn (Database Writer):** Escribe los dirty blocks del Buffer Cache a los datafiles. Escritura diferida por rendimiento.

2.  **LGWR (Log Writer):** Vuelca el Redo Log Buffer a los Online Redo Logs. Es crítico: un COMMIT no finaliza hasta que LGWR escribe en disco (garantiza la Durabilidad).

3.  **SMON (System Monitor):** Recupera el sistema tras una caída (crash recovery). Aplica cambios confirmados (roll forward) y deshace los no confirmados (roll back).

4.  **PMON (Process Monitor):** Limpia sesiones abandonadas, libera recursos (PGA) y elimina bloqueos.

5.  **CKPT (Checkpoint Process):** Fuerza la sincronización y actualiza cabeceras de datafiles y control files indicando el punto de consistencia.

6.  **ARCn (Archiver):** (Solo en modo ARCHIVELOG). Copia los Redo Logs a un histórico antes de sobrescribirse.

## 5. Tablespaces: La Abstracción Lógica del Almacenamiento

**Tablespace** capa de abstracción entre estructura lógica y almacenamiento físico, independencia física arquitectura ANSI/SPARC.

**Tablespaces predefinidos en Oracle:**
*   **SYSTEM:** Contiene el diccionario de datos (tablas del sistema, vistas de catálogo).
*   **SYSAUX:** Componentes auxiliares (estadísticas AWR).
*   **UNDO:** Datos de deshacer (undo data) para Rollback y consistencia de lectura (Read Consistency mediante MVCC).
*   **TEMP:** Operaciones de ordenación que desbordan la PGA.
*   **USERS:** Datos y objetos de los usuarios.

## 6. Modos de Funcionamiento

### 6.1. Modos de arranque de la instancia

1.  **NOMOUNT:** Levanta la Instancia (memoria y procesos). No lee disco. (Para crear BD).
2.  **MOUNT:** Lee Control Files y localiza Datafiles., no está abierta a usuarios. Uso mantenimiento
3.  **OPEN:** Se abre a usuarios. 

### 6.2. Modo ARCHIVELOG vs. NOARCHIVELOG

*   **NOARCHIVELOG:** Los Redo Logs se sobrescriben cíclicamente. Impide recuperación Point-in-Time. Inviable en producción.
*   **ARCHIVELOG:** Exige copia histórica del Redo Log antes de sobrescribirlo. Obligatorio para Data Guard y backups en caliente con RMAN.

## 7. Administración de Oracle Database

### 7.1. El rol del DBA

El DBA garantiza disponibilidad, rendimiento, seguridad e integridad. Tareas clave: despliegue físico, tuning SQL, gestión de usuarios, parches y diseño de backups.

### 7.2. Herramientas de administración

*   **SQL*Plus:** Consola CLI nativa.
*   **Oracle Enterprise Manager (OEM):** Monitorización gráfica y centralizada.
*   **RMAN (Recovery Manager):** Herramienta oficial para backups (completos, incrementales) y recuperación
*   **Data Pump (expdp/impdp):** Exportación/importación lógica de alta velocidad.

## 8. Conclusión

La arquitectura de Oracle Database -> diseños más sofisticados y robustos de los SGBDR. Sistema que garantiza las propiedades ACID.

Herramientas Oracle permiten DBA gestionar BD de cualquier escala. Imprescindible garantizar la disponibilidad, el rendimiento y la seguridad de los datos.
