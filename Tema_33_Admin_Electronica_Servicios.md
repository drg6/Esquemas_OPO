# Tema 33.- La administración electrónica. Sede electrónica y carpeta ciudadana. Registro electrónico e interconexión de registros. Digitalización certificada. Firma y sellado digital. Gestor documental.

## 1. Introducción

La Ley 39/2015 (LPACAP) y la Ley 40/2015 (LRJSP), analizadas en el Tema 26, establecen la obligatoriedad del medio electrónico como canal nativo de la tramitación administrativa. Este tema profundiza en los instrumentos técnicos y jurídicos que materializan esa obligación: la **Sede Electrónica**, la **Carpeta Ciudadana**, el **Registro Electrónico**, la **interconexión de registros (SIR/SICRES)**, la **firma y sellado digital**, la **digitalización certificada** y el **gestor documental**.

## 2. Sede Electrónica

### 2.1. Concepto y naturaleza jurídica

La **Sede Electrónica** es la dirección electrónica (URL) cuya titularidad, gestión y administración corresponde a una Administración Pública. A diferencia de una página web informativa convencional, la Sede Electrónica tiene **plenos efectos jurídicos**: una notificación publicada en la Sede tiene la misma validez legal que una notificación en papel entregada en mano.

### 2.2. Requisitos legales y técnicos

*   **Identificación del titular:** Debe identificar de forma inequívoca al órgano administrativo titular.
*   **Seguridad:** Certificado SSL/TLS cualificado, conformidad con el ENS (Tema 27).
*   **Disponibilidad:** Operativa 24 horas al día, 365 días al año. Las interrupciones programadas deben anunciarse con antelación.
*   **Accesibilidad:** Nivel AA de las WCAG 2.1 (RD 1112/2018 — Tema 17).
*   **Integridad y autenticidad:** Los contenidos publicados son responsabilidad del titular.
*   **Fecha y hora oficial:** Sincronización con el Real Instituto y Observatorio de la Armada.
*   **Contenido obligatorio:** Directorio de sedes electrónicas, relación de servicios disponibles, normativa aplicable, carta de servicios, acceso al perfil del contratante.

### 2.3. Sede Electrónica Asociada

El RD 203/2021 introduce la **Sede Electrónica Asociada**, un portal web vinculado a una Sede Electrónica que facilita el acceso a servicios sin la totalidad de los requisitos de la Sede principal. Permite a las AAPP ofrecer servicios adicionales con menor rigidez formal.

## 3. Carpeta Ciudadana — Mi Carpeta

### 3.1. Concepto

La **Carpeta Ciudadana** (actualmente denominada **Mi Carpeta** en el PAG — Punto de Acceso General) es un espacio personal del ciudadano al que accede tras autenticarse (mediante Cl@ve, DNIe o certificado FNMT). Desde ella puede:

*   Consultar el estado de tramitación de sus expedientes abiertos en cualquier Administración.
*   Acceder a sus notificaciones electrónicas pendientes.
*   Consultar sus datos de identidad, domicilio, vehículos, títulos y otros registrados en las AAPP.
*   Gestionar sus representaciones y apoderamientos.
*   Consultar los datos que las AAPP poseen sobre él (ejercicio del derecho de acceso — RGPD).

### 3.2. Integración técnica

La Carpeta Ciudadana se nutre de datos proporcionados por las distintas administraciones a través de la **Plataforma de Intermediación de Datos (PID/SCSP)** — Tema 31. Los Ayuntamientos deben integrar sus sistemas de gestión de expedientes con la Carpeta para que los ciudadanos puedan consultar el estado de sus trámites municipales.

## 4. Registro Electrónico e Interconexión de Registros

### 4.1. Registro Electrónico General

La Ley 39/2015 (artículo 16) obliga a cada Administración a disponer de un **Registro Electrónico General** que:

*   Opera las 24 horas del día, los 365 días del año.
*   Permite la presentación de documentos dirigidos a cualquier órgano de cualquier Administración Pública.
*   Emite un recibo electrónico que acredita la fecha y hora de presentación, el número de asiento y la relación de documentos presentados.
*   El cómputo de plazos se rige por la fecha y hora oficial del asiento registral.

### 4.2. Interconexión de Registros: SIR y SICRES

*   **SIR (Sistema de Interconexión de Registros):** Infraestructura que permite la interconexión de los registros electrónicos de las Administraciones Públicas para el intercambio de asientos registrales. Un ciudadano puede presentar una solicitud dirigida al Ministerio de Hacienda en el registro electrónico de su Ayuntamiento, y este la remite electrónicamente a través del SIR.

*   **SICRES (Sistema de Interconexión de Registros):** Norma técnica (actualmente en versión 4.0) que define el formato del asiento registral intercambiado: estructura del XML, metadatos obligatorios, documentos adjuntos, firmas electrónicas.

### 4.3. GEISER y ORVE

*   **GEISER:** Solución del MINHAP para la gestión integral del Registro Electrónico General, con integración nativa con SIR.
*   **ORVE (Oficina de Registro Virtual):** Herramienta web para la digitalización y remisión de documentos en papel a través de SIR, utilizada cuando un ciudadano presenta documentación en papel en una oficina de asistencia en materia de registros.

## 5. Firma y Sellado Electrónico

### 5.1. Firma electrónica: Concepto y tipos

La **firma electrónica** es el mecanismo que garantiza la autenticidad (identificación del firmante), la integridad (el documento no ha sido alterado) y el no repudio (el firmante no puede negar la firma).

El Reglamento eIDAS (UE 910/2014) define tres niveles:

| Tipo | Garantía | Ejemplo |
|------|----------|---------|
| **Firma electrónica simple** | Mínima | Nombre escrito en un email |
| **Firma electrónica avanzada** | Vinculada al firmante, permite detectar alteraciones | Firma con clave privada en dispositivo del usuario |
| **Firma electrónica cualificada** | Máxima. Equivalente legal a la firma manuscrita | Firma con certificado cualificado en dispositivo cualificado (DNIe, certificado FNMT en tarjeta criptográfica) |

### 5.2. Certificado electrónico X.509 v3

La infraestructura de clave pública (PKI) se basa en certificados digitales X.509 v3 emitidos por **Prestadores Cualificados de Servicios de Confianza**:
*   **FNMT-RCM (Fábrica Nacional de Moneda y Timbre):** Principal prestador en España.
*   **DNIe (Documento Nacional de Identidad electrónico):** Contiene dos certificados (autenticación y firma) en el chip de la tarjeta.

### 5.3. Sello electrónico de órgano

A diferencia de la firma electrónica (vinculada a una persona física), el **sello electrónico** identifica a una persona jurídica u órgano administrativo. Permite la firma automatizada y desatendida de actos administrativos masivos (liquidaciones tributarias, certificados de padrón) sin intervención humana en cada acto.

### 5.4. Plataforma @firma

**@firma** es la plataforma de validación de firma electrónica y certificados de la Administración General del Estado. Proporciona:
*   Validación de certificados y firmas electrónicas de múltiples prestadores.
*   Generación de firmas electrónicas en formatos estándar (XAdES, PAdES, CAdES).
*   Los Ayuntamientos integran sus aplicaciones con @firma para validar certificados y firmas.

### 5.5. Sellado de tiempo (Timestamp)

El **sello de tiempo** es una evidencia electrónica que acredita que un documento existía en un momento determinado y que no ha sido modificado desde entonces. Se emite por una Autoridad de Sellado de Tiempo (TSA). Es fundamental para garantizar la validez de las firmas electrónicas a largo plazo.

## 6. Digitalización Certificada

### 6.1. Concepto

La **digitalización certificada** (o copia auténtica electrónica) es el proceso mediante el cual un documento en papel se transforma en un documento electrónico con la misma validez legal que el original. La NTI de Digitalización de Documentos del ENI establece los requisitos:

*   La imagen debe ser fiel al documento original (resolución mínima de 200 ppp).
*   Se deben incluir los metadatos obligatorios del documento electrónico (NTI de Documento Electrónico del ENI).
*   La copia electrónica debe firmarse electrónicamente por el funcionario habilitado o mediante sello electrónico de órgano.
*   El formato resultante debe ser PDF/A o equivalente de conservación a largo plazo.

### 6.2. Proceso

1.  Escaneo del documento original en papel.
2.  Asignación de metadatos obligatorios.
3.  Firma electrónica del documento digitalizado (funcionario habilitado o sello de órgano).
4.  Incorporación al expediente electrónico.
5.  Destrucción del papel original (si procede, conforme a la política de gestión documental).

## 7. Gestor Documental

### 7.1. Concepto en el contexto de la Administración Electrónica

El **gestor documental** en el ámbito de las AAPP no es solo un repositorio de ficheros (analizado como DMS en el Tema 32). Es el componente del sistema de información que gestiona el ciclo de vida completo del documento electrónico administrativo conforme al ENI:

*   Captura y registro del documento (con metadatos ENI).
*   Clasificación y vinculación a expedientes.
*   Firma electrónica y sellado.
*   Versionado y trazabilidad.
*   Conservación a largo plazo (archivo electrónico).
*   Transferencia a archivos históricos.
*   Eliminación conforme a calendarios de conservación.

### 7.2. Archivo Electrónico Único

La Ley 40/2015 establece la obligación de un **Archivo Electrónico Único** que garantice la conservación, recuperación y acceso a los documentos y expedientes electrónicos a lo largo de todo su ciclo de vida, incluyendo la conservación a largo plazo con firmas válidas (firmas longevas — formatos AdES -T, -LT, -LTA).

## 8. Conclusión

La administración electrónica se materializa a través de un ecosistema de instrumentos técnico-jurídicos interconectados: la Sede Electrónica y la Carpeta Ciudadana proporcionan el punto de acceso del ciudadano; el Registro Electrónico y su interconexión mediante SIR/SICRES garantizan la presentación universal de documentos; la firma y el sellado electrónico confieren autenticidad e integridad a los actos administrativos; la digitalización certificada permite la transición del papel al formato digital con plena validez legal; y el gestor documental asegura el ciclo de vida completo del documento electrónico conforme al ENI.

La integración de todos estos componentes con las plataformas comunes del Estado (@firma, Cl@ve, Notific@, SIR) es la piedra angular de la modernización tecnológica de las Administraciones Públicas.
