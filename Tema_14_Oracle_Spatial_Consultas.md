# Tema 14.- Oracle Spatial and Graph: Sistemas de referencia espacial y consultas espaciales.

## 1. Introducción

Los Temas 12 y 13 han establecido los cimientos del ecosistema Oracle Spatial: el modelo de datos Objeto-Relacional con el tipo `SDO_GEOMETRY`, la indexación R-Tree y las vistas de metadatos. Este tema completa la trilogía abordando dos aspectos operativos esenciales: los **Sistemas de Referencia Espacial (SRS)**, que dotan de significado geográfico real a las coordenadas almacenadas, y el **motor de consultas espaciales**, compuesto por operadores y funciones que permiten responder a preguntas geoespaciales complejas directamente mediante SQL.

Sin un sistema de referencia correctamente asignado, las coordenadas (145.2, 567.8) carecen de significado: no es posible saber si representan metros, grados o unidades arbitrarias, ni a qué punto de la Tierra corresponden. Y sin los operadores y funciones espaciales, la potencia del índice R-Tree y del modelo SDO_GEOMETRY quedaría desaprovechada.

## 2. Sistemas de Referencia Espacial (SRS)

### 2.1. Concepto y necesidad

Un **Sistema de Referencia Espacial (SRS)** define cómo las coordenadas numéricas almacenadas en `SDO_GEOMETRY` se relacionan con posiciones reales sobre la superficie terrestre. El SRS determina:

*   El **datum** (modelo matemático de la forma de la Tierra): WGS84, ETRS89, ED50.
*   La **proyección cartográfica** (método para representar la esfera en un plano): UTM, Lambert, Mercator.
*   Las **unidades de medida**: grados decimales (para sistemas geográficos) o metros (para sistemas proyectados).

### 2.2. Sistemas geográficos vs. proyectados

| Característica | Geográfico (esférico) | Proyectado (plano) |
|----------------|----------------------|-------------------|
| Coordenadas | Latitud/Longitud (grados) | X/Y (metros) |
| Superficie | Esférica/elipsoidal | Plana |
| Distancias | En grados (requiere conversión) | Directamente en metros |
| Áreas | Distorsionadas en latitudes extremas | Precisas en la zona de proyección |
| Ejemplo | WGS84 (SRID 4326) | ETRS89/UTM30N (SRID 25830) |
| Uso típico | GPS, mapas globales | Catastro, urbanismo, topografía |

### 2.3. Sistemas más relevantes para España

*   **WGS84 (EPSG:4326):** Sistema de referencia global utilizado por el GPS. Coordenadas en grados de latitud (Norte/Sur) y longitud (Este/Oeste). Adecuado para aplicaciones globales y como sistema de intercambio.

*   **ETRS89 (EPSG:4258):** Sistema de Referencia Terrestre Europeo, adoptado como sistema oficial por la normativa española (Real Decreto 1071/2007) y europeo (Directiva INSPIRE). Prácticamente coincidente con WGS84 para fines prácticos, con diferencias del orden de centímetros.

*   **ETRS89 / UTM zona 30N (EPSG:25830):** Proyección UTM sobre el datum ETRS89 para el huso 30, que cubre la mayor parte de la España peninsular (incluidas las provincias de Alicante, Valencia, Madrid, Sevilla). Coordenadas en metros. Es el sistema estándar para la cartografía de detalle de las Administraciones Públicas españolas.

*   **ETRS89 / UTM zona 31N (EPSG:25831):** Para la franja oriental de Cataluña y las Islas Baleares.

*   **REGCAN95 / UTM zona 28N (EPSG:4083):** Sistema oficial para las Islas Canarias.

### 2.4. Gestión de SRS en Oracle Spatial

Oracle Spatial almacena el catálogo completo de sistemas de referencia en la tabla del sistema `SDO_COORD_REF_SYS`. La asignación del SRS a una geometría se realiza mediante el atributo `SDO_SRID` del tipo `SDO_GEOMETRY`.

**Transformación de coordenadas entre sistemas:**

```sql
-- Convertir una parcela de ETRS89/UTM30N (25830) a WGS84 (4326)
SELECT SDO_CS.TRANSFORM(forma, 4326) AS forma_wgs84
FROM PARCELAS
WHERE id_parcela = 1;
```

La función `SDO_CS.TRANSFORM` aplica la transformación geodésica correspondiente, convirtiendo las coordenadas de un SRS a otro con la precisión adecuada.

## 3. Operadores Espaciales (Spatial Operators)

### 3.1. Concepto y requisitos

Los **operadores espaciales** son predicados que se utilizan en la cláusula `WHERE` de las sentencias SQL para filtrar registros basándose en relaciones geoespaciales entre geometrías. A diferencia de las funciones espaciales (que devuelven valores), los operadores devuelven `TRUE` o `FALSE`.

**Requisito indispensable:** Los operadores espaciales **exigen obligatoriamente** que la columna espacial tenga creado un **índice R-Tree**. Sin índice, Oracle devolverá un error. Esto se debe a que los operadores utilizan el índice R-Tree en la fase de filtrado (Primary Filter) para acelerar la búsqueda.

### 3.2. Operadores principales

#### SDO_ANYINTERACT

Determina si dos geometrías tienen algún tipo de interacción espacial (se tocan, se cruzan, se superponen o una contiene a la otra). Es el operador más genérico y el más utilizado.

```sql
-- Parcelas que intersectan con una zona de inundabilidad
SELECT p.id_parcela, p.propietario
FROM PARCELAS p, ZONAS_RIESGO z
WHERE z.tipo = 'INUNDABLE'
AND SDO_ANYINTERACT(p.forma, z.forma) = 'TRUE';
```

#### SDO_CONTAINS

Devuelve `TRUE` si la primera geometría contiene completamente a la segunda.

```sql
-- ¿Qué distrito municipal contiene este punto (coordenada GPS)?
SELECT d.nombre_distrito
FROM DISTRITOS d
WHERE SDO_CONTAINS(d.geometria,
    SDO_GEOMETRY(2001, 25830, SDO_POINT_TYPE(724500, 4247800, NULL), NULL, NULL)
) = 'TRUE';
```

#### SDO_INSIDE

Inverso de `SDO_CONTAINS`. Devuelve `TRUE` si la primera geometría está completamente dentro de la segunda.

#### SDO_WITHIN_DISTANCE

Identifica las geometrías que se encuentran dentro de una distancia especificada respecto a una geometría de referencia.

```sql
-- Farolas a menos de 200 metros de un incidente
SELECT f.id_farola, f.tipo
FROM FAROLAS f
WHERE SDO_WITHIN_DISTANCE(f.ubicacion,
    SDO_GEOMETRY(2001, 25830, SDO_POINT_TYPE(724500, 4247800, NULL), NULL, NULL),
    'distance=200 unit=METER'
) = 'TRUE';
```

#### SDO_NN (Nearest Neighbor)

Encuentra las geometrías más cercanas a una geometría de referencia, ordenadas por proximidad. Es el operador de **vecino más cercano**.

```sql
-- Los 5 hospitales más cercanos a un punto de emergencia
SELECT h.nombre_hospital,
       SDO_NN_DISTANCE(1) AS distancia_metros
FROM HOSPITALES h
WHERE SDO_NN(h.ubicacion,
    SDO_GEOMETRY(2001, 25830, SDO_POINT_TYPE(724500, 4247800, NULL), NULL, NULL),
    'sdo_num_res=5 unit=METER', 1
) = 'TRUE'
ORDER BY distancia_metros;
```

#### SDO_RELATE

Operador genérico que permite especificar el tipo exacto de relación topológica según el modelo **DE-9IM** (Dimensionally Extended 9-Intersection Model):

```sql
-- Parcelas que comparten borde con la parcela 100 (TOUCH)
SELECT p.id_parcela
FROM PARCELAS p
WHERE SDO_RELATE(p.forma,
    (SELECT forma FROM PARCELAS WHERE id_parcela = 100),
    'mask=TOUCH'
) = 'TRUE';
```

**Valores de mask:** `ANYINTERACT`, `CONTAINS`, `INSIDE`, `TOUCH`, `OVERLAPBDYINTERSECT`, `OVERLAPBDYDISJOINT`, `EQUAL`, `COVEREDBY`, `COVERS`.

## 4. Funciones Espaciales (Spatial Functions)

### 4.1. Concepto

Las **funciones espaciales** devuelven valores numéricos o geometrías como resultado de cálculos geométricos. A diferencia de los operadores, **no requieren obligatoriamente un índice R-Tree** y pueden utilizarse en la cláusula `SELECT`, `WHERE` o en expresiones.

### 4.2. Funciones de medición

*   **`SDO_GEOM.SDO_AREA(geometry, tolerance [, unit])`:** Calcula el área de un polígono.
    ```sql
    SELECT id_parcela,
           SDO_GEOM.SDO_AREA(forma, 0.005, 'unit=SQ_M') AS superficie_m2
    FROM PARCELAS;
    ```

*   **`SDO_GEOM.SDO_LENGTH(geometry, tolerance [, unit])`:** Calcula el perímetro de un polígono o la longitud de una línea.

*   **`SDO_GEOM.SDO_DISTANCE(geom1, geom2, tolerance [, unit])`:** Calcula la distancia más corta entre dos geometrías.
    ```sql
    SELECT SDO_GEOM.SDO_DISTANCE(p.forma, h.ubicacion, 0.005, 'unit=METER') AS distancia
    FROM PARCELAS p, HOSPITALES h
    WHERE p.id_parcela = 1 AND h.id_hospital = 10;
    ```

*   **`SDO_GEOM.SDO_CENTROID(geometry, tolerance)`:** Calcula el centroide (punto central geométrico) de un polígono.

### 4.3. Funciones de análisis geométrico

*   **`SDO_GEOM.SDO_INTERSECTION(geom1, geom2, tolerance)`:** Devuelve la geometría resultante de la intersección de dos geometrías.

*   **`SDO_GEOM.SDO_UNION(geom1, geom2, tolerance)`:** Devuelve la unión geométrica de dos geometrías.

*   **`SDO_GEOM.SDO_DIFFERENCE(geom1, geom2, tolerance)`:** Devuelve la diferencia geométrica (lo que está en geom1 pero no en geom2).

*   **`SDO_GEOM.SDO_BUFFER(geometry, distance, tolerance [, unit])`:** Genera un polígono envolvente (buffer) a una distancia especificada alrededor de una geometría.
    ```sql
    -- Zona de afección de 500 metros alrededor de una carretera
    SELECT SDO_GEOM.SDO_BUFFER(c.trazado, 500, 0.005, 'unit=METER') AS zona_afeccion
    FROM CARRETERAS c
    WHERE c.nombre = 'AP-7';
    ```

### 4.4. Funciones de agregación espacial

*   **`SDO_AGGR_UNION(geometry_column)`:** Función de agregación que fusiona múltiples geometrías en una sola (equivalente a un `SUM` para geometrías).
    ```sql
    -- Unir todos los distritos en una geometría del municipio completo
    SELECT SDO_AGGR_UNION(SDOAGGRTYPE(geometria, 0.005)) AS limite_municipal
    FROM DISTRITOS;
    ```

## 5. Modelo de Relaciones Topológicas: DE-9IM

Oracle Spatial implementa el modelo **DE-9IM (Dimensionally Extended 9-Intersection Model)** del OGC, que clasifica sistemáticamente todas las posibles relaciones espaciales entre dos geometrías. Las relaciones fundamentales son:

| Relación | Descripción |
|----------|-------------|
| **DISJOINT** | Las geometrías no tienen ningún punto en común |
| **TOUCH** | Las geometrías comparten solo puntos de sus bordes |
| **OVERLAPBDYINTERSECT** | Las geometrías se superponen parcialmente |
| **EQUAL** | Las geometrías son idénticas |
| **CONTAINS** | La primera geometría contiene completamente a la segunda |
| **INSIDE** | La primera geometría está completamente dentro de la segunda |
| **COVERS** | Similar a CONTAINS, incluyendo bordes compartidos |
| **COVEREDBY** | Similar a INSIDE, incluyendo bordes compartidos |

## 6. Conclusión

Los Sistemas de Referencia Espacial y el motor de consultas espaciales completan el ecosistema Oracle Spatial and Graph. La correcta asignación del SRS (especialmente ETRS89/UTM zona 30N con SRID 25830 para la Administración Pública española) y la capacidad de transformación entre sistemas mediante `SDO_CS.TRANSFORM` garantizan la coherencia y la interoperabilidad de los datos geográficos.

Los operadores espaciales (`SDO_ANYINTERACT`, `SDO_CONTAINS`, `SDO_WITHIN_DISTANCE`, `SDO_NN`, `SDO_RELATE`), potenciados por el índice R-Tree, y las funciones espaciales (`SDO_AREA`, `SDO_DISTANCE`, `SDO_BUFFER`, `SDO_INTERSECTION`, `SDO_UNION`), conforman un motor analítico geoespacial de primer nivel que permite a las Administraciones Públicas ejecutar consultas territoriales complejas directamente en SQL, integrando la dimensión espacial en sus procesos de gestión urbanística, tributaria, medioambiental y de protección civil.
