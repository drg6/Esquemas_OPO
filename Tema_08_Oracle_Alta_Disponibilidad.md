# Tema 8.- El SGBDR Oracle: Alta Disponibilidad (Data Guard y RAC).

## 1. Introducción
* **Imperativo en AAPP:** Continuidad ininterrumpida en servicios críticos (sede electrónica, recaudación, padrón) alineada con la dimensión de **Disponibilidad** del Esquema Nacional de Seguridad (ENS).
* **Estrategia de Alta Disponibilidad (HA):** Enmascarar fallos físicos o lógicos mediante redundancia activa: **Oracle RAC** (protección frente a caída de nodo/servidor dentro del mismo CPD) y **Oracle Data Guard** (protección frente a desastres completos de CPD - *Disaster Recovery*).

## 2. Fundamentos y Métricas de Alta Disponibilidad
* **Métricas clave:**
  * **MTBF (*Mean Time Between Failures*):** Tiempo medio entre fallos (fiabilidad; se maximiza con hardware redundante).
  * **MTTR (*Mean Time To Repair/Recovery*):** Tiempo medio de recuperación (se minimiza con *failover* automático).
  * **Fórmula de Disponibilidad:** $$\text{Disponibilidad} = \frac{\text{MTBF}}{\text{MTBF} + \text{MTTR}}$$
* **Escala de los "Nueves" (Inactividad anual):**
  * **99% (2 nueves):** ~3,65 días | **99,9% (3 nueves):** ~8,76 horas.
  * **99,99% (4 nueves):** ~52,6 minutos | **99,999% (5 nueves):** ~5,26 minutos (objetivo Tier 1 en AAPP).

## 3. Oracle RAC (Real Application Clusters)
* **3.1. Paradigma Activo-Activo:**
  * Múltiples nodos procesan carga simultáneamente (**N Instancias en RAM $\leftrightarrow$ 1 única Base de Datos compartida en disco**), evitando tener hardware pasivo ocioso.
* **3.2. Componentes Arquitectónicos:**
  * **Almacenamiento compartido:** Red SAN/NAS gestionada mediante **Oracle ASM (*Automatic Storage Management*)**, que automatiza *striping*, espejado y rebalanceo de discos.
  * **Interconexión Privada (*Interconnect*):** Red dedicada de baja latencia (10/25 GbE o InfiniBand) para latidos (*heartbeats*) y tráfico de memoria entre nodos.
* **3.3. Cache Fusion y GRD:**
  * **Cache Fusion:** Transfiere bloques de datos directamente entre las memorias SGA (Buffer Cache) de los nodos por la red privada sin pasar por disco, reduciendo drásticamente la E/S.
  * **GRD (*Global Resource Directory*):** Directorio distribuido en memoria que coordina la propiedad y bloqueos de cada bloque en el clúster.
* **3.4. Balanceo, Failover y Acceso:**
  * **SCAN (*Single Client Access Name*):** Nombre único DNS resuelto en 3 IPs virtuales; desacopla a los clientes de la topología física del clúster.
  * **Failover transparente:** **TAF** (*Transparent Application Failover*, migra sesiones y cursores SELECT abiertos) y **FAN** (*Fast Application Notification*, alerta inmediata de caída de nodo vía ONS).
  * **Oracle Clusterware:** Gestiona membresía del clúster y aplica *fencing/eviction* mediante discos de votación (*Voting Disks*) para evitar corrupción por **Split-Brain** (cerebro dividido).

## 4. Oracle Data Guard (Recuperación ante Desastres)
* **4.1. Arquitectura Primaria - Standby:**
  * Réplica geográficamente separada sincronizada mediante el envío continuo de **Redo Logs** desde la BD Primaria hacia una o varias BD Standby.
* **4.2. Tipos de Base de Datos Standby:**
  * **Physical Standby (Réplica bloque a bloque):** Sincronizada mediante el proceso **MRP (*Managed Recovery Process*)** (*Redo Apply*).Con la opción **Active Data Guard**, permite abrir la réplica en solo lectura mientras aplica cambios en tiempo real (ideal para descargar consultas pesadas de reporting y backups RMAN).
  * **Logical Standby (Réplica SQL):** Transforma los redo logs en sentencias SQL mediante **SQL Apply**, permitiendo índices o tablas adicionales en destino.
* **4.3. Modos de Protección (Compromiso Rendimiento vs. RPO):**
  * **Maximum Performance (Por defecto):** Envío asíncrono de redo; máximo rendimiento en primaria, riesgo mínimo de pérdida de transacciones en vuelo ($RPO > 0$).
  * **Maximum Availability:** Envío síncrono (COMMIT espera confirmación de la standby); si cae la red o la standby, degrada automáticamente a modo asíncrono sin detener la primaria ($RPO = 0$ en condiciones normales).
  * **Maximum Protection:** Envío síncrono estricto; si la standby no confirma la recepción, **la BD primaria se detiene** para garantizar pérdida cero absoluta ($RPO = 0$ garantizado).
* **4.4. Conmutación de Roles y Gestión:**
  * **Switchover:** Intercambio de roles planificado, reversible y sin pérdida de datos (mantenimientos/parches).
  * **Failover:** Conmutación de emergencia ante caída destructiva de la primaria (requiere reconstruir o hacer *Flashback* de la antigua primaria).
  * **Fast-Start Failover (FSFO):** Conmutación automática en segundos gobernada por un proceso **Observer** externo sin intervención del DBA.
  * **Data Guard Broker:** Administración y monitorización centralizada vía CLI (`DGMGRL`) o Enterprise Manager (control de *transport lag* y *apply lag*).

## 5. Arquitectura Combinada: Oracle MAA
* **MAA (*Maximum Availability Architecture*):** Marco de referencia de Oracle que combina **RAC en el CPD primario** (tolerancia a fallos de nodo y escalabilidad horizontal) + **Data Guard hacia el CPD de respaldo** (tolerancia a caídas completas del centro de datos), pudiendo desplegar también RAC en el sitio de contingencia.

## 6. Conclusión
Oracle RAC y Data Guard resuelven de forma integral pero diferenciada el problema de la continuidad de negocio. Mientras RAC opera a nivel local mediante el paradigma Activo-Activo y *Cache Fusion* para eliminar cuellos de botella y caídas de servidor, Data Guard protege la integridad del dato a distancia frente a catástrofes mediante replicación síncrona o asíncrona. Su despliegue conjunto bajo el estándar MAA permite a las Administraciones Públicas alcanzar disponibilidades del 99,999%, cumpliendo con los niveles de seguridad ALTO del ENS en materia de continuidad del servicio.

---------------

RAC	Muchos nodos → 1 BD
ASM	Gestiona almacenamiento
Interconnect	Carretera privada entre nodos
Cache Fusion	Mueve bloques entre memorias
GRD	Controla/coordina propiedad de bloques
SCAN	Puerta de entrada
FAN	Avisa de eventos/fallos
TAF	Intenta mantener la sesión
Clusterware	Policía del clúster
Voting Disk	Decide membresía
Fencing/Eviction	Expulsa nodo problemático
Split-Brain	Dos cerebros del clúster
Data Guard	Primaria ↔ Standby
Redo	Cambios que se envían
Physical	Copia física → MRP
Logical	SQL → SQL Apply
Performance	Asíncrono → rendimiento
Availability	Síncrono → puede degradar
Protection	Síncrono estricto → puede parar
Switchover	Cambio planificado
Failover	Cambio por desastre
FSFO	Failover automático
Observer	Vigila para FSFO
Broker	Administra Data Guard
DGMGRL	CLI del Broker

1. Cache Fusion mueve → GRD coordina.
2. SCAN entra → FAN avisa → TAF continúa.
3. Physical = MRP → Logical = SQL Apply.
4. Performance no espera → Availability puede degradar → Protection se para.
5. Switchover planificado → Failover desastre → FSFO automático.

-----------------------

# Tema 8.- El SGBDR Oracle. Alta disponibilidad: Data Guard y RAC.

## 1. Introducción

AP continuidad del servicio -> Requisito ineludible. Indisponibilidad -> perjuicios económicos, administrativos y legales significativos.

Servidores BD expuestos a fallos (discos, fuentes de alimentación, cortes eléctricos,...) -> **Alta Disponibilidad (High Availability - HA)**, **enmascarar los fallos** y servicio operativo con un tiempo de inactividad prácticamente nulo.

**Oracle RAC (Real Application Clusters)** protección fallos de servidor mismo CPD, y **Oracle Data Guard** protección frente a desastres CPD completo (Disaster Recovery).

## 2. Fundamentos de la Alta Disponibilidad

### 2.1. Métricas fundamentales: MTBF y MTTR

Disponibilidad:

*   **MTBF (Mean Time Between Failures):** Tiempo medio entre fallos (caidas del sistema) 

*   **MTTR (Mean Time To Repair/Recovery):** Tiempo medio de recuperación. 

**Fórmula de disponibilidad:**

```
Disponibilidad = MTBF / (MTBF + MTTR)
```

HA -> **maximizar el MTBF** (utilizando hardware redundante / calidad y mantenimiento preventivo) y **minimizar el MTTR** (failover automático).

### 2.2. Niveles de disponibilidad

Niveles: Básica 99%(2 nueves), Alta (3 nueves), Muy Alta(4 nueves), Extrema (5 nueves). 
AP exige 4 o 5 nueves. Oracle RAC y Data Guard, combinados, permiten alcanzar estos niveles.

## 3. Oracle RAC (Real Application Clusters)

### 3.1. Concepto y paradigma Activo-Activo

Modelo **Activo-Pasivo**: servidor principal atiende peticiones, servidor de respaldo permanece inactivo, arranca principal falla. Inversión infrautilizada.
Oracle RAC -> Modelo **Activo-Activo**: múltiples servidores (nodos) trabajan simultáneamente, distribuyendo carga de trabajo. Nodo falla, no interrupción del servicio.

### 3.2. Arquitectura de Oracle RAC

**Múltiples Instancias, una única Base de Datos compartida**.

*   **Almacenamiento compartido:** Nodos del clúster acceden a misma BD física en almacenamiento compartido. 
    *   **SAN (Storage Area Network):** Red de almacenamiento dedicada de alta velocidad.
    *   **Oracle ASM (Automatic Storage Management):** Gestor de volúmenes y sistema de archivos de Oracle.

*   **Múltiples Instancias:** Nodo del clúster ejecuta una Instancia de Oracle. Mismos datafiles, control files y redo logs compartidos. 

*   **Interconexión privada (Interconnect):** Red de alta velocidad y baja latencia conecta nodos. Coordinación de bloqueos distribuidos (Global Resource Directory).

### 3.3. Cache Fusion

**Cache Fusion** nodos comparten datos en memoria sin leer del disco.

*   **Funcionamiento:** Cache Fusion transfiere bloques necesarios **directamente de RAM Nodo A a RAM del Nodo B** a través de la interconexión privada. Mayor velocidad que E/S en disco.
*   **Global Resource Directory (GRD):**  Ubicación y el estado de bloques.

### 3.4. Balanceo de carga y Failover

*   **Balanceo de carga (Load Balancing):** Distribución conexiones entre nodos.

*   **Failover transparente:** Nodo falla, sesiones usuarios se transfieren:
    *   **Connection Failover:** Se establece nueva conexión en otro nodo. Aplicación debe reintentar operación fallida.
    *   **Transparent Application Failover (TAF):** Oracle reestablece sesión de forma transparente para la aplicación.

*   **Fast Application Notification (FAN):** Notificación de eventos (Oracle Notification Service - ONS) reaccionar ante eventos del clúster.

### 3.5. SCAN (Single Client Access Name)

Simplificar configuración aplicaciones -> **SCAN**: nombre DNS único, múltiples IPs.

*   **Ventaja:** Si se añaden o eliminan nodos del clúster, SCAN Listeners distribuyen automáticamente las conexiones.

### 3.6. Oracle Clusterware

**Oracle Clusterware** software de gestión del clúster del nodo (Monitorización, Gestión nodos y recursos para prevenir corrupción de datos (split-brain, cerebro dividido, donde dos nodos creen ser el principal y corrompen los datos compartidos), Fencing (aislamiento nodo problematico)).

## 4. Oracle Data Guard

### 4.1. Concepto: Disaster Recovery

**Data Guard** protección desastres al CPD completo: incendios, inundaciones, terremotos, fallos alimentación eléctrica o ataques destructivos.
**Bases de datos réplica (Standby)** en ubicaciones distantes del CPD principal. Sincronizadas mediante transmisión y aplicación Redo Log.

### 4.2. Arquitectura de Data Guard

*   **Primary Database (Base de datos primaria):** Atiende operaciones R/W. Genera Redo Log.
*   **Standby Database (Base de datos en espera):** Copias BD primaria, en CPD remotos:
    *   **Physical Standby:** Réplica exacta, **Managed Recovery Process (MRP)**, aplica cambios a nivel de bloques. **Active Data Guard**, standby solo lectura mientras aplica redo logs, alivia carga produccion (Reports, Backup).
    *   **Logical Standby:** Sincronizada mediante SQL en BD standby. Standby estructura lógica diferente para reporting o testing. Oracle SQL Apply.

### 4.3. Modos de protección de Data Guard

*   **Maximum Performance (Rendimiento máximo):** Por defecto. **Asíncrona**. La transacción se confirma en la primaria sin esperar a que la standby reciba los datos. Ofrece el máximo rendimiento, posible perdida de datos.
*   **Maximum Availability (Disponibilidad máxima):** **Síncrona**. La transacción en la primaria no se confirma hasta que standby almacena los redo datos. Garantiza cero pérdida de datos.
*   **Maximum Protection (Protección máxima):** Igual que Maximum Availability, pero si standby no disponible, la primaria se detiene para evitar pérdida de datos. 

### 4.4. Operaciones de conmutación

*   **Switchover:** Intercambio planificado de roles entre la primaria y la standby. Sin pérdida de datos y reversible. Uso mantenimiento planificado (aplicación de parches, actualizaciones de hardware).
*   **Failover:** Conmutación de emergencia (fallo catastrófico e irrecuperable). La standby asume el rol de primaria. 
*   **Fast-Start Failover (FSFO):** Failover automático gestionado por **Observer**, proceso independiente que monitoriza salud primaria y standby. Detecta fallo en primaria, ejecuta automáticamente failover sin intervención del DBA, reduce MTTR a segundos.

### 4.5. Data Guard Broker

Herramienta gestión centralizada que simplifica configuración, monitorización y administración (Automatiza operaciones de switchover y failover)

## 5. RAC y Data Guard: Arquitectura combinada MAA (Maximum Availability Architecture):

*   **RAC en el sitio primario:** Alta disponibilidad local, balanceo de carga y failover instantáneo.
*   **Data Guard hacia el sitio remoto:** Protección ante desastres CPD primario, réplica sincronizada en ubicación distante.
*   **RAC en el sitio standby (opcional):** Alta disponibilidad local en el sitio de recuperación.

Combinación -> máxima resiliencia posible

## 6. Conclusión

Alta disponibilidad en Oracle Database: 
**Oracle RAC** protege frente a fallos de servidores individuales dentro deel mismo CPD.
**Oracle Data Guard** proporciona protección frente a desastres que afecten a instalaciones completas.

Ambas tecnologías (Arquitectura MAA) en AP (continuidad del servicio y la protección de los datos no negociables) ->  garantizan sistemas resilientes, fiables y capaces de operar ininterrumpidamente frente a cualquier contingencia.
