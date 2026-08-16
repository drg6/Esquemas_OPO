# Tema 30.- Auditorías de conformidad con el Esquema Nacional de Seguridad (ENS).

## 1. Introducción

El Esquema Nacional de Seguridad (ENS, RD 311/2022) no se limita a definir las medidas de seguridad que deben implementar las Administraciones Públicas: exige además un mecanismo de **verificación independiente** que acredite que esas medidas se cumplen efectivamente. La auditoría de conformidad con el ENS es el proceso formal, reglado y periódico mediante el cual se evalúa si los sistemas de información de una Administración Pública satisfacen los requisitos de seguridad establecidos por el ENS.

La auditoría constituye la fase **"Check" (Verificación)** del ciclo PDCA (Plan-Do-Check-Act) de mejora continua del SGSI, analizado en el Tema 24. Sin esta fase de verificación, la implantación de medidas de seguridad carecería de garantía objetiva y la Declaración de Aplicabilidad sería un documento sin validación.

## 2. Fundamento Legal y Obligatoriedad

### 2.1. Base normativa

El artículo 34 del RD 311/2022 establece la obligación de auditar los sistemas de información de las Administraciones Públicas para verificar el cumplimiento del ENS.

### 2.2. Periodicidad según la categoría del sistema

| Categoría | Tipo de verificación | Periodicidad |
|-----------|---------------------|-------------|
| **BÁSICA** | **Autoevaluación** mediante la herramienta INES del CCN | Al menos cada 2 años |
| **MEDIA** | **Auditoría formal** (interna o externa) | Al menos cada 2 años |
| **ALTA** | **Auditoría formal** (interna o externa) | Al menos cada 2 años |

### 2.3. Auditorías extraordinarias

Además de las auditorías ordinarias bienales, debe realizarse una auditoría **extraordinaria** cuando se produzcan cambios sustanciales en el sistema de información que puedan afectar significativamente a la seguridad:
*   Migración a infraestructura cloud.
*   Reestructuración significativa de la red.
*   Implantación de un nuevo sistema de información de categoría Media o Alta.
*   Incidente de seguridad grave que evidencie carencias.

## 3. Tipos de Auditoría

### 3.1. Autoevaluación (categoría BÁSICA)

Para sistemas de categoría Básica, el ENS no exige una auditoría formal externa, sino una **autoevaluación** realizada por el propio personal de la organización. Se utiliza la herramienta **INES (Informe Nacional del Estado de Seguridad)** del CCN, que guía el proceso mediante un cuestionario estructurado que cubre todas las medidas del ENS aplicables.

El resultado es una **Declaración de Conformidad** que la organización publica en su Sede Electrónica.

### 3.2. Auditoría interna

Realizada por personal de la propia organización o por un servicio interno de auditoría. Requisitos:
*   El auditor debe ser **independiente** del sistema auditado: el administrador del sistema no puede auditar sus propios sistemas (principio de segregación de funciones).
*   El auditor debe tener competencias acreditadas en materia de seguridad de la información.
*   Los resultados tienen validez para verificación interna pero no para la obtención del **sello de certificación de conformidad** con el ENS.

### 3.3. Auditoría externa (de certificación)

Es la única que permite obtener la **Certificación de Conformidad con el ENS**, registrada oficialmente en el CCN. Requisitos:

*   Debe ser realizada por una **entidad de certificación independiente** acreditada por **ENAC (Entidad Nacional de Acreditación)**.
*   Los auditores deben estar cualificados conforme a los esquemas de certificación de auditores del ENS (esquema CCN).
*   La entidad certificadora no puede haber participado en la implantación de las medidas auditadas (independencia).
*   Entidades certificadoras habituales: AENOR, BSI, Bureau Veritas, TÜV, DNV.

## 4. Fases del Proceso de Auditoría

### 4.1. Fase 1: Planificación

1.  **Definición del alcance (Scope):** Determinación precisa de qué sistemas de información se incluyen en la auditoría. Puede ser un sistema específico (la Sede Electrónica) o todos los sistemas de la organización.

2.  **Revisión documental previa:** El auditor solicita y examina la documentación básica del SGSI:
    *   Política de Seguridad aprobada.
    *   Declaración de Aplicabilidad (SoA).
    *   Análisis de riesgos (MAGERIT/PILAR).
    *   Normativas y procedimientos de seguridad.
    *   Resultados de auditorías anteriores.
    *   Registro de incidentes de seguridad.

3.  **Elaboración del Plan de Auditoría:** Cronograma, equipo auditor, áreas a inspeccionar.

### 4.2. Fase 2: Ejecución (auditoría in situ)

El auditor verifica el cumplimiento de las medidas del ENS mediante:

*   **Revisión documental detallada:** Verificación de que los procedimientos documentados se ajustan a los requisitos del ENS.

*   **Entrevistas:** Con los responsables de seguridad, responsables de sistemas, administradores, usuarios y la dirección.

*   **Inspección técnica:** Comprobación in situ de la implantación real de las medidas:
    *   Verificación de políticas de contraseñas en Active Directory.
    *   Comprobación de reglas de firewall.
    *   Revisión de configuración de copias de seguridad (RMAN, Veeam).
    *   Prueba de restauración de backups (muestreo).
    *   Verificación de segmentación de red.
    *   Comprobación de cifrado de comunicaciones y datos en reposo.
    *   Revisión de logs de auditoría.
    *   Verificación de gestión de parches.

*   **Muestreo de evidencias:** El auditor solicita pruebas concretas (screenshots, informes de herramientas, registros de acceso) que demuestren el cumplimiento efectivo.

### 4.3. Fase 3: Informe de auditoría

El informe de auditoría es el documento formal que recoge los resultados. Incluye:

1.  **Alcance y metodología:** Sistemas auditados, marco de referencia (ENS), técnicas empleadas.

2.  **Hallazgos:** Para cada medida del ENS evaluada, el auditor determina su estado:
    *   **Conforme:** La medida se cumple satisfactoriamente.
    *   **No conformidad menor:** La medida se cumple parcialmente o con deficiencias que no comprometen significativamente la seguridad.
    *   **No conformidad mayor:** La medida no se cumple o se cumple de forma insuficiente, comprometiendo significativamente la seguridad. Las no conformidades mayores **impiden la certificación** hasta su resolución.
    *   **Observación:** Recomendación de mejora sin constituir una no conformidad.

3.  **Conclusión general:** Dictamen sobre la conformidad del sistema con el ENS.

4.  **Plan de acciones correctivas:** Recomendaciones para subsanar las no conformidades detectadas, con plazos propuestos.

**Principio fundamental:** El auditor **diagnostica pero no repara**. No es función del auditor configurar firewalls ni corregir vulnerabilidades; su función es identificar las carencias y documentarlas para que la organización las subsane.

### 4.4. Fase 4: Seguimiento y cierre

*   La organización implementa las acciones correctivas para las no conformidades.
*   El auditor verifica la eficacia de las correcciones (auditoría de seguimiento).
*   Si las no conformidades mayores se resuelven, se emite (o renueva) la certificación.
*   El ciclo se reinicia: la próxima auditoría ordinaria se programa dentro de los 2 años siguientes.

## 5. Certificación de Conformidad con el ENS

### 5.1. Proceso de certificación

1.  La organización contrata una **entidad de certificación acreditada por ENAC**.
2.  La entidad realiza la auditoría conforme al proceso descrito.
3.  Si el resultado es favorable, la entidad emite un **certificado de conformidad** con el ENS.
4.  El certificado se inscribe en el **Registro de Conformidad con el ENS** del CCN.
5.  La organización puede publicar el sello de conformidad en su Sede Electrónica.

### 5.2. Validez y renovación

*   La certificación tiene validez de **2 años**.
*   La renovación requiere una nueva auditoría completa.
*   Entre auditorías, la organización debe mantener las medidas y documentar los cambios.

### 5.3. Niveles de certificación

El certificado indica la categoría del sistema auditado (Básica, Media o Alta), permitiendo a terceros conocer el nivel de seguridad garantizado.

## 6. Herramienta INES

**INES (Informe Nacional del Estado de Seguridad)** es la herramienta del CCN que permite a las Administraciones Públicas:
*   Realizar la **autoevaluación** de conformidad con el ENS (obligatoria para categoría Básica).
*   Generar la **Declaración de Conformidad**.
*   Reportar el estado de seguridad al CCN para la elaboración del Informe Nacional del Estado de Seguridad.
*   El CCN utiliza los datos agregados para evaluar el estado de la ciberseguridad del sector público a nivel nacional.

## 7. Conclusión

La auditoría de conformidad con el ENS constituye el mecanismo de verificación que garantiza la efectividad real de las medidas de seguridad implantadas en los sistemas de información de las Administraciones Públicas. Su periodicidad bienal (para categorías Media y Alta), su ejecución por entidades independientes acreditadas por ENAC, y la clasificación de hallazgos en conformidades y no conformidades (mayores y menores) aseguran un proceso riguroso y objetivo.

El ciclo completo — planificación, ejecución, informe, plan correctivo, seguimiento y certificación — materializa la fase de verificación del PDCA y alimenta la mejora continua del SGSI. La inscripción en el Registro de Conformidad del CCN y la publicación del sello en la Sede Electrónica proporcionan transparencia a los ciudadanos sobre el nivel de seguridad con el que la Administración gestiona sus datos y servicios.
