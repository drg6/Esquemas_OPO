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
