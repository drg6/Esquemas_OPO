# Tema 35.- Redes. Redes virtuales (VLAN) y Protocolos seguros.

## 1. Introducción

En una red de área local (LAN) tradicional, todos los dispositivos conectados al mismo switch forman un único **dominio de broadcast**: cualquier trama de difusión (ARP, DHCP Discover) se propaga a todos los puertos del switch. En un Ayuntamiento con cientos de equipos, esta situación degrada el rendimiento (tormentas de broadcast) y compromete la seguridad (el tráfico de Intervención es visible para cualquier equipo conectado al mismo switch).

Las **VLANs (Virtual Local Area Networks)** resuelven este problema segmentando lógicamente la red, mientras que los **protocolos seguros** (IPSec, TLS) garantizan la confidencialidad e integridad del tráfico.

## 2. VLANs (Virtual Local Area Networks)

### 2.1. Concepto

Una **VLAN** es una red lógica independiente creada dentro de un switch físico. Los puertos del switch se agrupan en VLANs, y cada VLAN constituye un **dominio de broadcast separado**: las tramas de difusión de una VLAN no se propagan a las demás.

### 2.2. Estándar IEEE 802.1Q

El estándar **IEEE 802.1Q** define el mecanismo de etiquetado (tagging) de VLANs. Cuando una trama Ethernet viaja entre switches, se inserta una etiqueta de 4 bytes en la cabecera de la trama que contiene, entre otros campos, el **VLAN ID** (VID), un identificador numérico de 12 bits que permite distinguir hasta 4.094 VLANs.

### 2.3. Tipos de puertos

*   **Puerto de acceso (Access port):** Pertenece a una única VLAN. Conecta dispositivos finales (PCs, impresoras). Las tramas entran y salen sin etiqueta 802.1Q.
*   **Puerto troncal (Trunk port):** Transporta tráfico de múltiples VLANs entre switches. Las tramas viajan etiquetadas con el VLAN ID correspondiente.

### 2.4. Ejemplo de segmentación en un Ayuntamiento

| VLAN ID | Nombre | Dispositivos |
|---------|--------|-------------|
| 10 | Gestión Tributaria | PCs de recaudación, impresoras de tributos |
| 20 | Urbanismo | PCs de licencias, plóteres |
| 30 | Policía Local | Terminales de comisaría, cámaras |
| 40 | Servicios Sociales | PCs de trabajadores sociales |
| 50 | Gestión TIC | Servidores, PCs de administradores de sistemas |
| 99 | WiFi invitados | Puntos de acceso de la red de invitados |

### 2.5. Ventajas de las VLANs

*   **Seguridad:** El tráfico de Tributos (VLAN 10) no es accesible desde Urbanismo (VLAN 20) a nivel de capa 2. Un malware en la VLAN de invitados no puede propagarse a la VLAN de servidores.
*   **Rendimiento:** Los dominios de broadcast se reducen; las tormentas de broadcast se limitan a la VLAN afectada.
*   **Flexibilidad:** Un funcionario puede trasladarse físicamente de planta y mantener su VLAN simplemente reconfigurando el puerto de acceso del switch.
*   **Aislamiento regulatorio:** Los datos de Servicios Sociales (categoría alta en el ENS) se aíslan en su propia VLAN con medidas de seguridad reforzadas.

### 2.6. Inter-VLAN Routing

Las VLANs son dominios de broadcast independientes: los dispositivos de VLANs diferentes no pueden comunicarse entre sí a nivel de capa 2. Para permitir la comunicación entre VLANs es necesario un dispositivo de **capa 3** (router o switch multicapa) que encamine el tráfico entre ellas, aplicando las listas de control de acceso (ACL) que definan qué tráfico inter-VLAN está permitido.

## 3. Protocolos Seguros

### 3.1. El problema de la seguridad en IP

El protocolo IP (Capa 3) fue diseñado sin mecanismos de seguridad: los paquetes viajan en claro, sin cifrado ni autenticación. Cualquier nodo intermedio puede leer, modificar o suplantar el tráfico. Los protocolos seguros añaden cifrado, autenticación e integridad a las comunicaciones.

### 3.2. IPSec (Internet Protocol Security)

**IPSec** es un conjunto de protocolos que opera en la **Capa 3 (Red)** del modelo OSI, proporcionando seguridad a nivel de paquete IP.

#### Servicios de seguridad

*   **Autenticación:** Verificación de la identidad del emisor.
*   **Integridad:** Garantía de que los datos no han sido alterados en tránsito.
*   **Confidencialidad:** Cifrado de los datos para impedir su lectura por terceros.
*   **Anti-replay:** Protección contra la retransmisión maliciosa de paquetes capturados.

#### Protocolos

| Protocolo | Función |
|-----------|---------|
| **AH (Authentication Header)** | Autenticación e integridad (sin cifrado) |
| **ESP (Encapsulating Security Payload)** | Autenticación, integridad y cifrado (AES-256, AES-GCM) |

#### Modos de operación

*   **Modo Transporte:** Solo se cifra/autentica el payload del paquete IP. La cabecera IP original se mantiene. Se usa en comunicación host-to-host.
*   **Modo Túnel:** Se cifra/autentica el paquete IP completo (cabecera + payload) y se encapsula en un nuevo paquete IP. Se usa en VPNs Site-to-Site.

#### IKE (Internet Key Exchange)

**IKE** (versiones IKEv1 e IKEv2) es el protocolo de negociación de claves que establece las **Asociaciones de Seguridad (SA)** entre los extremos del túnel IPSec. Negocia los algoritmos criptográficos, intercambia claves y autentica a las partes.

### 3.3. TLS (Transport Layer Security)

**TLS (Transport Layer Security)** opera en las **Capas 4-6** (Transporte-Presentación). Es el sucesor de SSL (Secure Sockets Layer). Proporciona:

*   **Cifrado:** Protege la confidencialidad del tráfico (AES-256-GCM, ChaCha20-Poly1305).
*   **Autenticación:** Mediante certificados X.509 del servidor (y opcionalmente del cliente).
*   **Integridad:** Códigos HMAC que detectan alteraciones.

#### Versiones

| Versión | Estado |
|---------|--------|
| SSL 2.0, SSL 3.0 | **Obsoletas y prohibidas** (vulnerabilidades graves: POODLE, DROWN) |
| TLS 1.0, TLS 1.1 | **Deprecadas** (deshabilitadas por navegadores modernos) |
| TLS 1.2 | **Vigente** (ampliamente soportada) |
| TLS 1.3 | **Recomendada** (handshake simplificado, mayor seguridad, menor latencia) |

#### Aplicación

TLS se utiliza para securizar protocolos de aplicación:
*   HTTPS = HTTP + TLS (puerto 443)
*   SMTPS = SMTP + TLS (puerto 465/587)
*   LDAPS = LDAP + TLS (puerto 636)
*   IMAPS = IMAP + TLS (puerto 993)

### 3.4. IPSec vs. TLS

| Aspecto | IPSec | TLS |
|---------|-------|-----|
| Capa OSI | Capa 3 (Red) | Capas 4-6 (Transporte-Presentación) |
| Ámbito | Todo el tráfico IP entre endpoints | Conexiones específicas de aplicación |
| Implementación | En el sistema operativo o router | En la aplicación o biblioteca |
| Uso típico | VPN Site-to-Site | HTTPS, correo seguro |
| Transparencia | Transparente para las aplicaciones | Requiere soporte en la aplicación |

### 3.5. Otros protocolos seguros relevantes

*   **SSH (Secure Shell):** Acceso remoto seguro a servidores (sustituto de Telnet). Puerto 22.
*   **DNSSEC:** Extensiones de seguridad para DNS que protegen contra ataques de envenenamiento de caché.
*   **WPA3 (Wi-Fi Protected Access 3):** Protocolo de seguridad para redes inalámbricas. Sustituto de WPA2 con cifrado más robusto (SAE/Dragonfly).

## 4. Conclusión

Las VLANs proporcionan la segmentación lógica de la red que el ENS exige para aislar dominios de seguridad dentro de la infraestructura municipal, mientras que los protocolos seguros (IPSec para VPNs y comunicaciones entre sedes, TLS para servicios web y correo electrónico) garantizan la confidencialidad, integridad y autenticidad del tráfico. La combinación de ambas tecnologías — segmentación de red y cifrado de comunicaciones — constituye la base de la defensa en profundidad de las redes de las Administraciones Públicas.
