# Tema 32.- Gestión documental y de contenidos (CMS). Formularios. Búsqueda (Crawlers) y SEO. Trabajo colaborativo.

## 1. Introducción
* **Contexto:** Las AAPP generan un volumen masivo de información (bandos, trámites, actas). 
* **Necesidad:** Herramientas para que personal no técnico (ej. prensa, funcionarios) publique información localizable por el ciudadano (SEO) y colabore internamente.
* **El ciclo de la información:** *Se gestiona (DMS) → Se publica (CMS) → Se interactúa (Formularios) → Se posiciona (SEO) → Se trabaja en equipo (Colaboración).*

## 2. Sistemas de Gestión Documental (SGD / DMS)
* **2.1. Concepto:** Plataformas para el ciclo de vida del documento vivo. (No confundir con el Archivo definitivo a largo plazo exigido por el ENI).
* **2.2. Funcionalidades (Mnemotecnia CABORV):** 
  * **C**aptura (Digitalización/OCR).
  * **A**lmacenamiento central.
  * **B**úsqueda (Full-text search y metadatos).
  * **O**rganización (Expedientes).
  * **R**ecuperación (Control de Versiones y retención).
  * **V**alidación (Workflows de aprobación).
* **2.3. Soluciones:** Alfresco y Nuxeo (Open Source/API-First), OpenText, SharePoint.
* **2.4. DMS vs. Archivo Electrónico Único (El salto al ENI):**
  * El **DMS** (Ej: Alfresco) gestiona el expediente mientras está abierto. 
  * Cuando el expediente se cierra, sale del DMS y va al **Archivo Definitivo** (Ej: la herramienta **Archive** de la SGAD). Su función vital es aplicar el **resellado criptográfico** periódico para que las firmas electrónicas no caduquen con los años, cumpliendo la NTI de Política de Gestión de Documentos.

## 3. Sistemas CMS (Content Management System)
* **3.1. Concepto:** Software que permite crear y publicar contenido web separando el contenido de la presentación visual.
* **3.2. Arquitectura Clásica:** *Backend* (panel de administración WYSIWYG) + *Frontend* (lo que ve el ciudadano).
* **3.3. Principales CMS:** WordPress (43% de la web), Drupal (muy robusto, estándar en AAPP grandes), Joomla!, Liferay (Portales de empleado).
* **3.4. Headless CMS:** Separa totalmente Backend y Frontend. Expone el contenido vía **APIs REST/GraphQL**. 
  * *Uso AAPP:* Editar una noticia una vez (Backend) y publicarla simultáneamente en la Web Municipal, la App Ciudadana y Kioscos interactivos. Ej: Strapi, Contentful.

## 4. Generadores de Formularios
* **4.1. Concepto:** Herramientas sin código (*no-code*) para capturar datos (citas, quejas, encuestas).
* **4.2. Funcionalidades:** Diseño *drag-and-drop*, validación de campos, antispam (CAPTCHA), accesibilidad (WCAG 2.1) e integración con gestores de expedientes.
* **4.3. Soluciones:** Plugins de CMS (Gravity Forms), Independientes (Typeform) o Nativos de plataformas de e-Administración (Gestiona, SIGM).

## 5. Búsqueda de Información (Crawlers)
* **5.1. Concepto:** *Crawlers / Spiders / Robots* son programas automatizados de motores de búsqueda que recorren la web para indexarla.
* **5.2. Ciclo de vida:** 
  1. *Descubrimiento* (leyendo enlaces).
  2. *Indexación* (almacenando el contenido en su base de datos masiva).
  3. *Ranking* (ordenando por relevancia).
* **5.3. Control del Crawling:** Uso del archivo `robots.txt` (permisos de rastreo), etiquetas Meta `noindex` / `nofollow` y envíos de `Sitemap XML`.

## 6. Posicionamiento (SEO - Search Engine Optimization)
* **Concepto:** Técnicas para mejorar el ranking orgánico (gratuito) de una web institucional.
* **6.1. SEO On-Page (Factores internos):**
  * URLs amigables (`/licencia-obra`), Etiquetas `<title>` y `<meta description>`.
  * Jerarquía HTML (`<h1>-<h6>`) y atributos `alt` en imágenes (crucial para WCAG 2.1).
  * Rendimiento web (*Core Web Vitals* de Google) y *Mobile-First*.
  * Datos estructurados (Schema.org / JSON-LD).
* **6.2. SEO Off-Page (Externos):** *Backlinks* (enlaces entrantes de webs con autoridad, ej. ministerios o universidades) y señales sociales.
* **6.3. SEO Técnico:** Certificados HTTPS/SSL (Seguridad), URLs canónicas (evita contenido duplicado).

## 7. Herramientas de Trabajo Colaborativo
* **7.1. Concepto:** Plataformas para coedición, comunicación y gestión de tareas en equipos.
* **7.2. Soluciones AAPP:** 
  * *Suites:* Microsoft 365, **Nextcloud** (Muy implantado en AAPP como alternativa de código abierto y soberana para nube privada).
  * *Comunicación:* Teams, Slack, Jitsi (Videoconferencia Open Source).
  * *Gestión:* Trello, Jira, Confluence (Wikis de conocimiento).
* **7.3. Restricciones AAPP:** Cumplimiento estricto del ENS, protección de datos (RGPD) priorizando la **soberanía del dato** (almacenamiento en territorio nacional/UE).

## 8. Conclusión
El ecosistema digital de un Ayuntamiento requiere una orquestación perfecta: el DMS custodia la vida del documento, el CMS (clásico o Headless) y los formularios interactúan con el ciudadano, el SEO y los crawlers garantizan la transparencia haciendo la información localizable, y las herramientas colaborativas (bajo el paraguas del ENS) garantizan la productividad de los empleados públicos en la era del teletrabajo.

-----------------------

# Tema 32.- Sistemas de gestión documental y de contenidos. Sistemas CMS: definición y conceptos. Generadores de formularios. Búsqueda de información: robots, spiders, otros. Posicionamiento y buscadores (SEO). Herramientas de trabajo colaborativo.

## 1. Introducción

- Las AAPP generan gran cantidad de **contenido digital**.
- Necesitan herramientas para **gestionar, publicar, buscar y compartir información**.
- Elementos principales: **SGD → CMS → Formularios → Crawlers (indexación web; robots y spiders) → SEO → Colaboración**.

## 2. Sistemas de Gestión Documental

### 2.1. Concepto

**SGD/DMS:** sistema para **capturar, almacenar, organizar, versionar, buscar y recuperar (CABORV)** documentos electrónicamente.

### 2.2. Funcionalidades principales

- **Captura:** digitalización/OCR e importación.
- **Almacenamiento:** repositorio centralizado.
- **Metadatos:** autor, fecha, tipo, estado, expediente.
- **Versionado:** historial y recuperación de versiones.
- **Acceso:** permisos por usuario/rol/grupo.
- **Workflows:** revisión → aprobación → publicación.
- **Búsqueda:** metadatos + texto completo.
- **Retención:** conservación y eliminación conforme a normativa archivística.

### 2.3. Soluciones de gestión documental

- **Alfresco:** ECM, open source/comercial.
- **OpenText:** ECM empresarial.
- **SharePoint:** Microsoft 365 + colaboración.
- **Nuxeo:** open source, API-first.

* **2.4. DMS vs. Archivo Electrónico Único (Proyecto Archive):**
  * Vinculado a la funcionalidad de *Retención*, cuando un expediente se cierra, sale del DMS diario y debe ir al **Archivo Definitivo** (Ej: herramienta **Archive** de la SGAD). 
  * Su función crítica es aplicar el **resellado criptográfico periódico** para que las firmas electrónicas de los documentos no caduquen con los años.

## 3. Sistemas CMS (Content Management System)

### 3.1. Concepto

- **CMS:** permite **crear, editar, organizar y publicar contenido web sin programar**.
- Separa **contenido y presentación**.

### 3.2. Arquitectura del CMS

- **Frontend:** parte pública que ve el ciudadano.
- **Backend:** administración y edición de contenidos. **WYSIWYG** edición visual sin HTML.

### 3.3. Principales CMS

- **WordPress:** PHP/MySQL, extensible mediante plugins/temas.
- **Drupal:** flexible y robusto, habitual en grandes organizaciones.
- **Joomla:** complejidad intermedia.
- **Liferay:** Java, portales empresariales, roles y workflows.

### 3.4. Headless CMS

- Separa **backend y frontend**.
- Contenido accesible mediante **APIs REST/GraphQL**.
* *Uso AAPP:* Editar una noticia una vez (Backend) y publicarla simultáneamente en la Web Municipal, la App Ciudadana y Kioscos interactivos. Ej: Strapi, Contentful.

## 4. Generadores de Formularios

### 4.1. Concepto

- Herramientas para crear **formularios web dinámicos sin programar**.
- Ej.: citas, encuestas, quejas y solicitudes.

### 4.2. Funcionalidades

- Diseño **drag-and-drop**.
- Validación de datos.
- **CAPTCHA/antispam**.
- Envío por correo o BD.
- Integración con sistemas de tramitación.
- **Accesibilidad WCAG 2.1**.

### 4.3. Soluciones

- **Plugins:** Gravity Forms, Contact Form 7 (WordPress), Webform (Drupal).
- **Independientes:** JotForm, Typeform, Microsoft Forms.
- **Administración electrónica:** formularios integrados en gestores de expedientes (SIGM, Gestiona) 

## 5. Búsqueda de Información: Robots, Spiders y Crawlers

### 5.1. Concepto

**Crawler/Spider/Robot/Bot:** programa que recorre la web para **descubrir e indexar contenido**. 

### 5.2. Funcionamiento

1. **Crawling:** descubre y descarga páginas. Extrae enlaces
2. **Enlaces:** sigue enlaces y descubre nuevas URLs.
3. **Indexación:** almacena y procesa el contenido.
4. **Ranking:** ordena resultados según relevancia.

### 5.3. Control del crawling

- **robots.txt:** controla qué rutas pueden rastrearse.
- **Meta robots:** Controlan indexación (`noindex`, `nofollow`)
- **Sitemap XML:** facilita al crawler descubrir URLs.

## 6. SEO (Search Engine Optimization)

### 6.1. Concepto

- **SEO (Search Engine Optimization):** técnicas para mejorar el posicionamiento (ranking) en resultados **orgánicos/no pagados**.

### 6.2. SEO On-Page (factores internos)

- URLs descriptivas (`/tramites/licencia-obra` vs. `/page?id=4523`)
- `<title>` y `meta description`.
- Encabezados `<h1>-<h6>`.
- Contenido relevante y actualizado.
- Velocidad y **Core Web Vitals**.
- Accesibilidad WCAG 2.1 (`alt`, HTML semántico).
- Diseño **responsive/mobile-first**.
- Datos estructurados **Schema.org/JSON-LD**.

### 6.3. SEO Off-Page (factores externos)

Reputación y autoridad del sitio web fuera de él:
    - **Backlinks** de sitios de autoridad.
    - Directorios oficiales.
    - Menciones y enlaces en redes sociales.

### 6.4. SEO Técnico

- Sitemap XML.
- robots.txt correctamente configurado.
- **HTTPS/SSL-TLS**.
- URLs canónicas (evitar contenido duplicado)
- `hreflang` para webs multilingües.

## 7. Herramientas de Trabajo Colaborativo

### 7.1. Concepto

Plataformas para **compartir documentos, comunicarse, gestionar tareas y coordinar proyectos**.

### 7.2. Categorías y soluciones

- **Ofimática:** Microsoft 365, Google Workspace.
- **Gestión documental:** SharePoint, Alfresco, **Nextcloud** (Muy implantado en AAPP como alternativa de código abierto y soberana para nube privada).
- **Comunicación:** Teams, Slack.
- **Proyectos:** Jira, Trello, Planner.
- **Wikis:** Confluence, MediaWiki.
- **Videoconferencia:** Teams, Zoom, Jitsi.

### 7.3. Consideraciones para las AAPP

- **Soberanía de datos:** Almacen de datos en UE o infraestructura propia.
- **Cumplimiento:** RGPD, LOPDGDD y ENS.
- **Accesibilidad:** WCAG 2.1 AA.
- **Interoperabilidad:** formatos abiertos (ODF) y estándares.

## 8. Conclusión

- **SGD:** gestiona documentos.
- **CMS:** gestiona contenidos web.
- **Formularios:** facilitan interacción ciudadana.
- **Crawlers + SEO:** permiten localizar la información.
- **Colaboración:** facilita trabajo y coordinación.