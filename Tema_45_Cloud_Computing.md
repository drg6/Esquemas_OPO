# Tema 45.- Cloud Computing. IaaS, PaaS, SaaS. Nubes privadas, públicas e híbridas.

## 1. Introducción: Cloud Computing

### 1.1. Definición

La computación en la nube ha transformado la forma en que las organizaciones, incluidas las Administraciones Públicas, gestionan sus sistemas de información. Frente al modelo tradicional on-premise (infraestructura propia en un CPD), el cloud computing permite consumir recursos TIC como un servicio, pagando solo por lo que se utiliza, sin inversión inicial en hardware y con capacidad de escalar instantáneamente.

El **NIST (National Institute of Standards and Technology)** define Cloud Computing como un modelo que permite el acceso bajo demanda y a través de la red a un conjunto de recursos compartidos y configurables (como redes, servidores, capacidad de almacenamiento, aplicaciones y servicios) que pueden ser rápidamente asignados y liberados con una mínima gestión por parte del proveedor del servicio.

### 1.2. Características esenciales (NIST)

El NIST define cinco características esenciales que debe reunir un servicio para considerarse cloud computing:

| Característica | Descripción |
|---------------|-------------|
| **Autoservicio bajo demanda (On-demand self-service)** | El usuario aprovisiona recursos (máquinas virtuales, almacenamiento, bases de datos) automáticamente a través de un portal web, sin intervención humana del proveedor |
| **Acceso amplio a la red (Broad network access)** | Los recursos son accesibles mediante mecanismos estándar (HTTP, API REST) desde cualquier dispositivo (PC, tablet, móvil) |
| **Agrupación de recursos (Resource Pooling)** | Los recursos del proveedor se agrupan (pool compartido de recursos: servidores, almacenamiento, red) para servir a múltiples clientes mediante un modelo **multi-tenant**, asignándose dinámicamente según la demanda |
| **Elasticidad rápida (Rapid elasticity)** | Los recursos se escalan automáticamente hacia arriba o hacia abajo según la carga de trabajo, de forma inmediata y transparente. Un pico de tráfico en la Sede Electrónica durante el periodo de pago de tributos se absorbe añadiendo servidores temporalmente. |
| **Servicio medido (Pay-as-you-go)** | El uso se controla, mide y factura de forma transparente según el consumo real (horas de CPU, GB almacenados, peticiones realizadas) |

### 1.3. Contexto normativo asociado al Cloud

| Norma | Aplicación al Cloud |
|-------|---------------------|
| **RGPD (UE 2016/679)** | Protección de datos personales almacenados en la nube; ubicación de los datos y transferencias internacionales |
| **ENS (RD 311/2022)** | Incluye medida específica "Protección de servicios en la nube" aplicable desde categoría básica |
| **CCN-STIC 823** | Recomendaciones para uso, contratación y auditoría de servicios en la nube. Niveles de seguridad según categoría ENS |
| **CCN-STIC 887A / 884A / 888B** | Guías de configuración segura para AWS, Azure y Google Cloud respectivamente |
| **ENI** | Garantía de **portabilidad**: el proveedor debe entregar los datos en formato interoperable al finalizar el contrato |
| **Ley 9/2017 de Contratos del Sector Público** | Contratación cloud tipificada como contrato de servicios o suministro |

## 2. IaaS, PaaS, SaaS

### 2.1. IaaS (Infrastructure as a Service)

El proveedor proporciona **infraestructura virtualizada** (máquinas virtuales, redes, almacenamiento). El cliente gestiona el sistema operativo, middleware y aplicaciones.

| Responsabilidad | Proveedor | Cliente |
|----------------|-----------|---------|
| Hardware físico, red, CPD | ✓ | |
| Hipervisor | ✓ | |
| Sistema operativo | | ✓ |
| Middleware, runtime | | ✓ |
| Aplicaciones y datos | | ✓ |

**Ejemplos:** Amazon EC2, Azure Virtual Machines, Google Compute Engine.
**Uso en AAPP:** Migración de servidores del CPD propio a la nube, entornos de desarrollo y pruebas.

### 2.2. PaaS (Platform as a Service)

El proveedor proporciona una **plataforma de ejecución** (sistema operativo, middleware, runtime). El cliente solo gestiona su código y sus datos.

| Responsabilidad | Proveedor | Cliente |
|----------------|-----------|---------|
| Hardware, red, SO, middleware | ✓ | |
| Runtime, plataforma | ✓ | |
| Aplicaciones | | ✓ |
| Datos | | ✓ |

**Ejemplos:** Google App Engine, Azure App Service, Heroku, Azure SQL Database.
**Uso en AAPP:** Despliegue de aplicaciones web sin gestionar la infraestructura subyacente.

### 2.3. SaaS (Software as a Service)

El proveedor proporciona la **aplicación completa** lista para usar. El cliente solo configura y consume.

| Responsabilidad | Proveedor | Cliente |
|----------------|-----------|---------|
| Todo (infraestructura, plataforma, aplicación) | ✓ | |
| Configuración de usuario | | ✓ |

**Ejemplos:** Microsoft 365, Google Workspace, Salesforce, SAP SuccessFactors.
**Uso en AAPP:** Correo electrónico (Microsoft 365), ofimática colaborativa, CRM.

### 2.4. Otros modelos emergentes

*   **FaaS (Function as a Service) / Serverless:** Ejecución de funciones individuales sin gestionar servidores. El código se ejecuta en respuesta a eventos. Ejemplos: AWS Lambda, Azure Functions.
*   **CaaS (Container as a Service):** Plataforma gestionada para contenedores. Ejemplo: Azure AKS, Amazon EKS.

## 3. Nubes Privadas, Públicas e Híbridas

### 3.1. Nube pública

La infraestructura es propiedad del proveedor y está disponible para el público general a través de Internet. Los recursos se comparten entre múltiples clientes (multi-tenant).

| Ventajas | Inconvenientes |
|----------|---------------|
| Escalabilidad y elasticidad inmediatas | Limitaciones en la personalización de la infraestructura |
| Pago por uso (sin inversión inicial en hardware) | Dependencia del proveedor (vendor lock-in) |
| Actualizaciones automáticas, mantenimiento incluido | Preocupaciones de seguridad y privacidad de datos |
| Alcance global (datacenters en múltiples regiones) | Dependencia de la conexión a Internet |
| Gran variedad de servicios disponibles | Restricciones en la integración con sistemas legacy |

### 3.2. Nube privada

Infraestructura dedicada exclusivamente a una organización, controlada y operada en su beneficio exclusivo. Puede estar gestionada internamente o por un tercero, y ubicada en las instalaciones del cliente o en un datacenter externo.

| Ventajas | Inconvenientes |
|----------|---------------|
| Mayor seguridad y privacidad de los datos | Mayor coste inicial y de mantenimiento |
| Control total y personalización de la infraestructura | Necesidad de personal especializado |
| Cumplimiento normativo más sencillo | Menor flexibilidad y escalabilidad |
| Mejor integración con sistemas legacy | Dependencia de la infraestructura contratada |

**NubeSARA** es la nube privada de las Administraciones Públicas, disponible para los servicios de la SGAD. Permite a los organismos públicos desplegar servicios en una infraestructura controlada y conforme al ENS.

### 3.3. Nube comunitaria

Dos o más organizaciones con objetivos similares comparten una infraestructura cloud con un marco de seguridad y privacidad común. Reduce costes mediante la compartición de recursos entre organizaciones de un mismo sector.

**GAIA-X** es el principal ejemplo europeo: una federación de infraestructura de datos promovida por Alemania, Francia y la Comisión Europea, con más de 300 organizaciones. Busca establecer una alternativa europea a los proveedores estadounidenses, garantizando la **soberanía digital** europea. Sus principios son: apertura, transparencia, soberanía, interoperabilidad (principios FAIR: Findability, Accessibility, Interoperability, Reuse), independencia, federación y mejora continua.

### 3.4. Nube híbrida

Combinación de al menos una nube privada y una nube pública, mantenidas como entidades separadas pero unidas por tecnología que permite la **portabilidad de datos y aplicaciones** entre ellas.

**Caso de uso típico:** Mantener los datos sensibles y los sistemas críticos en la nube privada (o en NubeSARA) y utilizar la nube pública para cargas de trabajo elásticas, entornos de desarrollo/pruebas y servicios no críticos que requieran escalabilidad inmediata.

### 3.5. Cloud vs On-Premise

La decisión entre nube y on-premise depende de las necesidades del organismo:

| Criterio | Cloud | On-Premise |
|----------|-------|-----------|
| **Inversión inicial** | Baja (pago por uso) | Alta (CPD, hardware, licencias) |
| **Escalabilidad** | Elástica e inmediata | Limitada por el hardware disponible |
| **Mantenimiento** | Responsabilidad del proveedor | Equipo interno especializado |
| **Personalización** | Limitada por el proveedor | Total |
| **Seguridad** | Modelo de responsabilidad compartida | Control total del organismo |
| **Disponibilidad** | SLA del proveedor (99,9%+) | Depende de la infraestructura propia |
| **Vendor lock-in** | Riesgo si se usan servicios propietarios | No aplica |

### 3.6. Seguridad cloud según el ENS

El RD 311/2022 incluye la medida "Protección de servicios en la nube" dentro del marco operacional, aplicable desde categoría básica. La guía **CCN-STIC 823** establece recomendaciones específicas y niveles de seguridad según la categoría del sistema.

Los sistemas cloud son compatibles con el cumplimiento del ENS. Los tres grandes proveedores (AWS, Azure, Google Cloud) cuentan con certificación de **categoría Alta del ENS**, y el CCN publica guías de bastionado específicas para cada uno de ellos (CCN-STIC 887A, 884A y 888B).

### 3.7. Contratación de infraestructura cloud en las AAPP

La contratación de servicios cloud por parte de las AAPP se rige por la **Ley 9/2017 de Contratos del Sector Público**:

- Si se contratan únicamente servicios de procesamiento y almacenamiento, se clasifica como **contrato de suministro**.
- Si incluye desarrollo de software, se clasifica como **contrato de servicios**.
- La DGRCC ha promovido la contratación mediante **acuerdo marco** (AM 27/2023) para infraestructura en la nube.
- Es importante establecer contratos separados con el proveedor de servicios cloud y con el intermediario de facturación, ya que la facturación requiere intermediación (tarjeta de crédito).

## 4. Conclusión

Cloud Computing ha consolidado un modelo de consumo de recursos TIC bajo demanda que permite a las Administraciones Públicas escalar sus sistemas, reducir costes y acelerar la provisión de servicios digitales. Los modelos de servicio (IaaS, PaaS, SaaS y derivados como FaaS) distribuyen la responsabilidad entre proveedor y cliente según el nivel de abstracción. Los modelos de despliegue (pública, privada, comunitaria e híbrida) ofrecen el equilibrio adecuado entre elasticidad, control y seguridad para cada caso de uso. La estrategia recomendada para las AAPP es el modelo **híbrido** — infraestructura propia o NubeSARA para los datos más sensibles y sistemas legacy, y nube pública certificada (categoría Alta del ENS) para cargas elásticas y servicios no críticos —, siempre dentro del marco del ENS, las guías CCN-STIC 823 y la normativa de contratación pública vigente.
