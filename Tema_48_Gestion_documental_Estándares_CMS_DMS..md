# Tema 48.- Gestión documental, estándares. Gestión de contenidos. Tecnologías CMS y DMS de alta implantación.

## 1. Introducción: Contexto y Obligación Legal
- **El Activo Estratégico:** En las AAPP, el mayor volumen de conocimiento es **información no estructurada** (documentos de texto, imágenes, contenido web), que supera ampliamente a los datos relacionales tradicionales.
- **La Obligación Legal (Leyes 39 y 40/2015):** La exigencia de una tramitación íntegramente electrónica obliga a gobernar esta información mediante dos motores complementarios:
  * **DMS (Gestor Documental):** La maquinaria del "Back-Office". Centrado en el valor jurídico, la gestión del expediente, la trazabilidad y el archivo definitivo.
  * **CMS (Gestor de Contenidos):** El escaparate del "Front-Office". Centrado en la publicación ágil, la transparencia ciudadana y el diseño de la Sede Electrónica.

## 2. Gestión documental, estándares
- **2.1. Concepto y Ciclo de Vida:**
  * *Definición:* Normas y prácticas para administrar el flujo de información, garantizando su autenticidad e integridad. 
  * *Las 5 Fases:* 1. Creación/Captura (digitalización certificada) -> 2. Tramitación (workflows) -> 3. Almacenamiento -> 4. Archivo (preservación) -> 5. Expurgo (destrucción legal dictaminada).
- **2.2. El Núcleo de la AAPP: Documento y Expediente Electrónico:**
  * *El Documento Electrónico (ENI):* No es un simple PDF. Es información estructurada formada obligatoriamente por 3 piezas: `Contenido + Firma Electrónica + Metadatos (esquema eMGDE)`. Para mantener su validez legal al imprimirse en papel, incorpora el **CSV (Código Seguro de Verificación)**.
  * *El Expediente Electrónico (ENI):* Objeto digital complejo (XML) que agrupa documentos. Su pieza vital es el **Índice Electrónico firmado** (el "candado" que garantiza la integridad, impidiendo añadir o extraer folios a posteriori).
- **2.3. El Marco Normativo (Las NTI del ENI):**
  * Toda esta gestión se rige por las Normas Técnicas de Interoperabilidad: NTI de Documento, NTI de Expediente, NTI de Digitalización Certificada (para el papel antiguo) y la NTI de Política de Gestión de Documentos (PGDE).
- **2.4. Estándares Core:**
  * *Organizativos:* **ISO 15489** (metodología integral) e **ISO 30300** (mejora continua).
  * *Funcionales (DMS y Archivo):* **MoReq** (requisitos europeos del software, cómo debe clasificar y expurgar el software) y **OAIS / ISO 14721** (archivo a largo plazo mediante paquetes: *SIP ingesta, AIP preservación, DIP difusión*).
  * *Técnicos (IT):* **CMIS** (API universal multiproveedor), **JCR** (estándar para Java) y **WebDAV** (edición y versionado remoto vía HTTP).

## 3. Gestión de contenidos
- **3.1. Concepto:** Software que desacopla el diseño visual de los datos, permitiendo a perfiles no técnicos (periodistas, administrativos) publicar mediante flujos de aprobación.
- **3.2. Arquitecturas:**
  * *Monolítica:* Backend y Frontend acoplados (el CMS pinta la web entera).
  * *Headless (Sin cabeza):* Backend puro que expulsa contenido vía API. Fundamental para la **omnicanalidad** (mandar el mismo dato a la Sede web, a la App móvil y a tótems del Ayuntamiento).

## 4. Tecnologías CMS y DMS de alta implantación
- **4.1. Tecnologías DMS (Gestión Documental):**
  * *Mercado Open Source:* **Alfresco** (Estándar de facto en Ayuntamientos medianos, basado en Java y CMIS).
  * *Mercado Propietario:* Microsoft SharePoint (líder en Office 365), Documentum.
  * *Ecosistema AGE (Red SARA):* INSIDE (tramitación viva) y ARCHIVE (archivo definitivo OAIS).
- **4.2. Tecnologías CMS (Gestión de Contenidos):**
  * **WordPress (PHP):** Rey por usabilidad (intranets ligeras). ⚠️ *Riesgo AAPP:* Su abuso de *plugins* de terceros sin auditar es un vector crítico de *ransomware*.
  * **Drupal (PHP):** Modular y robusto. Orientado a *Security by Design*. Es el estándar del PAe (Portal de Administración Electrónica).
  * **Liferay (Java):** Gestor pesado para portales corporativos e intranets complejas.

## 5. Conclusión (El puente DMS-CMS)
El éxito de la Administración Electrónica reside en la perfecta integración de estas herramientas. El **DMS** (ej. Alfresco) genera el expediente jurídico cumpliendo las normas ENI y OAIS, mientras que el **CMS** (ej. Liferay) publica la Sede Electrónica. El nexo físico entre ambos es el **CSV (Código Seguro de Verificación)**, que permite al ciudadano introducir en el CMS web un código impreso para recuperar instantáneamente la evidencia electrónica custodiada en el DMS.

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