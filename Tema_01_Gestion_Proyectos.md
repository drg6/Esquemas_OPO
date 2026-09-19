# Tema 1. Diseño, dirección y gestión de proyectos de tecnologías de la información. Procesos de gestión. Fases de los proyectos. Planificación de recursos materiales y humanos.

## 1. Introducción
* **Reto:** Proyectos con alta intangibilidad, obsolescencia rápida y volatilidad de requisitos.
* **El objetivo:** Equilibrar el *Triángulo de Hierro* (Alcance, Tiempo y Coste).
* **Marcos:** Estándar internacional (**PMBOK / ISO 21500**) y metodología histórica de las AAPP (**Métrica v3**).

## 2. Concepto, Diseño y Dirección
* **Proyecto TI:** Esfuerzo temporal para crear un producto o servicio único.
* **Diseño:** Definir Alcance, Estudio de Viabilidad e Interesados (*Stakeholders*).
* **Dirección vs. Gestión:**
  * *Gestión:* Planificación, cronograma, costes y control cuantitativo.
  * *Dirección:* Liderazgo, motivación, negociación y resolución de bloqueos.

## 3. Procesos de Gestión (PMBOK / ISO 21500)
* **1. Inicio:** Acta de Constitución (*Project Charter*) e identificación de interesados.
* **2. Planificación:** Plan de Proyecto y desglose de tareas en la **EDT (Estructura de Desglose del Trabajo) / WBS (Work Breakdown Structure)**.
* **3. Ejecución:** Coordinación de personas y desarrollo de entregables.
* **4. Monitoreo y Control:** Seguimiento del rendimiento y control de cambios para evitar el **Scope Creep** (corrupción del alcance).
* **5. Cierre:** Aceptación formal, lecciones aprendidas y traspaso a Operaciones.

## 4. Fases Técnicas (Ciclo de Vida / SDLC) y Métrica v3
Métrica v3 estructura el ciclo de vida en procesos normalizados que guían el desarrollo en el sector público:

* **4.0. Marco Previo:** **PSI** (Plan de Sistemas de Información) → Planificación y alineación estratégica.
* **4.1. Análisis y Viabilidad:**
  * **EVS (Estudio de Viabilidad del Sistema):** Alternativas técnicas, económicas y legales.
  * **ASI (Análisis del Sistema de Información):** Elicitación de requisitos, modelado (Casos de Uso / Historias de Usuario) y redacción de la ERS (Especificación de Requisitos Software) (*qué*, no *cómo*).
* **4.2. Diseño:** **DSI** (Diseño del Sistema de Información):
  * *Alto nivel (Arquitectura):* Hardware, software base, redes y patrones (MVC, microservicios).
  * *Bajo nivel (Detalle):* Modelado de BD (E/R), interfaces (UI/UX), algoritmos y APIs.
* **4.3. Construcción:** **CSI** (Construcción del Sistema de Información):
  * Generación de código fuente.
  * Pruebas unitarias (desarrollador / componentes aislados).
  * Pruebas de integración (interfaces y comunicación entre módulos ensamblados).
* **4.4. Implantación y Pruebas Finales:** **IAS** (Implantación y Aceptación del Sistema):
  * *Testing:* Pruebas de sistema (funcionales, carga, ENS / CCN-STIC) y pruebas UAT (validación formal del usuario).
  * *Despliegue:* Puesta en producción (*Big Bang*, paralelo, por fases), migración de datos y formación.
* **4.5. Mantenimiento:** **MSI** (Mantenimiento del Sistema de Información):
  * *Correctivo:* Corrección de errores en explotación.
  * *Evolutivo:* Nuevas funcionalidades.
  * *Perfectivo:* Optimización de rendimiento y refactorización.
  * *Preventivo:* Actualizaciones de seguridad y parches de entorno.

## 5. Planificación de Recursos
* **5.1. Recursos Humanos:**
  * Perfiles técnicos: Analistas, Desarrolladores, DBAs, DevOps y CISO (Director de seguridad de la información).
  * **Matriz RACI:** Define quién es Responsable (**R**), Aprueba (**A**), es Consultado (**C**) o Informado (**I**).
  * **Contratación Pública (LCSP - Ley 9/2017):** Redacción de Pliegos Técnicos (PPT) y control riguroso para evitar la *cesión ilegal de trabajadores*.
* **5.2. Recursos Materiales:**
  * *Entornos:* Separación estricta de Desarrollo, Preproducción y Producción.
  * *Infraestructura:* On-Premise (CPD local) vs. Cloud corporativo (con ENS nivel Alto).
  * *Herramientas:* Repositorios de código (GitLab/GitHub), gestión ágil (Jira) y automatización CI/CD.

## 6. Conclusión
El éxito de un proyecto TI en la Administración Pública exige aunar rigor técnico, metodológico y normativo. La combinación de las áreas de conocimiento del PMBOK con la estructura por fases de Métrica v3 y las garantías de la LCSP asegura que el software entregado cumpla los plazos, respete el presupuesto y proporcione un servicio seguro, interoperable y de valor al ciudadano.

----------------

# Tema 1. Diseño, dirección y gestión de proyectos de tecnologías de la información. Procesos de gestión. Fases de los proyectos. Planificación de recursos materiales y humanos.

## 1. Introducción

Disciplina critíca en AAPP con alta intangibilidad, rápida obsolencia y complejidad de procesos.
Fundamentos teóricos y prácticos de la gestión de proyectos TI.

## 2. Diseño, dirección y gestión de proyectos de tecnologías de la información

### 2.1. Concepto y características de los proyectos TI

Proyecto = Esfuerzo temporal crear producto / servicio.
Caracteristicas: Intangibilidad, Complejidad, Volatilidad de los requisitos, Incertidumbre.

### 2.2. Diseño del proyecto

Diseño: Definición del Alcance, Estudio de Viabilidad, Identificación de Interesados (Stakeholders).
Alcance = Descripcion detallada de todo el trabajo, los límites, los requisitos y los entregables necesarios para completar el proyecto con éxito.

### 2.3. Dirección y Gestión

Gestión: Planificación, Monitoreo y Control
Dirección: Liderazgo, Motivación, Negociación y Toma de decisiones.

Director de Proyecto (Project Manager) en TI: Competencias técnicas + Competencias humanas.

## 3. Procesos de gestión

La gestión de proyectos segun el PMBOK (Project Management Body of Knowledge) + norma ISO 21500: 5 categorías.

### 3.1. Grupo de Procesos de Inicio.
*   **Desarrollar el Acta de Constitución del Proyecto (Project Charter).
*   **Identificar a los Interesados.

### 3.2. Grupo de Procesos de Planificación
*   **Plan de Gestión del Proyecto:**.
*   **Definición de la Estructura de Desglose del Trabajo (EDT/WBS [Work Breakdown Structure]).** Descomposición jerárquica del trabajo

### 3.3. Grupo de Procesos de Ejecución
*   Dirigir / Gestionar proyecto.
*   Gestionar el conocimiento y la calidad.
*   Adquirir, desarrollar y dirigir al equipo.
*   Gestionar Comunicaciones y Participación de Interesados.

### 3.4. Grupo de Procesos de Monitoreo y Control
Identificar áreas donde plan requiera cambios.
*   Control integrado de cambios: Evitar "scope creep" (corrupción del alcance) -> expansión descontrolada de los requisitos o entregables de un proyecto más allá del acuerdo inicial
*   Validar y controlar el alcance.
*   Controlar Cronograma y Costes.

### 3.5. Grupo de Procesos de Cierre
*   Cierre administrativo y financiero.
*   Lecciones aprendidas.
*   Entrega del producto final a operaciones.

## 4. Fases de los Proyectos (SDLC / Métrica v3)

Ciclo de vida técnico del desarrollo del producto. Modelo clásico (Ciclo de Vida en Cascada) y el Ciclo de Vida del Desarrollo de Software (SDLC) establecen:

* **4.0. Marco Previo:** **PSI** (Plan de Sistemas de Información) → Planificación y alineación estratégica.
* **4.1. Análisis y Viabilidad:**
  * **EVS (Estudio de Viabilidad del Sistema):** Alternativas técnicas, económicas y legales.
  * **ASI (Análisis del Sistema de Información):** Elicitación de requisitos, modelado (Casos de Uso / Historias de Usuario) y redacción de la ERS (Especificación de Requisitos Software) (*qué*, no *cómo*).
* **4.2. Diseño:** **DSI** (Diseño del Sistema de Información):
  * *Alto nivel (Arquitectura):* Hardware, software base, redes y patrones (MVC, microservicios).
  * *Bajo nivel (Detalle):* Modelado de BD (E/R), interfaces (UI/UX), algoritmos y APIs.
* **4.3. Construcción:** **CSI** (Construcción del Sistema de Información):
  * Generación de código fuente.
  * Pruebas unitarias (desarrollador / componentes aislados).
  * Pruebas de integración (interfaces y comunicación entre módulos ensamblados).
* **4.4. Implantación y Pruebas Finales:** **IAS** (Implantación y Aceptación del Sistema):
  * *Testing:* Pruebas de sistema (funcionales, carga, ENS / CCN-STIC) y pruebas UAT (validación formal del usuario).
  * *Despliegue:* Puesta en producción (*Big Bang*, paralelo, por fases), migración de datos y formación.
* **4.5. Mantenimiento:** **MSI** (Mantenimiento del Sistema de Información):
  * *Correctivo:* Corrección de errores en explotación.
  * *Evolutivo:* Nuevas funcionalidades.
  * *Perfectivo:* Optimización de rendimiento y refactorización.
  * *Preventivo:* Actualizaciones de seguridad y parches de entorno.

## 5. Planificación de recursos materiales y humanos

Estimar medios para evitar retrasos y sobrecostes.

### 5.1. Planificación de Recursos Humanos

Activo más valioso.

#### 5.1.1. Identificación de Roles y Responsabilidades
*   Analistas funcionales y orgánicos.
*   Arquitectos de software.
*   Desarrolladores (Frontend, Backend, Fullstack).
*   Ingenieros de pruebas (QA).
*   Administradores de sistemas y bases de datos (DevOps/DBA).
*   Expertos en seguridad.

**Matriz de Asignación de Responsabilidades (RACI)**: **R (Responsible -> Ejectuta)** , **A (Accountable -> Aprueba):**, **C (Consulted -> Experto):**, **I (Informed -> Informado):** .

#### 5.1.2. Estimación y Adquisición
*   **Estimación del esfuerzo:** Determinar horas/hombre necesarias.
*   **Adquisición:** Personal interno o subcontratación

#### 5.1.3. Desarrollo y Dirección del Equipo
Crear un entorno colaborativo. Es fundamental planificar la formación necesaria si el equipo no domina las tecnologías elegidas y gestionar la rotación de personal, muy alta en el sector TI.

### 5.2. Planificación de Recursos Materiales

#### 5.2.1. Infraestructura Hardware
*   **Entornos:** Desarrollo, Pruebas (Pre-producción) y Producción.
*   **Equipamiento de usuario:** Ordenadores y dispositivos móviles.
*   **Redes:** Ancho de banda, firewalls
*   **Cloud:** Costes de consumo (Azure, AWS, Oracle Cloud, Google Cloud).

#### 5.2.2. Software y Licencias
*   **Software Base:** SO, BD y servidores de aplicaciones
*   **Herramientas de Desarrollo:** IDEs 
*   **Herramientas de Gestión:** Software de gestión de proyectos (Jira, MS Project), repositorios de código (GitLab, GitHub).
*   **Gestión de Licencias:**  (propietarias vs. Open Source)

#### 5.2.3. Instalaciones y Logística

## 6. Conclusión

La gestión de proyectos TI: rigor metodológico donde éxito = Técnica + Gestión procesos + Recursos humanos y materiales. 
Entorno transformación digital prioritaria -> Gestionar proyectos correctamente garantiza Inversion = Valor esperado.