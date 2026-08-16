# Tema 34.- Redes. El modelo de referencia de interconexión de sistemas abiertos (OSI) de ISO: Arquitectura, capas, interfaces, protocolos, direccionamiento y encaminamiento.

## 1. Introducción

A principios de la década de 1980, la comunicación entre equipos informáticos de distintos fabricantes era prácticamente imposible: cada fabricante (IBM con SNA, Digital con DECnet, Apple con AppleTalk) utilizaba protocolos propietarios incompatibles entre sí. Un mainframe IBM no podía comunicarse con un equipo Digital sin costosos equipos de traducción.

Para resolver esta fragmentación, la **ISO (Organización Internacional de Normalización)** publicó en 1984 el **Modelo de Referencia OSI (Open Systems Interconnection)**, una arquitectura conceptual que establece un marco estándar de comunicación en redes de computadoras. Aunque en la práctica el modelo TCP/IP se impuso como estándar de facto, el modelo OSI sigue siendo el marco teórico de referencia universal para comprender, diseñar y diagnosticar problemas en redes de comunicaciones.

## 2. Arquitectura del Modelo OSI

### 2.1. Principios de diseño

*   **Estructura en capas (layers):** La comunicación se divide en 7 capas funcionales, cada una con una responsabilidad específica.
*   **Encapsulamiento:** Cada capa añade su propia cabecera (header) a los datos recibidos de la capa superior, formando una **PDU (Protocol Data Unit)** específica de cada nivel.
*   **Interfaces definidas:** Cada capa ofrece servicios a la capa superior a través de interfaces estandarizadas, ocultando los detalles de implementación.
*   **Independencia:** El cambio de la tecnología en una capa no afecta a las demás (se puede cambiar el medio físico de cobre a fibra óptica sin alterar las capas superiores).

### 2.2. Proceso de comunicación

Cuando un usuario envía un correo electrónico:
1.  Los datos descienden desde la Capa 7 (Aplicación) hasta la Capa 1 (Física), y cada capa añade su cabecera (**encapsulamiento**).
2.  Los datos se transmiten por el medio físico (cable, fibra, inalámbrico).
3.  En el receptor, los datos ascienden desde la Capa 1 hasta la Capa 7, y cada capa retira su cabecera (**desencapsulamiento**).

## 3. Las Siete Capas del Modelo OSI

### Capa 1 — Física (Physical Layer)

*   **Función:** Transmisión de bits brutos (0 y 1) a través del medio físico.
*   **Define:** Voltajes, frecuencias, tipo de conector, tipo de cable, tasa de bits.
*   **PDU:** Bit.
*   **Medios:** Par trenzado (Cat5e, Cat6, Cat6a), fibra óptica (monomodo, multimodo), aire (inalámbrico).
*   **Dispositivos:** Hubs, repetidores, concentradores, transceptores.
*   **Estándares:** IEEE 802.3 (Ethernet físico), TIA/EIA-568 (cableado estructurado).

### Capa 2 — Enlace de Datos (Data Link Layer)

*   **Función:** Comunicación fiable entre nodos adyacentes en la misma red local. Detección y corrección de errores a nivel de trama.
*   **Subcapas:**
    *   **LLC (Logical Link Control):** Control de flujo y multiplexación de protocolos.
    *   **MAC (Media Access Control):** Control de acceso al medio (CSMA/CD en Ethernet).
*   **PDU:** Trama (Frame).
*   **Direccionamiento:** Direcciones MAC (48 bits, formato `AA:BB:CC:DD:EE:FF`), únicas para cada interfaz de red.
*   **Dispositivos:** Switches (conmutadores), bridges (puentes).
*   **Estándares:** IEEE 802.3 (Ethernet), IEEE 802.11 (Wi-Fi), IEEE 802.1Q (VLANs).

### Capa 3 — Red (Network Layer)

*   **Función:** Encaminamiento (routing) de paquetes entre redes diferentes. Determinación de la ruta óptima.
*   **PDU:** Paquete (Packet).
*   **Direccionamiento:** Direcciones IP (lógicas, jerárquicas).
    *   **IPv4:** 32 bits (4 octetos): `192.168.1.100`.
    *   **IPv6:** 128 bits (8 grupos hexadecimales): `2001:0db8:85a3::8a2e:0370:7334`.
*   **Dispositivos:** Routers (encaminadores), switches de capa 3.
*   **Protocolos:** IP, ICMP (diagnóstico: ping, traceroute), ARP (resolución IP→MAC).
*   **Protocolos de encaminamiento:** OSPF, BGP, RIP, EIGRP.

### Capa 4 — Transporte (Transport Layer)

*   **Función:** Comunicación extremo a extremo (end-to-end) entre procesos en máquinas diferentes. Segmentación, control de flujo y control de errores.
*   **PDU:** Segmento (TCP) o Datagrama (UDP).
*   **Direccionamiento:** Puertos (0-65535).
*   **Protocolos principales:**

| Protocolo | Características | Uso típico |
|-----------|----------------|-----------|
| **TCP** | Orientado a conexión, fiable, control de flujo | HTTP/S, SMTP, FTP, SSH |
| **UDP** | Sin conexión, no fiable, rápido | DNS, VoIP, streaming, SNMP |

*   **Puertos conocidos:** 80 (HTTP), 443 (HTTPS), 22 (SSH), 25 (SMTP), 53 (DNS), 3389 (RDP), 1521 (Oracle).

### Capa 5 — Sesión (Session Layer)

*   **Función:** Establecimiento, gestión y terminación de sesiones (diálogos) entre aplicaciones.
*   **Mecanismos:** Puntos de sincronización (checkpoints) para recuperación ante fallos, control de turnos de diálogo (half-duplex, full-duplex).
*   **Protocolos:** NetBIOS, RPC, PPTP (fase de control).

### Capa 6 — Presentación (Presentation Layer)

*   **Función:** Traducción, cifrado y compresión de datos para garantizar que la información sea comprensible por la capa de aplicación, independientemente de la representación interna de cada sistema.
*   **Funciones específicas:**
    *   Traducción de formatos (ASCII ↔ EBCDIC, UTF-8).
    *   Cifrado y descifrado (SSL/TLS — funciones criptográficas).
    *   Compresión de datos.
*   **Formatos:** JPEG, MPEG, GIF, ASCII, Unicode.

### Capa 7 — Aplicación (Application Layer)

*   **Función:** Interfaz directa con el usuario y las aplicaciones. Proporciona los protocolos de alto nivel que los programas utilizan para comunicarse.
*   **Protocolos principales:**

| Protocolo | Función | Puerto |
|-----------|---------|--------|
| **HTTP/HTTPS** | Navegación web | 80/443 |
| **SMTP** | Envío de correo electrónico | 25/587 |
| **POP3/IMAP** | Recepción de correo electrónico | 110/143 |
| **FTP/SFTP** | Transferencia de ficheros | 21/22 |
| **DNS** | Resolución de nombres de dominio | 53 |
| **DHCP** | Asignación dinámica de direcciones IP | 67/68 |
| **SNMP** | Gestión de dispositivos de red | 161/162 |
| **LDAP** | Acceso a directorios (Active Directory) | 389/636 |
| **SSH** | Acceso remoto seguro | 22 |

## 4. Direccionamiento

### 4.1. Direccionamiento IPv4

*   **Formato:** 32 bits divididos en 4 octetos: `192.168.10.25`.
*   **Estructura:** Parte de red + Parte de host, delimitada por la **máscara de subred** (`255.255.255.0` o `/24` en notación CIDR).
*   **Clases (modelo clásico):** A (grandes redes), B (medianas), C (pequeñas), D (multicast), E (experimental).
*   **Direcciones privadas (RFC 1918):** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` — utilizadas en redes internas, no enrutables en Internet.
*   **NAT (Network Address Translation):** Traduce direcciones privadas a direcciones públicas para el acceso a Internet.

### 4.2. Direccionamiento IPv6

*   **Formato:** 128 bits en 8 grupos hexadecimales: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`.
*   **Motivación:** Agotamiento del espacio de direcciones IPv4 (~4.300 millones de direcciones).
*   **Ventajas:** Espacio de direcciones virtualmente ilimitado (3,4 × 10³⁸ direcciones), autoconfiguración (SLAAC), eliminación de NAT, cabecera simplificada.

## 5. Encaminamiento (Routing)

### 5.1. Concepto

El **encaminamiento** es el proceso por el cual un router determina la ruta óptima para reenviar un paquete desde la red de origen hasta la red de destino.

### 5.2. Tabla de enrutamiento

El router mantiene una **tabla de enrutamiento** que asocia redes de destino con interfaces de salida y "siguientes saltos" (next hops). La tabla se puede construir:

*   **Estáticamente:** Rutas configuradas manualmente por el administrador. Adecuado para redes pequeñas.
*   **Dinámicamente:** Rutas aprendidas automáticamente mediante protocolos de enrutamiento.

### 5.3. Protocolos de enrutamiento

| Protocolo | Tipo | Algoritmo | Ámbito |
|-----------|------|-----------|--------|
| **OSPF** | Link-State (estado de enlace) | Dijkstra (SPF) | Interior (IGP) — redes medianas/grandes |
| **BGP** | Path-Vector | Atributos de ruta | Exterior (EGP) — Internet (interconexión de AS) |
| **RIP** | Distance-Vector | Bellman-Ford | Interior — redes pequeñas (obsoleto) |
| **EIGRP** | Híbrido | DUAL | Interior — redes Cisco |

## 6. Modelo OSI vs. Modelo TCP/IP

| Modelo OSI (7 capas) | Modelo TCP/IP (4 capas) |
|----------------------|------------------------|
| 7. Aplicación | Aplicación |
| 6. Presentación | Aplicación |
| 5. Sesión | Aplicación |
| 4. Transporte | Transporte |
| 3. Red | Internet |
| 2. Enlace de Datos | Acceso a red |
| 1. Física | Acceso a red |

El modelo TCP/IP es el que se implementa en la práctica (Internet). El modelo OSI es el marco de referencia teórico que permite comprender los conceptos y diagnosticar problemas capa por capa.

## 7. Conclusión

El modelo OSI de ISO proporciona el marco teórico fundamental para la comprensión de las comunicaciones en red. Su arquitectura de siete capas establece una separación clara de responsabilidades: desde la transmisión física de bits (Capa 1) hasta los protocolos de aplicación que utiliza el usuario (Capa 7), pasando por el direccionamiento lógico IP y el encaminamiento (Capa 3) que permiten interconectar redes heterogéneas.

El direccionamiento (IPv4/IPv6) y los protocolos de encaminamiento (OSPF, BGP) constituyen los cimientos sobre los que se construye la conectividad de las redes municipales, su interconexión con la Red SARA y su acceso a Internet, todo ello conforme a los requisitos de seguridad del ENS.
