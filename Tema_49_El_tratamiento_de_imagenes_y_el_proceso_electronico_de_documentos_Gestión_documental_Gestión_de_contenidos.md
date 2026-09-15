# Tema 49.- El tratamiento de imágenes y el proceso electrónico de documentos. DMS y CMS.

## 1. Introducción
* **El reto del "Papel Cero":** Las Leyes 39/2015 y 40/2015 prohíben (salvo excepciones) el archivo físico.
* **El ecosistema:** Si entra papel, se **digitaliza** (Imagen) → se integra en un **Expediente** (ENI) → se custodia internamente (**DMS**) → y la información se publica al ciudadano (**CMS**).

## 2. El tratamiento de imágenes (Del papel al píxel)
* **2.1. Captura:** Resolución mínima exigida por el ENI para copias auténticas: **200 DPI** (puntos por pulgada) en B/N o escala de grises (8 bits).
* **2.2. Formatos:**
  * *Transitorios:* TIFF (alta calidad sin compresión), JPEG (con pérdida, fotos).
  * *Final (Archivo):* **PDF/A**, estándar ISO que incrusta fuentes y prohíbe multimedia para garantizar legibilidad a largo plazo.
* **2.3. Pre-procesamiento:** Binarización (a blanco y negro puro), Deskewing (enderezar) y Despeckling (limpiar motas de polvo).
* **2.4. Extracción de datos (El cerebro):**
  * **OCR:** Reconoce texto de imprenta. ⚠️ *Detalle clave:* Obligatorio para cumplir el **RD 1112/2018 de Accesibilidad** (permite que los lectores de pantalla lean el PDF a personas ciegas).
  * **ICR:** Reconoce caligrafía manual mediante IA.
  * **OMR:** Reconoce marcas en casillas (ej. exámenes o tributos).

## 3. El proceso electrónico de documentos
* **3.1. Documento y Expediente (ENI):**
  * *Documento:* Archivo + Metadatos + Firma.
  * *Expediente:* Conjunto de documentos + Índice electrónico firmado (garantiza que no se alteran los folios una vez cerrado).
* **3.2. Digitalización Certificada y OAMR:**
  * Se realiza en las Oficinas de Registro (SIR). Garantiza que la copia digital es exacta al original en papel mediante un Sello Electrónico de Órgano, permitiendo destruir el papel físico.
  * *Alternativa "Papel Cero" nativo:* Uso de **tabletas de firma biométrica** en ventanilla para que el ciudadano firme sin llegar a imprimir el papel.

## 4. Gestión Documental (DMS)
* Norma base: **ISO 15489** (organiza, custodia y expurga).
* **4.1. Metadatos y el DIR3:** Uso del Esquema **eMGDE** del ENI. Destaca el uso del código **DIR3** para identificar unívocamente al órgano productor o destino en toda la Administración.
* **4.2. Infraestructura DMS:**
  * Plataformas como **Alfresco** (Open Source) o SharePoint. Controlan el versionado, las auditorías de acceso y los flujos de trabajo (workflows).
  * Archivo final: Remisión al **Archivo Electrónico Único** (ej. plataforma ARCHIVE bajo estándar OAIS).

## 5. Gestión de Contenidos (CMS)
* Separa el diseño visual de los datos, permitiendo publicar sin saber programar.
* **5.1. Tecnologías clave en AAPP:**
  * **Portales Institucionales (Ministerios/Sedes críticas):** Soluciones robustas y seguras como **Drupal** (PHP) o **Liferay** (Java).
  * **Portales Ágiles (Ayuntamientos):** Dominio de **WordPress**. Fácil uso, pero exige parcheo constante de seguridad.
  * **Tendencia (Headless CMS):** Sistemas "sin cabeza" que solo guardan texto y lo envían por APIs REST simultáneamente a la Web, la App móvil y los Kioscos digitales (**Omnicanalidad**).

## 6. Conclusión
El tránsito de la burocracia física a la e-Administración requiere una cadena tecnológica ininterrumpida. La correcta aplicación del OCR y los formatos PDF/A garantizan la accesibilidad y preservación; la plataforma **DMS** asegura el rigor jurídico del expediente mediante metadatos ENI y códigos DIR3; y finalmente, el **CMS** democratiza el acceso a esta información, ofreciendo al ciudadano sedes electrónicas transparentes, ágiles y omnicanales.

--------------------

# Tema 49.- El tratamiento de imágenes y el proceso electrónico de documentos. Gestión documental. Gestión de contenidos.

## 1. Introducción

- La Administración Pública avanza hacia el **"papel cero"**.
- Las Leyes **39/2015 y 40/2015** impulsan la tramitación electrónica.
- El proceso documental se basa en:
  - **Digitalización y tratamiento de imágenes**.
  - **Documento y expediente electrónico**.
  - **Gestión documental (DMS)**.
  - **Gestión de contenidos (CMS/WCM)**.
- Todo ello bajo el **ENI** y sus Normas Técnicas de Interoperabilidad (NTI).

## 2. El tratamiento de imágenes

### 2.1. Digitalización y captura

- Escaneado, conversión del documento físico en imagen digital.
- Parámetros principales de la captura:
  - **Resolución:** DPI/PPI → nivel de detalle.
  - **Profundidad de color:**
    - B/N → 1 bit.
    - Escala de grises → 8 bits.
    - Color Completo → 24 bits.
- Para copias verificadas: ENI exige de **200 DPI** en B/N o grises.

### 2.2. Formatos de almacenamiento

RAW o BMP tamaño inmanejable, necesario comprimir.

- **TIFF:** alta calidad y compresión sin pérdidas → digitalización documental.
- **JPEG:** compresión con pérdidas → fotografías.
- **PNG:** compresión sin pérdidas y transparencia → principalmente web.
- **PDF/A:** formato para **conservación documental a largo plazo**. Normalizado ISO.

### 2.3. Herramientas técnicas de mejora (Pre-procesamiento)

Elimina defectos por grapas, manchas o dobleces.

- **Binarización:** convierte píxeles a blanco/negro → mejora OCR.
- **Deskewing:** corrige la inclinación del documento.
- **Despeckling:** elimina manchas y ruido.

### 2.4. Obtención de datos estructurados: OCR, ICR y OMR

- **OCR (Reconocimiento Óptico de Caracteres):** reconoce caracteres impresos y los convierte en texto. Obligatorio para cumplir el **RD 1112/2018 de Accesibilidad** (permite que los lectores de pantalla lean el PDF a personas ciegas).
- **ICR (Reconocimiento Inteligente de Caracteres):** reconoce caracteres manuscritos mediante técnicas avanzadas (Redes neuronales y Machine Learning)
- **OMR (Reconocimiento de Marcas Ópticas):** reconoce marcas o casillas de formularios.

## 3. El proceso electrónico de documentos

Digitalizar los documentos de papel es el primer paso. Después hay que realizar el Procedimiento Administrativo bajo el amparo del **ENI**.

### 3.1. Documento Electrónico y Expediente

- **Documento electrónico:**
  - Unidad básica de información digital.
  - Incluye **metadatos**.
  - Puede incorporar **firma electrónica**.
- **Expediente electrónico:**
  - Conjunto ordenado de documentos de un procedimiento.
  - Incluye un **índice electrónico**.
  - El índice se firma para garantizar su integridad.

### 3.2. Fases del proceso y Digitalización Certificada

- Se realiza en las Oficinas de Registro (SIR)
- Permite convertir documentos físicos en **copias electrónicas auténticas**.
- Se apoya en el **ENI y las NTI de copiado auténtico**.
- Puede incorporar:
  - **Sello electrónico automático**.
  - **Sello de tiempo (Timestamp) - opcional**.
- Permite prescindir del original en papel cuando la normativa lo permite.

## 4. Gestión Documental

- Gestiona el ciclo de vida de los documentos:
  - Clasificación.
  - Almacenamiento.
  - Búsqueda.
  - Control de versiones.
  - Conservación.
  - Eliminación.
- Referencia: **ISO 15489**.

### 4.1. Esquemas y Metadatos de la Información

- **Esquema de Metadatos para la Gestión del Documento Electrónico (eMGDE)** 
- Permite describir y gestionar los documentos de forma normalizada (metadatos obligatorios).
- Facilita la **interoperabilidad** entre Administraciones.
- Destaca el uso del código **DIR3** para identificar unívocamente al órgano productor o destino en toda la Administración.

### 4.2. Sistemas DMS e Infraestructura

- **DMS (Document Management System):** sistemas de gestión documental.
- Ejemplos:
  - **Alfresco / Nexus:** Open Source.
  - **SharePoint / OpenText Documentum:** soluciones propietarias.
- Funciones:
  - **Versionado**.
  - **Auditoría y trazabilidad**.
  - **Control de acceso**.
  - **Workflows/BPM**.
  - Gestión del ciclo de vida documental.
- El documento puede finalizar en el **Archivo Electrónico Único**.
- **ARCHIVE (SGAD):** plataforma de archivo electrónico de la AGE.

## 5. Gestión de Contenidos

  - **DMS →** gestiona documentos y expedientes internos (custodiando expedientes legales).
  - **CMS →** gestiona y publica contenidos en Sedes Electrónicas y portales web ara comunicarse con los ciudadanos.

### 5.1. Paradigma CMS y tecnologías

Un **CMS (Content Management System)** permite a los empleados públicos publicar noticias, normativas o avisos en la web del Ayuntamiento sin necesidad de saber programar. El sistema separa el texto (contenido) del diseño visual de la página.

* **1. Portales Institucionales (Ministerios / Grandes sedes)**
  * **Requisitos:** Alta seguridad e integración compleja (ej. Single Sign-On).
  * **Soluciones robustas:** **Drupal** (en PHP) o **Liferay** (en Java).

* **2. Portales ágiles (Ayuntamientos / Nivel local)**
  * **Requisitos:** Facilidad de uso para personal administrativo (no técnico).
  * **Solución dominante:** **WordPress**.
  * **Punto crítico:** Obliga a mantener los componentes de seguridad muy actualizados para evitar ciberataques.

* **3. Tendencia Actual: Headless CMS ("Sin cabeza")**
  * Separa **contenido y presentación**.
  * **Funcionamiento:** El contenido se ofrece mediante **APIs REST/GraphQL**.
  * **Gran ventaja:** **Omnicanalidad**. Permite reutilizarlo en web, apps móviles y otros dispositivos.

## 6. Conclusión

La modernización de la Administración Pública -> ecosistema tecnológico integrado. 

1. **Digitalización:** convierte papel en documento digital.
2. **DMS:** gestión interna del ciclo de vida documental. Cumpliendo requisitos legales del ENI (firmas, metadatos, expedientes XML)
3. **CMS:** proyecta actividad administrativa hacia el exterior, creación y publicación de contenidos en sedes electrónicas. Cumpliendo así con los principios de transparencia y servicio público del entorno digital.