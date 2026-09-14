# Tema 46.- Identidad y Firma (eIDAS), Criptografía, Certificados (X.509) y Formatos.

## 1. Introducción
* El papel de la firma electrónica es aportar al mundo digital las **4 garantías jurídicas**:
  1. Identificación (Quién eres).
  2. Integridad (No se ha modificado).
  3. No repudio (No puedes negar haberlo firmado).
  4. Confidencialidad (Solo lo lee el destino).

## 2. Marco Normativo (La Base Legal)
* **2.1. Marco Europeo (eIDAS - Reg. UE 910/2014):**
  * Crea el Mercado Único Digital. Principio clave: La firma electrónica *cualificada* equivale jurídicamente a la manuscrita en toda la UE.
  * Define los QTSP (Prestadores Cualificados de Servicios de Confianza).
* **2.2. Marco Nacional:**
  * **Ley 6/2020:** Regula los servicios electrónicos de confianza en España.
  * **Ley 39/2015:** Art. 9 (Sistemas de Identificación) y Art. 10 (Sistemas de Firma).
  * **Ley 40/2015:** Art. 42 y 43 (Firma por actuación automatizada y Sello de Órgano).

## 3. La Maquinaria Criptográfica
* **3.1. Criptografía Simétrica (Clave concertada):** Misma clave (ej. AES-256). Rápida, pero con el problema de cómo enviar la clave de forma segura.
* **3.2. Criptografía Asimétrica:** Un par de claves (Pública + Privada) basadas en RSA o ECDSA. 
  * *Privada:* Custodiada (Firma y descifra). *Pública:* Distribuida (Verifica y cifra).
* **3.3. El proceso de firma (El Hash):**
  1. El firmante extrae el **Hash SHA-256** (huella digital) del documento.
  2. Cifra ese Hash con su **Clave Privada** (= Firma electrónica).
  3. El receptor descifra la firma con la **Clave Pública** (obtiene el Hash 1) y calcula el Hash del documento recibido (Hash 2). Si son idénticos = Firma válida.

## 4. Certificados Digitales (X.509 v3)
* **4.1. Concepto:** Un tercero de confianza (CA) vincula matemáticamente a una persona con su Clave Pública.
* **4.2. Estructura (X.509 v3):** Emisor (Issuer), Sujeto (Subject), Validez, Clave Pública (RSA 2048) y las extensiones de validación (**OCSP/CRL**).
* **4.3. Detalle AAPP - Revocación y Seudónimos:**
  * Para comprobar si la firma es válida hoy, el Ayuntamiento usa el protocolo **OCSP** (verificación online en tiempo real), descartando las pesadas listas CRL.
  * Por seguridad (ENS), muchos Ayuntamientos expiden **Certificados de Empleado Público con Seudónimo** (ej. para Policía Local), ocultando el nombre real.

## 5. Tipos de Firma y Sistemas Admitidos
* **5.1. Tipos (eIDAS):** 
  * *Simple:* Datos asociados (Poco valor probatorio).
  * *Avanzada:* Identifica, control exclusivo y detecta cambios.
  * *Cualificada:* Avanzada + Certificado cualificado + Dispositivo cualificado (QSCD, ej. tarjeta chip). Equivale a firma manuscrita.
* **5.2. AAPP (Sistemas Admitidos):** 
  * Ciudadanos: DNIe, Certificado cualificado (FNMT) y sistemas Cl@ve.
  * Administración: Portafirmas corporativo, Sello Electrónico de Órgano y el **CSV (Código Seguro de Verificación)**, que permite imprimir un PDF y verificar su autenticidad en la Sede Electrónica.

## 6. Formatos y Evolución de la Firma
* **6.1. Formatos Base:**
  * **XAdES (XML):** Estándar para FacturaE.
  * **PAdES (PDF):** Firma incrustada visualmente en un PDF.
  * **CAdES (CMS):** Archivos binarios genéricos.
  * **JAdES (JSON):** Nuevo estándar para APIs REST.
* **6.2. La Firma Longeva (Fase de Archivo):**
  * Para que la firma valga dentro de 20 años (cuando el certificado haya caducado), se requiere el formato **LTA (Long Term Archival)**, que incrusta sellos de tiempo y evidencias OCSP iterativamente.
* **6.3. Plataforma @firma (Red SARA):** La pasarela del Estado que usan los Ayuntamientos para validar firmas y certificados.

## 7. Conclusión
El acto administrativo digital carece de validez sin el respaldo de la criptografía asimétrica y el estándar X.509 v3. Gracias al reglamento eIDAS y a herramientas nacionales como @firma y Cl@ve, los Ayuntamientos pueden orquestar plataformas (Sede Electrónica y Portafirmas) que garantizan la integridad, confidencialidad y el no repudio, transformando el papel firmado por un sello electrónico plenamente auditable en el tiempo mediante formatos longevos LTA.

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