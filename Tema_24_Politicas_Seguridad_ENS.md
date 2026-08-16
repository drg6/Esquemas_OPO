# Tema 24.- Diseño e implantación de políticas de seguridad. Implantación de las medidas de seguridad en Administraciones Locales. Respuesta a incidentes de seguridad. Gestión continua. Funciones de un CERT/CSIRT.

## 1. Introducción

La seguridad de la información en las Administraciones Públicas no es un producto que se adquiere, sino un **proceso continuo** que se diseña, implanta, verifica y mejora cíclicamente. Un Ayuntamiento gestiona datos especialmente sensibles (padrón municipal, tributos, servicios sociales, policía local) cuya vulneración tendría consecuencias directas sobre los derechos de los ciudadanos y la continuidad de los servicios públicos esenciales.

El Esquema Nacional de Seguridad (ENS, RD 311/2022) establece la obligación de que toda Administración Pública disponga de una **Política de Seguridad** formal, aprobada al máximo nivel, que articule un Sistema de Gestión de Seguridad de la Información (SGSI). Este tema analiza el diseño e implantación de dicha política, las medidas de seguridad específicas para Administraciones Locales, la respuesta ante incidentes y las funciones de los equipos CERT/CSIRT.

## 2. Diseño de la Política de Seguridad

### 2.1. Concepto

La **Política de Seguridad** es el documento de máximo nivel que establece las directrices, principios y compromisos de la organización en materia de seguridad de la información. No es un documento técnico de configuración de firewalls, sino un instrumento de gobierno que emana de la máxima autoridad del Ayuntamiento.

### 2.2. Contenido obligatorio según el ENS

El ENS (artículo 12 del RD 311/2022) exige que la Política de Seguridad incluya, como mínimo:

1.  **Objetivos y misión de la organización:** Cómo la seguridad de la información contribuye a los fines del Ayuntamiento.

2.  **Marco normativo vinculante:** Referencia expresa al ENS (RD 311/2022), al RGPD (Reglamento 2016/679), a la LOPDGDD (LO 3/2018) y a cualquier normativa sectorial aplicable.

3.  **Organización de la seguridad — Roles y responsabilidades:**
    *   **Responsable de la Información:** Determina los requisitos de seguridad de la información manejada (habitualmente, el responsable funcional del área).
    *   **Responsable del Servicio:** Determina los requisitos de seguridad de los servicios prestados.
    *   **Responsable de la Seguridad (CISO):** Determina las decisiones para satisfacer los requisitos de seguridad de la información y los servicios. Debe ser una persona distinta del Responsable del Sistema.
    *   **Responsable del Sistema:** Gestiona la operación del sistema de información.
    *   **Delegado de Protección de Datos (DPD/DPO):** Obligatorio para todas las Administraciones Públicas.

    > **Principio de segregación de funciones:** La persona que determina los requisitos de seguridad (CISO) no debe ser la misma que opera los sistemas (Responsable del Sistema).

4.  **Criterios de categorización de los sistemas:** Según las dimensiones del ENS (Confidencialidad, Integridad, Disponibilidad, Autenticidad, Trazabilidad).

5.  **Directrices de gestión de riesgos:** Metodología aprobada (MAGERIT), criterios de aceptación de riesgos.

6.  **Proceso de aprobación y revisión:** Periodicidad de revisión de la política.

## 3. Implantación de Medidas de Seguridad en Administraciones Locales

### 3.1. Ciclo PDCA (Plan-Do-Check-Act)

La implantación del SGSI sigue el ciclo de mejora continua de Deming (PDCA), alineado con ISO 27001:

1.  **Plan (Planificar):**
    *   Aprobar la Política de Seguridad al máximo nivel.
    *   Categorizar los sistemas de información (Básica, Media, Alta).
    *   Ejecutar el Análisis y Gestión de Riesgos (MAGERIT/PILAR — Tema 29).
    *   Elaborar la Declaración de Aplicabilidad (SoA).
    *   Definir el Plan de Adecuación al ENS.

2.  **Do (Ejecutar):**
    *   Implantar las medidas de seguridad técnicas: firewalls, segmentación de redes, cifrado de discos, autenticación multifactor (MFA), copias de seguridad automatizadas (RMAN).
    *   Implantar las medidas organizativas: formación y concienciación del personal, clasificación de la información, gestión de accesos.
    *   Implantar las medidas de protección: control de acceso físico, gestión de soportes extraíbles.

3.  **Check (Verificar):**
    *   Auditorías internas y externas (Tema 30).
    *   Monitorización continua de la seguridad (SIEM, análisis de logs).
    *   Pruebas de penetración (pentesting) periódicas.

4.  **Act (Mejorar):**
    *   Análisis de los hallazgos de las auditorías.
    *   Implementación de acciones correctivas.
    *   Actualización de la Política y las medidas de seguridad.

### 3.2. Medidas de seguridad del ENS por marcos

El ENS organiza las medidas de seguridad en tres marcos:

*   **Marco Organizativo (org):** Política de seguridad, normativa de seguridad, procedimientos, proceso de autorización.
*   **Marco Operacional (op):** Planificación, control de acceso, explotación, servicios externos, continuidad del servicio, monitorización.
*   **Marco de Protección (mp):** Protección de instalaciones, gestión del personal, protección de equipos, protección de comunicaciones, protección de soportes de información, protección de aplicaciones, protección de la información, protección de servicios.

### 3.3. Particularidades de las Administraciones Locales

Los Ayuntamientos presentan características específicas que condicionan la implantación:
*   **Recursos limitados:** Presupuestos y personal TIC reducidos, especialmente en municipios pequeños.
*   **Dependencia de proveedores externos:** Muchos servicios TIC están externalizados.
*   **Heterogeneidad tecnológica:** Convivencia de sistemas modernos con aplicaciones legacy.
*   **Solución:** Las Diputaciones Provinciales frecuentemente proporcionan servicios de seguridad compartidos a los municipios de su provincia, y el CCN ofrece herramientas y guías específicas para Administración Local.

## 4. Respuesta a Incidentes de Seguridad

### 4.1. Concepto de incidente de seguridad

Un **incidente de seguridad** es cualquier evento que comprometa la confidencialidad, integridad, disponibilidad, autenticidad o trazabilidad de la información o los servicios. Ejemplos: infección por ransomware, acceso no autorizado a la base de datos del padrón, exfiltración de datos personales, caída del servicio de Sede Electrónica.

### 4.2. Plan de Respuesta a Incidentes

El ENS exige disponer de un Plan de Respuesta a Incidentes que defina:

1.  **Detección e identificación:** Sistemas de detección (SIEM, IDS/IPS, antivirus, monitorización de logs) y criterios de clasificación de la severidad del incidente.

2.  **Contención:** Acciones inmediatas para limitar el alcance del incidente: aislamiento de sistemas afectados, desconexión de la red, bloqueo de cuentas comprometidas.

3.  **Erradicación:** Eliminación de la causa raíz del incidente: desinfección de malware, parcheado de vulnerabilidades explotadas, restablecimiento de configuraciones.

4.  **Recuperación:** Restauración de sistemas y servicios desde copias de seguridad verificadas. Verificación de la integridad de los datos restaurados.

5.  **Notificación obligatoria:**
    *   **Al CCN-CERT:** Todo incidente significativo en el sector público debe notificarse al CCN-CERT en un plazo máximo de **72 horas**.
    *   **A la AEPD:** Si el incidente implica una brecha de datos personales, debe notificarse a la Agencia Española de Protección de Datos en un plazo máximo de **72 horas** (artículo 33 del RGPD).
    *   **A los afectados:** Si la brecha puede suponer un alto riesgo para los derechos de los ciudadanos, deben ser notificados sin dilación (artículo 34 del RGPD).

6.  **Lecciones aprendidas:** Análisis post-incidente para identificar las causas raíz, evaluar la eficacia de la respuesta y actualizar los procedimientos y medidas de seguridad.

## 5. Gestión Continua de la Seguridad

La seguridad es un proceso vivo, no un estado estático. La gestión continua incluye:

*   **Monitorización continua:** Análisis de logs, SIEM (Security Information and Event Management), alertas automatizadas.
*   **Gestión de vulnerabilidades:** Escaneos periódicos de vulnerabilidades, aplicación de parches, gestión del ciclo de vida del software.
*   **Concienciación del personal:** Programas de formación periódica, simulacros de phishing, campañas de sensibilización.
*   **Revisión periódica:** La Política de Seguridad debe revisarse al menos anualmente y actualizarse ante cambios significativos.
*   **Indicadores de seguridad:** Métricas como el tiempo medio de detección (MTTD), el tiempo medio de respuesta (MTTR), el número de incidentes por categoría y la tasa de parcheado.

## 6. CERT/CSIRT: Equipos de Respuesta ante Emergencias

### 6.1. Concepto

Un **CERT (Computer Emergency Response Team)** o **CSIRT (Computer Security Incident Response Team)** es un equipo especializado en la prevención, detección, gestión y respuesta coordinada ante incidentes de ciberseguridad.

### 6.2. Funciones principales

*   **Alerta temprana (Early Warning):** Emisión de avisos sobre nuevas vulnerabilidades, parches críticos y campañas de ciberataques dirigidas contra el sector público.
*   **Gestión y coordinación de incidentes:** Apoyo técnico durante la respuesta a incidentes graves: análisis forense digital, contención, erradicación.
*   **Análisis de vulnerabilidades:** Evaluación proactiva de la seguridad de los sistemas.
*   **Formación y concienciación:** Capacitación de los equipos TIC de las administraciones.
*   **Inteligencia de amenazas:** Recopilación y análisis de información sobre amenazas, atacantes y técnicas empleadas.

### 6.3. CERTs relevantes en España

| CERT | Ámbito | Dependencia |
|------|--------|------------|
| **CCN-CERT** | Sector público (AGE, CCAA, EELL) | Centro Criptológico Nacional (Ministerio de Defensa) |
| **INCIBE-CERT** | Sector privado, ciudadanos, infraestructuras críticas | Instituto Nacional de Ciberseguridad (Ministerio de Transformación Digital) |
| **ESP-DEF-CERT** | Sistemas del Ministerio de Defensa | Mando Conjunto del Ciberespacio |

### 6.4. Aplicación en Administraciones Locales

Los Ayuntamientos, dada la limitación de sus recursos, no suelen disponer de un CERT propio. En su lugar:
*   Se adscriben al **CCN-CERT**, que les proporciona herramientas (serie CCN-STIC), alertas y apoyo ante incidentes graves.
*   Utilizan herramientas proporcionadas gratuitamente por el CCN: **LUCÍA** (gestión de incidentes), **ANA** (análisis de vulnerabilidades), **CARMEN** (detección de APT), **microCLAUDIA** (protección contra ransomware).
*   Las Diputaciones Provinciales pueden actuar como CERT delegado para los municipios de su provincia.

## 7. Conclusión

El diseño e implantación de políticas de seguridad en las Administraciones Locales constituye un mandato legal del ENS que debe abordarse como un proceso continuo (ciclo PDCA), no como un proyecto puntual. La Política de Seguridad, aprobada al máximo nivel y articulada en torno a roles claramente segregados, fundamenta un SGSI que abarca medidas organizativas, operacionales y de protección.

La gestión de incidentes, con sus fases de detección, contención, erradicación, recuperación y notificación obligatoria (al CCN-CERT y a la AEPD), y el apoyo de los equipos CERT/CSIRT —especialmente el CCN-CERT para el sector público— completan el ecosistema de ciberseguridad que protege los sistemas de información municipales y, con ellos, los datos y derechos de los ciudadanos.
