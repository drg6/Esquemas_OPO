# Tema 27.- Esquema Nacional de Seguridad. Normas CCN-STIC. Aspectos básicos de la Estrategia de Ciberseguridad Nacional y principales entidades actoras en relación con el sector público y privado.

## 1. Introducción

El **ENS** establece el marco normativo unificado y obligatorio para la protección de los sistemas de información del sector público. Las **Guías CCN-STIC** desarrollan técnicamente sus requisitos. Y la **Estrategia de Ciberseguridad Nacional** coordina los esfuerzos de todas las entidades públicas y privadas implicadas en la defensa del ciberespacio español.

## 2. El Esquema Nacional de Seguridad (ENS)

### 2.1. Marco legal

*   **Origen:** Establecido por la Ley 40/2015 (LRJSP), artículo 156.2.
*   **Regulación:** Real Decreto 311/2022, de 3 de mayo, que deroga el anterior RD 3/2010. Alinea el ENS con el Marco Europeo de Ciberseguridad y las directivas NIS/NIS2.

### 2.2. Ámbito de aplicación

El ENS es de obligado cumplimiento para todo el sector público:
*   Administración General del Estado (AGE).
*   Administraciones de las Comunidades Autónomas.
*   Administración Local.
*   Organismos públicos y entidades de derecho público.
*   Entidades del sector privado que presten servicios a las Administraciones Públicas.

### 2.3. Principios básicos del ENS

1.  **Seguridad como proceso integral:** Proceso continuo que abarca aspectos técnicos, humanos, materiales y organizativos.
2.  **Gestión de la seguridad basada en los riesgos:** Las medidas de seguridad deben ser proporcionales a los riesgos identificados. Requiere un análisis de riesgos formal (MAGERIT).
3.  **Prevención, detección, respuesta y conservación:**
    *   **Prevención:** Reducir la superficie de ataque (hardening, parcheado, formación).
    *   **Detección:** Monitorización continua (SIEM, IDS/IPS).
    *   **Respuesta:** Plan de respuesta a incidentes para contener y erradicar amenazas.
    *   **Conservación:** Copias de seguridad para garantizar la recuperación.
4.  **Existencia de líneas de defensa (Defensa en profundidad):** Múltiples capas de seguridad independientes: firewall perimetral, segmentación de red, cifrado de datos, autenticación multifactor, monitorización.
5.  **Vigilancia continua y reevaluación periódica:** 
6.  **Diferenciación de responsabilidades:** Separación entre quien determina los requisitos de seguridad (Responsable de la Seguridad) y quien opera los sistemas (Responsable del Sistema).

### 2.4. Dimensiones de seguridad

- **D → Disponibilidad** → poder acceder cuando se necesita.
- **I → Integridad** → datos no alterados sin autorización.
- **C → Confidencialidad** → acceso solo a autorizados.
- **A → Autenticidad** → verificar quién es el emisor.
- **T → Trazabilidad** → saber quién hizo cada acción.

🧠 **Mnemotecnia: D-I-C-A-T**

### 2.5. Categorización de los sistemas

- **BÁSICA** → perjuicio **limitado** → **autoevaluación**.
- **MEDIA** → perjuicio **grave** → **auditoría externa cada 2 años**. 
- **ALTA** → perjuicio **muy grave/irreparable** → **auditoría externa cada 2 años**.

**Se toma la categoría MÁS ALTA de sus dimensiones.**

### 2.6. Medidas de seguridad del ENS

**73 medidas de seguridad → 3 marcos:**

- **ORG → Organizativo**
  - Política y normativa de seguridad
  - Procedimientos.
  - Autorización.

- **OP → Operacional**
  - Planificación y análisis de riesgos.
  - Control de acceso.
  - Explotación.
  - Servicios externos.
  - Continuidad del servicio.
  - Monitorización.

- **MP → Protección**
  - Instalaciones y personal.
  - Equipos y comunicaciones.
  - Soportes.
  - Aplicaciones.
  - Información.
  - Servicios.

## 3. Guías CCN-STIC

### 3.1. Concepto

**CCN-STIC (Seguridad de las Tecnologías de la Información y las Comunicaciones)** → guías técnicas del **CCN** que desarrollan el **ENS** con instrucciones prácticas.

### 3.2. Series principales

- **000** → Políticas generales.
- **100** → Acreditación y certificación.
- **200** → Normas del ENS (categorización, declaración de conformidad).
- **300** → Instrucciones técnicas de cumplimiento.
- **400** → Guías generales de seguridad.
- **500** → Windows / hardening.
- **600** → Linux, móviles, cloud...
- **800** → ENS: implantación, auditoría y métricas.
- **1000** → Procedimientos específicos.

### 3.3. Guías más relevantes

*   **CCN-STIC 801:** Responsabilidades y funciones en el ENS.
*   **CCN-STIC 803:** Valoración de los sistemas en el ENS.
*   **CCN-STIC 804:** Medidas de implantación del ENS.
*   **CCN-STIC 808:** Verificación del cumplimiento del ENS.
*   **CCN-STIC 811:** Interconexión de la Red SARA.
*   **CCN-STIC 817:** Gestión de ciberincidentes.

### 3.4. Herramientas del CCN para AAPP

** LUCÍA (gestión incidentes), microCLAUDIA (vacunación ransomware), INES (estado de seguridad).

## 4. Estrategia de Ciberseguridad Nacional

### 4.1. Concepto

La **Estrategia de Seguridad Nacional** (revisada en 2021) definn las líneas de acción y los objetivos del Estado en materia de defensa del ciberespacio. 
Establece un modelo de gobernanza que coordina la actuación de múltiples organismos públicos y privados.

### 4.2. Estructura de gobernanza

- **CSN → Consejo de Seguridad Nacional**
  - Asesora al Presidente del Gobierno.
  - Coordina la respuesta ante **crisis de seguridad nacional**.

- **DSN → Departamento de Seguridad Nacional**
  - Apoyo **técnico** al CSN.
  - Coordina la gestión de **crisis de ciberseguridad**.

- **Consejo Nacional de Ciberseguridad**
  - Órgano **colegiado**.
  - Asesora al CSN en **ciberseguridad**.

## 5. Principales Entidades Actoras

### 5.1. Sector Público

- **CCN-CERT (Computer Emergency Response Team)** → ciberincidentes de las **AAPP**.
  - Depende del **CCN → CNI → Defensa**.
  - Herramientas: LUCÍA, ANA, CARMEN, PILAR, microCLAUDIA.
  - Guías → **CCN-STIC**.
  - Alertas y respuesta ante incidentes.

- **MCCE (Mando Conjunto del CiberEspacio)** → **ciberdefensa militar** → Defensa.
- **CNPIC (Centro Nacional de Protección de Infraestructuras Críticas)** → **infraestructuras críticas** → Interior.
- **AEPD (Agencia Española de protección de Datos)** → **protección de datos** → independiente.

### 5.2. Sector Privado y Ciudadanía

- **INCIBE (Instituto Nacional de Ciberseguridad)** → ciberseguridad de **empresas y ciudadanos**.
  - Ministerio de Transformación Digital.
  - **017** → atención y asesoramiento.
  - Concienciación.
- **INCIBE-CERT** → incidentes de empresas e infraestructuras críticas.

### 5.3. Cooperación público-privada

*   Intercambio de información sobre amenazas.
*   Ejercicios conjuntos de ciberseguridad.
*   Órgano supremo de colaboración público-privada (Foro Nacional de Ciberseguridad).
*   Impulso de la **Red Nacional de SOC (RNS)** para que las Diputaciones actúen como SOC (Centros de Operaciones de Ciberseguridad) Local dando cobertura a municipios pequeños.

## 6. Certificación de Conformidad con el ENS

## 📌 6. Certificación de conformidad con el ENS

- **BÁSICA** → **autoevaluación** → herramienta **INES (Informe Nacional del Estado de Seguridad) del CCN**.
- **MEDIA / ALTA** → **auditoría externa** → entidad acreditada por **ENAC (Entidad Nacional de Acreditación)**.
  - Periodicidad → **mínimo cada 2 años**.
- Resultado → inscripción en el **Registro de Conformidad del ENS (CCN)**.

## 7. Conclusión

El ENS es el marco normativo obligatorio de ciberseguridad de las Administraciones Públicas. 
Las Guías CCN-STIC concretan técnicamente cómo aplicar sus requisitos. 
La Estrategia Nacional de Ciberseguridad coordina a los principales organismos —CCN-CERT, INCIBE-CERT y MCCE— para prevenir, detectar y responder ante ciberamenazas, protegiendo los sistemas públicos y los datos y servicios de los ciudadanos.