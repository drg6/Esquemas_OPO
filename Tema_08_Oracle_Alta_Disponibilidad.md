# Tema 8.- El SGBDR Oracle. Alta disponibilidad: Data Guard y RAC.

## 1. Introducción

En el contexto de las Administraciones Públicas y las infraestructuras tecnológicas críticas, la continuidad del servicio no es una opción, sino un requisito ineludible. Un sistema tributario municipal, una sede electrónica o una plataforma de gestión sanitaria deben permanecer operativos de forma ininterrumpida, ya que cualquier periodo de indisponibilidad puede acarrear perjuicios económicos, administrativos y legales significativos.

Los servidores de bases de datos, por robustos que sean, están expuestos a fallos: discos que se deterioran, fuentes de alimentación que fallan, cortes eléctricos, errores de software o incluso desastres naturales que destruyen centros de datos completos. Ante esta realidad, la **Alta Disponibilidad (High Availability - HA)** no pretende conseguir un hardware indestructible, sino diseñar arquitecturas capaces de **enmascarar los fallos** y mantener el servicio operativo con un tiempo de inactividad prácticamente nulo.

Oracle Database ofrece dos tecnologías complementarias de alta disponibilidad que abordan escenarios de fallo diferentes: **Oracle RAC (Real Application Clusters)** para la protección frente a fallos de servidor dentro de un mismo centro de datos, y **Oracle Data Guard** para la protección frente a desastres que afecten a un centro de datos completo (Disaster Recovery).

## 2. Fundamentos de la Alta Disponibilidad

### 2.1. Métricas fundamentales: MTBF y MTTR

La disponibilidad de un sistema se cuantifica mediante dos métricas complementarias:

*   **MTBF (Mean Time Between Failures):** Tiempo medio entre fallos. Indica la frecuencia con la que se produce una caída del sistema. Un MTBF elevado (por ejemplo, 10.000 horas) indica un sistema robusto con fallos infrecuentes.

*   **MTTR (Mean Time To Repair/Recovery):** Tiempo medio de recuperación. Indica el tiempo que transcurre desde que se produce un fallo hasta que el sistema vuelve a estar operativo. Un MTTR bajo significa que el sistema se recupera rápidamente.

**Fórmula de disponibilidad:**

```
Disponibilidad = MTBF / (MTBF + MTTR)
```

La estrategia de alta disponibilidad se basa en dos ejes simultáneos: **maximizar el MTBF** (utilizando hardware redundante, componentes de calidad y mantenimiento preventivo) y **minimizar el MTTR** (mediante mecanismos de failover automático que reducen el tiempo de recuperación a segundos o milisegundos).

### 2.2. Niveles de disponibilidad

La industria clasifica la disponibilidad según el porcentaje de tiempo operativo anual:

| Nivel | Disponibilidad | Tiempo de inactividad anual |
|-------|---------------|-----------------------------|
| 99% (dos nueves) | Básica | ~3,65 días |
| 99,9% (tres nueves) | Alta | ~8,76 horas |
| 99,99% (cuatro nueves) | Muy alta | ~52,6 minutos |
| 99,999% (cinco nueves) | Extrema | ~5,26 minutos |

Los sistemas críticos de la Administración Pública aspiran típicamente a niveles de cuatro o cinco nueves. Oracle RAC y Data Guard, combinados, permiten alcanzar estos niveles.

## 3. Oracle RAC (Real Application Clusters)

### 3.1. Concepto y paradigma Activo-Activo

La solución clásica de alta disponibilidad dentro de un centro de datos es el modelo **Activo-Pasivo**: un servidor principal atiende todas las peticiones mientras un servidor de respaldo permanece inactivo, arrancando únicamente cuando el principal falla. Este modelo presenta un inconveniente significativo: la inversión en hardware del servidor pasivo está infrautilizada durante el funcionamiento normal.

Oracle RAC rompe con este paradigma implementando un modelo **Activo-Activo**: múltiples servidores (nodos) trabajan simultáneamente, distribuyendo la carga de trabajo entre todos ellos. Si un nodo falla, los restantes absorben automáticamente sus sesiones sin interrumpir el servicio.

### 3.2. Arquitectura de Oracle RAC

El principio fundamental de RAC es: **múltiples Instancias, una única Base de Datos compartida**.

*   **Almacenamiento compartido:** Todos los nodos del clúster acceden a la misma base de datos física, almacenada en un sistema de almacenamiento compartido. Las tecnologías de almacenamiento habituales son:
    *   **SAN (Storage Area Network):** Red de almacenamiento dedicada de alta velocidad.
    *   **Oracle ASM (Automatic Storage Management):** Gestor de volúmenes y sistema de archivos propio de Oracle, diseñado específicamente para gestionar los datafiles de la base de datos con redundancia, striping y rebalanceo automático.

*   **Múltiples Instancias:** Cada nodo del clúster ejecuta su propia Instancia de Oracle (con su SGA y sus procesos de fondo independientes). Cada Instancia tiene un identificador único (`INSTANCE_NAME`) pero todas acceden a los mismos datafiles, control files y redo logs compartidos. Un clúster RAC puede contar con 2, 4, 8 o incluso más nodos.

*   **Interconexión privada (Interconnect):** Red de alta velocidad y baja latencia (típicamente InfiniBand o Ethernet de 10/25 Gbps) que conecta exclusivamente los nodos del clúster entre sí. Se utiliza para la transferencia de bloques de datos entre las SGAs de los diferentes nodos (Cache Fusion) y para la coordinación de bloqueos distribuidos (Global Resource Directory).

### 3.3. Cache Fusion

**Cache Fusion** es la tecnología central de RAC que permite que los nodos compartan datos en memoria sin necesidad de escribir ni leer del disco.

*   **Funcionamiento:** Si el Nodo A tiene en su SGA (Database Buffer Cache) un bloque de datos que el Nodo B necesita, en lugar de obligar al Nodo A a escribir el bloque en disco para que el Nodo B lo lea posteriormente, Cache Fusion transfiere el bloque **directamente de la memoria RAM del Nodo A a la memoria RAM del Nodo B** a través de la interconexión privada. Esta transferencia en memoria es varios órdenes de magnitud más rápida que una operación de E/S en disco.

*   **Global Resource Directory (GRD):** Para coordinar qué nodo posee qué bloque en su caché, RAC mantiene un directorio global distribuido que rastrea la ubicación y el estado de cada bloque de datos en todo el clúster.

### 3.4. Balanceo de carga y Failover

*   **Balanceo de carga (Load Balancing):** Cuando múltiples usuarios se conectan al sistema, RAC distribuye las conexiones entre los nodos disponibles, equilibrando la carga de trabajo. Si un clúster RAC de 2 nodos recibe 1.000 conexiones simultáneas, cada nodo atiende aproximadamente 500.

*   **Failover transparente:** Si un nodo falla, las sesiones de los usuarios de ese nodo se transfieren automáticamente a los nodos supervivientes. Oracle ofrece dos niveles de failover:
    *   **Connection Failover:** Se establece una nueva conexión en otro nodo. La aplicación debe reintentar la operación fallida.
    *   **Transparent Application Failover (TAF):** Oracle reestablece automáticamente la sesión en otro nodo, incluyendo la restauración de cursores SELECT abiertos, de forma transparente para la aplicación.

*   **Fast Application Notification (FAN):** Sistema de notificación de eventos que permite a las aplicaciones (mediante Oracle Notification Service - ONS) reaccionar inmediatamente ante eventos del clúster (caída de un nodo, reinicio de un servicio), reduciendo los tiempos de detección y reconexión.

### 3.5. SCAN (Single Client Access Name)

Para simplificar la configuración de las aplicaciones cliente, RAC proporciona **SCAN**: un nombre DNS único con múltiples direcciones IP (típicamente tres) que los clientes utilizan como punto de entrada único al clúster.

*   **Ventaja:** Las aplicaciones solo necesitan conocer el nombre SCAN, no las IPs individuales de cada nodo. Si se añaden o eliminan nodos del clúster, la configuración de las aplicaciones no requiere modificación alguna. Los SCAN Listeners distribuyen automáticamente las conexiones al nodo más adecuado.

### 3.6. Oracle Clusterware

**Oracle Clusterware** es la capa de software de gestión del clúster que se instala en cada nodo y proporciona:

*   Monitorización del estado de los nodos mediante intercambio de heartbeats (latidos).
*   Gestión de la membresía del clúster: detección de nodos caídos y expulsión del clúster (eviction) para prevenir corrupción de datos (split-brain).
*   Gestión de recursos (VIPs, servicios de base de datos, listeners SCAN).
*   Mecanismo de fencing (aislamiento) mediante votación en discos de quorum para resolver situaciones de partición de red.

## 4. Oracle Data Guard

### 4.1. Concepto: Disaster Recovery

Mientras que RAC protege frente a fallos de servidores individuales dentro de un mismo centro de datos, **Data Guard** proporciona protección frente a desastres que afecten al centro de datos completo: incendios, inundaciones, terremotos, fallos masivos de alimentación eléctrica o ataques destructivos.

Data Guard mantiene una o varias **bases de datos réplica (Standby)** en ubicaciones geográficamente distantes del centro de datos principal. Estas bases de datos standby se mantienen sincronizadas con la base de datos principal mediante la transmisión y aplicación continua de los archivos de Redo Log generados por la primaria.

### 4.2. Arquitectura de Data Guard

La configuración de Data Guard se compone de:

*   **Primary Database (Base de datos primaria):** La base de datos operativa que atiende todas las operaciones de lectura y escritura de las aplicaciones. Genera continuamente archivos Redo Log que registran todos los cambios realizados.

*   **Standby Database (Base de datos en espera):** Una o varias copias de la base de datos primaria, mantenidas en centros de datos remotos. Reciben los redo logs de la primaria y los aplican para mantenerse sincronizadas. Existen dos tipos:
    *   **Physical Standby:** Réplica exacta a nivel de bloque físico. Se mantiene sincronizada mediante la aplicación de los redo logs con el proceso **Managed Recovery Process (MRP)**, que aplica los cambios a nivel de bloque de datos. Puede configurarse en modo **Active Data Guard**, que permite abrir la standby en modo solo lectura mientras continúa aplicando los redo logs, permitiendo descargar consultas de reporting del servidor primario.
    *   **Logical Standby:** Se mantiene sincronizada mediante la traducción de los redo logs a sentencias SQL que se ejecutan sobre la base de datos standby. Permite que la standby tenga una estructura lógica diferente (tablas adicionales, índices diferentes), lo que la hace útil para reporting o testing. Implementada mediante Oracle SQL Apply.

### 4.3. Modos de protección de Data Guard

Data Guard ofrece tres modos de protección que equilibran rendimiento y nivel de garantía de los datos:

*   **Maximum Performance (Rendimiento máximo):** Configuración por defecto. Los redo logs se transmiten de forma asíncrona a la standby. La transacción se confirma en la primaria sin esperar a que la standby reciba los datos. Ofrece el máximo rendimiento, pero en caso de desastre podría perderse una pequeña cantidad de transacciones recientes no transmitidas.

*   **Maximum Availability (Disponibilidad máxima):** Los redo logs se transmiten de forma síncrona. La transacción en la primaria no se confirma hasta que al menos una standby ha recibido y almacenado los redo datos. Si la standby no está disponible, el sistema conmuta automáticamente a modo asíncrono para no detener la operativa de la primaria. Garantiza cero pérdida de datos cuando la standby está accesible.

*   **Maximum Protection (Protección máxima):** Igual que Maximum Availability, pero si la standby no está disponible, la primaria se detiene para evitar cualquier posibilidad de pérdida de datos. Es el modo más restrictivo y solo se utiliza cuando la pérdida de datos es absolutamente intolerable.

### 4.4. Operaciones de conmutación

*   **Switchover:** Intercambio planificado de roles entre la primaria y la standby. La primaria se convierte en standby y viceversa. Se ejecuta sin pérdida de datos y es reversible. Se utiliza para mantenimiento planificado (aplicación de parches, actualizaciones de hardware).

*   **Failover:** Conmutación de emergencia cuando la primaria ha sufrido un fallo catastrófico e irrecuperable. La standby asume el rol de primaria. Puede implicar pérdida de datos si el modo de protección era Maximum Performance y existían redo logs no transmitidos. Tras un failover, la antigua primaria suele requerir una reconstrucción (reinstantiation) como nueva standby.

*   **Fast-Start Failover (FSFO):** Failover automático gestionado por el **Observer**, un proceso independiente que monitoriza continuamente la salud de la primaria y la standby. Cuando detecta que la primaria ha fallado y no se recupera dentro de un umbral de tiempo configurable, ejecuta automáticamente el failover sin intervención del DBA, reduciendo el MTTR a segundos.

### 4.5. Data Guard Broker

**Data Guard Broker** es la herramienta de gestión centralizada que simplifica la configuración, monitorización y administración de todo el entorno Data Guard:

*   Proporciona una interfaz de línea de comandos (DGMGCL) y una interfaz gráfica integrada en Oracle Enterprise Manager.
*   Gestiona la configuración completa de las bases de datos primaria y standby como una unidad lógica.
*   Automatiza las operaciones de switchover y failover.
*   Monitoriza el estado de sincronización (transport lag, apply lag) y genera alertas ante desviaciones.

## 5. RAC y Data Guard: Arquitectura combinada MAA

Oracle recomienda la combinación de RAC y Data Guard como parte de su arquitectura de referencia **MAA (Maximum Availability Architecture)**:

*   **RAC en el sitio primario:** Proporciona alta disponibilidad local, balanceo de carga y failover instantáneo ante la caída de un nodo del clúster.
*   **Data Guard hacia el sitio remoto:** Proporciona protección ante desastres que destruyan el centro de datos primario, manteniendo una réplica sincronizada en una ubicación geográficamente distante.
*   **RAC en el sitio standby (opcional):** La base de datos standby también puede desplegarse sobre un clúster RAC, proporcionando alta disponibilidad local en el sitio de recuperación.

Esta combinación proporciona la máxima resiliencia posible: protección frente a fallos de nodo (RAC), fallos de sitio (Data Guard) y, opcionalmente, alta disponibilidad también en el sitio de recuperación (RAC en standby).

## 6. Conclusión

La alta disponibilidad en Oracle Database se sustenta sobre dos tecnologías complementarias que abordan niveles de fallo diferentes. **Oracle RAC**, con su paradigma Activo-Activo, su mecanismo de Cache Fusion y su capacidad de failover transparente, protege frente a fallos de servidores individuales dentro de un mismo centro de datos, aprovechando simultáneamente la capacidad de procesamiento de todos los nodos del clúster.

**Oracle Data Guard**, con sus bases de datos standby mantenidas en ubicaciones remotas, sus tres modos de protección (Maximum Performance, Maximum Availability, Maximum Protection) y sus mecanismos de switchover y failover automático (Fast-Start Failover), proporciona la capa de protección frente a desastres que afecten a instalaciones completas.

La combinación de ambas tecnologías, siguiendo la arquitectura de referencia MAA de Oracle, constituye la solución de alta disponibilidad más completa del ecosistema Oracle y una de las más robustas de la industria de los SGBDR. Para las Administraciones Públicas, donde la continuidad del servicio y la protección de los datos ciudadanos no son negociables, el dominio de estas tecnologías resulta fundamental para garantizar sistemas de información resilientes, fiables y capaces de operar ininterrumpidamente frente a cualquier contingencia.
