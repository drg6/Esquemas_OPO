# Tema 39.- Sistemas de altas prestaciones. Arquitecturas escalables. Arquitecturas multinúcleo. Sistemas grandes, medios y pequeños. Servidores de datos y de aplicaciones. Centros de Proceso de Datos.

## 1. Introducción

Las Administraciones Públicas operan sistemas de información que deben atender simultáneamente a miles de usuarios: la Sede Electrónica recibe picos de tráfico durante los periodos de pago voluntario de tributos, el sistema de nóminas procesa masivamente los datos de miles de empleados, y las bases de datos del padrón municipal gestionan millones de registros. Estos escenarios exigen infraestructuras de **altas prestaciones** (High Performance Computing — HPC) que garanticen disponibilidad, rendimiento y escalabilidad.

Este tema analiza las arquitecturas escalables, los procesadores multinúcleo, la clasificación de los sistemas informáticos, los tipos de servidores y los Centros de Proceso de Datos (CPD).

## 2. Arquitecturas Escalables

### 2.1. Escalabilidad vertical (Scale-Up)

Consiste en **aumentar la capacidad de un único servidor** añadiendo más recursos hardware:
*   Más memoria RAM (de 64 GB a 512 GB).
*   Procesadores más potentes (de 8 a 64 núcleos).
*   Discos más rápidos (de HDD a NVMe SSD).

**Ventajas:** Simplicidad de gestión (un solo servidor), no requiere modificar la aplicación.
**Inconvenientes:** Coste exponencial (cada incremento es desproporcionadamente más caro), techo físico insuperable (un servidor tiene límites de memoria, CPU y slots de expansión), punto único de fallo.

### 2.2. Escalabilidad horizontal (Scale-Out)

Consiste en **añadir más servidores** al conjunto, distribuyendo la carga entre ellos:
*   De 1 servidor a un clúster de 10, 50 o cientos de servidores.
*   Se utilizan servidores commodity (estándar, económicos) en lugar de un único servidor premium.

**Ventajas:** Coste lineal, sin techo teórico, mayor tolerancia a fallos (si un nodo falla, los demás asumen la carga).
**Inconvenientes:** Mayor complejidad de gestión, requiere que la aplicación soporte distribución.

### 2.3. Clústeres

Un **clúster** es un conjunto de servidores interconectados que trabajan como una unidad lógica. Tipos:

| Tipo | Objetivo | Ejemplo |
|------|----------|---------|
| **Alta disponibilidad (HA)** | Garantizar continuidad del servicio | Oracle RAC, Windows Failover Cluster |
| **Balanceo de carga** | Distribuir peticiones entre servidores | HAProxy, F5, NGINX |
| **Computación (HPC)** | Procesamiento paralelo masivo | Apache Spark, Hadoop |

### 2.4. Balanceo de carga

Un **balanceador de carga** (Load Balancer) distribuye las peticiones de los usuarios entre múltiples servidores de forma transparente:

```
                        ┌─ Servidor Web 1
Ciudadano → Internet → [Balanceador] ─┤─ Servidor Web 2
                        └─ Servidor Web 3
```

Algoritmos de balanceo: Round Robin, Least Connections, Weighted, IP Hash.

## 3. Arquitecturas Multinúcleo (Multi-Core)

### 3.1. Evolución

Los procesadores alcanzaron un límite termodinámico en frecuencia de reloj (~4 GHz): aumentar la frecuencia genera calor insostenible. La industria evolucionó hacia el paralelismo: en lugar de un procesador más rápido, se integran múltiples **núcleos (cores)** en un único chip.

### 3.2. Procesadores actuales para servidores

| Procesador | Núcleos máx. | Uso |
|-----------|-------------|-----|
| Intel Xeon Scalable (Sapphire Rapids) | Hasta 60 cores | Servidores de propósito general |
| AMD EPYC (Genoa) | Hasta 96 cores | Servidores de alta densidad |
| ARM (Ampere Altra) | Hasta 128 cores | Cloud, eficiencia energética |

### 3.3. Conceptos relacionados

*   **Hyper-Threading / SMT:** Cada núcleo físico se presenta al sistema operativo como dos núcleos lógicos (threads), mejorando el rendimiento en cargas multihilo.
*   **NUMA (Non-Uniform Memory Access):** Arquitectura de memoria en servidores multinúcleo donde cada procesador tiene acceso rápido a su memoria local y más lento a la memoria de otros procesadores.

## 4. Clasificación de Sistemas: Grandes, Medios y Pequeños

| Categoría | Características | Uso en AAPP |
|-----------|----------------|-------------|
| **Grandes (Mainframes / HPC)** | Altísima capacidad de procesamiento, miles de cores, terabytes de RAM, disponibilidad 99,999% | Seguridad Social, Hacienda estatal, procesamiento masivo de datos |
| **Medios (Servidores departamentales)** | Rack/Blade, decenas de cores, cientos de GB de RAM | Ayuntamientos medianos/grandes, bases de datos Oracle, servidores de aplicaciones |
| **Pequeños (Workstations / Micro)** | PCs, servidores torre, NAS | Ayuntamientos pequeños, oficinas remotas |

## 5. Servidores de Datos y de Aplicaciones

### 5.1. Servidores de datos (BBDD)

Optimizados para operaciones intensivas de entrada/salida (I/O):
*   Almacenamiento en SSD NVMe de alta velocidad.
*   Gran cantidad de RAM para caché de datos.
*   Controladoras RAID y conexión a cabinas SAN (Tema 42).
*   Ejemplos de SGBD: Oracle Database, PostgreSQL, SQL Server.
*   Hardware especializado: Oracle Exadata (engineered system optimizado para Oracle DB).

### 5.2. Servidores de aplicaciones

Optimizados para procesamiento intensivo de CPU y memoria:
*   Alta capacidad de procesamiento (muchos cores y threads).
*   Gran cantidad de RAM para la JVM y la gestión de sesiones.
*   Ejecutan servidores de aplicaciones Java EE (Tomcat, WildFly, WebLogic) o frameworks (Spring Boot).
*   No requieren almacenamiento masivo local (los datos residen en los servidores de BBDD).

### 5.3. Otros tipos de servidores

*   **Servidores web:** NGINX, Apache HTTP Server (frontend que recibe las peticiones HTTP).
*   **Servidores de correo:** Microsoft Exchange, Postfix.
*   **Servidores de ficheros:** Windows File Server, Samba, NAS.
*   **Servidores de directorio:** Active Directory (Windows Server), OpenLDAP.

## 6. Centros de Proceso de Datos (CPD)

### 6.1. Concepto

Un **Centro de Proceso de Datos (CPD o Data Center)** es la instalación física que alberga la infraestructura informática crítica de una organización: servidores, cabinas de almacenamiento, equipos de red, sistemas de comunicaciones y sistemas auxiliares.

### 6.2. Elementos fundamentales

*   **Racks:** Armarios estándar de 19 pulgadas (42U de altura) donde se montan los servidores, switches y patch panels.
*   **Suministro eléctrico redundante:**
    *   Dos acometidas eléctricas independientes.
    *   Sistemas de Alimentación Ininterrumpida (SAI/UPS) para absorber microcortes.
    *   Generadores diésel de emergencia para cortes prolongados.
*   **Climatización:**
    *   Pasillo frío / pasillo caliente: separación de flujos de aire para optimizar la refrigeración.
    *   Sistemas de aire acondicionado de precisión (CRAC — Computer Room Air Conditioning).
    *   Temperatura objetivo: 18-27 °C (recomendación ASHRAE).
*   **Protección contra incendios:** Sistemas de detección VESDA (Very Early Smoke Detection Apparatus) y extinción por gas (Novec 1230, FM-200) que no dañan los equipos.
*   **Control de acceso físico:** Lectores biométricos, tarjetas de proximidad, videovigilancia, registro de accesos.
*   **Cableado:** Cableado estructurado de cobre y fibra óptica, canalizaciones separadas para datos y alimentación.

### 6.3. Clasificación TIER (Uptime Institute)

El Uptime Institute clasifica los CPDs en cuatro niveles según su disponibilidad:

| Tier | Disponibilidad | Horas de caída/año | Características |
|------|---------------|--------------------|----------------|
| **Tier I** | 99,671% | 28,8 h | Sin redundancia |
| **Tier II** | 99,741% | 22,7 h | Componentes redundantes |
| **Tier III** | 99,982% | 1,6 h | Mantenimiento sin parada (Concurrently Maintainable) |
| **Tier IV** | 99,995% | 0,4 h | Tolerante a fallos (Fault Tolerant) |

### 6.4. CPD de respaldo (DRP Site)

Para garantizar la continuidad del servicio ante desastres (incendio, inundación, terremoto), las organizaciones deben disponer de un **centro de respaldo** geográficamente separado:

*   **Cold Site:** Infraestructura básica (electricidad, red) sin servidores. Tiempo de recuperación: días.
*   **Warm Site:** Infraestructura con servidores parcialmente configurados. Tiempo de recuperación: horas.
*   **Hot Site:** Réplica exacta del CPD principal, con datos sincronizados. Tiempo de recuperación: minutos.

## 7. Conclusión

Los sistemas de altas prestaciones, con sus arquitecturas escalables (vertical y horizontal), procesadores multinúcleo y servidores especializados (datos, aplicaciones, web), proporcionan la capacidad de procesamiento que los sistemas de información de las Administraciones Públicas necesitan. Los Centros de Proceso de Datos (CPD), con sus rigurosos requisitos de suministro eléctrico redundante, climatización, protección contra incendios y clasificación TIER, garantizan la infraestructura física necesaria para alcanzar los niveles de disponibilidad que el ENS exige para los sistemas de categoría Media y Alta.
