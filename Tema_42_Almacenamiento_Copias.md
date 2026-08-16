# Tema 42.- Sistemas de almacenamiento para sistemas grandes y departamentales. DAS, SAN, NAS. Virtualización del almacenamiento. Políticas, procedimientos y métodos de copias de seguridad.

## 1. Introducción

Los sistemas de información de las Administraciones Públicas almacenan volúmenes crecientes de datos: bases de datos del padrón municipal, expedientes electrónicos, imágenes de digitalización certificada, correo electrónico, logs de auditoría. La arquitectura de almacenamiento debe proporcionar capacidad suficiente, rendimiento adecuado, alta disponibilidad y, sobre todo, mecanismos de respaldo que garanticen la recuperación de los datos ante cualquier contingencia.

Este tema analiza las tipologías de almacenamiento (DAS, NAS, SAN), la virtualización del almacenamiento y las políticas y métodos de copias de seguridad.

## 2. Tipologías de Almacenamiento

### 2.1. DAS (Direct Attached Storage)

El almacenamiento se conecta **directamente** al servidor a través de un bus interno (SATA, SAS, NVMe).

*   **Características:** Máximo rendimiento (mínima latencia), dedicado a un único servidor.
*   **Ventajas:** Simplicidad, coste bajo, máxima velocidad de acceso.
*   **Inconvenientes:** No compartible entre servidores, escalabilidad limitada por los slots del servidor, punto único de fallo.
*   **Uso:** Servidores pequeños, estaciones de trabajo, almacenamiento temporal.

### 2.2. NAS (Network Attached Storage)

Dispositivo de almacenamiento independiente conectado a la **red LAN** (Ethernet). Ofrece almacenamiento compartido a nivel de **sistema de ficheros**.

*   **Protocolos:** NFS (Linux/Unix), CIFS/SMB (Windows).
*   **Acceso:** Cualquier servidor o PC de la red puede acceder a las carpetas compartidas.
*   **Ventajas:** Compartición de ficheros entre múltiples servidores, fácil gestión, escalabilidad.
*   **Inconvenientes:** Rendimiento limitado por la red Ethernet (latencia mayor que DAS/SAN), no es óptimo para bases de datos de alto rendimiento.
*   **Uso:** Almacenamiento de ficheros compartidos, copias de seguridad, documentos de usuario, repositorios de imágenes.
*   **Productos:** Synology, QNAP, NetApp (gama NAS), Dell EMC Isilon.

### 2.3. SAN (Storage Area Network)

Red de almacenamiento **dedicada y separada** de la LAN, diseñada para proporcionar almacenamiento a nivel de **bloques** con máximo rendimiento.

*   **Protocolos:**
    *   **Fibre Channel (FC):** Protocolo nativo de SAN. Velocidades de 16/32/64 Gbps. Latencia ultrabaja.
    *   **iSCSI:** Protocolo de bloques sobre Ethernet TCP/IP. Menor coste que FC, rendimiento aceptable con redes 10/25 GbE.
    *   **FCoE (Fibre Channel over Ethernet):** Encapsula FC sobre Ethernet, convergiendo las redes de datos y almacenamiento.
*   **Acceso a nivel de bloques:** El servidor "ve" los discos de la SAN como si fueran discos locales. El sistema operativo monta sobre ellos su sistema de ficheros.
*   **Ventajas:** Máximo rendimiento (I/O intensivo), compartición entre múltiples servidores (clústeres), alta disponibilidad (paths redundantes — multipath I/O).
*   **Inconvenientes:** Coste elevado (switches FC, HBA — Host Bus Adapters), complejidad de administración.
*   **Uso:** Bases de datos de alto rendimiento (Oracle, SQL Server), servidores de aplicaciones, virtualización (datastores VMware).
*   **Productos:** Dell EMC PowerStore/Unity, NetApp AFF, HPE 3PAR/Primera, IBM FlashSystem.

### 2.4. Comparativa DAS vs. NAS vs. SAN

| Aspecto | DAS | NAS | SAN |
|---------|-----|-----|-----|
| Nivel de acceso | Bloques (local) | Ficheros (red) | Bloques (red dedicada) |
| Red | No aplica (bus interno) | LAN Ethernet | Red FC o iSCSI dedicada |
| Compartición | No | Sí (multiusuario) | Sí (multisistema) |
| Rendimiento | Alto (local) | Medio (Ethernet) | Muy alto (FC) |
| Coste | Bajo | Medio | Alto |
| Uso típico | Servidor individual | Ficheros compartidos | BBDD, virtualización |

## 3. Virtualización del Almacenamiento

### 3.1. Concepto

La **virtualización del almacenamiento** abstrae el almacenamiento físico (discos, cabinas, arrays) en un pool lógico unificado. Los administradores gestionan volúmenes lógicos sin preocuparse de en qué cabina o disco físico residen los datos.

### 3.2. Tecnologías

*   **RAID (Redundant Array of Independent Disks):** Agrupación de discos físicos en una unidad lógica con redundancia:

| Nivel RAID | Descripción | Discos mínimos | Tolerancia a fallos |
|------------|-------------|----------------|---------------------|
| **RAID 0** | Striping (sin redundancia) | 2 | Ninguna |
| **RAID 1** | Mirroring (espejo) | 2 | 1 disco |
| **RAID 5** | Striping + paridad distribuida | 3 | 1 disco |
| **RAID 6** | Striping + doble paridad | 4 | 2 discos |
| **RAID 10** | Combinación de RAID 1 + RAID 0 | 4 | 1 disco por par |

*   **LVM (Logical Volume Manager):** Gestión de volúmenes lógicos en Linux que permite redimensionar particiones en caliente.
*   **Storage Pools (ZFS, Btrfs):** Sistemas de ficheros avanzados que integran RAID, compresión, deduplicación y snapshots.
*   **Software-Defined Storage (SDS):** El almacenamiento se gestiona completamente por software, independiente del hardware:
    *   **VMware vSAN:** Almacenamiento distribuido integrado en los hosts ESXi.
    *   **Ceph:** Almacenamiento distribuido open source (bloques, ficheros, objetos).
    *   **GlusterFS:** Sistema de ficheros distribuido open source.

### 3.3. Thin Provisioning

Técnica que asigna a un volumen lógico más capacidad de la que físicamente existe (sobre-aprovisionamiento controlado). El espacio físico se consume solo cuando se escriben datos reales, optimizando el uso del almacenamiento.

## 4. Copias de Seguridad (Backup)

### 4.1. Importancia y obligación legal

El ENS (medidas op.exp.6 y mp.info.6 del RD 311/2022) exige que las Administraciones Públicas dispongan de **políticas de copias de seguridad** que garanticen la recuperación de los datos y los servicios ante incidentes (fallo de hardware, ransomware, error humano, desastre natural).

### 4.2. Regla 3-2-1

La regla de oro del backup:
*   **3** copias de cada dato (la original + 2 copias de seguridad).
*   **2** tipos de soporte diferentes (disco + cinta, disco + cloud).
*   **1** copia en una ubicación remota (fuera del edificio, en un CPD de respaldo).

### 4.3. Tipos de copias de seguridad

| Tipo | Descripción | Ventajas | Inconvenientes |
|------|-------------|----------|----------------|
| **Completa (Full)** | Copia de todos los datos | Restauración rápida y sencilla | Consume mucho tiempo, espacio y ancho de banda |
| **Incremental** | Solo copia los datos modificados desde la **última copia (de cualquier tipo)** | Rápida, consume poco espacio | Restauración lenta (requiere la Full + todas las incrementales) |
| **Diferencial** | Solo copia los datos modificados desde la **última copia completa (Full)** | Restauración más rápida que incremental | Consume más espacio que incremental (crece cada día) |

### 4.4. Ejemplo de esquema semanal

| Día | Tipo de backup | Datos respaldados |
|-----|---------------|-------------------|
| Domingo | **Full** | Todos los datos |
| Lunes | Incremental | Cambios desde el domingo |
| Martes | Incremental | Cambios desde el lunes |
| Miércoles | Incremental | Cambios desde el martes |
| Jueves | Incremental | Cambios desde el miércoles |
| Viernes | Incremental | Cambios desde el jueves |
| Sábado | Incremental | Cambios desde el viernes |

Para restaurar el estado del jueves: se restaura la Full del domingo + incrementales del lunes, martes, miércoles y jueves.

### 4.5. Copias de seguridad de bases de datos

*   **Oracle RMAN (Recovery Manager):** Herramienta nativa de Oracle para backups en caliente (sin parar la BBDD). Soporta Full, Incremental Nivel 0 (base) e Incremental Nivel 1 (cambios desde el último Nivel 0 o Nivel 1). Block change tracking para optimizar.
*   **pg_dump / pg_basebackup:** Herramientas nativas de PostgreSQL.
*   **SQL Server Backup:** Full, Differential, Transaction Log Backup.

### 4.6. Soportes de almacenamiento

| Soporte | Características |
|---------|----------------|
| **Disco (D2D — Disk-to-Disk)** | Rapidez, deduplicación en línea. Coste medio. Productos: Dell EMC Data Domain, Veeam |
| **Cinta (LTO — Linear Tape-Open)** | Alta capacidad (18 TB/cinta en LTO-9), coste por TB muy bajo, vida útil 30+ años. Ideal para archivo a largo plazo |
| **Cloud** | Almacenamiento fuera de las instalaciones. Pay-per-use. AWS S3, Azure Blob, Google Cloud Storage |

### 4.7. Deduplicación

La **deduplicación** es una técnica de optimización que identifica bloques de datos duplicados y almacena una sola copia, sustituyendo las copias redundantes por punteros. Un algoritmo hash (SHA-256) identifica cada bloque. Si 1.000 funcionarios tienen el mismo archivo en sus buzones de correo, se almacena una sola instancia.

Tipos:
*   **Deduplicación en origen (source):** Se realiza en el servidor antes de enviar los datos al almacenamiento de backup. Reduce el tráfico de red.
*   **Deduplicación en destino (target):** Se realiza en el dispositivo de backup tras recibir los datos.

### 4.8. Snapshots y CDP

*   **Snapshot:** Captura instantánea del estado de un volumen de almacenamiento o una VM en un momento dado. No es un backup completo, sino una referencia al estado del sistema que permite restauraciones rápidas. Utilizado en cabinas SAN y hipervisores.
*   **CDP (Continuous Data Protection):** Protección continua que registra cada cambio en los datos en tiempo real, permitiendo restaurar a cualquier punto en el tiempo (RPO ~ 0).

### 4.9. RPO y RTO

| Métrica | Descripción | Ejemplo |
|---------|-------------|---------|
| **RPO (Recovery Point Objective)** | Cantidad máxima de datos que la organización acepta perder | RPO = 24h: se acepta perder hasta un día de datos |
| **RTO (Recovery Time Objective)** | Tiempo máximo aceptable para restaurar el servicio | RTO = 4h: el servicio debe estar operativo en 4 horas |

El RPO determina la frecuencia de los backups; el RTO determina la tecnología de restauración necesaria.

## 5. Plan de Recuperación ante Desastres (DRP)

El **DRP (Disaster Recovery Plan)** es el plan documentado que define los procedimientos para restaurar los sistemas de información tras un desastre (incendio, inundación, ciberataque destructivo). Incluye:
*   Priorización de sistemas según criticidad.
*   RPO y RTO definidos para cada sistema.
*   Ubicación del CPD de respaldo (Hot/Warm/Cold Site — Tema 39).
*   Procedimientos de restauración paso a paso.
*   Pruebas periódicas del DRP (al menos anuales).

## 6. Conclusión

La arquitectura de almacenamiento de una Administración Pública combina distintas tecnologías según las necesidades: DAS para servidores individuales, NAS para almacenamiento compartido de ficheros, y SAN para bases de datos y entornos de virtualización que requieren máximo rendimiento de E/S. La virtualización del almacenamiento (RAID, LVM, SDS) y técnicas como el thin provisioning optimizan el uso de los recursos.

Las copias de seguridad — organizadas según la regla 3-2-1, con esquemas de backup completos, incrementales y diferenciales, soportadas por la deduplicación y conformes a los RPO/RTO definidos — constituyen la última línea de defensa contra la pérdida de datos. El Plan de Recuperación ante Desastres (DRP) garantiza la continuidad de los servicios públicos digitales conforme a los requisitos del ENS.
