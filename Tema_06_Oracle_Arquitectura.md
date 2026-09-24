# Tema 6.- El SGBDR Oracle: Arquitectura, Modos y Administración.

## 1. Introducción
* **Relevancia:** Estándar de facto en sistemas transaccionales de misión crítica (Tier 1) de las Administraciones Públicas (padrón, tributos, contabilidad).
* **Garantía ACID:** Arquitectura diseñada para maximizar concurrencia (OLTP) y análisis masivo (OLAP) con alta disponibilidad y tolerancia a fallos.
* **Principio rector:** Dicotomía estricta entre elementos volátiles en memoria (Instancia) y elementos persistentes en almacenamiento (Base de Datos).

## 2. Dicotomía Fundamental: Instancia vs. Base de Datos
* **2.1. Base de Datos (Física / Persistente en Disco):**
  * **Datafiles (`.dbf`):** Almacenan datos reales (tablas, índices, PL/SQL). Asociados unívocamente a Tablespaces.
  * **Control Files:** Metadatos críticos (rutas, nombres, checkpoints, números SCN). Obligatorio multiplexar en discos físicos distintos para evitar el punto único de fallo.
  * **Online Redo Log Files:** Mecanismo Write-Ahead Logging (WAL). Registran cronológicamente todo cambio DML antes de tocar disco. Mínimo 2 grupos circulares.
  * *Archivos auxiliares:* Parameter File (SPFILE binario dinámico / PFILE texto) y Password File (autenticación remota SYSDBA).
* **2.2. Instancia (Lógica / Volátil en RAM y Procesos SO):**
  * Nace al arrancar el servicio y desaparece al apagarlo; manipula la Base de Datos persistente.
  * *Topologías:* Single Instance (1 Instancia $\leftrightarrow$ 1 BD) vs. **Oracle RAC** (N Instancias concurrentes en clúster $\leftrightarrow$ 1 BD en almacenamiento compartido).

## 3. Estructuras de Memoria
* **3.1. SGA (System Global Area - Compartida):**
  * **Database Buffer Cache:** Almacena bloques de datos (8 KB típicos). Mitiga accesos a disco mediante política de bloques limpios y *dirty blocks* (sucios).
  * **Shared Pool:** 
    * *Library Cache:* Almacena planes de ejecución parseados (fomenta *soft parse* reutilizando consultas preparadas con bind variables).
    * *Data Dictionary Cache:* Metadatos de tablas, permisos y usuarios en memoria.
  * **Redo Log Buffer:** Buffer circular en RAM donde se encolan transacciones antes de su volcado a disco por LGWR.
  * *Áreas adicionales:* Large Pool (RMAN, I/O paralela) y Java Pool.
* **3.2. PGA (Program Global Area - Privada):**
  * Memoria exclusiva por sesión de usuario. Contiene el Sort Area (ORDER BY, GROUP BY), Hash Area (hash joins) y variables de sesión (controlada globalmente por `PGA_AGGREGATE_TARGET`).

## 4. Procesos de Fondo (Background Processes)
Sincronizan la memoria RAM con el disco garantizando durabilidad y consistencia:

* **DBWn (Database Writer):** Escribe *dirty blocks* del Buffer Cache a los Datafiles de forma asíncrona/diferida (en checkpoints, saturación o timeout).
* **LGWR (Log Writer):** Escribe el Redo Log Buffer a los Online Redo Logs en disco. **Garantiza la Durabilidad (D de ACID):** el COMMIT no retorna éxito al usuario hasta que LGWR confirma la escritura en disco.
* **SMON (System Monitor):** Recuperación automática tras caída (*crash recovery*). Aplica *roll forward* (rehace confirmadas desde Redo Logs) y *roll back* (deshace incompletas vía Undo).
* **PMON (Process Monitor):** Monitoriza sesiones caídas. Libera bloqueos de fila/tabla, cancela transacciones huérfanas y recupera memoria PGA.
* **CKPT (Checkpoint):** Sincroniza cabeceras de Datafiles y Control Files actualizando el System Change Number (SCN) y avisa a DBWn para acortar tiempos de recuperación de SMON.
* **ARCn (Archiver):** Copia los Online Redo Logs a almacenamiento seguro cuando se llenan (exclusivo de modo ARCHIVELOG).

## 5. Almacenamiento Lógico: Tablespaces
Abstracción ANSI/SPARC que aísla las aplicaciones de la estructura física del sistema de ficheros:
* **Mapeo:** Un Tablespace (lógico) contiene uno o varios Datafiles (físicos). Una tabla pertenece a un Tablespace, nunca directamente a un fichero.
* **Tablespaces predefinidos:**
  * **SYSTEM:** Diccionario de datos y catálogo base del motor.
  * **SYSAUX:** Metadatos auxiliares de monitorización (vistas AWR, métricas ASH).
  * **UNDO:** Segmentos de deshacer para rollback transaccional y consistencia de lectura (MVCC).
  * **TEMP:** Segmentos temporales para ordenaciones y agrupaciones que desbordan la PGA.
  * **USERS:** Contenedor por defecto para los esquemas y datos de aplicación.

## 6. Modos de Funcionamiento
* **6.1. Fases de Arranque:**
  * **NOMOUNT:** Asigna SGA y arranca procesos de fondo leyendo el SPFILE (creación de BD o recuperación de control files).
  * **MOUNT:** Abre y lee los Control Files; conoce la ubicación de Datafiles y Redo Logs pero no permite acceso a usuarios (mantenimiento, backups fríos, cambio a ARCHIVELOG).
  * **OPEN:** Abre Datafiles y Redo Logs, verifica coherencia SCN y permite conexiones concurrentes.
* **6.2. Registro de Transacciones:**
  * **NOARCHIVELOG:** Sobrescritura cíclica de Redo Logs. Solo admite copias completas en frío; pérdida de datos desde el último backup.
  * **ARCHIVELOG:** ARCn preserva cada Redo Log lleno. Habilita copias en caliente con RMAN, recuperación puntual (*Point-in-Time Recovery*) y replicación con **Oracle Data Guard**.

## 7. Administración y Operación (El rol del DBA)
* **Responsabilidades:** Gestión del ciclo de vida, tuning de planes de ejecución, seguridad de privilegios, gestión de tablespaces y políticas de continuidad de negocio.
* **Herramientas de Gestión:**
  * **SQL\*Plus:** CLI nativo para scripts de administración y compilación PL/SQL.
  * **Oracle Enterprise Manager (OEM):** Consola centralizada web de observabilidad, alertas y diagnóstico.
  * **RMAN (Recovery Manager):** Utilidad nativa de backup/recovery a nivel de bloque (soporta backups incrementales, compresión, cifrado y catálogo central).
  * **Data Pump (`expdp` / `impdp`):** Utilidad de alta velocidad a nivel lógico para migración de esquemas y metadatos.

## 8. Conclusión
La robustez de Oracle Database en infraestructuras públicas críticas radica en su desacoplamiento arquitectónico: la coordinación entre los procesos de fondo (LGWR/DBWn/SMON) y la memoria SGA/PGA asegura rendimiento transaccional sin comprometer las garantías ACID. Complementado con la abstracción lógica de los Tablespaces y la operatividad de RMAN en modo ARCHIVELOG, proporciona un ecosistema resistente a desastres, auditable y alineado con los requerimientos de continuidad que impone el Esquema Nacional de Seguridad (ENS).

----------------------

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
