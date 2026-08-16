# Tema 46.- Identificación y firma electrónica. Marco europeo y nacional. Certificados digitales. Claves privadas, públicas y concertadas. Formatos de firma electrónica.

## 1. Introducción

La administración electrónica exige que las transacciones telemáticas ofrezcan las mismas garantías jurídicas que las presenciales: saber quién es el ciudadano que presenta una solicitud (**identificación**), garantizar que la solicitud no ha sido alterada (**integridad**), que su autor no pueda negar haberla firmado (**no repudio**) y que solo los destinatarios autorizados accedan a la información (**confidencialidad**).

La **firma electrónica** y los **certificados digitales** proporcionan estas garantías, amparados por un marco jurídico europeo (Reglamento eIDAS) y nacional (Ley 6/2020, Ley 39/2015) que regula su validez legal y los prestadores de servicios de confianza.

## 2. Marco Regulatorio

### 2.1. Marco Europeo: Reglamento eIDAS

El **Reglamento (UE) 910/2014 (eIDAS — Electronic Identification, Authentication and Trust Services)** establece un marco jurídico común para toda la UE en materia de:

*   **Identificación electrónica (eID):** Reconocimiento mutuo de los sistemas de identidad digital de los Estados miembros. Un ciudadano español puede identificarse con su DNIe ante una administración alemana.
*   **Servicios de confianza:** Regulación de la firma electrónica, sellos electrónicos, sellos de tiempo, servicios de entrega electrónica certificada y certificados de autenticación de sitios web.
*   **Efectos jurídicos:** Una firma electrónica cualificada tiene el mismo efecto jurídico que una firma manuscrita en todos los Estados miembros.
*   **Prestadores cualificados de servicios de confianza (QTSP):** Entidades supervisadas que emiten certificados cualificados y prestan servicios de confianza bajo estrictos requisitos de seguridad y auditoría.

### 2.2. Marco Nacional

*   **Ley 6/2020, reguladora de determinados aspectos de los servicios electrónicos de confianza:** Transpone e implementa el Reglamento eIDAS en España. Regula los prestadores de servicios de confianza cualificados.
*   **Ley 39/2015 (LPACAP):** Artículos 9 (identificación) y 10 (firma) establecen los sistemas de identificación y firma electrónica admitidos en las relaciones con la Administración.
*   **Ley 40/2015 (LRJSP):** Artículo 43 — sistemas de firma para la actuación administrativa automatizada (sello electrónico de órgano).

## 3. Criptografía de Clave Pública y Privada

### 3.1. Criptografía simétrica (Clave concertada)

*   Emisor y receptor comparten la **misma clave secreta** para cifrar y descifrar.
*   Algoritmo: **AES-256** (rápido, eficiente).
*   **Problema:** ¿Cómo transmitir la clave secreta de forma segura? (Problema de distribución de claves).
*   **Uso:** Cifrado de datos masivos (disco, comunicaciones — dentro de TLS/IPSec).

### 3.2. Criptografía asimétrica (Clave pública + Clave privada)

Cada participante dispone de un **par de claves** matemáticamente relacionadas:

| Clave | Custodia | Función |
|-------|----------|---------|
| **Clave Privada** | Secreta, solo en poder del titular (tarjeta criptográfica, HSM) | Firmar documentos, descifrar mensajes recibidos |
| **Clave Pública** | Distribuida libremente (dentro del certificado digital) | Verificar firmas, cifrar mensajes para el titular |

*   **Algoritmos:** RSA (2048/4096 bits), ECDSA (Curvas Elípticas — mayor seguridad con claves más cortas).
*   **Resuelve:** El problema de distribución de claves (la clave pública se puede enviar por canales inseguros).

### 3.3. Proceso de firma electrónica

1.  El firmante calcula el **hash** (huella digital) del documento con SHA-256.
2.  Cifra el hash con su **clave privada** → resultado: **firma electrónica** del documento.
3.  Envía el documento + la firma + su certificado digital (que contiene su clave pública).
4.  El destinatario:
    *   Extrae la clave pública del certificado del firmante.
    *   Descifra la firma con la clave pública → obtiene el hash original.
    *   Calcula el hash del documento recibido.
    *   **Si ambos hashes coinciden:** La firma es válida (integridad + autenticidad + no repudio).

### 3.4. Función Hash

Una **función hash** transforma un dato de cualquier tamaño en una cadena de longitud fija (huella digital):

| Algoritmo | Longitud | Estado |
|-----------|----------|--------|
| MD5 | 128 bits | **Obsoleto** (vulnerabilidades de colisión) |
| SHA-1 | 160 bits | **Obsoleto** (vulnerabilidades demostradas) |
| **SHA-256** (SHA-2) | 256 bits | **Estándar vigente** |
| **SHA-3** | 256/512 bits | Alternativa moderna |

## 4. Certificados Digitales

### 4.1. Concepto

Un **certificado digital** es un documento electrónico emitido por una Autoridad de Certificación (CA) que vincula de forma fehaciente la identidad de una persona (o entidad) con su clave pública. Estándar: **X.509 v3** (ITU-T).

### 4.2. Contenido de un certificado X.509 v3

| Campo | Contenido |
|-------|-----------|
| Versión | v3 |
| Número de serie | Identificador único asignado por la CA |
| Algoritmo de firma | SHA-256 con RSA |
| Emisor (Issuer) | CA emisora (ej. FNMT-RCM) |
| Periodo de validez | Fecha de inicio y expiración |
| Sujeto (Subject) | Identidad del titular (nombre, DNI, organización) |
| Clave pública del sujeto | RSA 2048 bits o ECDSA |
| Extensiones | Uso de la clave (firma, autenticación), restricciones, CRL Distribution Points, OCSP, Subject Alternative Name |
| Firma de la CA | Firma digital de la CA sobre todo el certificado |

### 4.3. Principales prestadores en España

| Prestador | Tipo de certificado |
|-----------|-------------------|
| **FNMT-RCM** | Certificado de persona física, representante, sede electrónica |
| **DNIe** (DGP) | Certificados de autenticación y firma en el chip del DNI |
| **ACCV** (Comunidad Valenciana) | Certificados de empleado público, sede electrónica |
| **Camerfirma** | Certificados de persona jurídica, empleado, factura electrónica |

## 5. Tipos de Firma Electrónica (Clasificación eIDAS)

| Tipo | Nivel de seguridad | Requisitos | Efecto jurídico |
|------|-------------------|------------|-----------------|
| **Firma electrónica simple** | Mínimo | Datos electrónicos asociados a otros datos (nombre en un email) | No se puede rechazar como prueba judicial, pero débil |
| **Firma electrónica avanzada** | Medio | Vinculada al firmante, creada con datos bajo su control exclusivo, detecta alteraciones | Aceptada con valor probatorio. Cumple los 4 requisitos del art. 26 eIDAS |
| **Firma electrónica cualificada** | Máximo | Firma avanzada + certificado cualificado + dispositivo cualificado de creación de firma (QSCD) | **Equivalente legal a la firma manuscrita** en toda la UE |

### 5.1. Sistemas admitidos en la Administración (Ley 39/2015)

**Para los ciudadanos (art. 9 y 10):**
*   Certificado electrónico cualificado (FNMT, DNIe).
*   Cl@ve (sistema de identificación/firma para ciudadanos: Cl@ve PIN, Cl@ve Permanente, Cl@ve Firma).
*   Cualquier otro admitido por la Administración.

**Para la actuación automatizada de la Administración (art. 42 Ley 40/2015):**
*   Sello electrónico de Administración Pública.
*   Código seguro de verificación (CSV).

## 6. Formatos de Firma Electrónica

Los formatos de firma definen la estructura técnica de la firma y determinan su validez a largo plazo:

| Formato | Basado en | Uso típico |
|---------|-----------|-----------|
| **XAdES** (XML Advanced Electronic Signatures) | XML | Firma de documentos XML, facturas electrónicas (Facturae) |
| **PAdES** (PDF Advanced Electronic Signatures) | PDF | Firma de documentos PDF (la firma se incrusta en el propio PDF) |
| **CAdES** (CMS Advanced Electronic Signatures) | CMS/PKCS#7 | Firma de ficheros binarios genéricos |
| **JAdES** (JSON Advanced Electronic Signatures) | JSON | Firma de datos JSON (APIs REST) |

### 6.1. Niveles de firma longeva

Para que una firma sea válida a largo plazo (más allá de la expiración del certificado), se añaden evidencias:

| Nivel | Contenido adicional | Validez |
|-------|---------------------|---------|
| **-B** (Basic) | Solo la firma | Corto plazo |
| **-T** (Timestamp) | + Sello de tiempo | Prueba temporal |
| **-LT** (Long Term) | + Certificados + respuestas OCSP/CRL | Validable sin conexión |
| **-LTA** (Long Term Archival) | + Sellos de tiempo periódicos de archivo | Validez indefinida |

### 6.2. Plataforma @firma

**@firma** es la plataforma de la Administración General del Estado para la creación y validación de firmas electrónicas. Soporta todos los formatos (XAdES, PAdES, CAdES) y valida certificados de múltiples prestadores.

## 7. Conclusión

La identificación y firma electrónica constituyen la base tecnológica y jurídica de la administración electrónica. El Reglamento eIDAS proporciona un marco europeo armonizado, implementado en España por la Ley 6/2020. La criptografía asimétrica (clave pública/privada) y los certificados digitales X.509 v3 emitidos por prestadores cualificados (FNMT, DNIe) garantizan la autenticidad, integridad y no repudio de los actos administrativos electrónicos. Los formatos de firma longeva (XAdES-LTA, PAdES-LTA) aseguran la validez de las firmas a lo largo del tiempo, requisito esencial para el archivo electrónico a largo plazo de los expedientes administrativos.
