# Tema 42.- Sistemas de almacenamiento para sistemas grandes y departamentales. DAS, SAN, NAS. Virtualización del almacenamiento. Políticas, procedimientos y métodos de copias de seguridad.

## 1. Introducción
* Las AAPP generan un volumen masivo de datos (Expedientes, Sede Electrónica, BBDD).
* El reto es equilibrar **Rendimiento** (para acceso rápido) con **Disponibilidad y Recuperación** estricta conforme dicta el Esquema Nacional de Seguridad (ENS).

## 2. Tipologías de Almacenamiento
* **2.1. DAS (Direct Attached Storage):** 
  * Discos directamente conectados al servidor (SATA/SAS/NVMe). 
  * *Ventajas:* Máximo rendimiento, Baja latencia, Bajo coste. *Inconvenientes:* No se comparte -> islas de datos, Escalabilidad limitada, Punto único de fallo.
  * *Uso AAPP:* Servidores pequeños, estaciones de trabajo
* **2.2. NAS (Network Attached Storage):**
  * Acceso por **Red LAN** a nivel de **Ficheros** (NFS en Linux, Samba en Windows).
  * *Ventajas:* Compartir ficheros entre servidores, escalabilidad. *Inconvenientes:* Ethernet -> latencia mayor
  * *Uso AAPP:* Carpetas departamentales, documentos de usuario (Synology, QNAP, Dell).
* **2.3. SAN (Storage Area Network):**
  * Red de fibra **dedicada y separada** de la LAN, para acceso a nivel de **Bloques**. El servidor lo ve como un disco local "bruto".
  * *Protocolos:* **Fibre Channel (FC)** (rendimiento premium) o **iSCSI** (sobre IP, más barato).
  * *Ventajas:* Máximo rendimiento (I/O intensivo). *Inconvenientes:* Coste elevado (switches FC, HBA — Host Bus Adapters)
  * *Uso AAPP:* Datastores de virtualización (VMware/Hyper-V) y BBDD pesadas (Oracle, SQL Server).
  * ⚠️ *(Auto-Tiering):* Las cabinas modernas (ej. NetApp, Dell Unity) mueven automáticamente los datos "calientes" a discos NVMe caros y los "fríos" a discos magnéticos baratos.

## 3. Virtualización del Almacenamiento
Abstrae el almacenamiento físico (discos y cabinas) en un pool lógico unificado, se gestionan volumenes sin saber en que disco fisico residen.
- **3.1. RAID (Redundancia Física):** Agrupación de discos físicos en una unidad lógica.
  * **RAID 0 (Striping / Reparto):** Reparte los archivos entre los discos. Máxima velocidad, pero 0 redundancia (si cae uno, se pierde todo). Mínimo 2 discos.
  * **RAID 1 (Mirror / Espejo):** Clon exacto y simultáneo. Tolerancia a la pérdida de 1 disco. Mínimo 2 discos.
  * **RAID 5 (Paridad distribuida):** Reparte archivos entre discos y guarda cálculo matemático. Si muere 1 disco, lo reconstruye "adivinando" los datos. Mínimo 3 discos. Habitual en cabinas de almacenamiento.
  * **RAID 10 (Espejos + Reparto):** Combina RAID 1 y RAID 0. Máxima velocidad y máxima seguridad (ideal para Bases de Datos como Oracle). Mínimo 4 discos.
* **3.2. Software-Defined Storage (SDS):** VMware vSAN o Ceph (open source). El software une por red los discos locales de decenas de servidores distintos para crear una única "súper cabina" virtual.
* **3.3. Thin Provisioning:** Técnica que asigna a un volumen lógico más capacidad de la que físicamente existe. El SO cree que tiene un disco de 1TB, pero la cabina solo consume el espacio real que se va escribiendo. Maximiza el ahorro.

## 4. Copias de Seguridad (El escudo del Ayuntamiento)
ENS exige *políticas de copias de seguridad* que garanticen la recuperación de los datos y los servicios ante incidentes.
* **4.1. La Regla 3-2-1-1-0 (Recomendación CCN-CERT contra Ransomware):**
  * **3** copias de los datos.
  * En **2** soportes diferentes (Disco y Nube/Cinta LTO para retención a largo plazo).
  * **1** copia Off-Site (fuera del edificio, ej. Cloud AWS/Azure/Google).
  * ⚠️ **1** copia **INMUTABLE** (WORM): El backup se bloquea por hardware/software durante X días para que ni un administrador comprometido ni un ransomware puedan borrarlo.
  * **0** errores en la restauración (pruebas periódicas obligatorias).
* **4.2. Tipos de Copia:**
  * **Full:** Todo. Restaura rapidísimo. Tarda y ocupa mucho. (Ej: Domingo)
  * **Incremental:** Cambios desde el último backup (sea full o no). Ocupa poco. Restaura lento (hay que sumar la Full + todas las incrementales encadenadas). (Ej: Lunes a Sábado)
  * **Diferencial:** Cambios desde el último FULL. Término medio.
* **4.3. Backups de BBDD en caliente:**
  * Las BBDD exigen copias consistentes sin paradas. Herramientas nativas como **RMAN (Oracle)** o **pg_basebackup (PostgreSQL)**.
* **4.4. Tecnologías de Ahorro y Agilidad:**
  * **Deduplicación:** Elimina bloques repetidos (en origen o en destino) usando Hash SHA-256. Ahorra hasta un 90% de espacio.
  * **Snapshots y CDP:** Foto instantánea de la VM (*Snapshot*) y protección de datos en tiempo real (*CDP (Continuous Data Protection)* para RPO=0). Ideal para actualizar y dar marcha atrás al instante. (*Nota: Un snapshot NO sustituye a un backup off-site*).

## 5. DRP (Disaster Recovery Plan) y Métricas Vitales
* El Plan de Continuidad de Negocio municipal exige medir:
  * **RPO (Recovery Point Objective):** Cuántos datos históricos estoy dispuesto a perder (El "cuándo"). Define la frecuencia del backup.
  * **RTO (Recovery Time Objective):** Cuánto tiempo tolero que la Sede Electrónica esté caída hasta que levante el backup (El "cuánto"). Define la velocidad del hardware.

## 6. Conclusión
La arquitectura de almacenamiento municipal no puede ser plana. Requiere una SAN para virtualización ágil, apoyada por redes NAS para el usuario final. Todo orquestado bajo un plan de contingencia estricto (RPO/RTO) que aplique políticas de backup inmutables como última línea de defensa técnica exigida por el Esquema Nacional de Seguridad frente a ciberataques de índole extorsiva (ransomware).

-----------------------------

# Tema 42.- Sistemas de almacenamiento para sistemas grandes y departamentales. DAS, SAN, NAS. Virtualización del almacenamiento. Políticas, procedimientos y métodos de copias de seguridad.

## 1. Introducción

Las AAPP necesitan almacenamiento con **capacidad, rendimiento, alta disponibilidad y recuperación ante fallos**.

Principales tecnologías:
- **DAS:** almacenamiento directo al servidor.
- **NAS:** almacenamiento de ficheros mediante red LAN.
- **SAN:** almacenamiento de bloques mediante red dedicada.
- **Backup:** protección y recuperación de los datos.

## 2. Tipologías de Almacenamiento

### 2.1. DAS (Direct Attached Storage)

Almacenamiento conectado **directamente al servidor** mediante SATA, SAS o NVMe.

**Características:**
- Acceso local y baja latencia.
- Dedicado a un único servidor.

**Ventajas:**
- Simplicidad.
- Bajo coste.
- Alto rendimiento.

**Inconvenientes:**
- No compartible entre servidores.
- Escalabilidad limitada.
- Puede constituir un punto único de fallo.

**Uso:** servidores pequeños, estaciones de trabajo y almacenamiento temporal.

### 2.2. NAS (Network Attached Storage)

Dispositivo conectado a la **LAN** que proporciona almacenamiento compartido a nivel de **ficheros**.

**Protocolos:**
- **NFS:** Linux/Unix.
- **SMB/CIFS:** Windows.

**Ventajas:**
- Compartición de ficheros.
- Fácil administración.
- Escalabilidad.

**Inconvenientes:**
- Dependencia de la red Ethernet.
- Mayor latencia que DAS/SAN.
- Menos adecuado para BBDD de alto rendimiento.

**Uso:** ficheros compartidos, documentos, backups e imágenes (Synology, QNAP, NetApp (gama NAS), Dell EMC Isilon)

### 2.3. SAN (Storage Area Network)

Red de almacenamiento **dedicada y separada de la LAN**, que proporciona acceso a nivel de **bloques**.

**Protocolos:**
- **Fibre Channel (FC):** alto rendimiento y baja latencia. Protocolo nativo SAN
- **iSCSI:** bloques sobre Ethernet/TCP-IP. Menor coste.
- **FCoE:** Fibre Channel sobre Ethernet.

**Características:**
- El servidor ve los volúmenes SAN como discos locales.
- Permite acceso desde múltiples servidores.
- Puede utilizar **multipath I/O** para redundancia.

**Ventajas:**
- Máximo rendimiento de E/S 
- Alta disponibilidad.
- Adecuada para clústeres.

**Inconvenientes:**
- Coste elevado (switches FC, HBA — Host Bus Adapters)
- Mayor complejidad de administración.

**Uso:** BBDD, virtualización y servidores de aplicaciones (Dell EMC PowerStore/Unity, NetApp AFF, HPE 3PAR/Primera, IBM FlashSystem.)

### 2.4. Comparativa DAS vs. NAS vs. SAN

| Aspecto | DAS | NAS | SAN |
|---------|-----|-----|-----|
| Nivel de acceso | Bloques (local) | Ficheros (red) | Bloques (red dedicada) |
| Red | No aplica (bus interno) | LAN Ethernet | Red FC o iSCSI dedicada |
| Compartición | No | Sí (multiusuario) | Sí (multisistema) |
| Rendimiento | Alto (local) | Medio (Ethernet) | Muy alto (FC) |
| Coste | Bajo | Medio | Alto |
| Uso típico | Servidor individual | Ficheros compartidos | BBDD, virtualización |

  * ⚠️ *Detalle Táctico (Auto-Tiering):* Las cabinas modernas (ej. NetApp, Dell Unity) mueven automáticamente los datos "calientes" a discos NVMe caros y los "fríos" a discos magnéticos baratos.

## 3. Virtualización del Almacenamiento

### 3.1. Concepto

La **virtualización del almacenamiento** abstrae discos y cabinas físicas en un **pool lógico**, permitiendo gestionar volúmenes sin depender directamente del hardware físico.

### 3.2. Tecnologías

- **RAID (Redundant Array of Independent Disks):** agrupa discos físicos proporcionando rendimiento y/o redundancia.
  - **RAID 0:** Striping, sin redundancia.
  - **RAID 1:** Mirroring (espejo), tolerancia 1 disco.
  - **RAID 5:** Striping + paridad, tolerancia 1 disco.
  - **RAID 6:** Doble paridad, tolerancia 2 discos.
  - **RAID 10:** RAID 1 + RAID 0, alto rendimiento y redundancia.

- **LVM (Logical Volume Manager):** gestión de volúmenes lógicos en Linux; permite redimensionamiento en caliente.

- **Storage Pools:** agrupación lógica de almacenamiento.
  - ZFS (Solaris).
  - Btrfs (Linux).
  - Permiten funciones como RAID, snapshots, compresión y deduplicación.

- **Software-Defined Storage (SDS):** almacenamiento gestionado por software independientemente del hardware.
  - VMware vSAN.
  - Ceph.
  - GlusterFS.

### 3.3. Thin Provisioning

Asigna a un volumen lógico **más capacidad de la físicamente disponible**.

El espacio físico se consume **solo cuando realmente se escriben datos**, optimizando el almacenamiento.

## 4. Copias de Seguridad (Backup)

### 4.1. Importancia y obligación legal

El **ENS (RD 311/2022)** exige políticas de backup que permitan recuperar datos y servicios ante:
- Fallos hardware.
- Ransomware.
- Errores humanos.
- Desastres naturales.

### 4.2. La Regla 3-2-1-1-0 (Recomendación CCN-CERT contra Ransomware)

* **3** copias de los datos.
* En **2** soportes diferentes (Disco y Nube/Cinta LTO).
* **1** copia Off-Site (fuera del edificio).
* ⚠️ **1** copia **INMUTABLE** (WORM): El backup se bloquea por hardware/software durante X días para que ni un administrador comprometido ni un ransomware puedan borrarlo.
* **0** errores en la restauración (pruebas periódicas obligatorias).

### 4.3. Tipos de copias de seguridad

**Completa (Full):**
- Copia todos los datos.
- Restauración rápida.
- Mayor consumo de tiempo y espacio.

**Incremental:**
- Copia cambios desde la **última copia de cualquier tipo**.
- Rápida y ocupa poco.
- Restauración más compleja → Full + todas las incrementales.

**Diferencial:**
- Copia cambios desde la **última Full**.
- Restauración más rápida → Full + última diferencial.
- Ocupa progresivamente más espacio.

### 4.4. Ejemplo de esquema semanal

- **Domingo → Full.**
- **Lunes-Sábado → Incremental.**

Para restaurar el jueves:
**Full domingo + Incrementales lunes + martes + miércoles + jueves.**

### 4.5. Copias de seguridad de bases de datos

- **Oracle RMAN:** backups en caliente, Full e Incrementales.
- **PostgreSQL:** pg_dump / pg_basebackup.
- **SQL Server:** Full, Differential y Transaction Log.

### 4.6. Soportes de almacenamiento

- **Disco (D2D — Disk-to-Disk):** rápido, permite deduplicación; coste medio (Dell EMC Data Domain, Veeam)
- **Cinta (LTO — Linear Tape-Open):** gran capacidad y bajo coste/TB; adecuada para archivo a largo plazo.
- **Cloud:** almacenamiento externo bajo modelo de pago por uso (AWS S3, Azure Blob, Google Cloud Storage)

### 4.7. Deduplicación

Elimina bloques de datos duplicados almacenando **una sola copia**, sustituyendo las copias redundantes por punteros.

- **En origen (Source):** antes de enviar los datos al backup → reduce tráfico de red.
- **En destino (Target):** en el dispositivo de backup → recibe primero los datos.

### 4.8. Snapshots y CDP

- **Snapshot:** captura instantánea del estado de un volumen o VM.
  - Restauración rápida.
  - **No sustituye a un backup completo.**

- **CDP (Continuous Data Protection):**
  - Registra continuamente los cambios.
  - Permite recuperar a cualquier punto temporal.
  - **RPO cercano a 0.**

### 4.9. RPO y RTO

- **RPO (Recovery Point Objective):**
  - Máxima cantidad de datos que se acepta perder.
  - Determina la **frecuencia de los backups**.
  - Ej.: RPO = 24 h → se acepta perder hasta 24 h de datos.

- **RTO (Recovery Time Objective):**
  - Tiempo máximo para recuperar el servicio.
  - Determina la **tecnología de restauración necesaria**.
  - Ej.: RTO = 4 h → servicio operativo en máximo 4 h.

## 5. Plan de Recuperación ante Desastres (DRP)

El **DRP (Disaster Recovery Plan)** define cómo recuperar los sistemas después de un desastre.

Incluye:
- **Priorización** de sistemas según criticidad.
- **RPO y RTO** de cada sistema.
- **CPD de respaldo:** Hot / Warm / Cold Site.
- Procedimientos de restauración.
- **Pruebas periódicas** del plan.

## 6. Conclusión

- **DAS →** almacenamiento directo y local.
- **NAS →** ficheros compartidos por LAN.
- **SAN →** bloques mediante red dedicada.
- **RAID/LVM/SDS →** optimización y virtualización del almacenamiento.
- **3-2-1 →** regla básica de backup.
- **Full / Incremental / Diferencial →** principales tipos de backup.
- **RPO →** cuánto dato podemos perder.
- **RTO →** cuánto tiempo podemos estar sin servicio.
- **DRP →** plan para recuperar los sistemas ante desastres.

El objetivo final es garantizar **disponibilidad, integridad, recuperación y continuidad de los servicios públicos** conforme al ENS.