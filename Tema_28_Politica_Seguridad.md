# Tema 28.- Política de Seguridad. Diseño, aprobación y aplicación.

## 1. Introducción

El Esquema Nacional de Seguridad (ENS, RD 311/2022) exige que toda Administración Pública disponga de una **Política de Seguridad** formal que articule las directrices de protección de sus sistemas de información. La Política de Seguridad no es un documento técnico elaborado por el departamento de informática, sino un instrumento de **gobierno organizacional** que debe emanar de la máxima autoridad de la organización y vincular a todo su personal.

Este tema analiza el proceso completo de diseño, aprobación y aplicación de la Política de Seguridad, así como el cuerpo documental normativo que se despliega por debajo de ella.

## 2. Diseño de la Política de Seguridad

### 2.1. Naturaleza del documento

La Política de Seguridad es el documento de **máximo nivel** en la jerarquía documental de seguridad de la organización. Define el "qué" y el "por qué" de la seguridad, pero no el "cómo" técnico. Establece los principios, las directrices y los compromisos de la organización en materia de seguridad de la información.

### 2.2. Contenido obligatorio

De acuerdo con el artículo 12 del RD 311/2022 y la guía CCN-STIC 805, la Política de Seguridad debe incluir:

1.  **Misión y objetivos de la organización:** Descripción de cómo la seguridad de la información contribute a la misión institucional del Ayuntamiento (garantizar servicios públicos de calidad, proteger los derechos de los ciudadanos, cumplir el marco legal).

2.  **Marco regulatorio aplicable:**
    *   ENS (RD 311/2022).
    *   RGPD (Reglamento 2016/679) y LOPDGDD (LO 3/2018).
    *   Ley 39/2015 (LPACAP) y Ley 40/2015 (LRJSP).
    *   Normativa sectorial específica.

3.  **Organización de la seguridad:** Definición de los roles, sus funciones y las relaciones entre ellos:

    | Rol | Responsabilidad |
    |-----|----------------|
    | **Responsable de la Información** | Determina los requisitos de seguridad de la información |
    | **Responsable del Servicio** | Determina los requisitos de seguridad de los servicios |
    | **Responsable de la Seguridad (CISO)** | Toma decisiones para satisfacer los requisitos de seguridad |
    | **Responsable del Sistema** | Opera el sistema de información conforme a las directrices del CISO |
    | **Delegado de Protección de Datos (DPD)** | Supervisa el cumplimiento de la normativa de protección de datos |
    | **Comité de Seguridad de la Información** | Órgano colegiado que coordina la seguridad a nivel directivo |

    **Principio fundamental de segregación de funciones:** El Responsable de la Seguridad y el Responsable del Sistema deben ser **personas distintas**, evitando que quien define las medidas de seguridad sea la misma persona que las implementa y opera.

4.  **Criterios de gestión de riesgos:** Metodología de análisis de riesgos adoptada (MAGERIT), criterios de aceptación de riesgos, periodicidad del análisis.

5.  **Directrices de concienciación y formación:** Compromiso de formar y sensibilizar al personal en materia de seguridad.

6.  **Gestión de incidentes de seguridad:** Referencia al Plan de Respuesta a Incidentes.

7.  **Proceso de revisión y actualización:** Periodicidad (al menos anual) y circunstancias que desencadenan una revisión extraordinaria (cambios organizativos significativos, incidentes graves, cambios normativos).

### 2.3. Proceso de diseño

1.  **Análisis del contexto:** Inventario de sistemas, identificación de partes interesadas, análisis del marco normativo.
2.  **Definición de objetivos de seguridad:** Alineados con los objetivos estratégicos de la organización.
3.  **Identificación de roles y responsabilidades:** Asignación de funciones a personas concretas.
4.  **Redacción del documento:** Lenguaje claro, sin jerga técnica excesiva, comprensible para todos los niveles de la organización.
5.  **Revisión por el Comité de Seguridad:** Validación del contenido antes de la aprobación formal.

## 3. Aprobación de la Política de Seguridad

### 3.1. Requisito de aprobación al máximo nivel

La Política de Seguridad carece de autoridad si es firmada únicamente por el responsable de informática. El ENS exige que sea aprobada por la **máxima autoridad** de la organización:

*   En un Ayuntamiento: el **Pleno Municipal**, la **Junta de Gobierno Local** o el **Alcalde**, según la estructura de delegaciones.
*   En la AGE: el órgano directivo correspondiente (Subsecretaría, Secretaría General).

### 3.2. Publicación y difusión

Una vez aprobada:
*   Se publica formalmente (Boletín Oficial de la Provincia, Portal de Transparencia, Intranet corporativa).
*   Se comunica a todo el personal: funcionarios, personal laboral, personal eventual y empresas proveedoras de servicios TIC.
*   Se incorpora a las cláusulas contractuales de los contratos de servicios informáticos (los proveedores deben comprometerse a cumplirla).

### 3.3. Actualización

*   **Revisión ordinaria:** Al menos una vez al año.
*   **Revisión extraordinaria:** Ante cambios significativos: reorganización administrativa, migración tecnológica importante (paso a cloud), incidente de seguridad grave, cambio normativo relevante.

## 4. Aplicación: Desarrollo del Cuerpo Documental

La Política de Seguridad (Nivel 1) se desarrolla mediante un cuerpo documental jerárquico que detalla progresivamente el "cómo":

### 4.1. Nivel 1: Política de Seguridad

Documento de máximo nivel, ya analizado. Define principios, directrices y compromisos.

### 4.2. Nivel 2: Normativas de Seguridad

Reglas obligatorias que desarrollan aspectos específicos de la Política. Aprobadas por el Comité de Seguridad o el CISO. Ejemplos:

*   **Normativa de Control de Acceso:** Reglas para la gestión de usuarios, contraseñas, autenticación multifactor, bloqueo de cuentas.
*   **Normativa de Clasificación de la Información:** Criterios para clasificar información como pública, interna, confidencial o reservada.
*   **Normativa de Uso Aceptable:** Reglas sobre el uso de equipos informáticos, correo electrónico, navegación web y dispositivos personales (BYOD).
*   **Normativa de Escritorio Limpio:** Obligación de no dejar documentos sensibles a la vista al abandonar el puesto de trabajo.
*   **Normativa de Dispositivos Móviles y Teletrabajo:** Requisitos de seguridad para el acceso remoto (VPN, cifrado, MDM).

### 4.3. Nivel 3: Procedimientos Operativos (SOPs)

Instrucciones técnicas detalladas, paso a paso, dirigidas al personal TIC. Describen el "cómo" concreto de las operaciones de seguridad. Ejemplos:

*   Procedimiento de alta y baja de usuarios en Active Directory.
*   Procedimiento de configuración del túnel IPSec entre sedes.
*   Procedimiento de backup y restauración con RMAN (Oracle).
*   Procedimiento de aplicación de parches de seguridad.
*   Procedimiento de respuesta ante un ransomware.
*   Procedimiento de gestión de vulnerabilidades (escaneo, priorización, parcheado).

### 4.4. Nivel 4: Registros y Evidencias

Documentación que demuestra el cumplimiento: logs de auditoría, registros de acceso, informes de escaneos de vulnerabilidades, actas de formación, informes de incidentes.

## 5. Declaración de Aplicabilidad (SoA)

La **Declaración de Aplicabilidad (Statement of Applicability — SoA)** es un documento obligatorio que lista todas las medidas de seguridad del Anexo II del ENS y, para cada una:

*   Indica si es **aplicable** o **no aplicable** al sistema.
*   Si es aplicable, describe las **medidas implementadas**.
*   Si no es aplicable, justifica la **exclusión**.

La SoA constituye el mapa de control de seguridad del sistema y es el documento de referencia para las auditorías.

## 6. Concienciación y Formación

La Política de Seguridad solo es efectiva si todo el personal la conoce y la aplica. El ENS exige programas de concienciación que incluyan:

*   **Formación inicial:** Para todo el personal de nueva incorporación.
*   **Formación periódica:** Sesiones de actualización al menos anuales.
*   **Simulacros:** Ejercicios de phishing ético, pruebas de ingeniería social.
*   **Comunicación continua:** Píldoras informativas, avisos de seguridad, boletines.
*   **Formación especializada:** Para el personal TIC, sobre las herramientas y procedimientos específicos de seguridad.

## 7. Conclusión

La Política de Seguridad es el documento fundacional del Sistema de Gestión de Seguridad de la Información de una Administración Pública. Su efectividad depende de tres factores: un **diseño** riguroso que defina roles, responsabilidades y directrices conforme al ENS; una **aprobación** al máximo nivel que le otorgue autoridad vinculante para toda la organización; y una **aplicación** efectiva mediante un cuerpo documental jerárquico (normativas, procedimientos, registros) que traduzca los principios abstractos en acciones concretas.

La Declaración de Aplicabilidad documenta el estado de implantación de las medidas del ENS, y los programas de concienciación garantizan que la seguridad no sea solo responsabilidad del departamento de informática, sino una cultura compartida por toda la organización.
