# Tema 11.- Sistema de información Geográfica y sus aplicaciones municipales. Funcionalidades y tecnologías aplicables. Procesos de carga de la información y controles de calidad. Geoportales.

## 1. Introducción

**Sistemas de Información Geográfica (SIG o GIS - Geographic Information Systems)**: sistemas integrados para gestionar información **georreferenciada**, vinculada a coordenadas de la superficie terrestre.
SIG integran datos urbanísticos, catastrales, medioambientales, de infraestructuras y de servicios públicos.

## 2. Definición 

**Sistema integrado de hardware, software, datos geográficos y personal**, para CAAAVM -capturar, almacenar, actualizar, manipular, analizar y visualizar- información geográficamente referenciada. 

### 2.2. Componentes arquitectónicos (5 pilares)

1.  **Hardware (Infraestructura física):** CPD almacenamiento de terabytes de cartografía y ortofotos, estaciones de trabajo con capacidad gráfica avanzada, periféricos especializados (plotters, escáneres de planos, dispositivos GPS de precisión, drones para captura de datos LIDAR - Light Detection and Ranging) y redes de alta velocidad.
2.  **Software (Componentes lógicos):**
    *   **SGBD con extensión espacial:** Almacena y gestiona datos geográficos. **PostGIS**, **Oracle Spatial** y **Microsoft SQL Server Spatial**.
    *   **Clientes de escritorio (Desktop GIS):** Análisis y edición cartográfica **ArcGIS Pro**, **QGIS** y **gvSIG**.
    *   **Servidores de mapas:** Publican los datos geográficos. **GeoServer**, **MapServer**, y **ArcGIS Server**.
    *   **Bibliotecas de visualización web:** Integran mapas en aplicaciones web: **Leaflet**, **OpenLayers**, **MapLibre** y la API de ArcGIS.
3.  **Datos geográficos (El componente más crítico y costoso):** 
    *   **Componente espacial (geometría):** Puntos, líneas, polígonos definidos mediante coordenadas en un sistema de referencia determinado.
    *   **Componente alfanumérica (atributos):** Información descriptiva asociada: titular de la parcela, valor catastral, año de construcción, referencia catastral, etc.
4.  **Procedimientos y metodologías:** Protocolos formales regular gestión de datos espaciales.
5.  **Personal (Capital humano):** Ingenieros en geomática, cartógrafos, analistas GIS, DBA espaciales y desarrolladores especializados.

## 3. Modelos de Representación Espacial: Vectorial y Ráster

### 3.1. Modelo Vectorial

Representa las entidades geográficas como objetos geométricos definidos por coordenadas (X, Y, Z). Cartografía urbana y catastral.

1.  **Punto (dimensión 0D):** ubicación de semáforo, farola, boca de riego, contenedor de residuos...
2.  **Línea o Polilínea (dimensión 1D):** Calles, tuberías, autobús, cauces de ríos, tendidos eléctricos...
3.  **Polígono (dimensión 2D):** parcelas catastrales, zonas verdes, edificios...

**Formatos vectoriales habituales:** Shapefile (.shp), GeoJSON, GML (Geography Markup Language, estándar OGC), KML (Google Earth), GeoPackage (.gpkg).

### 3.2. Modelo Ráster

Se Representa como **malla regular de celdas (píxeles)**, donde cada celda almacena un valor numérico que representa una característica del terreno en esa ubicación. 

*   **Aplicaciones principales:** Modelos Digitales del Terreno (MDT), ortofotos , teledetección, mapas de temperaturas o pendientes, modelos de propagación de incendios...
*   **Característica clave:** La **resolución espacial** (tamaño de la celda) determina el nivel de detalle. 
*   **Formatos habituales:** GeoTIFF, ECW, MrSID, JPEG2000.

### 3.3. Comparativa Vectorial vs. Ráster

Vectorial: representa puntos, líneas y polígonos. Tiene alta precisión, muchos atributos y es eficiente para entidades discretas. Ideal para catastro, redes y urbanismo.
Ráster: representa el espacio mediante una malla de celdas, cada una con un valor. Depende de la resolución, puede ocupar mucho espacio y es ideal para superficies y análisis del terreno.

## 4. Sistemas de Coordenadas y Proyecciones

*   **WGS84 (EPSG:4326):** Sistema geodésico mundial. GPS. Coordenadas expresadas en latitud y longitud. Estándar global.
*   **ETRS89 (EPSG:4258):** Oficial en Europa, coincidente con WGS84.
*   **UTM (Universal Transverse Mercator):** Uso cartografía de detalle en la Administración Pública española. 60 Husos (Huso 30N (EPSG:25830))

## 5. Procesos de Carga de Información y Controles de Calidad

### 5.1. Procesos ETL Espacial

Uso en la carga de datos 

*   **Extract (Extracción):** Obtención de los datos desde las fuentes originales: Shapefile, CAD, GPS, ortofotos...
*   **Transform (Transformación):** Homogeneización de coordenadas (reproyección) y formatos.
*   **Load (Carga):** Inserción en BD espacial.

**Herramientas ETL espaciales:** FME (Feature Manipulation Engine), ogr2ogr, GeoKettle y QGIS.

### 5.2. Controles de Calidad Topológica

Errores sutiles con consecuencias graves. 

*   **Overshoots:** Líneas que se extienden más allá del vértice de intersección.
*   **Undershoots:** Líneas que no llegan a conectarse.
*   **Polígonos no cerrados (Gaps):** Provoca que carezca de área calculable.
*   **Superposiciones (Overlaps):** Duplicando el área computable.
*   **Slivers (Astillas):** Polígonos microscópicos de intersecciones de capas.

**Validadores topológicos** detectan y corrigen automáticamente estos errores.

## 6. Geoportales e Infraestructuras de Datos Espaciales (IDE)

### 6.1. Concepto de Geoportal

**Geoportal** sitio web de acceso centralizado a datos, servicios y metadatos geográficos. 

### 6.2. Estándares OGC (Open Geospatial Consortium)
*   **WMS (Web Map Service):** Imágenes renderizadas (PNG, JPEG) sin datos vectoriales. Solo foto
*   **WFS (Web Feature Service):** Datos vectoriales original (GML, GeoJSON). Interactuar
*   **WMTS (Web Map Tile Service):** Mapas pre-renderizados en teselas (tiles).
*   **WCS (Web Coverage Service):** WFS para datos ráster.
*   **CSW (Catalogue Service for the Web):** Consultar los metadatos de un catálogo.

### 6.3. Infraestructuras de Datos Espaciales (IDE) en España

Las IDE constituyen el marco institucional, tecnológico y normativo para compartir información geográfica entre administraciones:

*   **IDEE (Infraestructura de Datos Espaciales de España):** Conforme a la Directiva INSPIRE de la Unión Europea (Directiva 2007/2/CE).
*   **IDE de la Comunitat Valenciana (IDEV):** 
*   **Sede Electrónica del Catastro:** Servicios WMS y WFS.
*   **Instituto Geográfico Nacional (IGN):** Cartografía topográfica.

### 6.4. Directiva INSPIRE

La **Directiva 2007/2/CE (INSPIRE)** de la Unión Europea establece la obligación AP europeas de publicar su información geográfica mediante servicios interoperables, utilizando estándares OGC y metadatos conformes a la norma ISO 19115. Transposicion en España Ley 14/2010 (LISIGE - Ley sobre las Infraestructuras y los Servicios de Información Geográfica en España).

## 7. Aplicaciones Municipales de los SIG

*   **Urbanismo y Catastro:** Clasificación y calificación del suelo, consulta de parcelas, Cálculo IBI, Gestión de licencias de obras.
*   **Gestión de Infraestructuras y Servicios Urbanos:** Inventario georreferenciado de redes de abastecimiento de agua, saneamiento, alumbrado público y fibra óptica. Rutas de recogidas de residuos. Arbolado y espacios verdes.
*   **Protección Civil y Emergencias:** Zonas inundables o de riesgo de incendio forestal, rutas de evacuación.
*   **Movilidad y Tráfico:**Rutas óptimas para servicios de emergencia (policía, bomberos, ambulancias), flotas municipales en tiempo real, transporte público.
 *   **Medio Ambiente:** Calidad del aire, Control de vertidos y calidad de aguas.

## 8. Conclusión

SIG fundamental gestión territorial municipal. Tomar decisiones para planificación urbanística hasta la gestión de emergencias.

La interoperabilidad -> Estándares del OGC (WMS, WFS, WMTS) y (INSPIRE, IDEE, LISIGE)
Geoportales -> transparencia y acceso a la información pública. 
