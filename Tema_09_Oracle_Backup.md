# Tema 9.- El SGBDR Oracle: Backup y Recuperación.

## 1. Introducción
* **Criticidad en AAPP:** La persistencia de datos (padrón, recaudación, registro, expedientes) constituye una obligación jurídica; su pérdida compromete derechos ciudadanos e incumple el Esquema Nacional de Seguridad (ENS).
* **Amenazas:** Fallos físicos (almacenamiento), errores humanos (`DROP`/`DELETE` accidental), corrupción lógica, *ransomware* o desastres de CPD.
* **Desafío técnico:** Garantizar copias consistentes que respeten las propiedades **ACID** bajo alta concurrencia sin interrumpir la operativa 24/7.

## 2. Clasificación de las Estrategias de Backup
* **2.1. Según la naturaleza del dato:**
  * **Backup Físico:** Copia bloques binarios de disco (Datafiles, Control Files, Archived Redo Logs, SPFILE). Base obligatoria para *Disaster Recovery*. Herramienta: **RMAN**.
  * **Backup Lógico:** Extrae objetos y filas mediante SQL hacia un fichero binario portable (*dump*). Útil para migraciones, refresco de entornos o archivado selectivo; **no sustituye al backup físico**. Herramienta: **Data Pump**.
* **2.2. Según el estado de la Base de Datos:**
  * **Cold Backup (En frío / Offline):** Instancia detenida (`SHUTDOWN` limpio). Copia 100% consistente de origen, pero inviable en servicios críticos por exigir parada del sistema.
  * **Hot Backup (En caliente / Online):** Base de datos abierta y operando. Exige obligatoriamente que la base de datos opere en modo **ARCHIVELOG**.

## 3. Modo ARCHIVELOG y Recuperación Temporal
* **NOARCHIVELOG vs. ARCHIVELOG:**
  * *NOARCHIVELOG:* Sobrescritura circular de Online Redo Logs. Solo permite restaurar en frío hasta el último backup completo (pérdida de transacciones posteriores).
  * *ARCHIVELOG:* El proceso **ARCn** archiva cada Redo Log lleno antes de su reutilización. Habilita *Hot Backups*, recuperación completa sin pérdida de datos y alimentación de *Data Guard*.
* **Point-in-Time Recovery (PITR / Recuperación incompleta):**
  * Permite detener la aplicación secuencial de *Archived Redo Logs* en el segundo u operación (SCN) inmediatamente anterior a un error humano o corrupción lógica (ej. `DROP TABLE`), abriendo posteriormente la BD con `ALTER DATABASE OPEN RESETLOGS`.

## 4. RMAN (Recovery Manager): Backup y Recuperación Física
* **4.1. Ventajas arquitectónicas:**
  * Interactúa con el kernel de Oracle: verifica *checksums* por bloque (detecta corrupción silenciosa), comprime en origen, cifra copias en reposo (cumplimiento ENS) y gestiona políticas de retención mediante el *Control File* o un *Recovery Catalog*.
* **4.2. Tipos de Backup en RMAN:**
  * **Full Backup:** Copia todos los bloques con datos de los datafiles.
  * **Incremental Nivel 0:** Equivalente en contenido al Full, pero actúa como base obligatoria de la estrategia incremental.
  * **Incremental Nivel 1:**
    * *Diferencial (por defecto):* Copia bloques modificados desde el último Nivel 0 o Nivel 1 (menor tamaño de copia).
    * *Acumulativo:* Copia bloques modificados desde el último Nivel 0 (restauración más rápida).
  * **BCT (*Block Change Tracking*):** Fichero bitmap en disco que registra qué bloques cambian en tiempo real, evitando que RMAN escanee los datafiles enteros durante los backups incrementales.
* **4.3. Operaciones de Recuperación:**
  * **RESTORE:** Reconstruye físicamente los ficheros desde el backup hacia el almacenamiento.
  * **RECOVER:** Aplica hacia adelante (*roll forward*) los Archived y Online Redo Logs sobre los datafiles restaurados.
  * **Block Media Recovery:** Recupera únicamente los bloques corruptos aislados manteniendo el datafile online.
  * **Flashback Database:** "Rebobina" la BD rápidamente hacia atrás aplicando *Flashback Logs* (sin restaurar backups completos) ante errores lógicos recientes.
* **4.4. Sintaxis esencial RMAN:**
  * Backup: `BACKUP AS COMPRESSED BACKUPSET INCREMENTAL LEVEL 0 DATABASE PLUS ARCHIVELOG DELETE INPUT;`
  * Recuperación PITR: `STARTUP MOUNT;` $\rightarrow$ `RESTORE DATABASE UNTIL TIME "...";` $\rightarrow$ `RECOVER DATABASE UNTIL TIME "...";` $\rightarrow$ `ALTER DATABASE OPEN RESETLOGS;`

## 5. Oracle Data Pump: Backup Lógico
* **Arquitectura:** Ejecución 100% en el servidor de base de datos (elimina cuellos de botella de red cliente). Sustituto de alto rendimiento de las antiguas utilidades `exp`/`imp`.
* **Utilidades y Granularidad:**
  * Binarios: **`expdp`** (exportación) e **`impdp`** (importación).
  * Alcance: Base de datos completa (`FULL=Y`), esquemas (`SCHEMAS=...`), tablas (`TABLES=...`) o subconjuntos filtrados (`QUERY="WHERE..."`).
* **Capacidades avanzadas de explotación:**
  * Ejecución multihilo (`PARALLEL=N`).
  * Transformación en destino: `REMAP_SCHEMA` (cambio de propietario, ej. de Producción a Preproducción) y `REMAP_TABLESPACE`.
  * **Network Link (`NETWORK_LINK`):** Migración directa entre instancias por red a través de un *DB Link* sin generar ficheros *dump* intermedios en disco.

## 6. Estrategia Corporativa en AAPP (RPO y RTO)
* **Métricas objetivo:**
  * **RPO (*Recovery Point Objective*):** Pérdida máxima de datos tolerable (determinada por la frecuencia de backup de *Archive Logs*).
  * **RTO (*Recovery Time Objective*):** Tiempo máximo de indisponibilidad para restaurar el servicio (optimizado con Nivel 1 + BCT).
* **Planificación tipo:**
  * *Semanal (Domingo):* RMAN Incremental Nivel 0 + Export lógico Data Pump de esquemas críticos + validación de integridad (`RMAN VALIDATE`).
  * *Diario (Madrugada):* RMAN Incremental Nivel 1.
  * *Cada 30 min:* Backup de *Archived Redo Logs* (garantiza un RPO $\le 30\text{ min}$).
  * *Trimestral:* Simulacro real de restauración en entorno aislado (exigencia de auditoría ENS).

## 7. Conclusión
La combinación del modo ARCHIVELOG, el motor físico RMAN y la versatilidad lógica de Data Pump proporciona una arquitectura de protección integral. Mientras RMAN asegura la integridad a nivel de bloque, el cifrado y tiempos mínimos de RTO/RPO mediante BCT y PITR, Data Pump agiliza el movimiento lógico de esquemas. Su correcta orquestación y verificación periódica mediante simulacros garantizan la resiliencia de los datos públicos frente a desastres o ciberataques.

--------------

# Tema 9.- El SGBDR Oracle. Backup y Recuperación.

## 1. Introducción

Persistencia BD -> activo crítico. Datos tributarios, padronales, urbanísticos y de gestión de personal en SGBDR -> representan obligaciones legales, derechos ciudadanos y procesos administrativos.
Pérdida o corrupción -> consecuencias jurídicas, económicas y operativas devastadoras.

**Backup y Recuperación (Backup & Recovery)** -> defensa frente amenazas (fallos de hardware, errores humanos, corrupción de datos, ciberataques, desastres naturales o cortes prolongados electricidad)

Backup -> alta complejidad técnica para garantizar la **Consistencia** de los datos respaldados.

## 2. Clasificación de las Estrategias de Backup

### 2.1. Según la naturaleza del dato copiado

*   **Backup Físico:** Copia archivos binarios (datafiles, control files, archived redo logs y el SPFILE). Método principal de protección frente a desastres (**RMAN**, Recovery Manager).
*   **Backup Lógico (Export):** Fichero binario portable en SQL. Migrar datos, exportar esquemas o archivar tablas históricas. **No protege frente a la pérdida de datafiles, control files o redo logs**, herramienta **Data Pump** (`expdp`/`impdp`).

### 2.2. Según el estado de la base de datos

*   **Cold Backup (Backup en frío / Offline):** BD detenida. Copia los archivos físicos. Indisponibilidad total, inaceptable en sistemas 24/7.
*   **Hot Backup (Backup en caliente / Online):** BD abierta y operativa, debe estar configurada en modo **ARCHIVELOG**.

## 3. Modo ARCHIVELOG: La Piedra Angular de la Recuperación

### 3.1. NOARCHIVELOG vs. ARCHIVELOG

*   **Modo NOARCHIVELOG:** Los Online Redo Logs se sobrescriben sin conservar copia. Se pueden perder datos (entornos de desarrollo o pruebas).
*   **Modo ARCHIVELOG:** Antes de sobrescribir un Online Redo Log, **ARCn (Archiver): Proceso de fondo que copia el Redo Log lleno a disco** realiza una copia en (**Archived Redo Logs**). Permite Hot Backups, Recuperación completa (Complete Recovery), Alimentación de Data Guard.

### 3.2. Recuperación en un punto en el tiempo (Point-in-Time Recovery - PITR)

ARCHIVELOG habilita recuperación completa BD hasta un segundo antes del incidente, sin pérdida de datos.

## 4. RMAN (Recovery Manager): La Herramienta de Backup Físico

### 4.1. Concepto y ventajas

**RMAN (Recovery Manager)** gestiona operaciones de backup y recovery físico. RMAN interactúa directamente con el kernel (Conocimiento de la estructura interna, Backups incrementales, Detección de corrupción, Compresión nativa, Cifrado, Catálogo de backups)

### 4.2. Tipos de backup en RMAN (BACKUP AS)

*   **Backup completo (Full Backup):** Base para backups incrementales posteriores.
*   **Backup incremental de nivel 0:** Backup completo para referencia base de backups incrementales de nivel 1.
*   **Backup incremental de nivel 1:** BCT (Block Change Tracking): Fichero que rastrea bloques modificados (acelera backups incrementales)
    *   **Diferencial:** Modificaciones desde el último backup incremental (de nivel 0 o nivel 1). Por defecto.
    *   **Acumulativo:** Modificaciones desde el último backup de nivel 0. Requiere más espacio, más simple.
*   **Backup de archived redo logs:** Liberar espacio.


### 4.3. Operaciones de recuperación con RMAN

*   **RESTORE:** 
*   **RECOVER:** Aplica archived redo logs para BD al punto de recuperación deseado (recovery completo o PITR).
*   **Block Media Recovery:** Permite recuperar un único bloque corrupto dentro de un datafile. Minimiza el impacto en la disponibilidad.
*   **Flashback Database:** No restaura BD pero "rebobina" BD para errores lógicos recientes. Utiliza los Flashback Logs (área de recuperación rápida / FRA)

## 5. Data Pump: La Herramienta de Backup Lógico

### 5.1. Concepto

**Oracle Data Pump**  herramienta de exportación e importación. Crea fichero binario portable (dump file). 

### 5.2. Arquitectura

Server-side (Ejecuta en BD, no cliente). Elimina latencia red. expdp (Export) e impdp (Import).

### 5.3. Niveles de granularidad

*   **Base de datos completa:** `FULL=Y`
*   **Esquema/Usuario:** `SCHEMAS=FINANZAS,RRHH`
*   **Tablas específicas:** `TABLES=EMPLEADO,DEPARTAMENTO`
*   **Con filtros:** `QUERY="WHERE departamento_id = 10"

### 5.4. Funcionalidades avanzadas

*   **Paralelismo:** Múltiples workers
*   **Remap de esquema:** `REMAP_SCHEMA=PROD:TEST` Cambiar esquema/TS en vuelo
*   **Remap de tablespace:** `REMAP_TABLESPACE=TS_PROD:TS_TEST`
*   **Estimación de tamaño:** 
*   **Modo de red (Network Link):** Importar datos desde otra BD.

## 6. Estrategia de Backup Recomendada

Domingo: Nivel 0 (RMAN). Madrugada: Nivel 1 (RMAN). Cada 30 min: Archived Logs. Semanal: Data Pump (Lógico).

Esta estrategia garantiza:
*   **RPO (Recovery Point Objective)** máxima de datos 30 minutos.
*   **RTO (Recovery Time Objective)** reducido: Backups incrementales -> menor restauración y recuperación.

Imprescindible: Simulacros trimestrales y RMAN VALIDATE Semanal.

## 7. Conclusión

Continuidad del servicio SGBDR Oracle = ARCHIVELOG + RMAN + Data Pump.
Control de RPO/RTO evita el incumplimiento normativo y protege los derechos de la ciudadanía frente a contingencias críticas.