# Tema 24.- Diseño e implantación de políticas de seguridad. Implantación de las medidas de seguridad en Administraciones Locales. Respuesta a incidentes de seguridad. Gestión continua. Funciones de un CERT/CSIRT.

## 1. Introducción
La seguridad es un **proceso continuo**. Los Ayuntamientos gestionan datos muy sensibles (padrón, tributos, policía) cuya vulneración afecta derechos fundamentales y servicios esenciales.
El **ENS (RD 311/2022)** obliga a toda AAPP a disponer de una **Política de Seguridad** formal que articule un Sistema de Gestión de Seguridad de la Información (SGSI).

## 2. Diseño de la Política de Seguridad
### 2.1. Concepto
Documento de máximo nivel que establece directrices, principios y compromisos de la organización en seguridad.
### 2.2. Contenido obligatorio (Art. 12 ENS)
1. **Misión y objetivos.**
2. **Marco normativo:** ENS, RGPD, LOPDGDD.
3. **Roles y responsabilidades (Clave del ENS):**
   * *Responsable de la Información:* Define requisitos de la información.
   * *Responsable del Servicio:* Define requisitos del servicio.
   * *Responsable de la Seguridad (CISO):* Decisiones de seguridad. (Apoyado por el Comité de Seguridad).
   * *Responsable del Sistema:* Operación técnica del sistema.
   * ⚠️ *Principio de segregación de funciones:* Responsable de Seguridad ≠ Responsable del Sistema.
   * *Delegado de Protección de Datos (DPO).*
4. **Categorización de sistemas:** Dimensiones (Confidencialidad, Integridad, Disponibilidad, Autenticidad, Trazabilidad) → Básica, Media, Alta.
5. **Gestión de riesgos:** Metodología (MAGERIT).
6. **Revisión:** Periodicidad de actualización.

## 3. Implantación en Administraciones Locales
### 3.1. Ciclo PDCA (Deming)
1. **Plan (Planificar):** Aprobar Política → Categorizar Sistemas → Análisis de Riesgos (PILAR) → Declaración de Aplicabilidad (SoA) → Plan de Adecuación. *(Reporte anual mediante INES).*
2. **Do (Ejecutar):** Implantar medidas técnicas (MFA, cifrado, backups RMAN), organizativas (formación) y físicas (controles de acceso).
3. **Check (Verificar):** Auditorías (bienales), monitorización (SIEM), pentesting.
4. **Act (Mejorar):** Acciones correctivas post-auditoría.

### 3.2. Medidas del ENS
Divididas en tres marcos: **Organizativo** (políticas/procedimientos), **Operacional** (gestión/explotación/continuidad) y **Protección** (instalaciones/equipos/aplicaciones).

### 3.3. Particularidades Locales (Ayuntamientos)
* **Retos:** Presupuesto/Personal TIC limitado, dependencia de proveedores externos, sistemas *legacy*.
* **Soluciones ENS 2022:** Adopción del **Perfil de Cumplimiento Específico para EELL (PCE-EELL)**, servicios compartidos (Diputaciones) y herramientas del CCN.

## 4. Respuesta a Incidentes de Seguridad
### 4.1. Concepto
Evento que compromete las 5 dimensiones de seguridad de la información. (Ej. Ransomware, exfiltración de padrón, caída de Sede Electrónica).
### 4.2. Plan de Respuesta (Fases estándar)
1. **Detección e Identificación:** Mediante SIEM / IDS.
2. **Contención:** Aislar sistemas, desconectar red.
3. **Erradicación:** Eliminar malware, parchear vulnerabilidad.
4. **Recuperación:** Restaurar desde backups seguros.
5. **Notificación (Plazos legales críticos):**
   * Al **CCN-CERT** (Incidentes significativos sector público): Máx **72h**.
   * A la **AEPD** (Brechas de datos personales - RGPD): Máx **72h**.
   * A los **Afectados**: Sin dilación si hay alto riesgo para sus derechos.
6. **Lecciones aprendidas:** Análisis forense y mejora continua.

## 5. Gestión Continua de la Seguridad
La seguridad es viva. Implica: Monitorización continua (Logs/SIEM), gestión de vulnerabilidades (parcheo constante), concienciación del usuario (eslabón más débil) y medición de métricas como el *MTTD* (Tiempo Medio de Detección) y *MTTR* (Tiempo Medio de Respuesta).

## 6. Funciones de un CERT/CSIRT
### 6.1. Concepto y Funciones
Equipos especializados en prevención, detección y respuesta ante incidentes. Funciones: Alerta temprana, coordinación ante incidentes graves, análisis de vulnerabilidades, ciberinteligencia y formación.
### 6.2. CERTs de Referencia (Red Nacional)
* **CCN-CERT:** Sector Público (Ayuntamientos).
* **INCIBE-CERT:** Empresas, ciudadanos e Infraestructuras Críticas.
* **ESP-DEF-CERT:** Ministerio de Defensa.
### 6.3. Herramientas CCN para Ayuntamientos
* **LUCÍA:** Gestión/notificación de incidentes.
* **ANA:** Análisis de vulnerabilidades.
* **microCLAUDIA:** Vacunación contra ransomware.
* *Nota:* Las Diputaciones Provinciales pueden actuar como *CERT delegado* para municipios pequeños.

## 7. Conclusión
La ciberseguridad municipal no es una opción técnica, es un mandato legal (ENS, RGPD). Exige una Política de Seguridad aprobada por el Pleno/Alcaldía, segregación de roles, gestión por riesgos (MAGERIT) y un modelo de mejora continua. Ante incidentes inevitables, la agilidad del Plan de Respuesta y el apoyo del CCN-CERT son vitales para garantizar la resiliencia de los servicios públicos locales.

-----------------

# Tema 24.- Diseño e implantación de políticas de seguridad. Implantación de las medidas de seguridad en Administraciones Locales. Respuesta a incidentes de seguridad. Gestión continua. Funciones de un CERT/CSIRT.

## 1. Introducción

La seguridad es un **proceso continuo**. Los Ayuntamientos gestionan datos muy sensibles (padrón, tributos, policía, servicios sociales) cuya vulneración afecta derechos fundamentales y servicios esenciales.
El **ENS (RD 311/2022)** obliga a toda AAPP a disponer de una **Política de Seguridad** formal que articule un Sistema de Gestión de Seguridad de la Información (SGSI).

## 2. Diseño de la Política de Seguridad

### 2.1. Concepto

Documento de máximo nivel que establece directrices, principios y compromisos de la organización en seguridad.

### 2.2. Contenido obligatorio (Art. 12 ENS)

1.  **Objetivos y misión de la organización:** 
2.  **Marco normativo vinculante:** ENS (RD 311/2022), RGPD (Reglamento 2016/679), LOPDGDD (LO 3/2018).
3.  **Organización de la seguridad — Roles y responsabilidades (Clave del ENS):**
    *   **Responsable de la Información:** Define requisitos de la información.
    *   **Responsable del Servicio:** Define requisitos del servicio.
    *   **Responsable de la Seguridad (CISO, Chief Information Security Officer):** Decisiones de seguridad. (Apoyado por el Comité de Seguridad).
    *   **Responsable del Sistema:** Operación técnica del sistema. Persona distinta del Responsable de la Seguridad *Principio de segregación de funciones*
    *   **Delegado de Protección de Datos (DPD/DPO):** Obligatorio en AP.
4.  **Criterios de categorización de los sistemas:** Dimensiones (Confidencialidad, Integridad, Disponibilidad, Autenticidad, Trazabilidad).
5.  **Directrices de gestión de riesgos:** Metodología (MAGERIT), criterios de aceptación de riesgos.
6.  **Proceso de aprobación y revisión:** Periodicidad de revisión de la política.

## 3. Implantación de Medidas de Seguridad en Administraciones Locales

### 3.1. Ciclo PDCA (Plan-Do-Check-Act)

La implantación del SGSI sigue el ciclo de mejora continua de Deming (PDCA), alineado con ISO 27001:

1.  **Plan (Planificar):**
    *   Aprobar Política de Seguridad.
    *   Categorizar los sistemas de información (Básica, Media, Alta).
    *   Ejecutar el Análisis y Gestión de Riesgos (MAGERIT/PILAR).
    *   Elaborar la Declaración de Aplicabilidad (SoA).
    *   Definir el Plan de Adecuación al ENS. *(Reporte anual mediante INES).*
2.  **Do (Ejecutar):**
    *   Implantar las medidas de seguridad técnicas: firewalls, segmentación de redes, cifrado de discos, MFA, backups RMAN.
    *   Implantar las medidas organizativas: formación y concienciación del personal, gestión de accesos.
    *   Implantar las medidas de protección: control de acceso físico, gestión de soportes extraíbles.
3.  **Check (Verificar):**
    *   Auditorías internas y externas (bienales).
    *   Monitorización continua de la seguridad (SIEM).
    *   Pruebas de penetración (pentesting) periódicas.
4.  **Act (Mejorar):** Acciones correctivas post-auditoría.

### 3.2. Medidas de seguridad del ENS por marcos

*   **Marco Organizativo (org):** Política y normativa de seguridad y procedimientos.
*   **Marco Operacional (op):** Planificación, control de acceso, explotación, servicios externos, continuidad del servicio, monitorización.
*   **Marco de Protección (mp):** Instalaciones y personal, equipos y comunicaciones, soportes e información, aplicaciones y servicios.

### 3.3. Particularidades de las Administraciones Locales

* **Retos:** Presupuesto/Personal TIC limitado, dependencia de proveedores externos, sistemas *legacy*.
* **Soluciones ENS 2022:** Adopción del **Perfil de Cumplimiento Específico para EELL (PCE-EELL)**, servicios compartidos (Diputaciones) y herramientas del CCN.

## 4. Respuesta a Incidentes de Seguridad

### 4.1. Concepto de incidente de seguridad

Un **incidente de seguridad** es cualquier evento que comprometa la confidencialidad, integridad, disponibilidad, autenticidad o trazabilidad de la información o los servicios. Ejemplos: infección por ransomware, acceso no autorizado a la base de datos del padrón, exfiltración de datos personales, caída del servicio de Sede Electrónica.

### 4.2. Plan de Respuesta a Incidentes

El ENS exige disponer de un Plan de Respuesta a Incidentes que defina:

1.  **Detección e identificación:** Sistemas de detección (SIEM, IDS/IPS, antivirus, monitorización de logs) y clasificación del incidente.
2.  **Contención:** Limitar el alcance del incidente: Aislar sistemas, desconectar red.
3.  **Erradicación:** Eliminar malware, parchear vulnerabilidad.
4.  **Recuperación:** Restaurar desde backups seguros.
5.  **Notificación obligatoria:**
    * Al **CCN-CERT** (Incidentes significativos sector público): Máx **72h**.
    * A la **AEPD** (Brechas de datos personales - artículo 34 del RGPD): Máx **72h**.
    * A los **Afectados**: Sin dilación si hay alto riesgo para sus derechos (artículo 34 del RGPD).
6.  **Lecciones aprendidas:** AAnálisis forense y mejora continua.

## 5. Gestión Continua de la Seguridad

La seguridad es un proceso vivo. La gestión continua incluye:

*   **Monitorización continua:** Logs, SIEM y alertas.
*   **Gestión de vulnerabilidades:** Escaneos vulnerabilidades y parches.
*   **Concienciación del personal:** Formación, simulacros y campañas de sensibilización (eslabón más débil).
*   **Revisión periódica:** Política de seguridad al menos anual.
*   **Indicadores de seguridad:** Métricas como el tiempo medio de detección (MTTD), el tiempo medio de respuesta (MTTR).

*Monitorizar → Corregir → Formar → Revisar → Medir*

## 6. CERT/CSIRT: Equipos de Respuesta ante Emergencias

### 6.1. Concepto

Un **CERT (Computer Emergency Response Team)** o **CSIRT (Computer Security Incident Response Team)** es un equipo especializado en la prevención, detección, gestión y respuesta coordinada ante incidentes de ciberseguridad.

### 6.2. Funciones principales

Alerta temprana, coordinación ante incidentes graves, análisis de vulnerabilidades, ciberinteligencia y formación.

### 6.3. CERTs relevantes en España

* **CCN-CERT:** Administraciones Públicas.
* **INCIBE-CERT:** Empresas, ciudadanos e Infraestructuras Críticas.
* **ESP-DEF-CERT:** Ministerio de Defensa.

### 6.4. Herramientas CCN para Ayuntamientos

- **Herramientas CCN:**
  - **LUCÍA** → Gestión/notificación de incidentes.
  - **ANA** → análisis de vulnerabilidades.
  - **CARMEN** → detección de APT.
  - **microCLAUDIA** → protección frente a ransomware (vacunas)
- **Diputaciones** → pueden actuar como *CERT delegado* para municipios pequeños.

## 7. Conclusión

La ciberseguridad municipal no es una opción técnica, es un mandato legal (ENS, RGPD). Exige una Política de Seguridad aprobada por el Pleno/Alcaldía, segregación de roles, gestión por riesgos (MAGERIT) y un modelo de mejora continua. Ante incidentes inevitables, la agilidad del Plan de Respuesta y el apoyo del CCN-CERT son vitales para garantizar la resiliencia de los servicios públicos locales.