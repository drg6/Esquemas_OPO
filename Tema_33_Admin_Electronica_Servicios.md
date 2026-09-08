# Tema 33.- La administración electrónica. Sede electrónica y carpeta ciudadana. Registro electrónico e interconexión de registros. Digitalización certificada. Firma y sellado digital. Gestor documental.

## 1. Introducción

- **Ley 39/2015 + Ley 40/2015:** establecen la tramitación administrativa electrónica.
- Instrumentos: **Sede → Carpeta → Registro → SIR/SICRES → Firma/Sello → Digitalización → Gestor documental**.

## 2. La Administración Electrónica: Concepto y Arquitectura

### 2.1. Concepto

- **Administración Electrónica (e-Government)** = transformación integral de los procesos administrativos mediante **TIC**.
- No es solo digitalizar papel.
- Objetivos:
  - **Eficiencia** interna.
  - **Transparencia**.
  - Servicios más **accesibles, ágiles y orientados al ciudadano**.

### 2.2. El modelo dual: Front-Office y Back-Office

- **Front-Office → relación con el ciudadano**
  - Parte visible/externa.
  - **Sede Electrónica**.
  - **Carpeta Ciudadana**.
  - **Registro Electrónico**.

- **Back-Office → gestión interna**
  - Parte interna de tramitación.
  - **SIR** → interconexión.
  - **@firma** → firma y sellado.
  - **Digitalización certificada**.
  - **Gestor Documental** → custodia del expediente.

## 3. Sede Electrónica

### 3.1. Concepto y naturaleza jurídica

- Dirección electrónica cuya titularidad y gestión corresponde a una **AAPP**.
- Tiene **efectos jurídicos**, a diferencia de una web informativa.

### 3.2. Requisitos legales y técnicos

- **Identificación** del titular.
- **Seguridad:** SSL/TLS + ENS.
- **Disponibilidad:** 24×7.
- **Accesibilidad:** WCAG 2.1 AA.
- **Integridad y autenticidad** de contenidos.
- **Fecha y hora oficial:** sincronización con ROA(Real Instituto y Observatorio de la Armada).
- Información obligatoria: servicios, normativa, carta de servicios, perfil del contratante, etc.

### 3.3. Sede Electrónica Asociada

Portal vinculado a una Sede principal para facilitar el acceso a determinados servicios.

## 4. Carpeta Ciudadana — Mi Carpeta

### 4.1. Concepto

- Espacio personal del ciudadano en el **PAG — Punto de Acceso General**, accesible mediante **Cl@ve, DNIe o certificado**.
- Permite:
  - Consultar **expedientes y su estado**.
  - Consultar **notificaciones**.
  - Consultar datos personales registrados.
  - Gestionar **representaciones y apoderamientos**.
  - Ejercer el **derecho de acceso** a sus datos.

### 4.2. Integración técnica

- Se alimenta de información de las AAPP mediante **Plataforma de Intermediación de Datos PID/SCSP**.
- Los Ayuntamientos integran sus sistemas para mostrar los trámites municipales.

## 5. Registro Electrónico e Interconexión de Registros

### 5.1. Registro Electrónico General

La Ley 39/2015 (artículo 16) obliga a cada Administración a disponer de un **Registro Electrónico General** que:

*   Opera las 24 horas del día, los 365 días del año.
*   Permite la presentación de documentos dirigidos a cualquier órgano de cualquier Administración Pública.
*   Emite un recibo electrónico que acredita la fecha y hora de presentación, el número de asiento y la relación de documentos presentados.
*   El cómputo de plazos se rige por la fecha y hora oficial del asiento registral.

### 5.2. Interconexión de Registros: SIR y SICRES

*   **SIR (Sistema de Interconexión de Registros):** Infraestructura que permite la interconexión de los registros electrónicos de las Administraciones Públicas para el intercambio de asientos registrales. Un ciudadano puede presentar una solicitud dirigida al Ministerio de Hacienda en el registro electrónico de su Ayuntamiento, y este la remite electrónicamente a través del SIR.

*   **SICRES (Sistema de Interconexión de Registros):** Norma técnica (actualmente en versión 4.0) que define el formato del asiento registral intercambiado: estructura del XML, metadatos obligatorios, documentos adjuntos, firmas electrónicas.

### 5.3. GEISER y ORVE

*   **GEISER:** Solución del MINHAP para la gestión integral del Registro Electrónico General, con integración nativa con SIR.
*   **ORVE (Oficina de Registro Virtual):** Herramienta web para la digitalización y remisión de documentos en papel a través de SIR, utilizada cuando un ciudadano presenta documentación en papel en una oficina de asistencia en materia de registros.

## 6. Firma y Sellado Electrónico

### 6.1. Firma electrónica: Concepto y tipos

La **firma electrónica** es el mecanismo que garantiza la autenticidad (identificación del firmante), la integridad (el documento no ha sido alterado) y el no repudio (el firmante no puede negar la firma).

El Reglamento eIDAS (UE 910/2014) define tres niveles:

| Tipo | Garantía | Ejemplo |
|------|----------|---------|
| **Firma electrónica simple** | Mínima | Nombre escrito en un email |
| **Firma electrónica avanzada** | Vinculada al firmante, permite detectar alteraciones | Firma con clave privada en dispositivo del usuario |
| **Firma electrónica cualificada** | Máxima. Equivalente legal a la firma manuscrita | Firma con certificado cualificado en dispositivo cualificado (DNIe, certificado FNMT en tarjeta criptográfica) |

### 6.2. Certificado electrónico X.509 v3

La infraestructura de clave pública (PKI) se basa en certificados digitales X.509 v3 emitidos por **Prestadores Cualificados de Servicios de Confianza**:
*   **FNMT-RCM (Fábrica Nacional de Moneda y Timbre):** Principal prestador en España.
*   **DNIe (Documento Nacional de Identidad electrónico):** Contiene dos certificados (autenticación y firma) en el chip de la tarjeta.

### 6.3. Sello electrónico de órgano

A diferencia de la firma electrónica (vinculada a una persona física), el **sello electrónico** identifica a una persona jurídica u órgano administrativo. Permite la firma automatizada y desatendida de actos administrativos masivos (liquidaciones tributarias, certificados de padrón) sin intervención humana en cada acto.

### 6.4. Plataforma @firma

**@firma** es la plataforma de validación de firma electrónica y certificados de la Administración General del Estado. Proporciona:
*   Validación de certificados y firmas electrónicas de múltiples prestadores.
*   Generación de firmas electrónicas en formatos estándar (XAdES, PAdES, CAdES).
*   Los Ayuntamientos integran sus aplicaciones con @firma para validar certificados y firmas.

### 6.5. Sellado de tiempo (Timestamp)

El **sello de tiempo** es una evidencia electrónica que acredita que un documento existía en un momento determinado y que no ha sido modificado desde entonces. Se emite por una Autoridad de Sellado de Tiempo (TSA). Es fundamental para garantizar la validez de las firmas electrónicas a largo plazo.

## 7. Digitalización Certificada

### 7.1. Concepto

La **digitalización certificada** (o copia auténtica electrónica) es el proceso mediante el cual un documento en papel se transforma en un documento electrónico con la misma validez legal que el original. La NTI de Digitalización de Documentos del ENI establece los requisitos:

*   La imagen debe ser fiel al documento original (resolución mínima de 200 ppp).
*   Se deben incluir los metadatos obligatorios del documento electrónico (NTI de Documento Electrónico del ENI).
*   La copia electrónica debe firmarse electrónicamente por el funcionario habilitado o mediante sello electrónico de órgano.
*   El formato resultante debe ser PDF/A o equivalente de conservación a largo plazo.

### 7.2. Proceso

1.  Escaneo del documento original en papel.
2.  Asignación de metadatos obligatorios.
3.  Firma electrónica del documento digitalizado (funcionario habilitado o sello de órgano).
4.  Incorporación al expediente electrónico.
5.  Destrucción del papel original (si procede, conforme a la política de gestión documental).

## 8. Gestor Documental

### 8.1. Concepto en el contexto de la Administración Electrónica

El **gestor documental** en el ámbito de las AAPP no es solo un repositorio de ficheros (analizado como DMS en el Tema 32). Es el componente del sistema de información que gestiona el ciclo de vida completo del documento electrónico administrativo conforme al ENI:

*   Captura y registro del documento (con metadatos ENI).
*   Clasificación y vinculación a expedientes.
*   Firma electrónica y sellado.
*   Versionado y trazabilidad.
*   Conservación a largo plazo (archivo electrónico).
*   Transferencia a archivos históricos.
*   Eliminación conforme a calendarios de conservación.

### 8.2. Archivo Electrónico Único

La Ley 40/2015 establece la obligación de un **Archivo Electrónico Único** que garantice la conservación, recuperación y acceso a los documentos y expedientes electrónicos a lo largo de todo su ciclo de vida, incluyendo la conservación a largo plazo con firmas válidas (firmas longevas — formatos AdES -T, -LT, -LTA).

## 9. Conclusión

La administración electrónica se materializa a través de un ecosistema de instrumentos técnico-jurídicos interconectados: la Sede Electrónica y la Carpeta Ciudadana proporcionan el punto de acceso del ciudadano; el Registro Electrónico y su interconexión mediante SIR/SICRES garantizan la presentación universal de documentos; la firma y el sellado electrónico confieren autenticidad e integridad a los actos administrativos; la digitalización certificada permite la transición del papel al formato digital con plena validez legal; y el gestor documental asegura el ciclo de vida completo del documento electrónico conforme al ENI.

La integración de todos estos componentes con las plataformas comunes del Estado (@firma, Cl@ve, Notific@, SIR) es la piedra angular de la modernización tecnológica de las Administraciones Públicas.