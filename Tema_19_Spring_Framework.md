# Tema 19.- Spring: Qué es Spring Framework, Ventajas de uso, Ecosistema Spring.

## 1. Introducción y Origen
* **El problema de Java EE:** La especificación original (J2EE) exigía arquitecturas pesadas, descriptores XML complejos y EJBs difíciles de mantener.
* **El origen:** Creado por **Rod Johnson** (2003) como respuesta a esta complejidad, proponiendo un modelo ligero basado en **POJOs** (*Plain Old Java Objects*) y la **Inversión de Control**. Hoy es el estándar *de facto* para backend corporativo.

## 2. Filosofía de Diseño y Conceptos Core
* **Inversión de Control (IoC):** El framework asume el control del flujo y del ciclo de vida de los objetos (cuándo se crean y destruyen), liberando al programador de esa gestión.
* **Inyección de Dependencias (DI):** Es la materialización del IoC. El framework inyecta las dependencias necesarias en tiempo de ejecución (ej. mediante `@Autowired`), eliminando el acoplamiento fuerte (uso de `new`).
* **Programación Orientada a Aspectos (AOP):** Permite aislar y encapsular la lógica transversal (seguridad, auditoría, logs, transacciones) para no "ensuciar" las clases de negocio principales.
* **Evolución *Cloud Native*:** Soporte para programación reactiva (Spring WebFlux) y **compilación nativa AOT (*Ahead-of-Time*) mediante GraalVM**, reduciendo el arranque a milisegundos para competir en entornos Kubernetes.

## 3. Arquitectura del Contenedor
* **`ApplicationContext`:** Es el corazón (Contenedor IoC) de Spring.
* **Ciclo de vida:** Al arrancar, escanea las clases anotadas (`@Service`, `@Repository`), las instancia creando **Beans** que quedan residentes en memoria, y resuelve el árbol de dependencias inyectándolas automáticamente.
* **Modularidad:** Estructurado en bloques independientes: *Core Container, AOP, Data Access (JDBC/ORM/Transacciones), Web (MVC)* y *Test*.

## 4. Ventajas de Uso
* **Ventajas Técnicas:**
  * **Desacoplamiento:** Facilitado por DI, permite evolucionar el código de forma segura.
  * **Transaccionalidad Declarativa:** Uso de `@Transactional` para que el framework gestione los *commits* y *rollbacks* automáticamente.
  * **Alta Testabilidad:** Al basarse en POJOs, facilita la creación de pruebas unitarias aisladas con JUnit y Mockito.
* **Ventajas de Productividad:**
  * Reducción extrema del código repetitivo (*boilerplate*).
  * Independencia del servidor externo (despliegues ágiles).
  * Respaldo de una gran comunidad y soporte comercial corporativo (Tanzu/Broadcom).

## 5. El Ecosistema Spring
El framework base se amplía con proyectos satélite especializados:
* **Spring Boot (El Acelerador):**
  * **Autoconfiguración:** Detecta e inicializa librerías sin código manual.
  * **Starters:** Paquetes de dependencias listos para usar (ej. `spring-boot-starter-web`).
  * **Servidores Embebidos:** Incrusta Tomcat o Undertow para generar un `.jar` autoejecutable.
  * **Actuator:** Expone *endpoints* de métricas y salud (`/health`) para entornos de producción.
* **Spring Data:** Abstrae el acceso a datos. Con **Spring Data JPA**, el desarrollador define interfaces y Spring genera automáticamente las sentencias SQL.
* **Spring Security:** Estándar robusto para autenticación (LDAP, Active Directory, OAuth2) y autorización por roles/URLs.
* **Spring Cloud:** Herramientas para la orquestación de arquitecturas de Microservicios (*API Gateway, Config Server, Service Discovery*).

## 6. Spring vs. Jakarta EE
* **Complementariedad, no rivalidad:** Spring no reinventa los estándares; aporta el modelo productivo (IoC, Boot), pero internamente **se apoya en las especificaciones de Jakarta EE**. Spring Web usa la *Servlet API*, Spring Data usa *Jakarta Persistence (Hibernate)*, y las validaciones usan *Jakarta Bean Validation*.

## 7. Spring en la Administración Pública
* **Por qué domina en las AAPP:** Estabilidad de 20 años, integración natural con bases de datos relacionales (Oracle/PostgreSQL), cumplimiento de normativas CCN-STIC y facilidad para consumir servicios del Estado.
* **Arquitectura de Referencia (Caso Típico):**
  1. **Frontend:** SPA en Angular o React.
  2. **Backend:** API REST construida con **Spring Boot**.
  3. **Seguridad e Identidad:** **Spring Security** integrado con pasarelas públicas como Cl@ve.
  4. **Interoperabilidad:** Clientes consumiendo servicios REST/SOAP de la plataforma @firma o SCSP.
  5. **Persistencia:** **Spring Data JPA** conectando con la base de datos municipal.

-------------------

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
