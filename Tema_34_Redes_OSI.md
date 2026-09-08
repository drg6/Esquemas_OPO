# Tema 34.- Redes. Modelo OSI de ISO: Arquitectura, capas, interfaces, protocolos, direccionamiento y encaminamiento.

## 1. Introducción
* **Contexto:** En los años 80, cada fabricante usaba protocolos propietarios e incompatibles (SNA, AppleTalk).
* **Solución:** La ISO crea el **Modelo OSI** en 1984. Un marco teórico universal para estandarizar la comunicación.
* *Nota:* TCP/IP es el estándar real (de facto), pero OSI es el mapa conceptual para el diseño y diagnóstico (ej. "Tengo un problema de Capa 3").

## 2. Arquitectura del Modelo (Principios)
* **Capas (Layers):** 7 niveles funcionales.
* **Encapsulamiento:** Descenso añadiendo cabeceras (creando **PDUs**).
* **Desencapsulamiento:** Ascenso retirando cabeceras.
* **Independencia:** Cambiar el cable de red (Capa 1) no afecta al software de correo (Capa 7).

## 3. Las Siete Capas del Modelo OSI
1. **Física (Capa 1):**
   * Transmisión de *bits* (0s y 1s). PDU: Bit.
   * Medio físico (cobre, fibra, aire). Dispositivos: Hubs.
2. **Enlace de Datos (Capa 2):**
   * Comunicación local (LAN). Direcciones **MAC** (48 bits). PDU: Trama (*Frame*). Dispositivos: Switches.
   * **Detalle AAPP:** Uso intensivo del estándar **IEEE 802.1Q (VLANs)** para segmentar la red (ej. Policía Local aislada del resto) como exige el **ENS**.
3. **Red (Capa 3):**
   * Encaminamiento (*Routing*) entre distintas redes. Direcciones **IP** lógicas. PDU: Paquete. Dispositivos: Routers.
4. **Transporte (Capa 4):**
   * Comunicación extremo a extremo (Puertos 0-65535). PDU: Segmento (TCP) o Datagrama (UDP).
   * **TCP:** Orientado a conexión, fiable (HTTP, SSH).
   * **UDP:** Sin conexión, rápido, no fiable (DNS, streaming, VoIP).
5. **Sesión (Capa 5):** * Mantiene el diálogo (sincronización) entre aplicaciones.
6. **Presentación (Capa 6):**
   * Traducción, compresión y **cifrado** de los datos (SSL/TLS para HTTPS).
7. **Aplicación (Capa 7):**
   * Interfaz con el usuario. Protocolos: HTTP (80), HTTPS (443), SMTP (25), DNS (53), LDAP (389).

## 4. Direccionamiento (Capa 3)
* **IPv4:** 32 bits (4 octetos). Parte de Red + Parte de Host (separado por la máscara / CIDR).
  * *Direcciones Privadas (RFC 1918):* `10.x.x.x`, `172.16.x.x`, `192.168.x.x`. Usadas en intranets municipales y la **Red SARA** (requieren NAT para salir a Internet).
* **IPv6:** 128 bits (8 bloques hex). Soluciona el agotamiento de IPv4. Autoconfiguración y elimina la necesidad de NAT.

## 5. Encaminamiento (Routing)
* **Concepto:** Proceso del router para enviar paquetes por la ruta óptima mediante su **Tabla de Enrutamiento**.
* **Protocolos IGP (Interiores):** **OSPF** (Estado de enlace/Algoritmo Dijkstra).
* **Protocolos EGP (Exteriores):** **BGP** (Para interconectar Internet).
* **Tendencia actual (El toque pro):** Adopción de **SD-WAN** en AAPP descentralizadas para gestionar el *routing* entre múltiples sedes de forma centralizada por software, optimizando costes y mejorando la seguridad.

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