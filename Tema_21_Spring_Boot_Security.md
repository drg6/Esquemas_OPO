# Tema 21.- Spring: Spring Boot, Spring Data, Spring Security.

## 1. Introducción: La Industrialización del Ecosistema Spring
* **La trinidad del desarrollo corporativo:** Si el contenedor IoC y la AOP constituyen la base teórica de Spring, este tema aborda sus tres módulos de producción más críticos: Spring Boot, Spring Data y Spring Security.
* **El cambio de paradigma:** Históricamente, el desarrollo en Java empresarial (JEE) exigía configurar complejos ficheros XML, escribir código repetitivo de acceso a datos y desplegar pesados ficheros WAR en servidores de aplicaciones externos.
* **El objetivo:** Adoptar el principio de **Convención sobre Configuración** para erradicar el código redundante (*boilerplate*) y acelerar la entrega de software listo para producción.

## 2. Spring Boot: Autonomía y Despliegue Moderno
* **2.1. Starters y la resolución del conflicto de dependencias:**
  * Resuelven el histórico problema del empaquetado de librerías (*Dependency Hell*).
  * Son agrupaciones preconfiguradas que importan todas las dependencias compatibles en un único bloque dentro de Maven o Gradle:
    * `spring-boot-starter-web`: Importa Spring MVC, serialización JSON y un servidor embebido.
    * `spring-boot-starter-data-jpa`: Incluye Hibernate, Spring Data y el pool de conexiones de alto rendimiento HikariCP.
    * `spring-boot-starter-security`: Activa el motor de autenticación y autorización.
* **2.2. Autoconfiguración (`@EnableAutoConfiguration`):**
  * El framework escanea el *classpath* en el arranque y autoconfigura la infraestructura necesaria. Si detecta el conector de una base de datos, inicializa el *DataSource* y el control transaccional sin intervención manual.
  * El desarrollador siempre mantiene el control, pudiendo sobrescribir cualquier propiedad mediante `application.properties` o `application.yml`.
* **2.3. Servidor Embebido (El modelo Fat JAR):**
  * Rompe con la necesidad de servidores web externos instalados en el sistema operativo.
  * Integra directamente Tomcat o Undertow dentro de un archivo `.jar` ejecutable. Esto convierte a la aplicación en un componente autónomo, diseñado a medida para su empaquetado en contenedores **Docker** y su orquestación en **Kubernetes**.
* **2.4. Gestión de entornos mediante Perfiles (`@Profile`):**
  * Aplica los principios de la metodología *Twelve-Factor App*. Desacopla el código compilado de la configuración sensible, permitiendo inyectar contraseñas de bases de datos mediante variables de entorno en entornos de desarrollo, pruebas o producción.
* **2.5. Observabilidad con Spring Boot Actuator:**
  * **Detalle técnico de infraestructura:** Vital para la monitorización municipal. Expone endpoints nativos como `/actuator/health`, utilizado como sonda de comprobación (*liveness* y *readiness*) por Kubernetes, y `/actuator/metrics`, integrable con sistemas corporativos como Prometheus y Grafana.

## 3. Spring Data: La Abstracción Universal del Almacenamiento
* **3.1. Supresión del código redundante (Boilerplate):**
  * Elimina la necesidad de implementar manualmente la capa DAO (*Data Access Object*) para cada una de las tablas de una base de datos.
* **3.2. El Patrón Repository (`JpaRepository`):**
  * Basta con declarar una interfaz Java que herede de `JpaRepository<Entidad, ID>`. En tiempo de ejecución, Spring genera automáticamente en memoria la implementación completa con operaciones CRUD transaccionales como `save()`, `findById()` o `delete()`.
* **3.3. Consultas Semánticas (Query Methods):**
  * El framework infiere la sentencia SQL directamente leyendo el nombre del método. Por ejemplo, `findByDniAndEstado(String dni, String est)` genera internamente el correspondiente `SELECT` con cláusulas `WHERE`, operadores lógicos y ordenaciones automáticas.
* **3.4. Consultas Avanzadas y Optimización (`@Query`):**
  * Permite redactar consultas explícitas en JPQL (orientadas a objetos) o SQL nativo mediante el atributo `nativeQuery`.
  * **Detalle de rendimiento (El problema N+1):** En tramitación de expedientes, consultar una entidad con relaciones (*hijos*) puede provocar decenas de consultas secundarias innecesarias. Un arquitecto de software debe solventarlo utilizando cláusulas `JOIN FETCH` en la anotación `@Query` o definiendo *Entity Graphs*.
* **3.5. Paginación y modelo políglota:**
  * Soporte nativo de paginación mediante interfaces `Pageable`, optimizando el tráfico de red en listados masivos.
  * El mismo patrón de repositorios es reutilizable en motores NoSQL mediante **Spring Data MongoDB** o **Spring Data Redis**.

## 4. Spring Security: El Bastión de Autenticación y Control de Acceso
* **4.1. Arquitectura basada en filtros (`SecurityFilterChain`):**
  * Implementa el patrón *Intercepting Filter*. Una cadena de filtros de seguridad intercepta cada petición HTTP antes de llegar a la capa de negocio, comprobando identidades y roles.
* **4.2. Mecanismos de Autenticación en el Sector Público:**
  * **JWT (JSON Web Tokens):** Estándar para APIs REST *stateless*, donde el cliente envía un token criptográfico firmado en cada cabecera HTTP sin necesidad de guardar sesión en servidor.
  * **Integración con Cl@ve y SSO:** Soporte nativo de protocolos estándar como **OAuth 2.0 / OpenID Connect** y **SAML2**, permitiendo delegar la identidad en pasarelas públicas como Cl@ve o directorios corporativos Microsoft Entra ID.
  * **Certificados Digitales (X.509):** Autenticación mediante tarjeta chip o DNI electrónico para empleados públicos y sedes.
* **4.3. Niveles de Autorización:**
  * *A nivel de red (HTTP):* Configuración declarativa en la cadena de filtros para proteger rutas (por ejemplo, restringiendo `/api/admin/**` a determinados perfiles).
  * *A nivel de método (AOP):* Mediante anotaciones como **`@PreAuthorize`** directamente sobre los métodos de servicio, validando expresiones lógicas de seguridad antes de ejecutar el trámite administrativo.
* **4.4. Mitigación de vulnerabilidades por diseño:**
  * Protección activa contra ataques CSRF (Falsificación de Petición en Sitios Cruzados).
  * Inyección automática de cabeceras de protección web contra *Clickjacking* (`X-Frame-Options`) y Cross-Site Scripting (XSS), garantizando el cumplimiento de las guías de bastionado del **CCN-STIC**.

## 5. Conclusión: Relevancia en la Administración Electrónica
La madurez combinada de Spring Boot, Spring Data y Spring Security ha transformado la ingeniería del software en el sector público. Spring Boot dota a los servicios municipales de portabilidad y facilidad de despliegue mediante microservicios contenerizados; Spring Data optimiza y acelera la persistencia de expedientes reduciendo errores humanos; y Spring Security blinda los datos de los ciudadanos conforme a las directrices de integridad, autenticidad y trazabilidad que exige el **Esquema Nacional de Seguridad (RD 311/2022)**.

---------------------

# Tema 21.- Spring: Spring Boot, Spring Data, Spring Security

## 1. Introducción

Históricamente, configurar una aplicación Spring requería extensos ficheros XML, un despliegue manual en servidores de aplicaciones (como Tomcat o WebLogic) y la escritura de código repetitivo para el acceso a datos y la seguridad. Estos tres módulos nacen para erradicar esa complejidad, permitiendo a los desarrolladores centrarse exclusivamente en la lógica de negocio.

## 2. Spring Boot: Configuración por Convención

Spring Boot no es un framework distinto, sino una capa que simplifica la creación de aplicaciones listas para producción (*production-ready*)

### 2.1. Starters: Gestión simplificada de dependencias

Los **Starters** son dependencias preconfiguradas que agrupan funcionalmente las librerías necesarias. En lugar de buscar y configurar manualmente, el desarrollador añade un único Starter al fichero `pom.xml` (Maven) o `build.gradle` (Gradle):

* **`spring-boot-starter-web`:** Importa Spring MVC, JSON y un servidor embebido.
* **`spring-boot-starter-data-jpa`:** Agrupa Hibernate, Spring Data y el pool HikariCP (pool de conexiones).
* **`spring-boot-starter-security`:** Despliega el motor de autenticación y protección de endpoints.
* **`spring-boot-starter-actuator`:** Endpoints de monitorización y gestión.

### 2.2. Autoconfiguración (@EnableAutoConfiguration)

Spring Boot examina el **classpath** (las bibliotecas presentes en el proyecto) y configura automáticamente los componentes necesarios:

*   Si detecta el driver JDBC de Oracle y JPA, autoconfigura un `DataSource`, el `EntityManager` y el control de transacciones.
*   Si detecta Spring Security, protege por defecto todos los endpoints exigiendo autenticación HTTP Basic.

Personalizar cualquier autoconfiguración mediante el fichero `application.properties` o `application.yml`.

### 2.3. Servidor embebido (Fat JAR)

Spring Boot no utiliza archivos `.war` sino que empaqueta el servidor web (Tomcat) directamente dentro del ejecutable. 
**"Fat JAR"** autónomo que se ejecuta con un simple comando `java -jar aplicacion.jar`, alineandose arquitecturas modernas (microservicios y contenedores Docker/Kubernetes).

### 2.4. Perfiles de configuración

Spring Boot desacopla la configuración del código fuente `application-produccion.yml` mediante **perfiles** (ej. *desarrollo*, *preproduccion*, *produccion*). 
Ej. Credenciales de base de datos se inyectan en tiempo de ejecución mediante variables de entorno, garantizando la seguridad.

### 2.5. Actuator: Monitorización en producción

Actuator expone *endpoints* HTTP nativos para la monitorización de la aplicación en producción. Destacan `/actuator/health` (estado de salud y conexiones), `/actuator/metrics` (consumo de CPU, memoria, tiempos de respuesta) y `/actuator/loggers` (para modificar niveles de traza dinámicamente sin reiniciar el servicio).

## 3. Spring Data: Acceso a Datos sin Boilerplate

### 3.1. El problema del código repetitivo

En Java EE (JEE) clásico, DAO (*Data Access Object*) por cada tabla (operaciones CRUD, conexiones y excepciones). Spring Data nace para erradicar este código redundante.

### 3.2. Repositorios Spring Data

**Spring Data JPA** elimina este código repetitivo mediante **Repositorio** `JpaRepository`.
Genera automáticamente la implementación en tiempo de ejecución, inyectando métodos funcionales como `save()`, `findById()`, `findAll()` y `deleteById()`, con transaccionalidad automática.

### 3.3. Query Methods: Consultas derivadas del nombre del método

Spring Data puede inferir consultas SQL a partir del nombre del método, siguiendo una estricta convención de nomenclatura:
* Un método llamado `findByApellidosAndEdadGreaterThan(String ap, int edad)` es traducido automáticamente por el framework a: `SELECT * FROM tabla WHERE apellidos = ? AND edad > ?`.
* Soporta operadores lógicos, ordenación (`OrderByNombreAsc`) y conteo (`countByMunicipio`).

### 3.4. Consultas personalizadas con @Query

Anotación **`@Query`** para consultas personalizadas y permite sentencias complejas en JPQL (orientadas a objetos) o en SQL nativo (`nativeQuery = true`), parámetros de entrada con `@Param`.

### 3.5. Paginación y ordenación

Grandes volúmenes de datos -> objeto `Pageable` (ej. `PageRequest.of(0, 20)`), devuelve registros y calcula además total de registros, páginas disponible.

### 3.6. Ecosistema Multimodelo

Ecosistemas NoSQL: **Spring Data MongoDB** (documentos), **Redis** (caché clave-valor) o **Elasticsearch** (motores de búsqueda).

## 4. Spring Security: Autenticación y Autorización

### 4.1. Concepto

Gestiona **Autenticación** (verificar la identidad del usuario,quién es) y **Autorización** (controlar el acceso a recursos,qué puede hacer).
Patrón *Intercepting Filter*: Inyecta una **Security Filter Chain** que intercepta toda petición HTTP *antes* de llegar al controlador `DispatcherServlet`, validando credenciales o tokens.

### 4.2. Mecanismos de Autenticación

* **Form-based y HTTP Basic:** Tradicionales para aplicaciones web y APIs internas simples.
* **JWT (JSON Web Token):** El estándar para APIs REST. Es un mecanismo *stateless* (sin sesión en servidor) donde el cliente envía un token criptográfico firmado en cada cabecera HTTP que se genera tras el login.
* **OAuth 2.0 / OpenID Connect:** Delegación de la autenticación a un proveedor externo (Google, Cl@ve, Azure AD). El estándar para Single Sign-On (SSO).
* **X.509 / DNIe:** Autenticación mediante certificados digitales.
* **LDAP/Active Directory:** Autenticación contra un directorio corporativo (Active Directory).

### 4.3.  Autorización y Control de Acceso

Spring Security permite aplicar políticas de control de acceso a dos niveles:

1. **A nivel de ruta (HTTP):** Mediante la configuración `SecurityFilterChain`, definiendo reglas (ej. permitir acceso `/api/public/**` a todos, pero restringir `/api/tributos/**` mediante `hasRole('TECNICO')`).
2. **A nivel de método (AOP):** Utilizando anotaciones como **`@PreAuthorize("hasRole('ADMIN'))`** .

###  4.4. Defensas Integradas por Defecto

Spring Security incluye protecciones automáticas contra vulnerabilidades comunes:

* **CSRF (Cross-Site Request Forgery, Falsificación de Petición en Sitios Cruzados):** Exige e inyecta tokens transaccionales en operaciones mutables. *(Se deshabilita habitualmente en APIs REST stateless con JWT).*
* **Cabeceras de Seguridad:** Inyección automática de `X-Frame-Options` (contra *Clickjacking*, Secuestro de clic) y protecciones **XSS** (*Cross-Site Scripting*).
* **Gestión CORS:** Control estricto de los orígenes cruzados (*Cross-Origin Resource Sharing*).

## 5. Conclusión

Esta trinidad tecnológica es el estándar del desarrollo Java moderno:

* **Spring Boot:** Elimina configuraciones manuales y despliega aplicaciones autónomas en minutos.
* **Spring Data:** Suprime el código repetitivo automatizando las consultas a la base de datos.
* **Spring Security:** Blinda la aplicación gestionando identidades, permisos y defendiendo contra ataques web.

En la **Administración Pública**, dominar estas herramientas es clave para crear servicios robustos, integrables con sistemas estatales (Cl@ve, DNIe) y alineados estrictamente con el **Esquema Nacional de Seguridad (ENS)**