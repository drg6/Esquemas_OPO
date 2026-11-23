- **1-2. El motor:** Origen (Sun/Oracle) → WORA (`.java` → `javac` → `.class` → JVM) → Muñecas rusas (JVM ⊂ JRE ⊂ JDK) → GC (Young/Old, G1/ZGC) y LTS (**8, 11, 17, 21**).
- **3. Las ediciones:** SE (base), **Jakarta EE** (Core / Web / Full), ME (IoT/JavaCard).
- **4. Las 3 capas de Jakarta EE (la parte más densa):**
  - *Web:* Servlet + JSP / Faces.
  - *Negocio:* EJB (Stateless, Stateful, Singleton + MDB) con **S.T.C.P.** + CDI.
  - *Persistencia/Integración:* Las tres J (**JPA** datos, **JMS** colas/temas, **JTA** transacciones) + **JAX-RS** (REST) / **JAX-WS** (SOAP).
- **5. Evolución de servidores:** Clásicos (Tomcat vs. WildFly/GlassFish/WebLogic/WebSphere) → **Spring** (IoC/DI, AOP) y **Spring Boot** (JAR embebido) → **Quarkus + GraalVM** (nativo/AOT).
- **6. La caja de herramientas:** Build (Maven/Gradle), IDEs (IntelliJ/Eclipse), Calidad (JUnit/Mockito, JMeter, SonarQube), CI/CD (Git/Jenkins/GitLab) y Políglotas (Kotlin/Scala).

-----------------

# Tema 18.- Entorno de desarrollo JAVA.

## 1. **Introducción:**
   * Origen (Sun Microsystems, 1995 / Oracle, 2010).
   * Lenguaje dominante en AAPP y sector empresarial corporativo por su robustez, seguridad, ecosistema maduro y su independencia de plataforma.

## 2. **Fundamentos Java:**
   * **Principio WORA (*Write Once, Run Anywhere*):** El código se compila una sola vez a un formato intermedio (*bytecode*, archivos `.class`) que se ejecuta en cualquier sistema operativo. 
   Código fuente (`.java`) -> Compilador (`javac`) -> *Bytecode* (`.class`) -> JVM.
   * **Arquitectura de Ejecución (3 Niveles):**
    - **JVM (*Java Virtual Machine*):** Motor que interpreta y compila (JIT - Just-In-Time) el *bytecode* a código máquina. Incluye el **Garbage Collector**.
    - **JRE (*Java Runtime Environment*):** JVM + Bibliotecas estándar de ejecución.
    - **JDK (*Java Development Kit*):** JRE + Herramientas de desarrollo (compilador `javac`, empaquetador `jar`, depurador).
   * **Gestión de memoria:** *Garbage Collector*, división generacional (*Young* vs *Old*) y recolectores concurrentes (G1, ZGC).
   * **Soporte LTS:** Cadencia semestral frente a versiones corporativas de largo recorrido (Lambdas en version 8, HTTP Client en version 11, Records en version 17, Virtual Threads en version 21).

## 3. Ediciones y Perfiles de la Plataforma
  * **Java SE (*Standard Edition*):** Núcleo del lenguaje y bibliotecas base (colecciones, concurrencia, E/S, red).
  * **Jakarta EE (antiguo Java EE):** Especificación (gobernada por la *Eclipse Foundation*, migrada al paquete `jakarta.*`) que define contratos e interfaces para arquitecturas empresariales. Se organiza en **Perfiles**:
    * **Core Profile:** Conjunto mínimo para microservicios y contenedores (CDI Lite [Inyección dep], JSON, REST).
    * **Web Profile:** Orientado a aplicaciones web dinámicas ligeras.
    * **Full Platform:** Especificación completa para sistemas corporativos pesados.
  * **Java ME (*Micro Edition*):** Edición reducida para dispositivos con recursos muy limitados de memoria, procesamiento y pantalla, IoT y tarjetas inteligentes (*JavaCard*).

## 4. Arquitectura de N-Capas (Jakarta EE)
* **4.1. Capa de Presentación (Web Tier):**
  * **Jakarta Servlet:** Pieza central que procesa peticiones y emite respuestas HTTP.
  * **JSP / Faces:** Tecnologías de UI (JSP incrusta código en HTML; Jakarta Faces implementa un framework MVC basado en componentes).
* **4.2. Capa de Negocio (Business Tier):**
  * Gestionada por el contenedor EJB, provee servicios declarativos **S.T.C.P.** (Seguridad, Transacciones, Concurrencia y Persistencia).
  * **Enterprise Beans (EJB):**
    * *Session Beans:* **Stateless** (sin estado conversacional), **Stateful** (mantienen estado entre llamadas) y **Singleton** (instancia única compartida).
    * *MDB (*Message-Driven Beans*):* Consumidores asíncronos de mensajes.
  * **CDI:** Estándar para Inyección de Dependencias y ciclo de vida de componentes.
* **4.3. Capa de Persistencia e Integración (EIS - Enterprise Information System Tier):**
  * **JPA (*Jakarta Persistence API*):** Estándar ORM (ej. Hibernate) mapeado mediante anotaciones (`@Entity`, `@Table`, `@Id`).
  * **JMS (*Jakarta Messaging Service*):** Mensajería asíncrona desacoplada mediante Colas (*Queue*) y Temas (*Topic*).
  * **JTA (*Jakarta Transactions API*):** Coordinador de transacciones distribuidas (garantiza atomicidad global "todo o nada").
  * **Servicios Web:** 
    - **JAX-RS** (APIs RESTful mediante `@Path`, `@GET`) 
    - **JAX-WS** (servicios SOAP/WSDL tradicionales en la AAPP).

## 5. Servidores de Aplicaciones y Ecosistema Spring
* **Servidores y Contenedores Tradicionales:**
  * **Contenedores Web:** **Apache Tomcat** (el más extendido, implementa el perfil web/servlets).
  * **Servidores Completos (Full Stack):** **WildFly** (JBoss) y **Eclipse GlassFish** (Open Source), junto a opciones comerciales como **Oracle WebLogic** o **IBM WebSphere**.
* **Spring Framework:** Estándar *de facto* en la industria. Basado en POJOs simples, **Inversión de Control / Inyección de Dependencias (IoC/DI)** y **Programación Orientada a Aspectos (AOP)**.
* **Spring Boot y Microservicios:** Convención sobre configuración, empaquetado en ejecutable autónomo con servidor embebido (JAR) y orientación nativa a microservicios.
* **Cloud-Native y GraalVM:** Frameworks modernos (**Quarkus**) optimizados para Kubernetes mediante arranque ultrarrápido y compilación nativa (Ahead-Of-Time o AOT) en GraalVM .

## 6. Herramientas del Entorno, Calidad y CI/CD
* **Construcción y Dependencias:** **Maven** (basado en ciclo de vida y fichero declarativo `pom.xml`) y **Gradle** (compilación incremental con Groovy/Kotlin DSL).
* **IDEs:** **IntelliJ IDEA** (líder actual) y **Eclipse** (histórico y de código abierto en la AAPP).
* **Pruebas y Calidad de Código:**
  * Pruebas Unitarias y Mocks: **JUnit 5** y **Mockito**.
  * Pruebas de Carga/Estrés: **Apache JMeter** (clave para validar concurrencia masiva en la AAPP).
  * Análisis Estático: **SonarQube** (control de deuda técnica, cobertura y vulnerabilidades OWASP).
* **CI/CD y Control de Versiones:** **Git** integrado en pipelines automatizados con **Jenkins** o GitLab CI.
* **Lenguajes Políglotas sobre la JVM:** **Kotlin** (Android, conciso e interoperable con Java) y **Scala** (Big Data con Apache Spark).

## 7. Conclusión
Java mantiene su dominio en los sistemas de misión crítica de las Administraciones Públicas combinando la solidez de sus fundamentos (JVM, Garbage Collector, Jakarta EE) con una gran capacidad de adaptación. Su transición desde servidores monolíticos pesados hacia microservicios y despliegues en contenedores (Spring Boot, Quarkus), respaldada por el soporte LTS y herramientas industriales de CI/CD, garantiza la viabilidad técnica y económica de las inversiones públicas a largo plazo.

-----------------

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
