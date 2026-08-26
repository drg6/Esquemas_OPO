# Tema 14.- Oracle Spatial and Graph: Sistemas de referencia espacial y consultas espaciales.

## 1. Introducción

**Sistemas de Referencia Espacial (SRS)**, dotan de significado geográfico a las coordenadas almacenadas, y el **motor de consultas espaciales**, permiten responder a preguntas geoespaciales complejas mediante SQL.

## 2. Sistemas de Referencia Espacial (SRS)

### 2.1. Concepto y necesidad

Un **Sistema de Referencia Espacial (SRS)** define cómo las coordenadas numéricas `SDO_GEOMETRY` se relacionan con posiciones reales sobre la superficie terrestre. 

*   El **datum** (modelo matemático de la forma de la Tierra): WGS84, ETRS89, ED50.
*   La **proyección cartográfica** (método para representar la esfera en un plano): UTM, Lambert, Mercator.
*   Las **unidades de medida**: grados decimales (para sistemas geográficos) o metros (para sistemas proyectados).

### 2.2. Sistemas geográficos vs. proyectados

*   1. Geográfico → superficie curva 
**Coordenadas:** Latitud / Longitud → grados
**Superficie:** Esférica o elipsoidal
**Distancias:** En grados → hay que convertir
**Áreas:** Más distorsionadas, especialmente en latitudes extremas
**Ejemplo:** WGS84 → SRID 4326
**Uso:** GPS, mapas globales

*   2. Proyectado → superficie plana 
**Coordenadas:** X / Y → metros
**Superficie:** Plana
**Distancias:** Directamente en metros
**Áreas:** Más precisas dentro de la zona de proyección
**Ejemplo:** ETRS89/UTM30N → SRID 25830
**Uso:** Catastro, urbanismo, topografía

SRID (Spatial Reference System Identifier): Es un número único que identifica un sistema de referencia espacial 

### 2.3. Sistemas más relevantes para España

*   **WGS84 (EPSG:4326):** Sistema de referencia global, GPS. Latitud (Norte/Sur) y longitud (Este/Oeste). 
*   **ETRS89 (EPSG:4258):** Sistema de Referencia Terrestre Europeo, oficial normativa española (Real Decreto 1071/2007) y europeo (Directiva INSPIRE). Coincidente con WGS84 (diferencias de centímetros). **ETRS89 / UTM zona 30N (EPSG:25830):** España peninsular.

### 2.4. Gestión de SRS en Oracle Spatial

Oracle Spatial almacena sistemas de referencia en la tabla del sistema `SDO_COORD_REF_SYS`. Sistema Referencia -> Atributo `SDO_SRID` del tipo `SDO_GEOMETRY`.

**Transformación de coordenadas entre sistemas:** SDO_CS.TRANSFORM(forma, 4326) -> Convertir una parcela de ETRS89/UTM30N (25830) a WGS84 (4326)
Aplica la transformación geodésica correspondiente, convirtiendo las coordenadas de un SRS a otro con la precisión adecuada.

## 3. Operadores Espaciales (Spatial Operators)

### 3.1. Concepto y requisitos

Los **operadores espaciales** uso cláusula `WHERE` filtro registros basándose en relaciones geoespaciales entre geometrías. Devuelven `TRUE` o `FALSE`.
**Exigen obligatoriamente** que la columna espacial tenga creado un **índice R-Tree**. Sin índice, Oracle devolverá un error. Lo usan para acelerar la búsqueda.

2 FASES (Primario/Secundario)

### 3.2. Operadores principales

#### SDO_ANYINTERACT

Determina si dos geometrías se tocan, se cruzan, se superponen o una contiene a la otra). Más usado.
Ej. WHERE SDO_ANYINTERACT(p.forma, z.forma) = 'TRUE';

#### SDO_CONTAINS

Devuelve `TRUE` si la primera geometría contiene completamente a la segunda.
Ej. WHERE SDO_CONTAINS(p.forma, z.forma) = 'TRUE';

#### SDO_INSIDE

Inverso de `SDO_CONTAINS`. Devuelve `TRUE` si la primera geometría está completamente dentro de la segunda.

#### SDO_WITHIN_DISTANCE

Identifica las geometrías que se encuentran dentro de una distancia especificada respecto a una geometría de referencia.
Ej. Farolas a menos de 200 metros de un incidente -> SDO_WITHIN_DISTANCE(f.ubicacion, p.punto, 'distance=200 unit=METER') = 'TRUE'

#### SDO_NN (Nearest Neighbor)

Geometrías más cercanas a una geometría de referencia, ordenadas por proximidad (**vecino más cercano**).
Ej. Los 5 hospitales más cercanos a un punto de emergencia -> SELECT h.nombre_hospital, SDO_NN_DISTANCE(1) AS distancia_metros FROM... WHERE SDO_NN(h.ubicacion, p.punto, 'sdo_num_res=5 unit=METER', 1) = 'TRUE';
Parámetro 1 auxiliar -> Conecta el 1 del SDO_NN_DISTANCE con el 1 del SDO_NN, resuelve la ambigüedad en consultas con múltiples operadores de vecino más cercano

#### SDO_RELATE

Relación topológica según el modelo **DE-9IM** (Dimensionally Extended 9-Intersection Model):
Ej. Parcelas que comparten borde con la parcela 100 (TOUCH) -> WHERE SDO_RELATE(p1.forma, p2.forma, 'mask=TOUCH') = 'TRUE';

**Valores de mask:** `ANYINTERACT`, `CONTAINS`, `INSIDE`, `TOUCH`, `OVERLAPBDYINTERSECT`, `OVERLAPBDYDISJOINT`, `EQUAL`, `COVEREDBY`, `COVERS`.

## 4. Funciones Espaciales (Spatial Functions)

### 4.1. Concepto

Devuelven valores numéricos o geometrías como resultado de cálculos geométricos. **No requieren obligatoriamente un índice R-Tree** -> CPU -> Combinar siempre con WHERE operador

### 4.2. Funciones de medición

*   **`SDO_GEOM.SDO_AREA(geometry, tolerance [, unit])`:** Calcula el área de un polígono.  
*   **`SDO_GEOM.SDO_LENGTH(geometry, tolerance [, unit])`:** Calcula el perímetro de un polígono o la longitud de una línea.
*   **`SDO_GEOM.SDO_DISTANCE(geom1, geom2, tolerance [, unit])`:** Calcula la distancia más corta entre dos geometrías.
*   **`SDO_GEOM.SDO_CENTROID(geometry, tolerance)`:** Calcula el centroide (punto central geométrico) de un polígono.

### 4.3. Funciones de análisis geométrico

*   **`SDO_GEOM.SDO_INTERSECTION(geom1, geom2, tolerance)`:** Devuelve la geometría resultante de la intersección de dos geometrías.
*   **`SDO_GEOM.SDO_UNION(geom1, geom2, tolerance)`:** Devuelve la unión geométrica de dos geometrías.
*   **`SDO_GEOM.SDO_DIFFERENCE(geom1, geom2, tolerance)`:** Devuelve la diferencia geométrica (lo que está en geom1 pero no en geom2).
*   **`SDO_GEOM.SDO_BUFFER(geometry, distance, tolerance [, unit])`:** Genera un polígono envolvente (buffer) a una distancia especificada alrededor de una geometría. Ej. Zona de afección de 500 metros alrededor de una carretera

### 4.4. Funciones de agregación espacial

*   **`SDO_AGGR_UNION(geometry_column)`:** Función de agregación que fusiona múltiples geometrías en una sola. 
Ej. Unir todos los distritos en una geometría del municipio completo -> SELECT SDO_AGGR_UNION(SDOAGGRTYPE(geometria, 0.005)) AS limite_municipal

## 5. Modelo de Relaciones Topológicas: DE-9IM

Oracle Spatial implementa el modelo **DE-9IM (Dimensionally Extended 9-Intersection Model)** del OGC, que clasificarelaciones espaciales entre dos geometrías. 

**DISJOINT**: Las geometrías no tienen ningún punto en común 
**TOUCH**: Las geometrías comparten solo puntos de sus bordes
**OVERLAPBDYINTERSECT**: Las geometrías se superponen parcialmente 
**EQUAL**: Las geometrías son idénticas 
**CONTAINS**: La primera geometría contiene completamente a la segunda 
**INSIDE**: La primera geometría está completamente dentro de la segunda 
**COVERS**: Similar a CONTAINS, incluyendo bordes compartidos 
**COVEREDBY**: Similar a INSIDE, incluyendo bordes compartidos 

## 6. Conclusión

La correcta asignación del SRS (especialmente ETRS89/UTM zona 30N con SRID 25830 para la Administración Pública española) y la capacidad de transformación entre sistemas mediante `SDO_CS.TRANSFORM` garantizan la coherencia y la interoperabilidad de los datos geográficos.

Los operadores espaciales y las funciones espaciales permite a las Administraciones Públicas ejecutar consultas territoriales complejas directamente en SQL, integrando la dimensión espacial en sus procesos de gestión urbanística, tributaria, medioambiental y de protección civil.
