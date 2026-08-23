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