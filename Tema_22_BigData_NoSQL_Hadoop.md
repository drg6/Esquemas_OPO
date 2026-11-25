# Tema 22.- Big Data. Captura, análisis, transformación, almacenamiento y explotación de conjuntos masivos de datos. Entornos Hadoop o similares. Bases de datos NoSQL.

## 1. Introducción 
* **Contexto:** SGBDR (Oracle, SQL Server) son monolíticos. Fallan ante alta volumetría o desestructuración.
* **Concepto Big Data:** Tecnologías/arquitecturas para procesar datos que el paradigma relacional no soporta. Capturar, almacenar, transformar y explotar conjuntos masivos (IoT, Redes Sociales, logs)

## 2. Big Data. Las "V" del Big Data (Doug Laney)
* **Volumen:** Escala masiva (Terabytes, Petabytes). Ej: Histórico de imágenes satelitales.
* **Velocidad:** Ritmo de generación. *Batch* (procesamiento por lotes programados) vs *Streaming* (flujo continuo en tiempo real, ej. semáforos).
* **Variedad:** Estructurados (tablas), Semiestructurados (JSON/XML/GeoJSON), No estructurados (imágenes/vídeo/PDF). Ej. Emergencias e inundaciones
* **Veracidad:** Gestión del ruido y limpieza de datos. Ej.: Un sensor meteorológico averiado que marca 85 °C en invierno
* **Valor:** Utilidad real del dato. Sin valor, no tiene sentido almacenar el dato. Ej. usar datos de tráfico para reducir atascos.

## 3. Captura, análisis, transformación, almacenamiento y explotación de conjuntos masivos de datos. Ciclo de Vida 
* **3.1. Captura (Ingesta):** Recolección. *Batch* (Apache Sqoop desde SGBDR) o *Streaming/Tiempo real* (Apache Kafka, Flume para IoT/Logs).
* **3.2. Almacenamiento:** Guardar el dato bruto. Sistemas distribuidos (HDFS), Data Lakes (Amazon S3) y BD NoSQL.
* **3.3. Transformación (Procesamiento):** Limpieza, normalización y preparación. Paradigma MapReduce o motores en memoria (Apache Spark).
* **3.4. Análisis:** Aplicar lógica. Descriptivo -> Consultas SQL-on-Hadoop (Apache Hive), Predicitivo -> Machine Learning (Spark MLlib).
* **3.5. Explotación:** Consumo del dato final. Dashboards, BI (PowerBI), y cuadros de mando institucionales.

## 4. Bases de Datos NoSQL ("Not Only SQL")
* **4.1. Motivación vs SGBDR:** Sacrificar el modelo rígido ACID por flexibilidad de esquema y escalabilidad horizontal masiva.
* **4.2. Teorema CAP (Brewer):** Consistencia, Disponibilidad, Tolerancia a Particiones (solo 2 simultáneas, pero dado que particiones de red son inevitables -> **consistencia (CP)** vs **disponibilidad (AP)**).
* **4.3. Modelo BASE:**  Filosofía flexible de NoSQL que prioriza la velocidad y disponibilidad frente a la rigidez del SQL clásico. 
  - *Disponibilidad básica:* el sistema siempre responde aunque haya fallos
  - *Estado flexible:* los datos pueden variar mientras se sincronizan
  - *Consistencia eventual:* los cambios tardan unos instantes en replicarse a todos los nodos, pero al final todos ven el mismo dato.
* **4.4. Tipos:** 
  * **Clave-Valor:** Funcionan como un diccionario gigante donde cada dato se busca por un identificador único. Ultras rápidas (caché/sesiones). *Ej: Redis.*
  * **Documentales:** Almacenan datos en formato flexible tipo JSON/BSON. Expedientes heterogéneos. *Ej: MongoDB.*
  * **Columnares:** Ingesta masiva continua (IoT). *Ej: Apache Cassandra.*
  * **Grafos:** Almacenan entidades (nodos) y sus relaciones (aristas), facilitando el análisis de conexiones complejas en redes sociales, rutas o detección de fraude. *Ej: Neo4j.*

## 5. Entornos Hadoop o similares
* **5.1. Arquitectura Hadoop:** Usa escalado horizontal (*Scale-Out*), sumando muchos ordenadores normales en red (*clúster*) en lugar de un único superordenador caro.
* **5.2. HDFS (Hadoop Distributed File System - Almacenamiento):** Sistema de archivos distribuido en bloques. • El *NameNode* (maestro) guarda el mapa de dónde está cada bloque, y los *DataNodes* (esclavos) guardan los datos reales con 3 copias de seguridad (replicación 3x) por si falla un equipo.
* **5.3. MapReduce:** Paradigma clásico de procesamiento distribuido ("divide y vencerás"). Se divide en tres fases: *Map* (cada nodo procesa sus datos localmente y genera pares clave-valor), *Shuffle* (reordena y agrupa las claves iguales por la red) y *Reduce* (consolida y resume los resultados finales). Es fiable pero lento porque escribe en disco duro tras cada paso.
* **5.4. Apache Spark:** Evolución que reemplaza a MapReduce. Procesa los datos directamente en *memoria RAM* en lugar de escribir constantemente en disco, haciéndolo hasta *100x más rápido*. Soporta procesamiento por lotes (Batch), Streaming y ML.
* **5.5. Data Lake:** Almacén masivo y barato (como S3 o HDFS) donde se guardan datos en bruto tal como llegan. A diferencia del Data Warehouse (que obliga a ordenar los datos antes de guardarlos, *Schema-on-Write*), el Data Lake permite guardarlos sin orden previo y estructurarlos solo cuando se van a consultar (*Schema-on-Read*). Usa formatos como *Apache Parquet* (que agrupan datos por columnas) para ahorrar espacio y leer consultas muy rápido.

## 6. Conclusión
El Big Data rompe los límites relacionales. En la AAPP no sustituyen al SGBDR transaccional, sino que actúan en simbiosis: permiten crear *Smart Cities* (sensores), detectar patrones de fraude, gestionar expedientes masivos y explotar el *Open Data*.

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