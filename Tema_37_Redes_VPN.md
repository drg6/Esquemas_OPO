# Tema 37.- Redes. Redes privadas virtuales (VPN).

## 1. Introducción

Antes de la popularización de Internet como medio de transporte universal, extender la red corporativa de un Ayuntamiento a una sede remota (una jefatura de Policía Local, un centro cultural, un empleado en teletrabajo) requería contratar **líneas dedicadas (leased lines)** a un operador de telecomunicaciones: enlaces punto a punto exclusivos, extremadamente costosos y con ancho de banda limitado.

Las **Redes Privadas Virtuales (VPN — Virtual Private Network)** permiten crear conexiones seguras sobre infraestructuras de red públicas (Internet), emulando un enlace privado dedicado mediante cifrado y encapsulamiento. El tráfico viaja por Internet pero, gracias a un **túnel cifrado**, los datos son ilegibles para cualquier observador intermedio.

## 2. Concepto y Fundamentos

### 2.1. Funcionamiento básico

Una VPN crea un **túnel** lógico entre dos puntos (un dispositivo cliente y un servidor VPN, o entre dos routers):

1.  Los datos originales se **encapsulan** dentro de un nuevo paquete con cabeceras de túnel.
2.  El contenido se **cifra** (AES-256, ChaCha20) para garantizar la confidencialidad.
3.  Se añade **autenticación** e **integridad** (HMAC, SHA-256) para garantizar que el paquete no ha sido alterado ni proviene de un impostor.
4.  El paquete encapsulado y cifrado viaja por Internet como tráfico ordinario.
5.  En el destino, se descifra, se retira la cabecera de túnel y se entrega al destinatario original.

### 2.2. Servicios de seguridad

| Servicio | Descripción |
|----------|-------------|
| **Confidencialidad** | Los datos viajan cifrados; un interceptor solo ve datos ilegibles |
| **Integridad** | Se detecta cualquier alteración del paquete en tránsito |
| **Autenticación** | Se verifica la identidad de los extremos del túnel |
| **Anti-replay** | Se impide la retransmisión maliciosa de paquetes capturados |

## 3. Tipologías de VPN

### 3.1. Site-to-Site VPN (Red a Red)

Conecta dos redes completas (sedes) de forma permanente. Los routers o firewalls de cada sede establecen un túnel cifrado entre ellos. Los dispositivos finales (PCs, impresoras) no necesitan software VPN ni configuración especial: el cifrado es transparente.

**Caso de uso:** Conectar la sede central del Ayuntamiento con una oficina descentralizada (OMIC, Jefatura de Policía Local, centro de servicios sociales).

```
[Red sede central] ←→ [Router/FW VPN] === TÚNEL CIFRADO === [Router/FW VPN] ←→ [Red sede remota]
                                         (Internet)
```

### 3.2. Client-to-Site VPN (Acceso remoto)

Un usuario individual se conecta desde un dispositivo remoto (portátil, smartphone) a la red corporativa. Requiere un **cliente VPN** instalado en el dispositivo (o acceso a través del navegador en VPNs SSL).

**Caso de uso:** Teletrabajo de funcionarios que acceden a aplicaciones internas (gestor de expedientes, base de datos de tributos, correo corporativo) desde su domicilio.

```
[Portátil funcionario] ←→ [Cliente VPN] === TÚNEL CIFRADO === [Concentrador VPN / FW] ←→ [Red municipal]
                                           (Internet)
```

### 3.3. Client-to-Client VPN (Host a Host)

Conexión cifrada directa entre dos dispositivos específicos. Poco habitual en entornos corporativos; se usa en escenarios de alta seguridad.

## 4. Protocolos VPN

### 4.1. IPSec VPN

**Protocolo de referencia** para VPNs Site-to-Site. Opera en la **Capa 3 (Red)** del modelo OSI.

*   **Fases:** Fase 1 (IKE: negociación de parámetros criptográficos y autenticación mutua) → Fase 2 (establecimiento del túnel de datos con ESP/AH).
*   **Cifrado:** AES-256, AES-GCM.
*   **Autenticación:** Certificados X.509, Pre-Shared Key (PSK).
*   **Ventajas:** Máxima seguridad, estándar universal, transparente para las aplicaciones.
*   **Inconvenientes:** Configuración compleja, puede tener problemas con NAT traversal (solucionable con NAT-T).

### 4.2. SSL/TLS VPN

Opera en las **Capas 4-6** (Transporte-Presentación). Utiliza el protocolo TLS (el mismo que HTTPS) para crear el túnel.

*   **Clientless (sin cliente):** El usuario accede a la VPN a través del navegador web (HTTPS). Solo permite acceso a aplicaciones web internas. No requiere instalación de software.
*   **Con cliente (full-tunnel):** Un agente VPN en el dispositivo redirige todo el tráfico a través del túnel TLS.
*   **Ventajas:** Fácil despliegue, funciona a través de firewalls y proxies (puerto 443), ideal para acceso remoto.
*   **Productos:** Cisco AnyConnect, Fortinet FortiClient, OpenVPN, GlobalProtect (Palo Alto).

### 4.3. WireGuard

Protocolo VPN moderno, de código abierto, que opera en la **Capa 3**:
*   Código base reducido (~4.000 líneas vs. ~100.000 de OpenVPN).
*   Rendimiento superior (menor overhead criptográfico).
*   Configuración más sencilla.
*   Criptografía moderna: Curve25519, ChaCha20, Poly1305.
*   Integrado en el kernel de Linux desde la versión 5.6.

### 4.4. Protocolos obsoletos

| Protocolo | Estado | Motivo |
|-----------|--------|--------|
| **PPTP** | **Prohibido por el ENS** | Cifrado débil (MS-CHAPv2 roto), vulnerabilidades conocidas |
| **L2TP/IPSec** | Aceptable con precauciones | L2TP no cifra por sí mismo; depende de IPSec para la seguridad |

## 5. Split Tunneling vs. Full Tunneling

| Modo | Comportamiento | Seguridad | Rendimiento |
|------|---------------|-----------|-------------|
| **Full Tunnel** | Todo el tráfico del usuario pasa por la VPN | Mayor (todo filtrado por el firewall corporativo) | Menor (mayor consumo de ancho de banda de la VPN) |
| **Split Tunnel** | Solo el tráfico dirigido a la red corporativa pasa por la VPN; el tráfico a Internet sale directamente | Menor (el tráfico a Internet no se filtra) | Mayor (menor consumo de ancho de banda de la VPN) |

El ENS recomienda **Full Tunneling** para teletrabajadores que manejen información sensible.

## 6. VPN en las Administraciones Públicas

### 6.1. Red SARA y VPN

La **Red SARA (Sistema de Aplicaciones y Redes para las Administraciones)** proporciona conectividad segura entre las Administraciones Públicas. Los Ayuntamientos se conectan a Red SARA normalmente mediante VPNs IPSec desde su router de frontera hacia el nodo provincial o autonómico de Red SARA.

### 6.2. Requisitos del ENS para VPNs

*   Cifrado mínimo: AES-128 (recomendado AES-256).
*   Autenticación mediante certificados digitales (preferido sobre PSK).
*   Protocolo: IPSec o TLS 1.2+ (PPTP prohibido).
*   Autenticación del usuario: Doble factor (MFA) obligatorio para acceso remoto.
*   Registro (logging) de todas las conexiones VPN.

## 7. Conclusión

Las Redes Privadas Virtuales (VPN) permiten extender la red municipal de forma segura sobre Internet, eliminando la necesidad de costosas líneas dedicadas. Las VPNs Site-to-Site (IPSec) conectan sedes de forma transparente, mientras que las VPNs Client-to-Site (SSL/TLS) habilitan el teletrabajo seguro de los empleados públicos. Los protocolos modernos (IPSec con IKEv2, TLS 1.3, WireGuard) proporcionan cifrado robusto, y la integración con la Red SARA garantiza la interconexión segura entre Administraciones Públicas conforme a los requisitos del ENS.
