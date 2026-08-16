# Tema 15.- Arquitectura de desarrollo en la web. Desarrollo web front-end. Scripts de cliente.

## 1. Introducción

Las arquitecturas de software tradicionales del siglo XX se basaban en el modelo **Cliente-Servidor Pesado (Fat Client)**: cada usuario necesitaba instalar un programa ejecutable (.exe) en su ordenador local para acceder a los servicios de la organización. Este modelo implicaba despliegues costosos (instalación PC por PC), problemas de mantenimiento (actualizar cada máquina individualmente) y dependencia del sistema operativo del usuario.

La evolución del protocolo HTTP y el auge de la World Wide Web transformaron radicalmente este paradigma. El **navegador web** (Chrome, Firefox, Edge) se convirtió en el cliente universal: una aplicación ligera ya instalada en el dispositivo del usuario que permite acceder a cualquier servicio a través de una URL, sin instalaciones adicionales. Las **aplicaciones web** desplazan la lógica de negocio y el almacenamiento de datos a servidores centralizados (backend), limitando el cliente (frontend) a la presentación visual y la interacción con el usuario.

En las Administraciones Públicas, este modelo resulta especialmente ventajoso: una sede electrónica, un portal tributario o un sistema de gestión interna se despliegan una sola vez en el servidor y son accesibles instantáneamente desde cualquier navegador, sin necesidad de instalar software en los equipos de los funcionarios ni en los dispositivos de los ciudadanos.

## 2. Protocolo HTTP/HTTPS: El Fundamento de la Web

### 2.1. Modelo de comunicación

La comunicación web se basa en el protocolo **HTTP (HyperText Transfer Protocol)**, que opera según un modelo **petición-respuesta (request-response)**:

1.  El **cliente** (navegador) envía una **petición HTTP (Request)** al servidor, indicando qué recurso solicita y qué operación desea realizar.
2.  El **servidor** procesa la petición y devuelve una **respuesta HTTP (Response)** con el recurso solicitado (una página HTML, datos JSON, una imagen) y un código de estado.

### 2.2. Característica sin estado (Stateless)

HTTP es un protocolo **sin estado**: cada petición es independiente; el servidor no conserva información de las peticiones anteriores del mismo usuario. Para mantener sesiones de usuario (por ejemplo, que un ciudadano permanezca autenticado mientras navega por la sede electrónica), se utilizan mecanismos complementarios como **cookies**, **tokens JWT (JSON Web Tokens)** o **almacenamiento de sesión en el servidor**.

### 2.3. Métodos HTTP

| Método | Operación | Ejemplo |
|--------|-----------|---------|
| `GET` | Solicitar un recurso (lectura) | Obtener la página de inicio |
| `POST` | Enviar datos para crear un recurso | Enviar un formulario de alta en el padrón |
| `PUT` | Reemplazar completamente un recurso | Actualizar todos los datos de un contribuyente |
| `PATCH` | Modificar parcialmente un recurso | Cambiar el domicilio de un contribuyente |
| `DELETE` | Eliminar un recurso | Dar de baja una solicitud |

### 2.4. HTTPS y seguridad

**HTTPS** añade una capa de cifrado **TLS (Transport Layer Security)** sobre HTTP, garantizando la confidencialidad, integridad y autenticación de las comunicaciones. El Esquema Nacional de Seguridad (ENS) y la normativa de protección de datos exigen HTTPS para cualquier servicio de la Administración Pública que maneje datos personales.

## 3. Arquitectura de Capas de una Aplicación Web

La arquitectura de una aplicación web moderna se organiza en capas (tiers) con responsabilidades claramente separadas:

### 3.1. Capa de Presentación (Frontend / Cliente)

El navegador del usuario, que renderiza la interfaz visual e interactúa con el usuario. Se construye con las tres tecnologías fundamentales de la web: HTML, CSS y JavaScript.

### 3.2. Capa Lógica (Backend / Servidor)

Comprende dos componentes diferenciados:

*   **Servidor Web (Apache HTTP Server, Nginx):** Atiende las peticiones HTTP, sirve archivos estáticos (imágenes, CSS, JavaScript) y actúa como proxy inverso, redirigiendo las peticiones dinámicas al servidor de aplicaciones.

*   **Servidor de Aplicaciones (Apache Tomcat, JBoss/WildFly, WebLogic, WebSphere):** Ejecuta el código de la aplicación (Java, C#, Python, PHP), procesa la lógica de negocio (cálculo de impuestos, validación de expedientes, gestión del padrón), gestiona la seguridad y se comunica con la capa de datos.

### 3.3. Capa de Datos (Persistencia)

La base de datos relacional (Oracle, PostgreSQL, SQL Server) que almacena la información persistente. El servidor de aplicaciones se comunica con ella mediante drivers de conectividad (JDBC para Java, ADO.NET para .NET) o frameworks ORM (Hibernate, Entity Framework).

## 4. Las Tres Tecnologías del Frontend Web

### 4.1. HTML5 (HyperText Markup Language)

HTML es el **lenguaje de marcado** que define la estructura y el contenido semántico de las páginas web. No es un lenguaje de programación: no procesa lógica ni ejecuta operaciones. Su función es describir qué elementos componen la página y cuál es su significado semántico.

**HTML5**, la versión actual estandarizada por el W3C (World Wide Web Consortium), introdujo mejoras fundamentales:

*   **Etiquetas semánticas:** `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>` — sustituyen el uso genérico de `<div>` y proporcionan significado estructural al documento, mejorando la accesibilidad y el SEO.
*   **Formularios mejorados:** Nuevos tipos de input (`email`, `date`, `number`, `url`) con validación nativa del navegador.
*   **Multimedia nativa:** `<video>` y `<audio>` eliminan la dependencia de plugins como Flash.
*   **APIs nativas:** Geolocalización, almacenamiento local (LocalStorage/SessionStorage), Canvas 2D, WebSockets.

### 4.2. CSS3 (Cascading Style Sheets)

CSS es el lenguaje que define la **presentación visual** de los documentos HTML: colores, tipografías, márgenes, disposición de los elementos, animaciones y adaptación a diferentes tamaños de pantalla. CSS separa la presentación de la estructura, permitiendo modificar completamente el aspecto visual de una web sin alterar el HTML.

**Conceptos fundamentales:**

*   **Modelo de Caja (Box Model):** Todo elemento HTML se renderiza como una caja rectangular compuesta por cuatro capas concéntricas: **Content** (el contenido), **Padding** (relleno interior), **Border** (borde) y **Margin** (margen exterior).

*   **Selectores:** Mecanismos para identificar los elementos HTML a los que se aplican los estilos:
    *   Selector de etiqueta: `h1 { color: blue; }`
    *   Selector de clase: `.boton-primario { background: #0056b3; }`
    *   Selector de ID: `#formulario-padron { width: 80%; }`
    *   Selectores combinados, de atributo, pseudo-clases (`:hover`, `:focus`, `:nth-child`).

*   **Flexbox y Grid:** Sistemas de layout modernos que sustituyen las técnicas obsoletas basadas en floats:
    *   **Flexbox:** Layout unidimensional (fila o columna) para alinear y distribuir elementos.
    *   **CSS Grid:** Layout bidimensional (filas y columnas) para diseños complejos de página completa.

*   **Media Queries:** Reglas condicionales que aplican estilos diferentes según las características del dispositivo:
    ```css
    @media (max-width: 768px) {
        .menu-lateral { display: none; }
        .contenido { width: 100%; }
    }
    ```

### 4.3. JavaScript (JS)

JavaScript es el **lenguaje de programación** del frontend web. A diferencia de HTML (estructura) y CSS (presentación), JavaScript proporciona **comportamiento e interactividad**: validación de formularios, manipulación dinámica del contenido, comunicación asíncrona con el servidor y construcción de interfaces ricas.

**Características fundamentales:**

*   **Lenguaje interpretado:** Se ejecuta directamente en el motor JavaScript del navegador (V8 en Chrome, SpiderMonkey en Firefox) sin necesidad de compilación previa.
*   **Tipado dinámico:** Las variables no requieren declaración de tipo.
*   **Orientado a eventos:** El código se ejecuta en respuesta a eventos del usuario (clic, tecleo, envío de formulario) o del sistema (carga de página, respuesta del servidor).

**Manipulación del DOM (Document Object Model):**

El DOM es la representación en memoria de la estructura del documento HTML, organizada como un árbol de nodos. JavaScript puede modificar dinámicamente cualquier elemento de la página:

```javascript
// Cambiar el texto de un elemento
document.getElementById('mensaje').textContent = 'Solicitud enviada correctamente';

// Ocultar un elemento
document.getElementById('spinner').style.display = 'none';

// Añadir un evento a un botón
document.getElementById('btn-enviar').addEventListener('click', function() {
    validarFormulario();
});
```

**AJAX (Asynchronous JavaScript and XML) y Fetch API:**

Permiten comunicarse con el servidor sin recargar la página completa, proporcionando una experiencia de usuario fluida:

```javascript
// Petición asíncrona al backend
fetch('/api/contribuyentes/12345678Z')
    .then(response => response.json())
    .then(data => {
        document.getElementById('nombre').textContent = data.nombre;
        document.getElementById('domicilio').textContent = data.domicilio;
    })
    .catch(error => console.error('Error:', error));
```

## 5. Topologías Arquitectónicas

### 5.1. Arquitectura Monolítica

Toda la lógica de la aplicación (presentación, negocio, acceso a datos) se empaqueta en un único artefacto desplegable (un archivo `.war` en Java, por ejemplo). Es simple de desarrollar y desplegar, pero presenta limitaciones de escalabilidad: para escalar, hay que replicar toda la aplicación, incluso si solo un módulo recibe mucha carga.

### 5.2. Arquitectura de Microservicios

La aplicación se descompone en servicios independientes, pequeños y autónomos, cada uno responsable de una funcionalidad específica (servicio de padrón, servicio de tributos, servicio de licencias). Cada microservicio:
*   Se despliega, escala y actualiza de forma independiente.
*   Se comunica con los demás mediante APIs REST o mensajería asíncrona.
*   Puede utilizar su propia tecnología y base de datos.

### 5.3. Aplicaciones SPA (Single Page Application)

En una SPA, el navegador carga una única página HTML inicial y, a partir de ese momento, toda la navegación se realiza mediante JavaScript, que solicita datos al servidor (vía API REST) y actualiza dinámicamente el contenido sin recargar la página. Frameworks como React, Angular y Vue.js facilitan la construcción de SPAs.

## 6. Conclusión

La arquitectura de desarrollo web ha evolucionado desde el modelo cliente-servidor pesado hacia arquitecturas basadas en el navegador como cliente universal, que permiten a las Administraciones Públicas ofrecer servicios digitales accesibles desde cualquier dispositivo sin instalaciones locales.

Las tres tecnologías del frontend —HTML5 para la estructura semántica, CSS3 para la presentación visual y JavaScript para el comportamiento dinámico— constituyen el estándar universal estandarizado por el W3C. Su dominio, junto con la comprensión de las capas arquitectónicas (presentación, lógica, datos), los protocolos de comunicación (HTTP/HTTPS) y las topologías de despliegue (monolítica, microservicios, SPA), resulta imprescindible para el profesional de las tecnologías de la información al servicio de la Administración Pública.
