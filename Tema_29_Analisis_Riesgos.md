# Tema 29.- Análisis y gestión de riesgos. Herramientas.

## 1. Introducción

En el Anexo IV del Real Decreto 311/2022, por el que se regula el Esquema Nacional de Seguridad (ENS), se define la seguridad de los sistemas de información como la capacidad de las redes y sistemas de información de resistir, con un nivel determinado de fiabilidad, toda acción que comprometa la disponibilidad, autenticidad, integridad o confidencialidad de los datos almacenados, transmitidos o tratados, o los servicios correspondientes ofrecidos por tales redes y sistemas de información.

Las organizaciones trabajan con distintas clases de información sensibles a amenazas (ataques, errores, desastres naturales) y a vulnerabilidades propias de su uso. Proteger estos activos de información mediante un proceso sistemático de **análisis y gestión de riesgos** es esencial para que una organización pueda alcanzar sus objetivos. Este proceso no es un fin en sí mismo, sino que se integra en la actividad continua de gestión de la seguridad y constituye un principio básico del ENS (artículo 7).

El Análisis y Gestión de Riesgos es el proceso sistemático que permite a una organización identificar qué necesita proteger, de qué debe protegerlo y cuánto debe invertir en esa protección

## 2. Análisis y Gestión de Riesgos

### 2.1. Concepto

La **gestión de riesgos** es el proceso destinado a identificar, analizar, evaluar y modificar el riesgo al que están expuestos los sistemas de información. Supone buscar el equilibrio entre los riesgos a asumir y el coste que suponen las medidas de control para mitigarlos. La gestión de riesgos comprende dos tareas fundamentales: el **análisis de riesgos** y el **tratamiento de los riesgos**.

### 2.2. Definiciones clave

| Concepto | Definición | Ejemplos |
|----------|-----------|-----------|
| **Activo** | Componente o funcionalidad de un sistema de información susceptible de ser atacado con consecuencias para la organización. Los activos esenciales son la **información** que se maneja y los **servicios** que se prestan | Servidor Oracle, base de datos del padrón, certificados digitales, personal TIC |
| **Amenaza** | Causa potencial de un incidente que puede causar daños a un sistema de información. Tipos: desastres naturales, de origen industrial, errores no intencionados, ataques intencionados | Ransomware, fallo eléctrico, robo de portátil, ingeniería social |
| **Vulnerabilidad** | Toda debilidad de un activo que puede ser aprovechada por una amenaza | Software sin parchear, contraseñas débiles, falta de cifrado |
| **Riesgo** | Estimación del grado de exposición a que una amenaza se materialice sobre uno o más activos, causando daños a la organización. Se calcula como función de la **probabilidad** de ocurrencia y el **impacto** | **Riesgo = Probabilidad × Impacto** |
| **Impacto** | Medida del daño sobre el activo derivado de la materialización de una amenaza | Pérdida económica, daño reputacional, sanción legal, interrupción del servicio | - |
| **Salvaguarda (contramedida)** | Procedimiento o mecanismo tecnológico que reduce el riesgo, ya sea disminuyendo la probabilidad de que una amenaza se materialice o limitando el daño que causaría | - |
| **Riesgo residual** | Riesgo que permanece sobre el activo una vez implantadas las salvaguardas | - |

**Relación entre conceptos:** Los sistemas (activos) pueden tener vulnerabilidades que son explotables porque estamos expuestos a amenazas. La probabilidad de que esto ocurra y tenga efecto es el riesgo. El impacto es el daño que produce en la organización. Para minimizar el riesgo implantamos salvaguardas. El riesgo que queda tras aplicarlas es el riesgo residual.

### 2.3. Análisis de riesgos

El **análisis de riesgos** es el proceso sistemático para estimar la magnitud de los riesgos a que está expuesta una organización. Permite determinar qué se tiene (activos), qué podría pasar (amenazas), cuánto daño causaría (impacto) y cómo de protegido se encuentra (salvaguardas). Proporciona un modelo del sistema en términos de activos, amenazas, vulnerabilidades y salvaguardas.

Tomando como referencia **MAGERIT** (metodología creada para las Administraciones Públicas), el análisis de riesgos comprende las siguientes fases:

1. **Caracterización de los activos:**
    - Identificación de los activos: esenciales (información y servicios) y de soporte (software, hardware, soportes de información, equipamiento auxiliar, redes de comunicación, instalaciones, personas).
    
    | Tipo de activo | Ejemplos |
    |---------------|----------|
    | **Servicios** | Sede electrónica, correo electrónico, portal de tributos |
    | **Datos/Información** | Base de datos del padrón, expedientes electrónicos, datos tributarios |
    | **Aplicaciones** | ERP municipal, gestor de expedientes, aplicación de tributos |
    | **Equipamiento informático** | Servidores, puestos de trabajo, switches, firewalls |
    | **Comunicaciones** | Red LAN, enlace a Internet, VPN, Red SARA |
    | **Soportes de información** | Cintas de backup, discos USB, documentos en papel |
    | **Equipamiento auxiliar** | SAI, aire acondicionado del CPD, cableado |
    | **Instalaciones** | Centro de Proceso de Datos (CPD), oficinas |
    | **Personal** | Administradores de sistemas, operadores, usuarios |

    - Identificación de las dependencias entre activos (árboles/grafos de dependencias, donde la seguridad de los activos superiores depende de los inferiores).
    - Valoración del activo en cada dimensión de seguridad (D, I, C, A, T).

    | Nivel | Valor | Criterio |
    |-------|-------|----------|
    | 0 | Despreciable | Sin impacto apreciable |
    | 1 | Bajo | Daño menor y fácilmente reparable |
    | 2 | Medio | Daño importante pero reparable |
    | 3 | Alto | Daño grave con consecuencias significativas |
    | 4 | Muy alto | Daño muy grave o irreparable |

2. **Caracterización de las amenazas:**
    - Identificación de las amenazas: de origen natural, del entorno, defectos, accidentales, intencionadas.
    *   **Desastres naturales:** Inundación, terremoto, incendio fortuito.
    *   **Industriales:** Fallo eléctrico, fallo de climatización, avería de hardware.
    *   **Errores y fallos no intencionados:** Error de usuario, error de administración, fallo de software.
    *   **Ataques deliberados:** Malware, acceso no autorizado, denegación de servicio (DDoS), robo, ingeniería social.
    - Valoración de las amenazas en función de la **degradación** que sufriría el activo y la **probabilidad** de ocurrencia.
    Para cada par activo-amenaza se estima:
    *   **Frecuencia (probabilidad):** Con qué frecuencia puede materializarse la amenaza.
    *   **Degradación (impacto):** Qué porcentaje del valor del activo se vería afectado.

3. **Determinación del impacto:** Daño producido en el activo como consecuencia de la materialización de la amenaza.

4. **Determinación del riesgo potencial:** En función de la probabilidad de ocurrencia y del impacto. Riesgo calculado asumiendo que no existen medidas de seguridad implementadas. Corresponde al escenario hipotético de "desprotección total".

5. **Caracterización de las salvaguardas:**
    - Identificación de las salvaguardas existentes (controles que reducen la probabilidad o limitan el daño).
    - Valoración de la eficacia de las salvaguardas.

6. **Estimación del estado de riesgo:** Cálculo del **impacto residual** y del **riesgo residual** que permanecen sobre el activo una vez implantadas las salvaguardas. Riesgo que permanece después de aplicar las **salvaguardas (contramedidas)** existentes. Se calcula evaluando la eficacia de las medidas de seguridad ya implementadas.

### 2.4. Gestión de riesgos

la **gestión de riesgos** es el conjunto de actividades destinadas a modificar el estado de los riesgos identificados en el análisis, organizando las defensas hasta alcanzar un nivel de riesgo residual que la organización esté dispuesta a asumir.

**Estrategias de tratamiento:**

| Estrategia | Descripción | Ejemplo |
|------------|-------------|---------|
| **Aceptar** | Asumir el riesgo porque el coste de mitigarlo supera el daño potencial | Riesgo de caída de un servicio auxiliar no crítico |
| **Mitigar / Reducir** | Implementar nuevas salvaguardas para reducir la probabilidad o el impacto | Instalar un segundo firewall, implementar MFA, cifrar los backups |
| **Transferir** | Delegar el riesgo a un tercero | Contratar un ciberseguro, externalizar el servicio a un proveedor con SLA |
| **Evitar** | Eliminar la actividad o el activo que genera el riesgo | Desmantelar un sistema obsoleto e inseguro |

**Principio coste-beneficio:** El coste de las salvaguardas nunca debe superar el coste potencial de la materialización de las amenazas que mitigan.

**Tipos de controles:**

- **Preventivos:** Impiden que se produzcan los problemas antes de que ocurran (hardening, formación, control de acceso).
- **Detectivos:** Identifican cuándo se ha producido un error, omisión o acto indebido e informan de ello (IDS/IPS, SIEM, auditoría de logs).
- **Correctivos:** Minimizan el impacto de una amenaza materializada, remedian los problemas identificados y modifican los sistemas para evitar su repetición (planes de contingencia, restauración de backups, parcheo).

### 2.5. Análisis de riesgos en el ENS

El artículo 7 del ENS establece que el análisis y gestión de riesgos será parte esencial del proceso de seguridad y deberá mantenerse permanentemente actualizado. La medida **[op.pl.1]** del Anexo II exige un nivel de formalización creciente según la categoría del sistema:

| Categoría | Tipo de análisis | Requisitos |
|-----------|-----------------|-----------|
| **BÁSICA** | Informal (lenguaje natural) | Identificar los activos más valiosos, las amenazas más probables, las salvaguardas existentes y los principales riesgos residuales |
| **MEDIA** (Refuerzo R1) | Semiformal (tablas, valoración cualitativa) | Valorar cualitativamente los activos, cuantificar las amenazas, valorar las salvaguardas y el riesgo residual |
| **ALTA** (Refuerzo R2) | Formal (fundamento matemático reconocido) | Valorar cuantitativamente los activos, cuantificar las amenazas posibles, valorar y priorizar las salvaguardas, asumir formalmente el riesgo residual |

## 3. Herramientas

### 3.1. Marcos de referencia

#### ISO/IEC 27000 (Familia de normas de seguridad de la información)

- **ISO/IEC 27000:2018:** Descripción general y vocabulario de seguridad de la información.
- **ISO/IEC 27001:2022:** Norma principal de la serie. Establece los requisitos para implantar, mantener y mejorar un **Sistema de Gestión de Seguridad de la Información (SGSI)**. Es certificable.
- **ISO/IEC 27005:2022:** Guía específica para el análisis y gestión de riesgos de seguridad de la información. Describe las fases: establecimiento del contexto, evaluación del riesgo (identificación, análisis, valoración), tratamiento, aceptación, comunicación, monitorización y revisión.

#### ISO 31000:2018 (Gestión del riesgo)

Estándar internacional para la gestión de riesgos en cualquier organización (no limitado a TI). Cubre el análisis, tratamiento, comunicación, responsabilidades, evaluación, mantenimiento y seguimiento del sistema de gestión de riesgos. En España: UNE-ISO 31000:2018.

#### COBIT

Marco de buenas prácticas para el gobierno y la gestión de las TI. Incluye procesos específicos dedicados a la gestión del riesgo: **EDM03** (Asegurar la optimización del riesgo) y **APO12** (Gestionar el riesgo).

#### COSO ERM

Marco de referencia (versión actual COSO ERM 2017) que proporciona directrices sobre gestión del riesgo empresarial, control interno y disuasión del fraude. Orientado a la alta dirección.

### 3.2. MAGERIT v3 (Metodología de Análisis y Gestión de Riesgos de los Sistemas de Información)

MAGERIT es la metodología de referencia para las Administraciones Públicas españolas, promovida por el **CSAE** (Comisión Sectorial de Administración Electrónica). Su versión actual es MAGERIT v3.

**Utilidad:** Según MAGERIT, el análisis de riesgos es una actividad previa obligatoria para los procesos de:

- **Evaluación:** Medir el grado de confianza que inspira un sistema de información.
- **Certificación:** Asegurar responsablemente y por escrito un comportamiento (de productos o de sistemas).
- **Auditoría:** Verificar el cumplimiento normativo (por ley, por la dirección o por entidades colaboradoras).
- **Acreditación:** Legitimar al sistema para integrarse en sistemas más amplios.

**Informes resultado del análisis de riesgos con MAGERIT:**

| Informe | Contenido |
|---------|-----------|
| **Modelo de valor** | Activos, dependencias, dimensiones de valor y estimación del valor en cada dimensión |
| **Mapa de riesgos** | Amenazas significativas por activo, con frecuencia de ocurrencia y degradación |
| **Declaración de aplicabilidad** | Contramedidas consideradas apropiadas para defender el sistema |
| **Evaluación de salvaguardas** | Salvaguardas existentes calificadas según su eficacia |
| **Informe de insuficiencias** | Salvaguardas necesarias pero ausentes o insuficientemente eficaces |
| **Estado de riesgo** | Impacto y riesgo (potencial y residual) por activo y amenaza |


**PILAR** es la herramienta oficial desarrollada y mantenida por el **CCN (Centro Criptológico Nacional)** para automatizar la metodología MAGERIT. Está disponible gratuitamente para las Administraciones Públicas.

**Características:**
*   Catálogo integrado de activos, amenazas y salvaguardas alineado con el ENS.
*   Cálculo automático de riesgos potenciales y residuales.
*   Generación de informes de análisis de riesgos y declaración de aplicabilidad.
*   Soporte para las cinco dimensiones del ENS.
*   Módulo de cumplimiento del ENS que verifica la adecuación de las medidas implantadas.

### 3.3. Técnicas para la gestión de riesgos

MAGERIT v3 propone las siguientes técnicas:

**Técnicas específicas del análisis de riesgos:**

- **Tablas de estimación del impacto y el riesgo:** Representación cualitativa de impacto y probabilidad en una escala de riesgo despreciable a crítico.
- **Análisis algorítmico:** Modelos de valoración cuantitativa o cualitativa del riesgo.
- **Árboles de ataque:** Representación gráfica de las posibles vías que un atacante podría emplear para alcanzar su objetivo.

**Técnicas generales:**

- **Técnicas gráficas:** Histogramas, diagramas de Pareto, diagramas de radar para representar visualmente el estado de la seguridad y facilitar la toma de decisiones.
- **Sesiones de trabajo:** Entrevistas, reuniones y presentaciones para obtener información, comunicar resultados y fomentar la participación de los usuarios.
- **Valoración Delphi:** Técnica cualitativa basada en cuestionarios en rondas sucesivas que permite identificar problemas y desarrollar estrategias contrastando las opiniones de los participantes.

### 3.4. Herramientas EAR (Entorno de Análisis de Riesgos)

Las herramientas **EAR** soportan el análisis y la gestión de riesgos siguiendo la metodología MAGERIT y están desarrolladas y financiadas parcialmente por el **CCN**:

| Herramienta | Descripción |
|-------------|-------------|
| **PILAR** | Versión íntegra de la herramienta. Análisis completo de riesgos conforme a MAGERIT |
| **PILAR Basic** | Versión simplificada orientada a PYMEs y Administración Local |
| **μPILAR** | Versión reducida para análisis de riesgos rápidos y preliminares |
| **RMAT** | Risk Management Additional Tools. Personalización y extensión de las herramientas |

## 4. Conclusión

El análisis y la gestión de riesgos constituyen el proceso central de la seguridad de los sistemas de información, integrado como principio básico del ENS (artículo 7) y como medida obligatoria [op.pl.1] con un nivel de formalización creciente según la categoría del sistema. El proceso comprende la identificación y valoración de activos, amenazas, vulnerabilidades e impactos, seguida del tratamiento del riesgo mediante estrategias de evitación, mitigación, transferencia o aceptación, apoyadas por controles preventivos, detectivos y correctivos. MAGERIT v3, como metodología de referencia de las Administraciones Públicas españolas, proporciona un marco estructurado de análisis, complementado por marcos internacionales (ISO 27001/27005, ISO 31000, COBIT, COSO) y por las herramientas EAR del CCN (PILAR, PILAR Basic, μPILAR) que automatizan y facilitan el proceso. La gestión de riesgos no es una actividad puntual sino un proceso continuo y permanentemente actualizado, indispensable para la implantación del SGSI (ISO 27001), el cumplimiento del ENS y la determinación de medidas de seguridad en el tratamiento de datos personales (RGPD).
