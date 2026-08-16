# Tema 11.- Sistema de información Geográfica y sus aplicaciones municipales. Funcionalidades y tecnologías aplicables. Procesos de carga de la información y controles de calidad. Geoportales.

## 1. Introducción

Los Sistemas de Gestión de Bases de Datos Relacionales analizados en los temas anteriores son extraordinariamente eficaces para gestionar información alfanumérica: pueden determinar *quién* es el titular de un inmueble, *cuánto* adeuda de IBI o *cuándo* se emitió una licencia de obras. Sin embargo, presentan una limitación intrínseca: no pueden responder de forma nativa a preguntas de naturaleza **espacial**, como *dónde* se encuentra el inmueble en relación con una zona inundable, qué parcelas se ven afectadas por un nuevo trazado viario, o cuál es la ruta óptima para un vehículo de emergencias.

Para cubrir esta dimensión geoespacial surgen los **Sistemas de Información Geográfica (SIG o GIS - Geographic Information Systems)**: sistemas integrados de hardware, software, datos y procedimientos diseñados para capturar, almacenar, manipular, analizar, modelizar y visualizar información **georreferenciada**, es decir, vinculada inequívocamente a coordenadas de la superficie terrestre.

En el ámbito municipal, los SIG se han convertido en herramientas estratégicas de primer orden, permitiendo a los ayuntamientos gestionar su territorio con una precisión y una capacidad analítica sin precedentes, integrando datos urbanísticos, catastrales, medioambientales, de infraestructuras y de servicios públicos en un único marco espacial de referencia.

## 2. Definición Formal y Componentes de un SIG

### 2.1. Definición

Según la definición del National Center for Geographic Information and Analysis (NCGIA), un SIG es un **sistema integrado de hardware, software, datos geográficos y personal**, diseñado para capturar, almacenar, actualizar, manipular, analizar y visualizar toda forma de información geográficamente referenciada. No se trata de un simple mapa digital ni de un visor cartográfico, sino de un sistema analítico que permite correlacionar variables de naturaleza diversa en función de su ubicación espacial.

### 2.2. Componentes arquitectónicos

Un SIG operativo a nivel municipal se sustenta sobre cinco pilares:

1.  **Hardware (Infraestructura física):** Servidores del Centro de Procesamiento de Datos (CPD) con capacidad para almacenar terabytes de cartografía y ortofotos, estaciones de trabajo con capacidad gráfica avanzada, periféricos especializados (plotters de gran formato A0/A1, escáneres de planos, dispositivos GPS de precisión, drones para captura de datos LIDAR) y redes de comunicaciones de alta velocidad.

2.  **Software (Componentes lógicos):**
    *   **SGBD con extensión espacial:** Almacena y gestiona los datos geográficos. Los más relevantes son PostgreSQL con la extensión **PostGIS** (software libre), **Oracle Spatial** y **Microsoft SQL Server Spatial**.
    *   **Clientes de escritorio (Desktop GIS):** Aplicaciones de análisis y edición cartográfica como **ArcGIS Pro** (Esri, propietario), **QGIS** (libre y de código abierto) y **gvSIG** (libre, de origen valenciano).
    *   **Servidores de mapas:** Publican los datos geográficos como servicios web para su consumo por aplicaciones y geoportales. Destacan **GeoServer** y **MapServer** (libres), y **ArcGIS Server** (Esri).
    *   **Bibliotecas de visualización web:** Permiten integrar mapas interactivos en aplicaciones web: **Leaflet**, **OpenLayers**, **MapLibre** y la API de ArcGIS.

3.  **Datos geográficos (El componente más crítico y costoso):** Los datos espaciales integran dos componentes inseparables:
    *   **Componente espacial (geometría):** La representación gráfica del fenómeno geográfico: puntos, líneas, polígonos o superficies, definidos mediante coordenadas en un sistema de referencia determinado.
    *   **Componente alfanumérica (atributos):** La información descriptiva asociada a cada entidad geográfica: titular de la parcela, valor catastral, año de construcción, referencia catastral, etc.

4.  **Procedimientos y metodologías:** Protocolos formales que regulan la captura, actualización, validación y difusión de los datos espaciales: cuándo se actualiza la cartografía urbana, en qué sistema de coordenadas se trabaja, cómo se aprueba la incorporación de nuevas capas, quién valida la calidad topológica de los datos.

5.  **Personal (Capital humano):** Ingenieros en geomática, cartógrafos, analistas GIS, administradores de bases de datos espaciales y desarrolladores especializados en tecnologías geoespaciales.

## 3. Modelos de Representación Espacial: Vectorial y Ráster

Toda información geográfica se representa mediante uno de los dos modelos fundamentales de datos espaciales:

### 3.1. Modelo Vectorial

Representa las entidades geográficas como objetos geométricos discretos definidos por coordenadas (X, Y, Z). Es el modelo predominante en cartografía urbana y catastral, donde se requiere precisión geométrica y atributos asociados.

Las tres primitivas geométricas vectoriales son:

1.  **Punto (dimensión 0D):** Un único par de coordenadas (X, Y) que representa entidades sin extensión superficial: la ubicación de un semáforo, una farola, una boca de riego, un contenedor de residuos o la posición de un vehículo GPS.

2.  **Línea o Polilínea (dimensión 1D):** Secuencia ordenada de puntos (vértices) conectados que define una entidad con longitud pero sin área: ejes de calles, trazado de tuberías de abastecimiento, líneas de autobús, cauces de ríos, tendidos eléctricos.

3.  **Polígono (dimensión 2D):** Secuencia cerrada de líneas donde el vértice inicial coincide con el final, delimitando una superficie con área y perímetro: parcelas catastrales, manzanas urbanas, zonas verdes, distritos municipales, edificios.

**Formatos vectoriales habituales:** Shapefile (.shp — formato de Esri, estándar de facto), GeoJSON, GML (Geography Markup Language, estándar OGC), KML (Google Earth), GeoPackage (.gpkg — estándar OGC basado en SQLite).

### 3.2. Modelo Ráster

Representa el espacio geográfico como una **malla regular de celdas (píxeles)**, donde cada celda almacena un valor numérico que representa una característica del terreno en esa ubicación. No trabaja con objetos geométricos discretos, sino con superficies continuas.

*   **Aplicaciones principales:** Modelos Digitales del Terreno (MDT) y Modelos Digitales de Elevación (MDE), ortofotografías aéreas y satelitales, imágenes de teledetección, mapas de temperaturas, mapas de pendientes, análisis de visibilidad y modelos de propagación de incendios.
*   **Característica clave:** La **resolución espacial** (tamaño de la celda) determina el nivel de detalle. Una resolución de 1 metro significa que cada píxel representa 1 m² de terreno. A mayor resolución, mayor detalle pero también mayor volumen de datos.
*   **Formatos habituales:** GeoTIFF, ECW, MrSID, JPEG2000.

### 3.3. Comparativa Vectorial vs. Ráster

| Característica | Vectorial | Ráster |
|----------------|-----------|--------|
| Representación | Objetos discretos (puntos, líneas, polígonos) | Malla continua de celdas |
| Precisión geométrica | Alta | Limitada por la resolución |
| Atributos asociados | Ricos y múltiples por entidad | Un valor por celda |
| Volumen de datos | Eficiente para entidades discretas | Puede ser muy voluminoso |
| Análisis topológico | Excelente (adyacencia, conectividad) | Limitado |
| Análisis de superficies | Limitado | Excelente (pendientes, cuencas) |
| Uso típico | Catastro, redes, urbanismo | Ortofotos, MDT, teledetección |

## 4. Sistemas de Coordenadas y Proyecciones

Toda información geográfica debe estar referenciada a un **sistema de coordenadas** que permita posicionar inequívocamente los datos sobre la superficie terrestre. Los más relevantes son:

*   **WGS84 (EPSG:4326):** Sistema geodésico mundial utilizado por el GPS. Coordenadas expresadas en latitud y longitud (grados decimales). Estándar global.
*   **ETRS89 (EPSG:4258):** Sistema de referencia oficial en Europa, prácticamente coincidente con WGS84 para fines prácticos.
*   **UTM (Universal Transverse Mercator):** Proyección que divide la Tierra en 60 husos, expresando las coordenadas en metros. España peninsular se ubica en los husos 29, 30 y 31. Alicante se encuentra en el huso 30 (EPSG:25830 para ETRS89/UTM zona 30N). Es el sistema habitual para cartografía de detalle en la Administración Pública española.

La homogeneidad del sistema de coordenadas es una condición indispensable para que las capas de información se superpongan correctamente. La mezcla de sistemas de referencia sin la debida transformación es una de las fuentes de error más frecuentes en proyectos GIS.

## 5. Procesos de Carga de Información y Controles de Calidad

### 5.1. Procesos ETL Espacial

La carga de datos en un SIG no se realiza manualmente, sino mediante procesos **ETL (Extract, Transform, Load)** especializados que adaptan los datos desde sus fuentes de origen al modelo de datos del SIG:

*   **Extract (Extracción):** Obtención de los datos desde las fuentes originales: ficheros Shapefile, archivos CAD (DXF/DWG), bases de datos alfanuméricas, servicios web WFS, ficheros GPS, levantamientos topográficos, ortofotos.
*   **Transform (Transformación):** Conversión de sistemas de coordenadas (reproyección), homogeneización de formatos, normalización de nomenclaturas, enriquecimiento con atributos de fuentes complementarias.
*   **Load (Carga):** Inserción de los datos procesados en la base de datos espacial de destino (PostGIS, Oracle Spatial).

**Herramientas ETL espaciales:** **FME (Feature Manipulation Engine)** de Safe Software es la herramienta líder del mercado. También se utilizan **ogr2ogr** (de la librería GDAL/OGR, libre), **GeoKettle** y los módulos de importación de QGIS.

### 5.2. Controles de Calidad Topológica

A diferencia de los datos alfanuméricos, donde un error tipográfico es fácilmente detectable, los errores en datos espaciales pueden ser sutiles e imperceptibles visualmente pero con consecuencias graves:

*   **Overshoots:** Líneas que se extienden más allá del vértice de intersección, creando falsos callejones sin salida.
*   **Undershoots:** Líneas que no llegan a conectarse con la entidad adyacente, rompiendo la continuidad de la red viaria.
*   **Polígonos no cerrados (Gaps):** Polígonos cuyo vértice final no coincide exactamente con el inicial, provocando que la entidad carezca de área calculable y arruine los cálculos de superficie para la liquidación de impuestos.
*   **Superposiciones (Overlaps):** Dos polígonos que comparten una superficie que debería pertenecer a uno solo, duplicando el área computable.
*   **Slivers (Astillas):** Polígonos microscópicos espurios creados en las intersecciones de capas provenientes de fuentes diferentes.

Las herramientas GIS incluyen **validadores topológicos** (Topology Checker en QGIS, herramientas de topología en ArcGIS) que detectan y, en muchos casos, corrigen automáticamente estos errores antes de que los datos se integren en el sistema de producción.

## 6. Geoportales e Infraestructuras de Datos Espaciales (IDE)

### 6.1. Concepto de Geoportal

Un **Geoportal** es un sitio web que proporciona acceso centralizado a datos, servicios y metadatos geográficos mediante una interfaz de usuario intuitiva. Permite a los ciudadanos, técnicos y otras administraciones consultar, visualizar y descargar información geográfica sin necesidad de software especializado.

### 6.2. Estándares OGC (Open Geospatial Consortium)

Los geoportales interoperables se basan en los estándares de servicios web definidos por el **OGC**:

*   **WMS (Web Map Service):** Sirve mapas como imágenes renderizadas (PNG, JPEG). El usuario visualiza el mapa pero no accede a los datos vectoriales subyacentes.
*   **WFS (Web Feature Service):** Sirve los datos vectoriales en su formato original (GML, GeoJSON), permitiendo al cliente descargar, consultar y analizar las geometrías y sus atributos.
*   **WMTS (Web Map Tile Service):** Versión optimizada de WMS que sirve mapas pre-renderizados en teselas (tiles) para una visualización rápida y fluida.
*   **WCS (Web Coverage Service):** Equivalente al WFS pero para datos ráster (modelos digitales del terreno, ortofotos).
*   **CSW (Catalogue Service for the Web):** Permite buscar y consultar los metadatos de los recursos geográficos disponibles en un catálogo.

### 6.3. Infraestructuras de Datos Espaciales (IDE) en España

Las IDE constituyen el marco institucional, tecnológico y normativo para compartir información geográfica entre administraciones:

*   **IDEE (Infraestructura de Datos Espaciales de España):** Portal nacional que integra los servicios geográficos de todas las administraciones españolas, conforme a la Directiva INSPIRE de la Unión Europea (Directiva 2007/2/CE).
*   **IDE de la Comunitat Valenciana (IDEV):** IDE autonómica que integra los datos geográficos de la Generalitat Valenciana y los municipios de su ámbito territorial.
*   **Sede Electrónica del Catastro:** Geoportal del Catastro que permite consultar parcelas catastrales, valores de referencia y cartografía catastral mediante servicios WMS y WFS.
*   **Instituto Geográfico Nacional (IGN):** Publica cartografía topográfica, ortofotos del PNOA (Plan Nacional de Ortofotografía Aérea) y modelos digitales del terreno mediante servicios estandarizados.

### 6.4. Directiva INSPIRE

La **Directiva 2007/2/CE (INSPIRE)** de la Unión Europea establece la obligación de las administraciones públicas europeas de publicar su información geográfica mediante servicios interoperables, utilizando estándares OGC y metadatos conformes a la norma ISO 19115. Su transposición en España se realizó mediante la Ley 14/2010 (LISIGE - Ley sobre las Infraestructuras y los Servicios de Información Geográfica en España).

## 7. Aplicaciones Municipales de los SIG

### 7.1. Urbanismo y Catastro

*   Gestión del planeamiento urbanístico: clasificación y calificación del suelo (urbano, urbanizable, no urbanizable).
*   Consulta de parcelas catastrales con referencia catastral, superficie, titularidad y valor catastral integrados.
*   Cálculo automatizado del IBI por parcela y manzana urbana.
*   Gestión de licencias de obras con geolocalización del solar.

### 7.2. Gestión de Infraestructuras y Servicios Urbanos

*   Inventario georreferenciado de redes de abastecimiento de agua, saneamiento, alumbrado público y fibra óptica.
*   Planificación de itinerarios de recogida de residuos con optimización de rutas.
*   Gestión del arbolado urbano y espacios verdes.

### 7.3. Protección Civil y Emergencias

*   Modelado de zonas inundables y evaluación de riesgos de avenida.
*   Análisis de zonas de riesgo de incendio forestal en la interfaz urbano-forestal.
*   Planificación de rutas de evacuación y ubicación óptima de recursos de emergencia.

### 7.4. Movilidad y Tráfico

*   Análisis de redes viarias y cálculo de rutas óptimas para servicios de emergencia (policía, bomberos, ambulancias).
*   Gestión de flotas municipales con seguimiento GPS en tiempo real.
*   Planificación del transporte público con análisis de cobertura poblacional.

### 7.5. Medio Ambiente

*   Monitorización de la calidad del aire con sensores georreferenciados.
*   Control de vertidos y calidad de aguas mediante SIG asociados a la red de saneamiento.
*   Estudios de impacto ambiental con análisis de visibilidad, ruido y afección a espacios protegidos.

## 8. Conclusión

Los Sistemas de Información Geográfica constituyen una herramienta imprescindible para la gestión territorial municipal moderna. Su capacidad para integrar la dimensión espacial con los datos alfanuméricos de las bases de datos relacionales permite a las Administraciones Públicas tomar decisiones fundamentadas en análisis geográficos rigurosos, desde la planificación urbanística hasta la gestión de emergencias.

La interoperabilidad, garantizada por los estándares del OGC (WMS, WFS, WMTS) y el marco normativo de las Infraestructuras de Datos Espaciales (INSPIRE, IDEE, LISIGE), asegura que la información geográfica generada por las distintas administraciones pueda compartirse y reutilizarse de forma transparente, evitando duplicidades y maximizando el valor de los datos públicos.

Los procesos de carga ETL espacial y los controles de calidad topológica garantizan la fiabilidad de los datos, mientras que los geoportales acercan la información geográfica al ciudadano, cumpliendo los principios de transparencia y acceso a la información pública. El dominio de estas tecnologías, modelos de datos y estándares resulta una competencia esencial para los profesionales de las tecnologías de la información al servicio de la Administración Local.
