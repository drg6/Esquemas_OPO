# Tema 34.- Redes. El modelo de referencia de interconexión de sistemas abiertos (OSI) de ISO: Arquitectura, capas, interfaces, protocolos, direccionamiento y encaminamiento.

## 1. Introducción
- **Contexto:** En los años 80, cada fabricante usaba protocolos propietarios e incompatibles (SNA de IBM, AppleTalk).
- **OSI (Open Systems Interconnection)** → modelo de referencia creado por **ISO en 1984** para estandarizar la comunicación entre sistemas de distintos fabricantes.
- OSI es un **modelo teórico**; en la práctica se utiliza **TCP/IP**, aunque OSI sigue siendo fundamental para **comprender y diagnosticar redes por capas**.

## 2. Arquitectura del Modelo e Interfaces
- **Capas (Layers):** 7 niveles funcionales.
- **Encapsulamiento:** Descenso de datos añadiendo cabeceras (creando **PDUs - Protocol Data Unit**). **Desencapsulamiento:** Ascenso retirando cabeceras.
- **Independencia:** Cambiar el cable de red (Capa 1) no afecta al software de correo (Capa 7).
- **Interfaces:** Límite entre capas adyacentes. **Servicios:** Función que una capa realiza para entregársela a la capa superior a través de la interfaz, ocultando su implementación técnica.

## 3. Las Siete Capas del Modelo OSI y sus Protocolos
1. **Física (Capa 1):**
   * Transmisión de *bits* (0s y 1s). PDU: Bit.
   * Medio físico (Par trenzado, fibra, aire). Dispositivos: Hubs y repetidores. 
   * *Estándar:* 802.3 (Ethernet)
2. **Enlace de Datos (Capa 2):**
   * Comunicación fiable local (LAN) y detección de errores. PDU: Trama (*Frame*). 
   * Direcciones **MAC** (48 bits). Subcapas: LLC (Logical Link Control) y MAC (Media Access Control). Dispositivos: Switches, bridges y puntos de acceso (AP) 
   * *Protocolos/Estándares:* 802.3 (Ethernet), 802.11 (Wi-Fi)
   * *Detalle AAPP:* Uso intensivo del estándar **802.1Q (VLANs)** para segmentar por departamentos (ej. Policía Local, Intervención, Wifi invitados) como exige el **ENS** y control de acceso por puerto físico (roseta) mediante **802.1X**.
3. **Red (Capa 3):**
   * Encaminamiento (*Routing*) entre distintas redes. PDU: Paquete.
   * Direcciones **IP** lógicas (IPv4 32bits, IPv6 128bits). Dispositivos: Routers y Switches L3. 
   * *Protocolos:* IP, ICMP, ARP (resolver de IP a MAC, protocolo puente L2/L3)
4. **Transporte (Capa 4):**
   * Comunicación extremo a extremo entre procesos/aplicaciones. (Puertos 0-65535). PDU: Segmento (TCP) o Datagrama (UDP). 
   * **TCP (Transmission Control Protocol):** Orientado a conexión, fiable (HTTP, SSH, SMTP).
   * **UDP (User Datagram Protocol)** No orientado a conexión, rápido, no fiable (DNS, streaming, VoIP).
5. **Sesión (Capa 5):** 
   * Establece, mantiene (sincronización) y cierra el diálogo entre aplicaciones. PDU: Datos.
   * *Protocolos:* NetBIOS, RPC (Remote Procedure Call), PPTP
6. **Presentación (Capa 6):**
   * Traducción, compresión y **cifrado** de los datos. PDU: Datos.
   * *Protocolos:* SSL/TLS para HTTPS.
7. **Aplicación (Capa 7):**
   * Interfaz directa con las aplicaciones del usuario. PDU: Datos.
   * *Protocolos:* HTTP/HTTPS (web), SMTP/IMAP/POP3 (correo), DNS, LDAP.

## 4. Direccionamiento (Capa 3)
* **IPv4:** 32 bits (4 octetos). Parte de red + Parte de host, delimitada por la **máscara de subred** / CIDR (Abreviar: /16 es 255.255.0.0).
  * *Direcciones Privadas (RFC 1918):* `10.x.x.x`, `172.16.x.x`, `192.168.x.x`. — utilizadas en redes internas, en intranets municipales y la Red SARA. No enrutables en Internet.
  * *NAT (Network Address Translation):* Traduce direcciones privadas a direcciones públicas para el acceso a Internet.
* **IPv6:** 128 bits (8 bloques hex). Soluciona el agotamiento de IPv4. Autoconfiguración y elimina la necesidad de NAT.

## 5. Encaminamiento (Routing)
* **Concepto:** Proceso del router (Capa 3) para enviar paquetes por la mejor ruta usando su **Tabla de Enrutamiento**:
  * **Estático:** Rutas escritas a mano por el administrador (redes pequeñas, no se adapta solo a fallos).
  * **Dinámico:** Rutas aprendidas automáticamente entre routers (redes grandes, busca otro camino si falla un enlace).
* **Protocolos IGP (*Interior Gateway Protocol* - dentro de una misma red u organización):**
  * **OSPF (*Open Shortest Path First*):** El más usado. Elige la ruta más rápida según la velocidad de los enlaces.
  * **RIP (*Routing Information Protocol*):** Elige la ruta más corta contando saltos de router (máximo 15).
* **Protocolos EGP (*Exterior Gateway Protocol* - hacia fuera de la organización):**
  * **BGP (*Border Gateway Protocol*):** El protocolo que conecta **Internet** (une distintas redes y operadores entre sí).

## 6. OSI vs. TCP/IP
* El Modelo TCP/IP comprime las 7 capas en 4 prácticas: 
  * Aplicación (OSI 5,6,7).
  * Transporte (OSI 4).
  * Internet (OSI 3).
  * Acceso a Red (OSI 1,2).

## 7. Conclusión
El Modelo OSI sigue siendo la hoja de ruta imprescindible para el arquitecto de redes. En el ámbito municipal, comprender desde la segmentación 802.1Q en Capa 2 (para cumplir el ENS), hasta el enrutamiento IP en Capa 3 (para acceder a la Red SARA) y el cifrado TLS en Capa 6 (para proteger datos ciudadanos), es fundamental para desplegar una infraestructura resiliente y segura.

----------------------------------

# Tema 34.- Redes. El modelo de referencia de interconexión de sistemas abiertos (OSI) de ISO: Arquitectura, capas, interfaces, protocolos, direccionamiento y encaminamiento.

## 1. Introducción

- **OSI (Open Systems Interconnection)** → modelo de referencia creado por **ISO en 1984** para estandarizar la comunicación entre sistemas de distintos fabricantes.
- Es un **modelo teórico**; en la práctica se utiliza **TCP/IP**.
- OSI sigue siendo fundamental para **comprender y diagnosticar redes por capas**.

## 2. Arquitectura del Modelo OSI

### 2.1. Principios de diseño

- **7 capas**, cada una con funciones específicas.
- **Encapsulamiento** → cada capa añade información propia (header), formando una **PDU (Protocol Data Unit)** específica de cada nivel.
- **Interfaces** → cada capa ofrece servicios a la superior.
- **Independencia** → cambios en una capa no afectan a las demás.

### 2.2. Proceso de comunicación

Cuando un usuario envía un correo electrónico:
1.  Los datos descienden desde la Capa 7 (Aplicación) hasta la Capa 1 (Física), y cada capa añade su cabecera (**encapsulamiento**).
2.  Los datos se transmiten por el medio físico (cable, fibra, inalámbrico).
3.  En el receptor, los datos ascienden desde la Capa 1 hasta la Capa 7, y cada capa retira su cabecera (**desencapsulamiento**).

## 3. Las Siete Capas del Modelo OSI

### Capa 1 — Física (Physical Layer)

*   **Función:** Transmisión de bits (0 y 1) por el medio físico.
*   **Define:** Voltajes, frecuencias, tipo de conector, tipo de cable, tasa de bits (velocidad).
*   **PDU:** Bit.
*   **Medios:** Par trenzado (Cat5e, Cat6, Cat6a), fibra óptica (monomodo, multimodo), aire (inalámbrico).
*   **Dispositivos:** Hubs, repetidores, concentradores, transceptores.
*   **Estándares:** IEEE 802.3 (Ethernet físico), TIA/EIA-568 (cableado estructurado).

### Capa 2 — Enlace de Datos (Data Link Layer)

*   **Función:** Comunicación entre **nodos de la misma red**. Detección y corrección de errores a nivel de trama.
*   **Subcapas:**
    *   **LLC (Logical Link Control):** Control de flujo y multiplexación de protocolos.
    *   **MAC (Media Access Control):** Control de acceso al medio (CSMA/CD en Ethernet).
*   **PDU:** Trama (Frame).
*   **Direccionamiento:** Direcciones MAC (48 bits).
*   **Dispositivos:** Switches, bridges.
*   **Estándares:** IEEE 802.3 (Ethernet), IEEE 802.11 (Wi-Fi), IEEE 802.1Q (VLANs).
    Uso intensivo del estándar **IEEE 802.1Q (VLANs)** para segmentar la red (ej. Policía Local aislada del resto) como exige el **ENS**.

### Capa 3 — Red (Network Layer)

*   **Función:** Encaminamiento (routing) entre redes. 
*   **PDU:** Paquete (Packet).
*   **Direccionamiento lógicas:** IP 
    *   **IPv4:** 32 bits 
    *   **IPv6:** 128 bits 
*   **Dispositivos:** Routers.
*   **Protocolos:** IP, ICMP (diagnóstico: ping, traceroute), ARP (resolución IP→MAC).
*   **Protocolos de encaminamiento:** OSPF, BGP, RIP, EIGRP.

### Capa 4 — Transporte (Transport Layer)

*   **Función:** Comunicación extremo a extremo (end-to-end) entre procesos. Segmentación, control de flujo y control de errores.
*   **PDU:** Segmento (TCP) o Datagrama (UDP).
*   **Direccionamiento:** Puertos (0-65535).
*   **TCP** → fiable, orientado a conexión.
*   **UDP** → rápido, sin conexión.
*   **Puertos importantes:** 80 (HTTP), 443 (HTTPS), 22 (SSH), 25 (SMTP), 53 (DNS), 3389 (RDP), 1521 (Oracle).

### Capa 5 — Sesión (Session Layer)

*   **Función:** Establecimiento, gestión y terminación de sesiones entre aplicaciones.
*   **Mecanismos:** Sincronización y control del diálogo (half-duplex, full-duplex).
*   **Protocolos:** NetBIOS, RPC.

### Capa 6 — Presentación (Presentation Layer)

*   Garantiza la correcta **representación de los datos**.
*    Funciones:
  - **Traducción** de formatos/codificación.
  - **Cifrado/descifrado**.
  - **Compresión**.
*   **Formatos:** JPEG, MPEG, GIF, ASCII, Unicode.

### Capa 7 — Aplicación (Application Layer)

*   **Función:** Interfaz directa con el usuario y las aplicaciones. 
*   **Protocolos principales:**
  - **HTTP/HTTPS** → web (80/443)
  - **SMTP** → correo (25/587)
  - **POP3/IMAP** → recepción de correo (110/143)
  - **FTP/SFTP** → ficheros (21/22)
  - **DNS** → nombres (53)
  - **DHCP** → configuración IP (67/68)
  - **SNMP** → gestión de red (161/162)
  - **LDAP** → directorios (389/636)
  - **SSH** → acceso remoto (22)

## 4. Direccionamiento

### 4.1. Direccionamiento IPv4

*   **Formato:** 32 bits divididos en 4 octetos.
*   **Estructura:** Parte de red + Parte de host, delimitada por la **máscara de subred**
*   **Clases (modelo clásico):** A (grandes redes), B (medianas), C (pequeñas), D (multicast), E (experimental).
*   **Direcciones privadas (RFC 1918):** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` — utilizadas en redes internas, en intranets municipales y la **Red SARA**. No enrutables en Internet. 
*   **NAT (Network Address Translation):** Traduce direcciones privadas a direcciones públicas para el acceso a Internet.

### 4.2. Direccionamiento IPv6

*   **Formato:** 128 bits en 8 grupos hexadecimales.
*   **Motivación:** Agotamiento del espacio de direcciones IPv4.
*   **Ventajas:** 
  - Gran espacio de direcciones.
  - **SLAAC** (autoconfiguración).
  - Menor necesidad de NAT.
  - Cabecera simplificada.

## 5. Encaminamiento (Routing)

### 5.1. Concepto

Proceso mediante el que un **router determina la ruta** para llevar un paquete de origen a destino.

### 5.2. Tabla de enrutamiento

- Relaciona **red destino → interfaz de salida y siguiente salto**.
- **Estático** → configurado manualmente.
- **Dinámico** → aprendido mediante protocolos de routing.

### 5.3. Protocolos de enrutamiento

- **OSPF** → Link-State + Dijkstra → redes internas medianas/grandes.
- **BGP** → Path-Vector → Internet / entre AS.
- **RIP** → Distance-Vector → redes pequeñas, obsoleto.
- **EIGRP** → DUAL → principalmente Cisco.

* **Tendencia actual:** Adopción de **SD-WAN** en AAPP descentralizadas para gestionar el *routing* entre múltiples sedes de forma centralizada por software, optimizando costes y mejorando la seguridad.

## 6. Modelo OSI vs. Modelo TCP/IP

- **OSI → 7 capas**, modelo teórico.
- **TCP/IP → 4 capas**, modelo práctico utilizado en Internet.

Correspondencia:

- OSI 7 + 6 + 5 → **Aplicación**
- OSI 4 → **Transporte**
- OSI 3 → **Internet**
- OSI 2 + 1 → **Acceso a red**

> **🧠 OSI explica → TCP/IP implementa.**

## 7. Conclusión

- **OSI** proporciona el marco teórico para comprender las comunicaciones de red.
- Sus **7 capas** separan responsabilidades desde la **transmisión física de bits** hasta las aplicaciones.
- **IP** proporciona direccionamiento y **routing (capa 3)** permite interconectar redes.
- **TCP/IP** es el modelo utilizado en la práctica.
- En AAPP, estas redes deben cumplir además los requisitos de **seguridad del ENS**.