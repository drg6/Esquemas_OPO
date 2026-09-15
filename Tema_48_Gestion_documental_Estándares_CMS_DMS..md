# Tema 48.- Gestión documental (DMS), estándares (OAIS, ENI) y Gestión de contenidos (CMS).

## 1. Introducción
* **Doble obligación legal (Leyes 39 y 40/2015):** La Administración tramita electrónicamente.
  * **DMS (Gestor Documental):** Para el "Back-Office" (tramitar expedientes, conservar, archivar).
  * **CMS (Gestor de Contenidos):** Para el "Front-Office" (Sedes electrónicas, portales de transparencia, comunicación con el ciudadano).

## 2. Gestión Documental y el ENI (Esquema Nacional de Interoperabilidad)
* **2.1. El Documento Electrónico (ENI):** No es solo un PDF. Es `Contenido + Firma Electrónica + Metadatos obligatorios (eMGDE)`.
* **2.2. El Expediente Electrónico (ENI):** Agrupación en XML de documentos electrónicos + un **Índice Electrónico firmado** (el "candado" que impide meter o sacar folios a posteriori).
* **2.3. El reto operativo (El Expediente Híbrido):** La realidad municipal obliga a integrar papel antiguo con documentos nativos digitales mediante procesos de **digitalización certificada**.

## 3. Estándares Core de la Gestión Documental
* **3.1. Normas ISO y Funcionales:**
  * **ISO 15489:** Metodología de gestión documental integral.
  * **MoReq:** Requisitos funcionales europeos (cómo debe clasificar y expurgar el software).
* **3.2. Modelo OAIS (Archivo a largo plazo - ISO 14721):**
  * **SIP (Ingesta):** El paquete original que entra al sistema.
  * **AIP (Archivo):** El SIP transformado (ej. convertido a PDF/A) para preservación eterna.
  * **DIP (Difusión):** El paquete que se le entrega al investigador o ciudadano que lo pide.
* **3.3. CMIS (La API Universal):** Estándar que permite a un gestor de expedientes de Java hablar con un repositorio documental de Microsoft sin importar el fabricante.

## 4. Tecnologías DMS (Document Management Systems)
* Funcionalidades vitales: Control de versiones, metadatos, workflows, retención y auditoría.
* **Mercado Propietario:** Microsoft SharePoint (Líder en intranets/Office 365), Documentum (Grandes cuentas), IBM FileNet.
* **Mercado Open Source:** **Alfresco** (El estándar de facto en Ayuntamientos medianos, basado en Java y CMIS).
* **Ecosistema AGE (Red SARA):** INSIDE (tramitación), ARCHIVE (archivo definitivo OAIS).

## 5. Gestión de Contenidos (CMS)
* Software que desacopla el diseño visual de los datos, permitiendo a perfiles no técnicos (periodistas, administrativos) publicar en la web mediante flujos de aprobación.
* **5.1. Arquitecturas:**
  * *Monolítica:* Backend y Frontend unidos (WordPress).
  * *Headless (Sin cabeza):* Backend puro que expulsa contenido vía API a apps móviles, tótems municipales o webs (Omnicanalidad).
* **5.2. Mercado de CMS:**
  * **WordPress (PHP):** Rey absoluto por usabilidad. ⚠️ *Riesgo en AAPP:* Su exceso de *plugins* de terceros sin auditar es un vector de entrada común para ransomware.
  * **Drupal (PHP):** Más modular y robusto. Orientado a *Security by Design*. Elegido por el PAe y portales críticos.
  * **Liferay (Java):** Gestor de portales pesados corporativos e intranets complejas.

## 6. Conclusión (El puente DMS-CMS)
El éxito de la Administración Electrónica reside en la integración de ambos mundos. El **DMS** (Alfresco/Inside) genera y custodia el expediente jurídico cumpliendo las normas ENI y OAIS. El **CMS** (Liferay/Drupal) publica la Sede Electrónica para el ciudadano. El nexo físico entre ambos mundos es el **Código Seguro de Verificación (CSV)**: permite al ciudadano introducir en el portal web (CMS) un código impreso en papel para recuperar instantáneamente la copia electrónica auténtica custodiada en el repositorio documental (DMS).

-----------

# Tema 48.- Gestión documental, estándares. Gestión de contenidos. Tecnologías CMS y DMS de alta implantación.

## 1. Introducción

- La información es un activo estratégico de las organizaciones.
- En las AAPP gestión información = obligación legal (Leyes 39/2015 y 40/2015):
  - **Gestión documental:** documentos, expedientes y conservación.
  - **Gestión de contenidos:** publicación y transparencia en la sed.
- Tecnologías principales:
  - **DMS (Document Management Systems):** gestión de documentos.
  - **CMS (Content Management Systems):** gestión y publicación de contenidos.

## 2. Gestión Documental: Conceptos Fundamentales

- Conjunto de normas, técnicas y prácticas para:
  - Crear y capturar documentos.
  - Organizarlos y recuperarlos.
  - Controlar su conservación.
  - Eliminar los que no sean necesarios.
  - Preservar los documentos de valor permanente.

### 2.1. El Ciclo de Vida del Documento

1. **Creación/Captura:** creación electrónica o digitalización.
2. **Gestión/Tramitación:** clasificación, metadatos, workflow, versionado y expediente electrónico.
3. **Almacenamiento/Recuperación:** repositorio estructurado (inalterabilidad) y búsqueda.
4. **Preservación/Archivo:** conservación a largo plazo y garantía de legibilidad.
5. **Expurgo/Destrucción:** eliminación segura sin valor juridico / administrativo.

### 2.2. El Documento y el Expediente Electrónico en la AAPP

- **Documento electrónico (ENI):**
  - Contenido.
  - Firma electrónica.
  - Metadatos obligatorios.
- **Expediente electrónico (XML):**
  - Conjunto de documentos electrónicos.
  - **Índice electrónico firmado** que garantiza su integridad.
  - Firma del órgano correspondiente.
  
* **2.3. El reto operativo (El Expediente Híbrido):** 

La realidad municipal obliga a integrar papel antiguo con documentos nativos digitales mediante procesos de **digitalización certificada**.

## 3. Estándares en la Gestión Documental

### 3.1. Principales estándares

- **ISO 15489:** gestión integral de documentos. Implementación DMS
- **ISO 30300:** sistema de gestión documental basado en mejora continua.
- **MoReq (Modelo Europeo de Requerimientos para DMS):** requisitos funcionales y no funcionales para DMS (clasificacion, control, conservación, eliminación...)
- **OAIS (Open Archival Information System- ISO 14721):** modelo de referencia para preservación y archivo digital a largo plazo.

### 3.2. OAIS

- Define un modelo para la preservación de información digital. Ciclo completo.
- Paquetes principales:
  - **SIP (Submission Information Package):** paquete enviado para su ingesta.
  - **AIP (Archival Information Package):** paquete destinado al archivo y conservación.
  - **DIP (Dissemination Information Package):** paquete generado para su difusión al usuario tras solicitud.

### 3.3. ENI

- El **Esquema Nacional de Interoperabilidad (ENI)** establece las reglas de interoperabilidad en las AAPP.
- Principales NTI:
  - **Documento y Expediente Electrónico:** estructuras, metadatos e índice.
  - **Política de Gestión de Documentos Electrónicos.**
  - **Digitalización y Copiado Auténtico.**
  - **Catálogo de Estándares:** formatos admitidos como PDF/A, ODF o XML.

### 3.4. Otros estándares

- **CMIS (Content Management Interoperability Services):** interoperabilidad entre diferentes gestores y repositorios documentales.
- **JCR (Content Repository API for Java):** acceso uniforme desde aplicaciones Java a repositorios de contenidos.
- **WebDAV:** gestión remota de documentos mediante HTTP.
- **CIFS/SMB:** acceso compartido a ficheros en red. 

## 4. Gestión de Contenidos (ECM - Enterprise Content Management y WCM - Web Content Management)

- **CMS (Content Management System):** software para:
  - Crear.
  - Editar.
  - Gestionar.
  - Publicar contenidos digitales.
- Permite:
  - Gestión de usuarios y roles.
  - Flujos de revisión y publicación.
  - Separación entre contenido y diseño.
  - Gestión de versiones.
  - Multidioma.
  - Estadísticas y analítica.
  - SSO e integración con otros sistemas.

### 4.1 Evolución de Arquitecturas CMS

- **Tradicional/Monolítica:**
  - Backend, base de datos, plugins y frontend integrados.
- **Headless:**
  - Separa frontend y backend.
  - El contenido se distribuye mediante **APIs** a múltiples canales.
  - Facilita la estrategia omnicanal (web, movil, ...).
- **Híbrida:**
  - Combina presentación tradicional con exposición de contenidos mediante APIs.

## 5. Tecnologías DMS de Alta Implantación

- Gestión y archivo de documentos.
- **Control de versiones.**
- **Auditoría de accesos.**
- **Búsqueda de texto completo.**
- Integración con herramientas ofimáticas.
- Gestión de metadatos.
- Retención y conservación.
- **CMIS (Content Management Interoperability Services)** como estándar de interoperabilidad.

### 5.1. Soluciones Privativas (Propietarias)

- **Microsoft SharePoint:**
  - Integración con Microsoft 365, Teams y OneDrive.
  - Gestión documental, intranets y workflows.
- **OpenText Documentum:**
  - Grandes organizaciones.
  - Alta escalabilidad y cumplimiento legal.
- **IBM FileNet P8:**
  - Gestión documental + BPM/workflows.
  
### 5.2. Soluciones Open Source

- **Alfresco:**
  - DMS open source de amplia implantación.
  - Java.
  - Soporte **CMIS**.
  - Metadatos y workflows.
- **Nuxeo:**
  - ECM/DMS Java.
  - Orientado a grandes volúmenes y entornos cloud.

### 5.3. Soluciones en el Ámbito de la Administración Pública

- **Espacios de Colaboración:** basado en Liferay; gestión documental y colaboración.
- **Inside:** gestión de documentos y expedientes electrónicos compatible con **CMIS**.
- **G-Inside:** servicio SaaS para generar documentos y expedientes conforme al **ENI**.
- **@Doc:** gestión de documentos y expedientes electrónicos.
- **ARCHIVE:** archivo definitivo y preservación a largo plazo.

## 6. Tecnologías CMS de Alta Implantación

### 6.1. Características y Arquitectura

- Habitualmente basada en arquitecturas **LAMP/LEMP - Linux, Apache o Engine-x (Nginx), MySQL, PHP**.
- **Backend:**
  - Administración.
  - Creación y edición de contenidos.
  - Gestión de plantillas y jerarquías.
- **Frontend:**
  - Parte pública.
  - Generación dinámica del contenido.

### 6.2. Soluciones Tradicionales Dominantes

- **WordPress:**
  - PHP.
  - Gran facilidad de uso y ecosistema de plugins.
  - Muy extendido en portales web, más vulnerable su exceso de *plugins* de terceros sin auditar es un vector de entrada común para ransomware.
- **Drupal:**
  - PHP.
  - Robusto y modular.
  - Adecuado para grandes portales institucionales, seguridad avanzada *Security by Design*.
- **Joomla:**
  - PHP.
  - Alternativa intermedia entre WordPress y Drupal.

### 6.3. Otros gestores

- **Portales:** Liferay, SharePoint.
- **Educación:** Moodle, Claroline.
- **Comercio electrónico:** Magento, PrestaShop, osCommerce.
- **Wikis:** MediaWiki, DokuWiki.
- **Foros:** phpBB.
- **Administración Pública:** ACCEDA.

## 7. Conclusión

Crecimiento exponencial información en AAPP, necesidad:
- **DMS →** gestión documental, expedientes, conservación y archivo cumpliendo las normas ENI y OAIS. Alfresco y Archive
- **CMS →** creación y publicación de contenidos web. Drupal, WordPress o Liferay

El nexo físico entre ambos mundos es el **Código Seguro de Verificación (CSV)**: permite al ciudadano introducir en el portal web (CMS) un código impreso en papel para recuperar instantáneamente la copia electrónica auténtica custodiada en el repositorio documental (DMS).

Imprescindibles modernización y transparencia en la Administración Electrónica.