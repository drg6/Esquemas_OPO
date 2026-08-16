# Tema 40.- Dispositivos personales de PC y dispositivos móviles. La conectividad de los dispositivos personales. Sistemas de administración de entorno de usuario y estaciones de trabajo. Distribución de software. Gestión de dispositivos externos (BYOD).

## 1. Introducción

El puesto de trabajo del empleado público ha evolucionado de un ordenador de sobremesa fijo, aislado y monofuncional, a un ecosistema híbrido de dispositivos — PC de escritorio, portátil, tablet, smartphone — que se conectan desde múltiples ubicaciones (oficina, domicilio, movilidad) a los recursos corporativos del Ayuntamiento. Esta evolución, acelerada por la implantación del teletrabajo, plantea desafíos de gestión, seguridad y soporte.

Este tema analiza los dispositivos del puesto de trabajo digital, sus mecanismos de conectividad, los sistemas de administración centralizada, la distribución automatizada de software y la gestión de dispositivos personales (BYOD).

## 2. El Puesto de Trabajo Digital

### 2.1. Estaciones de trabajo (PC)

*   **PC de escritorio (Desktop):** Equipo fijo con monitor, teclado y ratón. Mayor rendimiento y durabilidad. Ideal para puestos de atención presencial.
*   **Portátil (Laptop):** Movilidad para teletrabajo y reuniones. Requiere medidas adicionales de seguridad (cifrado de disco, VPN).
*   **Thin Client (Cliente ligero):** Dispositivo de bajo coste y consumo que no ejecuta aplicaciones localmente, sino que se conecta a un escritorio virtual (VDI — Tema 41). Ventajas: seguridad (sin datos locales), gestión centralizada, menor consumo energético.
*   **All-in-One:** PC con el equipo integrado en el monitor. Ahorro de espacio en oficinas.

### 2.2. Dispositivos móviles

*   **Smartphones:** Acceso al correo corporativo, aplicaciones móviles internas, firma electrónica desde campo.
*   **Tablets:** Levantamiento de actas de inspección, consulta de expedientes en movilidad.
*   **Wearables:** Casos específicos (policía local con cámaras corporales).

### 2.3. Conectividad

*   **Red corporativa LAN/WLAN:** Ethernet (Gigabit), Wi-Fi (WPA3-Enterprise con 802.1X/RADIUS).
*   **VPN:** Acceso remoto seguro desde el domicilio o desde redes públicas (Tema 37).
*   **Redes móviles (4G/5G):** Conectividad en movilidad, con tarjeta SIM corporativa o APN privada.
*   **Bluetooth:** Para periféricos (auriculares, ratones). Riesgo de seguridad si no se controla.

## 3. Sistemas de Administración del Entorno de Usuario

### 3.1. Active Directory (AD)

**Active Directory** es el servicio de directorio de Microsoft que centraliza la gestión de identidades y accesos en entornos Windows:

*   **Usuarios y grupos:** Cada funcionario tiene una cuenta de usuario con su nombre principal (UPN).
*   **Unidades Organizativas (OU):** Estructura jerárquica para organizar usuarios por departamento.
*   **GPO (Group Policy Objects):** Políticas de grupo que controlan la configuración de los equipos y las restricciones de los usuarios de forma centralizada:
    *   Política de contraseñas (longitud mínima, complejidad, caducidad).
    *   Restricciones de software (impedir la instalación de aplicaciones no autorizadas).
    *   Configuración de seguridad (bloqueo de puertos USB, cifrado BitLocker obligatorio).
    *   Mapeo de unidades de red e impresoras.
    *   Configuración del fondo de pantalla y menú de inicio.

### 3.2. Azure Active Directory (Entra ID)

Extensión cloud de Active Directory que permite la gestión de identidades en entornos híbridos (on-premise + cloud):
*   Single Sign-On (SSO) para aplicaciones SaaS (Microsoft 365, aplicaciones web).
*   Autenticación multifactor (MFA).
*   Conditional Access: políticas de acceso basadas en ubicación, dispositivo, riesgo.

## 4. Distribución de Software

### 4.1. El problema

Un Ayuntamiento con 2.000 puestos de trabajo no puede gestionar manualmente la instalación, actualización y parcheado del software. La distribución de software automatizada es imprescindible para:
*   Instalar aplicaciones corporativas (gestor de expedientes, cliente de correo).
*   Aplicar parches de seguridad (actualizaciones de Windows, navegadores, Java).
*   Desplegar configuraciones estándar (perfiles de certificados, configuración VPN).

### 4.2. Herramientas de distribución

| Herramienta | Tipo | Características |
|-------------|------|----------------|
| **Microsoft SCCM/MECM** | Comercial | Distribución de software, parches, inventario HW/SW, imágenes de SO |
| **Microsoft Intune** | Cloud (SaaS) | Gestión de dispositivos (MDM/MAM), distribución de apps, políticas de cumplimiento |
| **WSUS** | Gratuito (Windows) | Gestión centralizada de actualizaciones de Windows |
| **Ansible** | Open source | Automatización de configuración y despliegue (infraestructura como código) |
| **Puppet / Chef** | Open source | Gestión de configuración declarativa |

### 4.3. Proceso de parcheado

1.  **Evaluación:** Identificar parches críticos de seguridad (CVE).
2.  **Prueba:** Desplegar el parche en un grupo piloto para verificar compatibilidad.
3.  **Aprobación:** Validar que el parche no genera problemas.
4.  **Despliegue:** Distribución masiva al resto de equipos (ventana de mantenimiento).
5.  **Verificación:** Confirmar la instalación exitosa y monitorizar incidencias.

## 5. BYOD (Bring Your Own Device)

### 5.1. Concepto

**BYOD** es una política organizacional que permite a los empleados utilizar sus **dispositivos personales** (smartphones, tablets, portátiles) para acceder a los recursos corporativos (correo electrónico, aplicaciones internas, documentos).

### 5.2. Riesgos

*   **Fuga de datos:** Datos corporativos almacenados en dispositivos no controlados.
*   **Malware:** El dispositivo personal puede estar infectado y propagar el malware a la red corporativa.
*   **Pérdida o robo:** Un dispositivo perdido con acceso al correo corporativo es una brecha de seguridad.
*   **Incumplimiento normativo:** Datos del padrón o tributos en un dispositivo personal vulneran el RGPD y el ENS.

### 5.3. MDM (Mobile Device Management)

Para mitigar los riesgos del BYOD, el departamento de TIC implanta un **MDM (Mobile Device Management)**: una plataforma que gestiona de forma remota los dispositivos móviles.

**Funcionalidades:**
*   **Contenedorización:** Separación del espacio personal y el corporativo en el mismo dispositivo:
    *   **Contenedor personal:** Fotos, apps, datos personales. El Ayuntamiento no tiene visibilidad ni control.
    *   **Contenedor corporativo:** Correo, documentos, apps corporativas. Cifrado independiente, gestión centralizada.
*   **Cifrado obligatorio:** El MDM puede exigir el cifrado del dispositivo como requisito de acceso.
*   **Políticas de seguridad:** PIN/biometría obligatorios, bloqueo tras intentos fallidos, cifrado de almacenamiento.
*   **Borrado remoto selectivo (Remote Wipe):** Si el empleado deja la organización o pierde el dispositivo, el MDM puede borrar únicamente el contenedor corporativo, sin afectar a los datos personales.
*   **Control de aplicaciones:** Lista blanca/negra de aplicaciones permitidas en el contenedor corporativo.

### 5.4. Soluciones MDM/UEM

| Solución | Tipo |
|----------|------|
| **Microsoft Intune** | Cloud (incluido en Microsoft 365 E3/E5) |
| **VMware Workspace ONE** | On-premise / Cloud |
| **MobileIron (Ivanti)** | On-premise / Cloud |
| **Samsung Knox** | Específico para dispositivos Samsung |
| **Apple Business Manager + MDM** | Específico para dispositivos Apple |

### 5.5. Modelos alternativos al BYOD

| Modelo | Propiedad del dispositivo | Control corporativo |
|--------|--------------------------|---------------------|
| **BYOD** | Empleado | Parcial (contenedorización) |
| **COPE** (Corporate-Owned, Personally Enabled) | Organización | Total. Se permite uso personal limitado |
| **COBO** (Corporate-Owned, Business Only) | Organización | Total. Solo uso corporativo |
| **CYOD** (Choose Your Own Device) | Organización | Total. El empleado elige entre modelos preaprobados |

## 6. Conclusión

La gestión del puesto de trabajo digital en las Administraciones Públicas abarca desde la administración centralizada de estaciones de trabajo (Active Directory, GPOs) hasta la distribución automatizada de software (SCCM, Intune, Ansible) y la gestión de dispositivos personales (BYOD con MDM). La contenedorización y el borrado remoto selectivo permiten conciliar la productividad del empleado en movilidad con la seguridad de los datos corporativos, cumpliendo los requisitos del ENS y del RGPD.
