# Tema 20.- Spring: Contexto, Inyección de dependencias, AOP

## 1. Introducción

**Tres pilares arquitectónicos** de Spring: el **Contexto de Aplicación (ApplicationContext)**, la **Inyección de Dependencias (Dependency Injection — DI)** y la **Programación Orientada a Aspectos (Aspect-Oriented Programming — AOP)**.

Base ecosistema Spring -> Esencial para Spring Boot, Spring Data, Spring Security, Spring MVC.

## 2. El Contexto de Aplicación (ApplicationContext)

### 2.1. Concepto

**ApplicationContext** contenedor central de Spring. Registro que contiene todos los objetos gestionados por el framework — denominados **beans** — y gestiona su ciclo de vida, configuración y dependencias.

Cuando una aplicación Spring arranca, el contenedor:
1.  Lee la configuración .
2.  Crea instancias de todos los beans declarados.
3.  Resuelve e inyecta las dependencias entre ellos.
4.  Pone todos los beans a disposición de la aplicación.

### 2.2. Tipos de ApplicationContext

- **AnnotationConfigApplicationContext** → Anotaciones / Java Config (Spring moderno)
- **ClassPathXmlApplicationContext** → XML (legacy)
- **WebApplicationContext** → Aplicaciones web / DispatcherServlet
- **SpringApplication** → Spring Boot / Autoconfiguración / servidor embebido

### 2.3. Definición de Beans

Un **bean** es cualquier objeto cuyo ciclo de vida es gestionado por el contenedor Spring.

**Mediante anotaciones (forma moderna):**
- **@Component** → Bean genérico.
- **@Service** → Lógica de negocio.
- **@Repository** → Acceso a datos.
- **@Controller** → Controlador web (MVC).
- **@RestController** → APIs REST.

**Definir Beans mediante configuración Java**
- **@Configuration** → indica una clase de configuración.
- **@Bean** → registra un objeto como Bean en Spring.
- Permite **crear y configurar manualmente** los Beans.

### 2.4. Ámbitos (Scopes) de los Beans

- **singleton** → 1 instancia por contexto (**por defecto**).
- **prototype** → nueva instancia cada vez que se solicita.
- **request** → 1 instancia por petición HTTP.
- **session** → 1 instancia por sesión HTTP.
- **application** → 1 instancia por ServletContext.

### 2.5. Ciclo de vida de un Bean

**Instanciación → Inyección de dependencias → @PostConstruct → Uso → @PreDestroy → Destrucción**

- **@PostConstruct** → inicialización tras la inyección de dependencias.
- **@PreDestroy** → liberación de recursos antes de destruir el Bean.

### 2.6. Perfiles (Profiles)

- **¿Qué hacen?** → Permiten activar distintos Beans según el entorno.
- **@Profile("desarrollo")** → Configuración de desarrollo (ej. Base de datos H2 en memoria).
- **@Profile("produccion")** → Configuración de producción (ej. Oracle).
- **Activación** → `spring.profiles.active=produccion`

## 3. Inyección de Dependencias (Dependency Injection — DI)

### 3.1. Concepto

- **DI** → Las dependencias **no las crea el objeto**; las proporciona Spring (**proporcionadas externamente** (inyectadas))
- **Sin DI** → `new` → **acoplamiento fuerte**.
- **Con DI** → Spring proporciona la dependencia → **acoplamiento débil** (@Autowired).
- **Constructor** → Forma recomendada de inyección.

### 3.2. Ventajas de la DI

- **Acoplamiento débil** → Depender de interfaces, no de implementaciones.
- **Testabilidad** → Permite usar mocks/stubs.
- **Flexibilidad** → Cambiar implementaciones sin modificar la lógica.
- **Reutilización** → Componentes independientes y reutilizables.
- **Responsabilidad única** → La clase se centra en su lógica, no en crear dependencias.

### 3.3. Tipos de inyección

#### Inyección por constructor (recomendada)

- Dependencias **obligatorias**.
- Permite campos `final` → **inmutabilidad**.
- **Fácil de testear**.
- `@Autowired` opcional si solo hay un constructor.

#### Inyección por setter

- Dependencias **opcionales**.
- Se inyectan mediante un método `set...()`.

#### Inyección por campo (no recomendada)

- `@Autowired` directamente sobre el atributo.
- No permite `final`.
- **Más difícil de testear**.

## 4. Programación Orientada a Aspectos (AOP)

### 4.1. El problema: Preocupaciones transversales

En una aplicación típica, ciertas funcionalidades se repiten en múltiples puntos del código sin pertenecer a la lógica de negocio:

*   **Logging:** Registrar cada invocación de un servicio.
*   **Seguridad:** Verificar permisos antes de ejecutar un método.
*   **Transacciones:** Abrir/cerrar transacciones de base de datos.
*   **Auditoría:** Registrar quién, cuándo y qué operación se realizó.
*   **Gestión de errores:** Capturar excepciones de forma homogénea.

* **Problema de la implementación manual:**
    * Genera código repetitivo (boilerplate).
    * Provoca dispersión.
    * Contamina y acopla la lógica de negocio.

### 4.2. Concepto de AOP y Terminología AOP

Modulariza preocupaciones transversales en **aspectos** que se aplican sin modificar la lógica.

* **Aspecto:** Módulo transversal.
    * *Ejemplo práctico (`AuditoriaAspect`):* Anotado con `@Aspect` y `@Component`.
* **Advice:** Acción a ejecutar.
    * **`@Before`:** Antes del método (validaciones, permisos).
    * **`@AfterReturning`:** Tras éxito (logging de resultados).
    * **`@AfterThrowing`:** Si hay error (gestión de excepciones).
    * **`@After`:** Siempre, sin importar el resultado (liberar recursos).
    * **`@Around`:** Control total antes y después (rendimiento, transacciones).
      * *Ejemplo práctico:* En Auditoria `@Around("execution(...)")` para capturar usuario e ejecutar metodo original con `joinPoint.proceed()`.
* **Join Point:** Punto de ejecución (método).
* **Pointcut:** Filtro/expresión de selección (ej. selección de paquetes de servicio).
* **Weaving:** Vinculación aspecto-código.

### 4.3. Implementación (Proxies dinámicos)

* **Tipos:** **JDK** (interfaces) o **CGLIB** (subclases).
* **Flujo:** Intercepta llamadas externas `[Proxy → Advice → Método → Advice]`.
* **Limitación crítica:** Las **llamadas internas** (en la misma clase) eluden el proxy = **El aspecto NO se ejecuta**.
* **Uso nativo:** `@Transactional`, `@Cacheable`, `@Async`, `@PreAuthorize`.

## 5. Conclusión

El ecosistema de Spring 3 componentes -> El **ApplicationContext** centraliza la gestión del contenedor y de los beans; la **Inyección de Dependencias** desacopla el diseño estructural de las clases; y la **AOP** aplica comportamientos transversales de forma transparente mediante proxies. 

Su dominio conjunto resulta indispensable para construir arquitecturas robustas, mantenibles y escalables en entornos profesionales y de la administración pública.
