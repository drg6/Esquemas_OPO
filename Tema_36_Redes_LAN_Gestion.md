# Tema 36.- Redes de área local (LAN). Gestión de dispositivos y Administración de redes.

## 1. Introducción
* **Concepto:** Las redes LAN conectan dispositivos en un ámbito geográfico limitado (un edificio municipal o campus) con altísima velocidad (1-10 Gbps) y baja latencia.
* **El reto:** Administrar cientos de puestos, switches y APs requiere herramientas de monitorización y control (NAC) para garantizar disponibilidad y cumplir el ENS.

## 2. Fundamentos de las Redes LAN
* **2.1. Arquitectura Jerárquica (Estrella extendida):** Es el estándar de diseño actual. Divide la red en tres capas físicas: **Núcleo (Core) → Distribución → Acceso**.
* **2.2. Ethernet (IEEE 802.3) y Medios:**
  * 1000BASE-T (1 Gbps - Cat5e/6).
  * 10GBASE-T (10 Gbps - Cat6a).
  * Enlaces *Backbone* de fibra óptica entre los armarios de planta y el CPD central.
* **2.3. Wi-Fi (IEEE 802.11):** 
  * Estándares: Wi-Fi 5 (802.11ac), Wi-Fi 6/6E (802.11ax).
  * Seguridad: Implementación obligatoria de **WPA3-Enterprise** (con RADIUS).

## 3. Gestión y Monitorización de Dispositivos
* **3.1. SNMP (Simple Network Management Protocol):** 
  * Opera por UDP (161/162).
  * *Componentes:* **NMS** (El servidor gestor), **Agente** (en el switch) y **MIB** (el árbol jerárquico de variables con sus OID).
  * *Versiones:* SNMPv1/v2c (texto claro) vs **SNMPv3 (Autenticación y cifrado, obligatorio ENS)**.
* **3.2. Herramientas NMS (Network Management System):** Zabbix, Nagios (Open Source), PRTG, SolarWinds (Comerciales). Sirven para hacer *polling* (consultas) y recibir *traps* (alertas).
* **3.3. Syslog y NetFlow:** 
  * **Syslog:** Envío centralizado de logs a un servidor SIEM para auditoría.
  * **NetFlow/sFlow:** Análisis profundo de tráfico (quién habla con quién y qué volumen) para detectar anomalías o DDoS.

## 4. Administración de Redes LAN
* **4.1. Segmentación:** División en VLANs (separación lógica) + Subredes IP + ACLs en los routers inter-VLAN.
* **4.2. Control de Acceso a la Red (NAC) e IEEE 802.1X:**
  * Arquitectura que impide el acceso a la red de dispositivos no autorizados en la propia roseta (puerto físico) o Wi-Fi.
  * *Componentes:* **Suplicante** (Equipo), **Autenticador / NAS** (Switch) y **Servidor** (RADIUS).
  * *Implementación real en AAPP:*
    * **1. Autenticación 802.1X:** Mediante certificados o credenciales (EAP).
    * **2. MAB (MAC Authentication Bypass):** Muy usado para simplificar el alta de PCs e impresoras. El switch envía la MAC al RADIUS; si no está en la "lista blanca", el equipo no tiene red.
  * *Acciones del NAC:* Cuando el equipo es autorizado, el NAC puede abrir simplemente el puerto (**Autorización de Puerto** con la VLAN estática de esa planta), o realizar una **Asignación Dinámica de VLAN** según el perfil del usuario. Los equipos no autorizados quedan en una VLAN de Cuarentena o aislados sin acceso (Drop).
* **4.3. Servicios de Red Base:** 
  * **DHCP:** Asignación dinámica de IPs para evitar conflictos manuales.
  * **DNS:** Resolución de nombres locales (integrado con Active Directory).
* **4.4. Alta Disponibilidad (Resiliencia):**
  * **STP / RSTP (Spanning Tree):** Evita bucles catastróficos en Capa 2 cuando hay cables redundantes.
  * **LACP (Link Aggregation):** Suma varios cables físicos en un único enlace lógico de más ancho de banda.
  * **Stacking:** Apilar varios switches físicos para que se comporten y administren como uno solo.

## 5. Conclusión
La red LAN de un Ayuntamiento ha dejado de ser un simple conjunto de cables para convertirse en una infraestructura inteligente. El uso de arquitecturas jerárquicas con Ethernet/Wi-Fi 6, monitorizadas cifradamente mediante SNMPv3 y aseguradas perimetralmente desde la misma roseta de la pared mediante políticas NAC (802.1X), es la única vía para garantizar una administración de red alineada con los altísimos requisitos del Esquema Nacional de Seguridad.

---------------------------------

# Tema 36.- Redes. Redes de área local. Gestión de dispositivos. Administración de redes LAN.

## 1. Introducción

- **LAN (Local Area Network)** → conecta dispositivos en un área limitada: edificio, oficinas, campus.
- Características:
  - **Alta velocidad**.
  - **Baja latencia**.
- Una LAN municipal requiere **gestión, monitorización, seguridad y control de acceso**.

## 2. Fundamentos de las Redes LAN

### 2.1. Topologías

- **Bus** → todos comparten un cable. **Obsoleta**.
- **Anillo** → comunicación mediante testigo. **Obsoleta**.
- **Estrella** → dispositivos conectados a un **switch central**. **Dominante actual**.
- **Estrella extendida/jerárquica** → switches de **acceso → distribución → core**. Típica de organizaciones grandes.

### 2.2. Ethernet (IEEE 802.3)

- **Ethernet** → estándar principal de LAN cableadas.
- Evolución: **10 Mbps (Cat3) → 100 Mbps (Cat5) → 1 Gbps (Cat5e/Cat6) → 10 Gbps (Cat6a/Cat7) → 25/40/100 Gbps (Fibra)**.
- Medios: **par trenzado y fibra óptica**.

### 2.3. Cableado estructurado

Basado en **TIA/EIA-568**:

- **Área de trabajo** → tomas de red.
- **Cableado horizontal** → toma ↔ armario telecomunicaciones de planta.
- **Armario de telecomunicaciones** → switches.
- **Backbone/vertical** → interconecta armarios, normalmente mediante fibra.
- **CPD/sala de equipos** → Switches de núcleo (core), routers y servidores.

### 2.4. Wi-Fi (IEEE 802.11)

- Complementa a la LAN cableada.
- **Wi-Fi 4 → 802.11n**
- **Wi-Fi 5 → 802.11ac**
- **Wi-Fi 6/6E → 802.11ax**
- Seguridad recomendada: **WPA3-Enterprise + 802.1X + RADIUS**.

## 3. Gestión y Monitorización de Dispositivos

### 3.1. SNMP (Simple Network Management Protocol)

- Protocolo estándar para **gestionar y monitorizar dispositivos de red**.
- Usa **UDP 161/162**.

**Componentes:**
- **Agente** → reside en el dispositivo y proporciona información.
- **MIB (Management Information Base)** → estructura que define las variables gestionables.
- **NMS (Network Management System / Gestor)** → sistema central que consulta agentes y recibe alertas.

**Versiones:**
- SNMPv1 → sin cifrado.
- SNMPv2c → mejoras rendimiento, pero sin cifrado.
- **SNMPv3 → autenticación + cifrado → recomendado ENS**.

### 3.2. Herramientas de monitorización

- **Nagios** → monitorizacion de servicios, hosts y alertas.
- **Zabbix** → monitorización, gráficos y autodescubrimiento.
- **Grafana** → dashboards y métricas.
- **PRTG / SolarWinds** → soluciones comerciales.
- **Cacti** → gráficos de tráfico.

### 3.3. Syslog

- Centraliza los **logs** de switches, routers, firewalls y servidores.
- Permite **almacenamiento, análisis y correlación**.
- Puede integrarse con un **SIEM**.

### 3.4. NetFlow / sFlow

- Analizan el **tráfico de red**:
  - IP origen/destino.
  - Puertos.
  - Volumen.
  - Duración.
- Útiles para detectar **anomalías, DDoS y usos indebidos**.

## 4. Administración de Redes LAN

### 4.1. Segmentación de red

- **VLANs** → separan dominios de seguridad  y optimizan rendimiento.
- **Subredes IP** → asignan rangos IP a cada VLAN.
- **ACL** → controlan tráfico entre redes/VLANs.
- **Firewalls** → filtrado avanzado y control del tráfico.

### 4.2. Control de Acceso a la Red (NAC) e IEEE 802.1X

- **NAC (Network Access Control)** → impide acceso a la red a dispositivos **no autorizados o comprometidos**.
- Se apoya en **IEEE 802.1X** → control de acceso basado en puerto.

**Componentes:**
- **Suplicante** → PC/dispositivo que solicita acceso.
- **Autenticador (NAS)** → switch/AP que bloquea inicialmente el tráfico salvo **EAP**.
- **RADIUS** → servidor que valida credenciales o certificado **X.509**, normalmente contra **Active Directory**.

**Implementación:**
1. **802.1X** → credenciales o certificado **X.509**.
2. **MAB (MAC Authentication Bypass)** → para dispositivos sin suplicante (impresoras, teléfonos IP, etc.).
   - Switch obtiene **MAC** → RADIUS → comprueba **lista blanca**.

**Acciones del NAC:**
- **Autorización de puerto** → permite el tráfico.
- **VLAN dinámica** → RADIUS asigna automáticamente la **VLAN según el perfil** del usuario.
- **Equipo desconocido** → **bloqueo (Drop)** o **VLAN de cuarentena**.

### 4.3. DHCP (Dynamic Host Configuration Protocol)

- Configura automáticamente:
  - **IP**.
  - **Máscara**.
  - **Puerta de enlace**.
  - **DNS**.
- Evita configuración manual.

### 4.4. DNS (Domain Name System)

- Traduce **nombres ↔ direcciones IP**.
- En redes corporativas puede integrarse con **Active Directory**.
- Permite resolver nombres de servidores, impresoras y servicios internos.

### 4.5. Alta disponibilidad en la LAN

- **Spanning Tree Protocol STP/RSTP** → evita **bucles** entre switches.
- **Link Aggregation  LACP (IEEE 802.3ad)** → agrupa enlaces físicos en uno lógico.
- **Switches redundantes  Stacking/Clustering** → varios switches funcionan como una unidad con redundancia/failover.

## 5. Conclusión

- LAN moderna → **Ethernet + estrella jerárquica + VLANs**.
- Gestión → **SNMPv3 + NMS + Syslog + NetFlow/sFlow**.
- Seguridad → **802.1X + RADIUS + ACL + Firewall**.
- Disponibilidad → **STP/RSTP + LACP + redundancia**.
- Todo ello debe cumplir los requisitos del **ENS** y las guías **CCN-STIC**.

🧠 **LAN = CONECTAR → SEGMENTAR → MONITORIZAR → AUTENTICAR → ASEGURAR**
**Ethernet** conecta · **VLAN** segmenta · **SNMP** monitoriza · **802.1X** autentica · **STP/LACP** aseguran disponibilidad.

