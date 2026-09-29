# Tema 18.- Entorno de desarrollo JAVA.

## 1. Introducción y Fundamentos de la Plataforma
* **Paradigma:** Lenguaje Orientado a Objetos (Sun 1995 / Oracle 2010), estándar en AAPP por su escalabilidad, seguridad y el uso de versiones **LTS** (soporte a largo plazo que garantiza estabilidad).
* **Principio WORA (*Write Once, Run Anywhere*):** El código se compila una sola vez a un formato intermedio (*bytecode*, archivos `.class`) que se ejecuta en cualquier sistema operativo.
* **Arquitectura de Ejecución (3 Niveles):**
  * **JVM (*Java Virtual Machine*):** Motor que interpreta y ejecuta el *bytecode*. Incluye el **Garbage Collector** (liberación automática de memoria, evitando fugas críticas).
  * **JRE (*Java Runtime Environment*):** JVM + Bibliotecas estándar de ejecución.
  * **JDK (*Java Development Kit*):** JRE + Herramientas de desarrollo (compilador `javac`, depurador).

## 2. Ediciones de la Plataforma
* **Java SE (*Standard Edition*):** Bibliotecas core (colecciones, concurrencia, E/S, red).
* **Jakarta EE (antiguo Java EE):** Especificación (actualmente en la Eclipse Foundation) que define APIs estandarizadas para sistemas corporativos distribuidos y transaccionales.
* **Java ME (*Micro Edition*):** Edición reducida para dispositivos limitados e IoT.

## 3. Arquitectura de N-Capas (Jakarta EE)
* **3.1. Capa de Presentación (Web Tier):**
  * **Servlets:** Clases Java que procesan peticiones y devuelven respuestas HTTP.
  * **JSP / JSF:** Tecnologías de UI (JSP incrusta código en HTML; JSF usa componentes visuales reutilizables).
* **3.2. Capa de Negocio (Business Tier):**
  * Gestionada por el contenedor EJB, provee servicios **S.T.C.P.** (Seguridad, Transacciones, Concurrencia y Persistencia).
  * **EJB (*Enterprise JavaBeans*):**
    * *Session Beans:* **Stateless** (operaciones sin estado), **Stateful** (mantienen estado conversacional) y **Singleton** (instancia única).
    * *MDB (*Message-Driven Beans*):* Procesamiento asíncrono en segundo plano.
  * **CDI:** Estándar para la Inyección de Dependencias.
* **3.3. Capa de Integración y Persistencia (EIS Tier):**
  * **JPA (*Java Persistence API*):** Estándar ORM (ej. Hibernate) mapeado directamente mediante anotaciones (`@Entity`, `@Table`, `@Id`).
  * **JMS (*Java Message Service*):** Mensajería asíncrona mediante Colas (*Queue*) y Temas (*Topic*).
  * **JTA (*Java Transaction API*):** Coordinador de transacciones distribuidas (garantiza atomicidad global "todo o nada").
  * **JAX-RS / JAX-WS:** Desarrollo de servicios web RESTful (anotaciones `@Path`, `@GET`) y SOAP.

## 4. Servidores de Aplicaciones y Evolución
* **Contenedores Tradicionales:** **Apache Tomcat** (contenedor web ligero), **WildFly** (Open Source líder para Jakarta EE completo), **WebLogic / WebSphere** (sistemas comerciales pesados/*legacy*).
* **Microservicios y Nube (El estándar actual):**
  * **Spring Boot:** Estándar de facto. Crea microservicios autocontenidos con el servidor web embebido.
  * **Quarkus / Micronaut:** Frameworks modernos *Cloud-Native* optimizados para Kubernetes (arranque ultrarrápido y mínimo consumo de memoria).

## 5. Ecosistema de Herramientas de Desarrollo
* **Construcción y Gestión de Dependencias:** **Maven** (utiliza `pom.xml`) y **Gradle**.
* **Entornos de Desarrollo (IDEs):** **IntelliJ IDEA** (líder actual) y **Eclipse** (histórico).
* **Calidad y Control de Código:** **Git** (estándar de versiones), **JUnit 5** y **Mockito** (pruebas unitarias y objetos simulados).

## 6. Conclusión
Java mantiene su dominio en sistemas de misión crítica de las Administraciones Públicas combinando la solidez de sus fundamentos (JVM, Garbage Collector, Jakarta EE) con una enorme capacidad de evolución técnica. La actual transición de pesados servidores de aplicaciones monolíticos hacia arquitecturas ágiles basadas en Spring Boot y Quarkus asegura su viabilidad y escalabilidad a largo plazo.

-------------------------

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
