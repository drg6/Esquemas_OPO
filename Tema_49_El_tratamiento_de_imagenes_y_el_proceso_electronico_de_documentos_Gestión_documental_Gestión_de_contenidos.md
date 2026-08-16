# Tema 49.- El tratamiento de imágenes y el proceso electrónico de documentos. Gestión documental. Gestión de contenidos.

## 1. Introducción

El paradigma hacia una Administración Pública del "papel cero" ha transformado radicalmente cómo se ingieren, tramitan y custodian los expedientes. Las Leyes 39/2015 y 40/2015 imponen que los procedimientos discurran por canales íntegramente electrónicos, prohibiendo, salvo excepciones, el archivo final en soporte físico.

Para habilitar este reto, cuando el ciudadano o registro aporta un papel físico, es obligatorio un proceso de desmaterialización técnica amparado en el **Esquema Nacional de Interoperabilidad (ENI)**. Este proceso requiere dominar el **tratamiento de imágenes** (para convertir la hoja física en un soporte digital y extraer sus datos), orquestar el **proceso electrónico** mediante plataformas de **gestión documental (DMS)**, y publicarlo en sedes mediante **sistemas de gestión de contenidos (CMS/WCM)**.

Este tema analiza las técnicas de digitalización, el modelo del documento electrónico, y las infraestructuras que conforman el ecosistema de la e-Administración.

## 2. El tratamiento de imágenes

### 2.1. Digitalización y captura

El tratamiento documental comienza con el escaneado (digitalización), un proceso de muestreo para traducir reflectancias lumínicas del papel en una matriz de píxeles inteligibles.

**Parámetros críticos de la captura:**
*   **Resolución Espacial:** Medida obligatoriamente en puntos por pulgada (DPI - Dots per Inch) o píxeles por pulgada (PPI). Dictamina el grado de detalle óptico con el cual es leída la matriz física (v.g.: si un carácter es nítido o borroso).
*   **Profundidad de Color:** La amplitud espectral capturada. Puede ser binaria (Blanco y Negro a 1 bit, óptima para texto puro); Escala de Grises (8 bits, revelando 256 niveles de gris para fotografías o sellos entintados); o Color Completo (24 bits TrueColor).

Para copias verificadas, el ENI suele exigir al menos **200 DPI en B/N o Grises** para garantizar que los textos y firmas manuscritas queden fijadas legal y técnicamente irreprochables.

### 2.2. Formatos de almacenamiento

Las imágenes capturadas "en crudo" (RAW o BMP) requieren tamaños inmanejables para las bases corporativas; es vital aplicar algoritmos que minimicen su capacidad (Megabytes):

*   **Formatos para imágenes estáticas:** 
    *   **TIFF:** Estándar de facto para escaneados binarizados en alta calidad empresarial porque emplea compresiones sin pérdidas matemáticas (Lossless, como CCITT G4).
    *   **JPEG:** Utiliza compresión matemática que suprime drásticamente frecuencias (Lossy). Ideal para fotografías, pero provoca halos borrosos inaceptables en texto de documentos puramente burocráticos.
    *   **PNG:** Ofrece transparencia y algoritmos sin alteración, más usado en diseño web y portales que para conservación documental masiva que es ineficiente.

*   **Formatos de portabilidad final:**
    *   **PDF/A:** Esencial en la Administración y normalizado ISO. Obliga a que tipografías y fuentes queden totalmente embebidas en el fichero, prohibiendo contenido multimedia. Así garantiza que cualquier archivo histórico que se abra treinta años después presente idéntico aspecto al original, salvaguardando su valor probatorio.

### 2.3. Herramientas técnicas de mejora (Pre-procesamiento)

La imagen bruta suele contener defectos por grapas, manchas o dobleces. Antes de procesarla, se ejecutan operaciones matriciales automáticas:
*   **Binarización:** Los píxeles fronterizos grises se fuerzan matemáticamente al blanco o negro pleno. Mejora el peso informático y la precisión en la posterior lectura OCR.
*   **Deskewing (Corrección de inclinación):** Alinea las líneas del texto torcidas al resbalar el papel en la bandeja del escáner.
*   **Despeckling (Limpieza):** Barrido para eliminar pequeñas motas y polvo oscurecido, evitando que los algoritmos de reconocimiento los tomen como signos de puntuación válidos (puntos o comas).

### 2.4. Obtención de datos estructurados: OCR, ICR y OMR

Poseer un TIFF perfecto es estéril si el ordenador no "entiende" el contenido. Técnicas específicas transforman píxeles ciegos en textos operables:
*   **OCR (Reconocimiento Óptico de Caracteres):** Evalúa píxeles para reconocer letras de imprenta, entregando volcados en texto plano estructurado indexable para búsquedas semánticas directas ("buscar un apellido").
*   **ICR (Reconocimiento Inteligente de Caracteres):** Analiza trazos de caligrafía manuscrita libre, algo imposible para el OCR tradicional. El ICR depende hoy fuertemente de complejas Redes Neuronales y Machine Learning para transcribir documentos rellenados a bolígrafo.
*   **OMR (Reconocimiento de Marcas Ópticas):** Reconoce marcas de casillas cuadriculadas (ej. sombreados o firmas de cuadrícula). Muy usado en corrección de test o modelos tributarios estándar, ofreciendo una fiabilidad del 100% en microsegundos si el ciudadano no tachó nada erróneamente.

## 3. El proceso electrónico de documentos

Transformar el soporte físico en bits marca el inicio. Acto seguido debe instruir el formalismo requerido por el Procedimiento Administrativo bajo el amparo del **ENI**.

### 3.1. Documento Electrónico y Expediente

El concepto e-Administrativo no habla de simples "archivos aislados u obsoletos esparcidos ciegamente o perdidos".
*   **Documento Electrónico:** Es la unidad atómica mínima regida legalmente. Constituye toda información digital dotada de **Metadatos** irremplazables y sellada inalterablemente bajo una **Firma Electrónica**, dotando al conjunto de un valor probatorio íntegramente asimilable ante jueces a un documento papel físico notariado.
*   **Expediente Electrónico:** Es la agrupación procedimental. Unifica varios documentos organizativamente con un índice reglado informático. Dicho índice también ostenta su propia huella/firma unívoca global, atestando y certificando que desde su clausura administrativa no faltan, sobran o han mutado expedientes u hojas posteriores internas, consolidando formalmente para archivo el folio probatorio global indivisible.

### 3.2. Fases del proceso y Digitalización Certificada

El circuito arranca en la ventanilla receptora (SIR - Sistema de Interconexión de Registros), originando la **Digitalización Certificada**. Con amparo normativo (NTI de Copiado Auténtico), esta operación certifica jurídicamente ante terceros perennes que la digitalización generada guarda estricta y literal obediencia milimétrica en validez respecto del original físico entrante ajeno presentado sobre mostrador.

Durante este escaneo primario, el sistema del órgano estampa su Sello Electrónico automático y opcionalmente el del sello temporal notarial (TSA/Timestamping). Tras esta vital transformación que crea "el e-documento verificado auténtico nativo", la administración se empodera y legitima procesal y normativamente a destruir el papel originario del particular aportador (salvando documentos con interés histórico puntualísimo), aboliendo los inviables almacenes burocráticos locales históricos atestados.

## 4. Gestión Documental

La Gestión Documental inter-AAPP es la rama archivística especializada en clasificar, posibilitar búsquedas, pautar la retención mínima impositiva jurídica y dictaminar los expurgos destructivos legales sobre bases informáticas conformes al estándar internacional **ISO 15489**.

### 4.1. Esquemas y Metadatos de la Información

Para rastrear y gobernar un corpus archivístico estatal con millones de expedientes deslocalizados informáticamente cruzados en red transeuropea, el ENI prohíbe el volcado de mallas aisladas. Instruye el denominado **Esquema de Metadatos para la Gestión del Documento Electrónico (eMGDE)**.
Las NTI especifican propiedades atadas en el XML del documento: Órgano origen (OCG), Tipo tipificado de Resolución, Identificador Único inalterable, formato subyacente y categoría vital ENS de confidencialidad y control policial o de salud, para así certificar total interoperabilidad automatizable burocrática ciega de trámites.

### 4.2. Sistemas DMS e Infraestructura

Las grandes organizaciones gestionan este circuito vital operando herramientas **DMS (Document Management Systems)** puristas y robustas; destacando referentes nativos Open Source en la administración española como **Alfresco** u OpenKM o propietarios robustísimos (SharePoint, IBM FileNet).

Un motor DMS suple abismalmente frente a meros repositorios ofimáticos redificados. Proveen funcional y rígidamente: Controles de Versionado inalterables (para revisiones en borradores encadenados burócratas), auditorías milimétricas trazables frente intromisiones en cuentas para expedientes en curso de salud (cumplimiento RGPD o tributario), y orquestan transversalmente flujos Business Process Management (BPM/Workflows) empujando de secretarios a alcaldes informáticamente la notificación para firmas al compás de plazos procedimentales legales.

Como hito conclusivo procedimental, este proceso estipulado culmina la andadura viva redirigiéndolo al confinamiento histórico legal del archivo cerrado inalterable (**Archivo Electrónico Único**). Plataformas estructurales impuestas y modélicas como **ARCHIVE** de la Secretaría General (SGAD) bajo directivas OAIS internacionales operan para salvaguardarlo durante siglos advenideros ante posibles enjuiciamientos revisionistas exentos de cualquier límite en capacidad tecnológica.

## 5. Gestión de Contenidos

Mientras la faceta DMS o gestión documental custodia el expediente y la formalidad jurídica procesalista inalterable estricta "hacia el centro del núcleo administrativo estatal" en canales cerrados; la **Gestión de Contenidos (CMS / WCM)** emerge funcionalmente diametral "hacia el exterior", gobernando y exponiendo la relación de Sedes Electrónicas o portales visuales transaccionales participativos, amoldando comunicación instantánea adaptada al ciudadano sin revelar a las bases internas.

### 5.1. Paradigma CMS y tecnologías

Un **Content Management System (CMS)** escinde arquitecturalmente la lógica computacional y del Diseño Visual Web frente al texto contenido original que el propio comunicador o empleado orgánico teclea en un formulario base, suprimiendo abrumadoramente el condicionante informático a los tramitadores directos que ahora generan instantáneamente boletines o avisos normativos sin programar código.

*   Para Sedes ministeriales puristas y desarrollos portal trasaccionales intra-institucionales, gozan vastísimo predominio y despliegue soluciones PHP ultraseguras como **Drupal** o portales corporativos estructurados Java Enterprise con Single Sign-On nativo unificable (**Liferay**).
*   En estamentos ágiles locales, predomina de forma casi hegemónica el uso mundial informático **WordPress** por su nula o diminuta curva de aprendizaje de sus reporteros corporativos publicadores municipales frente a eventos en calle e implantaciones genéricas o intranets, aunque exige blindar vulnerabilidades.
*   En la última vanguardia omnicanal afloran como tendencia arrolladora los denominados sistemas **Headless CMS**, esquemas ciegos escindidos que almacenan únicamente párrafos comunicativos normativos desde el back y carecen de plantilla o frontend HTML incorporado. El organismo genera el texto unificado general y microservicios APIs Rest/GraphQL empujan sirviéndolo puramente estructurado a milímetros simultáneos adaptativos para la propia aplicación nativa App móvil del teléfono móvil ciudadano oficial (Android/iOS), a su quiosco urbano tributario cívico, y al panel web Angular/React de un portal moderno unificado al compás o estricta simultaneidad, con cero costo y duplicidades redundantes corporativas operativamente.

## 6. Conclusión

El tratamiento desde del papel a la matriz digital para acabar frente en flujos de contenidos mundiales telemáticos ciudadanos encadena e involucra un ecosistema informático ineludible o impensable disolver de las interacciones legales vigentes en toda moderna jurisdicción actual en red española eIDAS. Extraer datos con riguroso y óptimo escaneo, aplicar pre filtros correctores matriciales ineludibles ciegos o desciframientos OCR o inteligencia ICR conforma la piedra de roseta base. La consiguiente estructuración imperiosa de empaques bajo esquemas rigurosos estandartes XML, Normas eMGDE ENI, expedientes con firma eIDAS y metadatos garantistas nutren operativamente a Gestores Documentales pesados ciegos DMS, garantizando legalidad impune irrefutable administrativa procedimental frente al órgano interviniente que dirima las resoluciones base. Todo el bagaje informativo remata volcándose con interoperabilidades puras finales publicándose e impactando al tejido y estrato del ciudadano, empresas y sociedad abierta generalizadamente conectada telemática usando y gobernando infraestructuras ágiles e omni presentativas web portables CMS logrando dotar, publicitar en el terminal telefónico ciudadano de su portal público en pro y a la altura e-Gobierno abierto participativo y garantizando constitucionalmente la ley administrativa 39/2015 del moderno Sector Público digital transparente.
