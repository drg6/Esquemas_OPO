# Tema 3.- Procesos de pruebas y garantía de calidad en el desarrollo de software. Niveles, técnicas y herramientas de pruebas de software. Buenas prácticas. Criterios de aceptación de software. Desarrollo orientado a test.

## 1. Introducción
* **Relevancia en AAPP:** El software público (tributos, padrón, sedes) maneja derechos y datos críticos protegidos por el **RGPD** y la **LOPDGDD**.
* **Concepto de SQA (Software Quality Assurance):** Marco preventivo integral durante todo el SDLC; evalúa el proceso y el producto, no solo defectos al final.
* **Norma ISO/IEC 25000 (SQuaRE):** Estándar de calidad del producto software (8 dimensiones: adecuación funcional, rendimiento, seguridad, mantenibilidad, etc.).
* **Axioma del Testing:** Las pruebas demuestran la presencia de errores, nunca su ausencia absoluta.

## 2. Niveles de Pruebas (Pirámide de Cohn)
Estructura en pirámide: base ancha de pruebas rápidas/económicas que se reduce hacia la cúspide en pruebas complejas y costosas.

* **2.1. Pruebas Unitarias:**
  * Validan la unidad mínima de código (método/clase) de forma aislada.
  * Uso de **Dobles de Prueba (Test Doubles):** Mocks, Stubs y Fakes para aislar dependencias externas (BD, APIs).
* **2.2. Pruebas de Integración:**
  * Verifican la comunicación, contratos y flujos de datos entre módulos o microservicios.
  * *Estrategias:* **Top-Down** (módulos superiores con *Stubs*), **Bottom-Up** (módulos inferiores con *Drivers*), **Big-Bang** (todo a la vez, alto riesgo) e **Incremental**.
* **2.3. Pruebas de Sistema:**
  * Validan el sistema completo contra especificaciones en entornos preproductivos idénticos a producción.
  * Incluyen rendimiento, carga, estrés, alta disponibilidad y seguridad física/lógica.
* **2.4. Pruebas de Aceptación (UAT):**
  * Validación funcional por los usuarios finales (técnicos y funcionarios de negocio) antes del pase a producción.
  * *Modalidades:* **Alpha** (entorno de desarrollo controlado) y **Beta** (entorno real del usuario sin tutela técnica).

## 3. Técnicas y Herramientas de Pruebas
* **3.1. Pruebas de Caja Blanca (Estructurales):**
  * Acceso total al código fuente.
  * *Técnicas de cobertura:* Cobertura de sentencias (líneas), de decisiones/ramas (if/else), de condiciones y de caminos (*Path Coverage*).
* **3.2. Pruebas de Caja Negra (Funcionales):**
  * Desconocimiento del código interno; foco en entradas y salidas esperadas.
  * *Técnicas:* Partición de equivalencia, **Análisis de Valores Límite** (fronteras), tablas de decisión y diagramas de transición de estados.
* **3.3. Pruebas de Caja Gris:** Enfoque híbrido (conocimiento de esquema de BD o contratos OpenAPI sin auditar código línea a línea).
* **3.4. Ecosistema de Herramientas:**
  * *Unitarias:* JUnit (Java), PyTest (Python), NUnit (.NET), Jest (JS/TS).
  * *End-to-End (E2E) y Navegador:* Selenium WebDriver, Playwright, Cypress.
  * *APIs:* Postman, SoapUI (validación REST/SOAP y contratos JSON/XML).
  * *Carga y Rendimiento:* Apache JMeter, Gatling (concurrencia masiva en sedes electrónicas).
  * *Seguridad:* **SAST** en reposo (SonarQube) y **DAST** en ejecución (OWASP ZAP).

## 4. Buenas Prácticas y Criterios de Aceptación
* **4.1. Criterios de Aceptación y BDD:**
  * Contrato formal de entrega redactado en lenguaje natural estructurado.
  * Estructura **Given-When-Then (Dado-Cuando-Entonces):** Precondición, Acción disparadora y Resultado esperado (ejecutable con herramientas como Cucumber).
* **4.2. Principios de Calidad Continua:**
  * **Shift-Left Testing:** Adelantar las pruebas al inicio del ciclo de vida; abarata drásticamente el coste de corrección (según el NIST, corregir en producción cuesta hasta 100 veces más).
  * **Integración Continua (CI/CD):** Ejecución automática de suites de test en cada `git push` (GitLab CI, GitHub Actions, Jenkins).
  * **Quality Gates (Puertas de Calidad):** Umbrales mínimos obligatorios en pipelines (ej. mínimo 80% de cobertura y cero vulnerabilidades críticas en SonarQube para permitir el despliegue).
  * *Revisiones de Código (Code Reviews):* Auditoría entre pares obligatoria antes del merge a la rama principal.

## 5. Desarrollo Orientado a Pruebas (TDD)
Metodología de Extreme Programming (Kent Beck) que invierte el orden tradicional: la prueba se diseña antes de implementar la funcionalidad.

* **5.1. Ciclo Red-Green-Refactor:**
  * **1. Red (Rojo):** Se escribe una prueba automatizada del requisito; la prueba falla obligatoriamente porque el código no existe.
  * **2. Green (Verde):** Se escribe el código mínimo necesario para que la prueba pase satisfactoriamente.
  * **3. Refactor (Refactorizar):** Se limpia y optimiza el diseño del código sin alterar su comportamiento, respaldado por la suite de pruebas verde.
* **5.2. Ventajas:** Diseño modular emergente con bajo acoplamiento, cobertura nativa cercana al 100%, documentación viva ejecutable y reducción drástica de la deuda técnica.

## 6. Conclusión
La garantía de calidad en el software público es un imperativo legal y funcional amparado por la norma ISO 25000 y el ENS. La estructuración piramidal de las pruebas, unida a la automatización mediante Quality Gates en pipelines CI/CD y a disciplinas preventivas como Shift-Left y TDD, transforma el aseguramiento de la calidad de un cuello de botella burocrático a una salvaguarda esencial para desplegar servicios públicos digitales seguros, robustos y confiables.

--------------------------

# Tema 3.- Procesos de pruebas y garantía de calidad en el desarrollo de software. Niveles, técnicas y herramientas de pruebas de software. Buenas prácticas. Criterios de aceptación de software. Desarrollo orientado a test.

## 1. Introducción

La calidad del software = pilar fundamental
Error aplicación municipal —> cálculo incorrecto de tributos o exposición indebida de datos personales (RGPD) y (LOPDGDD).

**Garantía de Calidad del Software (Software Quality Assurance - SQA)** -> marco estructurado de procesos, metodologías y controles en Ciclo de Vida del Desarrollo de Software (SDLC). Prácticas preventivas para prevenir defectos.

**ISO/IEC 25000 (SQuaRE - Software Product Quality Requirements and Evaluation)**, 8 características (ESPUMA CF): 
*   Adecuación funcional
*   Eficiencia de rendimiento
*   Compatibilidad
*   Usabilidad
*   Fiabilidad
*   Seguridad
*   Mantenibilidad
*   Portabilidad. 

**Pruebas de software (Software Testing)** esencial encontrar defectos, no garantizar su ausencia absoluta.

## 2. Niveles de Pruebas de Software

Las pruebas de software -> niveles progresivos de integración y complejidad **Pirámide de Pruebas** Mike Cohn. 
*   Base -> pruebas rápidas y económicas (unitarias)
*   Niveles superiores -> pruebas más complejas y costosas.

### 2.1. Pruebas Unitarias (Unit Testing)

Cimiento de la pirámide -> responsabilidad desarrolladores del software.

*   **Definición:** Verificar unidad más pequeña y aislable del código (función, método o clase)
*   **Características:** Ejecución aislada,  **Dobles de Prueba** (Test Doubles) como Mocks, Stubs y Fakes -> simular dependencias externas (BD, servicios web, sistema de archivos). Rápidas.

### 2.2. Pruebas de Integración (Integration Testing)

*   **Definición:** Verifican comunicación de dos o más módulos.
*   **Estrategias de integración:**
    *   **Top-Down:** Se prueban primero los módulos de nivel superior *Stubs*.
    *   **Bottom-Up:** Se comienza por los módulos de nivel inferior, *Drivers*.
    *   **Big-Bang:** Se integran todos los módulos simultáneamente y se prueban en conjunto.
    *   **Incremental / Mixta:** Combinación estrategias anteriores, integrando progresivamente los módulos.

### 2.3. Pruebas de Sistema (System Testing)

Representan la verificación del sistema completo, totalmente ensamblado y desplegado en un entorno de preproducción que replica fielmente las condiciones del entorno productivo.

*   **Definición:** Verificación del sistema completo, desplegado entorno de preproducción.
*   **Subtipos relevantes:** Pruebas funcionales del sistema completo, pruebas de rendimiento, pruebas de seguridad, pruebas de compatibilidad con diferentes navegadores y dispositivos, y pruebas de recuperación ante fallos.

### 2.4. Pruebas de Aceptación (User Acceptance Testing - UAT)

Último nivel de verificación antes despliegue producción, ejecutadas por usuarios finales.

*   **Modalidades:**
    *   **Pruebas Alpha:** Usuarios reales pero bajo supervisión técnica.
    *   **Pruebas Beta:** Usuarios reales sin supervisión directa del equipo de desarrollo.

## 3. Técnicas y Herramientas de Pruebas de Software

Según conocimiento interno del software.

### 3.1. Pruebas de Caja Blanca (White Box Testing)

Tester con acceso completo al código.

*   **Enfoque:** Análisis estructura interna del código.
*   **Técnicas principales:**
    *   **Cobertura de sentencias (Statement Coverage):** Cada línea de código se ejecute al menos una vez.
    *   **Cobertura de decisiones/ramas (Branch Coverage):** Cada punto de decisión se evalúe al menos una vez.
    *   **Cobertura de condiciones (Condition Coverage):** Condición se evalúe tanto a verdadero como a falso.
    *   **Cobertura de caminos (Path Coverage):** Prueba todos los caminos de ejecución posibles a través del código.

### 3.2. Pruebas de Caja Negra (Black Box Testing)

Tester desconoce código -> pruebas en base a requisitos.

*   **Enfoque:** Definir entrada, Ejecutar y Comparar salida.
*   **Técnicas principales:**
    *   **Partición de equivalencia (Equivalence Partitioning):** Selecciona un representante x clase.
    *   **Análisis de valores límite (Boundary Value Analysis):** Estadísticamente concentran más errores.
    *   **Tablas de decisión (Decision Tables):** Lógica de negocio compleja.
    *   **Transición de estados (State Transition Testing):** Transiciones válidas e inválidas entre estados.

### 3.3. Pruebas de Caja Gris (Grey Box Testing)

Enfoque híbrido. Tester conocimiento parcial del sistema -> pruebas más efectivas.

### 3.4. Herramientas de Pruebas

Imprescindible para integración y entrega continua (CI/CD). 

*   **Frameworks de pruebas unitarias:** JUnit y TestNG (Java), NUnit y xUnit (.NET), PyTest y unittest (Python), PHPUnit (PHP), Jest (JavaScript/TypeScript).
*   **Pruebas funcionales End-to-End (E2E):** **Selenium WebDriver** (estándar de facto para la automatización de navegadores web), Cypress, Playwright y Puppeteer.
*   **Pruebas de API (API Testing):** Postman y SoapUI en servicios REST y SOAP.
*   **Pruebas de rendimiento, carga y estrés:** Apache JMeter y Gatling.
*   **Análisis de seguridad del código:**
    *   **SAST (Static Application Security Testing):** SonarQube y Fortify detectar vulnerabilidades como inyecciones SQL, Cross-Site Scripting (XSS) o gestión insegura de credenciales.
    *   **DAST (Dynamic Application Security Testing):** OWASP ZAP analizan la aplicación en ejecución, simulando ataques reales.

## 4. Buenas Prácticas en el Aseguramiento de la Calidad

### 4.1. Criterios de Aceptación (Acceptance Criteria)

Los criterios de aceptación -> condiciones de un incremento de software debe cumplir para ser considerado completo y apto para despliegue. Contrato entre equipo desarrollo y usuarios.

**Historias de Usuario**, los criterios de aceptación mediante **BDD (Behavior-Driven Development)** con estructura **Given-When-Then** (Dado-Cuando-Entonces) -> Criterio sea específico, medible y verificable mediante prueba.

### 4.2. Buenas Prácticas de Calidad Continua

*   **Shift-Left Testing (Pruebas tempranas):** Antes se detecta un defecto, menor es su coste de corrección. 
*   **Integración Continua y Entrega Continua (CI/CD):** Cambio ejecuta conjunto de pruebas (pipelines). Jenkins, GitLab CI/CD, GitHub Actions o Azure DevOps.
*   **Gestión centralizada de defectos:** Vincular defecto con requisito mediante Jira, Azure Boards o Redmine.
*   **Puertas de calidad (Quality Gates):** Establecer umbrales mínimos de calidad. No despliegue si pruebas < 80% o vulnerabilidades de seguridad críticas, o umbral defectos.
*   **Revisiones de código (Code Reviews):** Revisión de código por otros desarrolladores.
*   **Documentación y trazabilidad:** Objetivo que ningún requisito quede sin verificar.

## 5. Desarrollo Orientado a Pruebas (TDD - Test-Driven Development)

**Test-Driven Development (TDD)** metodología de desarrollo originada en Extreme Programming (XP), Kent Beck, que invierte el orden de codificación. **primero se escribe prueba y después código necesario para superarla**.

### 5.1. El Ciclo Red-Green-Refactor

3 fases:

1.  **Red (Fase Roja - Escribir la prueba):** Creación prueba basada en funcionalidad. Al ejecutar, la prueba falla (indicador rojo), código de producción todavía no existe. 

2.  **Green (Fase Verde - Hacer pasar la prueba):** Escribir código mínimo para pasar prueba (indicador verde). 

3.  **Refactor (Refactorización - Mejorar el código):** Elimina duplicidades, mejora la legibilidad, aplica patrones de diseño y optimiza el rendimiento.

### 5.2. Beneficios del TDD

*   **Diseño emergente:** Pruebas primero, el código diseño más limpio, modular y con menor acoplamiento.
*   **Cobertura de pruebas inherente:** Funcionalidad con prueba correspondiente.
*   **Documentación ejecutable:** Pruebas como documentación del comportamiento esperado.
*   **Confianza para refactorizar:** Regresión será detectada de inmediato.
*   **Reducción de defectos:** Del 40% al 90%.

### 5.3. BDD como extensión de TDD

El **Behavior-Driven Development (BDD)**, Dan North, emplea un lenguaje natural estructurado (Given-When-Then). Fcilita colaboración entre perfiles técnicos y funcionales. Cucumber, SpecFlow o Behave.

## 6. Conclusión

Garantizar la calidad del software no es opcional: es obligatorio para ofrecer servicios digitales seguros y legales. 4 niveles progresivos (unitarias, de integración, de sistema y de aceptación)

Combinando pruebas de caja blanca, negra y gris + *pipelines* de CI/CD -> software rápido sin perder calidad. 

BDD (desarrollo guiado por comportamiento) asegura que programa cumpla necesidad.
TDD (desarrollo guiado por pruebas y su ciclo *Red-Green-Refactor*) mejoran el diseño. 

Pruebas -> crear sistemas públicos seguros, eficaces y preparados para la ciudadanía.