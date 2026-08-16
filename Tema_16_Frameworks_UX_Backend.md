# Tema 16.- Arquitectura de desarrollo en la web. Frameworks. UX. Desarrollo web en servidor, conexión a bases de datos e interconexión con sistemas y servicios.

## 1. Introducción

El Tema 15 estableció los fundamentos de la arquitectura web y las tecnologías del frontend (HTML5, CSS3, JavaScript). Este tema completa la visión abordando tres dimensiones complementarias: los **Frameworks de desarrollo** que aceleran la construcción de aplicaciones web, los principios de **UX (User Experience)** que garantizan la usabilidad de las interfaces, y el **desarrollo en servidor (backend)**, incluyendo la conexión a bases de datos y la interconexión con sistemas externos.

En las Administraciones Públicas, donde los portales web deben ser simultáneamente funcionales, accesibles, seguros y capaces de integrarse con múltiples sistemas internos y externos (padrón, catastro, sede electrónica, AEAT, Seguridad Social), el dominio de estos tres ejes resulta imprescindible.

## 2. Frameworks de Desarrollo Web

### 2.1. Concepto de Framework

Un **Framework** es un conjunto estructurado de bibliotecas, convenciones, herramientas y patrones de diseño que proporcionan un esqueleto predefinido para el desarrollo de aplicaciones. A diferencia de una biblioteca (que el programador invoca cuando lo necesita), un framework **invierte el control**: es el framework quien estructura la aplicación y llama al código del desarrollador en los puntos adecuados (Principio de Inversión de Control o IoC).

**Ventajas:**
*   Aceleración del desarrollo al reutilizar componentes probados.
*   Aplicación de buenas prácticas y patrones de diseño (MVC, componentes, inyección de dependencias).
*   Ecosistema de plugins, extensiones y comunidad activa.
*   Seguridad: protección integrada contra vulnerabilidades comunes (XSS, CSRF, SQL Injection).

### 2.2. Frameworks Frontend

#### React (Meta/Facebook)

*   **Paradigma:** Biblioteca de UI basada en componentes declarativos.
*   **Lenguaje:** JavaScript/TypeScript con JSX (sintaxis que combina HTML y JavaScript).
*   **Virtual DOM:** React mantiene una representación virtual del DOM en memoria; cuando el estado cambia, calcula las diferencias (diffing) y aplica solo las actualizaciones mínimas al DOM real, optimizando el rendimiento.
*   **Ecosistema:** React Router (navegación SPA), Redux/Zustand (gestión de estado), Next.js (renderizado en servidor).

#### Angular (Google)

*   **Paradigma:** Framework completo (full-featured) con opiniones fuertes sobre la arquitectura.
*   **Lenguaje:** TypeScript obligatorio.
*   **Características:** Inyección de dependencias nativa, sistema de módulos, formularios reactivos, cliente HTTP integrado, compilación AOT (Ahead-of-Time).
*   **Uso típico:** Aplicaciones empresariales de gran escala con equipos grandes.

#### Vue.js

*   **Paradigma:** Framework progresivo: se puede adoptar incrementalmente.
*   **Lenguaje:** JavaScript/TypeScript con Single File Components (`.vue`).
*   **Características:** Reactividad integrada, sistema de componentes ligero, curva de aprendizaje suave.
*   **Ecosistema:** Vue Router, Pinia (gestión de estado), Nuxt.js (renderizado en servidor).

### 2.3. Frameworks Backend

| Framework | Lenguaje | Características |
|-----------|----------|----------------|
| **Spring Boot** | Java | Framework dominante en el ecosistema Java empresarial. Autoconfiguración, servidor embebido, ecosistema completo |
| **Django** | Python | Full-stack con ORM integrado, panel de administración automático, sistema de plantillas |
| **ASP.NET Core** | C# | Framework de Microsoft para .NET. Alto rendimiento, multiplataforma |
| **Express.js** | Node.js/JS | Framework minimalista para APIs REST. Ecosistema npm extenso |
| **Laravel** | PHP | Full-stack con Eloquent ORM, sistema de migraciones, artisan CLI |

## 3. UX (User Experience) y UI (User Interface)

### 3.1. Definiciones

*   **UI (User Interface):** El conjunto de elementos visuales e interactivos que componen la interfaz: botones, menús, formularios, colores, tipografías, iconos. Es lo que el usuario *ve*.

*   **UX (User Experience):** La experiencia global del usuario al interactuar con el sistema: facilidad de uso, eficiencia, satisfacción, emociones y percepciones. Es lo que el usuario *siente*. La UX abarca la UI pero la trasciende, incluyendo la arquitectura de la información, los flujos de navegación, los tiempos de respuesta y la accesibilidad.

### 3.2. Principios de diseño UX

*   **Diseño Centrado en el Usuario (UCD - User-Centered Design):** El proceso de diseño se basa en las necesidades, capacidades y limitaciones reales de los usuarios finales, no en las preferencias del desarrollador.

*   **Arquitectura de la Información (AI):** Organización y jerarquización del contenido de forma que el usuario pueda encontrar intuitivamente lo que busca: menús claros, categorización lógica, búsqueda eficiente.

*   **Heurísticas de Nielsen:** Diez principios fundamentales de usabilidad, entre los que destacan:
    1.  Visibilidad del estado del sistema (feedback).
    2.  Correspondencia entre el sistema y el mundo real.
    3.  Control y libertad del usuario.
    4.  Consistencia y estándares.
    5.  Prevención de errores.
    6.  Ayuda al reconocimiento en lugar de al recuerdo.

*   **Wireframes y Prototipos:** Antes de codificar, se crean representaciones de baja fidelidad (wireframes) y alta fidelidad (mockups) que se testean con usuarios reales.

### 3.3. Diseño responsivo y Mobile First

El **Diseño Responsivo (Responsive Web Design - RWD)** adapta la misma página web a cualquier tamaño de pantalla mediante CSS Media Queries, Flexbox y Grid. La estrategia **Mobile First** prioriza el diseño para dispositivos móviles y lo amplía progresivamente para pantallas más grandes, garantizando que la experiencia en smartphones sea óptima.

## 4. Desarrollo Web en Servidor (Backend)

### 4.1. Rol del servidor de aplicaciones

El servidor de aplicaciones ejecuta la lógica de negocio de la aplicación: validaciones, cálculos, reglas de negocio, gestión de sesiones, autenticación y autorización. Los servidores más utilizados en el ámbito de la Administración Pública incluyen:

*   **Apache Tomcat:** Contenedor de Servlets y JSP. Ligero y ampliamente utilizado.
*   **JBoss/WildFly:** Servidor de aplicaciones Java EE completo con soporte para EJB, JMS y JPA.
*   **Oracle WebLogic:** Servidor empresarial de Oracle con soporte completo para Java EE/Jakarta EE.
*   **IBM WebSphere:** Servidor empresarial de IBM para entornos de misión crítica.
*   **Nginx + gunicorn/uWSGI:** Combinación habitual para aplicaciones Python (Django/Flask).

### 4.2. Conexión a bases de datos

La comunicación entre el servidor de aplicaciones y la base de datos se realiza mediante tecnologías de conectividad estandarizadas:

*   **JDBC (Java Database Connectivity):** API estándar de Java para conectarse a cualquier base de datos relacional. Requiere un driver específico del fabricante (ojdbc para Oracle, postgresql-jdbc para PostgreSQL).

*   **Connection Pooling:** Para evitar la sobrecarga de crear y destruir conexiones a la base de datos en cada petición HTTP, los servidores de aplicaciones mantienen un **pool de conexiones** (conjunto de conexiones preestablecidas y reutilizables), gestionado por frameworks como HikariCP o el datasource del servidor de aplicaciones.

*   **ORM (Object-Relational Mapping):** Frameworks como **Hibernate** (Java), **Entity Framework** (.NET) o **SQLAlchemy** (Python) proporcionan una capa de abstracción que permite al desarrollador trabajar con objetos del lenguaje de programación en lugar de escribir SQL manualmente, automatizando las operaciones CRUD y el mapeo entre clases y tablas.

*   **JPA (Java Persistence API):** Especificación estándar de Java para ORM, implementada por Hibernate, EclipseLink y OpenJPA.

### 4.3. Patrón MVC (Model-View-Controller)

El patrón arquitectónico dominante en el desarrollo web:

*   **Modelo (Model):** Representa los datos y la lógica de negocio. Interactúa con la base de datos.
*   **Vista (View):** Presenta los datos al usuario. Genera el HTML/JSON que recibe el cliente.
*   **Controlador (Controller):** Recibe las peticiones HTTP, invoca las operaciones del Modelo y selecciona la Vista adecuada para la respuesta.

## 5. Interconexión con Sistemas y Servicios

### 5.1. Necesidad de interoperabilidad

En las Administraciones Públicas, cada sistema puede utilizar tecnologías diferentes: el padrón puede estar en Oracle, el catastro en PostgreSQL, la Agencia Tributaria expone servicios SOAP y la pasarela de pagos utiliza APIs REST. La interconexión entre estos sistemas heterogéneos se realiza mediante:

*   **APIs REST:** Interfaces basadas en HTTP y JSON para la comunicación ligera entre servicios (detalladas en el Tema 19).
*   **Servicios Web SOAP:** Interfaces basadas en XML y WSDL para la comunicación formal entre administraciones (detalladas en el Tema 19).
*   **Red SARA:** Red de comunicaciones que interconecta las Administraciones Públicas españolas, proporcionando un canal seguro para el intercambio de datos entre ministerios, comunidades autónomas y entidades locales.
*   **Plataformas de intermediación:** @firma (firma electrónica), Cl@ve (identidad digital), SIR (Sistema de Interconexión de Registros) y PID (Plataforma de Intermediación de Datos) facilitan la integración entre administraciones.

### 5.2. Mensajería asíncrona

Para operaciones que no requieren respuesta inmediata (envío de notificaciones, procesamiento masivo de expedientes), se utilizan sistemas de **mensajería asíncrona**:

*   **Apache Kafka:** Plataforma de streaming distribuido para el procesamiento de eventos en tiempo real.
*   **RabbitMQ:** Broker de mensajes que implementa el protocolo AMQP.
*   **JMS (Java Message Service):** API estándar de Java para la mensajería.

## 6. Conclusión

El desarrollo web moderno se sustenta sobre un ecosistema complejo que integra frameworks de frontend (React, Angular, Vue.js), principios de UX y diseño centrado en el usuario, servidores de aplicaciones con conexión a bases de datos mediante JDBC/ORM, y mecanismos de interoperabilidad entre sistemas heterogéneos (APIs REST, SOAP, mensajería asíncrona).

Para las Administraciones Públicas, donde los portales web deben atender simultáneamente requisitos de funcionalidad, accesibilidad (WCAG), seguridad (ENS) e interoperabilidad (ENI), el dominio de estas tecnologías y patrones arquitectónicos constituye una competencia imprescindible para el profesional de las tecnologías de la información.
