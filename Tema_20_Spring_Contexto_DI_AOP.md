# Tema 20.- Spring: Contexto, Inyección de dependencias, AOP

## 1. Introducción

El Tema 19 presentó Spring Framework, sus ventajas y su ecosistema. Este tema profundiza en los **tres pilares arquitectónicos** que constituyen el núcleo de Spring: el **Contexto de Aplicación (ApplicationContext)**, la **Inyección de Dependencias (Dependency Injection — DI)** y la **Programación Orientada a Aspectos (Aspect-Oriented Programming — AOP)**.

Comprender estos tres mecanismos es esencial, ya que sobre ellos se construyen todos los demás proyectos del ecosistema Spring (Spring Boot, Spring Data, Spring Security, Spring MVC).

## 2. El Contexto de Aplicación (ApplicationContext)

### 2.1. Concepto

El **ApplicationContext** es el contenedor central de Spring. Es un registro (mapa) que contiene todos los objetos gestionados por el framework — denominados **beans** — y gestiona su ciclo de vida, configuración y dependencias.

Cuando una aplicación Spring arranca, el contenedor:
1.  Lee la configuración (anotaciones, clases `@Configuration`, ficheros XML).
2.  Crea instancias de todos los beans declarados.
3.  Resuelve e inyecta las dependencias entre ellos.
4.  Ejecuta métodos de inicialización (`@PostConstruct`).
5.  Pone todos los beans a disposición de la aplicación.

### 2.2. Tipos de ApplicationContext

| Implementación | Uso |
|---------------|-----|
| `AnnotationConfigApplicationContext` | Configuración basada en anotaciones y clases Java (Spring moderno) |
| `ClassPathXmlApplicationContext` | Configuración basada en ficheros XML (legacy) |
| `WebApplicationContext` | Contexto para aplicaciones web (integrado con DispatcherServlet) |
| `SpringApplication` (Spring Boot) | Contexto autoconfigurado con servidor embebido |

### 2.3. Definición de Beans

Un **bean** es cualquier objeto cuyo ciclo de vida es gestionado por el contenedor Spring.

**Mediante anotaciones (forma moderna):**
```java
@Component        // Bean genérico
@Service          // Bean de lógica de negocio
@Repository       // Bean de acceso a datos (agrega traducción de excepciones)
@Controller       // Bean controlador web (MVC)
@RestController   // @Controller + @ResponseBody (APIs REST)
```

**Mediante clase de configuración:**
```java
@Configuration
public class AppConfig {
    @Bean
    public DataSource dataSource() {
        return DataSourceBuilder.create()
            .url("jdbc:oracle:thin:@servidor:1521:MUNI")
            .username("app_tributos")
            .build();
    }
}
```

### 2.4. Ámbitos (Scopes) de los Beans

| Scope | Descripción | Uso |
|-------|-------------|-----|
| **singleton** (por defecto) | Una única instancia del bean en todo el contexto | Servicios stateless, repositorios |
| **prototype** | Nueva instancia cada vez que se solicita el bean | Objetos con estado variable |
| **request** | Una instancia por petición HTTP | Datos de la petición web |
| **session** | Una instancia por sesión HTTP | Datos de sesión del usuario |
| **application** | Una instancia por ServletContext | Configuración compartida de la aplicación web |

### 2.5. Ciclo de vida de un Bean

```
[Instanciación] → [Inyección de dependencias] → [@PostConstruct] → [Uso] → [@PreDestroy] → [Destrucción]
```

*   `@PostConstruct`: Método ejecutado después de la inyección de dependencias (inicialización personalizada).
*   `@PreDestroy`: Método ejecutado antes de la destrucción del bean (liberación de recursos).

### 2.6. Perfiles (Profiles)

Los **perfiles** permiten activar diferentes beans según el entorno de ejecución:

```java
@Configuration
@Profile("desarrollo")
public class DesarrolloConfig {
    @Bean
    public DataSource dataSource() {
        // Base de datos H2 en memoria para desarrollo
        return new EmbeddedDatabaseBuilder().setType(EmbeddedDatabaseType.H2).build();
    }
}

@Configuration
@Profile("produccion")
public class ProduccionConfig {
    @Bean
    public DataSource dataSource() {
        // Oracle en producción
        return DataSourceBuilder.create()
            .url("jdbc:oracle:thin:@oracleProd:1521:MUNI")
            .build();
    }
}
```

Activación: `spring.profiles.active=produccion` en `application.properties`.

## 3. Inyección de Dependencias (Dependency Injection — DI)

### 3.1. Concepto

La **Inyección de Dependencias** es un patrón de diseño (una forma concreta de implementar la Inversión de Control — IoC) en el que las dependencias de un objeto no son creadas por el propio objeto, sino que son **proporcionadas externamente** (inyectadas) por el contenedor.

**Sin DI (acoplamiento fuerte):**
```java
public class TributosService {
    // El servicio CREA su propia dependencia → acoplamiento fuerte
    private ContribuyenteRepository repo = new OracleContribuyenteRepository();
}
```

**Con DI (acoplamiento débil):**
```java
@Service
public class TributosService {
    private final ContribuyenteRepository repo;
    
    // Spring INYECTA la dependencia → acoplamiento débil
    @Autowired
    public TributosService(ContribuyenteRepository repo) {
        this.repo = repo;
    }
}
```

### 3.2. Ventajas de la DI

| Ventaja | Descripción |
|---------|-------------|
| **Acoplamiento débil** | Los componentes dependen de interfaces, no de implementaciones concretas |
| **Testabilidad** | Se pueden inyectar mocks o stubs en las pruebas unitarias |
| **Flexibilidad** | Cambiar la implementación (de Oracle a PostgreSQL) sin modificar el código de negocio |
| **Reutilización** | Los componentes son independientes y reutilizables |
| **Principio de responsabilidad única** | Cada clase se ocupa de su lógica, no de crear sus dependencias |

### 3.3. Tipos de inyección

#### Inyección por constructor (recomendada)

```java
@Service
public class ExpedienteService {
    private final ExpedienteRepository repo;
    private final NotificacionService notificador;
    
    @Autowired  // Opcional en Spring 4.3+ si solo hay un constructor
    public ExpedienteService(ExpedienteRepository repo, 
                              NotificacionService notificador) {
        this.repo = repo;
        this.notificador = notificador;
    }
}
```
**Ventajas:** Dependencias obligatorias, campos finales (inmutabilidad), fácil de testear.

#### Inyección por setter

```java
@Service
public class InformeService {
    private PlantillaService plantilla;
    
    @Autowired
    public void setPlantilla(PlantillaService plantilla) {
        this.plantilla = plantilla;
    }
}
```
**Uso:** Dependencias opcionales.

#### Inyección por campo (no recomendada)

```java
@Service
public class PadronService {
    @Autowired
    private PadronRepository repo;  // Difícil de testear, no inmutable
}
```
**Desventaja:** No se puede marcar como `final`, dificulta las pruebas unitarias.

### 3.4. Resolución de ambigüedades

Si existen múltiples implementaciones de la misma interfaz, Spring necesita saber cuál inyectar:

```java
public interface NotificacionService { void enviar(String mensaje); }

@Service("email")
public class EmailNotificacion implements NotificacionService { ... }

@Service("sms")
public class SmsNotificacion implements NotificacionService { ... }
```

**Soluciones:**

*   **@Qualifier:** Especifica el bean concreto por nombre.
    ```java
    @Autowired
    public TributosService(@Qualifier("email") NotificacionService notificador) { ... }
    ```

*   **@Primary:** Declara un bean como preferido por defecto.
    ```java
    @Service
    @Primary
    public class EmailNotificacion implements NotificacionService { ... }
    ```

## 4. Programación Orientada a Aspectos (AOP)

### 4.1. El problema: Preocupaciones transversales

En una aplicación típica, ciertas funcionalidades se repiten en múltiples puntos del código sin pertenecer a la lógica de negocio:

*   **Logging:** Registrar cada invocación de un servicio.
*   **Seguridad:** Verificar permisos antes de ejecutar un método.
*   **Transacciones:** Abrir/cerrar transacciones de base de datos.
*   **Auditoría:** Registrar quién, cuándo y qué operación se realizó.
*   **Caché:** Almacenar resultados frecuentes en memoria.
*   **Gestión de errores:** Capturar excepciones de forma homogénea.

Estas funcionalidades, denominadas **preocupaciones transversales (cross-cutting concerns)**, si se implementan manualmente en cada método, generan **código repetitivo y disperso** que contamina la lógica de negocio.

### 4.2. Concepto de AOP

La **Programación Orientada a Aspectos (AOP)** permite modularizar las preocupaciones transversales en componentes separados llamados **aspectos**, que se aplican automáticamente a los métodos de negocio sin modificar su código.

### 4.3. Terminología AOP

| Término | Definición | Ejemplo |
|---------|-----------|---------|
| **Aspecto (Aspect)** | Módulo que encapsula una preocupación transversal | Clase `AuditoriaAspect` que registra accesos |
| **Advice** | Acción que ejecuta el aspecto | El código de logging/auditoría |
| **Join Point** | Punto de ejecución donde se puede aplicar un aspecto | La invocación de un método |
| **Pointcut** | Expresión que selecciona los join points donde se aplica el advice | "Todos los métodos de los servicios del paquete `tributos`" |
| **Weaving** | Proceso de vincular los aspectos con el código de negocio | En tiempo de ejecución (Spring) o compilación (AspectJ) |

### 4.4. Tipos de Advice

| Tipo | Ejecución | Uso típico |
|------|-----------|-----------|
| **@Before** | Antes del método | Verificación de permisos, validación |
| **@AfterReturning** | Después del método (si termina correctamente) | Logging del resultado |
| **@AfterThrowing** | Después del método (si lanza excepción) | Gestión de errores, notificación |
| **@After** | Después del método (siempre, independientemente del resultado) | Liberación de recursos |
| **@Around** | Antes y después del método (control total) | Medición de rendimiento, caché, transacciones |

### 4.5. Ejemplo práctico: Auditoría

```java
@Aspect
@Component
public class AuditoriaAspect {

    private static final Logger log = LoggerFactory.getLogger(AuditoriaAspect.class);

    // Pointcut: todos los métodos públicos en el paquete de servicios
    @Around("execution(* es.alicante.tributos.service.*.*(..))")
    public Object auditarOperacion(ProceedingJoinPoint joinPoint) throws Throwable {
        String metodo = joinPoint.getSignature().toShortString();
        String usuario = SecurityContextHolder.getContext()
                            .getAuthentication().getName();
        
        log.info("INICIO - Usuario: {} - Método: {}", usuario, metodo);
        long inicio = System.currentTimeMillis();
        
        try {
            Object resultado = joinPoint.proceed(); // Ejecuta el método original
            long duracion = System.currentTimeMillis() - inicio;
            log.info("FIN OK - Método: {} - Duración: {}ms", metodo, duracion);
            return resultado;
        } catch (Exception e) {
            log.error("ERROR - Método: {} - Excepción: {}", metodo, e.getMessage());
            throw e;
        }
    }
}
```

Este aspecto registra automáticamente todas las operaciones de los servicios de tributos **sin modificar ni una línea del código de negocio**.

### 4.6. Expresiones Pointcut

| Expresión | Significado |
|-----------|-------------|
| `execution(* es.alicante.*.service.*.*(..))` | Todos los métodos de cualquier clase en paquetes `service` |
| `@annotation(es.alicante.Auditable)` | Métodos anotados con `@Auditable` |
| `within(es.alicante.tributos.service.*)` | Cualquier método dentro de las clases del paquete `tributos.service` |
| `@within(org.springframework.stereotype.Service)` | Cualquier método de clases anotadas con `@Service` |

### 4.7. AOP en Spring: Proxy-based

Spring implementa AOP mediante **proxies dinámicos** (JDK Dynamic Proxies o CGLIB):

```
[Llamada] → [Proxy (intercepta)] → [Advice @Before] → [Método real] → [Advice @After] → [Retorno]
```

*   **JDK Dynamic Proxy:** Se usa cuando el bean implementa una interfaz.
*   **CGLIB Proxy:** Se usa cuando el bean no implementa interfaz (crea una subclase).

**Limitación importante:** La AOP basada en proxies solo intercepta llamadas externas al bean. Las llamadas internas (un método del bean que invoca a otro método del mismo bean) no pasan por el proxy y no ejecutan los aspectos.

### 4.8. AOP en la práctica: Anotaciones Spring que usan AOP

Muchas funcionalidades de Spring se implementan internamente mediante AOP:

| Anotación | Función AOP |
|-----------|------------|
| `@Transactional` | Abre/cierra transacción automáticamente alrededor del método |
| `@Cacheable` | Almacena el resultado en caché; las siguientes llamadas devuelven el valor cacheado |
| `@Async` | Ejecuta el método en un hilo separado (asíncronamente) |
| `@Secured` / `@PreAuthorize` | Verifica permisos antes de ejecutar el método |
| `@Retryable` | Reintenta la ejecución del método en caso de excepción |

**Ejemplo de `@Transactional`:**
```java
@Service
public class LiquidacionService {

    @Transactional  // Spring AOP: abre transacción → ejecuta → commit (o rollback si hay excepción)
    public void generarLiquidacion(String dni, BigDecimal importe) {
        Contribuyente c = contribuyenteRepo.findByDni(dni);
        Liquidacion liq = new Liquidacion(c, importe, LocalDate.now());
        liquidacionRepo.save(liq);
        notificacionService.enviarNotificacion(c.getEmail(), liq);
        // Si notificacionService lanza excepción → rollback automático de todo
    }
}
```

## 5. Relación entre Contexto, DI y AOP

Los tres pilares están íntimamente relacionados:

```
ApplicationContext (Contenedor)
   ├── Gestiona los Beans (ciclo de vida, scopes)
   ├── Resuelve las Dependencias (DI: @Autowired, @Qualifier)
   └── Aplica los Aspectos (AOP: @Transactional, @Cacheable, aspectos personalizados)
```

1.  El **ApplicationContext** es el contenedor que alberga los beans.
2.  La **DI** es el mecanismo por el que el contenedor resuelve las dependencias entre beans.
3.  La **AOP** es el mecanismo por el que el contenedor envuelve los beans en proxies para aplicar funcionalidades transversales de forma transparente.

## 6. Conclusión

El Contexto de Aplicación (ApplicationContext), la Inyección de Dependencias (DI) y la Programación Orientada a Aspectos (AOP) constituyen los tres pilares fundamentales de Spring Framework. El ApplicationContext actúa como contenedor central que gestiona los beans y su ciclo de vida. La DI desacopla los componentes al externalizar la gestión de dependencias, mejorando la testabilidad y la mantenibilidad. Y la AOP modulariza las preocupaciones transversales (transacciones, seguridad, auditoría, caché) sin contaminar el código de negocio. Estos tres mecanismos trabajan conjuntamente para proporcionar la base sobre la que se construyen los servicios y aplicaciones de las Administraciones Públicas con el ecosistema Spring.
