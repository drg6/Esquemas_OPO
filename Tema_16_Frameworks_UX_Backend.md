# Tema 16.- Arquitectura de desarrollo en la web. Frameworks. UX. Desarrollo web en servidor, conexión a bases de datos e interconexión con sistemas y servicios.

## 1. Introducción

3 pilares fundamentales del desarrollo web: los Frameworks, que facilitan y aceleran la programación; la Experiencia de Usuario (UX), que garantiza interfaces fáciles de utilizar; y el desarrollo en servidor (backend), encargado de la lógica de negocio y el acceso a las bases de datos.

En AP, dominar estos tres pilares es vital, ya que nos permite crear sedes electrónicas y portales municipales que sean útiles para el ciudadano, totalmente seguros y capaces de conectarse con otros sistemas oficiales, como el padrón, la AEAT o la Seguridad Social.

## 2. Frameworks de Desarrollo Web

### 2.1. Concepto de Framework

Un **Framework** es un conjunto estructurado de bibliotecas, herramientas y patrones de diseño para el desarrollo de aplicaciones. Biblioteca (programador invoca), un framework **invierte el control**: y es este quien llama al código del desarrollador en los puntos adecuados (Principio de Inversión de Control o IoC).

**Ventajas:**
*   Aceleración del desarrollo al reutilizar componentes probados.
*   Aplicación de buenas prácticas y patrones de diseño (MVC, componentes, inyección de dependencias).
*   Ecosistema de plugins, extensiones y comunidad activa.
*   Seguridad contra vulnerabilidades comunes (XSS, CSRF, SQL Injection).

### 2.2. Frameworks Frontend

#### React (Meta/Facebook)

*   **Paradigma:** Biblioteca de UI basada en componentes declarativos.
*   **Lenguaje:** JavaScript/TypeScript con JSX.
*   **Virtual DOM:** Representación virtual del DOM en memoria, optimizando el rendimiento.
*   **Ecosistema:** React Router (navegación SPA), Redux/Zustand (gestión de estado), Next.js (renderizado en servidor, para Server-Side Rendering y mejora del SEO).

#### Angular (Google)

*   **Paradigma:** Framework completo (full-featured).
*   **Lenguaje:** TypeScript obligatorio.
*   **Características:** Inyección de dependencias nativa, sistema de módulos, formularios reactivos, cliente HTTP integrado, compilación AOT (Ahead-of-Time).
*   **Uso típico:** Aplicaciones empresariales de gran escala.

#### Vue.js

*   **Paradigma:** Framework progresivo: se puede adoptar incrementalmente.
*   **Lenguaje:** JavaScript/TypeScript con Single File Components (`.vue`).
*   **Características:** Reactividad integrada, sistema de componentes ligero, curva de aprendizaje suave.
*   **Ecosistema:** Vue Router, Pinia (gestión de estado), Nuxt.js (renderizado en servidor).

### 2.3. Frameworks CSS (Diseño y Maquetación)

Dos paradigmas opuestos para construir la Interfaz de Usuario (UI):

*   **Bootstrap:** Elementos prefabricados listos para usar (botones, menús). Alta velocidad de desarrollo. Ideal para intranets y backoffices municipales.
*   **Tailwind CSS:** Clases de bajo nivel para construir directamente en el HTML. Diseño 100% a medida y CSS final ultraligero. Ideal para Sedes Electrónicas y ecosistema React/Next.js.

### 2.4. Frameworks Backend

*   **Spring Boot:** Java, Framework dominante en el ecosistema Java empresarial. Autoconfiguración, servidor embebido, ecosistema completo 
*   **Django:** Python, Full-stack con ORM integrado, panel de administración automático, sistema de plantillas 
*   **ASP.NET Core:** C#, Framework de Microsoft para .NET. Alto rendimiento, multiplataforma 
*   **Express.js:** Node.js/JS, Framework minimalista para APIs REST. 
*   **Laravel:** PHP, Full-stack con Eloquent ORM, sistema de migraciones, artisan CLI

## 3. UX (User Experience) y UI (User Interface)

### 3.1. Definiciones

*   **UI (User Interface):** Elementos visuales e interactivos que componen la interfaz: botones, menús, formularios, colores, tipografías, iconos. Usuario *ve*.
*   **UX (User Experience):** La experiencia global del usuario al interactuar con el sistema: facilidad de uso, eficiencia, satisfacción.. Usuario *siente*. La UX abarca la UI.

### 3.2. Principios de diseño UX

*   **Diseño Centrado en el Usuario (UCD - User-Centered Design):** Diseño se basa en las necesidades, capacidades y limitaciones reales de usuarios finales.

*   **Arquitectura de la Información (AI):** Organización del contenido para encontrar intuitivamente lo que se busca (ej. menús claros).

*   **Heurísticas de Nielsen:** Diez principios fundamentales de usabilidad, entre los que destacan:
    1.  Visibilidad del estado del sistema (feedback) -> Ej. Cambio de color de un botón al pulsarle.
    2.  Correspondencia entre el sistema y el mundo real -> Ej. Usar un icono de carpeta para archivar o una papelera para borrar.
    3.  Control y libertad del usuario.
    4.  Consistencia y estándares.
    5.  Prevención de errores -> Ej. Diálogos de confirmación antes de eliminar un expediente
    6.  Ayuda al reconocimiento en lugar de al recuerdo.

*   **Wireframes y Prototipos:** Antes de codificar, se crean representaciones de baja fidelidad (wireframes) y alta fidelidad (mockups) que se testean con usuarios reales.

### 3.3. Diseño responsivo y Mobile First

El **Diseño Responsivo (Responsive Web Design - RWD)** adapta página web a cualquier tamaño de pantalla (CSS Media Queries, Flexbox y Grid). **Mobile First** prioriza móviles.

## 4. Desarrollo Web en Servidor (Backend)

### 4.1. Rol del servidor de aplicaciones

Ejecuta la lógica de negocio de la aplicación: validaciones, cálculos, reglas de negocio, gestión de sesiones, autenticación y autorización. Los servidores más utilizados:

*   **Apache Tomcat:** Contenedor de Servlets y JSP. 
*   **JBoss/WildFly:** Servidor de aplicaciones Java EE (EJB, JMS y JPA).
*   **Oracle WebLogic:** Servidor Oracle para Java EE/Jakarta EE.
*   **IBM WebSphere:** Entornos de misión crítica.
*   **Nginx + gunicorn/uWSGI:** Python (Django/Flask).

### 4.2. Conexión a bases de datos

*   **JDBC (Java Database Connectivity):** API estándar de Java para conectarse a cualquier base de datos relacional. Requiere un driver del fabricante (ojdbc para Oracle, postgresql-jdbc para PostgreSQL).

*   **Connection Pooling:** **Pool de conexiones** Para evitar la sobrecarga de crear y destruir conexiones a la BD en cada petición HTTP (conjunto de conexiones preestablecidas y reutilizables), gestionado datasource del servidor de aplicaciones.

*   **ORM (Object-Relational Mapping):** Frameworks como **Hibernate** (Java), **Entity Framework** (.NET) o **SQLAlchemy** (Python) capa de abstracción trabajar con objetos del lenguaje de programación en lugar de escribir SQL manualmente, automatizando las operaciones CRUD y el mapeo entre clases y tablas.

*   **JPA (Java Persistence API):** Especificación estándar de Java para ORM, implementada por Hibernate, EclipseLink y OpenJPA.

### 4.3. Patrón MVC (Model-View-Controller)

El patrón arquitectónico dominante en el desarrollo web:

*   **Modelo (Model):** Representa los datos y la lógica de negocio. Interactúa con la base de datos.
*   **Vista (View):** Presenta los datos al usuario. Genera el HTML/JSON que recibe el cliente.
*   **Controlador (Controller):** Recibe las peticiones HTTP, invoca las operaciones del Modelo y selecciona la Vista adecuada para la respuesta.

## 5. Interconexión con Sistemas y Servicios

### 5.1. Necesidad de interoperabilidad

*   **APIs REST:** Interfaces basadas en HTTP y JSON para la comunicación ligera entre servicios.
*   **Servicios Web SOAP:** Interfaces basadas en XML y WSDL.
*   **Red SARA:** Red de comunicaciones que interconecta las AP españolas, canal seguro para el intercambio de datos.
*   **Plataformas de intermediación:** @firma (firma electrónica), Cl@ve (identidad digital), SIR (Sistema de Interconexión de Registros) de las AP y PID (Plataforma de Intermediación de Datos) del usuario (REST).

### 5.2. Mensajería asíncrona

Para operaciones que no requieren respuesta inmediata (notificaciones). Se registran y mas tarde se procesan

*   **Apache Kafka:** Los eventos en tiempo real. (Ideal para delegar tareas asíncronas).
*   **RabbitMQ:** Broker de mensajes que implementa el protocolo AMQP(Advanced Message Queuing Protocol). Ideal para Big Data, IoT y auditorías

## 6. Conclusión

El desarrollo web moderno integra frameworks de frontend (React, Angular, Vue.js), principios de UX y diseño centrado en el usuario, servidores de aplicaciones con conexión a BD mediante JDBC/ORM, y mecanismos de interoperabilidad entre sistemas heterogéneos (APIs REST, SOAP, mensajería asíncrona).

En los portales web AP que deben atender simultáneamente requisitos de funcionalidad, accesibilidad (WCAG), seguridad (ENS) e interoperabilidad (ENI), el dominio de estas tecnologías constituye una competencia imprescindible para el profesional de las TI.