# Tema 46.- Identificación y firma electrónica. Marco europeo y nacional. Certificados digitales. Claves privadas, públicas y concertadas. Formatos de firma electrónica.

## 1. Introducción
- **Contexto:** La transformación digital municipal exige trasladar la confianza del papel manuscrito al entorno electrónico.
- **Las 4 Garantías Jurídicas:** El objetivo de la criptografía y la firma es aportar:
  1. **Identificación:** Certeza absoluta de quién es el autor.
  2. **Integridad:** El documento no ha sido alterado tras su firma.
  3. **No repudio:** El autor no puede negar su autoría a posteriori.
  4. **Confidencialidad:** Solo el destinatario legítimo puede leerlo (mediante cifrado).

## 2. Identificación y firma electrónica
- **2.1. Identificación vs. Firma (Ley 39/2015):** 
  * *Identificación (Art. 9):* Demostrar quién eres para acceder a un servicio (PIN en Cl@ve).
  * *Firma (Art. 10):* Expresar voluntad y consentimiento sobre un trámite (Portafirmas, CSV).
- **2.2. Tipos de Firma (eIDAS):** 
  * *Simple:* Datos lógicos asociados (Poco valor probatorio).
  * *Avanzada:* Identifica unívocamente, bajo el control exclusivo del firmante y detecta modificaciones.
  * *Cualificada:* Avanzada + Dispositivo cualificado (QSCD). Es la única que equivale legalmente a la firma manuscrita.
- **2.3. Sistemas Admitidos en las AAPP (Aplicación práctica):** 
  * *Ciudadanos:* DNIe, Certificado cualificado (FNMT,ACCV) y plataforma **Cl@ve**.
  * *Administración:* **Portafirmas corporativo** (para empleados), Sello Electrónico de Órgano (actuación automatizada) y el **CSV (Código Seguro de Verificación)**, vital porque permite imprimir un PDF firmado y verificar su autenticidad original en la Sede Electrónica.

## 3. Marco europeo y nacional
- **3.1. Marco Europeo (eIDAS - Reg. UE 910/2014):**
  * *Objetivo:* Crear el Mercado Único Digital mediante la **interoperabilidad transfronteriza** (que una firma de España valga en Alemania).
  * *Concepto clave:* Define los Servicios de Confianza y la figura del **QTSP** (Prestador Cualificado de Servicios de Confianza).
  * *Principio de equivalencia:* Establece por ley que la **firma electrónica cualificada** tiene el mismo efecto jurídico que la firma manuscrita en toda la UE.
- **3.2. Marco Nacional:**
  * **Ley 6/2020:** Traspone el eIDAS y regula los prestadores de servicios de confianza cualificados en España.
  * **Ley 39/2015:** Art. 9 (Sistemas de Identificación) y Art. 10 (Sistemas de Firma).
  * **Ley 40/2015:** Art. 42 y 43 (Firma por actuación automatizada y Sello de Órgano).
  * **Plataforma @firma:** Pasarela del Estado (Red SARA) que usan los Ayuntamientos para validar identidades y firmas.

## 4. Certificados digitales
- **4.1. Concepto y Estructura (X.509 v3):** Fichero electrónico emitido por una Autoridad de Certificación (CA) que vincula matemáticamente a una persona con su Clave Pública.
- **4.2. Validación (OCSP vs CRL):** Para comprobar si un certificado está revocado hoy, las AAPP usan **OCSP** (verificación online en tiempo real), descartando las pesadas listas CRL.
- **4.3. Uso en AAPP:** Destacan el Sello de Órgano y los **Certificados de Empleado Público con Seudónimo** (ej. Policía Local, exigido por el ENS para proteger la identidad real).

## 5. Claves privadas, públicas y concertadas
- **5.1. Claves Concertadas (Criptografía Simétrica):** Misma clave cifrar y descifrar (AES-256). Muy rápida, pero con el gran problema logístico de cómo distribuir la clave de forma segura. *Uso:* Bitlocker y TLS / IPSec
- **5.2. Claves Públicas y Privadas (Criptografía Asimétrica):** par de claves matemáticamente relacionadas (RSA/ECDSA Curvas Elípticas — mayor seguridad con claves más cortas).
  * *Privada:* Secreta, solo en poder del titular (Firma y descifra).
  * *Pública:* Distribuida abiertamente en el certificado (Verifica y cifra).
- **5.3. El proceso de firma (El Hash):**
  1. El firmante cifra el **Hash SHA-256** del documento usando su Clave Privada.
  2. El receptor descifra la firma con la Clave Pública y comprueba que el Hash coincida.

## 6. Formatos de firma electrónica
- **6.1. Formatos Base:**
  * **XAdES (XML):** Estándar estructurado, obligatorio para la FacturaE.
  * **PAdES (PDF):** Firma incrustada visualmente en un documento PDF.
  * **CAdES (CMS):** Archivos binarios genéricos.
  * **JAdES (JSON):** Nuevo estándar optimizado para APIs REST modernas.
- **6.2. La Firma Longeva (Fase de Archivo):**
  * Para que un expediente valga dentro de 20 años (cuando el certificado haya caducado), se usa el formato **LTA (Long Term Archival)**, incrustando sellos de tiempo (TSA) y evidencias OCSP de forma iterativa.

## 7. Conclusión
El acto administrativo digital exige robustez criptográfica (X.509 v3, RSA). Gracias al reglamento eIDAS y al marco nacional, los Ayuntamientos pueden orquestar plataformas que garantizan la integridad y el no repudio, transformando el documento electrónico en una evidencia perdurable en el tiempo mediante formatos longevos LTA.

---------------------------

# Tema 46.- Identificación y firma electrónica. Marco europeo y nacional. Certificados digitales. Claves privadas, públicas y concertadas. Formatos de firma electrónica.

## 1. Introducción

La administración electrónica exige que las transacciones telemáticas ofrezcan las mismas garantías jurídicas que las presenciales: 
    - Saber quién es el ciudadano que presenta una solicitud (**identificación**)
    - Garantizar que la solicitud no ha sido alterada (**integridad**)
    - Que su autor no pueda negar haberla firmado (**no repudio**) 
    - Que solo los destinatarios autorizados accedan a la información (**confidencialidad**).

La **firma electrónica** y los **certificados digitales** proporcionan estas garantías, amparados por un marco jurídico europeo (**Reglamento eIDAS**) y nacional (**Ley 6/2020**, **Ley 39/2015**).

## 2. Marco Regulatorio

### 2.1. Marco Europeo: Reglamento eIDAS

- **Reglamento (UE) 910/2014 (eIDAS — Electronic Identification, Authentication and Trust Services):** marco europeo común para:
  - **Identificación electrónica (eID):** reconocimiento mutuo de sistemas de identidad digital.
  - **Servicios de confianza:** firmas, sellos electrónicos, sellos de tiempo, entrega electrónica certificada y certificados web.
  - **Efectos jurídicos:** la **firma cualificada equivale a la firma manuscrita** en toda la UE.
  - **Prestadores cualificados de servicios de confianza (QTSP):** prestadores cualificados de servicios de confianza, sujetos a supervisión y auditoría.

### 2.2. Marco Nacional

- **Ley 6/2020:** regula determinados aspectos de los servicios electrónicos de confianza. Transpone e implementa el Reglamento eIDAS en España. 
- **Ley 39/2015 (LPACAP):**
  - Art. 9 → **identificación**.
  - Art. 10 → **firma**.
- **Ley 40/2015 (LRJSP):**
  - Art. 43 → firma en **actuaciones administrativas automatizadas** (sello electrónico de órgano).

## 3. Criptografía de Clave Pública y Privada

### 3.1. Criptografía simétrica (Clave concertada)

- Emisor y receptor utilizan la **misma clave secreta** para cifrar y descifrar.
- **Ejemplo:** AES-256.
- **Ventaja:** rápida y eficiente.
- **Problema:** distribución segura de la clave.
- **Uso:** cifrado de grandes volúmenes de datos.

### 3.2. Criptografía asimétrica (Clave pública + Clave privada)

- Cada usuario dispone de un **par de claves relacionadas matemáticamente**.

- **Clave privada:**
  - Secreta y bajo control exclusivo del titular.
  - Se utiliza para **firmar** y descifrar mensajes.

- **Clave pública:**
  - Se puede distribuir libremente.
  - Se utiliza para **verificar firmas** y cifrar mensajes destinados al titular.

- **Algoritmos:** RSA (2048/4096 bits), ECDSA (Curvas Elípticas — mayor seguridad con claves más cortas).
- **Ventaja:** resuelve el problema de distribución de claves.

### 3.3. Proceso de firma electrónica

1. Se calcula el **hash** del documento con SHA-256.
2. Se cifra el hash con la **clave privada** → firma electrónica.
3. Se envía **documento + firma + certificado digital**.
4. El receptor:
   - Obtiene la clave pública del certificado.
   - Verifica la firma.
   - Calcula nuevamente el hash del documento.
5. **Si los hashes coinciden → firma válida**, garantizando integridad + autenticidad + no repudio.

1.  El firmante calcula el **hash** (huella digital) del documento con SHA-256.
2.  Cifra el hash con su **clave privada** → resultado: **firma electrónica** del documento.
3.  Envía el documento + la firma + su certificado digital (que contiene su clave pública).
4.  El destinatario:
    *   Extrae la clave pública del certificado del firmante.
    *   Descifra la firma con la clave pública → obtiene el hash original.
    *   Calcula el hash del documento recibido.
    *   **Si ambos hashes coinciden:** La firma es válida (integridad + autenticidad + no repudio).

### 3.4. Función Hash

- Convierte datos de cualquier tamaño en una **huella digital de longitud fija**.
- **MD5:** obsoleto.
- **SHA-1:** obsoleto.
- **SHA-256 (SHA-2):** estándar vigente.
- **SHA-3:** alternativa moderna.

## 4. Certificados Digitales

### 4.1. Concepto

- **Certificado digital:** documento electrónico emitido por una **Autoridad de Certificación (CA)** que vincula la identidad del titular con su **clave pública**.
- Estándar: **X.509 v3**.

### 4.2. Contenido de un certificado X.509 v3

- **Versión.**
- **Número de serie.**
- **Algoritmo de firma.** SHA-256 con RSA
- **Emisor (Issuer):** CA que lo emite.
- **Periodo de validez.**
- **Sujeto (Subject):** identidad del titular.
- **Clave pública.** RSA 2048 bits o ECDSA
- **Extensiones:** usos, restricciones, OCSP, CRL, etc.
- **Firma de la CA.**

### 4.3. Principales prestadores en España

- **FNMT-RCM:** certificados de persona física, representante y sede electrónica.
- **DNIe:** certificados de autenticación y firma.
- **ACCV:** certificados de empleado público y sede electrónica.
- **Camerfirma:** certificados empresariales y profesionales.

* **4.4. Revocación y Seudónimos:**
  * Para comprobar si la firma es válida hoy, el Ayuntamiento usa el protocolo **OCSP** (verificación online en tiempo real), descartando las pesadas listas CRL.
  * Por seguridad (ENS), muchos Ayuntamientos expiden **Certificados de Empleado Público con Seudónimo** (ej. para Policía Local), ocultando el nombre real.

## 5. Tipos de Firma Electrónica (Clasificación eIDAS)

- **Firma electrónica simple**
    - Nivel de seguridad **básico**.
    - Datos electrónicos asociados a otros datos.
    - Ej.: nombre incluido en un correo.
    - Tiene valor probatorio, aunque menor.

- **Firma electrónica avanzada**
    - Vinculada al firmante.
    - Permite identificar al firmante.
    - Creada bajo su control exclusivo.
    - Detecta cualquier modificación posterior.
    - Mayor valor probatorio.

- **Firma electrónica cualificada**
    - **Firma avanzada + certificado cualificado + dispositivo cualificado de creación de firma (QSCD).**
    - Máximo nivel de seguridad.
    - **Equivalente legal a la firma manuscrita en toda la UE.**

### 5.1. Sistemas admitidos en la Administración (Ley 39/2015)

**Ciudadanos (Ley 39/2015):**
    - Certificado electrónico cualificado.
    - **DNIe.**
    - **Cl@ve:** PIN, Permanente y Firma.
    - Otros sistemas admitidos por la Administración.

**Actuación administrativa automatizada (Ley 40/2015):**
    - **Sello electrónico.**
    - **Código Seguro de Verificación (CSV).**
    - Portafirmas

## 6. Formatos de Firma Electrónica

- **XAdES:** basado en **XML**.
  - Firma de documentos XML y factura electrónica.

- **PAdES:** basado en **PDF**.
  - Firma de documentos PDF.

- **CAdES:** basado en **CMS/PKCS#7**.
  - Firma de ficheros binarios.

- **JAdES:** basado en **JSON**.
  - Firma de datos JSON y APIs REST.

### 6.1. Niveles de firma longeva

- **B (Basic):** firma básica.
- **T (Timestamp):** añade **sello de tiempo**.
- **LT (Long Term):** añade certificados y evidencias OCSP/CRL.
- **LTA (Long Term Archival):** añade sellos periódicos para conservación a largo plazo.

### 6.2. Plataforma @firma

- Plataforma de la **Administración General del Estado** para creación y validación de firmas electrónicas.
- Soporta formatos **XAdES, PAdES y CAdES**.
- Permite validar certificados de distintos prestadores.

## 7. Conclusión

La identificación y firma electrónica constituyen la base tecnológica y jurídica de la administración electrónica. 

- **eIDAS:** marco europeo de identificación y servicios de confianza.
- **Ley 6/2020:** marco nacional de servicios electrónicos de confianza.
- **Criptografía asimétrica:** utiliza clave pública + clave privada.
- **Certificado X.509:** vincula identidad y clave pública.
- **Firma cualificada:** equivalente legal a la manuscrita.
- **XAdES / PAdES / CAdES / JAdES:** principales formatos de firma.
- **LTA:** garantiza la conservación de la validez de la firma a largo plazo.
- **@firma:** plataforma AGE para creación y validación de firmas electrónicas.