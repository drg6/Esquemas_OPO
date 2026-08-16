# Tema 13.- Oracle Spatial and Graph: Tipo, métodos y constructores del objeto SDO_Geometry y Vistas de metadatos de geometría.

## 1. Introducción

En el Tema 12 se analizó la arquitectura general de Oracle Spatial and Graph, su modelo de datos Objeto-Relacional y los principios de la indexación R-Tree. Este tema profundiza en el componente central del sistema: el tipo de dato **`SDO_GEOMETRY`**, detallando su estructura interna, los métodos y constructores disponibles para crear y manipular geometrías, y las vistas de metadatos que Oracle Spatial exige como requisito previo a la indexación y el procesamiento espacial.

El dominio de `SDO_GEOMETRY` es una competencia esencial para cualquier profesional que deba administrar o desarrollar aplicaciones de información geográfica sobre Oracle Database en el ámbito de la Administración Pública.

## 2. Estructura Detallada del Tipo SDO_GEOMETRY

### 2.1. Definición formal

`SDO_GEOMETRY` es un tipo de dato objeto (Object Type) definido en el esquema `MDSYS` de Oracle. Su definición formal es:

```sql
CREATE TYPE SDO_GEOMETRY AS OBJECT (
    SDO_GTYPE      NUMBER,
    SDO_SRID       NUMBER,
    SDO_POINT      SDO_POINT_TYPE,
    SDO_ELEM_INFO  SDO_ELEM_INFO_ARRAY,
    SDO_ORDINATES  SDO_ORDINATE_ARRAY
);
```

### 2.2. SDO_GTYPE: Tipología y dimensionalidad

El atributo `SDO_GTYPE` codifica simultáneamente la dimensionalidad y el tipo de geometría en un número entero de cuatro dígitos con el formato **DLTT**:

| Componente | Significado | Valores |
|------------|-------------|---------|
| **D** | Número de dimensiones | 2 (2D), 3 (3D), 4 (4D con medida) |
| **L** | Referencia lineal (LRS) | 0 en la mayoría de los casos |
| **TT** | Tipo de geometría | 00-07 (véase tabla inferior) |

**Tipos de geometría (TT):**

| Código | Tipo | Descripción |
|--------|------|-------------|
| 00 | UNKNOWN_GEOMETRY | Tipo desconocido |
| 01 | POINT | Punto |
| 02 | LINE o CURVE | Línea o polilínea |
| 03 | POLYGON | Polígono |
| 04 | COLLECTION | Colección heterogénea de geometrías |
| 05 | MULTIPOINT | Conjunto de puntos |
| 06 | MULTILINE | Conjunto de líneas |
| 07 | MULTIPOLYGON | Conjunto de polígonos |

**Ejemplos prácticos:**
*   `2001` → Punto 2D (semáforo, farola, contenedor).
*   `2002` → Línea 2D (eje de calle, tubería de agua).
*   `2003` → Polígono 2D (parcela catastral, edificio, zona verde).
*   `3001` → Punto 3D (posición GPS con altitud).
*   `2007` → Multipolígono 2D (archipiélago, municipio con enclaves).

### 2.3. SDO_SRID: Sistema de referencia espacial

Identifica el sistema de coordenadas utilizado para definir la geometría. Los valores más relevantes para España:

*   **4326** — WGS84: Coordenadas geográficas en grados de latitud/longitud. Sistema del GPS.
*   **4258** — ETRS89: Sistema de referencia europeo, prácticamente coincidente con WGS84.
*   **25830** — ETRS89 / UTM zona 30N: Coordenadas proyectadas en metros. Sistema oficial para la cartografía de detalle en la mayor parte de España peninsular (incluida Alicante).
*   **25831** — ETRS89 / UTM zona 31N: Para la franja este de Cataluña y Baleares.
*   **32628** — WGS84 / UTM zona 28N: Para Canarias.

Si el SRID es `NULL`, la geometría se trata como un sistema de coordenadas local sin referencia geodésica.

### 2.4. SDO_POINT: Optimización para geometrías puntuales

Cuando la geometría es un punto simple, Oracle permite utilizar el atributo `SDO_POINT` (de tipo `SDO_POINT_TYPE(X, Y, Z)`) en lugar de los arrays `SDO_ELEM_INFO` y `SDO_ORDINATES`, simplificando la construcción y mejorando el rendimiento:

```sql
-- Punto 2D (semáforo en Alicante)
SDO_GEOMETRY(
    2001,           -- Punto 2D
    25830,          -- ETRS89/UTM30N
    SDO_POINT_TYPE(724500, 4247800, NULL),  -- Coordenadas X, Y
    NULL,           -- SDO_ELEM_INFO no necesario
    NULL            -- SDO_ORDINATES no necesario
)
```

Para líneas y polígonos, `SDO_POINT` se establece a `NULL` y las coordenadas se almacenan en `SDO_ORDINATES`.

### 2.5. SDO_ELEM_INFO: Descriptor de elementos

Array numérico que describe cómo interpretar las coordenadas almacenadas en `SDO_ORDINATES`. Cada elemento geométrico se define mediante tres valores consecutivos (tripletes):

*   **Offset:** Posición de inicio en `SDO_ORDINATES` (base 1).
*   **ETYPE (Element Type):**
    *   `1` = Punto
    *   `2` = Línea
    *   `1003` = Anillo exterior de polígono (sentido antihorario)
    *   `2003` = Anillo interior / hueco (sentido horario)
*   **Interpretation:**
    *   `1` = Vértices conectados por segmentos rectos
    *   `2` = Vértices conectados por arcos de circunferencia
    *   `3` = Rectángulo optimizado (solo 2 vértices: esquina inferior izquierda y superior derecha)

**Ejemplo — Polígono con un hueco:**
```sql
SDO_ELEM_INFO_ARRAY(
    1,    1003, 1,   -- Desde posición 1: anillo exterior, segmentos rectos
    11,   2003, 1    -- Desde posición 11: anillo interior (hueco), segmentos rectos
)
```

### 2.6. SDO_ORDINATES: Coordenadas

Array numérico que almacena la secuencia de coordenadas de todos los elementos de la geometría. Las coordenadas se organizan de forma secuencial: (X1, Y1, X2, Y2, ..., Xn, Yn) para 2D, o (X1, Y1, Z1, X2, Y2, Z2, ...) para 3D. En polígonos, el primer y último vértice deben coincidir para cerrar el anillo.

## 3. Constructores y Métodos de SDO_GEOMETRY

### 3.1. Constructor principal

El constructor estándar recibe los cinco atributos como parámetros:

```sql
SDO_GEOMETRY(sdo_gtype, sdo_srid, sdo_point, sdo_elem_info, sdo_ordinates)
```

### 3.2. Constructores simplificados (a partir de Oracle 12c)

Oracle proporciona constructores abreviados para geometrías comunes:

```sql
-- Punto desde coordenadas
SDO_GEOMETRY(724500, 4247800, 25830)

-- Punto desde WKT (Well-Known Text)
SDO_GEOMETRY('POINT (724500 4247800)', 25830)

-- Polígono desde WKT
SDO_GEOMETRY('POLYGON ((720100 4250200, 720200 4250200, 720200 4250300, 720100 4250300, 720100 4250200))', 25830)
```

### 3.3. Formato WKT (Well-Known Text)

Oracle Spatial soporta el formato estándar OGC **WKT** para la representación textual de geometrías. Las funciones de conversión son:

*   **`SDO_UTIL.TO_WKTGEOMETRY(geometry)`:** Convierte un objeto `SDO_GEOMETRY` a su representación WKT.
*   **`SDO_GEOMETRY(wkt, srid)`:** Crea un objeto `SDO_GEOMETRY` a partir de una cadena WKT.

### 3.4. Formato WKB (Well-Known Binary)

Para la transferencia eficiente de datos entre sistemas:
*   **`SDO_UTIL.TO_WKBGEOMETRY(geometry)`:** Convierte a formato binario WKB.
*   **`SDO_UTIL.FROM_WKBGEOMETRY(wkb)`:** Convierte desde WKB a `SDO_GEOMETRY`.

### 3.5. Formato GeoJSON

A partir de Oracle 12c, Oracle Spatial soporta la conversión a y desde GeoJSON:
*   **`SDO_UTIL.TO_GEOJSON(geometry)`:** Convierte a formato GeoJSON.
*   **`SDO_UTIL.FROM_GEOJSON(geojson)`:** Convierte desde GeoJSON.

### 3.6. Métodos del objeto SDO_GEOMETRY

*   **`geometry.GET_GTYPE()`** — Devuelve el tipo de geometría (TT del SDO_GTYPE).
*   **`geometry.GET_DIMS()`** — Devuelve el número de dimensiones.
*   **`geometry.ST_ISVALID()`** — Verifica si la geometría es topológicamente válida.
*   **`SDO_GEOM.VALIDATE_GEOMETRY_WITH_CONTEXT(geometry, tolerance)`** — Validación detallada con mensaje de error descriptivo.

## 4. Vistas de Metadatos de Geometría

### 4.1. USER_SDO_GEOM_METADATA

Oracle Spatial **exige obligatoriamente** que el DBA registre las columnas espaciales en la vista `USER_SDO_GEOM_METADATA` antes de crear un índice espacial. Sin este registro, Oracle rechazará la creación del índice R-Tree.

La vista tiene cuatro columnas:

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `TABLE_NAME` | VARCHAR2 | Nombre de la tabla que contiene la columna espacial |
| `COLUMN_NAME` | VARCHAR2 | Nombre de la columna de tipo SDO_GEOMETRY |
| `DIMINFO` | SDO_DIM_ARRAY | Array de dimensiones con rangos y tolerancias |
| `SRID` | NUMBER | Identificador del sistema de referencia espacial |

### 4.2. DIMINFO: Definición de dimensiones

`DIMINFO` es un array de objetos `SDO_DIM_ELEMENT`, uno por cada dimensión de la geometría:

```sql
SDO_DIM_ARRAY(
    SDO_DIM_ELEMENT('X', min_x, max_x, tolerancia),
    SDO_DIM_ELEMENT('Y', min_y, max_y, tolerancia)
)
```

*   **Min/Max:** Rangos de coordenadas válidos para la dimensión. Para ETRS89/UTM30N en la Comunitat Valenciana, rangos típicos serían X: 680000-780000, Y: 4200000-4350000.
*   **Tolerancia:** Distancia mínima en las unidades del SRID por debajo de la cual dos puntos se consideran idénticos. Para coordenadas en metros (UTM), un valor típico es 0.005 (5 milímetros).

### 4.3. Ejemplo completo de registro

```sql
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
COMMIT;
```

### 4.4. Otras vistas de metadatos relevantes

*   **`ALL_SDO_GEOM_METADATA`:** Muestra los metadatos de todas las columnas espaciales a las que el usuario tiene acceso (incluyendo las de otros esquemas).
*   **`USER_SDO_INDEX_METADATA`:** Contiene información detallada sobre los índices espaciales creados por el usuario.
*   **`SDO_COORD_REF_SYS`:** Catálogo de todos los sistemas de referencia de coordenadas soportados por Oracle.

## 5. Validación de Geometrías

Antes de indexar o consultar datos espaciales, es fundamental verificar que las geometrías almacenadas son topológicamente válidas. Oracle proporciona funciones de validación:

```sql
-- Validación rápida (devuelve 'TRUE' o error)
SELECT id_parcela, SDO_GEOM.VALIDATE_GEOMETRY_WITH_CONTEXT(forma, 0.005)
FROM PARCELAS
WHERE SDO_GEOM.VALIDATE_GEOMETRY_WITH_CONTEXT(forma, 0.005) <> 'TRUE';
```

Los errores más comunes incluyen:
*   **13356:** Polígono no cerrado (el primer y último vértice no coinciden).
*   **13349:** Anillo con autointersección.
*   **13367:** Orientación incorrecta del anillo (exterior debe ser antihorario, interior horario).

## 6. Conclusión

El tipo `SDO_GEOMETRY` constituye el pilar fundamental de Oracle Spatial and Graph. Su estructura de cinco atributos (`SDO_GTYPE`, `SDO_SRID`, `SDO_POINT`, `SDO_ELEM_INFO`, `SDO_ORDINATES`) permite representar cualquier tipo de geometría —desde puntos simples hasta multipolígonos con huecos— en una única celda de la tabla, conforme a los estándares OGC e ISO 19125.

Los constructores simplificados, el soporte de formatos estándar (WKT, WKB, GeoJSON) y los métodos de validación facilitan la integración con herramientas GIS externas y el mantenimiento de la calidad de los datos. Las vistas de metadatos (`USER_SDO_GEOM_METADATA`) constituyen un requisito indispensable para la creación de índices R-Tree y el correcto funcionamiento de los operadores espaciales que se analizarán en el Tema 14.
