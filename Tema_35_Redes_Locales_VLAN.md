# Tema 35.- Redes. Redes virtuales (VLAN) y Protocolos seguros.

## 1. Introducción
* **Problema:** En una LAN tradicional todos los equipos comparten el mismo *Dominio de Broadcast*. Esto genera "tormentas" de red y graves riesgos de seguridad (todos ven todo).
* **Solución:** **VLAN** para segmentar lógicamente la red, e **IPSec/TLS** para cifrar las comunicaciones.

## 2. VLANs (Virtual Local Area Networks)
* **2.1. Concepto:** Redes lógicas dentro de un switch físico. Cada VLAN es un dominio de broadcast aislado a Nivel 2.
* **2.2. Estándar 802.1Q:** Etiqueta (*Tag*) de 4 bytes insertada en la trama Ethernet. Incluye el **VLAN ID** (12 bits = 4094 VLANs posibles).
* **2.3. Tipos de Puertos:** 
  * *Access (Acceso):* Conecta PCs. Pertenece a 1 sola VLAN. Tráfico sin etiqueta.
  * *Trunk (Troncal):* Conecta switches. Transporta múltiples VLANs etiquetadas.
* **2.4. Caso de Uso (Aislamiento ENS):** Separar VLAN 10 (Tributos) de VLAN 20 (Policía) y VLAN 99 (Invitados WiFi).
* **2.5. Inter-VLAN Routing:** Como las VLAN están aisladas, para que se hablen hace falta un Router (Capa 3) que aplique **Listas de Control de Acceso (ACL)**.

## 3. Protocolos Seguros (La Criptografía en Red)
* **3.1. El problema de IP:** El protocolo IP fue creado para ser rápido, no seguro. Viaja en texto plano.
* **3.2. IPSec (Internet Protocol Security - Capa 3):**
  * Protege todo el paquete IP. Transparente para las aplicaciones.
  * *Protocolos:* **AH** (Autenticación/Integridad) y **ESP** (Cifra el dato completo).
  * *Modos:* Transporte (Host a Host) y **Túnel** (Encapsula IP dentro de otro IP. Se usa para VPNs *Site-to-Site*, ej. conectar Ayuntamiento con Biblioteca).
  * *Negociación:* **IKE** (Intercambio de claves).
* **3.3. TLS (Transport Layer Security - Capas 4 a 6):**
  * Protege conexiones concretas entre aplicaciones usando certificados **X.509**.
  * *Versiones:* SSL/TLS 1.0 y 1.1 obsoletas. **TLS 1.2 vigente, TLS 1.3 recomendada**.
  * *Puertos Seguros:* HTTPS (443), LDAPS (636), IMAPS (993).
* **3.4. El Escenario VPN Actual en AAPP:**
  * **IPSec** se usa para enlazar edificios (Site-to-Site).
  * **TLS** se usa para los portales de **Teletrabajo** (VPN Client-to-Site), porque es más amigable para conectar desde el navegador de casa.
* **3.5. Otros Protocolos Seguros:**
  * **SSH (22):** Consola remota cifrada (mata a Telnet).
  * **WPA3:** El estándar actual para Wi-Fi municipal segura.

## 4. Conclusión
La arquitectura de red de una Administración Pública no puede basarse en la confianza. La segmentación mediante **VLANs** aisla los departamentos, mientras que la criptografía de **IPSec** y **TLS** protege los datos en tránsito. Esta estrategia de Defensa en Profundidad es obligatoria para certificar la red municipal conforme al **Esquema Nacional de Seguridad (ENS)**.

> 🧠 **Mnemotecnia Táctica:** 
> **VLAN = Separar el tráfico | IPSec = Proteger la red (VPN) | TLS = Proteger la aplicación (Web/Teletrabajo)**

---------------------------

# Tema 35.- Redes. Redes virtuales (VLAN) y Protocolos seguros.

## 1. Introducción

- LAN tradicional → un único **dominio de broadcast**.
- Problemas: menor rendimiento (**tormentas de broadcast**) y seguridad.
- **VLAN** → segmentación lógica de la red.
- **IPSec/TLS** → protocolos seguros en las comunicaciones.

## 2. VLANs (Virtual Local Area Networks)

### 2.1. Concepto

- **VLAN** = red lógica independiente dentro de un switch físico.
- Cada VLAN = **dominio de broadcast independiente**.
- Las VLAN separan el tráfico a **nivel 2**.

### 2.2. Estándar IEEE 802.1Q

- **802.1Q** → etiquetado de VLANs (**tagging**).
- Añade una etiqueta de **4 bytes** a la trama Ethernet.
- Incluye **VLAN ID (VID)** de 12 bits → hasta **4094 VLANs**.

### 2.3. Tipos de puertos

- **Access** → pertenece a **una VLAN**; conecta equipos finales; tráfico sin etiqueta.
- **Trunk** → transporta **varias VLANs**; tráfico etiquetado con 802.1Q (VLAN ID).

### 2.4. Ejemplo de segmentación en un Ayuntamiento

- VLAN 10 → Gestión Tributaria.
- VLAN 20 → Urbanismo.
- VLAN 30 → Policía Local.
- VLAN 40 → Servicios Sociales.
- VLAN 50 → Gestión TIC.
- VLAN 99 → WiFi invitados.

### 2.5. Ventajas de las VLANs

*   **Seguridad:** aislamiento entre departamentos.
*   **Rendimiento:** reducen los dominios de broadcast.
*   **Flexibilidad:** independencia de la ubicación física (misma VLAN, distinta ubicación)
*   **Aislamiento regulatorio:** separación de redes con diferentes requisitos de seguridad.

### 2.6. Inter-VLAN Routing

- VLAN diferentes **no se comunican directamente en Capa 2**.
- Para comunicar VLANs se necesita un dispositivo **Capa 3**:
  - Router.
  - Switch multicapa.
- Permite aplicar **ACL - listas de control de acceso** para controlar el tráfico entre VLANs.

## 3. Protocolos Seguros

### 3.1. El problema de la seguridad en IP

- **IP no incorpora seguridad por sí mismo**.
- Sin protección → riesgo de lectura, modificación o suplantación.
- Protocolos seguros aportan:
  - **Confidencialidad**.
  - **Integridad**.
  - **Autenticación**.

### 3.2. IPSec (Internet Protocol Security)

- Conjunto de protocolos de seguridad en **Capa 3**.
- Servicios:
  - **Autenticación**.
  - **Integridad**.
  - **Confidencialidad**.
  - **Anti-replay**.

**Protocolos:**
- **AH (Authentication Header)** → autenticación + integridad, **sin cifrado**.
- **ESP (Encapsulating Security Payload)** → autenticación + integridad + **cifrado**.

**Modos:**
- **Transporte** → protege payload, mantiene cabecera; comunicación host-to-host.
- **Túnel** → protege paquete IP completo; típico de **VPN Site-to-Site**.

**IKE (Internet Key Exchange)** → protocolo que negocia claves, algoritmos y **Asociaciones de Seguridad (SA)**.

### 3.3. TLS (Transport Layer Security)

- Sucesor de **SSL**, opera en **Capas 4-6** (Transporte-Presentación).
- Protege las comunicaciones mediante:
  - **Cifrado**.
  - **Autenticación** mediante certificados X.509.
  - **Integridad** Códigos HMAC que detectan alteraciones.

**Versiones:**
- SSL 2.0/3.0 → **obsoletas**.
- TLS 1.0/1.1 → **deprecadas**.
- TLS 1.2 → **vigente**.
- TLS 1.3 → **recomendada**.

**Aplicaciones:**
- **HTTPS** = HTTP + TLS → 443.
- **SMTPS** → correo (puerto 465/587)
- **LDAPS** → LDAP seguro (puerto 636).
- **IMAPS** → IMAP seguro (puerto 993)

#### Aplicación

TLS se utiliza para securizar protocolos de aplicación:
*   HTTPS = HTTP + TLS (puerto 443)
*   SMTPS = SMTP + TLS (puerto 465/587)
*   LDAPS = LDAP + TLS (puerto 636)
*   IMAPS = IMAP + TLS (puerto 993)

### 3.4. IPSec vs. TLS

- **IPSec → Capa 3 → protege tráfico IP → VPN**.
- **TLS → Capas 4-6 (Transporte-Presentación) → protege conexiones de aplicaciones → HTTPS/correo**.
- IPSec es más **transparente para las aplicaciones**.
- TLS requiere soporte de la **aplicación/protocolo**.

* **IPSec** se usa para enlazar edificios (Site-to-Site).
 * **TLS** se usa para los portales de **Teletrabajo** (VPN Client-to-Site), porque es más amigable para conectar desde el navegador de casa.

### 3.5. Otros protocolos seguros relevantes

*   **SSH (Secure Shell):** Acceso remoto seguro a servidores (sustituto de Telnet). Puerto 22.
*   **DNSSEC:** protege DNS frente a manipulación/envenenamiento.
*   **WPA3 (Wi-Fi Protected Access 3):** seguridad Wi-Fi; sustituye a WPA2 y utiliza **SAE/Dragonfly**.

## 4. Conclusión

- **VLAN → segmentación y aislamiento** de la red.
- **IPSec → seguridad de tráfico IP/VPN**.
- **TLS → seguridad de servicios y aplicaciones**.
- Juntos proporcionan **defensa en profundidad**: segmentación + cifrado + autenticación + integridad.
- Aplicables a redes de las **AAPP** conforme a los requisitos del **ENS**.

> 🧠 **VLAN = separar | IPSec = proteger IP | TLS = proteger aplicaciones**
