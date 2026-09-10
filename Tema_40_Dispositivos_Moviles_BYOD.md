# Tema 40.- Puesto de trabajo, Conectividad, Entorno de usuario (AD/GPO), Distribución de software y BYOD.

## 1. Introducción
* **Contexto:** El puesto de trabajo público ha evolucionado del PC fijo aislado a un entorno híbrido (teletrabajo, movilidad).
* **El reto TIC:** Proporcionar herramientas ágiles al funcionario sin comprometer el cumplimiento del ENS y el RGPD.

## 2. El Puesto de Trabajo Digital
* **2.1. Estaciones (Hardware):**
  * *Desktop:* Puestos fijos (Atención al ciudadano).
  * *Laptop:* Teletrabajo. Exige cifrado de disco (BitLocker).
  * *Thin Client:* Terminal tonto conectado a VDI. Máxima seguridad y bajo consumo.
* **2.2. Dispositivos Móviles:** Smartphones (correo/apps) y Tablets (inspecciones a pie de calle).
* **2.3. Conectividad:** LAN (Gigabit), WLAN (WPA3-Enterprise con 802.1X), y VPN para acceso remoto.

## 3. Administración del Entorno de Usuario (El Cerebro)
* **3.1. Active Directory (AD On-Premise):**
  * Centraliza identidades (usuarios/UPN) y estructura organizativa en **OUs (Unidades Organizativas)** por concejalías. La mejor práctica es basándose exclusivamente en los **Usuarios** (departamentos/concejalías) y no en los equipos. Esto desacopla el ciclo de vida del hardware de la identidad del funcionario.
  * **GPO (Group Policy Objects):** Automatizan políticas de seguridad. 
  * *Ejemplo Táctico:* Una GPO bloquea los puertos USB en toda la red, pero aplica una excepción a la OU "Policía Local" para descargar cámaras corporales.
* **3.2. Microsoft Entra ID (El salto al Cloud):**
  * Extensión de AD a la nube para entornos híbridos.
  * Proporciona **SSO (Single Sign-On)** y **MFA (Doble factor)**, obligatorio por el ENS para accesos externos.

## 4. Distribución de Software y Parches
* **4.1. Necesidad:** Gestionar manualmente el ciclo de vida del software en 2.000 PCs es inviable.
* **4.2. Herramientas (UEM / Config Management):**
  * **MECM (antiguo SCCM):** Despliegue de imágenes SO y software pesado.
  * **Intune:** Gestión moderna basada en nube.
  * **WSUS:** Centralización de parches Windows.
  * **Ansible:** Automatización de configuraciones e infraestructura.
  * **WinGet (Windows Package Manager):** La herramienta de línea de comandos nativa de Microsoft. Su uso integrado con scripts es la tendencia actual en Microinformática para **automatizar la instalación y el parcheado ágil de aplicaciones de terceros** sin la sobrecarga operativa que exige MECM.
* **4.3. Proceso de Parcheado (El Ciclo):**
  1. *Evaluación:* Priorización de CVEs guiada por las alertas del **CCN-CERT**.
  2. *Piloto:* Pruebas en grupo reducido.
  3. *Aprobación & Despliegue:* Instalación masiva.
  4. *Verificación.*

## 5. BYOD (Bring Your Own Device) y Movilidad
* **5.1. Concepto y Riesgos:** Usar el móvil personal del empleado para trabajar. Riesgo altísimo de fuga de datos (Padrón/Tributos) o malware.
* **5.2. La Solución (MDM - Mobile Device Management):**
  * Herramientas como **Intune** o **Workspace ONE**.
  * **Contenedorización:** Aísla la parte corporativa de la personal (fotos).
  * **Remote Wipe (Borrado selectivo):** Si roban el móvil, el TIC borra el contenedor corporativo sin tocar lo personal.
* **5.3. Modelos de Movilidad:**
  * **BYOD:** Dispositivo personal (Control parcial).
  * **COPE:** Dispositivo corporativo pero con uso personal (Control total).
  * **COBO:** 100% corporativo (Solo uso laboral).
* **5.4. BYOD en el Teletrabajo (VDI):** 
  * La mejor forma de hacer BYOD seguro desde el PC personal de casa es mediante **Escritorios Virtuales (VDI)**. Los datos del ciudadano se procesan en el CPD municipal y nunca viajan al disco duro doméstico del empleado.

## 6. Conclusión
El puesto de trabajo moderno requiere un equilibrio absoluto entre productividad y seguridad. Mientras que **Active Directory y MECM** aseguran el cumplimiento del parque informático interno, soluciones como **Intune (MDM)** y tecnologías **VDI** garantizan que el perímetro del Ayuntamiento se extienda de forma segura al dispositivo móvil o al domicilio del empleado, dando pleno cumplimiento al ENS y al RGPD.

------

# Tema 40.- Dispositivos personales de PC y dispositivos móviles. La conectividad de los dispositivos personales. Sistemas de administración de entorno de usuario y estaciones de trabajo. Distribución de software. Gestión de dispositivos externos (BYOD).

## 1. Introducción

- Evolución del puesto de trabajo → **PC, portátil, tablet y smartphone**.
- Diferentes ubicaciones: oficina, domicilio, movilidad
- Retos: **gestión, seguridad y soporte**.

## 2. El Puesto de Trabajo Digital

### 2.1. Estaciones de trabajo (PC)

- **Desktop** → fijo, rendimiento y durabilidad. Presencial
- **Laptop** → movilidad; requiere **cifrado + VPN**.
- **Thin Client** → conecta con **VDI**; gestión centralizada y sin datos locales. Menor consumo y coste.
- **All-in-One** → PC integrado en monitor; ahorro de espacio.

### 2.2. Dispositivos móviles

*   **Smartphones:** correo, apps y firma.
*   **Tablets:** Levantamiento de actas de inspección, consulta de expedientes en movilidad.
*   **Wearables:** Casos específicos (policía local con cámaras corporales).

### 2.3. Conectividad

*   **Red corporativa LAN/WLAN:** Ethernet (Gigabit), Wi-Fi (WPA3-Enterprise con 802.1X/RADIUS).
*   **VPN:** Acceso remoto seguro.
*   **Redes móviles (4G/5G):** Conectividad en movilidad, con tarjeta SIM corporativa o APN privada.
*   **Bluetooth:** Para periféricos (auriculares, ratones). Riesgo de seguridad si no se controla.

## 3. Sistemas de Administración del Entorno de Usuario

### 3.1. Active Directory (AD)

- Centraliza **identidades y accesos**.

*   **Usuarios y grupos:** cuenta de usuario con su nombre principal (UPN).
*   **Unidades Organizativas (OU):** Estructura jerárquica para organizar usuarios por departamento. La mejor práctica es basándose exclusivamente en los **Usuarios** (departamentos/concejalías) y no en los equipos. Esto desacopla el ciclo de vida del hardware de la identidad del funcionario.
*   **GPO (Group Policy Objects):** Controlan la configuración de los equipos y las restricciones de usuarios centralizada:
    *   Política de contraseñas (longitud mínima, complejidad, caducidad).
    *   Restricciones de software (impedir la instalación de aplicaciones no autorizadas).
    *   Configuración de seguridad (bloqueo de puertos USB, cifrado BitLocker obligatorio).
    *   Mapeo de unidades de red e impresoras.
    *   Configuración del fondo de pantalla y menú de inicio.
* *Ejemplo:* Una GPO bloquea los puertos USB en toda la red, pero aplica una excepción a la OU "Policía Local" para descargar cámaras corporales.

### 3.2. Azure Active Directory (Entra ID)

Extensión cloud de Active Directory que permite la gestión de identidades en entornos híbridos (on-premise + cloud):
*   Single Sign-On (SSO) para aplicaciones SaaS (Microsoft 365, aplicaciones web).
*   Autenticación multifactor (MFA).
*   Conditional Access: políticas de acceso basadas en ubicación, dispositivo, riesgo.

## 4. Distribución de Software

### 4.1. El problema

- Automatizar **instalación, actualización y parcheado** a gran escala.
- Aplicaciones + parches + configuraciones.

### 4.2. Herramientas de distribución

- **Microsoft SCCM/MECM** → software, parches, inventario e imágenes SO.
- **MicrosoftIntune** → MDM/MAM, aplicaciones y políticas de cumplimiento.
- **WSUS** → actualizaciones Windows.
- **Ansible** → automatización y despliegue.
- **Puppet/Chef** → gestión declarativa de configuración.
* **WinGet (Windows Package Manager):** La herramienta de línea de comandos nativa de Microsoft. Su uso integrado con scripts es la tendencia actual en Microinformática para **automatizar la instalación y el parcheado ágil de aplicaciones de terceros** sin la sobrecarga operativa que exige MECM.

### 4.3. Proceso de parcheado

1. **Evaluación** → identificar parches críticos/CVE (Common Vulnerabilities and Exposures) guiada por las alertas del **CCN-CERT**.
2. **Prueba** → grupo piloto.
3. **Aprobación**.
4. **Despliegue** → resto de equipos.
5. **Verificación** → confirmar instalación e monitorizar incidencias.

## 5. BYOD (Bring Your Own Device)

### 5.1. Concepto

**BYOD** → empleado utiliza su **dispositivo personal** para acceder a recursos corporativos.

### 5.2. Riesgos

*   **Fuga de datos:** 
*   **Malware:** 
*   **Pérdida o robo:** Un dispositivo perdido con acceso al correo corporativo es una brecha de seguridad.
*   **Incumplimiento RGPD/ENS:** Datos del padrón o tributos en un dispositivo personal.

### 5.3. MDM (Mobile Device Management)

- Gestión centralizada y remota de dispositivos.

**Funcionalidades:**
*   **Contenedorización:** separa datos personales y corporativos.
*   **Cifrado obligatorio:**
*   **Políticas de seguridad:** PIN, biometría, bloqueo, etc.
*   **Borrado remoto selectivo (Remote Wipe):** borra solo los datos corporativos si pierde dispositivo
*   **Control de aplicaciones:** Lista blanca/negra de aplicaciones permitidas

### 5.4. Soluciones MDM/UEM

- **Microsoft Intune**
- **VMware Workspace ONE**
- **MobileIron (Ivanti)**
- **Samsung Knox**
- **Apple Business Manager + MDM**

### 5.5. Modelos alternativos al BYOD

- **BYOD** → dispositivo del empleado → control parcial.
- **COPE** → dispositivo corporativo + uso personal limitado.
- **COBO** → corporativo + uso exclusivamente laboral.
- **CYOD** → corporativo; empleado elige entre modelos autorizados.

* **5.6. BYOD en el Teletrabajo (VDI):** 
  * La mejor forma de hacer BYOD seguro desde el PC personal de casa es mediante **Escritorios Virtuales (VDI)**. 
  * Los datos del ciudadano se procesan en el CPD municipal y nunca viajan al disco duro doméstico del empleado.


## 6. Conclusión

- **AD/GPO** → administra usuarios y puestos.
- **SCCM/Intune/Ansible** → distribuye software y configuraciones.
- **MDM** → gestiona dispositivos móviles.
- **BYOD + contenedorización** → permite movilidad manteniendo separados los datos personales y corporativos

Cumpliendo los requisitos del ENS y del RGPD.