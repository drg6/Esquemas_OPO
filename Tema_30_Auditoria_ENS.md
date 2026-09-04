# Tema 30.- Auditorías de conformidad con el Esquema Nacional de Seguridad (ENS).

## 1. Introducción

- **ENS (RD 311/2022):** establece medidas de seguridad y exige verificar su cumplimiento.
- **Auditoría ENS:** proceso formal, periódico e independiente para comprobar el cumplimiento.
- Forma parte del **PDCA (Plan-Do-Check-Act) → CHECK (Verificar)**.
- Garantiza que las medidas implantadas son realmente efectivas.

## 2. Fundamento Legal y Obligatoriedad

### 2.1. Base normativa

- **Art. 34 RD 311/2022:** obligación de auditar los sistemas para verificar el cumplimiento del ENS.

### 2.2. Periodicidad según la categoría del sistema

- **BÁSICA:**
  - Autoevaluación mediante **INES**.
  - Al menos cada **2 años**.

- **MEDIA:**
  - Auditoría formal interna o externa.
  - Al menos cada **2 años**.

- **ALTA:**
  - Auditoría formal interna o externa.
  - Al menos cada **2 años**.

### 2.3. Auditorías extraordinarias

- Se realizan ante **cambios sustanciales** que afecten a la seguridad:
  - Migración a **cloud**.
  - Reestructuración importante de la red.
  - Nuevo sistema de categoría Media o Alta.
  - Incidente grave de seguridad.

## 3. Tipos de Auditoría

### 3.1. Autoevaluación (categoría BÁSICA)

- Realizada por la propia organización.
- Herramienta: **INES (Informe Nacional del Estado de Seguridad) del CCN**.
- Resultado → **Declaración de Conformidad**.
- Publicación en la **Sede Electrónica**.

### 3.2. Auditoría interna

- Realizada por personal o servicio interno.
- Auditor → **independiente del sistema auditado**.
- Debe tener competencias en seguridad.
- **No permite obtener el sello oficial de certificación ENS.**

### 3.3. Auditoría externa (de certificación)

- Permite obtener la **Certificación de Conformidad ENS**.
- Realizada por una entidad:
  - **Independiente**.
  - **Acreditada por ENAC**.
- Auditores cualificados según el esquema ENS.
- La certificadora **no puede haber participado en la implantación** de las medidas auditadas.
- Ejemplos: AENOR, BSI, Bureau Veritas, TÜV, DNV.

## 4. Fases del Proceso de Auditoría

### 4.1. Fase 1: Planificación

1. **Definir alcance** → qué sistemas se auditan.
2. **Revisión documental previa:**
   - Política de Seguridad.
   - Declaración de Aplicabilidad (SoA - Statement of Applicability).
   - Análisis de riesgos (MAGERIT/PILAR).
   - Normativas y procedimientos.
   - Auditorías anteriores.
   - Registro de incidentes.
3. **Plan de auditoría** → calendario, equipo y áreas.

### 4.2. Fase 2: Ejecución (auditoría in situ)

- **Revisión documental detallada**.
- **Entrevistas** → responsables de seguridad y sistemas, administradores, usuarios y dirección.
- **Inspección técnica:**
  - Contraseñas y Active Directory.
  - Reglas Firewall.
  - Copias de seguridad y restauración (RMAN, Veeam).
  - Segmentación de red.
  - Cifrado.
  - Logs.
  - Parches.
- **Muestreo de evidencias** → informes, registros, capturas, etc.

### 4.3. Fase 3: Informe de auditoría

1. **Alcance y metodología**.
2. **Hallazgos:**
   - **Conforme** → cumple.
   - **No conformidad menor** → deficiencias sin impacto significativo.
   - **No conformidad mayor** → incumplimiento significativo → **impide la certificación** hasta resolverla.
   - **Observación** → recomendación de mejora.
3. **Conclusión general** → grado de conformidad.
4. **Plan de acciones correctivas**.

- **Principio:** el auditor **diagnostica, pero no repara**.

### 4.4. Fase 4: Plan de Adecuación, seguimiento y cierre

- Ante las deficiencias, la organización elabora un **Plan de Adecuación** (cronograma con acciones correctivas).
- El auditor → verifica su eficacia (auditoría de seguimiento).
- Resolución de no conformidades → emisión o renovación de certificación.
- Nueva auditoría ordinaria → **≤ 2 años**.

## 5. Certificación de Conformidad con el ENS

### 5.1. Proceso de certificación

1. Contratar entidad **acreditada por ENAC**.
2. Realizar auditoría.
3. Resultado favorable → **certificado de conformidad**.
4. Inscripción en el **Registro de Conformidad ENS (CCN)**.
5. Publicación del **sello de conformidad** en la Sede Electrónica.

### 5.2. Validez y renovación

- Validez → **2 años**.
- Renovación → nueva auditoría completa.
- Entre auditorías → mantener las medidas y documentar cambios.

### 5.3. Niveles de certificación

El certificado indica la categoría del sistema auditado (Básica, Media o Alta), permitiendo a terceros conocer el nivel de seguridad garantizado.

## 6. Herramienta INES

**INES (Informe Nacional del Estado de Seguridad)** es la herramienta del CCN (basada en métricas de la guía **CCN-STIC 824**) que permite a las Administraciones Públicas:
*   Realizar **autoevaluación ENS** → categoría Básica.
*   Generar la **Declaración de Conformidad**.
*   Reportar el estado de seguridad al **CCN**.
- El CCN utiliza los datos agregados para elaborar el **Informe Nacional del Estado de Seguridad**.

## 7. Conclusión

- La auditoría ENS verifica el **cumplimiento real** de las medidas de seguridad.
- Proceso:
  **Planificación → Ejecución → Informe → Corrección → Seguimiento**.
- Garantiza la **conformidad con el ENS** y la **mejora continua**.