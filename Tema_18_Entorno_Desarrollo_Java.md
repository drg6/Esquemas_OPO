# Tema 18.- Entorno de desarrollo JAVA.

1. **Introducción:**
   * Origen (Sun Microsystems, 1995 / Oracle, 2010).
   * Justificación AAPP: Interoperabilidad sin *vendor lock-in*, seguridad/robustez y ciclo de vida de largo plazo.

2. **Fundamentos Java SE:**
   * Principio WORA: Código fuente (`.java`) -> Compilador (`javac`) -> *Bytecode* (`.class`) -> JVM.
   * Niveles: JVM (ejecución y JIT), JRE (runtime), JDK (herramientas completas).
   * Gestión de memoria: *Garbage Collector*, división generacional (*Young* vs *Old*) y recolectores concurrentes (G1, ZGC).
   * Soporte LTS: Cadencia semestral frente a versiones corporativas de largo recorrido (8, 11, 17, 21).

3. **Arquitectura Empresarial (Jakarta EE):**
   * Transición: De J2EE/Java EE a Eclipse Foundation / Jakarta EE (paquete `jakarta.*`).
   * Perfiles: *Core Profile* (microservicios), *Web Profile* (web ligera), *Full Platform* (corporativo completo).
   * Capa Web: Jakarta Servlet (base HTTP), JSP y Faces.
   * Capa Negocio: Contenedor EJB (servicios S.T.C.P.) y estándar CDI.
   * Capa Integración: JPA (ORM), JAX-RS (REST), JAX-WS (SOAP), JMS (asíncrono), JTA (transacciones distribuidas).
   * Servidores: Contenedores web (Tomcat) vs Servidores de aplicaciones (WildFly, GlassFish, WebLogic).

4. **Spring Framework y Ecosistema Cloud:**
   * Principios: Inversión de Control (IoC), Inyección de Dependencias (DI) y Programación Orientada a Aspectos (AOP).
   * Spring Boot: Convención sobre configuración, servidores embebidos (JAR autocontenido) y orientación a microservicios.
   * Frameworks Cloud-Native: Quarkus y Micronaut optimizados con compilación nativa AOT (GraalVM).

5. **Herramientas de Desarrollo, Calidad y CI/CD:**
   * Construcción: Maven (`pom.xml`) y Gradle.
   * IDEs: IntelliJ IDEA y Eclipse.
   * Pruebas y Calidad: JUnit 5 (unitarias), Mockito (aislamiento), JMeter (estrés/carga), SonarQube (análisis estático y deuda técnica).
   * CI/CD: Automatización con Git y pipelines en Jenkins.

6. **Lenguajes Políglotas sobre la JVM:**
   * Kotlin (Android y backend conciso), Scala (Big Data / Spark) y Clojure (paradigma funcional).

7. **Conclusión:**
   * Balance entre madurez corporativa, viabilidad económica de las inversiones públicas y adaptación nativa a la nube.

---------------

# Tema 18.- Entorno de desarrollo JAVA

## 1. Introducción y Fundamentos de la Plataforma
* **Paradigma y Relevancia:** Lenguaje Orientado a Objetos (Sun 1995 / Oracle 2010). No puramente OO por incluir tipos primitivos (`int`, `boolean`) y fuertemente tipado. Estándar en AAPP por su independencia de plataforma, robustez y el modelo de versiones **LTS** (soporte extendido que garantiza estabilidad, ej. Java 11, 17, 21).
* **Principio WORA (*Write Once, Run Anywhere*):** El código se compila una sola vez a un formato intermedio (*bytecode*, archivos `.class`) que se ejecuta en cualquier sistema operativo.
* **Arquitectura de Ejecución (3 Niveles):**
  * **JVM (*Java Virtual Machine*):** Motor que interpreta y compila (JIT) el *bytecode* a código máquina. Destacan implementaciones como **HotSpot** (OpenJDK) y **GraalVM** (compilación nativa AOT). Incluye el **Garbage Collector** (gestión automática de memoria dividida en generaciones *Young/Old*, evitando fugas críticas).
  * **JRE (*Java Runtime Environment*):** JVM + Bibliotecas estándar de ejecución. *(Nota: Desde Java 11 ya no se distribuye por separado)*.
  * **JDK (*Java Development Kit*):** JRE + Herramientas de desarrollo (compilador `javac`, empaquetador `jar`, depurador).

## 2. Ediciones y Perfiles de la Plataforma
* **Java SE (*Standard Edition*):** Núcleo del lenguaje y bibliotecas base (colecciones, concurrencia, E/S, red).
* **Jakarta EE (antiguo Java EE):** Especificación (gobernada por la *Eclipse Foundation*, migrada al paquete `jakarta.*`) que define contratos e interfaces para arquitecturas empresariales. Se organiza en **Perfiles**:
  * **Core Profile:** Conjunto mínimo para microservicios y contenedores (CDI Lite, JSON, REST).
  * **Web Profile:** Orientado a aplicaciones web dinámicas ligeras.
  * **Full Platform:** Especificación completa para sistemas corporativos pesados.
* **Java ME (*Micro Edition*):** Edición reducida para dispositivos limitados, IoT y tarjetas inteligentes (*JavaCard*).

## 3. Arquitectura de N-Capas (Jakarta EE)
* **3.1. Capa de Presentación (Web Tier):**
  * **Jakarta Servlet:** Pieza central que procesa peticiones y emite respuestas HTTP.
  * **JSP / Faces:** Tecnologías de UI (JSP incrusta código en HTML; Jakarta Faces implementa un framework MVC basado en componentes).
* **3.2. Capa de Negocio (Business Tier):**
  * Gestionada por el contenedor EJB, provee servicios declarativos **S.T.C.P.** (Seguridad, Transacciones, Concurrencia y Persistencia).
  * **Enterprise Beans (EJB):**
    * *Session Beans:* **Stateless** (sin estado conversacional), **Stateful** (mantienen estado entre llamadas) y **Singleton** (instancia única compartida).
    * *MDB (*Message-Driven Beans*):* Consumidores asíncronos de mensajes.
  * **CDI:** Estándar para Inyección de Dependencias y ciclo de vida de componentes.
* **3.3. Capa de Persistencia e Integración (EIS Tier):**
  * **JPA (*Jakarta Persistence*):** Estándar ORM (ej. Hibernate) mapeado mediante anotaciones (`@Entity`, `@Table`, `@Id`).
  * **JMS (*Jakarta Messaging*):** Mensajería asíncrona desacoplada mediante Colas (*Queue*) y Temas (*Topic*).
  * **JTA (*Jakarta Transactions*):** Coordinador de transacciones distribuidas (garantiza atomicidad global "todo o nada").
  * **Servicios Web:** **JAX-RS** (APIs RESTful mediante `@Path`, `@GET`) y **JAX-WS** (servicios SOAP/WSDL tradicionales en la AAPP).

## 4. Servidores de Aplicaciones y Ecosistema Spring
* **Servidores y Contenedores Tradicionales:**
  * **Contenedores Web:** **Apache Tomcat** (el más extendido, implementa el perfil web/servlets).
  * **Servidores Completos (Full Stack):** **WildFly** (JBoss) y **Eclipse GlassFish** (Open Source), junto a opciones comerciales como **Oracle WebLogic** o **IBM WebSphere**.
* **Spring Framework:** Estándar *de facto* en la industria. Basado en POJOs simples, **Inversión de Control / Inyección de Dependencias (IoC/DI)** y **Programación Orientada a Aspectos (AOP)**.
* **Spring Boot y Microservicios:** Convención sobre configuración, empaquetado en ejecutable autónomo con servidor embebido (JAR) y orientación nativa a microservicios.
* **Cloud-Native y GraalVM:** Frameworks modernos (**Quarkus**, **Micronaut**) optimizados para Kubernetes mediante arranque ultrarrápido y compilación nativa en GraalVM.

## 5. Herramientas del Entorno, Calidad y CI/CD
* **Construcción y Dependencias:** **Maven** (basado en ciclo de vida y fichero declarativo `pom.xml`) y **Gradle** (compilación incremental con Groovy/Kotlin DSL).
* **IDEs:** **IntelliJ IDEA** (líder actual) y **Eclipse** (histórico y de código abierto en la AAPP).
* **Pruebas y Calidad de Código:**
  * Pruebas Unitarias y Mocks: **JUnit 5** y **Mockito**.
  * Pruebas de Carga/Estrés: **Apache JMeter** (clave para validar concurrencia masiva en la AAPP).
  * Análisis Estático: **SonarQube** (control de deuda técnica, cobertura y vulnerabilidades OWASP).
* **CI/CD y Control de Versiones:** **Git** integrado en pipelines automatizados con **Jenkins** o GitLab CI.
* **Lenguajes Políglotas sobre la JVM:** **Kotlin** (conciso e interoperable con Java), **Scala** (Big Data con Apache Spark) y **Clojure** (programación funcional).

## 6. Conclusión
Java mantiene su dominio en los sistemas de misión crítica de las Administraciones Públicas combinando la solidez de sus fundamentos (JVM, Garbage Collector, Jakarta EE) con una gran capacidad de adaptación. Su transición desde servidores monolíticos pesados hacia microservicios y despliegues en contenedores (Spring Boot, Quarkus), respaldada por el soporte LTS y herramientas industriales de CI/CD, garantiza la viabilidad técnica y económica de las inversiones públicas a largo plazo.

----------------

# Tema 18.- Entorno de desarrollo JAVA.

## 1. Introducción

*   **Origen:** Sun Microsystems 1995 (Oracle 2010). Orientado a objetos.
*   **Relevancia:** Lenguaje dominante en AP y sector empresarial corporativo por su robustez, seguridad y ecosistema maduro.

## 2. Fundamentos de la Plataforma
*   **WORA:** *Write Once, Run Anywhere*.
*   **La Matrioska Java:**
    *   **JVM (Virtual Machine):** Ejecuta el *bytecode* (`.class`).
    *   **JRE (Runtime Environment):** JVM + Bibliotecas base (ejecución).
    *   **JDK (Development Kit):** JRE + Herramientas (`javac`, depurador).
*   **Garbage Collector (GC):** Gestión automática de memoria.
*   **Versiones LTS (Long Term Support):** 8, 11, 17 y 21 (Estabilidad para AP).

## 3. Ediciones Java
*   **Java SE (Standard Edition):** Base (`java.lang`, `.util`, `.io`, `.net`, `java.sql` - JDBC).
*   **Jakarta EE (antes Java EE - Enterprise Edition) :** Especificación (Eclipse Foundation) para apps empresariales.
*   **Java ME (Micro Edition):** Dispositivos limitados / IoT.

## 4. Arquitectura N-Capas
### 4.1. Presentación (Contenedor Web)
*   **Servlets:** Clases procesan HTTP (petición/respuesta).
*   **JSP / JSF:** Páginas dinámicas / Framework de componentes UI.
### 4.2. Negocio (Contenedor EJB)
*   **EJB:** Componentes con servicios automáticos (**S.T.C.P**: Seguridad, Transacciones, Concurrencia, Persistencia).
    *   *Stateless* (sin estado, cálculos), *Stateful* (con estado, carrito), *Singleton* (único global), *MDB* (asíncrono, colas).
*   **CDI:** Inyección de dependencias.
### 4.3. Integración/Persistencia (EIS Tier)
*   **JPA:** ORM (Mapeo objeto-relacional). Ej: Hibernate. `@Entity`, `@Table`.
*   **JTA:** Coordinador "Todo o nada" (Transacciones distribuidas).
*   **JMS:** Mensajería asíncrona.
*   **JAX-RS / JAX-WS:** APIs REST (anotación `@Path`, `@GET`) / Servicios SOAP.

## 5. Servidores y Ecosistema Moderno
*   **Servidores Clásicos:** Tomcat (Web, líder), WildFly (JEE Completo, Libre), WebLogic/WebSphere (Comerciales).
*   **Spring Framework / Spring Boot:** Estándar *de facto*. Apps autocontenidas con Tomcat embebido (Microservicios).
*   **Cloud Native:** Quarkus, Micronaut (Arranque ultra-rápido para contenedores).

## 6. Herramientas
*   **Construcción:** Maven (`pom.xml`), Gradle.
*   **IDEs:** IntelliJ, Eclipse.
*   **Control de Versiones & Testing:** Git, JUnit 5, Mockito.

## 7. Conclusión
*   Evolución constante: de monolitos Jakarta EE a microservicios Cloud Native. Viabilidad a largo plazo de inversiones TIC en la AP.
