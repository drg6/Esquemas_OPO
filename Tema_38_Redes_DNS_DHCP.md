# Tema 38.- Redes. Servicios DNS, DHCP.

## 1. Introducción

En una red IP, cada dispositivo necesita dos cosas para comunicarse: una **dirección IP** que lo identifique y un mecanismo para resolver **nombres de dominio** (como `sede.alicante.es`) a direcciones IP numéricas. Gestionar manualmente las direcciones IP de 2.000 puestos de trabajo es inviable, y obligar a los usuarios a memorizar direcciones como `192.168.10.45` es impracticable.

Los servicios **DHCP (Dynamic Host Configuration Protocol)** y **DNS (Domain Name System)** resuelven estos problemas fundamentales: DHCP asigna automáticamente la configuración de red a cada dispositivo, y DNS traduce los nombres de dominio legibles por humanos a las direcciones IP que los equipos necesitan para comunicarse.

## 2. Servicio DNS (Domain Name System)

### 2.1. Concepto y función

El **DNS** es un sistema de nomenclatura jerárquico y distribuido que actúa como la "guía telefónica" de Internet y las redes internas. Su función principal es la **resolución de nombres**: traducir un nombre de dominio (`www.alicante.es`) a una dirección IP (`83.45.67.12`) y viceversa.

### 2.2. Arquitectura jerárquica

El DNS se organiza en una estructura de árbol invertido:

```
                    . (raíz)
                   / \
                .es   .com   .org   .eu   ...
               /        \
         alicante.es    google.com
            /    \
       sede.     www.
```

*   **Raíz (root):** Los 13 servidores raíz (A-M) gestionados por organizaciones internacionales (ICANN, RIPE, NASA).
*   **TLD (Top-Level Domain):** Dominios de nivel superior: genéricos (.com, .org, .net) y geográficos (.es, .fr, .de).
*   **Dominio de segundo nivel:** `alicante.es`, `hacienda.gob.es`.
*   **Subdominio:** `sede.alicante.es`, `correo.alicante.es`.

### 2.3. Tipos de registros DNS

| Tipo | Función | Ejemplo |
|------|---------|---------|
| **A** | Nombre → IPv4 | `sede.alicante.es → 83.45.67.12` |
| **AAAA** | Nombre → IPv6 | `sede.alicante.es → 2001:db8::1` |
| **CNAME** | Alias (nombre → nombre) | `www.alicante.es → sede.alicante.es` |
| **MX** | Servidor de correo | `alicante.es → correo.alicante.es (prioridad 10)` |
| **NS** | Servidor DNS autoritativo | `alicante.es → ns1.alicante.es` |
| **PTR** | IPv4 → Nombre (resolución inversa) | `83.45.67.12 → sede.alicante.es` |
| **SOA** | Inicio de autoridad (zona) | Parámetros de la zona (serial, refresh, TTL) |
| **TXT** | Texto libre | SPF, DKIM, DMARC (autenticación de correo) |
| **SRV** | Servicio + puerto | `_ldap._tcp.alicante.es → dc01.alicante.es:389` |

### 2.4. Proceso de resolución DNS

Cuando un funcionario escribe `sede.alicante.es` en su navegador:

1.  El PC consulta su **caché DNS local**.
2.  Si no lo tiene, consulta al **servidor DNS recursivo** configurado (por DHCP): normalmente el servidor DNS interno del Ayuntamiento.
3.  Si el DNS recursivo no tiene la respuesta en su caché, inicia una **consulta recursiva/iterativa**:
    *   Pregunta a un **servidor raíz** → le redirige al servidor de `.es`.
    *   Pregunta al servidor de `.es` → le redirige al servidor de `alicante.es`.
    *   Pregunta al servidor **autoritativo** de `alicante.es` → responde con la IP `83.45.67.12`.
4.  El DNS recursivo almacena la respuesta en caché (TTL) y la devuelve al PC.
5.  El navegador se conecta a `83.45.67.12`.

### 2.5. DNS en entornos corporativos

*   **DNS interno (Split DNS):** El servidor DNS interno del Ayuntamiento resuelve los nombres de la red interna (servidores, impresoras, aplicaciones) y reenvía (forwarding) las consultas externas a los DNS de Internet.
*   **Active Directory Integrated DNS:** En entornos Windows Server, el DNS se integra con Active Directory. Los registros DNS se almacenan en la base de datos de AD y se replican entre los controladores de dominio.
*   **Zonas de búsqueda directa e inversa:**
    *   **Directa:** Nombre → IP (`servidor01.ayto.local → 10.0.1.10`).
    *   **Inversa:** IP → Nombre (`10.0.1.10 → servidor01.ayto.local`).

### 2.6. Seguridad DNS

*   **DNS Spoofing / Cache Poisoning:** Ataque que manipula la caché DNS para redirigir tráfico a un servidor malicioso.
*   **DNSSEC (DNS Security Extensions):** Extensiones que firman criptográficamente los registros DNS para garantizar su autenticidad e integridad. Impide la manipulación de respuestas DNS.
*   **DNS over HTTPS (DoH) / DNS over TLS (DoT):** Cifran las consultas DNS para proteger la privacidad del usuario.

### 2.7. Servidores DNS

| Servidor | Tipo |
|----------|------|
| **BIND (Berkeley Internet Name Domain)** | Open source, el más extendido en Internet |
| **Microsoft DNS Server** | Integrado en Windows Server / Active Directory |
| **Unbound** | Open source, recursivo, orientado a seguridad |
| **PowerDNS** | Open source, alto rendimiento |

## 3. Servicio DHCP (Dynamic Host Configuration Protocol)

### 3.1. Concepto y función

**DHCP** es un protocolo de red que permite a un servidor asignar **automáticamente** la configuración de red a los dispositivos que se conectan a la LAN. Sin DHCP, cada PC, impresora y dispositivo debería configurarse manualmente con dirección IP, máscara, puerta de enlace y servidores DNS.

### 3.2. Parámetros asignados por DHCP

| Parámetro | Ejemplo |
|-----------|---------|
| Dirección IP | `10.0.10.125` |
| Máscara de subred | `255.255.255.0 (/24)` |
| Puerta de enlace (Gateway) | `10.0.10.1` |
| Servidor DNS primario | `10.0.1.10` |
| Servidor DNS secundario | `10.0.1.11` |
| Nombre de dominio | `ayto.local` |
| Servidor NTP | `10.0.1.5` |
| Tiempo de concesión (Lease Time) | `8 horas` |

### 3.3. Proceso DORA (Discover, Offer, Request, Acknowledge)

El proceso de asignación de dirección IP sigue cuatro pasos:

1.  **DHCP Discover:** El cliente envía un mensaje de broadcast (`255.255.255.255`) buscando un servidor DHCP en la red.
2.  **DHCP Offer:** El servidor DHCP responde con una oferta de dirección IP disponible y los parámetros de configuración.
3.  **DHCP Request:** El cliente acepta la oferta y solicita formalmente la dirección IP ofrecida.
4.  **DHCP Acknowledge:** El servidor confirma la asignación y el cliente configura su interfaz de red con los parámetros recibidos.

### 3.4. Conceptos clave

*   **Ámbito (Scope):** Rango de direcciones IP que el servidor DHCP puede asignar (ej. `10.0.10.100` a `10.0.10.254`).
*   **Exclusiones:** Direcciones dentro del ámbito que se reservan y no se asignan dinámicamente (ej. IPs de servidores, impresoras, switches).
*   **Reservas (MAC binding):** Asignación fija de una IP específica a una dirección MAC concreta. La impresora `00:1A:2B:3C:4D:5E` siempre recibe la IP `10.0.10.50`.
*   **Tiempo de concesión (Lease Time):** Duración de la asignación. El cliente debe renovar la concesión antes de que expire. Típico: 8 horas en oficinas, 1 hora en redes Wi-Fi de invitados.
*   **DHCP Relay Agent:** En redes segmentadas por VLANs, un agente relay (configurado en el router o switch de capa 3) reenvía las solicitudes DHCP broadcast del cliente al servidor DHCP ubicado en otra VLAN.

### 3.5. DHCP en entornos corporativos

*   **Servidor DHCP en Windows Server:** Integrado con Active Directory. Permite autorización del servidor DHCP en AD (solo servidores autorizados pueden asignar IPs).
*   **DHCP en Linux:** ISC DHCP Server, Kea DHCP.
*   **DHCP Failover:** Configuración de dos servidores DHCP (principal y secundario) para alta disponibilidad. Si el principal falla, el secundario continúa asignando direcciones.
*   **Integración DHCP-DNS:** Cuando el servidor DHCP asigna una IP, actualiza dinámicamente el registro DNS del dispositivo (DNS dinámico — DDNS).

### 3.6. Seguridad DHCP

*   **DHCP Snooping:** Función de seguridad en los switches que filtra los mensajes DHCP, permitiendo respuestas DHCP solo desde puertos autorizados (trusted ports). Previene ataques de DHCP spoofing donde un servidor DHCP fraudulento asigna configuraciones maliciosas.
*   **IP Source Guard:** Complementa el DHCP Snooping filtrando tráfico con IPs de origen no asignadas por el servidor DHCP legítimo.
*   **Dynamic ARP Inspection (DAI):** Valida los mensajes ARP contra la tabla de DHCP Snooping para prevenir ataques ARP spoofing.

## 4. Integración DNS y DHCP

En un entorno corporativo, DNS y DHCP trabajan conjuntamente:

1.  Un PC se conecta a la red y obtiene una IP del servidor DHCP (`10.0.10.125`).
2.  El servidor DHCP actualiza automáticamente el registro DNS del PC (DDNS):
    *   Zona directa: `pc-tributos01.ayto.local → 10.0.10.125`.
    *   Zona inversa: `10.0.10.125 → pc-tributos01.ayto.local`.
3.  Otros dispositivos de la red pueden localizar al PC por su nombre (`pc-tributos01.ayto.local`) sin conocer su IP.

## 5. Conclusión

Los servicios DNS y DHCP son componentes fundamentales de la infraestructura de red de cualquier Administración Pública. DHCP automatiza la asignación de direcciones IP y configuración de red, eliminando la gestión manual y reduciendo errores. DNS proporciona la resolución de nombres que permite a los usuarios y las aplicaciones comunicarse utilizando nombres legibles en lugar de direcciones IP numéricas.

La seguridad de ambos servicios (DNSSEC contra la manipulación de registros DNS, DHCP Snooping contra servidores DHCP fraudulentos) es un requisito del ENS, y su integración con Active Directory y la actualización dinámica DNS (DDNS) garantizan una gestión coherente y centralizada de la red municipal.
