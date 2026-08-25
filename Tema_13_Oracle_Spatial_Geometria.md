# Tema 13.- Oracle Spatial and Graph: Tipo, métodos y constructores del objeto SDO_Geometry y Vistas de metadatos de geometría.

## 1. Introducción

Gestionar información geográfica -> Oracle Spatial and Graph. Nucleo -> tipo de dato nativo **SDO_GEOMETRY**, almacenar, indexar y consultar datos geométricos.

Estructura interna, constructores y métodos para manipular geometrías en SQL. 
Vistas de metadatos de geometría (**USER_SDO_GEOM_METADATA**), para registrar tablas espaciales y habilitar creación de índices .

Fundamental para la administración de BD espaciales en AP.

## 2. Estructura Detallada del Tipo SDO_GEOMETRY

### 2.1. Definición formal

`SDO_GEOMETRY` -> esquema `MDSYS` de Oracle. 

CREATE TYPE SDO_GEOMETRY AS OBJECT (
    SDO_GTYPE      NUMBER,
    SDO_SRID       NUMBER,
    SDO_POINT      SDO_POINT_TYPE,
    SDO_ELEM_INFO  SDO_ELEM_INFO_ARRAY,
    SDO_ORDINATES  SDO_ORDINATE_ARRAY
);

### 2.2. SDO_GTYPE: Tipología y dimensionalidad

Número entero que identifica el tipo de geometría y su dimensionalidad. Sigue el patrón **DLTT**, donde:
*   **D** = Número de dimensiones (2 para 2D, 3 para 3D, 4 para 4D con medida).
*   **L** = Identificador de referencia lineal (habitualmente 0).
*   **TT** = Tipo de geometría:
    *   `01` = Punto
    *   `02` = Línea o Polilínea
    *   `03` = Polígono
    *   `04` = Colección de geometrías
    *   `05` = Multipunto
    *   `06` = Multilínea
    *   `07` = Multipolígono

**Ejemplo:** Un polígono 2D tiene `SDO_GTYPE = 2003`. Un punto 2D tiene `SDO_GTYPE = 2001`.

### 2.3. SDO_SRID: Sistema de referencia espacial

Número entero que identifica el sistema de coordenadas de la geometría, referenciado al registro EPSG (European Petroleum Survey Group). Ejemplos:
*   `4326` = WGS84 (sistema GPS mundial, coordenadas en grados de latitud/longitud).
*   `25830` = ETRS89 / UTM zona 30N (sistema oficial en España peninsular, coordenadas en metros).
*   `NULL` = Geometría sin sistema de referencia asignado (coordenadas locales).

### 2.4. SDO_POINT: Optimización para geometrías puntuales

Almacena coordenadas X, Y (y opcionalmente Z) cuando la geometría es un punto simple. Línea o polígono, a `NULL` y las coordenadas se almacenan en `SDO_ORDINATES`.
Para que Oracle use la optimización de SDO_POINT, los atributos SDO_ELEM_INFO y SDO_ORDINATES deben ser obligatoriamente NULL. Si no lo son, Oracle ignora SDO_POINT

### 2.5. SDO_ELEM_INFO: Descriptor de elementos

Array numérico (`SDO_ELEM_INFO_ARRAY`) indica cómo interpretar la secuencia de coordenadas almacenada en `SDO_ORDINATES`. Cada elemento se define mediante tres valores consecutivos:
*   **Offset:** Posición de inicio en el array de coordenadas (base 1).
*   **Element Type:** Tipo de elemento (1 = punto, 2 = línea, 1003 = anillo exterior de polígono, 2003 = anillo interior/hueco).
*   **Interpretation:** Cómo se conectan los vértices (1 = segmentos rectos, 2 = arcos de circunferencia).

**Ejemplo para un polígono simple:** `SDO_ELEM_INFO_ARRAY(1, 1003, 1)` — comienza en la posición 1, es un anillo exterior de polígono, los vértices se conectan con segmentos rectos.

### 2.6. SDO_ORDINATES: Coordenadas

Array numérico (`SDO_ORDINATE_ARRAY`) que almacena la secuencia de coordenadas de la geometría, organizadas como (X1, Y1, X2, Y2, ..., Xn, Yn) para geometrías 2D. En polígonos, el primer y último vértice deben coincidir para cerrar el anillo.

## 3. Constructores y Métodos de SDO_GEOMETRY

### 3.1. Constructor principal

El constructor estándar recibe los cinco atributos como parámetros:

SDO_GEOMETRY(
    2001,                              -- SDO_GTYPE: Punto 2D - Farola o Contenedor
    25830,                             -- SDO_SRID: ETRS89 / UTM zona 30N
    SDO_POINT_TYPE(720100, 4250200, NULL), -- SDO_POINT: Coordenadas X, Y
    NULL,                              -- SDO_ELEM_INFO: NULL para puntos
    NULL                               -- SDO_ORDINATES: NULL para puntos
)

### 3.2. Constructores simplificados 

-- Punto desde coordenadas
SDO_GEOMETRY(724500, 4247800, 25830)

-- Punto desde WKT (Well-Known Text)
SDO_GEOMETRY('POINT (724500 4247800)', 25830)

-- Polígono desde WKT
SDO_GEOMETRY('POLYGON ((720100 4250200, 720200 4250200, 720200 4250300, 720100 4250300, 720100 4250200))', 25830)

### 3.3. Formato WKT (Well-Known Text)

Representación en texto plano de geometrías. Legible por humanos.

*   **`SDO_UTIL.TO_WKTGEOMETRY(geometry)`:** Convierte un objeto `SDO_GEOMETRY` a WKT.
*   **`SDO_GEOMETRY(wkt, srid)`:** Crea un objeto `SDO_GEOMETRY` a partir de una cadena WKT.

### 3.4. Formato WKB (Well-Known Binary)

Para la transferencia eficiente de datos entre sistemas:
*   **`SDO_UTIL.TO_WKBGEOMETRY(geometry)`:** Convierte a formato binario WKB.
*   **`SDO_UTIL.FROM_WKBGEOMETRY(wkb)`:** Convierte desde WKB a `SDO_GEOMETRY`.

### 3.5. Formato GeoJSON

*   **`SDO_UTIL.TO_GEOJSON(geometry)`:** Convierte a formato GeoJSON.
*   **`SDO_UTIL.FROM_GEOJSON(geojson)`:** Convierte desde GeoJSON.

### 3.6. Métodos del objeto SDO_GEOMETRY

*   **`geometry.GET_GTYPE()`** — Devuelve el tipo de geometría (TT del SDO_GTYPE).
*   **`geometry.GET_DIMS()`** — Devuelve el número de dimensiones.
*   **`geometry.ST_ISVALID()`** — Verifica si la geometría es topológicamente válida.
*   **`SDO_GEOM.VALIDATE_GEOMETRY_WITH_CONTEXT(geometry, tolerance)`** — Validación detallada con mensaje de error descriptivo.

## 4. Vistas de Metadatos de Geometría

### 4.1. USER_SDO_GEOM_METADATA

Oracle Spatial **exige obligatoriamente** que el DBA registre las columnas espaciales en la vista `USER_SDO_GEOM_METADATA` antes de crear un índice espacial. 

La vista tiene cuatro columnas: `TABLE_NAME` , `COLUMN_NAME` , `DIMINFO` (Array de dimensiones con rangos y tolerancias), `SRID` (Identificador del sistema de referencia espacial)

### 4.2. DIMINFO: Definición de dimensiones

`DIMINFO` es un array de objetos `SDO_DIM_ELEMENT`.

SDO_DIM_ARRAY(
    SDO_DIM_ELEMENT('X', min_x, max_x, tolerancia),
    SDO_DIM_ELEMENT('Y', min_y, max_y, tolerancia)
)

*   **Min/Max:** Rangos de coordenadas válidos para la dimensión. Para ETRS89/UTM30N en la Comunitat Valenciana, rangos típicos serían X: 680000-780000, Y: 4200000-4350000.
*   **Tolerancia:** Distancia mínima por debajo de la cual dos puntos se consideran idénticos. Para metros (UTM), un valor típico es 0.005 (5 milímetros). 

### 4.3. Ejemplo completo de registro

INSERT INTO USER_SDO_GEOM_METADATA (TABLE_NAME, COLUMN_NAME, DIMINFO, SRID)
VALUES (
    'PARCELAS',
    'FORMA',
    SDO_DIM_ARRAY(
        SDO_DIM_ELEMENT('X', 680000, 780000, 0.005),
        SDO_DIM_ELEMENT('Y', 4200000, 4350000, 0.005)
    ),
    25830
);

### 4.4. Otras vistas de metadatos relevantes

*   **`ALL_SDO_GEOM_METADATA`:** Metadatos columnas espaciales usuario tiene acceso.
*   **`USER_SDO_INDEX_METADATA`:** Indices espaciales creados por el usuario.
*   **`SDO_COORD_REF_SYS`:** Sistemas de referencia de coordenadas soportados por Oracle.

Los errores más comunes incluyen:
*   **13356:** Polígono no cerrado (el primer y último vértice no coinciden).
*   **13349:** Anillo con autointersección.
*   **13367:** Orientación incorrecta del anillo (exterior debe ser antihorario, interior horario).

## 5. Validación de Geometrías

Antes de indexar o consultar datos espaciales, es fundamental verificar que las geometrías almacenadas son topológicamente válidas. **SDO_GEOM.VALIDATE_GEOMETRY_WITH_CONTEXT**

## 6. Conclusión

El tipo `SDO_GEOMETRY` pilar fundamental de Oracle Spatial and Graph. Su estructura de cinco atributos (`SDO_GTYPE`, `SDO_SRID`, `SDO_POINT`, `SDO_ELEM_INFO`, `SDO_ORDINATES`) permite representar cualquier tipo de geometría en una única celda, conforme a los estándares OGC e ISO 19125.

Los constructores simplificados, el soporte de formatos estándar (WKT, WKB, GeoJSON) y los métodos de validación facilitan la integración con herramientas GIS externas y el mantenimiento de la calidad de los datos. 
Las vistas de metadatos (`USER_SDO_GEOM_METADATA`) constituyen un requisito indispensable para la creación de índices R-Tree.
