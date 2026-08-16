# Tema 19.- Spring: Qué es Spring Framework, Ventajas de uso, Ecosistema Spring

## 1. Introducción

Java EE (hoy Jakarta EE), analizado en el Tema 18, establece la especificación estándar para el desarrollo de aplicaciones empresariales en Java. Sin embargo, su complejidad de configuración (descriptores XML, servidores de aplicaciones pesados, EJBs complicados) y su curva de aprendizaje elevada impulsaron el surgimiento de una alternativa más productiva y flexible.

En 2003, **Rod Johnson** publicó *Expert One-on-One J2EE Design and Development*, donde criticaba la complejidad innecesaria de J2EE y proponía un enfoque basado en POJOs (Plain Old Java Objects)e inversión de control. De esa propuesta nació **Spring Framework**, que se ha convertido en el framework de facto para el desarrollo backend en Java, tanto en el sector empresarial como en las Administraciones Públicas.

## 2. ¿Qué es Spring Framework?

### 2.1. Definición

**Spring Framework** es un framework de aplicaciones empresariales de código abierto para la plataforma Java. Proporciona una infraestructura integral que simplifica el desarrollo de aplicaciones complejas, permitiendo al desarrollador centrarse en la lógica de negocio en lugar de en la plomería técnica (gestión de transacciones, acceso a datos, seguridad, etc.).

### 2.2. Filosofía de diseño

*   **Inversión de Control (IoC — Inversion of Control):** El framework toma el control del ciclo de vida y las dependencias de los objetos. El desarrollador "declara qué necesita" y Spring se encarga de proporcionarlo.
*   **Desarrollo basado en POJOs:** Las clases de negocio son simples objetos Java, sin necesidad de heredar de clases especiales del framework ni implementar interfaces invasivas.
*   **Modularidad:** Spring está organizado en módulos independientes. El desarrollador utiliza solo los que necesita.
*   **Testabilidad:** Al basarse en POJOs e inyección de dependencias, el código Spring es fácilmente testeable con pruebas unitarias (JUnit, Mockito) sin necesidad de un servidor de aplicaciones.
*   **No intrusividad:** El código de negocio no depende directamente de las clases de Spring (a diferencia de los EJB clásicos de J2EE).

### 2.3. Origen y evolución

| Versión | Año | Hitos |
|---------|-----|-------|
| Spring 1.0 | 2004 | Inversión de Control, AOP, configuración XML |
| Spring 2.0 | 2006 | Namespaces XML, soporte de anotaciones |
| Spring 3.0 | 2009 | Configuración basada en anotaciones y Java Config (eliminación progresiva de XML) |
| Spring 4.0 | 2013 | Soporte Java 8 (lambdas, streams), WebSocket |
| Spring 5.0 | 2017 | Programación reactiva (WebFlux), soporte Kotlin, JDK 9+ |
| Spring 6.0 | 2022 | Jakarta EE 9+ (cambio de namespace `javax.*` → `jakarta.*`), JDK 17+, AOT (Ahead-of-Time compilation) |

## 3. Ventajas de Uso de Spring

### 3.1. Ventajas técnicas

| Ventaja | Descripción |
|---------|-------------|
| **Inyección de Dependencias (DI)** | Gestión automática de dependencias entre componentes, eliminando el acoplamiento fuerte |
| **Programación Orientada a Aspectos (AOP)** | Separación de preocupaciones transversales (logging, seguridad, transacciones) sin contaminar el código de negocio |
| **Gestión declarativa de transacciones** | `@Transactional` gestiona automáticamente begin/commit/rollback sin código manual |
| **Abstracción de acceso a datos** | Templates y repositorios que simplifican JDBC, JPA, MongoDB, etc. |
| **Integración con estándares** | Compatible con JPA, JMS, JTA, Bean Validation, Servlet API |
| **Programación reactiva** | Spring WebFlux para aplicaciones no bloqueantes de alta concurrencia |
| **Testabilidad** | Inyección de mocks, `@SpringBootTest`, perfiles de prueba |

### 3.2. Ventajas de productividad

*   **Spring Boot:** Autoconfiguración que elimina la configuración XML manual. Un proyecto productivo en minutos con Spring Initializr.
*   **Convención sobre configuración:** Valores por defecto sensatos que el desarrollador puede sobrescribir cuando lo necesite.
*   **Servidor embebido:** Tomcat, Jetty o Undertow embebidos eliminan la dependencia de un servidor de aplicaciones externo.
*   **Comunidad y documentación:** Una de las comunidades Java más grandes. Documentación exhaustiva, tutoriales, Stack Overflow.
*   **Soporte comercial:** VMware (Broadcom) ofrece soporte empresarial a través de **Tanzu** (anteriormente Pivotal).

### 3.3. Spring vs. Java EE / Jakarta EE

| Aspecto | Spring | Java EE / Jakarta EE |
|---------|--------|---------------------|
| Modelo de programación | POJOs + anotaciones + IoC | Especificaciones (EJB, CDI, JPA) |
| Configuración | Autoconfiguración (Spring Boot), Java Config | Descriptores XML (web.xml, ejb-jar.xml) o anotaciones |
| Servidor de aplicaciones | Embebido (Tomcat, Jetty) | Servidor completo (WildFly, WebLogic, WebSphere) |
| Velocidad de desarrollo | Alta (Spring Boot, Spring Initializr) | Media-baja (más ceremonia) |
| Vendor lock-in | No (open source, independiente) | Potencial (servidor de aplicaciones específico) |
| Ecosistema | Enorme (Spring Boot, Data, Security, Cloud, Batch...) | Estándar pero más limitado |
| Uso en AAPP | Predominante en nuevos desarrollos | Legacy, sistemas existentes |

## 4. Arquitectura de Spring Framework

### 4.1. El Contenedor IoC (Inversión de Control)

El corazón de Spring es el **Contenedor IoC**, que gestiona la creación, configuración y ciclo de vida de los objetos de la aplicación (llamados **beans** en terminología Spring).

**Funcionamiento:**
1.  El desarrollador declara los beans y sus dependencias (mediante anotaciones o configuración Java).
2.  El contenedor IoC crea los beans y resuelve sus dependencias automáticamente.
3.  Los beans se almacenan en el **ApplicationContext** (contexto de aplicación), un registro central de componentes.

**Ejemplo:**
```java
@Service
public class TributosService {
    private final ContribuyenteRepository repo;
    
    @Autowired  // Spring inyecta automáticamente el repositorio
    public TributosService(ContribuyenteRepository repo) {
        this.repo = repo;
    }
}
```

### 4.2. Módulos de Spring Framework

Spring Framework se organiza en módulos agrupados por funcionalidad:

| Grupo | Módulos | Función |
|-------|---------|---------|
| **Core Container** | spring-core, spring-beans, spring-context, spring-expression (SpEL) | Contenedor IoC, inyección de dependencias, contexto de aplicación |
| **AOP** | spring-aop, spring-aspects | Programación Orientada a Aspectos |
| **Data Access** | spring-jdbc, spring-orm, spring-tx, spring-jms | Acceso a datos (JDBC, JPA/Hibernate, transacciones, mensajería) |
| **Web** | spring-web, spring-webmvc, spring-webflux | Desarrollo web MVC y reactivo |
| **Security** | spring-security-core, spring-security-web, spring-security-config | Autenticación y autorización |
| **Test** | spring-test | Soporte para pruebas (JUnit, TestContext, MockMvc) |

## 5. Ecosistema Spring

Spring Framework es el núcleo, pero a su alrededor existe un ecosistema de proyectos complementarios que cubren prácticamente cualquier necesidad de desarrollo empresarial:

### 5.1. Proyectos principales

| Proyecto | Función |
|----------|---------|
| **Spring Boot** | Autoconfiguración, servidor embebido, starters. Simplifica radicalmente la creación de aplicaciones Spring |
| **Spring Data** | Abstracción de acceso a datos: JPA, MongoDB, Redis, Elasticsearch. Repositorios con métodos derivados del nombre |
| **Spring Security** | Framework de seguridad: autenticación (LDAP, OAuth2, SAML), autorización (roles, permisos), protección CSRF/XSS |
| **Spring Cloud** | Herramientas para arquitecturas de microservicios: service discovery (Eureka), circuit breaker (Resilience4j), API Gateway, Config Server |
| **Spring Batch** | Procesamiento por lotes (batch processing): ETL, generación masiva de liquidaciones tributarias |
| **Spring Integration** | Integración de sistemas basada en patrones EIP (Enterprise Integration Patterns) |
| **Spring WebFlux** | Framework web reactivo y no bloqueante (alternativa a Spring MVC) |
| **Spring Session** | Gestión de sesiones HTTP distribuidas (Redis, JDBC) |
| **Spring AMQP** | Integración con colas de mensajería (RabbitMQ) |
| **Spring for Apache Kafka** | Integración con Apache Kafka (streaming de eventos) |

### 5.2. Spring Boot: El acelerador

**Spring Boot** merece mención especial por ser el proyecto que ha popularizado Spring en la última década:

*   **Autoconfiguración:** Detecta las dependencias del classpath y configura automáticamente los componentes necesarios (DataSource, EntityManagerFactory, DispatcherServlet).
*   **Starters:** Dependencias pre-empaquetadas que agrupan todo lo necesario para una funcionalidad:
    *   `spring-boot-starter-web` → Spring MVC + Tomcat embebido
    *   `spring-boot-starter-data-jpa` → Spring Data JPA + Hibernate
    *   `spring-boot-starter-security` → Spring Security
*   **Spring Initializr (start.spring.io):** Generador web de proyectos Spring Boot con las dependencias seleccionadas.
*   **Actuator:** Endpoints de monitorización (/health, /metrics, /info) para operaciones.
*   **Fichero de configuración único:** `application.properties` o `application.yml` centraliza toda la configuración.
*   **Perfiles:** Configuración diferenciada por entorno (dev, test, prod) con `application-{profile}.properties`.

### 5.3. Spring Cloud: Microservicios

Para arquitecturas de microservicios, Spring Cloud proporciona:

| Componente | Función |
|-----------|---------|
| **Spring Cloud Config** | Servidor centralizado de configuración (Git-backed) |
| **Spring Cloud Netflix (Eureka)** | Service Discovery: registro y descubrimiento de microservicios |
| **Spring Cloud Gateway** | API Gateway: punto de entrada único, routing, filtros |
| **Spring Cloud Circuit Breaker** | Patrón Circuit Breaker (Resilience4j) para tolerancia a fallos |
| **Spring Cloud Sleuth / Micrometer Tracing** | Trazabilidad distribuida de peticiones entre microservicios |

## 6. Spring en las Administraciones Públicas

### 6.1. Adopción

Spring es el framework predominante en los nuevos desarrollos de las AAPP gracias a:
*   Su madurez y estabilidad (~20 años de evolución).
*   La facilidad de integración con bases de datos Oracle/PostgreSQL (Spring Data JPA).
*   La integración con plataformas comunes (@firma, Cl@ve, SCSP) mediante servicios SOAP/REST.
*   El soporte de seguridad robusto (Spring Security con LDAP/Active Directory).
*   La compatibilidad con las guías CCN-STIC de desarrollo seguro.

### 6.2. Caso de uso típico

```
[Ciudadano] → [Sede Electrónica (Angular)] → [API REST (Spring Boot)] → [Oracle DB (Spring Data JPA)]
                                                     ↓
                                              [Spring Security]
                                              [Integración @firma (SOAP)]
                                              [Integración SCSP/PID (REST)]
```

## 7. Conclusión

Spring Framework ha revolucionado el desarrollo de aplicaciones empresariales en Java al proporcionar un modelo de programación basado en POJOs, Inversión de Control e Inyección de Dependencias que simplifica drásticamente el desarrollo frente a Java EE. Sus ventajas — productividad, modularidad, testabilidad y no intrusividad — lo han convertido en el estándar de facto para el backend de las aplicaciones de las Administraciones Públicas. El ecosistema Spring (Spring Boot, Spring Data, Spring Security, Spring Cloud, Spring Batch) cubre la totalidad de las necesidades de desarrollo empresarial, desde aplicaciones monolíticas hasta arquitecturas de microservicios.
