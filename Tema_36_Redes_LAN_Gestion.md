# Tema 36.- Redes. Redes de área local. Gestión de dispositivos. Administración de redes LAN.

## 1. Introducción

Las **Redes de Área Local (LAN — Local Area Network)** conectan dispositivos informáticos dentro de un ámbito geográfico limitado: un edificio municipal, un conjunto de oficinas adyacentes o un campus administrativo. Su característica distintiva frente a las redes metropolitanas (MAN) o de área extensa (WAN) es la alta velocidad de transmisión (1 Gbps, 10 Gbps) y la baja latencia.

La administración eficiente de una LAN municipal — que puede incluir cientos de puestos de trabajo, decenas de switches, servidores, puntos de acceso inalámbricos e impresoras de red — requiere herramientas de gestión, monitorización y control de acceso que garanticen la disponibilidad, el rendimiento y la seguridad de la infraestructura.

## 2. Fundamentos de las Redes LAN

### 2.1. Topologías

*   **Bus:** Todos los dispositivos comparten un único cable (coaxial). Obsoleta. Una rotura del cable afecta a toda la red.
*   **Anillo (Token Ring):** Los dispositivos se conectan en anillo cerrado, pasándose un testigo (token). Obsoleta.
*   **Estrella:** Todos los dispositivos se conectan a un nodo central (switch). **Topología dominante actual.** Si un cable falla, solo se desconecta ese dispositivo; el resto de la red funciona.
*   **Estrella extendida (jerárquica):** Múltiples switches de acceso conectados a switches de distribución, que a su vez se conectan a switches de núcleo (core). Arquitectura estándar en organizaciones medianas y grandes.

### 2.2. Ethernet (IEEE 802.3)

**Ethernet** es el estándar dominante para redes LAN cableadas. Evolución:

| Estándar | Velocidad | Medio |
|----------|-----------|-------|
| 10BASE-T | 10 Mbps | Par trenzado Cat3 |
| 100BASE-TX (Fast Ethernet) | 100 Mbps | Par trenzado Cat5 |
| 1000BASE-T (Gigabit Ethernet) | 1 Gbps | Par trenzado Cat5e/Cat6 |
| 10GBASE-T | 10 Gbps | Par trenzado Cat6a/Cat7 |
| 25GBASE / 40GBASE / 100GBASE | 25-100 Gbps | Fibra óptica |

### 2.3. Cableado estructurado

El cableado de la LAN se diseña conforme a la norma **TIA/EIA-568** y se estructura en:
*   **Área de trabajo:** Tomas de red en las mesas de los funcionarios.
*   **Cableado horizontal:** Cables desde las tomas de red hasta el armario de telecomunicaciones de la planta.
*   **Armario de telecomunicaciones:** Rack con los switches de acceso de la planta.
*   **Cableado vertical (backbone):** Fibra óptica entre los armarios de telecomunicaciones y el CPD central.
*   **Sala de equipos / CPD:** Switches de núcleo, routers, servidores.

### 2.4. Wi-Fi (IEEE 802.11)

Las redes inalámbricas complementan la LAN cableada:

| Estándar | Nombre comercial | Frecuencia | Velocidad máx. |
|----------|-----------------|------------|----------------|
| 802.11n | Wi-Fi 4 | 2,4 / 5 GHz | 600 Mbps |
| 802.11ac | Wi-Fi 5 | 5 GHz | 6,9 Gbps |
| 802.11ax | Wi-Fi 6 / 6E | 2,4 / 5 / 6 GHz | 9,6 Gbps |

Seguridad inalámbrica: **WPA3-Enterprise** con autenticación 802.1X y RADIUS.

## 3. Gestión y Monitorización de Dispositivos

### 3.1. SNMP (Simple Network Management Protocol)

**SNMP** es el protocolo estándar para la monitorización y gestión de dispositivos de red. Opera sobre UDP (puertos 161/162) y se basa en tres componentes:

*   **Agente SNMP:** Software residente en cada dispositivo gestionable (switch, router, servidor). Recolecta y expone variables de rendimiento (uso de CPU, tráfico por interfaz, errores, temperatura).
*   **MIB (Management Information Base):** Estructura jerárquica en forma de árbol que define las variables que pueden consultarse en cada dispositivo. Cada variable tiene un OID (Object Identifier) único.
*   **NMS (Network Management System / Gestor):** Servidor central que consulta periódicamente (polling) a los agentes SNMP y recibe alertas (traps) cuando se producen eventos anómalos.

#### Versiones de SNMP

| Versión | Seguridad |
|---------|-----------|
| SNMPv1 | Sin cifrado, comunidad en texto claro |
| SNMPv2c | Mejoras de rendimiento, comunidad en texto claro |
| **SNMPv3** | **Autenticación y cifrado (recomendada por el ENS)** |

### 3.2. Herramientas de monitorización

| Herramienta | Tipo | Características |
|-------------|------|----------------|
| **Nagios** | Open source | Monitorización de servicios y hosts, alertas |
| **Zabbix** | Open source | Monitorización, graficación, autodescubrimiento |
| **Grafana** | Open source | Visualización de métricas, dashboards |
| **PRTG** | Comercial | Monitorización all-in-one, sondas remotas |
| **SolarWinds** | Comercial | Suite completa de gestión de red |
| **Cacti** | Open source | Graficación de tráfico (RRDtool) |

### 3.3. Syslog

**Syslog** es el protocolo estándar para la recolección centralizada de logs de dispositivos de red (switches, routers, firewalls, servidores). Los logs se envían a un servidor Syslog central (rsyslog, Graylog, Elastic Stack) para su almacenamiento, análisis y correlación (SIEM).

### 3.4. NetFlow / sFlow

Protocolos para el análisis del tráfico de red: qué IPs se comunican, por qué puertos, con qué volumen y durante cuánto tiempo. Permiten detectar anomalías, ataques DDoS y usos indebidos de la red.

## 4. Administración de Redes LAN

### 4.1. Segmentación de red

La red se segmenta en VLANs (Tema 35) para aislar dominios de seguridad y optimizar el rendimiento. La segmentación se complementa con:
*   **Subredes IP:** Asignación de rangos IP diferentes a cada VLAN.
*   **Listas de Control de Acceso (ACL):** Reglas en routers y switches de capa 3 que filtran tráfico inter-VLAN.
*   **Firewalls:** Filtrado avanzado con inspección de estado (stateful inspection) y detección de intrusiones.

### 4.2. Control de acceso a la red: IEEE 802.1X

**IEEE 802.1X** es el estándar de **control de acceso a la red basado en puerto (Port-Based Network Access Control)**. Impide que un dispositivo acceda a la red hasta que se autentique.

**Componentes:**
*   **Suplicante:** Software en el dispositivo que solicita acceso (el PC del funcionario).
*   **Autenticador:** El switch de red que controla el acceso al puerto.
*   **Servidor de autenticación:** Servidor RADIUS (Remote Authentication Dial-In User Service) que valida las credenciales o el certificado del suplicante.

**Proceso:**
1.  El dispositivo se conecta al puerto del switch.
2.  El switch bloquea todo el tráfico excepto el protocolo EAP (Extensible Authentication Protocol).
3.  El suplicante envía sus credenciales (usuario/contraseña, certificado X.509) al switch.
4.  El switch las reenvía al servidor RADIUS.
5.  Si la autenticación es exitosa, RADIUS indica al switch que abra el puerto y asigne la VLAN correspondiente.
6.  Si falla, el puerto permanece bloqueado o se asigna a una VLAN de cuarentena.

### 4.3. DHCP (Dynamic Host Configuration Protocol)

**DHCP** asigna automáticamente direcciones IP, máscaras de subred, puerta de enlace y servidores DNS a los dispositivos de la red. Evita la configuración manual de cada equipo.

### 4.4. DNS (Domain Name System)

**DNS** resuelve nombres de dominio (ej. `sede.alicante.es`) a direcciones IP. En una LAN municipal se configura un servidor DNS interno (integrado en Active Directory) para resolver los nombres de los servidores, impresoras y servicios internos.

### 4.5. Alta disponibilidad en la LAN

*   **Spanning Tree Protocol (STP / RSTP):** Previene bucles en redes con enlaces redundantes entre switches.
*   **Link Aggregation (LACP / IEEE 802.3ad):** Agrupa múltiples enlaces físicos entre switches en un único enlace lógico de mayor ancho de banda.
*   **Switches redundantes (stacking/clustering):** Pilas de switches que operan como una unidad lógica con failover automático.

## 5. Conclusión

Las redes de área local (LAN) constituyen la columna vertebral de la infraestructura de comunicaciones de los Ayuntamientos. Su administración eficiente requiere una arquitectura basada en Ethernet, topología en estrella jerárquica, segmentación mediante VLANs, monitorización mediante SNMP (v3) y herramientas NMS, control de acceso con IEEE 802.1X y RADIUS, y mecanismos de alta disponibilidad (STP, LACP). Todo ello conforme a los requisitos del Esquema Nacional de Seguridad y las guías CCN-STIC aplicables a la protección de las comunicaciones.
