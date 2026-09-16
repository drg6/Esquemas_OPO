# Tema 22.- Big Data. Captura, análisis, transformación, almacenamiento y explotación...

## 1. Introducción y las "V" del Big Data
* **Contexto:** SGBDR (Oracle, SQL Server) son monolíticos. Fallan ante alta volumetría o desestructuración.
* **Concepto Big Data:** Tecnologías/arquitecturas para procesar datos que el paradigma relacional no soporta.
* **Las 5 "V" (Doug Laney):** Volumen (escala), Velocidad (batch vs streaming), Variedad (estructurado, semi, no estructurado), Veracidad (ruido/calidad) y Valor (utilidad final).

## 2. Ciclo de Vida (El Pipeline de Datos)
* **2.1. Captura (Ingesta):** Recolección. Batch (Apache Sqoop desde SGBDR) o Streaming/Tiempo real (Apache Kafka, Flume para IoT/Logs).
* **2.2. Almacenamiento:** Guardar el dato bruto. Sistemas distribuidos (HDFS), Data Lakes (Amazon S3) y BD NoSQL.
* **2.3. Transformación (Procesamiento):** Limpieza, normalización y preparación. Paradigma MapReduce o motores en memoria (Apache Spark).
* **2.4. Análisis:** Aplicar lógica. Consultas SQL-on-Hadoop (Apache Hive) o Machine Learning (Spark MLlib).
* **2.5. Explotación:** Consumo del dato final. Dashboards, BI, y cuadros de mando institucionales.

## 3. Bases de Datos NoSQL
* **3.1. Motivación vs SGBDR:** Sacrificar el modelo rígido ACID por flexibilidad de esquema y escalabilidad horizontal masiva.
* **3.2. Teorema CAP (Brewer):** Consistencia, Disponibilidad, Tolerancia a Particiones. Solo puedes elegir 2 (CP o AP).
* **3.3. Modelo BASE:** Disponibilidad básica, Estado flexible, Consistencia Eventual.
* **3.4. Tipos:** 
  * Clave-Valor (Redis - cachés).
  * Documentales (MongoDB - JSON flexible, expedientes).
  * Columnares (Cassandra - series temporales/IoT).
  * Grafos (Neo4j - relaciones).

## 4. Entornos Hadoop, Spark y Data Lakes
* **4.1. Arquitectura Hadoop:** Escalado horizontal (*Scale-Out*). 
* **4.2. HDFS (Almacenamiento):** Sistema de archivos distribuido en bloques. `NameNode` (maestro/metadatos) y `DataNodes` (esclavos/datos). Factor de replicación 3x.
* **4.3. MapReduce:** Paradigma clásico. *Map* (procesa localmente) → *Shuffle* (agrupa) → *Reduce* (consolida).
* **4.4. Apache Spark:** Evolución. Procesamiento en memoria (RAM), 100x más rápido. Soporta Batch, Streaming y ML.
* **4.5. Data Lake:** Repositorio central de datos brutos sin esquema (*schema-on-read*). Formatos columnares (Parquet).

## 5. Conclusión
Revolución del almacenamiento/procesamiento. Valor en la AAPP: Smart Cities (sensores), detección de fraude y analítica avanzada, complementando (no sustituyendo) al SGBDR transaccional clásico (ENS, Oracle).

----------------------

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