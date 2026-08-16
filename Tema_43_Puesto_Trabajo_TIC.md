# Tema 43.- El puesto de trabajo TIC: normalización, políticas de seguridad, actualización y despliegue. Sistemas de administración de entorno de usuario y estaciones de trabajo. Distribución de software. Centro de Atención a los usuarios. Herramientas digitales, políticas de seguridad y configuración de sistemas. Soporte y resolución de incidencias.

## 1. Introducción

El puesto de trabajo TIC es el punto de contacto directo entre el empleado público y los sistemas de información de la Administración. Su correcta normalización, securización y gestión determinan la productividad de los funcionarios, la seguridad de la información y la calidad del servicio público. Un Ayuntamiento con 2.000 puestos de trabajo necesita mecanismos centralizados de administración, distribución de software, aplicación de políticas de seguridad y un Centro de Atención a Usuarios (CAU) que garantice la resolución eficaz de incidencias.

Este tema analiza la normalización del puesto de trabajo TIC, los sistemas de administración del entorno de usuario, la distribución de software, las herramientas digitales, las políticas de seguridad, la configuración de sistemas y el soporte y resolución de incidencias bajo el marco ITIL.

## 2. Normalización del Puesto de Trabajo TIC

### 2.1. Concepto

La **normalización** consiste en establecer un estándar corporativo para todos los puestos de trabajo de la organización: hardware homogéneo, software estandarizado, configuración uniforme y políticas de seguridad aplicadas de forma consistente.

### 2.2. Componentes de la normalización

| Componente | Estándar |
|-----------|----------|
| **Hardware** | Catálogo de modelos aprobados (PC sobremesa, portátil, thin client, monitor), especificaciones mínimas (CPU, RAM, disco) |
| **Sistema operativo** | Versión corporativa estándar (ej. Windows 11 Enterprise LTSC), imagen maestra |
| **Software de base** | Paquete ofimático estándar (Microsoft 365, LibreOffice), navegador (Edge/Chrome), cliente de correo, visor PDF, antivirus |
| **Software departamental** | Aplicaciones específicas por perfil (gestor de expedientes, SIG para urbanismo, SGBD para informática) |
| **Configuración de seguridad** | Cifrado de disco (BitLocker), antivirus/EDR, firewall local, restricciones de acceso USB |
| **Periféricos** | Modelos de impresora, escáner, lector de tarjetas (DNIe/certificados) homologados |

### 2.3. Imagen maestra (Golden Image)

Se crea una **imagen maestra** del sistema operativo con todo el software de base preinstalado y configurado. Esta imagen se replica en todos los puestos de trabajo mediante herramientas de despliegue (WDS, MDT, SCCM/MECM). Ventajas:
*   Despliegue rápido de nuevos puestos (minutos en lugar de horas).
*   Garantía de configuración homogénea.
*   Facilidad de reinstalación ante incidencias graves.

## 3. Políticas de Seguridad del Puesto de Trabajo

### 3.1. Políticas de contraseñas

| Parámetro | Valor recomendado (ENS) |
|-----------|------------------------|
| Longitud mínima | 12 caracteres |
| Complejidad | Mayúsculas + minúsculas + números + caracteres especiales |
| Caducidad | 90 días (categoría alta: 60 días) |
| Historial | No reutilizar las últimas 12 contraseñas |
| Bloqueo | Tras 5 intentos fallidos (desbloqueo tras 30 min o por administrador) |

### 3.2. Políticas de cifrado

*   **Cifrado de disco completo:** BitLocker (Windows), LUKS (Linux). Obligatorio para portátiles (riesgo de pérdida/robo).
*   **Cifrado de dispositivos extraíbles:** Los USB deben cifrarse con BitLocker To Go o VeraCrypt.
*   **Clave de recuperación:** Almacenada en Active Directory para rescate por el administrador.

### 3.3. Control de dispositivos

*   **Restricción de puertos USB:** Bloqueo de dispositivos de almacenamiento USB no autorizados. Solo se permiten dispositivos corporativos cifrados.
*   **Deshabilitación de autorun/autoplay.**
*   **Control de periféricos:** Lista blanca de dispositivos permitidos (por ID de hardware).

### 3.4. Protección endpoint

*   **Antivirus / EDR (Endpoint Detection and Response):** Detección de malware, análisis de comportamiento, respuesta automatizada. Productos: Microsoft Defender for Endpoint, CrowdStrike Falcon, SentinelOne.
*   **Firewall local:** Reglas de entrada/salida configuradas por GPO.
*   **Control de aplicaciones (AppLocker / WDAC):** Solo se permite la ejecución de aplicaciones autorizadas (lista blanca).

### 3.5. Actualizaciones de seguridad

*   **Parcheado de SO:** Windows Update gestionado por WSUS o SCCM/MECM.
*   **Parcheado de aplicaciones:** Navegadores, Java, Adobe, aplicaciones de terceros.
*   **Ventana de parcheado:** Despliegue nocturno o en fin de semana para minimizar el impacto.
*   **Tiempo máximo de aplicación:** 30 días desde la publicación del parche (guías CCN-STIC).

## 4. Sistemas de Administración del Entorno de Usuario

### 4.1. Active Directory (AD) y GPOs

**Active Directory** centraliza la gestión de identidades y la aplicación de políticas:

*   **Unidades Organizativas (OU):** Estructura jerárquica que agrupa usuarios y equipos por departamento, planta o función.
*   **GPO (Group Policy Objects):** Políticas que se aplican automáticamente a usuarios y equipos vinculados a cada OU:
    *   Configuración de escritorio, menú inicio, fondo de pantalla corporativo.
    *   Mapeo de unidades de red e impresoras.
    *   Restricción de instalación de software.
    *   Configuración de proxy y certificados.
    *   Redirección de carpetas (Mis Documentos → servidor de ficheros).
    *   Scripts de inicio de sesión (logon scripts).

### 4.2. Azure AD / Microsoft Entra ID

Extensión cloud de Active Directory para entornos híbridos:
*   **Azure AD Connect:** Sincronización de usuarios entre AD on-premise y Azure AD.
*   **Single Sign-On (SSO):** Un único inicio de sesión para acceder a aplicaciones locales y cloud (Microsoft 365).
*   **Conditional Access:** Políticas que condicionan el acceso según la ubicación, el dispositivo, el nivel de riesgo y el cumplimiento de las políticas de seguridad.
*   **MFA (Multi-Factor Authentication):** Segundo factor obligatorio (app, SMS, token).

### 4.3. Microsoft Intune / Endpoint Manager

Plataforma de gestión unificada de dispositivos (UEM — Unified Endpoint Management):
*   Gestión de PCs, portátiles, móviles y tablets desde un único panel cloud.
*   Políticas de cumplimiento (compliance policies): si el dispositivo no cumple los requisitos (antivirus activo, SO actualizado, cifrado), se bloquea el acceso a recursos corporativos.
*   Despliegue de aplicaciones y actualizaciones.
*   Integración con Conditional Access de Azure AD.

## 5. Distribución de Software

### 5.1. Herramientas de distribución

| Herramienta | Tipo | Funcionalidades |
|-------------|------|----------------|
| **SCCM/MECM** | On-premise (Microsoft) | Distribución de software, parches, inventario HW/SW, imágenes de SO, informes |
| **Microsoft Intune** | Cloud (SaaS) | Distribución de apps (Win32, MSI, AppX, iOS, Android), políticas de cumplimiento |
| **WSUS** | On-premise (gratuito) | Gestión centralizada de Windows Update |
| **Ansible** | Open source (Red Hat) | Automatización de configuración y despliegue (Linux y Windows) |
| **Puppet** | Open source | Gestión de configuración declarativa |
| **PDQ Deploy** | Comercial | Distribución de software silenciosa en redes Windows |

### 5.2. Proceso de despliegue

1.  **Empaquetado:** El software se empaqueta en formato MSI, MSIX o script de instalación silenciosa.
2.  **Pruebas:** Se despliega en un grupo piloto (equipos de prueba) para verificar compatibilidad.
3.  **Aprobación:** El responsable de TIC aprueba el despliegue masivo.
4.  **Distribución:** Se distribuye a los puntos de distribución (distribution points) de SCCM o a través de Intune.
5.  **Instalación:** Se instala de forma silenciosa y desatendida (sin intervención del usuario), normalmente durante la noche o en el siguiente reinicio.
6.  **Verificación:** Se confirma la instalación exitosa mediante informes de cumplimiento.

### 5.3. Gestión de licencias

*   **Inventario de software:** SCCM/MECM realiza inventario automático del software instalado en cada equipo.
*   **Sam (Software Asset Management):** Control de licencias para evitar el uso de software sin licencia (auditorías de cumplimiento).
*   **Modelos de licenciamiento:** Licencia perpetua, suscripción (Microsoft 365), licencia por volumen (VL), acuerdo marco SARA.

## 6. Centro de Atención a Usuarios (CAU)

### 6.1. Concepto

El **CAU (Centro de Atención a Usuarios)**, también denominado **Service Desk** o **Help Desk**, es el punto único de contacto (SPOC — Single Point of Contact) entre los usuarios de la organización y el servicio de TIC. Su función es recibir, registrar, clasificar, resolver o escalar todas las incidencias y peticiones de los usuarios.

### 6.2. Marco ITIL

El CAU opera conforme a las mejores prácticas de **ITIL (Information Technology Infrastructure Library)**, el estándar de facto para la gestión de servicios TI:

*   **Gestión de Incidencias (Incident Management):** Restaurar el servicio normal lo antes posible minimizando el impacto.
*   **Gestión de Peticiones de Servicio (Service Request Management):** Solicitudes predefinidas (alta de usuario, cambio de contraseña, instalación de software).
*   **Gestión de Problemas (Problem Management):** Identificar y eliminar la causa raíz de incidencias recurrentes.
*   **Gestión de Cambios (Change Management):** Controlar los cambios en la infraestructura TIC para minimizar el riesgo.
*   **Gestión del Conocimiento (Knowledge Management):** Base de conocimiento con soluciones a incidencias conocidas (FAQ, artículos KB).

### 6.3. Niveles de soporte

| Nivel | Función | Personal |
|-------|---------|----------|
| **Nivel 0 (Autoservicio)** | Portal de autoservicio, FAQ, base de conocimiento | Automatizado / Usuario |
| **Nivel 1 (First Line)** | Registro, clasificación, resolución de incidencias básicas (contraseñas, impresoras, correo) | Técnicos de CAU |
| **Nivel 2 (Second Line)** | Resolución de incidencias complejas (software, red, sistemas) | Técnicos especializados |
| **Nivel 3 (Third Line)** | Incidencias que requieren desarrollo, cambio de infraestructura o intervención del fabricante | Ingenieros de sistemas, desarrollo, proveedores |

### 6.4. Herramientas de gestión de incidencias

| Herramienta | Tipo |
|-------------|------|
| **ServiceNow** | Cloud (SaaS), líder en ITSM |
| **Jira Service Management** | Cloud / On-premise (Atlassian) |
| **GLPI** | Open source |
| **OTRS** | Open source / Comercial |
| **Freshdesk** | Cloud (SaaS) |

### 6.5. Indicadores clave (KPIs) del CAU

| KPI | Descripción | Objetivo típico |
|-----|-------------|----------------|
| **Tiempo medio de respuesta** | Tiempo desde el registro hasta la primera respuesta | < 15 minutos |
| **Tiempo medio de resolución** | Tiempo desde el registro hasta la resolución | < 4 horas (Nivel 1), < 8 horas (Nivel 2) |
| **Tasa de resolución en primer contacto (FCR)** | % de incidencias resueltas en Nivel 1 sin escalar | > 70% |
| **Satisfacción del usuario** | Encuesta post-resolución | > 4/5 |
| **Incidencias pendientes (backlog)** | Incidencias abiertas sin resolver | Tendencia decreciente |

## 7. Soporte y Resolución de Incidencias

### 7.1. Ciclo de vida de una incidencia (ITIL)

```
[Detección] → [Registro] → [Clasificación/Priorización] → [Diagnóstico] → [Resolución] → [Cierre] → [Revisión]
```

1.  **Detección:** El usuario reporta la incidencia (teléfono, email, portal web) o se detecta por monitorización automática.
2.  **Registro:** Se crea un ticket en el sistema de gestión (ServiceNow, GLPI) con: fecha, usuario, descripción, categoría, urgencia.
3.  **Clasificación y priorización:** Se asigna categoría (hardware, software, red, acceso) y prioridad (según impacto × urgencia):

| | Urgencia Alta | Urgencia Media | Urgencia Baja |
|---|---|---|---|
| **Impacto Alto** | Crítica (P1) | Alta (P2) | Media (P3) |
| **Impacto Medio** | Alta (P2) | Media (P3) | Baja (P4) |
| **Impacto Bajo** | Media (P3) | Baja (P4) | Planificada (P5) |

4.  **Diagnóstico:** El técnico analiza la causa (consulta la base de conocimiento, replica el error, revisa logs).
5.  **Resolución:** Se aplica la solución (reinicio de servicio, reinstalación de driver, cambio de hardware, ajuste de GPO).
6.  **Cierre:** Se documenta la solución, se informa al usuario y se cierra el ticket.
7.  **Revisión:** Las incidencias recurrentes se escalan a Gestión de Problemas para investigar la causa raíz.

### 7.2. Herramientas de soporte remoto

*   **Microsoft Remote Desktop (RDP):** Conexión remota al escritorio del usuario.
*   **Asistencia remota de Windows:** El técnico visualiza y controla la pantalla del usuario con su consentimiento.
*   **TeamViewer / AnyDesk:** Herramientas de escritorio remoto multiplataforma.
*   **PowerShell Remoting:** Ejecución de comandos remotos en servidores y PCs Windows.

## 8. Herramientas Digitales del Puesto de Trabajo

### 8.1. Suite ofimática

*   **Microsoft 365:** Word, Excel, PowerPoint, Outlook, Teams, SharePoint, OneDrive.
*   **LibreOffice:** Alternativa open source (Writer, Calc, Impress, Draw).
*   **Formato estándar:** ODF (Open Document Format) — interoperabilidad conforme al ENI.

### 8.2. Herramientas de comunicación

*   **Microsoft Teams:** Chat, videollamadas, canales, integración con SharePoint y OneDrive.
*   **Correo electrónico corporativo:** Microsoft Exchange / Exchange Online.

### 8.3. Herramientas de firma y certificados

*   **AutoFirma:** Aplicación del Ministerio para la firma electrónica de documentos.
*   **Lector de tarjetas criptográficas:** Para DNIe y tarjetas FNMT.
*   **Drivers de certificados:** Módulos PKCS#11 para los navegadores.

### 8.4. Herramientas de seguridad

*   **VPN corporativa:** Acceso remoto seguro (Tema 37).
*   **Antivirus/EDR:** Microsoft Defender for Endpoint.
*   **Cifrado:** BitLocker, VeraCrypt.

## 9. Conclusión

El puesto de trabajo TIC es la interfaz entre el empleado público y los sistemas de información de la Administración. Su normalización (hardware y software estándar, imagen maestra), la aplicación de políticas de seguridad (GPOs, cifrado, control de dispositivos, parcheado), la distribución automatizada de software (SCCM, Intune) y un Centro de Atención a Usuarios (CAU) operando bajo las mejores prácticas ITIL garantizan la productividad, la seguridad y la continuidad del servicio. La gestión eficaz de incidencias — con niveles de soporte escalonados, priorización basada en impacto y urgencia, y herramientas de soporte remoto — asegura que los puestos de trabajo TIC funcionen de forma fiable y conforme a los requisitos del Esquema Nacional de Seguridad.
