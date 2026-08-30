# Tema 22.- Big Data. Captura, análisis, transformación, almacenamiento y explotación de conjuntos masivos de datos. Entornos Hadoop o similares. Bases de datos NoSQL.

## 1. Introducción
* **Contexto:** SGBDR (Oracle, PostgreSQL) son perfectos para transacciones e integridad, pero su arquitectura monolítica colapsa ante gran volumen/velocidad/variedad.
* **Concepto Big Data:** Tecnologías, arquitecturas y metodologías para capturar, almacenar, transformar y explotar conjuntos masivos (IoT, Redes Sociales, logs).

## 2. Las "V" del Big Data (Doug Laney)
* **Volumen:** Escala masiva (Terabytes, Petabytes). Ej: Histórico satelital.
* **Velocidad:** Ritmo de generación. *Batch* (lotes) vs *Streaming* (tiempo real, ej. semáforos).
* **Variedad:** Estructurados (tablas), Semiestructurados (JSON/XML), No estructurados (vídeo/PDF).
* **Veracidad:** Gestión del ruido y limpieza de datos.
* **Valor:** Extracción de conocimiento accionable. Sin valor, no tiene sentido.

## 3. El Ciclo de Vida del Dato (Big Data Pipeline)
* **3.1. Captura (Ingesta):** *Batch* (Apache Sqoop desde SGBDR) o *Streaming* (Apache Kafka, Flume para IoT/Logs).
* **3.2. Almacenamiento:** Distribuido (*Scale-Out*). HDFS, Nube (Amazon S3) y BD NoSQL.
* **3.3. Transformación:** Limpieza de datos en bruto (*raw*). Procesando in-situ mediante MapReduce o Apache Spark.
* **3.4. Análisis:** Descriptivo (SQL con Apache Hive) o Predictivo (Machine Learning con Spark MLlib).
* **3.5. Explotación:** Capa de consumo. Herramientas BI (PowerBI), APIs y cuadros de mando.

## 4. Bases de Datos NoSQL ("Not Only SQL")
* **4.1. Motivación y Teorema CAP:** 
  * ACID tradicional bloquea la escalabilidad horizontal.
  * **Teorema CAP (Brewer):** Consistencia, Disponibilidad, Tolerancia a Particiones (solo 2 simultáneas, pero dado que particiones de red son inevitables -> **consistencia (CP)** vs **disponibilidad (AP)**).
  * NoSQL usa el **Modelo BASE:** Basically Available, Soft state, Eventually consistent.
* **4.2. Tipologías NoSQL:** 
  * **Clave-Valor:** Ultras rápidas (caché/sesiones). *Ej: Redis.*
  * **Documentales:** Esquema dinámico (JSON/BSON). Expedientes heterogéneos. *Ej: MongoDB.*
  * **Columnares:** Ingesta masiva continua (IoT). *Ej: Apache Cassandra.*
  * **Grafos:** Nodos/aristas (Redes sociales/fraude). *Ej: Neo4j.*

## 5. Entornos Hadoop y Arquitecturas Modernas
* **5.1. HDFS y MapReduce:** Núcleo de Hadoop.
  * **HDFS (Almacenamiento):** Bloques de 128MB. `NameNode` (metadatos) y `DataNodes` (datos). Replicación x3.
  * **MapReduce (Procesamiento):** Código viaja al dato. Fases: *Map* (local), *Shuffle* (red de nodos), *Reduce* (consolidación).
* **5.2. Apache Spark:** Revolución en **RAM**. 100x más rápido que MapReduce. Unifica Batch, Streaming, SQL y Machine Learning.
* **5.3. El Data Lake:** Repositorio central de datos en bruto (HDFS/S3). *Schema-on-Read* (se estructura al leer) vs *Schema-on-Write* del Data Warehouse. Formatos columnares (Apache Parquet).

## 6. Conclusión

El Big Data rompe los límites relacionales. En la AAPP no sustituyen al SGBDR transaccional, sino que actúan en simbiosis: permiten crear *Smart Cities* (sensores), detectar patrones de fraude, gestionar expedientes masivos y explotar el *Open Data*.