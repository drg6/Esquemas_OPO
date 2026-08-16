# Tema 18.- Entorno de desarrollo JAVA.

## 1. Introducción

Java es un lenguaje de programación orientado a objetos creado por Sun Microsystems en 1995 (adquirida por Oracle Corporation en 2010). Su diseño se fundamentó en un principio revolucionario: **"Write Once, Run Anywhere" (WORA)** — escribe el código una sola vez y ejecútalo en cualquier plataforma (Windows, Linux, macOS, Solaris) sin modificaciones.

Esta portabilidad se logra gracias a la **Java Virtual Machine (JVM)**, una máquina virtual que actúa como capa intermedia entre el código compilado y el sistema operativo. El compilador Java (`javac`) no genera código máquina nativo, sino un formato intermedio denominado **bytecode** (archivos `.class`), que la JVM interpreta y ejecuta en cualquier plataforma donde esté instalada.

Java se ha consolidado como el lenguaje dominante en el desarrollo de aplicaciones empresariales de gran escala, especialmente en las Administraciones Públicas, entidades financieras y sistemas de misión crítica, gracias a su robustez, seguridad, extensa biblioteca estándar y ecosistema maduro.

## 2. Ediciones de la Plataforma Java

Java se distribuye en varias ediciones, cada una orientada a un ámbito de desarrollo específico:

### 2.1. Java SE (Standard Edition)

Es la edición base de la plataforma. Proporciona las bibliotecas fundamentales del lenguaje:

*   **Tipos de datos, colecciones y estructuras:** `java.lang`, `java.util` (List, Map, Set, Queue).
*   **Entrada/Salida (I/O):** `java.io`, `java.nio` para operaciones de lectura/escritura de archivos y streams.
*   **Networking:** `java.net` para comunicación TCP/UDP, HTTP.
*   **Concurrencia:** `java.util.concurrent` para programación multi-hilo (ExecutorService, CompletableFuture, Locks).
*   **JDBC:** `java.sql` para la conectividad con bases de datos.
*   **Seguridad:** `java.security` para criptografía, certificados y control de acceso.

### 2.2. Java EE / Jakarta EE (Enterprise Edition)

Es la extensión empresarial de Java SE, diseñada para construir aplicaciones de servidor distribuidas, multi-hilo, transaccionales y escalables. En 2017, Oracle transfirió la gobernanza de Java EE a la Eclipse Foundation, que la renombró como **Jakarta EE**.

Jakarta EE es una **especificación**: define las APIs y los contratos que deben implementar los servidores de aplicaciones, pero no proporciona la implementación. Los fabricantes (Red Hat, IBM, Oracle) ofrecen implementaciones compatibles.

### 2.3. Java ME (Micro Edition)

Edición reducida para dispositivos con recursos limitados (IoT, sistemas embebidos). Su relevancia ha disminuido con el auge de Android (que usa su propia VM: ART).

## 3. Arquitectura de N-Capas en Java EE / Jakarta EE

La plataforma Java EE fuerza una arquitectura de **N-Capas (Multi-Tier)** que separa las responsabilidades de la aplicación en capas independientes:

### 3.1. Capa de Presentación (Web Tier)

Gestiona la interacción con el usuario a través del protocolo HTTP. Los componentes se ejecutan en un **Contenedor Web** (como Apache Tomcat o el contenedor web de WildFly):

*   **Servlets:** Clases Java que procesan peticiones HTTP. Reciben un objeto `HttpServletRequest` (con los datos de la petición), ejecutan la lógica correspondiente y devuelven un `HttpServletResponse` al navegador.

    ```java
    @WebServlet("/contribuyente")
    public class ContribuyenteServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response)
                throws ServletException, IOException {
            String dni = request.getParameter("dni");
            Contribuyente c = contribuyenteService.buscar(dni);
            request.setAttribute("contribuyente", c);
            request.getRequestDispatcher("/contribuyente.jsp").forward(request, response);
        }
    }
    ```

*   **JSP (JavaServer Pages):** Páginas que combinan HTML con expresiones Java para generar contenido dinámico. En tiempo de ejecución, el contenedor las compila automáticamente a Servlets. Tecnología considerada legacy; sustituida progresivamente por Thymeleaf, JSF o frontends SPA.

*   **JSF (JavaServer Faces):** Framework de componentes UI del estándar Java EE. Orientado a componentes reutilizables con ciclo de vida gestionado. Incluye PrimeFaces y RichFaces como bibliotecas de componentes.

### 3.2. Capa de Negocio (Business Tier)

Contiene la lógica de negocio de la aplicación. Se ejecuta en un **Contenedor EJB**:

*   **EJB (Enterprise JavaBeans):** Componentes gestionados por el contenedor que proporcionan automáticamente servicios transaccionales, de seguridad, de concurrencia y de persistencia:
    *   **Session Beans (Stateless):** No mantienen estado entre invocaciones. Ideales para servicios sin estado (cálculos, validaciones).
    *   **Session Beans (Stateful):** Mantienen el estado de la conversación con un cliente específico.
    *   **Singleton Beans:** Una única instancia compartida por toda la aplicación.
    *   **Message-Driven Beans (MDB):** Procesan mensajes asíncronos de colas JMS.

*   **CDI (Contexts and Dependency Injection):** Estándar de inyección de dependencias de Jakarta EE que permite al contenedor gestionar el ciclo de vida de los objetos y sus dependencias.

### 3.3. Capa de Integración y Persistencia (EIS Tier)

Gestiona la comunicación con sistemas externos y bases de datos:

*   **JDBC (Java Database Connectivity):** API de bajo nivel para ejecutar sentencias SQL directamente contra la base de datos.

*   **JPA (Java Persistence API):** Estándar de mapeo objeto-relacional (ORM) que permite al desarrollador trabajar con objetos Java (entidades) en lugar de escribir SQL manualmente. Las implementaciones más utilizadas son **Hibernate** y **EclipseLink**.

    ```java
    @Entity
    @Table(name = "CONTRIBUYENTE")
    public class Contribuyente {
        @Id
        @Column(name = "DNI")
        private String dni;

        @Column(name = "NOMBRE")
        private String nombre;

        @Column(name = "DOMICILIO")
        private String domicilio;

        // Getters y setters
    }
    ```

*   **JMS (Java Message Service):** API para mensajería asíncrona. Permite enviar y recibir mensajes a través de colas (Queue) y temas (Topic), desacoplando los componentes de la aplicación.

*   **JTA (Java Transaction API):** Gestión de transacciones distribuidas que pueden abarcar múltiples recursos (base de datos + cola de mensajes).

*   **JAX-RS:** Especificación para construir APIs REST en Java mediante anotaciones:

    ```java
    @Path("/contribuyentes")
    @Produces(MediaType.APPLICATION_JSON)
    public class ContribuyenteResource {

        @GET
        @Path("/{dni}")
        public Contribuyente obtener(@PathParam("dni") String dni) {
            return contribuyenteService.buscar(dni);
        }
    }
    ```

*   **JAX-WS:** Especificación para construir y consumir servicios web SOAP.

## 4. Servidores de Aplicaciones Java

Los servidores de aplicaciones proporcionan los contenedores (Web y EJB) donde se despliegan las aplicaciones Java EE:

| Servidor | Fabricante | Perfil | Licencia |
|----------|-----------|--------|----------|
| **Apache Tomcat** | Apache Foundation | Contenedor Web (Servlets/JSP) | Libre |
| **WildFly (JBoss)** | Red Hat | Servidor JEE completo | Libre |
| **Oracle WebLogic** | Oracle | Servidor JEE empresarial | Comercial |
| **IBM WebSphere** | IBM | Servidor JEE empresarial | Comercial |
| **Payara** | Payara Foundation | Servidor JEE (fork de GlassFish) | Libre/Comercial |
| **GlassFish** | Eclipse Foundation | Implementación de referencia | Libre |

## 5. Herramientas del Ecosistema Java

### 5.1. Gestión de dependencias y construcción

*   **Maven:** Herramienta de gestión de proyectos basada en el fichero `pom.xml`. Gestiona dependencias (descarga automática de bibliotecas), compilación, empaquetado (`.jar`, `.war`, `.ear`) y ejecución de tests.
*   **Gradle:** Alternativa a Maven basada en scripts Groovy/Kotlin. Más flexible y con mejor rendimiento en proyectos grandes.

### 5.2. Entornos de desarrollo (IDEs)

*   **IntelliJ IDEA:** IDE líder para desarrollo Java empresarial (JetBrains).
*   **Eclipse:** IDE libre y extensible, históricamente dominante en Java EE.
*   **Visual Studio Code:** Editor ligero con extensiones para Java.

### 5.3. Control de versiones

*   **Git:** Sistema de control de versiones distribuido, estándar de la industria.
*   **Plataformas:** GitHub, GitLab, Bitbucket.

### 5.4. Testing

*   **JUnit 5:** Framework estándar para pruebas unitarias en Java.
*   **Mockito:** Framework de mocking para aislar componentes en pruebas unitarias.
*   **Selenium:** Automatización de pruebas de interfaz web.

## 6. Conclusión

La plataforma Java, con su edición empresarial Jakarta EE, constituye el ecosistema de desarrollo predominante en las grandes Administraciones Públicas y organizaciones empresariales. Su arquitectura de N-Capas, la separación de responsabilidades en contenedores Web y EJB, las APIs estandarizadas de persistencia (JPA), mensajería (JMS), servicios web (JAX-RS, JAX-WS) y la inyección de dependencias (CDI), proporcionan un marco robusto para la construcción de aplicaciones transaccionales de alta disponibilidad.

El principio WORA y la JVM garantizan la portabilidad del código entre plataformas, mientras que el extenso ecosistema de herramientas (Maven/Gradle, IDEs, frameworks de testing) y la activa comunidad de desarrolladores aseguran la viabilidad a largo plazo de las inversiones tecnológicas realizadas sobre la plataforma Java.
