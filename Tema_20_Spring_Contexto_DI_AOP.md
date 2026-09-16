# Tema 20.- Spring: Contexto, Inyección de dependencias, AOP.

## 1. Introducción: La Inversión de Control (IoC)
* **El estándar corporativo:** Spring es el framework de referencia en el desarrollo Java empresarial para las Administraciones Públicas.
* **El cambio de paradigma (IoC):** 
  * En la programación clásica, el código del desarrollador controla la creación de objetos y el flujo de ejecución mediante llamadas explícitas.
  * Con la Inversión de Control, el framework asume el control absoluto del ciclo de vida y del ensamblado de los componentes, aplicando el denominado *Principio de Hollywood*: "No nos llames, nosotros te llamaremos".
* **Los tres pilares:** El ecosistema se sustenta en el Contenedor (ApplicationContext), la Inyección de Dependencias (DI) y la Programación Orientada a Aspectos (AOP).

## 2. El Contexto de Aplicación (ApplicationContext)
* **2.1. Naturaleza y evolución del contenedor:**
  * Es la interfaz central de Spring y extiende de la primitiva `BeanFactory`. A diferencia de esta última, que utiliza carga perezosa (*lazy*), el `ApplicationContext` pre-instancia los componentes en el arranque (*eager loading*) y añade soporte para eventos, internacionalización y AOP.
  * Ha evolucionado desde la configuración en pesados ficheros XML (`ClassPathXmlApplicationContext`) hacia la configuración moderna basada en anotaciones Java (`@ComponentScan`), culminando en los ejecutables autocontenidos de Spring Boot con servidor embebido.
* **2.2. Definición semántica de Beans:**
  * Un bean es cualquier objeto gestionado por el ciclo de vida del contenedor.
  * Se definen mediante anotaciones de estereotipo: `@Component` para clases genéricas, `@Service` para lógica de negocio y `@RestController` para endpoints web.
  * **Detalle técnico de persistencia:** La anotación `@Repository` aplica automáticamente un aspecto AOP que intercepta excepciones nativas de la base de datos (como `SQLException`) y las traduce a la jerarquía de excepciones no comprobadas de Spring (`DataAccessException`), aislando el código de negocio del motor de base de datos.
* **2.3. Ámbitos (Scopes) y Ciclo de vida:**
  * **Singleton (por defecto):** Una única instancia compartida en toda la aplicación. Exige que el componente sea *stateless* (sin estado) para garantizar la concurrencia segura en entornos multihilo.
  * **Prototype:** Genera una instancia nueva cada vez que se inyecta el bean.
  * **Scopes web:** request, session y application, ligados al ciclo de vida de la petición HTTP.
  * **Secuencia de vida:** Instanciación → Inyección de dependencias → Métodos de inicialización (`@PostConstruct`) → Estado operativo → Destrucción controlada (`@PreDestroy`).
* **2.4. Gestión de entornos (`@Profile`):**
  * Permite desacoplar el código de la infraestructura, activando diferentes configuraciones (por ejemplo, base de datos local en desarrollo frente a un clúster corporativo en producción) mediante variables de entorno.

## 3. Inyección de Dependencias (DI)
* **3.1. Acoplamiento débil y testabilidad:**
  * Es la plasmación práctica del principio de Inversión de Control. Elimina la instanciación directa con el operador `new`, programando siempre contra interfaces.
  * Facilita el cumplimiento de los principios SOLID y permite una **testabilidad unitaria absoluta**, al posibilitar la inyección de dobles de prueba (*mocks*) sin necesidad de levantar el contexto completo del framework.
* **3.2. Modalidades de inyección (`@Autowired`):**
  * **Inyección por constructor (El estándar recomendado):** Permite declarar las dependencias como inmutables (`final`), hace explícitos los requisitos de la clase e impide instanciar objetos en estados inconsistentes.
  * **Inyección por setter:** Queda reservada únicamente para dependencias opcionales.
  * **Inyección por campo (*Field Injection*):** Consiste en anotar atributos privados directamente con `@Autowired`. Se considera un **antipatrón** grave porque oculta las dependencias de la clase, rompe el encapsulamiento, depende de la reflexión y dificulta la ejecución de tests unitarios aislados.
* **3.3. Resolución de ambigüedades:**
  * Si existen múltiples implementaciones para una misma interfaz, el arranque falla por colisión.
  * Se resuelve mediante `@Qualifier("nombreBean")`, indicando el identificador exacto a inyectar, o declarando una implementación como preferente mediante `@Primary`.

## 4. Programación Orientada a Aspectos (AOP)
* **4.1. Preocupaciones transversales (*Cross-Cutting Concerns*):**
  * Requisitos del sistema como la seguridad, las transacciones de base de datos, el cálculo de rendimiento y la auditoría tienden a dispersarse y duplicarse por todo el código de negocio.
  * La AOP extrae estas responsabilidades comunes hacia módulos independientes denominados **aspectos**.
* **4.2. Conceptos y tipos de Advice:**
  * **Join Point:** Punto del flujo del programa susceptible de ser interceptado (en Spring AOP siempre es la ejecución de un método).
  * **Pointcut:** Expresión que define sobre qué métodos concretos se aplicará la lógica.
  * **Advice:** La acción que se dispara. Incluye `@Before`, `@AfterReturning`, `@AfterThrowing`, `@After` y el más potente, **`@Around`**, capaz de envolver completamente la ejecución del método y decidir si continúa o modifica el resultado.
* **4.3. Implementación mediante Proxies dinámicos y la trampa del *Self-Invocation*:**
  * Spring AOP no modifica el código en compilación, sino que opera en tiempo de ejecución generando un **proxy dinámico** (mediante interfaces JDK o CGLIB para clases concretas) que envuelve al bean original.
  * **Limitación arquitectónica crítica:** Las llamadas internas dentro de una misma clase eluden el proxy. Si un método invoca internamente a otro método de la misma clase anotado con `@Transactional`, la llamada se ejecuta sobre la referencia directa (`this`) y la transacción **no se abrirá**, provocando errores silenciosos en la persistencia de datos.
  * Toda la infraestructura declarativa de Spring (`@Transactional`, `@Cacheable`, `@Async`, `@PreAuthorize`) funciona internamente apoyándose en esta arquitectura AOP.

## 5. Conclusión
El éxito y la vigencia del framework Spring en el sector público residen en la integración armónica de estos tres conceptos: el `ApplicationContext` garantiza un ciclo de vida ordenado y centralizado; la Inyección de Dependencias desacopla el diseño favoreciendo un mantenimiento ágil y código testable; y la Programación Orientada a Aspectos aísla la fontanería de seguridad y persistencia de la lógica de negocio. Este núcleo sigue siendo la base sobre la que descansan las arquitecturas modernas de microservicios y servicios web orientados a la interoperabilidad en la Administración Electrónica.

--------------------

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
