# Tema 12.- Oracle Spatial and Graph: Qué es, Modelo de datos e Indexación de datos espaciales.

## 1. Introducción

Los Sistemas Gestores de Bases de Datos Relacionales (SGBDR) tradicionales fueron diseñados para gestionar datos alfanuméricos: nombres, importes, fechas e identificadores. Las operaciones sobre estos datos se resuelven eficientemente mediante comparaciones lineales, ordenaciones y búsquedas indexadas con estructuras B-Tree. Sin embargo, la gestión territorial de las Administraciones Públicas requiere responder a preguntas de naturaleza radicalmente diferente: *¿qué parcelas se intersectan con la zona inundable del barranco?*, *¿cuántos contenedores de residuos hay en un radio de 500 metros de este punto?*, *¿cuál es la superficie exacta afectada por la nueva calificación urbanística?*

Estas consultas geoespaciales no pueden resolverse con los mecanismos relacionales estándar, ya que las geometrías (polígonos de miles de vértices, redes de tuberías, nubes de puntos) no se ordenan alfabéticamente ni se comparan con operadores `>` o `<`.

Para cubrir esta necesidad, Oracle integra nativamente en el kernel de su SGBDR la extensión **Oracle Spatial and Graph**, que transforma Oracle Database en un sistema gestor de bases de datos espaciales completo, capaz de almacenar, indexar y analizar datos geográficos con la misma robustez con la que gestiona datos alfanuméricos.

## 2. ¿Qué es Oracle Spatial and Graph?

### 2.1. Definición

**Oracle Spatial and Graph** es una extensión nativa del motor de la base de datos Oracle que incorpora tipos de datos espaciales, funciones de análisis geométrico, operadores de consulta espacial y mecanismos de indexación especializados para trabajar con información georreferenciada directamente dentro de la base de datos relacional.

### 2.2. Integración nativa vs. extensión externa

A diferencia de soluciones como PostGIS (que se instala como una extensión sobre PostgreSQL), Oracle Spatial está integrado en el núcleo mismo del SGBDR Oracle. Esta integración nativa implica que los datos espaciales heredan automáticamente todas las capacidades de la plataforma:

*   **Transacciones ACID:** Las operaciones sobre geometrías están protegidas por las mismas garantías transaccionales que cualquier dato alfanumérico.
*   **Alta Disponibilidad:** Los datos espaciales se benefician de Oracle RAC, Data Guard y failover automático.
*   **Particionamiento:** Las tablas con columnas espaciales pueden particionarse igual que cualquier otra tabla (Range, List, Hash, Composite).
*   **Backup y Recovery:** RMAN respalda los datos espaciales como parte integral de los datafiles.
*   **Seguridad:** Los privilegios DCL (GRANT/REVOKE) se aplican sobre las tablas y columnas espaciales con la misma granularidad que sobre datos convencionales.
*   **Oracle Spatial and Graph** también incluye capacidades de análisis de grafos (Oracle Property Graph) para modelar y consultar redes complejas (redes sociales, redes de transporte, análisis de dependencias).

## 3. Modelo de Datos Espacial: El Paradigma Objeto-Relacional

### 3.1. El problema del almacenamiento geométrico en tablas planas

Almacenar una geometría de miles de vértices en una tabla relacional plana plantea un problema estructural. Un polígono catastral con 50 vértices requeriría 100 columnas (X1, Y1, X2, Y2, ..., X50, Y50) en un modelo puramente relacional, o bien una tabla auxiliar con una fila por vértice. Ambas soluciones son ineficientes, difíciles de mantener y limitan drásticamente las capacidades de consulta.

### 3.2. La solución Objeto-Relacional: SDO_GEOMETRY

A partir de la versión 8i (y consolidado en 9i), Oracle adoptó el modelo **Objeto-Relacional**, que permite definir tipos de datos estructurados complejos como columnas de una tabla. La pieza central de Oracle Spatial es el tipo de dato **`SDO_GEOMETRY`**, un objeto estructurado que encapsula toda la información geométrica de una entidad espacial en una única celda de la tabla.

```sql
CREATE TABLE PARCELAS (
    id_parcela    NUMBER PRIMARY KEY,
    propietario   VARCHAR2(200),
    ref_catastral VARCHAR2(20),
    forma         SDO_GEOMETRY   -- Una sola columna almacena la geometría completa
);
```

En este esquema, la columna `forma` de tipo `SDO_GEOMETRY` puede almacenar un polígono con miles de vértices, un punto, una línea o cualquier geometría compleja, sin necesidad de columnas auxiliares ni tablas de detalle.

### 3.3. Conformidad con estándares

El modelo de datos espacial de Oracle es conforme con el estándar **Simple Features Specification (SFS)** del Open Geospatial Consortium (OGC) y con la norma **ISO 19125**, lo que garantiza la interoperabilidad con otras plataformas y herramientas GIS.

## 4. Los 5 Atributos del Objeto SDO_GEOMETRY

Todo objeto `SDO_GEOMETRY` se compone de cinco atributos internos que definen completamente la geometría:

### 4.1. SDO_GTYPE (Geometry Type)

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

### 4.2. SDO_SRID (Spatial Reference Identifier)

Número entero que identifica el sistema de coordenadas de la geometría, referenciado al registro EPSG (European Petroleum Survey Group). Ejemplos:
*   `4326` = WGS84 (sistema GPS mundial, coordenadas en grados de latitud/longitud).
*   `25830` = ETRS89 / UTM zona 30N (sistema oficial en España peninsular, coordenadas en metros).
*   `NULL` = Geometría sin sistema de referencia asignado (coordenadas locales).

### 4.3. SDO_POINT (Point Type)

Atributo de tipo `SDO_POINT_TYPE` que almacena directamente las coordenadas X, Y (y opcionalmente Z) cuando la geometría es un punto simple. Si la geometría es una línea o un polígono, este atributo se establece a `NULL` y las coordenadas se almacenan en `SDO_ORDINATES`.

### 4.4. SDO_ELEM_INFO (Element Info Array)

Array numérico (`SDO_ELEM_INFO_ARRAY`) que indica al motor cómo debe interpretar la secuencia de coordenadas almacenada en `SDO_ORDINATES`. Cada elemento se define mediante tres valores consecutivos:
*   **Offset:** Posición de inicio en el array de coordenadas (base 1).
*   **Element Type:** Tipo de elemento (1 = punto, 2 = línea, 1003 = anillo exterior de polígono, 2003 = anillo interior/hueco).
*   **Interpretation:** Cómo se conectan los vértices (1 = segmentos rectos, 2 = arcos de circunferencia).

**Ejemplo para un polígono simple:** `SDO_ELEM_INFO_ARRAY(1, 1003, 1)` — comienza en la posición 1, es un anillo exterior de polígono, los vértices se conectan con segmentos rectos.

### 4.5. SDO_ORDINATES (Ordinates Array)

Array numérico (`SDO_ORDINATE_ARRAY`) que almacena la secuencia de coordenadas de la geometría, organizadas como (X1, Y1, X2, Y2, ..., Xn, Yn) para geometrías 2D. En polígonos, el primer y último vértice deben coincidir para cerrar el anillo.

### 4.6. Ejemplo completo de construcción

```sql
-- Insertar un polígono rectangular (parcela catastral) en ETRS89/UTM30N
INSERT INTO PARCELAS (id_parcela, propietario, ref_catastral, forma) VALUES (
    1,
    'Ayuntamiento de Alicante',
    '03014A001000010001',
    SDO_GEOMETRY(
        2003,                          -- SDO_GTYPE: Polígono 2D
        25830,                         -- SDO_SRID:  ETRS89 / UTM zona 30N
        NULL,                          -- SDO_POINT: NULL (no es un punto)
        SDO_ELEM_INFO_ARRAY(1, 1003, 1), -- Anillo exterior, segmentos rectos
        SDO_ORDINATE_ARRAY(            -- Coordenadas UTM del polígono
            720100, 4250200,           -- Vértice 1
            720200, 4250200,           -- Vértice 2
            720200, 4250300,           -- Vértice 3
            720100, 4250300,           -- Vértice 4
            720100, 4250200            -- Vértice 5 = Vértice 1 (cierre)
        )
    )
);
```

## 5. Indexación Espacial: El Índice R-Tree

### 5.1. Limitaciones del B-Tree para datos espaciales

El índice B-Tree, pilar de la indexación relacional, funciona ordenando valores en una dimensión lineal: los DNI que comienzan por '2' van a la izquierda, los que comienzan por '7' van a la derecha. Sin embargo, las geometrías no tienen un orden lineal natural. No existe una forma significativa de decir que el polígono A es "anterior" al polígono B en un sentido alfanumérico. Un polígono se extiende simultáneamente en las dimensiones X e Y, lo que imposibilita su ordenación unidimensional.

Si se intentara buscar geométricamente sin índice espacial, el motor debería realizar un **Full Table Scan**: comparar la geometría de consulta con cada una de las geometrías de la tabla, una operación con coste O(n) que resulta prohibitiva en tablas con millones de registros.

### 5.2. El concepto de MBR (Minimum Bounding Rectangle)

Para evitar comparaciones geométricas complejas durante la fase de filtrado, Oracle Spatial utiliza una aproximación simplificada: el **MBR (Minimum Bounding Rectangle)** o Rectángulo Mínimo Envolvente.

El MBR es el rectángulo de menor superficie, con lados paralelos a los ejes de coordenadas, que contiene completamente la geometría original. Comparar si dos rectángulos se intersectan es computacionalmente trivial (requiere solo cuatro comparaciones numéricas), mientras que comparar dos polígonos irregulares de miles de vértices es extremadamente costoso.

### 5.3. Arquitectura del R-Tree (Region Tree)

El **R-Tree** es la estructura de indexación espacial utilizada por Oracle Spatial. Funciona como un árbol jerárquico de MBRs anidados:

1.  **Nodo raíz:** Contiene el MBR que engloba la totalidad del espacio indexado (por ejemplo, todo el territorio nacional).
2.  **Nodos intermedios (ramas):** El espacio se subdivide recursivamente en MBRs más pequeños. El nodo raíz puede contener MBRs correspondientes a las comunidades autónomas; cada comunidad se subdivide en MBRs provinciales; cada provincia en MBRs municipales.
3.  **Nodos hoja (leaf nodes):** Contienen los MBRs de las geometrías individuales almacenadas en la tabla (parcelas catastrales, edificios, redes de infraestructuras).

### 5.4. Proceso de consulta en dos fases

Oracle Spatial ejecuta las consultas espaciales en dos fases:

*   **Fase 1 — Filtrado (Primary Filter):** Utiliza el índice R-Tree para identificar rápidamente los **candidatos** cuyo MBR se intersecta con el MBR de la geometría de consulta. Esta fase es extremadamente rápida porque solo compara rectángulos. Produce un conjunto de candidatos que puede incluir **falsos positivos** (geometrías cuyo MBR se intersecta pero cuya geometría real no lo hace).

*   **Fase 2 — Refinamiento (Secondary Filter):** Sobre el conjunto reducido de candidatos de la Fase 1, Oracle ejecuta la **comparación geométrica exacta** (vértice a vértice) para eliminar los falsos positivos y devolver solo los resultados verdaderos. Esta fase es computacionalmente costosa, pero se aplica únicamente sobre un subconjunto muy reducido de la tabla.

### 5.5. Creación y requisitos del índice espacial

Para crear un índice R-Tree, es necesario registrar previamente los metadatos de la columna espacial en la vista `USER_SDO_GEOM_METADATA` (que se detallará en el Tema 13):

```sql
-- 1. Registrar metadatos
INSERT INTO USER_SDO_GEOM_METADATA (TABLE_NAME, COLUMN_NAME, DIMINFO, SRID) VALUES (
    'PARCELAS', 'FORMA',
    SDO_DIM_ARRAY(
        SDO_DIM_ELEMENT('X', 500000, 800000, 0.005),  -- Rango X, tolerancia
        SDO_DIM_ELEMENT('Y', 4100000, 4400000, 0.005)  -- Rango Y, tolerancia
    ),
    25830  -- SRID: ETRS89/UTM30N
);

-- 2. Crear el índice espacial R-Tree
CREATE INDEX idx_parcelas_forma ON PARCELAS(FORMA)
    INDEXTYPE IS MDSYS.SPATIAL_INDEX;
```

## 6. Conclusión

Oracle Spatial and Graph transforma el SGBDR Oracle en un sistema gestor de bases de datos espaciales completo, integrando las capacidades geoespaciales directamente en el kernel del motor relacional. El modelo de datos Objeto-Relacional, materializado en el tipo `SDO_GEOMETRY`, permite almacenar geometrías de cualquier complejidad en una única columna de la tabla, eliminando las limitaciones del modelo relacional plano.

La indexación R-Tree, basada en la jerarquía de MBRs y el proceso de consulta en dos fases (filtrado rápido por rectángulos envolventes y refinamiento geométrico exacto), resuelve el desafío de la búsqueda eficiente en el espacio bidimensional, algo imposible con los índices B-Tree convencionales. Esta arquitectura permite ejecutar consultas espaciales complejas sobre tablas con millones de geometrías en tiempos de respuesta aceptables para los sistemas de información de las Administraciones Públicas.
