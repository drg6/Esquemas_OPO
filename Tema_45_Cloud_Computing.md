# Tema 45.- Cloud Computing. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.

## 1. Introducción al Cloud Computing
- **Definición (NIST):** Modelo que permite el acceso bajo demanda, a través de la red, a un conjunto de recursos compartidos que se aprovisionan rápidamente con mínima intervención. Pagando solo por lo que se utiliza, sin inversión inicial en hardware y con capacidad de escalar instantáneamente.
- **5 Características Esenciales (El ADN del Cloud):** 
  1. Autoservicio bajo demanda.
  2. Acceso por red desde cualquier dispositivo.
  3. Pool de recursos compartidos (multi-tenant).
  4. **Elasticidad rápida** (crecer y decrecer automáticamente ante picos de demanda).
  5. Pago por uso según consumo real.
 * **El Cambio Financiero (CAPEX a OPEX):** El Cloud obliga a las AAPP a cambiar su modelo presupuestario, pasando de Inversiones en hardware (CAPEX) a Gasto Corriente variable mensual (OPEX).

## 2. Modelos de Servicio: IaaS, PaaS, SaaS
*(Se rigen por el **Modelo de Responsabilidad Compartida**, donde el proveedor asegura la infraestructura base (seguridad física y del hipervisor) y el Ayuntamiento protege sus datos y configuraciones según el ENS (parches del SO, MFA, cifrado de datos)).*
- **2.1. IaaS (Infraestructura como Servicio):** 
  * El proveedor da un infraestructura virtualizada (CPU, RAM, Almacenamiento, Red).
  * El TIC del Ayuntamiento gestiona desde el Sistema Operativo (parches) hacia arriba.
  * *Ejemplo:* Amazon EC2, Azure Virtual Machines, Google Compute Engine
- **2.2. PaaS (Plataforma como Servicio):** 
  * El proveedor da el SO y la plataforma lista para ejecutar código (BBDD, entornos de ejecución).
  * El TIC solo despliega su aplicación y gestiona sus datos.
  * *Ejemplo:* AWS Elastic Beanstalk, Azure SQL Database, Google App Engine .
- **2.3. SaaS (Software como Servicio):** 
  * La aplicación completa y acabada se consume directamente por red (navegador web).
  * *Ejemplo:* Microsoft 365, Google Workspace, Salesforce, SAP SuccessFactors.
  * *Caso de uso:* Migración del antiguo correo local a **Microsoft 365**, delegando en el proveedor el mantenimiento del software y heredando sus controles de seguridad físicos.
- **3.3. Contratación Pública (Ley 9/2017):**
  * Si solo es IaaS/Almacenamiento → **Contrato de Suministro**.
  * Si incluye desarrollo/SaaS → **Contrato de Servicios**.
  * Vía ágil de contratación: **Acuerdo Marco 27/2023** de la DGRCC (Dirección General de Racionalización y Centralización de la Contratación).

## 3. Modelos de Despliegue: Nubes privadas, públicas e híbridas
- **3.1. Nube Pública:**
  * Infraestructura global compartida entre distintas organizaciones y gestionada por proveedores como AWS, Azure o Google Cloud.
  * *Pro/Contra:* Máxima elasticidad y pago por uso, frente al riesgo de *vendor lock-in* (dependencia del proveedor) y la seguridad/privacidad de los datos.
  * *Cumplimiento (RD 311/2022):* Son viables para AAPP ya que los 3 grandes poseen certificación **ENS Categoría Alta**. Su uso exige aplicar la medida "Protección de servicios en la nube" mediante guías de bastionado **CCN-STIC**.
- **3.2. Nube Privada:**
  * Infraestructura de uso exclusivo para una sola organización (on-premise o alojada externamente).
  * *Pro/Contra:* Máximo control de los datos (soberanía), pero exige alta inversión inicial (hardware) y tiene menor flexibilidad/escalabilidad.
  * *Ejemplo en AAPP:* **NubeSARA**, nube privada exclusiva para la Administración. Al alojar servicios aquí, el Ayuntamiento hereda el cumplimiento físico del ENS. 
    *Ventaja táctica:* **Cumplimiento heredado del ENS**. Al alojar aquí el DRP o copias de seguridad, el Ayuntamiento hereda el Nivel Alto de certificación física de NubeSARA, ahorrando costes de auditoría local.
- **3.3. Nube Híbrida (El estándar en AAPP):** 
  * Combina Nube Pública y Privada conectadas entre sí para permitir el intercambio fluido de datos y aplicaciones entre ambos entornos.
  * *Táctica municipal:* Mantener el *Core* sensible (Padrón/Tributos) en local (Privada), y usar la Nube Pública para servicios web perimetrales o desbordamiento de tráfico web temporal (Cloud Bursting).

## 4. Conclusión
El Cloud Computing ha transformado la arquitectura tecnológica municipal. Su éxito radica en elegir el modelo de servicio adecuado (**IaaS** para control del SO, **PaaS** para agilizar el desarrollo, o **SaaS** para aplicaciones ofimáticas) y en diseñar un despliegue **Híbrido** que combine la elasticidad infinita de la nube pública con la soberanía del dato y el estricto cumplimiento del Esquema Nacional de Seguridad que exige la infraestructura privada.

-----------------------

# Tema 45.- Cloud Computing. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.

## 1. Introducción: Cloud Computing

### 1.1. Definición

AAPP transformación de modelo on-premise a cloud computing.
- **Cloud Computing:** modelo de consumo de recursos TIC bajo demanda a través de la red.
- Permite utilizar **servidores, almacenamiento, redes, aplicaciones y servicios** sin disponer necesariamente de infraestructura propia.
- **NIST (National Institute of Standards and Technology)** define Cloud Computing como: acceso bajo demanda a recursos compartidos y configurables que pueden aprovisionarse y liberarse rápidamente con mínima gestión.

### 1.2. Características esenciales (NIST)

- **Autoservicio bajo demanda (On-demand self-service):** el usuario aprovisiona recursos automáticamente.
- **Acceso amplio a la red (Broad network access):** acceso mediante mecanismos estándar (HTTP, API REST) desde distintos dispositivos.
- **Agrupación de recursos:** recursos compartidos entre múltiples clientes (**multi-tenant**).
- **Elasticidad rápida:** recursos que aumentan o disminuyen según la demanda.
- **Servicio medido:** consumo controlado y facturado según utilización (**pay-as-you-go**).

* **El Cambio Financiero (CAPEX a OPEX):** El Cloud obliga a las AAPP a cambiar su modelo presupuestario, pasando de Inversiones en hardware (CAPEX) a Gasto Corriente variable mensual (OPEX).

### 1.3. Contexto normativo asociado al Cloud

- **RGPD:** protección de datos personales, ubicación y transferencias internacionales.
- **ENS (RD 311/2022):** medidas específicas para la protección de servicios en la nube.
- **CCN-STIC:** recomendaciones para uso, contratación y auditoría de servicios cloud. Y guias de configuración de AWS, Azure y Google Cloud
- **ENI:** favorece la **portabilidad e interoperabilidad** de los datos.
- **Ley 9/2017 de Contratos del Sector Público:** regula la contratación de servicios cloud por las AAPP.

## 2. IaaS, PaaS, SaaS

### 2.1. IaaS (Infrastructure as a Service)

- El proveedor proporciona **infraestructura virtualizada**:
  - Máquinas virtuales.
  - Redes.
  - Almacenamiento.
- El cliente gestiona:
  - Sistema operativo.
  - Middleware.
  - Aplicaciones.
  - Datos.
- **Ejemplos:** Amazon EC2, Azure Virtual Machines, Google Compute Engine.
- **Uso en AAPP:** migración de servidores, desarrollo y pruebas.

### 2.2. PaaS (Platform as a Service)

- El proveedor proporciona una **plataforma completa de ejecución**:
  - Hardware.
  - Red.
  - Sistema operativo.
  - Middleware.
  - Runtime.
- El cliente gestiona principalmente:
  - Aplicaciones/código.
  - Datos.
- **Ejemplos:** Google App Engine, Azure App Service, Heroku, Azure SQL Database.
- **Uso:** desplegar aplicaciones sin administrar la infraestructura.

### 2.3. SaaS (Software as a Service)

- El proveedor proporciona la **aplicación completa lista para utilizar**.
- El cliente únicamente configura y utiliza el servicio.
- **Ejemplos:** Microsoft 365, Google Workspace, Salesforce.
- **Uso en AAPP:** correo, ofimática, colaboración y aplicaciones empresariales.

  * *Ej AAPP:* La migración del viejo correo Exchange local a **Microsoft 365**, liberando al Ayuntamiento de gestionar parches y copias, y delegando la seguridad a Entra ID.

### 2.4. Otros modelos emergentes

- **FaaS (Function as a Service) / Serverless:**
  - Ejecución de funciones sin gestionar servidores.
  - Ej.: AWS Lambda, Azure Functions.
- **CaaS (Container as a Service):**
  - Plataforma gestionada para contenedores.
  - Ej.: Azure Kubernetes Service (AKS), Amazon Elastic Kubernets Service (EKS).

## 3. Nubes Privadas, Públicas e Híbridas

### 3.1. Nube pública

- Infraestructura propiedad de un **proveedor cloud** y disponible para múltiples clientes.
- Modelo **multi-tenant**.

**Ventajas:**
- Gran escalabilidad y elasticidad.
- Pago por uso.
- Sin inversión inicial en hardware.
- Mantenimiento gestionado por el proveedor.

**Inconvenientes:**
- Dependencia del proveedor (**vendor lock-in**).
- Menor control sobre la infraestructura.
- Dependencia de Internet.
- Posibles problemas de seguridad y privacidad de datos.

### 3.2. Nube privada

- Infraestructura dedicada exclusivamente a **una organización**.
- Puede estar ubicada en las instalaciones propias o en un CPD externo.

**Ventajas:**
- Mayor control, seguridad y privacidad.
- Mayor capacidad de personalización.
- Facilita el cumplimiento normativo.
- Mejor integración con sistemas legacy.

**Inconvenientes:**
- Mayor coste de infraestructura y mantenimiento.
- Necesidad de personal especializado.
- Menor flexibilidad y escalabilidad que la nube pública.

### 3.3. Nube comunitaria

- Infraestructura compartida por **varias organizaciones con objetivos comunes**.
- Permite compartir costes y establecer un marco común de seguridad y privacidad.
- **GAIA-X:** iniciativa europea orientada a la **soberanía digital, interoperabilidad, transparencia y federación de datos**. Alternativa a proveedores EEUU.

### 3.4. Nube híbrida

- Combina **nube privada + nube pública**, conectadas para permitir la portabilidad de datos y aplicaciones.

**Uso típico en AAPP:**
- **Privada/NubeSARA:** datos sensibles, sistemas críticos y sistemas legacy.
- **Pública:** cargas elásticas, desarrollo/pruebas y servicios no críticos.

### 3.5. NubeSARA (La Nube Privada de la Administración)

- **NubeSARA:** plataforma de **nube privada de la SGAD**, exclusiva para las AAPP, con infraestructura bajo jurisdicción nacional y garantía de **soberanía del dato**.

**Usos en la Administración Local:**
- **IaaS:** despliegue de Máquinas Virtuales para nuevos proyectos.
- **BaaS:** almacenamiento de copias de seguridad cumpliendo la regla **3-2-1**.
- **DRP:** alojamiento del centro de respaldo sin necesidad de infraestructura física propia.

**Ventaja estratégica:**
- **Cumplimiento heredado del ENS:** infraestructura certificada en **Nivel Alto del ENS**, permitiendo aprovechar sus medidas de seguridad físicas y de infraestructura (control de acceso, redundancia eléctrica, protección contra incendios, etc.).
- Reduce el **coste y esfuerzo técnico** necesario para proteger y certificar infraestructuras propias.

### 3.6. Cloud vs On-Premise

- **Cloud:**
  - Menor inversión inicial.
  - Escalabilidad inmediata.
  - Mantenimiento principalmente del proveedor.
  - Pago por uso.
  - Riesgo de **vendor lock-in**.

- **On-Premise:**
  - Mayor inversión inicial.  
  - Escalabilidad limitada por el hardware disponible.  
  - Mantenimiento a cargo del organismo.  
  - Personalización máxima.
  - Control total de infraestructura.

### 3.7. Seguridad cloud según el ENS

- El **RD 311/2022 (ENS)** incluye medidas específicas de **protección de servicios en la nube**.
- **CCN-STIC 823:** recomendaciones para el uso seguro del cloud.
- AWS, Azure y Google Cloud cuentan con servicios certificados conforme a la **categoría Alta del ENS**.
- El CCN publica guías específicas de bastionado para los principales proveedores.

### 3.8. Contratación de infraestructura cloud en las AAPP

- Se rige por la **Ley 9/2017 de Contratos del Sector Público**.
- Procesamiento y almacenamiento → **contrato de suministro**.
- Si incluye desarrollo de software → **contrato de servicios**.
- La contratación puede realizarse mediante **acuerdos marco**.

## 4. Conclusión

Cloud Computing ha consolidado un modelo de consumo de recursos TIC bajo demanda que permite a las Administraciones Públicas escalar sus sistemas, reducir costes y acelerar la provisión de servicios digitales. 

- **IaaS:** infraestructura.
- **PaaS:** plataforma.
- **SaaS:** aplicación.

- **Nube privada:** máximo control.
- **Nube híbrida:** combina control y elasticidad.

- En las AAPP, el cloud debe cumplir **ENS, RGPD, CCN-STIC y normativa de contratación pública**.
- El modelo **híbrido** permite mantener sistemas sensibles bajo mayor control y utilizar la nube pública para cargas que necesitan mayor elasticidad.