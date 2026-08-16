# Tema 32.- Sistemas de gestión documental y de contenidos. Sistemas CMS: definición y conceptos. Generadores de formularios. Búsqueda de información: robots, spiders, otros. Posicionamiento y buscadores (SEO). Herramientas de trabajo colaborativo.

## 1. Introducción

Las Administraciones Públicas generan y publican diariamente grandes cantidades de contenido digital: noticias, bandos, convocatorias, actas, normativas, ofertas de empleo, información turística y trámites. Gestionar esta producción de contenido requiere herramientas que permitan a personal no técnico publicar y mantener información web sin necesidad de modificar código HTML. Paralelamente, la información publicada debe ser localizable tanto por los ciudadanos como por los motores de búsqueda.

Este tema analiza los Sistemas de Gestión de Contenidos (CMS), los generadores de formularios, los mecanismos de indexación web (robots y spiders), las técnicas de posicionamiento en buscadores (SEO) y las herramientas de trabajo colaborativo.

## 2. Sistemas de Gestión Documental

### 2.1. Concepto

Un **Sistema de Gestión Documental (SGD o DMS — Document Management System)** es una plataforma de software que permite capturar, almacenar, organizar, versionar, buscar y recuperar documentos electrónicos de forma centralizada y controlada.

### 2.2. Funcionalidades principales

*   **Captura e ingesta:** Digitalización de documentos en papel (escáner con OCR), importación de ficheros electrónicos.
*   **Almacenamiento centralizado:** Repositorio único con estructura jerárquica de carpetas o clasificación por metadatos.
*   **Metadatos:** Cada documento se enriquece con metadatos (autor, fecha, tipo documental, estado, expediente asociado) que permiten su clasificación y búsqueda.
*   **Control de versiones:** Registro de todas las modificaciones, con posibilidad de recuperar versiones anteriores.
*   **Control de acceso:** Permisos granulares por usuario, rol o grupo (lectura, escritura, eliminación).
*   **Flujos de trabajo (Workflows):** Circuitos de revisión y aprobación documental (un informe pasa de borrador → revisión → aprobación → publicación).
*   **Búsqueda avanzada:** Búsqueda por metadatos y búsqueda de texto completo (full-text search) dentro del contenido de los documentos.
*   **Retención y eliminación:** Políticas de conservación y expurgo de documentos conforme a la normativa archivística.

### 2.3. Soluciones de gestión documental

| Solución | Tipo | Características |
|----------|------|----------------|
| Alfresco | Open source / Comercial | ECM completo, integración con CMIS, APIs REST |
| OpenText | Comercial | Líder en ECM empresarial |
| Microsoft SharePoint | Comercial | Integración con Microsoft 365, colaboración |
| Nuxeo | Open source | API-first, arquitectura moderna |

## 3. Sistemas CMS (Content Management System)

### 3.1. Concepto

Un **CMS (Sistema de Gestión de Contenidos)** es una plataforma de software que permite crear, editar, organizar y publicar contenido web sin necesidad de conocimientos de programación. Separa el contenido de la presentación, permitiendo que personal no técnico (periodistas, funcionarios de comunicación) gestione la información publicada en el portal web.

### 3.2. Arquitectura del CMS

Un CMS se estructura en dos interfaces:

*   **Frontend (sitio público):** Lo que visualiza el ciudadano en su navegador: páginas, noticias, menús, formularios.
*   **Backend (panel de administración):** Interfaz privada para los editores y administradores, donde se crean y publican los contenidos mediante un editor WYSIWYG (What You See Is What You Get), sin necesidad de escribir código HTML.

### 3.3. Principales CMS

| CMS | Tipo | Características |
|-----|------|----------------|
| **WordPress** | Open source (PHP/MySQL) | El CMS más utilizado del mundo (~43% de la web). Extensible mediante plugins y temas. |
| **Drupal** | Open source (PHP) | Mayor robustez y flexibilidad que WordPress. Usado por gobiernos y grandes organizaciones. |
| **Joomla!** | Open source (PHP) | Intermedio entre WordPress y Drupal en complejidad. |
| **Liferay** | Open source (Java) | Portal empresarial con gestión de roles, workflows y personalización avanzada. |

### 3.4. Headless CMS

Los **Headless CMS** separan completamente el backend (gestión de contenido) del frontend (presentación). El contenido se expone a través de APIs REST o GraphQL, permitiendo que múltiples frontends (web, app móvil, kioscos) consuman el mismo contenido. Ejemplos: Strapi, Contentful, Sanity.

## 4. Generadores de Formularios

### 4.1. Concepto

Los **generadores de formularios** son herramientas integradas en los CMS o independientes que permiten crear formularios web dinámicos sin programación: buzones de quejas y sugerencias, solicitudes de cita previa, encuestas de satisfacción.

### 4.2. Funcionalidades

*   Diseño visual drag-and-drop de campos (texto, fecha, desplegable, adjuntos, firma).
*   Validación de datos en cliente y servidor.
*   Protección antispam (CAPTCHA, reCAPTCHA, honeypot).
*   Envío de datos por correo electrónico o almacenamiento en base de datos.
*   Integración con sistemas de tramitación (generación automática de asientos registrales).
*   Accesibilidad conforme a WCAG 2.1.

### 4.3. Soluciones

*   **Plugins de CMS:** Gravity Forms, Contact Form 7 (WordPress), Webform (Drupal).
*   **Herramientas independientes:** JotForm, Typeform, Microsoft Forms.
*   **Plataformas de administración electrónica:** Los gestores de expedientes (SIGM, Gestiona) incluyen sus propios generadores de formularios de tramitación.

## 5. Búsqueda de Información: Robots, Spiders y Crawlers

### 5.1. Concepto

Los **web crawlers** (también llamados spiders, robots o bots) son programas automatizados que recorren sistemáticamente la World Wide Web para indexar su contenido. Son la base del funcionamiento de los motores de búsqueda (Google, Bing).

### 5.2. Funcionamiento

1.  **Descubrimiento (Crawling):** El crawler parte de una lista de URLs conocidas (seeds). Accede a cada página, descarga su contenido HTML y extrae todos los enlaces (hipervínculos) que contiene.
2.  **Seguimiento de enlaces:** Visita recursivamente cada enlace descubierto, ampliando progresivamente el mapa de páginas conocidas.
3.  **Indexación:** El contenido descargado (texto, metadatos, estructura) se procesa y almacena en el índice del motor de búsqueda: una base de datos masiva y optimizada para consultas rápidas.
4.  **Ranking:** Cuando un usuario realiza una búsqueda, el motor consulta su índice y ordena los resultados según algoritmos de relevancia (PageRank de Google, entre otros).

### 5.3. Control del crawling

Los administradores web pueden controlar el comportamiento de los crawlers:
*   **robots.txt:** Fichero en la raíz del sitio web que indica qué rutas pueden o no pueden rastrear los bots.
*   **Meta robots:** Etiquetas HTML que controlan la indexación a nivel de página (`noindex`, `nofollow`).
*   **Sitemap XML:** Fichero que lista todas las URLs del sitio, facilitando al crawler el descubrimiento de páginas.

## 6. SEO (Search Engine Optimization)

### 6.1. Concepto

El **SEO (Optimización para Motores de Búsqueda)** es el conjunto de técnicas y estrategias orientadas a mejorar la posición (ranking) de un sitio web en los resultados orgánicos (no de pago) de los motores de búsqueda.

### 6.2. SEO On-Page (factores internos)

Optimización del propio sitio web:

*   **Estructura de URLs:** URLs descriptivas y legibles (`/tramites/licencia-obra` vs. `/page?id=4523`).
*   **Etiquetas de título (`<title>`):** Título descriptivo y único para cada página.
*   **Meta description:** Resumen atractivo del contenido de la página (se muestra en los resultados de búsqueda).
*   **Encabezados (`<h1>` a `<h6>`):** Jerarquía semántica del contenido, con un único `<h1>` por página.
*   **Contenido de calidad:** Texto original, relevante y actualizado.
*   **Rendimiento:** Velocidad de carga (Core Web Vitals de Google), optimización de imágenes, uso de CDN.
*   **Accesibilidad:** Atributos `alt` en imágenes, etiquetas semánticas HTML5, WCAG 2.1.
*   **Mobile-first:** Diseño responsive, adaptado a dispositivos móviles.
*   **Datos estructurados:** Schema.org (JSON-LD) para enriquecer los resultados de búsqueda.

### 6.3. SEO Off-Page (factores externos)

Reputación y autoridad del sitio web fuera de él:
*   **Backlinks:** Enlaces desde otros sitios web de calidad que apuntan al sitio municipal. A mayor número de enlaces desde sitios de autoridad (ministerios, universidades), mayor relevancia.
*   **Presencia en directorios oficiales:** Registro en el Punto de Acceso General, directorios de administraciones.
*   **Social signals:** Menciones y enlaces desde redes sociales institucionales.

### 6.4. SEO Técnico

*   Sitemap XML correctamente configurado.
*   robots.txt que no bloquee contenido importante.
*   Certificado SSL/TLS (HTTPS).
*   Canonicalización de URLs (evitar contenido duplicado).
*   Internacionalización (hreflang para sitios multilingües).

## 7. Herramientas de Trabajo Colaborativo

### 7.1. Concepto

Las **herramientas de trabajo colaborativo** son plataformas que permiten a equipos de trabajo compartir documentos, comunicarse en tiempo real, gestionar tareas y coordinar proyectos de forma remota.

### 7.2. Categorías y soluciones

| Categoría | Soluciones | Funcionalidad |
|-----------|-----------|---------------|
| **Suites ofimáticas colaborativas** | Microsoft 365, Google Workspace | Edición simultánea de documentos, hojas de cálculo y presentaciones |
| **Gestión documental colaborativa** | SharePoint, Alfresco, Nextcloud | Almacenamiento compartido, versionado, flujos de trabajo |
| **Mensajería y comunicación** | Microsoft Teams, Slack | Chat, videollamadas, canales temáticos |
| **Gestión de proyectos** | Jira, Trello, Planner | Tableros Kanban, asignación de tareas, seguimiento |
| **Wikis y bases de conocimiento** | Confluence, MediaWiki | Documentación colaborativa, FAQ internas |
| **Videoconferencia** | Microsoft Teams, Zoom, Jitsi (open source) | Reuniones virtuales, webinars |

### 7.3. Consideraciones para las AAPP

*   **Soberanía de datos:** Preferencia por soluciones que almacenen los datos en territorio de la UE o en infraestructura propia.
*   **Cumplimiento normativo:** RGPD, LOPDGDD, ENS.
*   **Accesibilidad:** Las herramientas deben cumplir WCAG 2.1 nivel AA.
*   **Interoperabilidad:** Formatos abiertos (ODF) y estándares de integración.

## 8. Conclusión

Los sistemas de gestión documental y de contenidos (CMS) permiten a las Administraciones Públicas publicar y mantener sus portales web de forma eficiente, separando la gestión del contenido de su presentación técnica. Los generadores de formularios facilitan la interacción con el ciudadano. Los crawlers y las técnicas SEO garantizan que la información publicada sea localizable por los motores de búsqueda. Y las herramientas de trabajo colaborativo proporcionan el entorno necesario para la coordinación de equipos en un contexto de creciente teletrabajo y digitalización de la Administración.
