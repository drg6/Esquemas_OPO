# Tema 9.- El SGBDR Oracle. Backup y Recuperación.

## 1. Introducción

La persistencia de los datos en una base de datos corporativa constituye un activo crítico e irremplazable para cualquier organización y, de manera especialmente acuciante, para las Administraciones Públicas. Los datos tributarios, padronales, urbanísticos y de gestión de personal almacenados en los SGBDR no son simples registros informáticos: representan obligaciones legales, derechos ciudadanos y procesos administrativos cuya pérdida o corrupción puede tener consecuencias jurídicas, económicas y operativas devastadoras.

Las amenazas son múltiples y reales: fallos de hardware (discos que se deterioran, controladores que fallan), errores humanos (un `DROP TABLE` ejecutado por error, un `DELETE` sin cláusula `WHERE`), corrupción lógica de datos, ciberataques como ransomware, desastres naturales o cortes prolongados de energía eléctrica. Ante este panorama, la capacidad de **Backup y Recuperación (Backup & Recovery)** constituye la última línea de defensa del DBA.

A diferencia de copiar un fichero ofimático, realizar un backup de una base de datos Oracle en producción —mientras miles de usuarios realizan operaciones simultáneamente— es un proceso de alta complejidad técnica que debe garantizar la consistencia transaccional (propiedades ACID) de los datos respaldados.

## 2. Clasificación de las Estrategias de Backup

Oracle distingue las estrategias de backup según dos ejes fundamentales: la profundidad del dato copiado y el estado operativo de la base de datos durante la copia.

### 2.1. Según la naturaleza del dato copiado

*   **Backup Físico:** Copia directamente los archivos binarios que componen la base de datos a nivel de bloque de disco: datafiles, control files, archived redo logs y el SPFILE. No interpreta el contenido de las tablas ni lee filas individuales. Al restaurarlo, reproduce la base de datos a nivel de bloque por bloque a su estado exacto. Es el método principal de protección frente a desastres y es el que utiliza **RMAN** (Recovery Manager).

*   **Backup Lógico (Export):** Utiliza el motor SQL para leer los datos de las tablas, vistas, procedimientos y demás objetos del esquema, y los empaqueta en un fichero binario portable. Es útil para migrar datos entre entornos (producción a preproducción), exportar esquemas específicos o archivar tablas históricas. Sin embargo, **no protege frente a la pérdida de datafiles, control files o redo logs**, por lo que no sustituye al backup físico para la recuperación ante desastres. La herramienta principal es **Data Pump** (`expdp`/`impdp`).

### 2.2. Según el estado de la base de datos

*   **Cold Backup (Backup en frío / Offline):** Se realiza con la base de datos completamente detenida (instancia apagada). El DBA copia los archivos físicos (datafiles, control files, redo logs) mediante el sistema operativo o RMAN. Garantiza una copia perfectamente consistente sin necesidad de aplicar redo logs durante la restauración. Su principal inconveniente es que requiere un periodo de indisponibilidad total, lo que resulta inaceptable en sistemas que exigen operatividad 24/7.

*   **Hot Backup (Backup en caliente / Online):** Se realiza mientras la base de datos permanece abierta y operativa, atendiendo las peticiones de los usuarios sin interrupción. Es el método estándar en entornos de producción. Para garantizar la consistencia de los bloques copiados mientras se producen modificaciones concurrentes, Oracle impone una condición indispensable: la base de datos debe estar configurada en modo **ARCHIVELOG**.

## 3. Modo ARCHIVELOG: La Piedra Angular de la Recuperación

Como se estudió en el Tema 6, los **Online Redo Log Files** registran cronológicamente todos los cambios realizados sobre los datos. Estos archivos se utilizan de forma circular: cuando Oracle llena el último grupo de redo log, sobrescribe el primero y reinicia el ciclo.

### 3.1. NOARCHIVELOG vs. ARCHIVELOG

*   **Modo NOARCHIVELOG:** Los Online Redo Logs se sobrescriben sin conservar copia. En caso de fallo, solo es posible restaurar hasta el último backup completo, perdiendo todas las transacciones posteriores. No permite hot backups consistentes. Solo apropiado para entornos de desarrollo o pruebas.

*   **Modo ARCHIVELOG:** Antes de sobrescribir un Online Redo Log, el proceso de fondo **ARCn (Archiver)** realiza una copia del mismo en una ubicación de archivo (**Archived Redo Logs**). Estos archivos archivados preservan el historial completo de cambios y habilitan:
    *   **Hot Backups:** Copias de seguridad con la base de datos abierta y operativa.
    *   **Recuperación completa (Complete Recovery):** Restaurar un backup y aplicar todos los archived redo logs hasta el momento del fallo, recuperando el 100% de las transacciones confirmadas.
    *   **Alimentación de Data Guard:** Las bases de datos standby se sincronizan mediante los archived redo logs.

### 3.2. Recuperación en un punto en el tiempo (Point-in-Time Recovery - PITR)

El modo ARCHIVELOG habilita una de las capacidades más potentes de Oracle: la recuperación hasta un instante preciso. Si un usuario ejecuta erróneamente un `DROP TABLE Oficinas` el martes a las 11:04, el DBA puede:

1.  Restaurar el backup más reciente (por ejemplo, el del domingo por la noche).
2.  Aplicar los archived redo logs del lunes y del martes, reproduciendo todos los cambios de forma secuencial.
3.  Detener la aplicación de logs exactamente en el instante anterior al error: las 11:03:59.

El resultado es la recuperación completa de la base de datos hasta un segundo antes del incidente, sin pérdida de ninguna transacción previa al error.

## 4. RMAN (Recovery Manager): La Herramienta de Backup Físico

### 4.1. Concepto y ventajas

**RMAN (Recovery Manager)** es la herramienta nativa de Oracle para gestionar todas las operaciones de backup y recovery físico. A diferencia de las copias de archivos mediante el sistema operativo, RMAN interactúa directamente con el kernel de la instancia de Oracle, lo que le confiere capacidades exclusivas:

*   **Conocimiento de la estructura interna:** RMAN conoce la organización de los datafiles, tablespaces, control files y redo logs, lo que le permite realizar operaciones inteligentes imposibles para herramientas genéricas de backup.
*   **Backups incrementales:** Solo copia los bloques que han sido modificados desde el último backup, reduciendo drásticamente el volumen de datos, el tiempo de copia y el espacio de almacenamiento necesario.
*   **Detección de corrupción:** Durante el proceso de lectura, RMAN verifica las sumas de comprobación (checksums) de cada bloque, detectando cualquier corrupción silenciosa antes de que cause un problema mayor.
*   **Compresión nativa:** Permite comprimir los backups durante el proceso de copia, reduciendo el espacio de almacenamiento sin necesidad de herramientas externas.
*   **Cifrado:** Soporta el cifrado de los backups para proteger los datos en reposo, cumpliendo requisitos de seguridad como los establecidos por el Esquema Nacional de Seguridad (ENS).
*   **Catálogo de backups:** Mantiene un registro detallado de todos los backups realizados, sus fechas, contenidos y estado de validez, facilitando la automatización de las políticas de retención.

### 4.2. Tipos de backup en RMAN

*   **Backup completo (Full Backup):** Copia todos los bloques de datos utilizados de los datafiles seleccionados. Es la base sobre la que se aplican los backups incrementales posteriores.

*   **Backup incremental de nivel 0:** Funcionalmente idéntico a un backup completo, pero sirve como punto de referencia base para los backups incrementales de nivel 1.

*   **Backup incremental de nivel 1:**
    *   **Diferencial:** Copia solo los bloques modificados desde el último backup incremental (de nivel 0 o nivel 1). Es el tipo por defecto.
    *   **Acumulativo:** Copia los bloques modificados desde el último backup de nivel 0. Requiere más espacio que el diferencial, pero simplifica la restauración al necesitar solo el nivel 0 más el último nivel 1 acumulativo.

*   **Backup de archived redo logs:** RMAN puede respaldar también los archived redo logs, eliminándolos del disco una vez respaldados para liberar espacio.

### 4.3. Operaciones de recuperación con RMAN

*   **RESTORE:** Extrae los archivos de backup y los coloca en sus ubicaciones originales (o en ubicaciones alternativas si es necesario).
*   **RECOVER:** Aplica los archived redo logs sobre los archivos restaurados para llevar la base de datos al punto de recuperación deseado (recovery completo o PITR).
*   **Block Media Recovery:** Permite recuperar un único bloque corrupto dentro de un datafile sin necesidad de restaurar ni recuperar el datafile completo. Minimiza el impacto en la disponibilidad.
*   **Flashback Database:** Funcionalidad complementaria que permite "rebobinar" la base de datos a un punto anterior en el tiempo sin necesidad de restaurar backups, utilizando los flashback logs. Es extremadamente rápido para errores lógicos recientes.

### 4.4. Ejemplo de script RMAN

```sql
RMAN> CONNECT TARGET /

-- Backup incremental de nivel 0 con compresión
RMAN> BACKUP AS COMPRESSED BACKUPSET
      INCREMENTAL LEVEL 0
      DATABASE
      PLUS ARCHIVELOG DELETE INPUT;

-- Backup incremental de nivel 1 diferencial diario
RMAN> BACKUP AS COMPRESSED BACKUPSET
      INCREMENTAL LEVEL 1
      DATABASE
      PLUS ARCHIVELOG DELETE INPUT;

-- Restauración y recuperación completa
RMAN> STARTUP MOUNT;
RMAN> RESTORE DATABASE;
RMAN> RECOVER DATABASE;
RMAN> ALTER DATABASE OPEN;

-- Recuperación hasta un punto en el tiempo (PITR)
RMAN> STARTUP MOUNT;
RMAN> RESTORE DATABASE UNTIL TIME "TO_DATE('2026-03-09 11:03:59','YYYY-MM-DD HH24:MI:SS')";
RMAN> RECOVER DATABASE UNTIL TIME "TO_DATE('2026-03-09 11:03:59','YYYY-MM-DD HH24:MI:SS')";
RMAN> ALTER DATABASE OPEN RESETLOGS;
```

## 5. Data Pump: La Herramienta de Backup Lógico

### 5.1. Concepto

**Oracle Data Pump** es la herramienta de exportación e importación lógica de datos introducida en Oracle 10g como sustituta de las antiguas utilidades `exp`/`imp`. A diferencia de RMAN, Data Pump no copia bloques físicos de disco, sino que utiliza el motor SQL para leer los objetos de la base de datos y empaquetarlos en un fichero binario portable (dump file).

### 5.2. Arquitectura

Data Pump se ejecuta íntegramente en el servidor de la base de datos (no en el cliente), lo que elimina la latencia de red y proporciona un rendimiento significativamente superior al de las antiguas utilidades cliente. Sus componentes principales son:

*   **`expdp` (Data Pump Export):** Exporta datos y metadatos desde la base de datos a un fichero dump.
*   **`impdp` (Data Pump Import):** Importa datos y metadatos desde un fichero dump hacia la base de datos.

### 5.3. Niveles de granularidad

Data Pump permite acotar la exportación/importación con precisión:

*   **Base de datos completa:** `FULL=Y` — exporta todos los esquemas, tablespaces y objetos.
*   **Esquema/Usuario:** `SCHEMAS=FINANZAS,RRHH` — exporta los objetos de los esquemas especificados.
*   **Tablas específicas:** `TABLES=EMPLEADO,DEPARTAMENTO` — exporta solo las tablas indicadas.
*   **Con filtros:** `QUERY="WHERE departamento_id = 10"` — permite filtrar las filas exportadas.

### 5.4. Funcionalidades avanzadas

*   **Paralelismo:** Permite utilizar múltiples procesos workers (`PARALLEL=4`) para acelerar la exportación/importación.
*   **Remap de esquema:** `REMAP_SCHEMA=PROD:TEST` — importa los objetos del esquema PROD asignándolos al esquema TEST.
*   **Remap de tablespace:** `REMAP_TABLESPACE=TS_PROD:TS_TEST` — redirige los objetos a un tablespace diferente.
*   **Estimación de tamaño:** Permite estimar el tamaño del dump file antes de ejecutar la exportación.
*   **Modo de red (Network Link):** Permite importar datos directamente desde otra base de datos a través de un database link, sin necesidad de fichero dump intermedio.

## 6. Estrategia de Backup Recomendada

Una estrategia de backup robusta para un entorno de producción Oracle en una Administración Pública combinaría:

| Componente | Frecuencia | Herramienta |
|------------|------------|-------------|
| Backup incremental nivel 0 | Semanal (domingo noche) | RMAN |
| Backup incremental nivel 1 | Diario (madrugada) | RMAN |
| Backup de archived redo logs | Cada 30 minutos | RMAN |
| Export lógico de esquemas críticos | Semanal | Data Pump |
| Validación de backups | Semanal | RMAN VALIDATE |
| Simulacro de recuperación | Trimestral | RMAN (en entorno de pruebas) |

Esta estrategia garantiza:
*   **RPO (Recovery Point Objective)** de 30 minutos: en el peor caso, la pérdida máxima de datos sería de 30 minutos de transacciones.
*   **RTO (Recovery Time Objective)** reducido: al utilizar backups incrementales, la restauración y recuperación se completa en una fracción del tiempo de un backup completo.

## 7. Conclusión

El sistema de Backup y Recuperación de Oracle Database constituye un pilar fundamental de la continuidad del servicio en sistemas de información críticos. La combinación del modo ARCHIVELOG —que habilita la recuperación continua y la operación en caliente—, la herramienta RMAN —con sus backups incrementales, detección de corrupción y recuperación granular— y Data Pump —para la exportación y migración lógica de datos— proporciona al DBA un arsenal completo para proteger la información frente a cualquier contingencia.

La definición de una estrategia de backup que contemple indicadores como el RPO y el RTO, junto con la ejecución periódica de simulacros de recuperación que validen la integridad de las copias, resulta imprescindible en las Administraciones Públicas, donde la pérdida de datos tributarios, padronales o registrales puede constituir un incumplimiento normativo y un perjuicio directo a los derechos de los ciudadanos.
