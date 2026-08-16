# Tema 3.- Procesos de pruebas y garantía de calidad en el desarrollo de software. Niveles, técnicas y herramientas de pruebas de software. Buenas prácticas. Criterios de aceptación de software. Desarrollo orientado a test.

## 1. Introducción

La calidad del software constituye un pilar fundamental en la ingeniería informática contemporánea y, de manera especialmente crítica, en el ámbito de las Administraciones Públicas. Un error en una aplicación municipal —como un sistema de gestión tributaria o un portal de atención al ciudadano— puede acarrear consecuencias graves: desde el cálculo incorrecto de tributos hasta la exposición indebida de datos personales protegidos por el Reglamento General de Protección de Datos (RGPD) y la Ley Orgánica 3/2018 de Protección de Datos Personales y Garantía de los Derechos Digitales (LOPDGDD).

Para mitigar estos riesgos, la disciplina de la **Garantía de Calidad del Software (Software Quality Assurance - SQA)** proporciona un marco estructurado de procesos, metodologías y controles aplicados a lo largo de todo el Ciclo de Vida del Desarrollo de Software (SDLC). La SQA no se limita a detectar defectos al final del proyecto, sino que integra prácticas preventivas desde las fases más tempranas, evaluando tanto el producto resultante como la madurez del propio proceso de construcción.

El marco normativo de referencia internacional para evaluar la calidad del software es la familia de normas **ISO/IEC 25000 (SQuaRE - Software Product Quality Requirements and Evaluation)**, que estructura la calidad en ocho características fundamentales: adecuación funcional, eficiencia de rendimiento, compatibilidad, usabilidad, fiabilidad, seguridad, mantenibilidad y portabilidad. Dentro de este ecosistema de calidad, las **pruebas de software (Software Testing)** constituyen el instrumento operativo esencial, partiendo del principio ampliamente aceptado de que las pruebas pueden demostrar la presencia de defectos, pero no garantizar su ausencia absoluta.

A lo largo de este tema se analizarán los distintos niveles de pruebas, las técnicas y herramientas disponibles, las buenas prácticas del sector, los criterios de aceptación del software y la metodología de desarrollo orientado a pruebas (TDD).

## 2. Niveles de Pruebas de Software

Las pruebas de software se organizan en niveles progresivos de integración y complejidad, habitualmente representados mediante la **Pirámide de Pruebas** propuesta por Mike Cohn. Esta pirámide establece que la base debe estar formada por un gran volumen de pruebas rápidas y económicas (unitarias), reduciéndose progresivamente hacia pruebas más complejas y costosas en los niveles superiores.

### 2.1. Pruebas Unitarias (Unit Testing)

Constituyen el cimiento de la pirámide y son responsabilidad directa de los desarrolladores del software.

*   **Definición:** Verifican el correcto funcionamiento de la unidad lógica más pequeña y aislable del código fuente, típicamente una función, un método o una clase individual.
*   **Características:** Se ejecutan de forma aislada respecto al resto del sistema, empleando **Dobles de Prueba** (Test Doubles) como Mocks, Stubs y Fakes para simular las dependencias externas (bases de datos, servicios web, sistema de archivos). Su ejecución es extremadamente rápida (del orden de milisegundos), lo que permite ejecutar miles de ellas en cada compilación.
*   **Ejemplo:** Dado un método `calcularTasa(base, tipo)`, una prueba unitaria verificaría que al introducir `base=1000` y `tipo=0.05`, el resultado devuelto sea exactamente `50.0`.

### 2.2. Pruebas de Integración (Integration Testing)

Aunque dos módulos superen individualmente sus pruebas unitarias, pueden presentar incompatibilidades al interactuar entre sí debido a diferencias en los formatos de datos, protocolos de comunicación o contratos de interfaz.

*   **Definición:** Verifican la correcta comunicación e interoperabilidad entre dos o más módulos o componentes del sistema cuando se conectan entre sí.
*   **Ejemplo práctico:** Comprobar que el módulo de consulta de padrón municipal envía correctamente la petición HTTP con el formato JSON esperado al microservicio de base de datos, y que este responde adecuadamente.
*   **Estrategias de integración:**
    *   **Top-Down:** Se prueban primero los módulos de nivel superior, utilizando *Stubs* para simular los módulos inferiores aún no integrados.
    *   **Bottom-Up:** Se comienza por los módulos de nivel inferior, empleando *Drivers* (módulos conductores) para invocar los componentes bajo prueba.
    *   **Big-Bang:** Se integran todos los módulos simultáneamente y se prueban en conjunto (mayor riesgo de localización de errores).
    *   **Incremental / Mixta:** Combinación de las estrategias anteriores, integrando progresivamente los módulos.

### 2.3. Pruebas de Sistema (System Testing)

Representan la verificación del sistema completo, totalmente ensamblado y desplegado en un entorno de preproducción que replica fielmente las condiciones del entorno productivo.

*   **Definición:** Validan el comportamiento global del sistema contra los requisitos especificados, incluyendo la interacción con la infraestructura real: hardware del Centro de Procesamiento de Datos (CPD), latencia de red, sistemas operativos, bases de datos y dispositivos de almacenamiento.
*   **Subtipos relevantes:** Pruebas funcionales del sistema completo, pruebas de rendimiento, pruebas de seguridad, pruebas de compatibilidad con diferentes navegadores y dispositivos, y pruebas de recuperación ante fallos.

### 2.4. Pruebas de Aceptación (User Acceptance Testing - UAT)

Constituyen el último nivel de verificación antes del despliegue en producción y son ejecutadas por los usuarios finales del sistema, no por el equipo de desarrollo.

*   **Definición:** Determinan si el software satisface las necesidades operativas reales y los requisitos de negocio definidos por la organización. En el contexto municipal, serían los funcionarios responsables (por ejemplo, técnicos del departamento de Recaudación) quienes validarían que la aplicación cumple los flujos de trabajo administrativos reales.
*   **Modalidades:**
    *   **Pruebas Alpha:** Realizadas en el entorno controlado del equipo de desarrollo, con usuarios reales pero bajo supervisión técnica.
    *   **Pruebas Beta:** Realizadas en el entorno real del usuario, sin supervisión directa del equipo de desarrollo, lo que permite detectar problemas de usabilidad y rendimiento en condiciones reales de uso.

## 3. Técnicas y Herramientas de Pruebas de Software

Las técnicas de diseño de pruebas se clasifican según el grado de conocimiento que el probador tiene sobre la estructura interna del software.

### 3.1. Pruebas de Caja Blanca (White Box Testing)

También denominadas pruebas estructurales o pruebas de caja transparente. El tester tiene acceso completo al código fuente del sistema.

*   **Enfoque:** El diseño de los casos de prueba se basa en el análisis de la estructura interna del código: bucles (`for`, `while`), sentencias condicionales (`if-else`, `switch`), caminos de ejecución y flujo de datos.
*   **Técnicas principales:**
    *   **Cobertura de sentencias (Statement Coverage):** Garantiza que cada línea de código se ejecute al menos una vez.
    *   **Cobertura de decisiones/ramas (Branch Coverage):** Asegura que cada rama de cada punto de decisión (verdadero y falso) se evalúe al menos una vez.
    *   **Cobertura de condiciones (Condition Coverage):** Verifica que cada condición atómica dentro de una expresión compuesta se evalúe tanto a verdadero como a falso.
    *   **Cobertura de caminos (Path Coverage):** Prueba todos los caminos de ejecución posibles a través del código, siendo la métrica más exhaustiva pero potencialmente inabordable en programas complejos.

### 3.2. Pruebas de Caja Negra (Black Box Testing)

También conocidas como pruebas funcionales. El tester desconoce por completo la estructura interna del código y diseña los casos de prueba exclusivamente a partir de las especificaciones de requisitos.

*   **Enfoque:** El sistema se trata como una caja opaca. Se definen conjuntos de entradas, se ejecutan sobre el sistema y se comparan las salidas obtenidas con las salidas esperadas según la especificación de requisitos.
*   **Técnicas principales:**
    *   **Partición de equivalencia (Equivalence Partitioning):** Divide el dominio de entrada en clases de equivalencia, asumiendo que todos los valores dentro de una clase producirán el mismo comportamiento, y selecciona un representante de cada clase.
    *   **Análisis de valores límite (Boundary Value Analysis):** Concentra los casos de prueba en los valores frontera de cada clase de equivalencia, donde estadísticamente se concentran más errores.
    *   **Tablas de decisión (Decision Tables):** Representan de forma tabular las combinaciones de condiciones de entrada y las acciones esperadas del sistema, especialmente útiles para lógica de negocio compleja.
    *   **Transición de estados (State Transition Testing):** Modela el sistema como una máquina de estados y diseña pruebas que ejerciten las transiciones válidas e inválidas entre estados.

### 3.3. Pruebas de Caja Gris (Grey Box Testing)

Representan un enfoque híbrido. El tester posee un conocimiento parcial de la arquitectura interna del sistema —como el esquema de la base de datos, la estructura del API o los diagramas de arquitectura— y utiliza dicho conocimiento para diseñar pruebas más efectivas, sin llegar a analizar el código fuente línea por línea.

### 3.4. Herramientas de Pruebas

La automatización de las pruebas resulta imprescindible en los actuales modelos de integración y entrega continua (CI/CD). Las herramientas más relevantes se agrupan por categoría:

*   **Frameworks de pruebas unitarias:** JUnit y TestNG (Java), NUnit y xUnit (.NET), PyTest y unittest (Python), PHPUnit (PHP), Jest (JavaScript/TypeScript).
*   **Pruebas funcionales End-to-End (E2E):** **Selenium WebDriver** (estándar de facto para la automatización de navegadores web), Cypress, Playwright y Puppeteer, que permiten simular la interacción completa del usuario con la aplicación a través del navegador.
*   **Pruebas de API (API Testing):** Postman y SoapUI permiten diseñar, ejecutar y automatizar pruebas sobre servicios REST y SOAP, verificando respuestas HTTP, estructuras JSON/XML y tiempos de respuesta sin depender de la interfaz gráfica.
*   **Pruebas de rendimiento, carga y estrés:** Apache JMeter y Gatling permiten simular cargas concurrentes masivas (por ejemplo, miles de usuarios simultáneos accediendo a la sede electrónica municipal) para evaluar tiempos de respuesta, throughput y el comportamiento del sistema bajo condiciones extremas de demanda.
*   **Análisis de seguridad del código:**
    *   **SAST (Static Application Security Testing):** Herramientas como SonarQube y Fortify analizan el código fuente en reposo para detectar vulnerabilidades como inyecciones SQL, Cross-Site Scripting (XSS) o gestión insegura de credenciales.
    *   **DAST (Dynamic Application Security Testing):** Herramientas como OWASP ZAP analizan la aplicación en ejecución, simulando ataques reales para identificar vulnerabilidades explotables.

## 4. Buenas Prácticas en el Aseguramiento de la Calidad

### 4.1. Criterios de Aceptación (Acceptance Criteria)

Los criterios de aceptación definen las condiciones concretas, verificables e inequívocas que un incremento de software debe cumplir para ser considerado completo y apto para su despliegue. Constituyen el contrato funcional entre el equipo de desarrollo y los usuarios o responsables del producto.

En entornos ágiles, donde los requisitos se expresan mediante **Historias de Usuario**, los criterios de aceptación se redactan frecuentemente utilizando la notación **BDD (Behavior-Driven Development)** con la estructura **Given-When-Then** (Dado-Cuando-Entonces):

*   **Dado (Given - Precondición):** *Dado que* el funcionario municipal ha iniciado sesión en la aplicación de gestión tributaria con un perfil autorizado.
*   **Cuando (When - Acción):** *Cuando* selecciona la opción "Generar emisión del padrón fiscal" para el ejercicio vigente.
*   **Entonces (Then - Resultado esperado):** *Entonces* el sistema genera el fichero del padrón en formato PDF, lo almacena en el repositorio documental corporativo y envía una notificación por correo electrónico al responsable del área.

Esta estructura garantiza que cada criterio sea específico, medible y directamente verificable mediante una prueba automatizada o manual.

### 4.2. Buenas Prácticas de Calidad Continua

*   **Shift-Left Testing (Pruebas tempranas):** Consiste en desplazar las actividades de prueba hacia las fases más tempranas del ciclo de desarrollo, en lugar de concentrarlas al final. Cuanto antes se detecta un defecto, menor es su coste de corrección. Según estudios del NIST, el coste de corregir un defecto en producción puede ser entre 15 y 100 veces superior al de corregirlo en fase de diseño.
*   **Integración Continua y Entrega Continua (CI/CD):** Cada cambio en el repositorio de código desencadena automáticamente la ejecución del conjunto completo de pruebas automatizadas, proporcionando retroalimentación inmediata al desarrollador. Herramientas como Jenkins, GitLab CI/CD, GitHub Actions o Azure DevOps orquestan estos pipelines.
*   **Gestión centralizada de defectos:** Vincular cada defecto detectado con el requisito o historia de usuario de origen, utilizando herramientas de gestión como Jira, Azure Boards o Redmine, permite trazabilidad completa y análisis de tendencias.
*   **Puertas de calidad (Quality Gates):** Establecer umbrales mínimos de calidad que deben superarse para permitir el avance del código entre entornos. Por ejemplo, configurar en SonarQube que un proyecto no pueda desplegarse si presenta una cobertura de pruebas inferior al 80%, si existen vulnerabilidades de seguridad críticas, o si la densidad de defectos supera un umbral definido. Si no se cumplen estos criterios, el pipeline de despliegue se detiene automáticamente.
*   **Revisiones de código (Code Reviews):** Proceso sistemático donde uno o varios desarrolladores revisan el código escrito por un compañero antes de su integración en la rama principal, detectando errores lógicos, incumplimientos de estándares y oportunidades de mejora.
*   **Documentación y trazabilidad:** Mantener una matriz de trazabilidad que vincule cada requisito con sus casos de prueba correspondientes, garantizando que ningún requisito quede sin verificar.

## 5. Desarrollo Orientado a Pruebas (TDD - Test-Driven Development)

El **Test-Driven Development (TDD)** es una metodología de desarrollo originada en el seno de Extreme Programming (XP), formulada por Kent Beck, que invierte deliberadamente el orden tradicional de codificación. Mientras que en el enfoque convencional el desarrollador primero escribe el código de la funcionalidad y después diseña las pruebas para verificarlo, en TDD el proceso se invierte: **primero se escribe la prueba y después se implementa el código mínimo necesario para superarla**.

### 5.1. El Ciclo Red-Green-Refactor

TDD se articula mediante un ciclo iterativo de tres fases claramente diferenciadas:

1.  **Red (Fase Roja - Escribir la prueba):** El desarrollador redacta una prueba automatizada que describe el comportamiento esperado de una funcionalidad que aún no ha sido implementada. Al ejecutarla, la prueba falla inevitablemente (indicador rojo), ya que el código de producción correspondiente todavía no existe. Esta fase obliga al desarrollador a pensar primero en el diseño de la interfaz pública y en el comportamiento esperado.

2.  **Green (Fase Verde - Hacer pasar la prueba):** Se escribe el código de producción mínimo e imprescindible para que la prueba recién creada pase satisfactoriamente (indicador verde). En esta fase no se busca la elegancia ni la optimización, sino exclusivamente la corrección funcional que satisfaga la prueba.

3.  **Refactor (Refactorización - Mejorar el código):** Con la seguridad que proporciona la prueba automatizada como red de protección, el desarrollador mejora la estructura interna del código: elimina duplicidades, mejora la legibilidad, aplica patrones de diseño y optimiza el rendimiento, ejecutando las pruebas repetidamente para verificar que cada modificación no introduce regresiones.

### 5.2. Beneficios del TDD

*   **Diseño emergente:** Al escribir las pruebas primero, el código resultante tiende a presentar un diseño más limpio, modular y con menor acoplamiento.
*   **Cobertura de pruebas inherente:** Toda funcionalidad nace acompañada de su prueba correspondiente, garantizando una alta cobertura desde el inicio.
*   **Documentación ejecutable:** El conjunto de pruebas actúa como documentación viva del comportamiento esperado del sistema.
*   **Confianza para refactorizar:** La suite de pruebas permite modificar el código con la certeza de que cualquier regresión será detectada de inmediato.
*   **Reducción de defectos:** Diversos estudios empíricos demuestran que los equipos que adoptan TDD experimentan reducciones significativas en la densidad de defectos, del orden del 40% al 90% según el contexto.

### 5.3. BDD como extensión de TDD

El **Behavior-Driven Development (BDD)**, propuesto por Dan North, extiende los principios de TDD al nivel de los requisitos de negocio. Mientras TDD se centra en pruebas técnicas escritas por desarrolladores, BDD emplea un lenguaje natural estructurado (Given-When-Then) comprensible por todos los interesados del proyecto, facilitando la colaboración entre perfiles técnicos y funcionales. Herramientas como Cucumber, SpecFlow o Behave permiten ejecutar automáticamente especificaciones escritas en este formato.

## 6. Conclusión

La garantía de calidad del software en el contexto de las Administraciones Públicas no es una actividad opcional ni secundaria, sino un requisito ineludible para ofrecer servicios digitales fiables, seguros y conformes con la normativa vigente. La estratificación de las pruebas en niveles progresivos —unitarias, de integración, de sistema y de aceptación— proporciona una cobertura sistemática que permite detectar defectos en cada fase del desarrollo antes de que alcancen el entorno productivo.

La combinación de técnicas de caja blanca, caja negra y caja gris, apoyadas por herramientas modernas de automatización integradas en pipelines de CI/CD, permite mantener ciclos de entrega ágiles sin sacrificar la calidad. El establecimiento de criterios de aceptación formales mediante metodologías como BDD asegura la alineación permanente entre el software desarrollado y las expectativas funcionales de la organización.

Metodologías como TDD, con su ciclo Red-Green-Refactor, no solo mejoran la calidad intrínseca del código, sino que fomentan un diseño más robusto y una cultura de calidad desde el origen. En definitiva, la implantación rigurosa de procesos de pruebas y aseguramiento de la calidad constituye la piedra angular para construir sistemas informáticos públicos que respondan con eficacia, seguridad y escalabilidad a las necesidades crecientes de la ciudadanía en la era de la transformación digital.
