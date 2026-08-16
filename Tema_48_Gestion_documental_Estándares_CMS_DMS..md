# Tema 48.- Gestión documental, estándares. Gestión de contenidos. Tecnologías CMS y DMS de alta implantación.

## 1. Introducción

En la actual era digital, la información constituye uno de los activos estratégicos más críticos para cualquier organización. A diferencia de los datos estructurados en bases de datos relacionales, la inmensa mayoría del conocimiento organizativo y de la interacción ciudadana se genera y custodia en forma de información no estructurada: documentos de texto, correos, imágenes y contenido web.

En el ámbito de las Administraciones Públicas, la correcta gestión de esta información trasciende la eficiencia operativa para convertirse en una obligación legal. Las Leyes 39/2015 y 40/2015 establecen que la tramitación administrativa debe ser íntegramente electrónica, obligando a dominar tanto la **gestión documental** (enfocada en el documento, el expediente electrónico y su valor legal) como la **gestión de contenidos** (enfocada en la publicación y transparencia a través de sedes electrónicas).

Para orquestar este volumen de información, el mercado proporciona soluciones de **DMS (Document Management Systems)** y **CMS (Content Management Systems)**. Este tema aborda los conceptos de gestión documental y de contenidos, los estándares internacionales y nacionales aplicables, y analiza las tecnologías de alta implantación que sustentan la Administración Electrónica.

## 2. Gestión Documental: Conceptos Fundamentales

La **Gestión Documental** (Records Management) es el conjunto de normas, técnicas y prácticas usadas para administrar el flujo de documentos en una organización, permitir la recuperación de información, determinar su tiempo de conservación, eliminar los prescindibles y asegurar la preservación indefinida de los más valiosos.

### 2.1. El Ciclo de Vida del Documento

La gestión documental moderna controla el ciclo de vida del documento, abarcando estas fases:
1.  **Creación o Captura:** El documento nace electrónicamente o se digitaliza desde papel. En la Administración, se exige la "digitalización certificada" para otorgar validez de copia auténtica.
2.  **Gestión y Tramitación:** El documento se clasifica, se le asignan metadatos, se inserta en flujos de trabajo (workflows) y se incorpora a un expediente electrónico con controles de acceso y versionado.
3.  **Almacenamiento y Recuperación:** Se deposita en un repositorio estructurado que garantiza inalterabilidad y permite su recuperación rápida mediante motores de búsqueda.
4.  **Preservación y Archivo:** Fase de inactividad. Se traslada a un archivo definitivo garantizando su legibilidad futura (formatos estándar, firma longeva).
5.  **Expurgo o Destrucción:** Eliminación segura y certificada de documentos que han perdido su valor administrativo o jurídico, según dictámenes de comisiones calificadoras.

### 2.2. El Documento y el Expediente Electrónico en la AAPP

En el sector público español, el documento electrónico va ligado a la **firma electrónica** y a los **metadatos**. El Esquema Nacional de Interoperabilidad (ENI) lo define como información digital estructurada que incluye contenido, firma y metadatos obligatorios.

El **expediente electrónico** es un objeto digital complejo (XML) que agrupa documentos electrónicos, un índice electrónico firmado que garantiza su integridad (asegurando que no se extraen ni añaden documentos a posteriori) y la firma del órgano que lo remite.

## 3. Estándares en la Gestión Documental

La normalización es crucial para garantizar la interoperabilidad, la migración tecnológica y la preservación a largo plazo. 

* **Normativa ISO 15489:** Esta norma regula de forma integral la gestión de los documentos que producen las organizaciones, ofreciendo una metodología clara para el diseño y la correcta implementación de un DMS.
* **Normativa ISO 30300:** Da continuidad a la norma anterior proponiendo un sistema formal para la gestión de los documentos que se fundamenta en el ciclo de mejora continua.
* **MoReq:** Modelo Europeo de Requerimientos para un Sistema de Gestión de Documentos Electrónicos de Archivo. Elaborado en la Unión Europea por el DLM-Forum, este documento estandariza los requisitos detallados funcionales y no funcionales que deben cumplir estos sistemas. Abarca sistemas de clasificación, control y seguridad, procesos de conservación y eliminación, motores de búsqueda, recuperación y la gestión estricta de metadatos.
* **OAIS (Open Archival Information System - Norma ISO 14721):** Modelo conceptual de referencia vital para el archivado y preservación a largo plazo de documentos. El estándar OAIS modela todo el ciclo definiendo las fases de ingesta, acceso, administración, gestión de datos, preservación y almacenamiento. Utiliza tres tipos de paquetes de información:
    * **SIP (Submission Information Package):** El paquete de información inicial que se envía al sistema para su ingesta. Contiene el objeto original (datos y metadatos) transferido por el productor.
    * **AIP (Archival Information Package):** Es la información de archivo final en la que se transforma un SIP para garantizar su almacenamiento seguro y duradero.
    * **DIP (Dissemination Information Package):** Paquete de información de difusión que contiene el objeto con la respuesta digital entregada a un consumidor tras una solicitud de acceso. (Cabe destacar que el servicio ARCHIVE de la AGE cumple con este estándar OAIS).

* **El Marco Nacional: El ENI** El Esquema Nacional de Interoperabilidad (ENI) establece las normas en la Administración Pública española a través de Normas Técnicas de Interoperabilidad (NTI) relativas a gestión documental:
    * **NTI de Documento y Expediente Electrónico:** Fijan estructuras XML, metadatos mínimos obligatorios (eMGDE) y reglas del índice.
    * **NTI de Política de Gestión de Documentos Electrónicos:** Obliga a cada organización a aprobar una política que rija todos sus documentos.
    * **NTI de Digitalización y Copiado Auténtico:** Regula formatos y resoluciones para garantizar el valor probatorio frente al papel.
    * **NTI de Catálogo de Estándares:** Lista formatos de archivo admitidos (PDF/A, ODF, XML).

Para la interoperabilidad a nivel técnico entre plataformas, la industria utiliza:

* **CMIS (Content Management Interoperability Services):** Estándar abierto que define una capa de abstracción para que diferentes sistemas y repositorios de gestión de contenidos operen a través de Internet usando protocolos web. Permite ejecutar operaciones básicas (crear, leer, actualizar, eliminar, proteger) sobre documentos, sus versiones y metadatos. Aunque facilita la interoperabilidad, no sustituye a las APIs nativas de cada producto.
* **JCR (Content Repository API for Java - JSR-170 / JSR-283):** Especificación para que las aplicaciones Java accedan uniformemente a los repositorios de contenido. Es implementada por Apache Jackrabbit y utilizada por gestores como Liferay o Magnolia.
* **WebDAV / CIFS:** WebDAV es una extensión HTTP que permite crear, cambiar y mover documentos almacenados en unidades virtuales remotas de un servidor. CIFS es un protocolo de capa de aplicación de Microsoft para el acceso compartido a ficheros en red.

 

## 4. Gestión de Contenidos (ECM y WCM)

En paralelo a los sistemas documentales orientados al registro legal y de negocio, surgen necesidades específicas de gestión para los contenidos digitales creados y mantenidos para el entorno web.

Un Sistema de Gestión de Contenidos (SGC o CMS por sus siglas en inglés, también conocidos como Web CMS) es una aplicación de software que provee un entorno de trabajo integral para la creación, publicación, gestión y administración de contenidos. Estos contenidos están diseñados para ser accesibles digitalmente a través de páginas web y múltiples dispositivos.

La gran ventaja organizativa de los CMS reside en su gestión de perfiles de usuario. Permiten configurar diferentes niveles de acceso (creadores, editores, revisores, publicadores o administradores) para simplificar y controlar el estado de revisión de cada contenido desde que se edita hasta que ve la luz pública. Un caso de uso clásico es aquel donde unos editores redactan y cargan el texto en el sistema, pero este no es visible al público general hasta que un usuario de nivel superior (moderador) lo revisa y aprueba explícitamente.

Técnicamente, el CMS maneja de manera completamente independiente el contenido (texto puro, datos) del diseño visual. A través de una interfaz que interactúa con una o varias bases de datos, es posible alterar el diseño de toda una web aplicando nuevas plantillas predefinidas sin necesidad de modificar el contenido subyacente cada vez. El flujo de vida estándar abarca la creación de nueva información (textos, gráficos), la presentación, la publicación y su posterior mantenimiento o actualización (edición o borrado).

Además, las versiones modernas de estos sistemas suelen contar con soporte multi idioma, generación de analíticas y estadísticas, URLs amigables, compatibilidad con SSO (Single Sign-On) y módulos para foros, blogs y encuestas. Incluso se integran con funcionalidades de Big Data para personalizar campañas y mejorar la experiencia de los usuarios.

### 4.1 Evolución de Arquitecturas CMS

Al igual que los ECM, los CMS han evolucionado drásticamente desde despliegues en Centros de Procesamiento de Datos (CPDs) locales hacia servicios en la nube pública y privada. Hoy en día, conviven tres arquitecturas principales:

1. **Sistemas Tradicionales:** Ofrecen toda la funcionalidad dentro de una misma arquitectura acoplada. La base de datos, el código subyacente, los plugins y las plantillas front-end conviven en un único bloque monolítico servido por el servidor web.
2. **Sistemas Headless (Desacoplados o "sin cabecera"):** Arquitectura moderna basada en microservicios que separa por completo el frontend (capa de presentación visual) del backend (lógica y almacenamiento de datos). El contenido centralizado se distribuye a través de APIs a múltiples canales, ya sean webs de escritorio, aplicaciones de dispositivos móviles o relojes inteligentes, garantizando la omnicanalidad requerida hoy en día. Exige, eso sí, la construcción de plantillas ad-hoc por parte de los desarrolladores.
3. **Sistemas Híbridos:** Muchas organizaciones complejas optan por un modelo mixto. Esta arquitectura combina una capa de presentación tradicional integrada en el propio CMS, mientras simultáneamente permite el desacoplamiento mediante APIs para exponer ciertos contenidos hacia otros canales o aplicaciones de terceros.

## 5. Tecnologías DMS de Alta Implantación

Los Sistemas de Gestión Documental (DMS), a menudo el núcleo de plataformas ECM mayores, son herramientas de software diseñadas para organizar, rastrear y archivar documentos electrónicos. 

Sus características indispensables incluyen: control de versiones, auditoría de accesos, búsqueda a texto completo, integración ofimática, metadatos personalizados y retención automatizada. El estándar para integrar distintos DMS es **CMIS** (Content Management Interoperability Services).

Se dividen en plataformas privativas y de código abierto:

### 5.1. Soluciones Privativas (Propietarias)

*   **Microsoft SharePoint:** Gigante del mercado corporativo. Evolucionado desde un portal intranet hacia un potente DMS, se integra nativamente con Microsoft 365, Teams y OneDrive. Permite construir intranets, listas de datos y flujos de trabajo con Power Automate.
*   **OpenText Documentum:** La plataforma DMS de gama alta más madura, orientada a gigantes corporativos o entes públicos estatales. Destaca por su estricto cumplimiento legal, arquitecturas masivas de escalabilidad y gestión de expedientes sumamente complejos.
*   **IBM FileNet P8:** Muy potente en entornos donde la carga documental masiva se une imperativamente a flujos de trabajo BPM estrictos (procesamiento de facturas o seguros).

### 5.2. Soluciones Open Source

*   **Alfresco:** Es el DMS open source de mayor implantación mundial y un referente absoluto en las Administraciones Públicas españolas. Basado en Java, ofrece edición comunitaria y empresarial. Destaca por su arquitectura abierta, soporte del estándar CMIS, soporte nativo de metadatos modelables y motor integrado de flujos de trabajo (Activiti).
*   **Nuxeo:** Potente plataforma ECM/DMS de código abierto en Java, orientada a desarrolladores mediante una arquitectura escalable diseñada para manejar volúmenes masivos e integrarse en ecosistemas modernos cloud.

### 5.3. Soluciones en el Ámbito de la Administración Pública

La Administración Pública española promueve el uso de alternativas reutilizables para la gestión documental electrónica:

* **Espacios de Colaboración:** Basado en la tecnología del portal Liferay (Java), ofrece funcionalidades de gestión documental, agendas y foros.
* **Inside:** Herramienta en Java gestionada por el MINHAP. Permite almacenar y modificar documentos y expedientes electrónicos en cualquier gestor documental del mercado que sea compatible con el estándar CMIS. También gestiona los metadatos obligatorios asociados a estos.
* **G-Inside:** Servicio basado en la nube (SaaS) con web services, también del MINHAP, que facilita la generación de documentos y expedientes electrónicos garantizando la conformidad con el Esquema Nacional de Interoperabilidad (ENI).
* **@Doc:** Plataforma del Ministerio de la Presidencia desarrollada en Java para la gestión de documentos y expedientes electrónicos.
* **ARCHIVE:** Aplicación web transversal destinada al archivo definitivo de documentos y expedientes a largo plazo.

## 6. Tecnologías CMS de Alta Implantación

Los CMS (Content Management Systems) o sistemas WCM revolucionaron la creación web institucional, facilitando a equipos de comunicación crear rápidamente portales y sedes corporativas sin programar cada página.

### 6.1. Características y Arquitectura

Suelen basarse en arquitecturas tipo LAMP/LEMP compuestos por dos interfaces:
*   **Backend / Panel de Administración:** Interfaz privada para crear contenidos y administrar jerarquías y plantillas.
*   **Frontend:** Interfaz pública generada dinámicamente que fusiona plantillas con datos de la base para mostrarlos al visitante.

### 6.2. Soluciones Tradicionales Dominantes

*   **WordPress:** El rey indiscutible de internet (más del 40% de la web). Desarrollado en PHP. Inmensamente popular por su extrema facilidad de uso y gigantesco ecosistema de plugins. Utilizado extensivamente para portales informativos de Ayuntamientos, exige severas políticas de actualización de seguridad.
*   **Drupal:** También en PHP, es el CMS predilecto para grandes portales interadministrativos y sedes estatales (el Portal de Administración Electrónica PAe lo emplea). Destaca por su robustez monumental, una arquitectura modular sofisticada, taxonomía avanzada y un núcleo sometido a estrictas auditorías de seguridad.
*   **Joomla:** Solución intermedia en PHP con mayor complejidad de roles nativos que WordPress, pero menor curva de desarrollo base que Drupal.

Existe también software encuadrado como Gestor de Portales, por tener funcionalidades más amplias para integrar aplicaciones complejas empresariales, como Liferay y Microsoft Office SharePoint Server. Otros gestores se especializan en nichos concretos: Moodle y Claroline (Educación), osCommerce, Magento y PrestaShop (Comercio), MediaWiki y Dokuwiki (Wikis), o phpBB (Foros). Dentro de las soluciones de la Administración, destaca la herramienta "ACCEDA".

## 7. Conclusión

La madurez tecnológica de la Administración Pública obliga a orquestar el crecimiento exponencial del volumen de la información operando sobre dos vertientes convergentes. La **Gestión Documental (DMS)** salvaguarda el corazón administrativo-legal mediante la creación controlada de documentos, el versionado del expediente amparado por el ENI y el resguardo futuro a largo plazo del archivo OAIS, con herramientas líderes como Alfresco y Archive. Por contraposición complementaria, los sistemas de **Gestión de Contenido (CMS)** como Drupal, WordPress o Liferay erigen el escaparate comunicativo, permitiendo adaptar webs y sedes electrónicas al ciudadano dinámicamente. Juntos conforman el binomio de tecnologías de información imprescindible que cimienta la modernización y transparencia de la actual y futura Administración Electrónica.
