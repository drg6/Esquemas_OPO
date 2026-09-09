# Tema 37.- Redes Privadas Virtuales (VPN).

## 1. Introducción y Concepto
* **Contexto:** Extender la red municipal a sedes remotas o teletrabajadores usando líneas dedicadas es inasumible en costes. 
* **Concepto VPN:** Conexión segura sobre una red pública (Internet) que emula un enlace privado.
* **El mecanismo:** Crea un **Túnel** lógico → **Encapsula** el paquete → **Cifra** el dato → Añade **Autenticación e Integridad** → Transmite.

## 2. Servicios de Seguridad Aportados
* **Confidencialidad:** Cifrado (AES-256). Solo los extremos pueden leer el dato.
* **Integridad:** Hashes (SHA-256/HMAC) detectan si el paquete fue alterado.
* **Autenticación:** Certificados (X.509) o PSK validan quién está al otro lado.
* **Anti-replay:** Protege contra la retransmisión de paquetes interceptados.

## 3. Tipologías de VPN (La Arquitectura)
* **3.1. Site-to-Site (Red a Red):**
  * Une sedes físicas de forma permanente (ej. Ayuntamiento con Comisaría).
  * Transparente para el usuario final (Capa 3).
  * Se levantan entre los **NGFW (Next-Generation Firewalls)** de cada sede.
* **3.2. Client-to-Site (Acceso Remoto / Teletrabajo):**
  * Conecta el dispositivo del empleado a la red corporativa.
  * Requiere **Agente VPN** (o navegador).
  * ⚠️ *Detalle AAPP:* Requiere integración con Directorio Activo y **MFA (Múltiple Factor de Autenticación)** por imperativo del ENS.

## 4. Protocolos VPN
* **4.1. IPSec (Capa 3 - Estándar Site-to-Site):**
  * *Fases:* IKE (Negociación/Claves) → ESP/AH (Cifrado e integridad de datos).
  * Seguro y transparente, pero complejo si hay NAT (requiere NAT-T).
* **4.2. SSL/TLS (Capas 4 a 6 - Estándar Teletrabajo):**
  * *Clientless* (vía web HTTPS) o *Full-Tunnel* (con Agente).
  * Amigable con firewalls de terceros (usa el puerto 443). Ej: GlobalProtect, AnyConnect.
* **4.3. WireGuard:** Protocolo de Capa 3 moderno, ligero (integrado en el kernel Linux), altísimo rendimiento (usa Curve25519/ChaCha20).
* **4.4. Obsoletos:** **PPTP** (Cifrado roto, prohibido explícitamente en el ENS).

## 5. El Control del Tráfico: Split vs. Full Tunneling
* **Split Tunneling:** Solo el tráfico corporativo va por la VPN. El tráfico de Internet va directo. (+ Rendimiento, - Seguridad).
* **Full Tunneling:** TODO el tráfico va por la VPN.
  * ⚠️ *Regla ENS:* Recomendado para teletrabajo, ya que obliga al PC doméstico a salir a Internet filtrado por el firewall, proxy y antivirus del Ayuntamiento.

## 6. Las VPN en la Administración Pública
* **Red SARA:** Se utiliza IPSec para conectar de forma segura los routers perimetrales de los Ayuntamientos con los nodos de la Red SARA.
* **El futuro (ZTNA - Zero Trust Network Access):** Ante los riesgos de la VPN tradicional (que da acceso a toda la subred), las AAPP están migrando a ZTNA. Principio de menor privilegio: el teletrabajador no se conecta a la "red", sino a una pasarela que solo le da acceso a "la aplicación" específica que necesita.

## 7. Conclusión
Las VPN son la infraestructura habilitadora del teletrabajo y la descentralización municipal. Ya sea mediante túneles IPSec para interconectar oficinas (Site-to-Site) o portales TLS para funcionarios, la VPN debe aplicarse bajo la política estricta del ENS: cifrado fuerte (AES), túnel completo (Full-Tunnel) y, obligatoriamente, doble factor de autenticación (MFA) evolucionando hacia arquitecturas Zero Trust.

-----------------------------

# Tema 37.- Redes. Redes privadas virtuales (VPN).

## 1. Introducción

- **VPN** → crea una conexión segura sobre **Internet**, sustituyendo la necesidad de líneas dedicadas.
- Utiliza **túnel + encapsulamiento + cifrado**, ilegible para un observador.

## 2. Concepto y Fundamentos

### 2.1. Funcionamiento básico

VPN crea un **túnel** lógico entre dos puntos:
1. **Encapsular** → datos dentro de un paquete de túnel.
2. **Cifrar** → AES-256 / ChaCha20.
3. **Autenticar + garantizar integridad** → HMAC / SHA-256.
4. **Transmitir** → por Internet.
5. **Descifrar y desencapsular** → entrega de los datos originales.

### 2.2. Servicios de seguridad

- **Confidencialidad** → cifrado.
- **Integridad** → detectar modificaciones.
- **Autenticación** → verificar extremos.
- **Anti-replay** → evitar reutilización de paquetes capturados.

## 3. Tipologías de VPN

### 3.1. Site-to-Site VPN (Red a Red)

- Conecta **redes completas** de distintas sedes.
- Túnel entre **routers/firewalls**.
- Transparente para los usuarios, sin software o configuracion especial.
- Habitualmente **IPSec**.
* Se levantan entre los **NGFW (Next-Generation Firewalls)** de cada sede (ej. Fortinet, Palo Alto, Sophos).

### 3.2. Client-to-Site VPN (Acceso remoto)

- Conecta **usuario/dispositivo → red corporativa**.
- Cliente VPN o navegador (VPNs SSL)).
- Uso típico: **teletrabajo**.
* Requiere integración con Directorio Activo y **MFA (Múltiple Factor de Autenticación)** por imperativo del ENS.

### 3.3. Client-to-Client VPN (Host a Host)

- Conecta directamente **dos dispositivos**.
- Poco habitual en entornos corporativos, escenarios de alta seguridad.

## 4. Protocolos VPN

### 4.1. IPSec VPN

- **Capa 3**.
- Referencia para **Site-to-Site**.
- Fase 1 **IKE** → negociación y autenticación.
- Fase 2 **ESP/AH** → protección de datos.
- Cifrado: **AES-256 / AES-GCM**.
- Autenticación: **X.509 / Pre-Shared Key (PSK)**.
- **NAT-T** → permite IPSec a través de NAT.

- **Ventajas:** Máxima seguridad, estándar universal, transparente para las aplicaciones. 
- **Inconvenientes:** Configuración compleja

### 4.2. SSL/TLS VPN

- Opera en **Capas 4-6** (Transporte-Presentación)
- Basada en **TLS** (como HTTPS).
- **Clientless** → navegador, normalmente HTTPS/443.
- **Con cliente (full-tunnel)** → agente VPN, tunel TLS.
- Ventajas: **fácil despliegue + compatible con firewalls/proxies**.
- Ej.: AnyConnect, FortiClient, OpenVPN, GlobalProtect.

### 4.3. WireGuard

- VPN moderna y de **código abierto**.
- **Capa 3**.
- Simple, ligera y de alto rendimiento.
- Criptografía: **Curve25519 + ChaCha20 + Poly1305**.
- Integrada en el kernel de Linux desde **5.6**.

### 4.4. Protocolos obsoletos

- **PPTP** → obsoleto/inseguro; prohibido ENS.
- **L2TP/IPSec** → L2TP no cifra; depende de **IPSec**. Aceptable con precauciones

## 5. Split Tunneling vs. Full Tunneling

- **Full Tunnel** → TODO el tráfico pasa por la VPN.
  - **+ Seguridad**
  - **− Rendimiento/ancho de banda**
- **Split Tunnel** → solo tráfico corporativo pasa por VPN.
  - **+ Rendimiento**
  - **− Seguridad**

El ENS recomienda **Full Tunneling** para teletrabajadores que manejen información sensible.

## 6. VPN en las Administraciones Públicas

### 6.1. Red SARA y VPN

- **Red SARA (Sistema de Aplicaciones y Redes para las Administraciones)** → conectividad segura entre AAPP.
- Puede utilizar **VPN IPSec** para conectar las redes administrativas.

### 6.2. Requisitos del ENS para VPNs

*   Cifrado mínimo: AES-128 (recomendado AES-256).
*   Autenticación mediante certificados digitales (preferido sobre PSK).
*   Protocolo: IPSec o TLS 1.2+ (PPTP prohibido).
*   Autenticación del usuario: Doble factor (MFA) obligatorio para acceso remoto.
*   Registro (logging) de todas las conexiones VPN.

### 6.3. El futuro (ZTNA - Zero Trust Network Access)

Ante los riesgos de la VPN tradicional (que da acceso a toda la subred), las AAPP están migrando a ZTNA. 
Principio de menor privilegio: el teletrabajador no se conecta a la "red", sino a una pasarela que solo le da acceso a "la aplicación" específica que necesita.

## 7. Conclusión

- **VPN = red segura sobre Internet**, eliminando lineas dedicadas costosas.
- **Site-to-Site → IPSec → conecta sedes.**
- **Client-to-Site → SSL/TLS → teletrabajo.**
- Protocolos modernos: **IPSec/IKEv2, TLS 1.3, WireGuard**.

🧠 **VPN = TÚNEL → CIFRAR → AUTENTICAR → CONECTAR**