# Tema 43.- El puesto de trabajo TIC: normalización, seguridad, distribución, CAU (ITIL) y soporte.

## 1. Introducción
* **El Reto:** El puesto TIC es la frontera entre el funcionario y los datos del ciudadano. Su gestión en una Administración de 2.000 equipos exige abandonar la administración manual en favor de políticas centralizadas, distribución automatizada y un soporte basado en estándares (ITIL) para garantizar la seguridad (ENS).

## 2. Normalización y Despliegue del Puesto de Trabajo
* **2.1. Normalización:** Estándar corporativo único (Hardware homologado, SO base como Windows 11 Enterprise LTSC, y software ofimático).
* **2.2. Despliegue Clásico (Imagen Maestra / Golden Image):** 
  * Captura de un SO "perfecto" clonado mediante **WDS, MDT o MECM (SCCM)**.
* **2.3. Despliegue Moderno (Zero-Touch Provisioning):**
  * Tendencia actual mediante **Windows Autopilot + Intune**. El equipo se entrega de fábrica al usuario; al loguearse con su cuenta corporativa, se autoconfigura sin que el departamento de Microinformática tenga que tocar el hardware físico.

## 3. Políticas de Seguridad del Entorno (Directrices ENS)
* **3.1. Identidad y Accesos:** Contraseñas de 12 caracteres, caducidad (90 días), bloqueo (5 intentos) y **MFA obligatorio** para administradores TIC y accesos remotos.
* **3.2. Cifrado y Control:** **BitLocker** obligatorio en portátiles y pendrives corporativos. Deshabilitación de *Autorun* y control de periféricos por lista blanca.
* **3.3. El blindaje del Administrador (LAPS):** 
  * Despliegue de **Microsoft LAPS** (Local Administrator Password Solution) para aleatorizar la contraseña del administrador local de cada PC, evitando movimientos laterales de malware/ransomware.
* **3.4. Actualizaciones (Patch Management):** Objetivo de parcheo < 30 días apoyado en las alertas del CCN-CERT.

## 4. Administración Centralizada y Distribución de Software
* **4.1. El Eje On-Premise (AD + GPO + MECM):**
  * **AD:** Organiza las identidades en OUs (desacopladas del hardware).
  * **GPOs:** Inyectan la configuración (fondo de pantalla, discos de red, LAPS).
  * **MECM / WSUS:** Distribución pesada de software y parches.
* **4.2. El Eje Cloud (Entra ID + Intune):** Gestión MDM/UEM y políticas de Acceso Condicional (ej. "no puedes abrir el correo municipal si te conectas desde fuera de España o sin antivirus").
* **4.3. Automatización Ágil (WinGet):** Uso de gestores de paquetes por línea de comandos (WinGet) combinados con scripts PowerShell para mantener el software de terceros actualizado silenciosamente.

## 5. El Centro de Atención a Usuarios (CAU) y el marco ITIL
* **5.1. Concepto SPOC:** Punto Único de Contacto entre el usuario y la tecnología.
* **5.2. Marco ITIL 4 (Prácticas clave):**
  * *Gestión de Incidencias:* Apagar el fuego rápido.
  * *Gestión de Peticiones:* Trámites estándar (pedir un ratón, instalar Adobe).
  * *Gestión de Problemas:* Investigar la causa raíz de incidencias repetitivas.
* **5.3. El ciclo de la incidencia:** Detección → Registro → Priorización (Impacto × Urgencia) → Diagnóstico → Resolución → Cierre.
* **5.4. Niveles de Soporte y Estrategia "Shift-Left":**
  * Potenciar el **Nivel 0 (Portal de Autoservicio / KB)** para que el usuario resuelva sus problemas (Shift-Left), filtrando el volumen que llega al Nivel 1 (CAU), Nivel 2 (Sistemas/Redes) y Nivel 3 (Fabricante).
* **5.5. KPIs y Herramientas:** Medir el FCR (First Contact Resolution, >70%), usando plataformas ITSM como GLPI (Open Source, muy común en AAPP) o ServiceNow/Jira.

## 6. Herramientas Digitales y Soporte Remoto
* **6.1. Productividad y Colaboración:** Microsoft 365, LibreOffice (formato estándar ODF/ENI), Teams/Exchange.
* **6.2. Certificados y Firma:** AutoFirma, middlewares (PKCS#11) y lectores de DNIe configurados por defecto.
* **6.3. Asistencia Remota:** Herramientas nativas (Asistencia RDP, PowerShell Remoting) o de terceros (AnyDesk/TeamViewer) para aplicar resoluciones en primer contacto sin desplazamiento físico.

## 7. Conclusión
El puesto de trabajo no se gestiona PC a PC, se gobierna. La transición desde el despliegue manual mediante imágenes maestras hacia el *Zero-Touch* con Autopilot e Intune, sumado al blindaje exigido por el ENS (LAPS, BitLocker, MFA) y el soporte estructurado bajo ITIL, garantiza que el Ayuntamiento preste un servicio digital continuo, ágil y, sobre todo, resiliente ante ciberamenazas.

--------------------------------

# Tema 43.- El puesto de trabajo TIC: normalización, políticas de seguridad, actualización y despliegue. Sistemas de administración de entorno de usuario y estaciones de trabajo. Distribución de software. Centro de Atención a los usuarios. Herramientas digitales, políticas de seguridad y configuración de sistemas. Soporte y resolución de incidencias.

## 1. Introducción

- El **puesto de trabajo TIC** es el punto de contacto entre el empleado público y los sistemas de información.
- Objetivos:
  - Productividad.
  - Seguridad de la información.
  - Calidad y continuidad del servicio.
- En organizaciones grandes se requiere **administración centralizada**, distribución de software, políticas de seguridad y **CAU**.


El puesto de trabajo TIC es el punto de contacto directo entre el empleado público y los sistemas de información de la Administración. Su correcta normalización, securización y gestión determinan la productividad de los funcionarios, la seguridad de la información y la calidad del servicio público. Un Ayuntamiento con 2.000 puestos de trabajo necesita mecanismos centralizados de administración, distribución de software, aplicación de políticas de seguridad y un Centro de Atención a Usuarios (CAU) que garantice la resolución eficaz de incidencias.

Este tema analiza la normalización del puesto de trabajo TIC, los sistemas de administración del entorno de usuario, la distribución de software, las herramientas digitales, las políticas de seguridad, la configuración de sistemas y el soporte y resolución de incidencias bajo el marco ITIL.

## 2. Normalización del Puesto de Trabajo TIC

### 2.1. Concepto

- **Normalización:** establecer un estándar corporativo común:
  - Hardware homogéneo.
  - Software estandarizado.
  - Configuración uniforme.
  - Políticas de seguridad consistentes.

### 2.2. Componentes de la normalización

- **Hardware:** modelos (PC sobremesa, portátil, thin client, monitor) y especificaciones aprobadas.
- **Sistema operativo:** versión corporativa e imagen maestra.
- **Software base:** ofimática, navegador, correo, PDF, antivirus.
- **Software departamental:** aplicaciones específicas según el puesto (gestor de expedientes, GIS, firmadoc)
- **Seguridad:** BitLocker, antivirus/EDR, firewall, restricciones USB.
- **Periféricos:** impresoras, escáneres, lectores de tarjetas homologados.

### 2.3. Imagen maestra (Golden Image)

- Imagen del SO con software y configuración corporativa.
- Se despliega mediante herramientas de depliegue **WDS, MDT, SCCM/MECM, KACE**.
- Ventajas:
  - Despliegue rápido.
  - Configuración homogénea.
  - Reinstalación sencilla ante incidencias graves.
  
 **Despliegue Moderno (Zero-Touch Provisioning)** 
 * Tendencia actual mediante **Windows Autopilot + Intune**. El equipo se entrega de fábrica al usuario; al loguearse con su cuenta corporativa, se autoconfigura sin que el departamento de Microinformática tenga que tocar el hardware físico.

## 3. Políticas de Seguridad del Puesto de Trabajo

### 3.1. Políticas de contraseñas (ENS)

- Longitud mínima recomendada: **12 caracteres**.
- Complejidad: mayúsculas, minúsculas, números y caracteres especiales.
- Caducidad: **90 días** (60 días en categoría alta).
- Historial: no reutilizar las últimas **12**.
- Bloqueo: tras **5 intentos fallidos** (desbloqueo tras 30 min).
- MFA (Doble Factor de Autenticación): *Obligatorio en Categoría Media* para **administradores de sistemas** y para cualquier **acceso remoto/externo** (ej. VPN de teletrabajo).

### 3.2. Políticas de cifrado

- **Cifrado completo del disco:** BitLocker (Windows), LUKS (Linux).
- Especialmente obligatorio en **portátiles** (riesgo de pérdida/robo).
- Cifrado de dispositivos extraíbles: **BitLocker To Go / VeraCrypt**.
- Clave de recuperación almacenada en **Active Directory**.

### 3.3. Control de dispositivos

- Restricción de **USB** no autorizados, solo se permiten cifrados.
- Deshabilitación de **Autorun/Autoplay**.
- Lista blanca de periféricos mediante ID de hardware.

### 3.4. Protección endpoint

- **Antivirus/EDR:** detección, análisis de comportamiento y respuesta frente a malware.
- **Firewall local:** reglas gestionadas mediante GPO.
- **AppLocker / WDAC:** ejecución únicamente de aplicaciones autorizadas (lista blanca).

### 3.5. Actualizaciones de seguridad

- **SO:** Windows Update gestionado mediante WSUS/SCCM.
- **Aplicaciones:** navegadores, Java, Adobe, etc.
- Despliegue preferentemente fuera del horario laboral.
- Objetivo de aplicación: **máximo 30 días** desde la publicación del parche.

## 4. Sistemas de Administración del Entorno de Usuario

### 4.1. Active Directory (AD) y GPOs

- **AD:** gestión centralizada de identidades y políticas. 
- **OU (Unidades Organizativas):** agrupan usuarios y equipos.
- **GPO:** aplican automáticamente configuraciones a usuarios y equipos.
    *   Configuración de escritorio, menú inicio, fondo de pantalla corporativo.
    *   Mapeo de unidades de red e impresoras.
    *   Restricción de instalación de software.
    *   Configuración de proxy y certificados.
    *   Redirección de carpetas (Mis Documentos → servidor de ficheros).
    *   Scripts de inicio de sesión (logon scripts).

### 4.2. Azure AD / Microsoft Entra ID

- Gestión de identidades en entornos híbridos.
- **Azure AD Connect:** sincronización AD local (on-premise) y Azure AD.
- **Single Sign-On (SSO):** acceso único a aplicaciones locales y cloud (Microsoft 365).
- **Conditional Access:** acceso condicionado por ubicación, dispositivo, riesgo, etc.
- **MFA:** autenticación mediante segundo factor.
- **LAPS** (Local Administrator Password Solution): genera una contraseña aleatoria distinta para cada PC y la guarda cifrada en el Active Directory.

### 4.3. Microsoft Intune / Endpoint Manager

- Plataforma **UEM - Unified Endpoint Management** para administrar dispositivos desde la nube.
- Gestiona:
  - PCs y portátiles.
  - Móviles y tablets.
  - Aplicaciones y actualizaciones.
  - Políticas de cumplimiento.
- Puede bloquear el acceso si el dispositivo no cumple requisitos de seguridad.
- Integración con **Conditional Access**.

## 5. Distribución de Software

### 5.1. Herramientas de distribución

- **SCCM/MECM:** software, parches, inventario, imágenes de SO e informes.
- **Microsoft Intune:** distribución de aplicaciones y políticas desde la nube.
- **WSUS:** gestión centralizada de actualizaciones Windows.
- **Ansible:** automatización de configuración y despliegue.
- **Puppet:** gestión declarativa de configuración.
- **PDQ Deploy:** distribución silenciosa en redes Windows.
* **WinGet (Windows Package Manager):** La herramienta de línea de comandos nativa de Microsoft. Su uso integrado con scripts es la tendencia actual en Microinformática para **automatizar la instalación y el parcheado ágil de aplicaciones de terceros** sin la sobrecarga operativa que exige MECM.

### 5.2. Proceso de despliegue

1. **Empaquetado:** MSI, MSIX o script.
2. **Pruebas:** grupo piloto.
3. **Aprobación:** autorización del despliegue.
4. **Distribución:** SCCM/Intune.
5. **Instalación:** silenciosa y desatendida.
6. **Verificación:** comprobación mediante informes de cumplimiento.

### 5.3. Gestión de licencias

- **Inventario de software:** control de aplicaciones instaladas (SCCM/MECM)
- **SAM (Software Asset Management):** gestión de licencias.
- Modelos:
  - Licencia perpetua.
  - Suscripción.
  - Licencia por volumen.
  - Acuerdos marco SARA.

## 6. Centro de Atención a Usuarios (CAU)

### 6.1. Concepto

- **CAU / Service Desk / Help Desk:** punto único de contacto (**SPOC — Single Point of Contact**) entre usuarios y TIC.
- Funciones:
  - Recibir incidencias y peticiones.
  - Registrar y clasificar.
  - Resolver o escalar.
  - Realizar seguimiento.

### 6.2. Marco ITIL (Information Technology Infrastructure Library)

Estándar de facto para la gestión de servicios TI:
- **Gestión de Incidencias:** restaurar el servicio rápidamente.
- **Gestión de Peticiones:** solicitudes predefinidas (alta de usuario, cambio de contraseña, instalación de software).
- **Gestión de Problemas:** Identificar y eliminar la causa raíz de incidencias recurrentes.
- **Gestión de Cambios:** controlar modificaciones de infraestructura.
- **Gestión del Conocimiento:** base de conocimiento y soluciones (FAQ, artículos KB).

### 6.3. Niveles de soporte

- **Nivel 0:** autoservicio, FAQ, base de conocimiento. Shift-Left (Portal Autoservicio), ej. que el usuario resetee su propia contraseña de AD o pida software que se instale solo.
- **Nivel 1:** incidencias básicas → CAU.
- **Nivel 2:** incidencias técnicas complejas → especialistas.
- **Nivel 3:** desarrollo, infraestructura o fabricante → expertos/proveedores.

### 6.4. Herramientas de gestión de incidencias

- **ServiceNow:** ITSM cloud.
- **Jira Service Management:** ITSM.
- **GLPI:** open source.
- **OTRS:** open source/comercial.
- **Freshdesk:** SaaS.

### 6.5. Indicadores clave (KPIs) del CAU

- **Tiempo medio de respuesta.** < 15 minutos
- **Tiempo medio de resolución.** < 4 horas (Nivel 1), < 8 horas (Nivel 2)
- **FCR:** porcentaje resuelto en primer contacto. > 70%
- **Satisfacción del usuario.** > 4/5
- **Backlog:** incidencias pendientes. Tendencia decreciente

## 7. Soporte y Resolución de Incidencias

### 7.1. Ciclo de vida de una incidencia (ITIL)

**Detección → Registro → Clasificación/Priorización → Diagnóstico → Resolución → Cierre → Revisión**

1. **Detección:** usuario o monitorización automática.
2. **Registro:** creación del ticket (fecha, usuario, descripción, categoría, urgencia)
3. **Clasificación y priorización:** según **impacto × urgencia**.
4. **Diagnóstico:** análisis de causa, logs y base de conocimiento.
5. **Resolución:** aplicación de la solución.
6. **Cierre:** documentación e información al usuario.
7. **Revisión:** incidencias recurrentes → Gestión de Problemas.

### 7.2. Herramientas de soporte remoto

- **RDP:** acceso remoto al escritorio.
- **Asistencia remota de Windows:** control con consentimiento.
- **TeamViewer / AnyDesk:** escritorio remoto multiplataforma.
- **PowerShell Remoting:** ejecución remota de comandos.
- **VNC** acceso remoto al equipo.

## 8. Herramientas Digitales del Puesto de Trabajo

### 8.1. Suite ofimática

*   **Microsoft 365:** Word, Excel, PowerPoint, Outlook, Teams, SharePoint, OneDrive.
*   **LibreOffice:** Alternativa open source (Writer, Calc, Impress, Draw).
*   **Formato estándar:** ODF (Open Document Format) — interoperabilidad conforme al ENI.

### 8.2. Herramientas de comunicación

*   **Microsoft Teams:** chat, videollamadas, canales y colaboración.
*   **Correo electrónico corporativo:** Microsoft Exchange / Exchange Online.

### 8.3. Herramientas de firma y certificados

*   **AutoFirma:** firma electrónica.
*   **Lector de tarjetas criptográficas:** DNIe y tarjetas FNMT.
*   **Drivers de certificados:** Módulos PKCS#11 para los navegadores.

### 8.4. Herramientas de seguridad

*   **VPN corporativa:** Acceso remoto seguro.
*   **Antivirus/EDR:** protección del endpoint
*   **Cifrado:** BitLocker, VeraCrypt.

## 9. Conclusión

- El puesto TIC debe estar **normalizado, securizado y administrado centralmente**.
- Elementos clave:
  - **Imagen maestra** → homogeneización.
  - **AD + GPO** → administración centralizada.
  - **Intune/SCCM** → gestión y distribución.
  - **Políticas de seguridad** → protección del puesto.
  - **CAU + ITIL** → gestión de incidencias y peticiones.
  - **Soporte remoto** → resolución eficiente.
- Objetivo final: **productividad + seguridad + continuidad del servicio público** conforme al **ENS**.
