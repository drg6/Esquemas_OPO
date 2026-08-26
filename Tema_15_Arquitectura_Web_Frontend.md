# Tema 15.- Arquitectura de desarrollo en la web. Desarrollo web front-end. Scripts de cliente.

## 1. Introducción

Siglo XX modelo **Cliente-Servidor Pesado (Fat Client)**: instalación ejecutable (.exe) en ordenador local para acceder a los servicios de la organización. Despliegues costosos (instalación PC por PC), problemas de mantenimiento (actualizar cada máquina individualmente) y dependencia del sistema operativo.

**Navegador web** cliente universal -> permite acceder a cualquier servicio a través de una URL. 
**Aplicaciones web** desplazan la lógica de negocio y el almacenamiento de datos a servidores centralizados (backend), limitando el cliente (frontend) a la presentación visual y la interacción con el usuario (AP -> sede electrónica o un portal tributario)

## 2. Protocolo HTTP/HTTPS: El Fundamento de la Web

### 2.1. Modelo de comunicación

Protocolo **HTTP (HyperText Transfer Protocol)**, que opera según un modelo **petición-respuesta (request-response)**:

1.  El **cliente** (navegador) envía una **petición HTTP (Request)** al servidor, indicando qué recurso solicita y qué operación desea realizar.
2.  El **servidor** procesa la petición y devuelve **respuesta HTTP (Response)** con el recurso solicitado (XML, JSON, ...) y un código de estado.

### 2.2. Característica sin estado (Stateless)

HTTP es un protocolo **sin estado**: cada petición es independiente; el servidor no conserva información de las peticiones anteriores. Para mantener sesiones de usuario -> **cookies**, **tokens JWT (JSON Web Tokens)** o **almacenamiento de sesión en el servidor**.

### 2.3. Métodos HTTP (Operaciones CRUD)

`GET` , `POST` ,  `PUT` (modificar todo) ,  `PATCH` (modificar una parte), `DELETE`

### 2.4. HTTPS y seguridad

**HTTPS** añade una capa de cifrado **TLS (Transport Layer Security)** sobre HTTP, garantizando la confidencialidad, integridad y autenticación de las comunicaciones. 
ENS y la normativa de protección de datos exigen HTTPS con datos personales en AP.

## 3. Arquitectura de Capas de una Aplicación Web

La arquitectura en capas (tiers):

### 3.1. Capa de Presentación (Frontend / Cliente)

Interfaz visual e interactúa con el usuario. HTML, CSS y JavaScript.

### 3.2. Capa Lógica (Backend / Servidor)

*   **Servidor Web (Apache HTTP Server, Nginx):** Atiende las peticiones HTTP, sirve archivos estáticos (imágenes, CSS, JavaScript) y actúa como proxy inverso, redirigiendo las peticiones dinámicas al servidor de aplicaciones.
*   **Servidor de Aplicaciones (Apache Tomcat, JBoss/WildFly, WebLogic, WebSphere):** Ejecuta el código (Java, C#, Python, PHP), procesa la lógica de negocio, gestiona la seguridad y se comunica con la capa de datos.

### 3.3. Capa de Datos (Persistencia)

La base de datos relacional (Oracle, PostgreSQL, SQL Server) que almacena la información persistente. El servidor de aplicaciones comunica mediante drivers de conectividad (JDBC para Java, ADO.NET para .NET) o frameworks ORM (Hibernate, Entity Framework).

## 4. Las Tres Tecnologías del Frontend Web

### 4.1. HTML5 (HyperText Markup Language)

HTML **lenguaje de marcado** que define la estructura y el contenido semántico de las páginas web. No es un lenguaje de programación: no procesa lógica ni ejecuta operaciones. 
**HTML5**, la versión actual estandarizada por el W3C (World Wide Web Consortium), mejoras fundamentales: **Etiquetas semánticas:** , **Formularios mejorados:** , **Multimedia nativa:** , **APIs nativas:**

Accesibilidad -> WCAG 2.1 / RD 1112/2018 + atributos WAI-ARIA]

### 4.2. CSS3 (Cascading Style Sheets)

Lenguaje **presentación visual** de los documentos HTML: (colores, tipografías, márgenes, ...). Separa presentación de la estructura, modificas aspecto visual sin alterar el HTML.

**Conceptos fundamentales:**

*   **Modelo de Caja (Box Model):** **Content** (el contenido), **Padding** (relleno interior), **Border** (borde) y **Margin** (margen exterior).
*   **Selectores:**  Selector de etiqueta: `h1 { ... }`, Selector de clase: `.boton-primario { ... }`, Selector de ID: `#formulario-padron { ... }`,  Selectores atributo (`:hover`, `:focus`).
*   **Flexbox y Grid:**
*   **Media Queries:** Reglas condicionales que aplican estilos diferentes según las características del dispositivo.

### 4.3. JavaScript (JS)

**Lenguaje de programación** del frontend web, proporciona **comportamiento e interactividad**: validación de formularios, manipulación dinámica del contenido, comunicación asíncrona con el servidor...

**Características fundamentales:**

*   **Lenguaje interpretado:** Sin necesidad de compilación previa.
*   **Tipado dinámico:** Las variables no requieren declaración de tipo.
*   **Orientado a eventos:** El código se ejecuta en respuesta a eventos del usuario o del sistema.

**Manipulación del DOM (Document Object Model):** Representación en memoria de la estructura del documento HTML, organizada como un árbol de nodos. 

**AJAX (Asynchronous JavaScript and XML) y Fetch API:** Permiten comunicarse con el servidor sin recargar la página completa

Seguridad Frontend -> Prevención XSS (Cross-Site Scripting) y políticas CORS

## 5. Topologías Arquitectónicas

### 5.1. Arquitectura Monolítica

Toda la lógica de la aplicación (presentación, negocio, acceso a datos) se empaqueta en un único artefacto desplegable (un archivo `.war` en Java, por ejemplo). Limitaciones de escalabilidad.

### 5.2. Arquitectura de Microservicios

Servicios independientes, pequeños y autónomos, cada uno responsable de una funcionalidad específica. Cada servicio se despliega, escala y actualiza de forma independiente y se comunica con los demás mediante APIs REST o mensajería asíncrona.

### 5.3. Aplicaciones SPA (Single Page Application)

En una SPA, el navegador carga una única página HTML inicial, toda la navegación se realiza mediante JavaScript, que solicita datos al servidor (vía API REST) y actualiza dinámicamente el contenido sin recargar la página. Frameworks como React, Angular y Vue.
SPA -> SSR (Server-Side Rendering) con frameworks como Next.js

## 6. Conclusión

Desarrollo web ha evolucionado desde el modelo cliente-servidor pesado hacia arquitecturas basadas en el navegador como cliente universal, que permiten AP ofrecer servicios digitales accesibles desde cualquier dispositivo sin instalaciones locales.

Las tres tecnologías del frontend —HTML5 para la estructura semántica, CSS3 para la presentación visual y JavaScript para el comportamiento dinámico— constituyen el estándar universal estandarizado por el W3C. 
Su dominio resulta imprescindible para el profesional de las tecnologías de la información al servicio de la AP.
