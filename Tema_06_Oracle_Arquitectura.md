# Tema 6.- El SGBDR Oracle. Arquitectura, modos de funcionamiento y administración.

## 1. Introducción

Oracle Database (Larry Ellison, 1977), SGBDR más implantados. Entornos donde rendimiento, fiabilidad y escalabilidad son críticos.

Ecosistema integral OLTP, OLAP, seguridad, alta disponibilidad y recuperación ante desastres.

## 2. Dicotomía Fundamental: Instancia vs. Base de Datos

### 2.1. La Base de Datos (The Database)

La Base de Datos componente **persistente y no volátil**. Archivos. Preservar la información de forma duradera, sobreviviendo a reinicios del sistema, cortes de energía y fallos de hardware.

1.  **Archivos de Datos (Datafiles - `.dbf` o `.ora`):** Datos reales. Cada datafile pertenece a un único Tablespace.

2.  **Archivos de Control (Control Files):** Metadatos estructurales: nombre BD, la ubicación datafiles y redo logs, timestamp de creación, secuencia log actual y puntos de control (checkpoints). Sin control files, BD no arranca.

3.  **Archivos de Redo Log en línea (Online Redo Log Files):** Registran cronológicamente todos los cambios sobre datos **antes** de escrirse en datafiles. **Write-Ahead Logging (WAL)**, ante un fallo del sistema, se puede reconstruir.

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
- Configuración estándar (Single Instance), una Instancia se asocia a una única BD.
- Configuración Oracle RAC (Real Application Clusters), múltiples Instancias acceden simultáneamente a una única BD compartida.

## 3. Estructuras de Memoria: SGA y PGA

Oracle reserva memoria RAM del servidor para datos y las instrucciones más utilizados. Más rápido.

### 3.1. SGA (System Global Area)

Area de memoria **compartida** entre usuarios. Contiene:

1.  **Database Buffer Cache (Caché de Bloques de Datos):** Es el componente de mayor impacto en el rendimiento. Cuando una consulta necesita leer datos, Oracle busca primero el bloque correspondiente (típicamente de 8 KB) en el Buffer Cache. Si lo encuentra (cache hit), evita la lectura de disco. Si no lo encuentra (cache miss), lee el bloque del datafile, lo almacena en el Buffer Cache y lo sirve al usuario. Los bloques modificados pero aún no escritos en disco se denominan **dirty blocks** (bloques sucios).

2.  **Shared Pool (Pool Compartido):** Almacena las estructuras de datos compartidas entre sesiones:
    *   **Library Cache:** Almacena los planes de ejecución de las sentencias SQL ya parseadas. Si 500 usuarios ejecutan la misma consulta SELECT, Oracle la analiza sintáctica y semánticamente una sola vez, reutilizando el plan de ejecución para todas las sesiones posteriores (concepto de *soft parse* vs. *hard parse*).
    *   **Data Dictionary Cache (Row Cache):** Almacena en memoria las definiciones de tablas, columnas, usuarios, privilegios y demás metadatos del diccionario de datos, evitando accesos repetidos al disco para resolver estos datos.

3.  **Redo Log Buffer:** Es un buffer circular de pequeño tamaño en memoria donde se registran temporalmente los cambios antes de ser escritos en los archivos Online Redo Log por el proceso LGWR. Su existencia permite que las operaciones de escritura del redo sean asíncronas respecto a la operación DML del usuario, acelerando significativamente el rendimiento.

4.  **Large Pool (opcional):** Área de memoria adicional utilizada para operaciones de backup/recovery (RMAN), sesiones de servidor compartido (Shared Server) y operaciones de E/S paralelas.

5.  **Java Pool (opcional):** Reserva de memoria para la máquina virtual Java integrada en Oracle, utilizada cuando se ejecutan procedimientos almacenados escritos en Java.

### 3.2. PGA (Program Global Area)

A diferencia de la SGA, la PGA es un área de memoria **privada y no compartida**. Oracle asigna una PGA independiente a cada sesión de usuario conectada. Contiene:

*   **Área de ordenación (Sort Area):** Espacio para operaciones ORDER BY, GROUP BY y operaciones de join que requieran ordenación.
*   **Área de hash:** Utilizada para hash joins y operaciones de agrupación.
*   **Variables de sesión:** Estado de la sesión, valores de bind variables y datos de contexto.

El tamaño de la PGA se gestiona mediante el parámetro `PGA_AGGREGATE_TARGET`, que establece un límite global para todas las sesiones.

## 4. Procesos de Fondo (Background Processes)

Los procesos de fondo son los componentes que sincronizan las estructuras de memoria (Instancia) con los archivos en disco (Base de Datos), garantizando la integridad, el rendimiento y la recuperabilidad del sistema. Los más relevantes son:

1.  **DBWn (Database Writer):** Responsable de escribir los bloques sucios (dirty blocks) del Database Buffer Cache a los datafiles en disco. No escribe en tiempo real tras cada operación DML, sino que lo hace de forma diferida cuando se cumplen ciertas condiciones: el Buffer Cache se llena, se produce un checkpoint, o transcurre un timeout. Pueden configurarse múltiples procesos DBWn (DBW0, DBW1, ...) para mejorar el rendimiento de escritura.

2.  **LGWR (Log Writer):** Escribe el contenido del Redo Log Buffer a los archivos Online Redo Log en disco. Es el proceso más crítico en cuanto a rendimiento transaccional, ya que se activa en tres situaciones obligatorias: cuando un usuario ejecuta un COMMIT, cuando el Redo Log Buffer está lleno al tercio de su capacidad, o cada tres segundos. La operación de COMMIT no se confirma al usuario hasta que LGWR haya escrito exitosamente en disco, garantizando así la **Durabilidad** (la "D" de ACID).

3.  **SMON (System Monitor):** El proceso de recuperación del sistema. Cuando la instancia se reinicia tras un fallo inesperado (crash), SMON es el primer proceso en actuar: examina los Online Redo Logs, re-aplica (roll forward) las transacciones confirmadas que no habían sido escritas en los datafiles, y deshace (roll back) las transacciones no confirmadas. También se encarga de la coalescencia de espacios libres contiguos y la limpieza de segmentos temporales.

4.  **PMON (Process Monitor):** Actúa como vigilante de las sesiones de usuario. Si una conexión se interrumpe abruptamente (por ejemplo, por un corte de red), PMON detecta la sesión abandonada, libera los bloqueos que mantenía, deshace las transacciones no confirmadas de esa sesión y libera los recursos de memoria (PGA) asociados.

5.  **CKPT (Checkpoint Process):** Señaliza periódicamente al DBWn para que escriba los bloques sucios en disco y actualiza las cabeceras de los datafiles y los control files con la información del último checkpoint. Los checkpoints reducen el tiempo de recuperación tras un fallo, ya que SMON solo necesita re-aplicar los cambios posteriores al último checkpoint.

6.  **ARCn (Archiver):** Activo únicamente cuando la base de datos opera en modo ARCHIVELOG. Copia los Online Redo Logs completos a una ubicación de archivo antes de que sean reutilizados, preservando el historial completo de cambios para la recuperación temporal (point-in-time recovery) y la alimentación de bases de datos standby (Data Guard).

## 5. Tablespaces: La Abstracción Lógica del Almacenamiento

Oracle introduce el concepto de **Tablespace** como capa de abstracción entre la estructura lógica de la base de datos y el almacenamiento físico, materializando la independencia física de datos promovida por la arquitectura ANSI/SPARC.

*   **Definición:** Un Tablespace es un contenedor lógico que agrupa uno o más datafiles físicos. Las tablas, índices y demás objetos se crean dentro de un Tablespace, no directamente sobre un datafile.
*   **Funcionamiento:** El DBA crea un Tablespace (por ejemplo, `TS_USUARIOS`) y le asocia uno o varios datafiles (por ejemplo, `datos01.dbf` en el disco 1 y `datos02.dbf` en el disco 2). Cuando un datafile se llena, Oracle puede expandir automáticamente el Tablespace utilizando los demás datafiles asignados, o el DBA puede añadir nuevos datafiles de forma dinámica. Las aplicaciones y los usuarios nunca interactúan directamente con los datafiles; trabajan únicamente con las tablas alojadas en los Tablespaces.

**Tablespaces predefinidos en Oracle:**
*   **SYSTEM:** Contiene el diccionario de datos (tablas del sistema, vistas de catálogo).
*   **SYSAUX:** Complemento del Tablespace SYSTEM para componentes auxiliares (AWR, ASH, Enterprise Manager).
*   **UNDO:** Almacena los datos de deshacer (undo data) necesarios para las operaciones de Rollback y para la consistencia de lectura (Read Consistency mediante MVCC).
*   **TEMP:** Utilizado para operaciones temporales de ordenación y hash que no caben en la PGA.
*   **USERS:** Tablespace por defecto para los objetos de los usuarios.

## 6. Modos de Funcionamiento

### 6.1. Modos de arranque de la instancia

Oracle contempla tres fases progresivas de arranque:

1.  **NOMOUNT:** Se crea la Instancia en memoria (SGA y procesos de fondo), pero aún no se accede a la base de datos. Se utiliza para crear una nueva base de datos o recrear control files.
2.  **MOUNT:** La Instancia lee los control files e identifica la ubicación de todos los datafiles y redo logs, pero la base de datos no está abierta a los usuarios. Se utiliza para operaciones de mantenimiento como renombrar datafiles o activar/desactivar el modo ARCHIVELOG.
3.  **OPEN:** La base de datos se abre completamente para las operaciones de los usuarios. Se verifican los datafiles y redo logs, y SMON ejecuta la recuperación automática si fuera necesario.

### 6.2. Modo ARCHIVELOG vs. NOARCHIVELOG

*   **NOARCHIVELOG:** Los Online Redo Logs se reutilizan cíclicamente, sobrescribiendo los anteriores. Solo permite recuperación hasta el último backup completo. No apto para entornos de producción críticos.
*   **ARCHIVELOG:** Antes de sobrescribir un Online Redo Log, el proceso ARCn crea una copia de archivo. Permite la recuperación hasta cualquier punto en el tiempo (point-in-time recovery) y es requisito indispensable para Oracle Data Guard y RMAN hot backups.

## 7. Administración de Oracle Database

### 7.1. El rol del DBA

El Administrador de Base de Datos (DBA) es el responsable de garantizar la disponibilidad, el rendimiento, la seguridad y la integridad de la base de datos. Sus funciones principales incluyen:

*   Instalación, configuración y actualización del software Oracle.
*   Diseño e implementación de la arquitectura de almacenamiento (Tablespaces, datafiles).
*   Gestión de usuarios, roles y privilegios (seguridad y control de acceso).
*   Monitorización del rendimiento y optimización (tuning) de consultas SQL.
*   Planificación y ejecución de copias de seguridad (backup) y recuperación (recovery) mediante RMAN.
*   Aplicación de parches de seguridad y actualizaciones críticas.
*   Implementación y gestión de soluciones de alta disponibilidad (RAC, Data Guard).

### 7.2. Herramientas de administración

*   **SQL*Plus:** Interfaz de línea de comandos nativa de Oracle para la ejecución de sentencias SQL, PL/SQL y comandos de administración.
*   **Oracle Enterprise Manager (OEM):** Consola de administración gráfica basada en web que proporciona monitorización en tiempo real, gestión de alertas, informes de rendimiento y administración centralizada de múltiples bases de datos.
*   **RMAN (Recovery Manager):** Herramienta integrada para la gestión de backups y recovery. Soporta backups completos, incrementales y acumulativos, compresión, cifrado y verificación de integridad.
*   **Data Pump (expdp/impdp):** Utilidad de exportación e importación de datos de alto rendimiento que permite la migración de esquemas, tablas o bases de datos completas entre instancias.

## 8. Conclusión

La arquitectura de Oracle Database representa uno de los diseños más sofisticados y robustos de la industria de los SGBDR. La separación conceptual entre Instancia (componente volátil en memoria) y Base de Datos (componente persistente en disco), junto con el diseño de las estructuras de memoria (SGA/PGA) y los procesos de fondo (DBWn, LGWR, SMON, PMON, CKPT, ARCn), conforman un sistema que garantiza las propiedades ACID incluso en los escenarios más adversos.

La abstracción del almacenamiento mediante Tablespaces, los modos de funcionamiento configurables y el completo conjunto de herramientas de administración (SQL*Plus, OEM, RMAN, Data Pump) proporcionan al DBA el control necesario para gestionar bases de datos de cualquier escala, desde pequeñas aplicaciones departamentales hasta los sistemas transaccionales más exigentes de la Administración Pública. El conocimiento profundo de esta arquitectura resulta imprescindible para cualquier profesional que deba garantizar la disponibilidad, el rendimiento y la seguridad de los datos en el ámbito del sector público.
