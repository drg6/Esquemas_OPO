# Tema 11.- Sistema de Información Geográfica (SIG) y Aplicaciones Municipales.

## 1. Introducción y Definición Formal
* **Limitación del SGBDR tradicional:** Resuelve consultas alfanuméricas (*quién, cuánto, cuándo*), pero carece de capacidad nativa para resolver relaciones topológicas y espaciales (*dónde, adyacencia, rutas, intersección*).
* **Definición (NCGIA):** Sistema integrado de **hardware, software, datos geográficos, procedimientos y personal** diseñado para capturar, almacenar, analizar, modelizar y visualizar información **georreferenciada** (vinculada a coordenadas terrestres).

## 2. Componentes Arquitectónicos de un SIG
1. **Hardware:** Servidores CPD de alta capacidad, estaciones gráficas, plotters gran formato (A0/A1), receptores GPS/GNSS submétricos y drones con sensores LIDAR.
2. **Software (Pila tecnológica en 4 capas):**
   * **SGBD Espacial:** Oracle Spatial, PostgreSQL/**PostGIS** (libre), SQL Server Spatial.
   * **GIS de Escritorio (*Desktop*):** **QGIS** y **gvSIG** (libres; origen valenciano), **ArcGIS Pro** (Esri, propietario).
   * **Servidores de Mapas:** **GeoServer**, **MapServer** (libres) y **ArcGIS Server**.
   * **Librerías Web (Visores):** **Leaflet**, **OpenLayers**, **MapLibre** y ArcGIS JS API.
3. **Datos Geográficos (Dualidad inseparable):**
   * **Componente Espacial (Geometría):** Posición y forma (puntos, líneas, polígonos).
   * **Componente Alfanumérica (Atributos):** Datos descriptivos asociados (titular, referencia catastral, estado).
4. **Procedimientos y Personal:** Normas de captura/actualización, control topológico y técnicos especialistas (geomáticos, analistas GIS, DBAs).

## 3. Modelos de Representación Espacial: Vectorial vs. Ráster
* **3.1. Modelo Vectorial (Objetos discretos mediante coordenadas $X, Y, Z$):**
  * **Primitivas:** **Punto (0D)** (farola, semáforo, contenedor), **Línea/Polilínea (1D)** (eje viario, tubería, ruta de autobús) y **Polígono (2D)** (parcela catastral, edificio, distrito).
  * **Ventajas:** Alta precisión geométrica, riqueza de atributos por entidad y soporte de **análisis topológico** (conectividad, adyacencia).
  * **Formatos:** **Shapefile (`.shp`)**, **GeoPackage (`.gpkg`**, estándar OGC sobre SQLite), **GeoJSON**, **GML** (OGC) y **KML**.
* **3.2. Modelo Ráster (Malla continua de celdas/píxeles):**
  * **Estructura:** Matriz regular donde cada píxel almacena un valor numérico (altitud, temperatura, reflectancia). Su nivel de detalle depende de la **resolución espacial** (tamaño de celda en terreno).
  * **Usos:** Ortofotos (PNOA), Modelos Digitales del Terreno/Elevación (**MDT/MDE**), cálculo de pendientes y cuencas visuales.
  * **Formatos:** **GeoTIFF**, **ECW**, **JPEG2000**, **MrSID**.

## 4. Sistemas de Referencia Espacial y Coordenadas (CRS)
* **WGS84 (`EPSG:4326`):** Sistema geodésico global (GPS), coordenadas geográficas en latitud/longitud.
* **ETRS89 (`EPSG:4258`):** Sistema de referencia geodésico oficial en Europa y España (Real Decreto 1071/2007).
* **Proyección UTM (Coordenadas planas en metros):** España peninsular abarca los husos 29, 30 y 31.
  * *Detalle clave municipal:* **Alicante** se ubica en el huso 30 norte bajo el código **`EPSG:25830` (ETRS89 / UTM zone 30N)**. Mezclar capas sin reproyectar a un CRS común provoca desplazamientos métricos graves.

## 5. Carga de Información (ETL Espacial) y Controles de Calidad
* **5.1. Procesos ETL (*Extract, Transform, Load*) Espacial:**
  * **Extracción:** Lectura desde CAD (`DWG`/`DXF`), Shapefiles, sensores GPS o servicios WFS.
  * **Transformación:** Reproyección de coordenadas (ej. de `EPSG:4326` a `EPSG:25830`), limpieza geométrica y cruce alfanumérico.
  * **Carga:** Inserción masiva en Oracle Spatial o PostGIS.
  * **Herramientas líderes:** **FME** (*Feature Manipulation Engine*, propietario) y **GDAL/OGR (`ogr2ogr`)** (software libre).
* **5.2. Controles de Calidad Topológica (Errores críticos a depurar):**
  * **Overshoots:** Línea que sobrepasa el nodo de intersección (falso tramo).
  * **Undershoots (Dangles):** Línea que no llega a tocar el eje adyacente (rompe el cálculo de rutas en redes viarias o de agua).
  * **Polígonos no cerrados (*Gaps*):** El vértice final no cierra con el inicial, impidiendo calcular superficies (crítico en liquidaciones de IBI o plusvalías).
  * **Overlaps (Solapes):** Superposición ilegal de dos parcelas (doble cómputo de suelo).
  * **Slivers (Astillas):** Micro-polígonos residuales generados al cruzar capas mal alineadas.

## 6. Geoportales, Estándares OGC e IDE
* **6.1. Estándares del OGC (*Open Geospatial Consortium*):**
  * **WMS (*Web Map Service*):** Devuelve una **imagen renderizada** (PNG/JPEG) del mapa; no permite editar geometrías.
  * **WMTS (*Web Map Tile Service*):** Sirve teselas (*tiles*) pre-generadas en caché para navegación web ultrarrápida.
  * **WFS (*Web Feature Service*):** Devuelve **datos vectoriales puros** (GML/GeoJSON) permitiendo descarga, consulta espacial y edición (**WFS-T**).
  * **WCS (*Web Coverage Service*):** Descarga de datos **ráster** brutos (MDT, imágenes).
  * **CSW (*Catalogue Service for the Web*):** Búsqueda y publicación de **metadatos** (estándar **ISO 19115**).
* **6.2. Marco Normativo e Infraestructuras de Datos Espaciales (IDE):**
  * **Normativa:** **Directiva 2007/2/CE (INSPIRE)** transpuesta en España mediante la **Ley 14/2010 (LISIGE)**, que obliga a las AAPP a publicar cartografía interoperable.
  * **Nodos oficiales:** **IDEE** (Estatal), **IDEV** (*Institut Cartogràfic Valencià* / Generalitat Valenciana), **Sede Electrónica del Catastro** e **IGN (PNOA)**.

## 7. Aplicaciones Municipales Clave
* **Urbanismo y Hacienda Local:** Planeamiento (PGOU, calificación del suelo), cruce con Catastro para detección de omisiones tributarias (IBI, vados, ocupación de vía pública) y geolocalización de licencias de obra.
* **Servicios Urbanos e Infraestructuras:** Inventario en red de alumbrado, saneamiento, agua potable y fibra municipal; optimización de rutas de recogida de residuos (RSU).
* **Seguridad, Movilidad y Emergencias:** Mapas de riesgo de inundación (PATRICOVA) e incendios, cálculo de rutas óptimas para bomberos/policía local y seguimiento de flotas por GPS.
* **Medio Ambiente y Smart City:** Mapas estratégicos de ruido, sensores de calidad del aire/ZBE y censo georreferenciado del arbolado municipal.

## 8. Conclusión
Los SIG transforman la gestión municipal al unir la semántica administrativa con la realidad física del territorio. Apoyados en bases de datos espaciales, procesos ETL con validación topológica estricta y estándares abiertos del OGC (WMS/WFS) bajo el paraguas de la Directiva INSPIRE y la LISIGE, los ayuntamientos logran interoperabilidad plena con Catastro y la IDEV, optimizando la recaudación, el urbanismo y los servicios públicos al ciudadano a través de sus Geoportales.

-----------------

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
