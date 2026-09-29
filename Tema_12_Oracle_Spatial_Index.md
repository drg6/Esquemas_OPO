# Tema 12.- Oracle Spatial and Graph: Qué es, Modelo de datos e Indexación de datos espaciales.

## 1. Introducción y Concepto
* **El problema del SGBDR tradicional:** Optimizado para datos alfanuméricos y ordenación unidimensional (índices B-Tree). Incapaz de resolver nativamente consultas espaciales complejas (intersecciones, distancias, cálculo de áreas).
* **Solución:** **Oracle Spatial and Graph**, una extensión integrada directamente en el kernel de Oracle Database que dota al motor relacional de tipos de datos, funciones geométricas y mecanismos de indexación espacial para procesar la información geográfica.

## 2. Ventajas de la Integración Nativa
A diferencia de soluciones externas (como PostGIS que se instala sobre PostgreSQL), al operar dentro del núcleo de la base de datos, los datos espaciales heredan todas las capacidades corporativas de Oracle:
* Cumplimiento estricto de transacciones **ACID**.
* Soporte nativo para **Alta Disponibilidad** (Oracle RAC, Data Guard).
* Compatibilidad con estrategias de **Particionamiento** (Range, Hash, List) y **Backup/Recovery** (RMAN).
* Seguridad granular aplicando privilegios DCL (GRANT/REVOKE) a nivel de tabla/columna.
* Inclusión de capacidades de análisis de redes complejas mediante Oracle Property Graph.

## 3. Modelo de Datos Espacial: Paradigma Objeto-Relacional
* **Problema:** Almacenar geometrías de miles de vértices utilizando columnas planas (X1, Y1, X2, Y2) es altamente ineficiente e insostenible.
* **Solución (`SDO_GEOMETRY`):** Oracle emplea un modelo **Objeto-Relacional** donde la geometría completa (punto, línea o polígono) se encapsula como un único objeto estructurado dentro de una sola celda/columna de tipo `SDO_GEOMETRY`.
* **Cumplimiento de estándares:** La arquitectura es conforme a la especificación **SFS (*Simple Features Specification*)** del OGC y la normativa **ISO 19125**.

## 4. Los 5 Atributos del Objeto `SDO_GEOMETRY`
El objeto se compone internamente de cinco atributos estructurales:
1. **`SDO_GTYPE` (Geometry Type):** Número entero con patrón **DLTT** (Dimensiones, Localización Lineal, Tipo). Ejemplos: `2001` = Punto 2D; `2003` = Polígono 2D.
2. **`SDO_SRID` (Spatial Reference Identifier):** Código EPSG del sistema de coordenadas asociado a la geometría (ej. `4326` para WGS84 o `25830` para ETRS89/UTM 30N).
3. **`SDO_POINT`:** Almacena directamente las coordenadas X, Y, Z **únicamente** cuando se trata de un punto simple (se deja en `NULL` para líneas o polígonos).
4. **`SDO_ELEM_INFO`:** Array de metadatos (tripletes numéricos) que indica cómo interpretar las coordenadas. Especifica el *Offset* (inicio), *Element Type* (tipo, ej. 1003 anillo exterior) e *Interpretation* (ej. 1 segmentos rectos).
5. **`SDO_ORDINATES`:** Array continuo que almacena la secuencia real de coordenadas (X1, Y1, X2, Y2...) para construir la forma geométrica.

## 5. Indexación Espacial: El Índice R-Tree
* **Limitación del B-Tree:** Carece de capacidad para ordenar objetos bidimensionales. Sin un índice espacial, Oracle forzaría un *Full Table Scan* para comparar cada geometría.
* **Concepto MBR (*Minimum Bounding Rectangle*):** Rectángulo envolvente mínimo de lados paralelos a los ejes que contiene completamente la geometría. Reduce cálculos matemáticos complejos a simples comparaciones de cuatro valores numéricos.
* **Estructura del R-Tree (*Region Tree*):** Árbol jerárquico de MBRs anidados. El nodo raíz engloba todo el territorio, las ramas son subdivisiones zonales, y las hojas contienen los MBRs de cada geometría individual.
* **Proceso de Consulta en 2 Fases:**
  1. **Fase 1 (Filtrado Primario):** Cruza el índice R-Tree detectando qué MBRs se intersectan. Es extremadamente rápido pero arroja candidatos y **falsos positivos** (MBRs que se tocan aunque las geometrías interiores no lo hagan).
  2. **Fase 2 (Refinamiento):** Toma el conjunto de candidatos de la Fase 1 y realiza el cálculo geométrico exacto vértice a vértice para devolver solo los resultados verdaderos.
* **Implementación:** Para crear un índice `MDSYS.SPATIAL_INDEX`, es obligatorio registrar previamente la tabla, la columna, las tolerancias y el SRID en la vista de metadatos del sistema **`USER_SDO_GEOM_METADATA`**.

## 6. Conclusión
Oracle Spatial and Graph resuelve el almacenamiento de información territorial encapsulando geometrías complejas bajo el estándar `SDO_GEOMETRY`, integrándolas como un objeto nativo del SGBDR sin sacrificar robustez. Su capacidad de respuesta ante grandes volúmenes de datos espaciales (VLDB) se sustenta en el índice jerárquico R-Tree y su algoritmo de búsqueda en dos fases (MBR + refinamiento), convirtiéndolo en una infraestructura indispensable para la gestión urbanística, catastral y medioambiental en las Administraciones Públicas.

---------------

# Tema 12.- Oracle Spatial and Graph: Qué es, Modelo de datos e Indexación de datos espaciales.

## 1. Introducción

Gestión territorial AP requiere **Oracle Spatial and Graph**, SGBD espacial completo, capaz de almacenar, indexar y analizar datos geográficos con la misma robustez con la que gestiona datos alfanuméricos.

## 2. ¿Qué es Oracle Spatial and Graph?

### 2.1. Definición

**Oracle Spatial and Graph** extensión nativa que incorpora tipos de datos espaciales, funciones de análisis geométrico, operadores de consulta espacial y mecanismos de indexación especializados en el kernel de Oracle.

### 2.2. Integración nativa vs. extensión externa

PostGIS (extensión sobre PostgreSQL), Oracle Spatial integrado SGBDR Oracle. Esta integración permite:

*   **Transacciones ACID:** Garantías transaccionales estándar (Commit/Rollback) sobre las propias geometrías.
*   **Alta Disponibilidad:** Totalmente compatible con Oracle RAC y Data Guard.
*   **Particionamiento:** Tablas espaciales particionables (Range, List, Hash) para gestionar grandes volúmenes municipales.
*   **Backup y Recovery:** Respaldo integrado nativamente mediante RMAN.
*   **Seguridad:** Privilegios DCL (GRANT/REVOKE) aplicables a nivel de tabla y columna espacial.
*   **Oracle Spatial and Graph** Análisis de grafos (Oracle Property Graph) para modelar y consultar redes complejas (redes de transporte, aguas).

## 3. Modelo de Datos Espacial: El Paradigma Objeto-Relacional

### 3.1. El problema del almacenamiento geométrico en tablas planas

Almacenar una geometría de miles de vértices en una tabla relacional plana plantea un problema estructural, es ineficiente, difícil de mantener y limita las capacidades de consulta.

### 3.2. La solución Objeto-Relacional: SDO_GEOMETRY

Modelo **Objeto-Relacional**, tipos de datos estructurados complejos como columnas de una tabla. 
Tipo de dato **`SDO_GEOMETRY`**, un objeto estructurado que encapsula toda la información geométrica en una única celda de la tabla.

### 3.3. Conformidad con estándares

Oracle es conforme estándar **Simple Features Specification (SFS)** del Open Geospatial Consortium (OGC) y con la norma **ISO 19125**, lo que garantiza la interoperabilidad con otras plataformas y herramientas GIS.

## 4. Los 5 Atributos del Objeto SDO_GEOMETRY

`SDO_GEOMETRY` se compone de cinco atributos internos que definen completamente la geometría:

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

Almacena  coordenadas X, Y (y opcionalmente Z) cuando la geometría es un punto simple. Línea o polígono, a `NULL` y las coordenadas se almacenan en `SDO_ORDINATES`.

### 4.4. SDO_ELEM_INFO (Element Info Array)

Array numérico (`SDO_ELEM_INFO_ARRAY`) indica cómo interpretar la secuencia de coordenadas almacenada en `SDO_ORDINATES`. Cada elemento se define mediante tres valores consecutivos:
*   **Offset:** Posición de inicio en el array de coordenadas (base 1).
*   **Element Type:** Tipo de elemento (1 = punto, 2 = línea, 1003 = anillo exterior de polígono, 2003 = anillo interior/hueco).
*   **Interpretation:** Cómo se conectan los vértices (1 = segmentos rectos, 2 = arcos de circunferencia).

**Ejemplo para un polígono simple:** `SDO_ELEM_INFO_ARRAY(1, 1003, 1)` — comienza en la posición 1, es un anillo exterior de polígono, los vértices se conectan con segmentos rectos.

### 4.5. SDO_ORDINATES (Ordinates Array)

Array numérico (`SDO_ORDINATE_ARRAY`) que almacena la secuencia de coordenadas de la geometría, organizadas como (X1, Y1, X2, Y2, ..., Xn, Yn) para geometrías 2D. En polígonos, el primer y último vértice deben coincidir para cerrar el anillo.

SDO_GEOMETRY(
    2001,                              -- SDO_GTYPE: Punto 2D - Farola o Contenedor
    25830,                             -- SDO_SRID: ETRS89 / UTM zona 30N
    SDO_POINT_TYPE(720100, 4250200, NULL), -- SDO_POINT: Coordenadas X, Y
    NULL,                              -- SDO_ELEM_INFO: NULL para puntos
    NULL                               -- SDO_ORDINATES: NULL para puntos
)

## 5. Indexación Espacial: El Índice R-Tree

### 5.1. Limitaciones del B-Tree para datos espaciales

B-Tree no puede indexar (ordenar en forma de arbol para busquedas) geometrias. Sin índice espacial, a buscar realizaria **Full Table Scan**, operación con coste prohibitivo.

### 5.2. El concepto de MBR (Minimum Bounding Rectangle)

Oracle Spatial simplifica **MBR (Minimum Bounding Rectangle)** o Rectángulo Mínimo Envolvente, rectángulo de menor superficie, con lados paralelos a los ejes de coordenadas, que contiene completamente la geometría original. Comparación sencilla.

### 5.3. Arquitectura del R-Tree (Region Tree)

El **R-Tree** es la estructura de indexación espacial, funciona como un árbol jerárquico de MBRs anidados:

1.  **Nodo raíz:** Engloba la totalidad del espacio indexado (ej. todo el término municipal).
2.  **Nodos intermedios (ramas):** Subdivisión recursiva en MBRs más pequeños (ej. distritos, barrios).
3.  **Nodos hoja (leaf nodes):** Contienen los MBRs de las geometrías individuales almacenadas en la tabla.

### 5.4. Proceso de consulta en dos fases

*   **Fase 1 — Filtrado (Primary Filter):** R-Tree para identificar **candidatos** cuyo MBR se intersecta con el MBR de la geometría de consulta. Rapido, muchas comparaciones.

*   **Fase 2 — Refinamiento (Secondary Filter):** Sobre los candidatos **comparación geométrica exacta** (vértice a vértice) para eliminar los falsos positivos . Costoso, pocas comparaciones.

### 5.5. Creación y requisitos del índice espacial

Para crear un índice R-Tree -> vista `USER_SDO_GEOM_METADATA`:

1.  **Registrar metadatos:** INSERT INTO USER_SDO_GEOM_METADATA ...
2.  **Crear el índice espacial R-Tree:** CREATE INDEX idx_espacial ON tabla(columna) INDEXTYPE IS MDSYS.SPATIAL_INDEX;

## 6. Conclusión

Oracle Spatial and Graph transforma SGBDR Oracle SGBD espaciales completo. `SDO_GEOMETRY`, permite almacenar geometrías de cualquier complejidad en una única columna.

La indexación R-Tree y su arquitectura basada en MBRs resuelve el desafío de la búsqueda eficiente bidimensional mediante sus dos fases (filtrado y refinamiento), permitiendo ejecutar consultas espaciales sobre millones de registros con tiempos de respuesta óptimos para la AP.
