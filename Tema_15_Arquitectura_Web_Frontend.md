# Tema 15.- Arquitectura de desarrollo en la web. Desarrollo web front-end. Scripts de cliente.

## 1. Introducción
* **Evolución del paradigma:** Transición del modelo tradicional de "Cliente Pesado" (*Fat Client*, que requería instalación local) al modelo Web, donde el **navegador** actúa como cliente universal ligero.
* **Ventaja estratégica en AAPP:** Permite un despliegue único, mantenimiento centralizado y acceso universal instantáneo a sedes electrónicas y portales del empleado sin instalación de software de terceros.

## 2. Arquitectura de desarrollo en la web
Este bloque define cómo se estructuran, comunican y despliegan los componentes a nivel global.
* **El Protocolo HTTP/HTTPS:** 
  * Ciclo de **Petición-Respuesta (*Request-Response*)** y métodos principales (`GET`, `POST`, `PUT`, `DELETE`). CRUD
  * Naturaleza **Sin Estado (*Stateless*)**, que obliga a gestionar sesiones mediante Cookies o Tokens JWT.
  * Uso obligatorio de **HTTPS (TLS)** por exigencia del Esquema Nacional de Seguridad (ENS) y el RGPD.
* **Arquitectura Clásica de 3 Capas (*Tiers*):**
  1. *Capa de Presentación:* Navegador del cliente.
  2. *Capa Lógica (Servidor):* Servidor Web (Apache, Nginx) proxy e intermediario, y Servidor de Aplicaciones (Tomcat, WebLogic) ejecutando la lógica de negocio.
  3. *Capa de Datos:* SGBDR (Oracle, PostgreSQL) con conexión vía JDBC/ORM.
* **Topologías de Despliegue:**
  * **Monolítica:** Toda la app en un único artefacto desplegable (ej. `.war`).
  * **Microservicios:** Desacoplamiento en servicios pequeños e independientes comunicados por APIs REST.

## 3. Desarrollo web front-end
Se centra en la construcción de la interfaz visual y la estructura semántica que recibe el navegador.
* **HTML5 (Estructura y Semántica):**
  * **Fundamento:** Lenguaje de marcado (no de programación) basado en jerarquía de etiquetas e hipertexto que define el esqueleto del documento (árbol DOM).
  * **Semántica y capacidades nativas (HTML5):** Aporta etiquetas con significado (`<header>`, `<nav>`, `<main>`) que sustituyen a los `<div>` genéricos. Integra multimedia (`<video>`), APIs y validación de formularios sin depender de software externo.
  * **Obligación legal (Accesibilidad):** El uso de HTML estructurado y semántico, complementado con atributos **WAI-ARIA**, es un imperativo legal (Real Decreto 1112/2018) para garantizar el acceso mediante lectores de pantalla.
* **CSS3 (Presentación Visual):**
  * Separa el diseño del contenido estructurado. Basa su renderizado en el **Modelo de Caja (*Box Model*)**.
  * Utiliza selectores (clase, ID, pseudo-clases) para aplicar estilos.
  * **Diseño Responsivo:** Uso de **Flexbox/Grid** y **Media Queries** para adaptar la interfaz a móviles (*Mobile First*).
* **Paradigmas del Frontend Moderno:**
  * Transición hacia las **SPA (*Single Page Application*)**, donde la interfaz no recarga la página, sino que se redibuja dinámicamente.

## 4. Scripts de cliente
Comprende la capa de lógica, interactividad y comunicaciones asíncronas que se ejecuta directamente en la máquina del usuario final.
* **JavaScript (JS):**
  * Es el lenguaje de programación estándar de la web. Interpretado nativamente por los motores de los navegadores (como V8 en Chrome o SpiderMonkey en Firefox).
* **Manipulación del DOM (*Document Object Model*):**
  * JS interactúa con el árbol de nodos de HTML, permitiendo modificar, ocultar o crear elementos de la interfaz en tiempo real sin intervención del servidor.
* **Comunicaciones Asíncronas (AJAX y Fetch API):**
  * Mecanismos que permiten al script de cliente lanzar peticiones HTTP (APIs REST) en segundo plano y actualizar partes específicas de la pantalla con los nuevos datos (formato JSON) sin recargar la página completa.
* **Seguridad en el Cliente:**
  * Al ejecutarse en un entorno no confiable (el navegador del usuario), los scripts son el vector principal de ataques **XSS (*Cross-Site Scripting*)**. Requiere un saneamiento estricto de entradas y validación por parte del servidor mediante políticas **CORS (*Cross-Origin Resource Sharing*)**.

## 5. Conclusión
La arquitectura web ha estandarizado el consumo de servicios públicos, erradicando los clientes pesados. El dominio integrado de su arquitectura global, el diseño front-end (HTML5 semántico, CSS3 responsivo y accesible bajo el RD 1112/2018) y la correcta implementación de scripts de cliente (JavaScript asíncrono y seguro frente a XSS) conforman la base técnica para desarrollar sedes electrónicas robustas, seguras (ENS) y universales.

--------------------

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
