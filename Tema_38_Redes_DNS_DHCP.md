# Tema 38.- Redes. Servicios DNS y DHCP.

## 1. Introducción
* **Contexto:** En una red municipal con miles de dispositivos, la gestión manual de direcciones IP es inviable y la memorización de IPs por parte de usuarios es impracticable.
* **Solución:** **DHCP** automatiza la configuración de red y **DNS** traduce los nombres lógicos (`sede.alicante.es`) a IPs numéricas.

## 2. Servicio DNS (Domain Name System)
* **2.1. Arquitectura Jerárquica:** Árbol invertido: Raíz (.) → Top-Level Domains (.es, .com) → Dominios (alicante.es) → Subdominios (sede).
* **2.2. Tipos de Registros Clave:**
  * **A / AAAA:** Nombre → IPv4 / IPv6.
  * **CNAME:** Alias (un nombre apunta a otro nombre).
  * **MX:** Servidor de correo.
  * **PTR:** Resolución inversa (IP → Nombre).
  * **TXT:** Texto plano (vital para seguridad de correo: SPF, DKIM, DMARC).
  * **SRV:** Localizador de servicios (ej. indispensable para que los PCs encuentren el Active Directory).
* **2.3. Proceso de Resolución:** Caché Local → DNS Recursivo interno → DNS Raíz → TLD → DNS Autoritativo.
* **2.4. DNS Corporativo (Detalle Táctico AAPP):**
  * **Split DNS (Horizontes divididos):** El DNS interno resuelve `sede.alicante.es` con la IP privada de la DMZ (optimizando tráfico interno), mientras el DNS externo da la IP pública a los ciudadanos.
  * Integración nativa con **Active Directory** (zonas replicadas entre controladores de dominio).
* **2.5. Seguridad DNS (ENS):**
  * **DNSSEC:** Firma criptográfica para evitar *Cache Poisoning*.
  * **DNS Sinkholing:** El DNS corporativo se usa como filtro de ciberseguridad (ej. herramientas del **CCN-CERT**) para bloquear resoluciones a dominios de malware/ransomware.

## 3. Servicio DHCP (Dynamic Host Configuration Protocol)
* **3.1. Parámetros asignados:** IP, Máscara, Puerta de Enlace, DNS primario/secundario y *Lease Time* (Tiempo de concesión).
* **3.2. El Proceso DORA (Transacción de asignación):**
  1. **D**iscover: Cliente busca servidor (Broadcast).
  2. **O**ffer: Servidor ofrece una IP.
  3. **R**equest: Cliente solicita formalmente esa IP.
  4. **A**ck: Servidor confirma la asignación.
* **3.3. Conceptos Clave de Administración:**
  * **Ámbito (Scope):** Rango de IPs asignables.
  * **Reservas (MAC Binding):** IP fija atada a una MAC concreta (ej. impresoras).
  * **DHCP Relay (IP Helper):** Agente en el router que permite a los PCs de una VLAN pedir IP a un servidor DHCP ubicado en otra VLAN diferente.
* **3.4. Seguridad DHCP (Detalle Táctico):**
  * **Rogue DHCP:** Riesgo crítico cuando un usuario conecta un router doméstico a la red y asigna IPs falsas. 
  * Se mitiga con **DHCP Snooping** (el switch solo permite ofertas DHCP desde puertos confiables/trusted) y **Dynamic ARP Inspection (DAI)**.

## 4. Integración DNS y DHCP (DDNS)
* En redes Microsoft, ambos servicios son simbióticos: cuando DHCP asigna una IP, realiza un registro dinámico (**DDNS**) actualizando instantáneamente los registros A y PTR en el DNS del Active Directory.

## 5. Conclusión
DNS y DHCP conforman la base operativa de cualquier infraestructura TIC municipal. Mientras que DHCP agiliza y centraliza el despliegue de redes (mitigando amenazas internas mediante DHCP Snooping), el DNS se erige no solo como el traductor de la web, sino como una pieza crítica de la arquitectura de seguridad perimetral (DNSSEC y Sinkholing) exigida por el Esquema Nacional de Seguridad.


-------------------------------

# Tema 38.- Redes. Servicios DNS, DHCP.

## 1. Introducción

Dispositivo -> IP + nombre de dominio
- **DHCP (Dynamic Host Configuration Protocol)** → asigna automáticamente la **configuración de red**. Gestionar direcciones IP manualmente es inviable.
- **DNS (Domain Name System)** → traduce **nombres de dominio a direcciones IP**. Memorizar ips es impracticable.

## 2. Servicio DNS (Domain Name System)

### 2.1. Concepto y función

- Sistema **jerárquico y distribuido**.
- Función principal → **resolución de nombres**:
  - Nombre → IP.
  - IP → nombre.

### 2.2. Arquitectura jerárquica

Estructura de árbol invertido:
- **Raíz (.)** → Servidores raiz gestionados por ICANN, RIPE, NASA.
- **TLD (Top-Level Domain)** → Nivel superior `.es`, `.com`, `.org`, etc.
- **Segundo nivel** → `alicante.es`.
- **Subdominio** → `sede.alicante.es`.

### 2.3. Tipos de registros DNS

- **A** → nombre → IPv4.
- **AAAA** → nombre → IPv6.
- **CNAME** → alias (www.alicante.es → sede.alicante.es)
- **MX** → servidor de correo.
- **NS** → servidor DNS autoritativo.
- **PTR** → IP → nombre (resolución inversa)
- **SOA** → autoridad/parámetros de zona (serial, refresh, TTL)
- **TXT** → texto (SPF, DKIM, DMARC) para autenticación de correo.
- **SRV** → servicio + puerto (_ldap._tcp.alicante.es → dc01.alicante.es:389)

### 2.4. Proceso de resolución DNS

1. **Caché local** del PC.
2. **DNS recursivo** configurado (DNS interno).
3. Si no tiene respuesta → consulta recursiva/iterativa **raíz → TLD → autoritativo**.
4. DNS recursivo guarda respuesta en **caché (TTL)** y la devuelve al PC.
5. El cliente conecta con la IP.

### 2.5. DNS en entornos corporativos

- **DNS interno** → resuelve nombres internos y reenvía consultas externas. 
- **Split DNS (Horizontes divididos):** El DNS interno resuelve `sede.alicante.es` con la IP privada de la DMZ (optimizando tráfico interno), mientras el DNS externo da la IP pública a los ciudadanos.
- **AD Integrated DNS** → DNS integrado con **Active Directory** y replicado entre DC.
- **Zona directa** → nombre → IP.
- **Zona inversa** → IP → nombre.

### 2.6. Seguridad DNS

- **DNS Spoofing / Cache Poisoning** → manipulación de respuestas/caché para redirigir tráfico a un servidor malicioso
- **DNSSEC** → firma criptográfica → **autenticidad + integridad**.
- ***DNS over HTTPS (DoH) / DNS over TLS (DoT)** → cifran las consultas DNS → **privacidad**.
- **DNS Sinkholing:** El DNS corporativo se usa como filtro de ciberseguridad (ej. herramientas del **CCN-CERT**) para bloquear resoluciones a dominios de malware/ransomware.

### 2.7. Servidores DNS

- **BIND (Berkeley Internet Name Domain)** → open source, muy extendido.
- **Microsoft DNS Server** → Windows Server / AD.
- **Unbound** → recursivo y orientado a seguridad.
- **PowerDNS** → alto rendimiento.

## 3. Servicio DHCP (Dynamic Host Configuration Protocol)

### 3.1. Concepto y función

- Asigna **automáticamente** la configuración de red.
- Evita configurar manualmente cada dispositivo.

### 3.2. Parámetros asignados por DHCP

    - **IP**
    - **Máscara**
    - **Gateway**
    - **DNS primario y secuandario**
    - **Dominio**
    - **Servidor NTP**
    - **Tiempo de concesión (Lease Time)**

### 3.3. Proceso DORA (Discover, Offer, Request, Acknowledge)

El proceso de asignación de dirección IP sigue cuatro pasos:

1.  **DHCP Discover:** El cliente envía mensaje de broadcast (`255.255.255.255`) buscando servidor DHCP.
2.  **DHCP Offer:** Servidor ofrece IP/configuración.
3.  **DHCP Request:** El cliente acepta la oferta y solicita formalmente la dirección IP ofrecida.
4.  **DHCP Acknowledge:** El servidor confirma y el cliente configura su interfaz de red con los parámetros recibidos.

### 3.4. Conceptos clave

- **Scope/Ámbito** → rango de IPs asignables (ej. `10.0.10.100` a `10.0.10.254`).
- **Exclusiones** → IPs del ámbito que no se asignan.
- **Reservas (MAC binding)** → IP fija asociada a una **MAC**.
- **Lease Time** → duración de la concesión antes de renovar
- **DHCP Relay Agent** → En redes segmentadas por VLANs, un agente relay (configurado en el router o switch de capa 3) reenvía las solicitudes DHCP broadcast del cliente al servidor DHCP ubicado en otra VLAN.

### 3.5. DHCP en entornos corporativos

- **Windows Server DHCP** → integración/autorización mediante AD.
- **Linux** → ISC DHCP / Kea.
- **DHCP Failover** → Configuración de dos servidores DHCP (principal y secundario) para alta disponibilidad.
- **DNS dinámico —DDNS** → DHCP actualiza automáticamente DNS (Integración DHCP-DNS).

### 3.6. Seguridad DHCP

- **DHCP Snooping** → solo puertos autorizados pueden proporcionar respuestas DHCP.
- **IP Source Guard** → bloquea IPs de origen no autorizadas.
- **DAI (Dynamic ARP Inspection)** → valida ARP mediante información de DHCP Snooping.

* **Rogue DHCP:** Riesgo crítico cuando un usuario conecta un router doméstico a la red y asigna IPs falsas. 
* Se mitiga con **DHCP Snooping** (el switch solo permite ofertas DHCP desde puertos confiables/trusted) y **Dynamic ARP Inspection (DAI)**.

## 4. Integración DNS y DHCP

1. PC obtiene **IP mediante DHCP**.
2. DHCP actualiza **DNS mediante DDNS**.
3. Otros equipos pueden localizarlo mediante **nombre**.

## 5. Conclusión

- **DHCP** → configura automáticamente los equipos.
- **DNS** → resuelve nombres.
- **DDNS** → integra ambos servicios.
- Requisito ENS. Seguridad en DNS Y DHCP:
  - **DNSSEC** → protege DNS. 
  - **DHCP Snooping** → protege DHCP.
  - **IP Source Guard + DAI** → refuerzan la seguridad de la LAN.
- Integración con **Active Directory** → gestión centralizada.