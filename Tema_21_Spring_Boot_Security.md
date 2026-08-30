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