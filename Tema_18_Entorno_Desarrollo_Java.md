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
