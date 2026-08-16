# Tema 22.- Big Data. Captura, análisis, transformación, almacenamiento y explotación de conjuntos masivos de datos. Entornos Hadoop o similares. Bases de datos NoSQL.

## 1. Introducción

Los Sistemas Gestores de Bases de Datos Relacionales (SGBDR) convencionales — Oracle, PostgreSQL, SQL Server — fueron diseñados en los años 70 para gestionar transacciones estructuradas: altas en el padrón, liquidaciones tributarias, registros contables. Estos sistemas procesan eficientemente millones de filas estructuradas en tablas relacionales, pero su arquitectura monolítica (escalado vertical) y su modelo rígido de esquema predefinido se ven desbordados cuando el volumen, la velocidad o la variedad de los datos exceden las capacidades de un único servidor.

En el siglo XXI, la proliferación de dispositivos IoT (sensores de calidad del aire, contadores inteligentes de agua), redes sociales, registros de tráfico web, imágenes de videovigilancia y registros de dispositivos móviles genera cantidades de datos que no encajan en el paradigma relacional tradicional. El concepto de **Big Data** engloba las tecnologías, arquitecturas y metodologías diseñadas para capturar, almacenar, transformar y explotar estos conjuntos masivos de datos.

## 2. Las "V" del Big Data

Para que un proyecto se considere Big Data, debe exhibir al menos una combinación significativa de las siguientes características, formuladas originalmente por Doug Laney (Gartner, 2001) y ampliadas posteriormente:

1.  **Volumen:** Magnitudes de datos que exceden la capacidad de procesamiento de un servidor convencional (terabytes, petabytes, exabytes). Ejemplo: el histórico completo de imágenes de satélite del territorio nacional.

2.  **Velocidad:** Ritmo al que se generan y deben procesarse los datos. Distingue entre procesamiento **batch** (por lotes, con latencia de horas) y procesamiento en **streaming** (tiempo real o casi real, con latencia de milisegundos). Ejemplo: el flujo continuo de datos de sensores de tráfico urbano que deben procesarse en tiempo real para ajustar los semáforos.

3.  **Variedad:** Datos en múltiples formatos heterogéneos:
    *   **Estructurados:** Tablas relacionales (filas y columnas con esquema fijo).
    *   **Semiestructurados:** JSON, XML, logs de servidor.
    *   **No estructurados:** Imágenes, vídeos, documentos PDF, correos electrónicos.

4.  **Veracidad:** El desafío de gestionar la incertidumbre, el ruido y las inconsistencias presentes en conjuntos masivos de datos de origen diverso. Requiere procesos de limpieza y validación.

5.  **Valor:** La capacidad de extraer información útil y conocimiento accionable de la masa de datos brutos. Sin generación de valor, el almacenamiento masivo carece de sentido.

## 3. Bases de Datos NoSQL ("Not Only SQL")

### 3.1. Motivación

Las bases de datos relacionales garantizan las propiedades **ACID** (Atomicidad, Consistencia, Aislamiento, Durabilidad), fundamentales para operaciones transaccionales. Sin embargo, estas garantías imponen restricciones que limitan la escalabilidad horizontal (distribución del procesamiento entre múltiples servidores).

Las bases de datos **NoSQL** surgieron como alternativa para escenarios donde la escalabilidad horizontal, la flexibilidad de esquema y el rendimiento en lectura/escritura masiva son prioritarios sobre la consistencia transaccional estricta.

### 3.2. Teorema CAP (Brewer, 2000)

El teorema CAP establece que un sistema distribuido no puede garantizar simultáneamente las tres propiedades:

*   **Consistency (Consistencia):** Todos los nodos ven los mismos datos al mismo tiempo.
*   **Availability (Disponibilidad):** Toda petición recibe una respuesta, aunque los datos no estén completamente actualizados.
*   **Partition Tolerance (Tolerancia a particiones de red):** El sistema sigue funcionando aunque se produzcan fallos de comunicación entre nodos.

En la práctica, dado que las particiones de red son inevitables en sistemas distribuidos, la elección real es entre **consistencia (CP)** y **disponibilidad (AP)**.

### 3.3. Modelo BASE vs. ACID

Las bases NoSQL adoptan frecuentemente el modelo **BASE**:
*   **Basically Available:** El sistema garantiza disponibilidad.
*   **Soft state:** El estado puede cambiar con el tiempo, incluso sin nuevas entradas.
*   **Eventually consistent:** La consistencia se alcanza eventualmente, no de forma inmediata.

### 3.4. Tipos de bases de datos NoSQL

| Tipo | Modelo de datos | Ejemplos | Caso de uso |
|------|----------------|----------|-------------|
| **Clave-Valor** | Pares clave → valor (blob opaco) | Redis, DynamoDB, Memcached | Caché, gestión de sesiones, colas de mensajes |
| **Documentales** | Documentos JSON/BSON con esquema flexible | MongoDB, CouchDB | Catálogos con atributos variables, CMS, gestión de expedientes |
| **Familia de columnas** | Tablas anchas con columnas dinámicas por fila | Apache Cassandra, HBase | Series temporales (sensores IoT), registros de eventos, logs masivos |
| **Grafos** | Nodos y aristas con propiedades | Neo4j, AWS Neptune, Oracle PGX | Redes sociales, detección de fraude, análisis de dependencias |

### 3.5. MongoDB: Ejemplo documental

```javascript
// Documento de un expediente municipal en MongoDB
{
    "_id": "EXP-2026-001234",
    "tipo": "LICENCIA_OBRA",
    "solicitante": {
        "dni": "12345678Z",
        "nombre": "María García López"
    },
    "documentos": [
        { "nombre": "proyecto_basico.pdf", "tamaño_mb": 45, "fecha": "2026-01-15" },
        { "nombre": "estudio_geologico.pdf", "tamaño_mb": 12, "fecha": "2026-01-20" }
    ],
    "estado": "EN_TRAMITE",
    "informes": []
}
```

A diferencia de una tabla relacional, cada documento puede tener una estructura diferente: un expediente de licencia de obra incluye `documentos` y `informes`, mientras que un expediente de queja ciudadana incluiría `respuestas` y `satisfaccion`.

## 4. Entornos Hadoop y Ecosistema

### 4.1. Apache Hadoop: Concepto

**Apache Hadoop** (2006) es un framework de código abierto para el almacenamiento y procesamiento distribuido de conjuntos masivos de datos. Su filosofía se basa en el **escalado horizontal (Scale-Out)**: en lugar de comprar un servidor más potente (Scale-Up), Hadoop distribuye el trabajo entre cientos de servidores económicos (commodity hardware) interconectados en un clúster.

### 4.2. HDFS (Hadoop Distributed File System)

HDFS es el sistema de archivos distribuido de Hadoop. Toma un archivo de gran tamaño y lo fragmenta en **bloques** de tamaño fijo (128 MB por defecto), distribuyéndolos y replicándolos entre múltiples servidores del clúster:

*   **NameNode (maestro):** Almacena los metadatos del sistema de archivos (qué bloques componen cada fichero, en qué DataNodes están almacenados).
*   **DataNodes (esclavos):** Almacenan los bloques de datos reales.
*   **Factor de replicación:** Cada bloque se replica en 3 DataNodes por defecto, garantizando la tolerancia a fallos. Si un DataNode falla, los datos están disponibles en las réplicas.

### 4.3. MapReduce: Procesamiento distribuido

**MapReduce** es el modelo de programación para procesamiento batch distribuido, inspirado en el artículo de Google "MapReduce: Simplified Data Processing on Large Clusters" (2004):

1.  **Fase Map:** En lugar de mover los datos hacia el procesador, MapReduce envía el programa a cada DataNode donde residen los datos. Cada nodo ejecuta la función Map sobre sus bloques locales, generando pares clave-valor intermedios.

2.  **Fase Shuffle:** Los pares intermedios se agrupan por clave y se redistribuyen entre los nodos.

3.  **Fase Reduce:** Cada nodo aplica la función Reduce sobre los pares agrupados, produciendo el resultado final.

### 4.4. Ecosistema Hadoop

Hadoop ha evolucionado hacia un ecosistema completo de herramientas:

| Herramienta | Función |
|-------------|---------|
| **Apache Hive** | Consultas SQL sobre datos en HDFS (SQL-on-Hadoop) |
| **Apache Pig** | Lenguaje de scripting para transformación de datos |
| **Apache Spark** | Motor de procesamiento en memoria, hasta 100x más rápido que MapReduce |
| **Apache Kafka** | Plataforma de streaming de eventos en tiempo real |
| **Apache HBase** | Base de datos NoSQL columnar sobre HDFS |
| **Apache Sqoop** | Transferencia de datos entre HDFS y SGBDR (Oracle, MySQL) |
| **Apache Flume** | Ingesta de datos de logs y streams |
| **YARN** | Gestor de recursos del clúster (sucesor del JobTracker original) |
| **Apache Zookeeper** | Coordinación y configuración distribuida |

### 4.5. Apache Spark: La evolución

**Apache Spark** ha reemplazado progresivamente a MapReduce como motor de procesamiento preferido. Sus ventajas:

*   **Procesamiento en memoria:** Almacena datos intermedios en RAM, evitando las costosas escrituras a disco de MapReduce.
*   **APIs en múltiples lenguajes:** Scala, Java, Python (PySpark), R.
*   **Procesamiento batch y streaming:** Spark Structured Streaming procesa datos en tiempo real.
*   **Machine Learning integrado:** Spark MLlib proporciona algoritmos de aprendizaje automático distribuido.

## 5. Arquitectura de Datos Moderna: Data Lake

Un **Data Lake** es un repositorio centralizado que almacena datos en bruto (raw data) en su formato original — estructurados, semiestructurados y no estructurados — sin necesidad de transformarlos previamente a un esquema relacional. A diferencia del Data Warehouse (Tema 23), que almacena datos transformados y estructurados, el Data Lake almacena los datos tal como llegan y los transforma cuando se necesitan (schema-on-read vs. schema-on-write).

**Plataformas Data Lake actuales:**
*   **Cloud:** Amazon S3 + AWS Glue, Azure Data Lake Storage, Google Cloud Storage.
*   **On-premise:** HDFS + Apache Spark.
*   **Formatos optimizados:** Apache Parquet (columnar), Apache ORC, Delta Lake.

## 6. Conclusión

Big Data, con sus cinco "V", ha transformado la forma en que las organizaciones almacenan y explotan la información. Las bases de datos NoSQL (clave-valor, documentales, columnares, grafos) complementan a los SGBDR tradicionales en escenarios de alta escalabilidad y flexibilidad de esquema. Apache Hadoop democratizó el procesamiento distribuido, y Apache Spark lo aceleró con su motor en memoria.

Para las Administraciones Públicas, estas tecnologías abren posibilidades en el análisis de datos de sensores urbanos (Smart Cities), la detección de patrones de fraude fiscal, el procesamiento de grandes volúmenes de expedientes y la explotación de datos abiertos, complementando los sistemas transaccionales Oracle que gestionan las operaciones diarias del municipio.
