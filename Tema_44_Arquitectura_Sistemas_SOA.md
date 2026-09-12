# Tema 44.- Arquitecturas (Cliente-Servidor, Multicapa), SOA, ESB y Aplicaciones Web (SPA/REST).

## 1. Introducción
* **Evolución:** De los sistemas centralizados (Mainframe monolítico) a sistemas distribuidos, buscando **Escalabilidad, Reusabilidad y Tolerancia a fallos**.

## 2. Arquitectura Cliente-Servidor y Multicapa (N-Tier)
* **2.1. Concepto:** Un *Cliente* solicita un recurso y un *Servidor* procesa la petición y responde.
* **2.2. De 2 a 3 Capas (N-Tier):**
  * *2 Capas:* Cliente pesado (Lógica + Interfaz) y BBDD. (Problema: Mantenimiento imposible al actualizar PCs).
  * *3 Capas:* **Presentación** (Web/App) + **Lógica de Negocio** (Servidor de Aplicaciones) + **Acceso a Datos** (BBDD).
  * *Ventajas:* Cliente ligero, escalabilidad independiente por capa y mantenimiento centralizado.

## 3. Arquitectura Orientada a Servicios (SOA)
* **3.1. Concepto:** Paradigma que huye de aplicaciones monolíticas gigantes y diseña el software como un conjunto de **servicios reutilizables**.
* **3.2. Principios clave:** Bajo acoplamiento, Abstracción, Autonomía, *Stateless* (Sin estado) y Descubrimiento.
* **3.3. Colaboración:**
  * *Orquestación:* Un proceso central "director de orquesta" dirige el flujo (Ej. estándar BPEL).
  * *Coreografía:* Reglas públicas de interacción sin un controlador central (como un baile sincronizado).
* **3.4. Implementación (El "Cómo"):**
  * **SOAP (Web Services clásicos):** Usa XML, contrato estricto WSDL. Típico en AAPP (Plataforma PID/SCSP).
  * **REST (Arquitectura moderna):** Usa HTTP nativo (GET, POST), formatos ligeros (JSON).

## 4. Buses de Servicio Empresarial (ESB)
* **4.1. Concepto y el "Problema Espagueti":** 
  * Si Tributos, Policía, Padrón y Urbanismo se conectan todos con todos, se crea una maraña de integraciones punto a punto (Espagueti). El ESB es el *middleware* central que elimina este caos: todos se conectan al ESB, y él reparte el tráfico.
* **4.2. Funcionalidades Core:**
  * **Enrutamiento:** Manda el mensaje al destino correcto.
  * **Transformación:** Traduce formatos (ej. recibe JSON y lo envía al legacy en XML).
  * **Mediación:** Pone de acuerdo protocolos incompatibles (HTTP a JMS).
  * **Seguridad y Monitorización:** Valida firmas/tokens y audita el tráfico.
* **4.3. Herramientas del mercado:** Oracle Service Bus, IBM Integration Bus, MuleSoft o Apache Camel (Open Source).

## 5. El Modelo Web Moderno y Microservicios
* **5.1. Evolución (Microservicios + API Gateway):**
  * SOA/ESB tiende a ser pesado (monolítico). La industria migra a **Microservicios** independientes.
  * El ESB se sustituye por un **API Gateway**, que centraliza la seguridad (valida tokens JWT), limita el tráfico y enruta peticiones hacia APIs ligeras.
* **5.2. El Modelo Web Moderno (SPA + API REST):**
  * **Frontend (SPA - Single Page Application):** Angular, React, Vue. Se ejecuta en el navegador del ciudadano. Carga dinámicamente sin refrescar la página entera.
  * **Backend (API REST):** Spring Boot, Node.js. Servidor sin estado que solo escupe datos en JSON (no pinta HTML).
  * *Ventajas:* Experiencia de usuario ultrafluida. La misma API REST sirve a la web del Ayuntamiento y a la App móvil oficial.

## 6. Conclusión
La arquitectura del software municipal debe garantizar la interoperabilidad (ENI). La convivencia de sistemas *legacy* integrados mediante un robusto **ESB** bajo principios **SOA**, junto con las nuevas plataformas de Sede Electrónica basadas en arquitecturas **SPA** consumiendo **Microservicios** vía **API REST**, conforman el ecosistema tecnológico híbrido de cualquier Administración Pública del siglo XXI.

------------------------

# Tema 44.- Arquitecturas de Sistemas de Información. Arquitectura cliente-servidor. Arquitectura orientada a servicios. Buses de servicio empresarial, principios básicos, características, ventajas y funcionamiento. Arquitectura multicapa y modelo de aplicaciones web.

## 1. Introducción

Evolución de los sistemas de información:

- **Modelo centralizado:** un único Host concentra toda la lógica.
- **Modelo distribuido:** los componentes se reparten entre diferentes sistemas interconectados.

**Objetivos:** escalabilidad, flexibilidad, reutilización, tolerancia a fallos, rendimiento, integración y seguridad.

## 2. Arquitecturas de Sistemas de Información

### 2.1. Del modelo centralizado al distribuido

**Sistema centralizado:**
- Un único Host ejecuta toda la lógica.
- Terminales principalmente de presentación.
- **Ventajas:** facilidad de gestión.
- **Inconvenientes:** poca escalabilidad, dependencia de una máquina, difícil integración.

**Sistema distribuido:**
- Componentes lógicos repartidos entre diferentes equipos interconectados.
- **Características:**
  - Concurrencia global (Ejecución paralela).
  - Tolerancia a fallos.
  - Sistemas abiertos y heterogéneos.
  - Transparencia para el usuario.

**Objetivos:** transparencia, fiabilidad, rendimiento, escalabilidad, flexibilidad y seguridad.

## 3. Arquitectura Cliente-Servidor

### 3.1. Concepto

Modelo distribuido basado en:

- **Cliente:** solicita un servicio.
- **Servidor:** proporciona el servicio.
- Comunicación mediante **comunicación (petición) → atiende (respuesta)**.

### 3.2. Cliente-servidor de 2 capas

- **Capa 1 - Cliente:** presentación + parte de la lógica de negocio.
- **Capa 2 - Servidor:** acceso a datos.

**Inconvenientes:**
- Cliente pesado (*fat client*).
- Los cambios en la lógica obligan a actualizar los clientes.
- Escalabilidad limitada.

### 3.3. Cliente-servidor de 3 capas (N-Tier)

- **Presentación:** interfaz de usuario.
  - HTML, CSS, JavaScript, Angular, React.
- **Lógica de negocio:** reglas, validaciones y procesos.
  - Java/Spring, .NET, Python.
- **Acceso a datos:** almacenamiento y recuperación.
  - Oracle, PostgreSQL, SQL Server, JDBC, JPA.

**Ventajas:**
- Cliente ligero (*thin client*).
- Lógica centralizada.
- Fácil mantenimiento.
- Cada capa puede escalar independientemente.

## 4. Arquitectura Orientada a Servicios (SOA)

### 4.1. Concepto

SOA organiza el sistema mediante **servicios reutilizables**.

- Cada servicio ofrece una funcionalidad concreta.
- Las aplicaciones **orquestan** diferentes servicios.
- Sustituye el enfoque de aplicaciones monolíticas por funcionalidades reutilizables.

### 4.2. Principios de diseño SOA

- **Contrato de servicio:** define la interfaz del servicio (WSDL para SOAP, OpenAPI para REST)
- **Bajo acoplamiento:** independencia entre servicios.
- **Abstracción:** solo se expone lo necesario.
- **Reusabilidad:** servicios reutilizables.
- **Autonomía:** cada servicio controla su lógica y recursos.
- **Sin estado (Stateless):** no mantiene estado entre invocaciones.
- **Descubrimiento:** posibilidad de localizar servicios (UDDI, registro de servicios)
- **Composición:** combinación de servicios para procesos complejos.

### 4.3. Capas de la arquitectura SOA

- **Sistemas operacionales:** sistemas existentes/legacy exponen sus funcionalidades como servicios.
- **Componentes de servicio:** disponibilidad, acuerdos de nivel de servicio (SLA) y balanceo.
- **Capa de servicios:** exposición e invocación de servicios.
- **Orquestación/Coreografía:** coordinación de múltiples servicios.

### 4.4. Colaboración entre servicios

**Orquestación:**
- Existe un **orquestador central**.
- Controla el flujo completo.
- Proceso privado y ejecutable.
- Ejemplo: BPEL (Business Process Execution Language), lenguaje estándar basado en XML que sirve para orquestar procesos.

**Coreografía:**
- No existe un controlador central.
- Define las reglas de interacción entre participantes.
- Proceso público y no ejecutable dicta las reglas de interacción.

### 4.5. SOA con Web Services

SOA define el **QUÉ** y los Web Services el **CÓMO**.

- **XML:** intercambio de datos.
- **SOAP:** protocolo de mensajería basado en XML.
- **WSDL:** contrato/interfaz del servicio.
- **UDDI:** registro y descubrimiento.
- **WS-Security:** seguridad de servicios SOAP.

**REST:**
- Utiliza HTTP: GET, POST, PUT, DELETE.
- Normalmente utiliza **JSON**.
- Más ligero que SOAP.
- Predomina en nuevos desarrollos.
- SOAP y REST pueden coexistir en las AAPP.

### 4.6. Ejemplo de SOA en Administración Pública

- Padrón → servicio `ConsultarDatosPadronales(DNI)`.
- Tributos → consume el servicio.
- Servicios Sociales → consume el mismo servicio.
- Plataforma de Intermediación de Datos PID/SCSP → puede consumirlo para otras Administraciones.

## 5. Buses de Servicio Empresarial (ESB)

### 5.1. Concepto

**ESB (Enterprise Service Bus):**

- Middleware utilizado para integrar servicios y sistemas.
- Actúa como **infraestructura central de integración**.
- Evita las conexiones **punto a punto** y la arquitectura "espagueti". Si Tributos, Policía, Padrón y Urbanismo se conectan todos con todos, se crea una maraña de integraciones punto a punto (Espagueti). 

### 5.2. Principios básicos

- **Mediación:** intermediario entre productor y consumidor. Ningún servicio necesita conocer la ubicación ni la tecnología del otro.
- **Virtualización de servicios:** los consumidores acceden al ESB, no directamente al servicio.
- **Estándares abiertos:** SOAP, REST, JMS, AMQP, HTTP.
- **Independencia de plataforma:** integra tecnologías y sistemas heterogéneos.

### 5.3. Características

- **Enrutamiento:** dirige mensajes al servicio correcto.
- **Transformación:** convierte formatos, ej. XML↔JSON, SOAP↔REST.
- **Orquestación:** coordina varios servicios.
- **Mediación de protocolos:** traduce entre HTTP, JMS, FTP, AMQP, etc.
- **Monitorización:** métricas, alertas y trazasde auditoría.
- **Seguridad:** autenticación, autorización y cifrado.
- **Gestión de errores:** reintentos, excepciones y alertas.

### 5.4. Ventajas

- **Desacoplamiento:** los servicios no dependen directamente unos de otros.
- **Reutilización:** un servicio puede ser utilizado por varias aplicaciones.
- **Integración de sistemas legacy.**
- **Gobierno centralizado:** seguridad, transformación y monitorización.
- **Escalabilidad:** posibilidad de desplegar el ESB en clúster.
- **Agilidad:** facilita incorporar nuevos sistemas.

### 5.5. Funcionamiento

Aplicación → ESB → Validación → Transformación → Enrutamiento → Servicio
Respuesta → Transformación → ESB → Aplicación

1. El consumidor envía el mensaje al ESB.
2. El ESB **valida** el mensaje (esquema XML/JSON, firma digital, token de seguridad).
3. El ESB **transforma** el formato.
4. El ESB **enruta** al servicio correspondiente.
5. El servicio procesa y devuelve la respuesta.
6. El ESB transforma la respuesta y la entrega al consumidor.

### 5.6. Productos ESB

- **Oracle Service Bus (OSB)** → Comercial (Oracle).
- **IBM Integration Bus** → Comercial (IBM).
- **MuleSoft Anypoint** → Comercial (Salesforce).
- **Apache Camel** → Open source.
- **WSO2 ESB** → Open source.

### 5.7. De SOA/ESB a microservicios

- Evolución hacia **microservicios**:
  - Servicios autónomos e independientes.
  - Comunicación mediante **APIs REST** y colas de mensajes.
  - Uso habitual de **API Gateway**, que centraliza la seguridad (valida tokens JWT), limita el tráfico (rate limiting) y enruta peticiones al microservicio correcto.

**Limitaciones del ESB:**
- Puede convertirse en **punto único de fallo**.
- Puede generar un **cuello de botella**.
- Dependencia del equipo que administra el ESB.
- Servicios SOA pueden ser grandes y acoplados.

## 6. Arquitectura Multicapa y Modelo de Aplicaciones Web

### 6.1. Arquitectura multicapa (N-Tier)

Separa las responsabilidades de la aplicación en capas independientes:

- **Presentación:** interfaz de usuario.
  - Angular, HTML/CSS.
- **Lógica de negocio:** reglas, validaciones y cálculos.
  - Spring Boot, API REST.
- **Acceso a datos:** persistencia y consultas.
  - Oracle + JPA.
- **Integración:** comunicación con sistemas externos.
  - @firma, PID/SCSP, SIR, ESB o REST.

### 6.2. Modelo de aplicaciones web moderno (SPA + API REST)

**Frontend - SPA (Single Page Application):**
- HTML con JavaScript (Angular, React, Vue).
- Se ejecuta en el navegador.
- Actualiza dinámicamente la interfaz.
- Utiliza AJAX/Fetch.

**Backend - API REST:**
- API RESTful (Spring Boot, .NET, Node.js)
- Expone servicios REST.
- Devuelve normalmente **JSON**.
- No genera HTML.

**Base de datos:**
- Oracle, PostgreSQL, etc.
- Accedida desde el backend.

**Ventajas:**
- Separación de responsabilidades.
- Experiencia de usuario fluida.
- Reutilización de la API.
- Una misma API puede ser utilizada por web, apps móviles y otros consumidores.

## 7. Conclusión

Las arquitecturas de sistemas de información han evolucionado desde el modelo centralizado hacia sistemas distribuidos cliente-servidor de N capas que separan presentación, lógica de negocio y acceso a datos. 
- **Cliente-servidor:** separa clientes y servidores.
- **N-Tier:** separa presentación, lógica y datos.
- **SOA:** organiza funcionalidades como **servicios reutilizables y poco acoplados**.
- **ESB:** proporciona integración centralizada mediante:
  - Mediación.
  - Enrutamiento.
  - Transformación.
  - Seguridad.
  - Monitorización.
- **Microservicios:** evolución hacia servicios autónomos comunicados mediante APIs.
- **SPA + API REST:** modelo moderno para aplicaciones web de las AAPP.
- En las AAPP **coexisten REST y SOAP/ESB**, especialmente por la integración con sistemas legacy y plataformas de administración electrónica.