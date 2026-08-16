# Tema 21.- Spring: Spring Boot, Spring Data, Spring Security

## 1. Introducción

El Tema 19 introdujo Spring Framework y Spring Boot a nivel conceptual. El Tema 20 profundizó en el Contexto de Aplicación, la Inyección de Dependencias y AOP. Este tema completa la trilogía de Spring abordando en detalle tres subproyectos que conforman la columna vertebral del desarrollo backend empresarial moderno: **Spring Boot** (simplificación y autoconfiguración), **Spring Data** (acceso a datos sin código repetitivo) y **Spring Security** (autenticación y autorización).

## 2. Spring Boot: Configuración por Convención

### 2.1. Starters: Gestión simplificada de dependencias

Los **Starters** son dependencias preconfiguradas que agrupan todas las bibliotecas necesarias para una funcionalidad específica. En lugar de buscar y configurar manualmente decenas de bibliotecas compatibles, el desarrollador añade un único Starter al fichero `pom.xml` (Maven) o `build.gradle` (Gradle):

| Starter | Incluye |
|---------|---------|
| `spring-boot-starter-web` | Tomcat embebido, Spring MVC, Jackson (JSON) |
| `spring-boot-starter-data-jpa` | Hibernate, HikariCP (pool de conexiones), Spring Data JPA |
| `spring-boot-starter-security` | Spring Security, filtros de autenticación |
| `spring-boot-starter-test` | JUnit 5, Mockito, Spring Test, MockMvc |
| `spring-boot-starter-validation` | Bean Validation (Hibernate Validator) |
| `spring-boot-starter-actuator` | Endpoints de monitorización y gestión |
| `spring-boot-starter-mail` | JavaMail para envío de correos electrónicos |

### 2.2. Autoconfiguración (@EnableAutoConfiguration)

Spring Boot examina el **classpath** (las bibliotecas presentes en el proyecto) y configura automáticamente los componentes necesarios:

*   Si detecta un driver JDBC de Oracle (`ojdbc`) y JPA en las dependencias → configura automáticamente un `DataSource`, un `EntityManagerFactory` y un `TransactionManager`.
*   Si detecta Spring Security → configura automáticamente la protección de todos los endpoints con autenticación HTTP Basic.
*   Si detecta Thymeleaf → configura automáticamente el motor de plantillas.

El desarrollador puede personalizar cualquier autoconfiguración mediante el fichero `application.properties` o `application.yml`.

### 2.3. Servidor embebido

Spring Boot integra el servidor web (Tomcat, Jetty o Undertow) dentro del propio ejecutable `.jar`:

```bash
# Compilar la aplicación
mvn clean package

# Ejecutar directamente — no requiere servidor externo
java -jar tributos-municipales-1.0.jar

# O con perfil de configuración específico
java -jar tributos-municipales-1.0.jar --spring.profiles.active=produccion
```

### 2.4. Perfiles de configuración

Spring Boot permite definir configuraciones diferentes para cada entorno:

```yaml
# application-desarrollo.yml
spring:
  datasource:
    url: jdbc:oracle:thin:@//localhost:1521/XEPDB1
    username: dev_user

# application-produccion.yml
spring:
  datasource:
    url: jdbc:oracle:thin:@//oracle-prod:1521/ORCL
    username: ${DB_USER}
    password: ${DB_PASSWORD}
```

### 2.5. Actuator: Monitorización en producción

El módulo **Spring Boot Actuator** expone endpoints HTTP para monitorizar la aplicación en producción:

| Endpoint | Función |
|----------|---------|
| `/actuator/health` | Estado de salud (UP/DOWN, conexión a BD, espacio en disco) |
| `/actuator/metrics` | Métricas de rendimiento (CPU, memoria, peticiones HTTP) |
| `/actuator/info` | Información de la aplicación (versión, descripción) |
| `/actuator/loggers` | Consulta y modificación dinámica del nivel de logging |
| `/actuator/env` | Variables de entorno y propiedades de configuración |

## 3. Spring Data: Acceso a Datos sin Boilerplate

### 3.1. El problema del código repetitivo

En la programación JEE tradicional, cada tabla de la base de datos requería la implementación manual de un DAO (Data Access Object) con métodos CRUD (Create, Read, Update, Delete), gestión de conexiones, transacciones y mapeo de resultados. Si un ayuntamiento tiene 50 tablas, se necesitan 50 DAOs con código prácticamente idéntico.

### 3.2. Repositorios Spring Data

**Spring Data JPA** elimina este código repetitivo mediante el concepto de **Repositorio**: una interfaz Java que extiende de `JpaRepository`. Spring genera automáticamente la implementación en tiempo de ejecución:

```java
public interface ContribuyenteRepository extends JpaRepository<Contribuyente, String> {

    // Spring genera automáticamente la implementación de estos métodos:
    // - save(contribuyente)
    // - findById(dni)
    // - findAll()
    // - deleteById(dni)
    // - count()
    // - existsById(dni)
}
```

### 3.3. Query Methods: Consultas derivadas del nombre del método

Spring Data genera consultas SQL automáticamente a partir del nombre del método, siguiendo convenciones de nomenclatura:

```java
public interface ContribuyenteRepository extends JpaRepository<Contribuyente, String> {

    // SELECT * FROM contribuyente WHERE apellidos = ? AND edad > ?
    List<Contribuyente> findByApellidosAndEdadGreaterThan(String apellidos, int edad);

    // SELECT * FROM contribuyente WHERE domicilio LIKE ?
    List<Contribuyente> findByDomicilioContaining(String fragmento);

    // SELECT * FROM contribuyente WHERE activo = true ORDER BY nombre ASC
    List<Contribuyente> findByActivoTrueOrderByNombreAsc();

    // SELECT COUNT(*) FROM contribuyente WHERE municipio = ?
    long countByMunicipio(String municipio);

    // SELECT * FROM contribuyente WHERE deuda > ? AND activo = true
    List<Contribuyente> findByDeudaGreaterThanAndActivoTrue(BigDecimal umbral);
}
```

### 3.4. Consultas personalizadas con @Query

Para consultas complejas que no se pueden expresar mediante nombres de métodos:

```java
@Query("SELECT c FROM Contribuyente c WHERE c.deuda > :umbral AND c.municipio = :municipio")
List<Contribuyente> buscarDeudoresPorMunicipio(
    @Param("umbral") BigDecimal umbral,
    @Param("municipio") String municipio
);

@Query(value = "SELECT * FROM contribuyente WHERE ROWNUM <= :limite", nativeQuery = true)
List<Contribuyente> buscarTopContribuyentes(@Param("limite") int limite);
```

### 3.5. Paginación y ordenación

```java
// Consulta paginada: página 0, 20 resultados, ordenados por nombre
Page<Contribuyente> pagina = repository.findAll(PageRequest.of(0, 20, Sort.by("nombre")));

pagina.getContent();       // Lista de contribuyentes de esta página
pagina.getTotalPages();     // Número total de páginas
pagina.getTotalElements();  // Número total de registros
```

### 3.6. Spring Data para otras bases de datos

Spring Data proporciona módulos específicos para bases de datos no relacionales:

| Módulo | Base de datos |
|--------|---------------|
| Spring Data MongoDB | MongoDB (documentos JSON) |
| Spring Data Redis | Redis (clave-valor en memoria) |
| Spring Data Elasticsearch | Elasticsearch (búsqueda textual) |
| Spring Data Neo4j | Neo4j (grafos) |
| Spring Data Cassandra | Apache Cassandra (columnas anchas) |

## 4. Spring Security: Autenticación y Autorización

### 4.1. Concepto

**Spring Security** es el framework de seguridad de Spring que proporciona autenticación (verificar la identidad del usuario) y autorización (controlar el acceso a recursos) de forma declarativa e integrada con el ecosistema Spring.

### 4.2. Cadena de filtros de seguridad (Security Filter Chain)

Spring Security intercepta todas las peticiones HTTP mediante una cadena de filtros que se ejecutan antes de que la petición llegue al controlador:

```
Petición HTTP → [Filtro CORS] → [Filtro CSRF] → [Filtro de Autenticación] 
    → [Filtro de Autorización] → Controlador
```

### 4.3. Autenticación: Mecanismos soportados

*   **Formulario de login (Form-based):** Página HTML de login clásica con usuario y contraseña.
*   **HTTP Basic:** Credenciales en la cabecera `Authorization` de cada petición (solo para APIs internas).
*   **JWT (JSON Web Token):** Token criptográfico que el servidor genera tras el login y que el cliente envía en cada petición. No requiere sesión en el servidor (stateless). Ideal para APIs REST.
*   **OAuth 2.0 / OpenID Connect:** Delegación de la autenticación a un proveedor externo (Google, Cl@ve, Azure AD). El estándar para Single Sign-On (SSO).
*   **Certificado X.509 / DNIe:** Autenticación mediante certificado digital del ciudadano.
*   **LDAP:** Autenticación contra un directorio corporativo (Active Directory).

### 4.4. Autorización: Control de acceso basado en roles

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/tributos/**").hasRole("TECNICO_TRIBUTOS")
                .requestMatchers("/api/admin/**").hasRole("ADMINISTRADOR")
                .anyRequest().authenticated()
            )
            .oauth2Login();  // Login con OAuth 2.0

        return http.build();
    }
}
```

### 4.5. Seguridad a nivel de método

```java
@Service
public class TributoService {

    @PreAuthorize("hasRole('ADMIN_TRIBUTOS')")
    public void anularRecibo(Long idRecibo) {
        // Solo usuarios con rol ADMIN_TRIBUTOS pueden ejecutar este método
    }

    @PreAuthorize("hasRole('TECNICO') or #dni == authentication.principal.username")
    public Contribuyente consultar(String dni) {
        // Técnicos o el propio contribuyente pueden consultar
    }
}
```

### 4.6. Protecciones integradas

Spring Security incluye protecciones automáticas contra vulnerabilidades comunes:
*   **CSRF (Cross-Site Request Forgery):** Token CSRF en formularios.
*   **XSS (Cross-Site Scripting):** Cabeceras de seguridad HTTP.
*   **Clickjacking:** Cabecera `X-Frame-Options`.
*   **Session Fixation:** Regeneración del ID de sesión tras el login.
*   **CORS (Cross-Origin Resource Sharing):** Control de orígenes permitidos.

## 5. Conclusión

Spring Boot, Spring Data y Spring Security conforman la trinidad del desarrollo backend empresarial moderno en Java. Spring Boot elimina la complejidad de configuración mediante autoconfiguración y starters, permitiendo arrancar una aplicación con servidor embebido en minutos. Spring Data erradica el código repetitivo de acceso a datos mediante repositorios con consultas derivadas del nombre del método. Spring Security proporciona un sistema de autenticación y autorización completo, con soporte para JWT, OAuth 2.0, roles y protecciones integradas contra las vulnerabilidades web más comunes.

En el contexto de las Administraciones Públicas, esta trinidad permite construir APIs REST seguras que se integran con los sistemas de identificación ciudadana (Cl@ve, certificados digitales) y con las bases de datos corporativas (Oracle, PostgreSQL), aplicando los niveles de seguridad exigidos por el Esquema Nacional de Seguridad.
