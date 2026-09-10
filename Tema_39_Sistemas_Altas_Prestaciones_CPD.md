# Tema 39.- Altas prestaciones, arquitecturas (escalables/multinúcleo), sistemas y CPD.

## 1. Introducción
* **Contexto AAPP:** Procesar nóminas masivas, picos de tráfico en pago de IBI y gestionar el Padrón exige infraestructuras HPC (High Performance Computing).
* **Objetivo:** Garantizar disponibilidad 24x7, rendimiento sostenido y escalabilidad según las exigencias del Esquema Nacional de Seguridad (ENS).

## 2. Arquitecturas Escalables (El reto del crecimiento)
* **2.1. Escalabilidad Vertical (Scale-Up):**
  * Más músculo en 1 servidor (más RAM, más CPU).
  * *Ventajas:* Simple, no hay que tocar el código de la app.
  * *Inconveniente:* Coste exponencial y punto único de fallo (SPOF).
* **2.2. Escalabilidad Horizontal (Scale-Out):**
  * Sumar más servidores *commodity* (baratos) a la granja.
  * *Ventajas:* Coste lineal, tolerancia a fallos.
  * *Inconveniente:* Complejidad (requiere que la aplicación soporte distribución).
* **2.3. Clústeres y Balanceo:** 
  * Unión lógica de servidores (Nodos). 
  * Tipos: Alta Disponibilidad (HA - Ej. Oracle RAC) y Balanceo de Carga (HAProxy/F5) usando algoritmos como *Round Robin* o *Least Connections*.

## 3. Arquitecturas Multinúcleo (Multi-Core)
* **3.1. Evolución:** Al topar con el límite térmico (frecuencia estancada en ~4 GHz), la industria opta por el paralelismo: meter muchos cerebros (cores) en un solo chip.
* **3.2. Mercado Actual:** Intel Xeon (Sapphire Rapids, 60+ cores) y AMD EPYC (Genoa, 96+ cores).
* **3.3. Optimizaciones:** **Hyper-Threading / SMT** (2 hilos lógicos por cada core físico) y **NUMA** (arquitectura de memoria segmentada para acceso ultra-rápido).

## 4. Clasificación de Sistemas
* **Grandes (Mainframes):** Uso masivo (Hacienda, TGSS). Redundancia absoluta.
* **Medios:** Servidores Rack/Blade, el estándar en el CPD del Ayuntamiento para virtualización corporativa y bases de datos.
* **Pequeños (Puesto de Usuario):** La microinformática actual huye del PC tradicional hacia portátiles ligeros o *Thin Clients* que se conectan a infraestructuras VDI (Escritorios Virtuales) albergadas en los sistemas medios.

## 5. Servidores de Datos vs. Aplicaciones (La Arquitectura 3 Capas)
En la AAPP moderna (ej. Portal del Ciudadano), se impone la **Arquitectura en 3 Capas**, aislando funciones por seguridad (ENS) y rendimiento:
* **5.1. Servidores Web (Capa Presentación):** NGINX o Apache en la DMZ.
* **5.2. Servidores de Aplicaciones (Capa Negocio):** Optimizados para **CPU y RAM** (Tomcat, Spring Boot, WebLogic). Procesan la lógica y consultan los datos.
* **5.3. Servidores de Datos / BBDD (Capa Datos):** Optimizados para intensividad **I/O** (Lectura/Escritura). Usan discos NVMe SSD y mucha RAM para caché. (Oracle, SQL Server, PostgreSQL). Protegidos en la LAN más restrictiva.

## 6. Centros de Proceso de Datos (CPD)
El "santuario" físico de la infraestructura (Art. 73 ENS - Protección de las instalaciones).
* **6.1. Componentes Físicos:** Racks (19"), falso suelo (para cableado) y pasillos frío/caliente separados.
* **6.2. Subsistemas Críticos:**
  * *Energía:* Acometida dual + SAI/UPS + Grupo electrógeno diésel.
  * *Clima:* CRAC (Precisión, 18-27ºC).
  * *Fuego:* Detección temprana VESDA y extinción limpia (gas Novec/FM-200, nunca agua).
* **6.3. Clasificación Uptime Institute (TIER):**
  * TIER I (Básico, 99.6%) → TIER II → TIER III (Mantenimiento sin parada) → **TIER IV (Tolerante a Fallos, 99.995%)**.
* **6.4. Recuperación de Desastres (DRP):** 
  * Necesidad de centro de respaldo: *Cold Site* (días), *Warm Site* (horas), *Hot Site* (minutos/sincronizado).
  * *Tendencia AAPP:* Migrar el sitio de respaldo a una **Nube Pública certificada en el nivel Alto del ENS** para garantizar la soberanía del dato y ahorrar costes inmobiliarios.

## 7. Conclusión
El servicio público digital del siglo XXI no admite interrupciones. La respuesta técnica requiere orquestar servidores multinúcleo en arquitecturas de 3 capas con escalabilidad horizontal, todo ello albergado en CPDs diseñados bajo los estándares TIER y respaldados por planes DRP. Solo esta infraestructura HPC (Hardware) asegura la base sobre la que descansan las garantías de disponibilidad del Esquema Nacional de Seguridad (Normativa).

-------------------------------------

# Tema 39.- Sistemas de altas prestaciones. Arquitecturas escalables. Arquitecturas multinúcleo. Sistemas grandes, medios y pequeños. Servidores de datos y de aplicaciones. Centros de Proceso de Datos.

## 1. Introducción

Sede -> picos de trafico periodos de pago voluntario de tributos
Nóminas -> procesar datos miles empleados
Padrón -> gestionar millones de registros
**Sistemas de altas prestaciones (High Performance Computing — HPC):** infraestructuras orientadas a garantizar **rendimiento, disponibilidad y escalabilidad**.

## 2. Arquitecturas Escalables

### 2.1. Escalabilidad vertical (Scale-Up)

- Aumentar recursos de **un único servidor**: CPU, RAM, almacenamiento.
- **Ventajas:** simplicidad, no requiere modificar la aplicación.
- **Inconvenientes:** Coste exponencial, límite físico y punto único de fallo.

### 2.2. Escalabilidad horizontal (Scale-Out)

- Añadir **más servidores** y distribuir la carga. Cluster. 
- Varios Servidores commodity vs Servidor premium único
- **Ventajas:** mayor escalabilidad (coste lineal) y tolerancia a fallos.
- **Inconvenientes:** mayor complejidad y aplicaciones preparadas para funcionar distribuidamente.

### 2.3. Clústeres

- Conjunto de servidores que funcionan como una **unidad lógica**.
- Tipos:
  - **HA:** continuidad del servicio (Oracle RAC, Windows Failover Cluster)
  - **Balanceo de carga:** distribución de peticiones (HAProxy, F5, NGINX)
  - **Computación (HPC - High-Performance Computing):** procesamiento paralelo masivo (Apache Spark, Hadoop)

### 2.4. Balanceo de carga

- Distribuye las peticiones entre varios servidores.
- Algoritmos: **Round Robin, Least Connections, Weighted e IP Hash**.

## 3. Arquitecturas Multinúcleo (Multi-Core)

### 3.1. Evolución

Los procesadores alcanzaron un límite termodinámico en frecuencia de reloj (~4 GHz): aumentar la frecuencia genera calor insostenible.
Integran múltiples **núcleos (cores)** en un procesador para aumentar el paralelismo y rendimiento.

### 3.2. Procesadores actuales para servidores

- Intel Xeon Scalable (Sapphire Rapids) -> Hasta 60 cores (Servidores de propósito general)
- AMD EPYC (Genoa) -> Hasta 96 cores (Servidores de alta densidad)
- ARM (Ampere Altra) -> Hasta 128 cores (Cloud, eficiencia energética)

### 3.3. Conceptos relacionados

- **Hyper-Threading / SMT:** varios hilos lógicos por núcleo.
- **NUMA (Non-Uniform Memory Access):** acceso más rápido a la memoria local que a la memoria de otros procesadores.

## 4. Clasificación de Sistemas: Grandes, Medios y Pequeños

- **Grandes:** Mainframes/HPC → procesamiento masivo y máxima disponibilidad (Seguridad Social, Hacienda estatal)
- **Medios:** servidores departamentales → aplicaciones y BBDD corporativas.
- **Pequeños:** PCs, servidores torre o NAS → pequeñas organizaciones y oficinas.
La microinformática actual huye del PC tradicional hacia portátiles ligeros o *Thin Clients* que se conectan a infraestructuras VDI (Escritorios Virtuales) albergadas en los sistemas medios.

## 5. Servidores de Datos y de Aplicaciones

En la AAPP moderna (ej. Portal del Ciudadano), se impone la **Arquitectura en 3 Capas - Datos, Negocio y Presentación**, aislando funciones por seguridad (ENS) y rendimiento.

### 5.1. Servidores de datos (BBDD) -> Capa Datos

- Optimizados para **operaciones I/O**.
- Características: **SSD/NVMe, gran RAM, RAID/cabinas SAN**.
- Ejemplos: **Oracle, PostgreSQL, SQL Server**.

### 5.2. Servidores de aplicaciones -> Capa Negocio

- Optimizados para **CPU y memoria**.
- Ejecutan aplicaciones y lógica de negocio. 
- No requieren almacenamiento masivo, datos en servidores BBDD
- Ejemplos: **Tomcat, WildFly, WebLogic, Spring Boot**.

### 5.3. Otros tipos de servidores

*   **Servidores web:** NGINX, Apache -> Capa Presentación
*   **Servidores de correo:** Microsoft Exchange, Postfix.
*   **Servidores de ficheros:** Windows File Server, Samba, NAS.
*   **Servidores de directorio:** Active Directory (Windows Server), OpenLDAP.

## 6. Centros de Proceso de Datos (CPD)

### 6.1. Concepto

Instalación física que alberga la **infraestructura informática crítica**: servidores, almacenamiento, redes. comunicaciones y sistemas auxiliares.

### 6.2. Elementos fundamentales

- **Racks** de 19".
- **Alimentación redundante:** acometidas eléctricas independientes, Sistemas de Alimentación Ininterrumpida (SAI/UPS) y generadores diesel.
- **Climatización:** pasillo frío/caliente y Sistemas de aire acondicionado de precisión (CRAC — Computer Room Air Conditioning). 18-27 °C (recomendación ASHRAE)
- **Protección contra incendios:** detección temprana VESDA (Very Early Smoke Detection Apparatus) y extinción por gas que no dañan los equipos.
- **Seguridad física:** control de acceso y videovigilancia.
- **Cableado:** cobre y fibra, separado de la alimentación.

### 6.3. Clasificación TIER (Uptime Institute)

Según disponibilidad:
- **Tier I:** sin redundancia (99,671%)
- **Tier II:** componentes redundantes (99,741%)
- **Tier III:** mantenimiento sin interrupción (99,982%)
- **Tier IV:** tolerante a fallos (99,995%)

### 6.4. CPD de respaldo (DRP Site)

Para garantizar la continuidad del servicio ante desastres (incendio, inundación, terremoto), las organizaciones deben disponer de un **centro de respaldo** geográficamente separado:
- **Cold Site:** infraestructura básica (electricidad, red) sin servidores → recuperación en días.
- **Warm Site:** Infraestructura con servidores parcialmente configurados → recuperación en horas.
- **Hot Site:** réplica sincronizada → recuperación en minutos.

* *Tendencia AAPP:* Migrar el sitio de respaldo a una **Nube Pública certificada en el nivel Alto del ENS** para garantizar la soberanía del dato y ahorrar costes inmobiliarios.

## 7. Conclusión

- Las **arquitecturas escalables y multinúcleo** permiten aumentar la capacidad de procesamiento.
- Los **servidores especializados** optimizan la ejecución de aplicaciones y el tratamiento de datos.
- El **CPD**, junto con redundancia y centros de respaldo, garantiza la **disponibilidad y continuidad** de los sistemas que el ENS para sistemas de categoría Media y Alta.