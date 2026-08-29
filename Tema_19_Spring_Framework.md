# Tema 19.- Spring: Qué es Spring Framework, Ventajas de uso, Ecosistema Spring

## 1. Introducción

*   **Problema inicial:** Complejidad de Java EE (descriptores XML, servidores pesados).
*   **Solución (Rod Johnson, 2003):** Framework basado en POJOs e Inversión de Control (IoC).
*   **Estado actual:** Estándar *de facto* para backend Java en sector privado y AP.

## 2. ¿Qué es y Filosofía de Diseño?
*   **Inversión de Control (IoC):** El framework crea y gestiona el ciclo de vida de los objetos (Beans).
*   **Inyección de Dependencias (DI):** Spring resuelve e inyecta dependencias automáticamente (`@Autowired`).
*   **POJOs:** Objetos planos, sin herencias intrusivas.
*   **AOP (Orientación a Aspectos):** Aísla lógica repetitiva (ej. logs, seguridad) fuera del código de negocio.
*   **Evolución:** XML → Anotaciones → Java Config → Reactivo (WebFlux) → Spring 6 / Boot 3 (GraalVM / Compilación nativa AOT).

## 3. Arquitectura (El Contenedor IoC)
1. El desarrollador declara Beans (ej. `@Service`).
2. El contenedor IoC los instancia y resuelve dependencias.
3. Se almacenan en el **ApplicationContext**.
*   *Módulos Core:* Core, Beans, Context, AOP, Data Access, Web.

## 4. Ventajas de Uso de Spring

*   **4.1. Técnicas:**
    *   **Desacoplamiento (DI):** Dependencias inyectadas = código fácil de refactorizar.
    *   **Modularidad (AOP):** Aísla la lógica transversal (logs, seguridad).
    *   **Transacciones Declarativas:** `@Transactional` (gestión automática de commit/rollback).
    *   **Testabilidad:** Uso de POJOs = pruebas ágiles (JUnit/Mockito) sin servidor.
    
*   **4.2. Productividad:**
    *   **Cero "Boilerplate":** Autoconfiguración gracias a Spring Boot.
    *   **Servidor Embebido:** Ejecución directa en `.jar` (ideal para Docker/Kubernetes).
    *   **Respaldo y Comunidad:** Soporte corporativo (Tanzu/Broadcom), vital para la AP.

## 5. El Ecosistema Spring
*   **Spring Boot:** El acelerador. Autoconfiguración, servidor embebido (Tomcat), *Starters*, métricas (Actuator).
*   **Spring Data:** Abstracción de bases de datos (JPA, Mongo).
*   **Spring Security:** Autenticación (LDAP, OAuth2) y autorización.
*   **Spring Cloud:** Para microservicios (Configuración, API Gateway, Eureka).

## 6. Spring vs. Jakarta EE (Matiz clave)
*   No son excluyentes. Spring aporta el modelo de programación (IoC, abstracción), pero **se apoya por debajo en estándares Jakarta EE** (Servlet API, JPA, Bean Validation).

## 7. Spring en las Administraciones Públicas
*   Framework predominante por su madurez, seguridad (CCN-STIC) e integraciones.
*   **Arquitectura típica:** Ciudadano → Sede (Angular) → API REST (Spring Boot) → BD (Oracle/Spring Data JPA).
*   **Integraciones AP:** @firma (SOAP), SCSP/PID (REST), Cl@ve (Spring Security).

## 8. Conclusión

Spring Framework ha revolucionado el desarrollo de aplicaciones empresariales en Java al simplificar drásticamente el desarrollo frente a Java EE. 
Sus ventajas — productividad, modularidad, testabilidad y no intrusividad — estándar de facto para el backend de las aplicaciones de las AP. 
El ecosistema Spring (Spring Boot, Spring Data, Spring Security, Spring Cloud) cubre la totalidad de las necesidades de desarrollo empresarial.
