# Tema 27.- Esquema Nacional de Seguridad. Normas CCN-STIC. Aspectos básicos de la Estrategia de Ciberseguridad Nacional y principales entidades actoras en relación con el sector público y privado.

## 1. Introducción

La ciberseguridad en las Administraciones Públicas no puede depender de decisiones ad-hoc de cada organismo. Un Ayuntamiento que elige sus medidas de seguridad sin un marco común genera "silos de seguridad" que impiden la interoperabilidad segura con otras administraciones y dejan brechas que los ciberatacantes pueden explotar.

El **Esquema Nacional de Seguridad (ENS)** establece el marco normativo unificado y obligatorio para la protección de los sistemas de información del sector público. Las **Guías CCN-STIC** desarrollan técnicamente sus requisitos. Y la **Estrategia de Ciberseguridad Nacional** coordina los esfuerzos de todas las entidades públicas y privadas implicadas en la defensa del ciberespacio español.

## 2. El Esquema Nacional de Seguridad (ENS)

### 2.1. Marco legal

*   **Origen:** Establecido por la Ley 40/2015 (LRJSP), artículo 156.2.
*   **Regulación:** Real Decreto 311/2022, de 3 de mayo, que deroga el anterior RD 3/2010. Esta versión actualizada alinea el ENS con el Marco Europeo de Ciberseguridad y las directivas NIS/NIS2.

### 2.2. Ámbito de aplicación

El ENS es de obligado cumplimiento para todo el sector público:
*   Administración General del Estado (AGE).
*   Administraciones de las Comunidades Autónomas.
*   Entidades que integran la Administración Local (Ayuntamientos, Diputaciones, Cabildos).
*   Organismos públicos y entidades de derecho público vinculados.
*   Entidades del sector privado que presten servicios o provean soluciones tecnológicas a las Administraciones Públicas (en lo que afecte a dichos servicios).

### 2.3. Principios básicos del ENS

El ENS se fundamenta en los siguientes principios:

1.  **Seguridad como proceso integral:** La seguridad no es un producto (un firewall) ni un proyecto puntual, sino un proceso continuo que abarca aspectos técnicos, humanos, materiales y organizativos.

2.  **Gestión de la seguridad basada en los riesgos:** Las medidas de seguridad deben ser proporcionales a los riesgos identificados. Requiere un análisis de riesgos formal (MAGERIT — Tema 29).

3.  **Prevención, detección, respuesta y conservación:**
    *   **Prevención:** Reducir la superficie de ataque (hardening, parcheado, formación).
    *   **Detección:** Monitorización continua para identificar incidentes (SIEM, IDS/IPS).
    *   **Respuesta:** Plan de respuesta a incidentes para contener y erradicar amenazas.
    *   **Conservación:** Copias de seguridad y planes de continuidad para garantizar la recuperación.

4.  **Existencia de líneas de defensa (Defensa en profundidad):** Múltiples capas de seguridad independientes: firewall perimetral, segmentación de red, cifrado de datos, autenticación multifactor, monitorización.

5.  **Vigilancia continua y reevaluación periódica:** La seguridad debe monitorizarse continuamente y reevaluarse ante cambios en el entorno, las amenazas o los sistemas.

6.  **Diferenciación de responsabilidades:** Separación clara entre quien determina los requisitos de seguridad (Responsable de la Seguridad) y quien opera los sistemas (Responsable del Sistema).

### 2.4. Dimensiones de seguridad

El ENS define cinco dimensiones de seguridad que deben evaluarse para cada sistema de información:

| Dimensión | Descripción |
|-----------|-------------|
| **Disponibilidad [D]** | Garantía de acceso al sistema cuando se necesita |
| **Integridad [I]** | Los datos no han sido alterados de forma no autorizada |
| **Confidencialidad [C]** | Solo acceden a la información quienes están autorizados |
| **Autenticidad [A]** | La identidad del emisor de la información es verificable |
| **Trazabilidad [T]** | Las acciones realizadas pueden rastrearse hasta su autor |

### 2.5. Categorización de los sistemas

Cada sistema de información se categoriza en uno de tres niveles según el impacto que tendría una brecha en cualquiera de las cinco dimensiones:

| Categoría | Impacto potencial | Auditoría requerida |
|-----------|-------------------|---------------------|
| **BÁSICA** | Perjuicio limitado | Autoevaluación |
| **MEDIA** | Perjuicio grave | Auditoría externa cada 2 años |
| **ALTA** | Perjuicio muy grave o irreparable | Auditoría externa cada 2 años |

La categoría del sistema es la **más alta** de las categorías asignadas a cada dimensión. Si la Disponibilidad es MEDIA y la Confidencialidad es ALTA, el sistema se categoriza como ALTA.

### 2.6. Medidas de seguridad del ENS

El Anexo II del RD 311/2022 establece 73 medidas de seguridad organizadas en tres marcos:

*   **Marco Organizativo [org]:** Política de seguridad, normativa de seguridad, procedimientos de seguridad, proceso de autorización.
*   **Marco Operacional [op]:** Planificación (análisis de riesgos, arquitectura de seguridad), control de acceso, explotación, servicios externos, continuidad del servicio, monitorización del sistema.
*   **Marco de Protección [mp]:** Protección de las instalaciones, gestión del personal, protección de los equipos, protección de las comunicaciones, protección de los soportes de información, protección de las aplicaciones, protección de la información, protección de los servicios.

Cada medida se aplica a un nivel u otro de categoría (Básica, Media, Alta), con requisitos crecientes.

## 3. Guías CCN-STIC

### 3.1. Concepto

Las **Guías CCN-STIC (Seguridad de las Tecnologías de la Información y las Comunicaciones)** son documentos técnicos elaborados por el **Centro Criptológico Nacional (CCN)** —adscrito al Centro Nacional de Inteligencia (CNI)— que desarrollan los requisitos del ENS con instrucciones técnicas concretas y detalladas.

### 3.2. Series principales

| Serie | Ámbito |
|-------|--------|
| **Serie 000** | Políticas generales de seguridad TIC |
| **Serie 100** | Procedimientos de acreditación y certificación |
| **Serie 200** | Normas del ENS (categorización, declaración de conformidad) |
| **Serie 300** | Instrucciones técnicas de cumplimiento |
| **Serie 400** | Guías generales de seguridad (ENS perfil de cumplimiento) |
| **Serie 500** | Entornos Windows (configuración segura, hardening) |
| **Serie 600** | Otros entornos (Linux, dispositivos móviles, cloud) |
| **Serie 800** | Esquema Nacional de Seguridad (guías de implantación, auditoría, métricas) |
| **Serie 1000** | Procedimientos de seguridad específicos |

### 3.3. Guías más relevantes

*   **CCN-STIC 801:** Responsabilidades y funciones en el ENS.
*   **CCN-STIC 803:** Valoración de los sistemas en el ENS.
*   **CCN-STIC 804:** Medidas de implantación del ENS.
*   **CCN-STIC 808:** Verificación del cumplimiento del ENS.
*   **CCN-STIC 811:** Interconexión de la Red SARA.
*   **CCN-STIC 817:** Gestión de ciberincidentes.

## 4. Estrategia de Ciberseguridad Nacional

### 4.1. Concepto

La **Estrategia de Seguridad Nacional** (revisada en 2021) y, dentro de ella, la **Estrategia Nacional de Ciberseguridad** (2019), definen las líneas de acción y los objetivos del Estado en materia de defensa del ciberespacio. Establecen un modelo de gobernanza que coordina la actuación de múltiples organismos públicos y privados.

### 4.2. Estructura de gobernanza

*   **Consejo de Seguridad Nacional (CSN):** Órgano de asistencia al Presidente del Gobierno en materia de seguridad nacional. Coordina la respuesta ante crisis que afecten al ciberespacio.
*   **Departamento de Seguridad Nacional (DSN):** Órgano de apoyo técnico al CSN. Coordina la gestión de situaciones de crisis de ciberseguridad.
*   **Consejo Nacional de Ciberseguridad:** Órgano colegiado que apoya al CSN en materia específica de ciberseguridad.

## 5. Principales Entidades Actoras

### 5.1. Sector Público

| Entidad | Ámbito | Dependencia |
|---------|--------|------------|
| **CCN-CERT** | Sector público (Administraciones Públicas) | Centro Criptológico Nacional (Ministerio de Defensa / CNI) |
| **Mando Conjunto del Ciberespacio (MCCE)** | Defensa nacional (ciberdefensa) | Ministerio de Defensa |
| **CNPIC** | Infraestructuras críticas | Ministerio del Interior |
| **AEPD** | Protección de datos personales | Autoridad independiente |

*   **CCN-CERT:** Responsable de la gestión de ciberincidentes en el sector público. Proporciona herramientas (LUCÍA, ANA, CARMEN, PILAR, microCLAUDIA), guías técnicas (CCN-STIC), alertas de vulnerabilidades y soporte de respuesta ante incidentes graves.

### 5.2. Sector Privado y Ciudadanía

| Entidad | Ámbito | Dependencia |
|---------|--------|------------|
| **INCIBE** | Empresas, ciudadanos, academia | Ministerio de Transformación Digital |
| **INCIBE-CERT** | Gestión de incidentes del sector privado e infraestructuras críticas | INCIBE |

*   **INCIBE (Instituto Nacional de Ciberseguridad):** Punto de referencia en ciberseguridad para el sector privado y la ciudadanía. Gestiona el teléfono de atención 017, campañas de concienciación, y INCIBE-CERT para la respuesta ante incidentes en empresas e infraestructuras críticas.

### 5.3. Cooperación público-privada

La Estrategia Nacional de Ciberseguridad promueve la colaboración entre el sector público y el privado mediante:
*   Intercambio de información sobre amenazas.
*   Ejercicios conjuntos de ciberseguridad.
*   Plataformas de coordinación (Foro Nacional de Ciberseguridad).

## 6. Certificación de Conformidad con el ENS

El proceso de certificación verifica la adecuación de los sistemas al ENS:
*   **Sistemas de categoría BÁSICA:** Autoevaluación mediante la herramienta INES (Informe Nacional del Estado de Seguridad) del CCN.
*   **Sistemas de categoría MEDIA y ALTA:** Auditoría externa realizada por una entidad acreditada por ENAC (Entidad Nacional de Acreditación), con periodicidad mínima bienal.
*   La certificación se inscribe en el **Registro de Conformidad del ENS** del CCN.

## 7. Conclusión

El Esquema Nacional de Seguridad constituye el pilar normativo de la ciberseguridad en el sector público español, estableciendo un marco unificado de principios, dimensiones de seguridad, categorización de sistemas y medidas de protección que las Administraciones Públicas deben cumplir obligatoriamente. Las Guías CCN-STIC del Centro Criptológico Nacional proporcionan la concreción técnica necesaria para la implantación de estos requisitos.

La Estrategia Nacional de Ciberseguridad coordina la actuación de los principales actores (CCN-CERT para el sector público, INCIBE-CERT para el sector privado, MCCE para la ciberdefensa) en un ecosistema integrado de prevención, detección y respuesta ante ciberamenazas que protege los sistemas de información de las Administraciones Públicas y, en última instancia, los datos y servicios de los ciudadanos.
