# Tema 44.- Arquitecturas de Sistemas de Información. Arquitectura cliente-servidor. Arquitectura orientada a servicios. Buses de servicio empresarial, principios básicos, características, ventajas y funcionamiento. Arquitectura multicapa y modelo de aplicaciones web.

## 1. Introducción

Los sistemas de información de las Administraciones Públicas han evolucionado desde modelos centralizados (un Host que ejecuta el 100% de la lógica) hacia arquitecturas distribuidas que reparten la carga de procesamiento entre múltiples componentes interconectados. Esta evolución ha perseguido objetivos clave: escalabilidad, flexibilidad, reutilización de funcionalidades, tolerancia a fallos e integración de sistemas heterogéneos.

Este tema analiza las principales arquitecturas de sistemas de información — cliente-servidor, orientada a servicios (SOA), buses de servicio empresarial (ESB) y multicapa — y el modelo de aplicaciones web que sustenta los servicios digitales de la Administración.

## 2. Arquitecturas de Sistemas de Información



### 2.1. Del modelo centralizado al distribuido

El **sistema centralizado** se caracteriza por un Host que ejecuta toda la lógica del sistema, mientras el terminal del usuario se limita a presentar resultados. Sus ventajas (facilidad de gestión) no compensan sus limitaciones: poca escalabilidad, dependencia de una única máquina, desaprovechamiento del procesamiento del cliente y dificultad de integración con otros sistemas.

La **arquitectura distribuida** (procesamiento cooperativo) distribuye los componentes lógicos entre unidades de hardware interconectadas que colaboran en una tarea común. Sus características son:

- **Concurrencia global:** Ejecución paralela por demanda de múltiples usuarios.
- **Tolerancia a fallos:** El fallo de un nodo es transparente al resto de la infraestructura.
- **Sistemas abiertos y heterogéneos:** Independencia de un único fabricante.
- **Transparencia:** El usuario percibe una visión de sistema único.

Los objetivos de los sistemas distribuidos son: transparencia, fiabilidad, rendimiento, escalabilidad, flexibilidad y seguridad.

## 3. Arquitectura Cliente-Servidor

### 3.1. Concepto

Es el modelo más extendido de sistema distribuido. Se basa en la existencia de un **servidor** que proporciona un servicio a través de una interfaz o protocolo a un **cliente** que lo solicita. El cliente inicia la comunicación (petición) y el servidor la atiende (respuesta).

### 3.2. Cliente-servidor de 2 capas

El servicio se divide en dos capas:

- **Capa 1 (Cliente):** Presentación y parte de la lógica de negocio.
- **Capa 2 (Servidor):** Acceso a datos.

**Limitaciones:** El cliente es "pesado" (fat client) porque ejecuta lógica de negocio. Cualquier cambio en la lógica obliga a actualizar todos los clientes. Difícil de escalar cuando crece el número de usuarios.

### 3.3. Cliente-servidor de 3 capas (N-Tier)

El servicio se divide en tres capas independientes:

| Capa | Función | Tecnologías |
|------|---------|-------------|
| **Presentación** | Interfaz de usuario (lo que ve el ciudadano o el funcionario) | HTML/CSS/JavaScript, Angular, React, navegador web |
| **Lógica de negocio** | Reglas de negocio, validaciones, cálculos, orquestación | Java (Spring), .NET, Python, servidores de aplicaciones |
| **Acceso a datos** | Almacenamiento y recuperación de la información | Oracle, PostgreSQL, SQL Server, JDBC, JPA |

**Ventajas:** El cliente es "ligero" (thin client, navegador web). La lógica de negocio se centraliza en el servidor de aplicaciones, facilitando el mantenimiento. Cada capa se puede escalar de forma independiente.

## 4. Arquitectura Orientada a Servicios (SOA)

### 4.1. Concepto

Según OASIS (Organization for the Advancement of Structured Information Standards), SOA es un "paradigma para organizar y utilizar capacidades distribuidas y bajo el control de diferentes propietarios y dominios, que provee una manera uniforme de ofrecer, descubrir, interactuar y usar dichas capacidades de forma consistente y medible".

El objetivo de SOA es ofrecer una estrategia de integración de aplicaciones enfocada en la construcción de **servicios reutilizables**, no de aplicaciones monolíticas. Un servicio expone una funcionalidad definida; la aplicación se limita a orquestar la ejecución de un conjunto de servicios que pueden ser reutilizados en otras aplicaciones.

### 4.2. Principios de diseño SOA

| Principio | Descripción |
|-----------|-------------|
| **Contrato de servicio** | Los servicios cumplen estándares de diseño comunes (WSDL para SOAP, OpenAPI para REST) |
| **Bajo acoplamiento (Loose Coupling)** | Los servicios son independientes de la tecnología que los implementa |
| **Abstracción** | Solo se expone la información mínima requerida por el consumidor |
| **Reusabilidad** | Los servicios se diseñan como activos reutilizables de la organización |
| **Autonomía** | Cada servicio controla su propia lógica y recursos |
| **Sin estado (Stateless)** | Los servicios no mantienen estado entre invocaciones; este se delega a la aplicación |
| **Descubrimiento** | Los servicios pueden ser descubiertos y localizados (UDDI, registro de servicios) |
| **Composición** | Los servicios están preparados para ser orquestados en procesos de negocio complejos |

### 4.3. Capas de la arquitectura SOA

- **Sistemas operacionales:** Sistemas legacy, aplicaciones orientadas a objetos, sistemas de BI, que se integran exponiendo sus funcionalidades como servicios.
- **Componentes de servicio:** Aseguran los acuerdos de nivel de servicio (SLA), la alta disponibilidad y el balanceo de carga. Soportados por los servidores de aplicaciones.
- **Capa de servicios:** Los servicios se exponen para ser descubiertos e invocados por las aplicaciones consumidoras.
- **Capa de coreografía/orquestación:** Define el flujo para que múltiples servicios actúen conjuntamente como si fueran una única aplicación.

### 4.4. Colaboración entre servicios

- **Orquestación:** Un proceso central (orquestador) controla totalmente las interacciones con los servicios. Solo la entidad orquestadora conoce el flujo de control. Es un proceso **privado y ejecutable** (ej. BPEL).
- **Coreografía:** Define las colaboraciones entre servicios sin que ningún participante controle el flujo. Es un proceso **público y no ejecutable**: un protocolo de negocio que dicta las reglas de interacción.

### 4.5. SOA con Web Services

SOA define el **QUÉ** (el paradigma); los servicios web definen el **CÓMO** (la implementación):

| Estándar | Organismo | Función |
|----------|-----------|---------|
| **XML** | W3C | Lenguaje de marcado para el intercambio de datos |
| **SOAP** | W3C | Protocolo de mensajería basado en XML |
| **WSDL** | W3C | Descripción formal de la interfaz del servicio (contrato) |
| **UDDI** | OASIS | Registro y descubrimiento de servicios |
| **WS-Security** | OASIS | Extensión SOAP para integridad, confidencialidad y tokens (SAML, Kerberos) |

**Alternativa moderna: REST.** Los servicios RESTful usan HTTP directamente (GET, POST, PUT, DELETE), intercambian datos en JSON (más ligero que XML) y no requieren WSDL ni UDDI. Hoy predominan sobre SOAP en nuevos desarrollos, aunque en las AAPP coexisten ambos (la PID/SCSP y muchos servicios de @firma usan SOAP).

### 4.6. Ejemplo de SOA en Administración Pública

*   El SGBD del Padrón expone un servicio `ConsultarDatosPadronales(DNI)`.
*   El sistema de Tributos consume ese servicio para verificar el domicilio fiscal.
*   El sistema de Servicios Sociales consume el mismo servicio para verificar el empadronamiento.
*   La Plataforma de Intermediación de Datos (PID/SCSP) consume el servicio para responder a consultas de otras Administraciones.
## 5. Buses de Servicio Empresarial (ESB)

### 5.1. Concepto

Un **ESB (Enterprise Service Bus)** es una categoría de productos middleware basados en estándares de servicios que habilitan la creación de arquitecturas SOA. Es la **infraestructura central de integración** donde reside el grueso de la comunicación entre los servicios de la arquitectura.

El problema de la integración punto a punto es que cuando múltiples sistemas necesitan comunicarse entre sí, la integración directa (punto a punto) genera una arquitectura "espagueti" en la que cada sistema tiene conexiones directas con todos los demás. Con N sistemas, se necesitan N×(N-1)/2 conexiones.

### 5.2. Principios básicos

- **Mediación:** El ESB actúa como intermediario entre los servicios, desacoplando al productor del consumidor. Ningún servicio necesita conocer la ubicación ni la tecnología del otro.
- **Virtualización de servicios:** Los consumidores invocan al ESB (no directamente al servicio), lo que permite sustituir, versionar o migrar servicios sin impactar a los consumidores.
- **Estándares abiertos:** El ESB se basa en protocolos estándar (SOAP, REST, JMS, AMQP, HTTP) para garantizar la interoperabilidad entre tecnologías heterogéneas.
- **Independencia de plataforma:** Permite integrar sistemas desarrollados en diferentes lenguajes (Java, .NET, COBOL) y ejecutados en diferentes plataformas (Windows, Linux, mainframes).

### 5.3. Características

| Característica | Descripción |
|---------------|-------------|
| **Enrutamiento (Routing)** | Dirige los mensajes al servicio destino correcto según reglas configurables (basadas en contenido, cabeceras, prioridad) |
| **Transformación** | Convierte los mensajes entre diferentes formatos (XML↔JSON, SOAP↔REST, juegos de caracteres) para que servicios heterogéneos puedan comunicarse |
| **Orquestación** | Coordina la invocación secuencial o paralela de múltiples servicios para completar un proceso de negocio complejo |
| **Protocolo de mediación** | Traduce entre diferentes protocolos de transporte (HTTP, JMS, FTP, AMQP), permitiendo que un servicio SOAP se comunique con uno REST o con una cola de mensajes |
| **Monitorización** | Registra y supervisa los mensajes que circulan por el bus, generando métricas, alertas y trazas de auditoría |
| **Seguridad** | Aplica políticas de autenticación, autorización, cifrado y validación de mensajes de forma centralizada |
| **Gestión de errores** | Manejo centralizado de excepciones, reintentos, dead-letter queues y alertas |

### 5.4. Ventajas

- **Desacoplamiento:** Los servicios no se conocen entre sí; interactúan a través del bus. Un cambio en un servicio no afecta a los demás.
- **Reutilización:** Un servicio expuesto en el ESB puede ser consumido por múltiples aplicaciones sin duplicar lógica.
- **Integración de sistemas legacy:** Permite exponer funcionalidades de sistemas antiguos (COBOL, CICS) como servicios modernos sin reescribirlos.
- **Gobierno centralizado:** Las políticas de seguridad, transformación, enrutamiento y monitorización se gestionan desde un punto único.
- **Escalabilidad:** El bus puede distribuirse en clúster para soportar grandes volúmenes de mensajes.
- **Agilidad:** Añadir un nuevo sistema o servicio requiere solo configurar su conexión al bus, sin modificar los sistemas existentes.

### 5.5. Funcionamiento

El flujo típico de un mensaje a través de un ESB es:

```
[Aplicación consumidora] → [ESB: Recepción] → [Validación] → [Transformación] → [Enrutamiento] → [Servicio destino]
                                                                                                         ↓
[Aplicación consumidora] ← [ESB: Transformación respuesta] ← [Respuesta del servicio] ←────────────────────┘
```

1. La aplicación consumidora envía un mensaje al ESB (no al servicio destino directamente).
2. El ESB **valida** el mensaje (esquema XML/JSON, firma digital, token de seguridad).
3. El ESB **transforma** el mensaje al formato que espera el servicio destino.
4. El ESB **enruta** el mensaje al servicio correcto según las reglas configuradas.
5. El servicio destino procesa la petición y devuelve la respuesta al ESB.
6. El ESB transforma la respuesta al formato esperado por el consumidor y se la entrega.

### 5.6. Productos ESB

| Producto | Tipo |
|----------|------|
| **Oracle Service Bus (OSB)** | Comercial (Oracle) |
| **IBM Integration Bus** | Comercial (IBM) |
| **MuleSoft Anypoint** | Comercial (Salesforce) |
| **Apache Camel** | Open source |
| **WSO2 ESB** | Open source |

### 5.7. De SOA/ESB a microservicios

La evolución natural de SOA ha sido la **arquitectura de microservicios**, que sustituye el ESB centralizado por servicios autónomos que se comunican directamente entre sí mediante APIs REST ligeras y colas de mensajes. El ESB sigue vigente en entornos con fuerte integración de sistemas legacy, pero los nuevos desarrollos tienden hacia microservicios con API Gateway.

*   El ESB puede convertirse en un **punto único de fallo** y un cuello de botella.
*   La centralización genera dependencia del equipo que gestiona el ESB.
*   Los servicios SOA tienden a ser grandes y acoplados.

## 6. Arquitectura Multicapa y Modelo de Aplicaciones Web

### 6.1. Arquitectura multicapa

La arquitectura multicapa (N-Tier) separa las responsabilidades de una aplicación en capas lógicas independientes, cada una desplegable en una infraestructura física diferente:

| Capa | Responsabilidad | Ejemplo en AAPP |
|------|----------------|-----------------|
| **Presentación** | Interfaz de usuario | Sede electrónica (Angular + HTML/CSS) |
| **Lógica de negocio** | Reglas, validaciones, cálculos | Spring Boot, API REST de gestión de expedientes |
| **Acceso a datos** | Persistencia y consulta | Oracle DB con Spring Data JPA |
| **Integración** | Comunicación con sistemas externos | Integración con @firma, PID/SCSP, SIR vía ESB o REST |

### 6.2. Modelo de aplicaciones web moderno (SPA + API REST)

En el modelo moderno, la arquitectura web se divide en:

- **Frontend (SPA — Single Page Application):** El navegador descarga una única página HTML con JavaScript (Angular, React, Vue). Toda la navegación y actualización de la interfaz se realiza dinámicamente en el cliente mediante peticiones asíncronas (AJAX/Fetch) a la API, sin recargar la página.
- **Backend (API REST):** Servidor que expone una API RESTful (Spring Boot, .NET, Node.js) que devuelve datos en formato JSON. No genera HTML; la presentación es responsabilidad exclusiva del frontend.
- **Base de datos:** Oracle, PostgreSQL u otra base de datos accesible desde el backend.

**Ventajas del modelo SPA + API:** Separación clara de responsabilidades, experiencia de usuario fluida, la misma API puede alimentar web, app móvil y otros consumidores, facilita la reutilización de servicios.

## 7. Conclusión

Las arquitecturas de sistemas de información han evolucionado desde el modelo centralizado hacia sistemas distribuidos cliente-servidor de N capas que separan presentación, lógica de negocio y acceso a datos. La arquitectura SOA ha aportado la orientación a servicios reutilizables, bajo acoplamiento y composición mediante orquestación y coreografía. Los Buses de Servicio Empresarial (ESB) materializan esta arquitectura proporcionando la infraestructura de integración con capacidades de enrutamiento, transformación, mediación de protocolos, seguridad centralizada y monitorización. El modelo de aplicaciones web moderno, basado en arquitectura multicapa con frontend SPA y backend API REST, constituye hoy el estándar de desarrollo de los servicios digitales de las Administraciones Públicas, coexistiendo con los servicios SOAP/ESB que sustentan la integración con sistemas legacy y las plataformas comunes de la administración electrónica.
