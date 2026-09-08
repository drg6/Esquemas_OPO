# Tema 33.- La administración electrónica. Sede, Carpeta, Registro (SIR), Digitalización, Firma y Gestor Documental.

## 1. Introducción
* **Contexto legal:** Leyes 39/2015 y 40/2015. El canal electrónico no es una alternativa, es el medio nativo y obligatorio para la tramitación administrativa.
* **El Ecosistema:** Sede → Carpeta → Registro (SIR) → Firma → Digitalización → Archivo.

## 2. Concepto y Arquitectura (El Modelo Dual)
* **e-Government:** No es escanear papel, es transformar procesos con TIC para ganar eficiencia y transparencia.
* **Front-Office (Hacia el ciudadano):** Escaparate digital (Sede Electrónica, Carpeta Ciudadana, Registro Electrónico).
* **Back-Office (Maquinaria interna):** Tramitación oculta (SIR, plataforma @firma, Gestor Documental).

## 3. Sede Electrónica (Front-Office)
* **3.1. Naturaleza:** URL oficial. Lo publicado aquí tiene **plena validez jurídica** (no es una web informativa normal).
* **3.2. Requisitos (ENS):** SSL/TLS, disponibilidad 24x7, Accesibilidad WCAG 2.1 AA, hora oficial del ROA.
* **3.3. Contenido Local Clave:** Carta de servicios, Perfil del Contratante y el **Tablón de Edictos Electrónico** (sustituye al tablón físico del Ayuntamiento).
* **3.4. Sede Asociada:** Subsede para servicios específicos con requisitos más flexibles.

## 4. Carpeta Ciudadana (Front-Office)
* **Concepto:** Espacio personal (autenticado con Cl@ve/DNIe) para ver el estado de expedientes y notificaciones.
* **Integración:** El Ayuntamiento debe conectar su Gestor de Expedientes a la **Carpeta Ciudadana del Estado (PAG)** mediante la Plataforma de Intermediación de Datos (PID).

## 5. Registro e Interconexión (Front/Back-Office)
* **5.1. Registro General:** Obligatorio (Art. 16 Ley 39/2015), 24x7, emite recibo con fecha y hora oficial que marca los plazos legales.
* **5.2. SIR y SICRES:** 
  * **SIR:** La "autopista" que interconecta los registros de todas las AAPP.
  * **SICRES 4.0:** El "idioma" (Norma Técnica) que define el XML del asiento registral.
* **5.3. Herramientas (GEISER/ORVE) y OAMR:** El ciudadano sin medios va a la **OAMR (Oficina de Asistencia en Materia de Registros)**, donde el Ayuntamiento usa ORVE para digitalizar y enviar por SIR.

## 6. Firma y Sellado Electrónico (Back-Office)
* **6.1. Conceptos y eIDAS:** Garantiza Autenticidad, Integridad y No Repudio. Tres niveles eIDAS: Simple, Avanzada y Cualificada (equivalente legal a manuscrita, ej. DNIe).
* **6.2. PKI y Certificados X.509 v3:** Emitidos por prestadores cualificados (FNMT).
* **6.3. Sello de Órgano:** Para actuaciones automatizadas (ej. emitir un volante de empadronamiento masivo sin intervención humana).
* **6.4. Plataforma @firma:** Del Estado, valida formatos XAdES, CAdES, PAdES.
* **6.5. Sellado de tiempo (Timestamping) y Funcionario Habilitado:** 
  * El sello de tiempo garantiza validez a largo plazo.
  * Si el ciudadano no tiene firma, actúa el **Funcionario Habilitado** firmando por él con consentimiento expreso.

## 7. Digitalización Certificada
* **Concepto:** Pasar de papel a digital con idéntica validez legal (fiel al original, >200ppp).
* **Proceso OAMR:** Ciudadano entrega papel → Funcionario escanea → Añade metadatos ENI → Firma electrónicamente → **Devuelve el papel original al ciudadano** → Se destruyen copias innecesarias.

## 8. Gestor Documental y Archivo
* **8.1. Gestor Documental (DMS):** Controla el documento "vivo" (metadatos, versionado, flujos).
* **8.2. Archivo Electrónico Único (Ley 40/2015):** Destino del expediente cerrado. Garantiza la conservación a largo plazo aplicando resellado criptográfico para mantener las **firmas longevas** (AdES-LTA).

## 9. Conclusión
El ecosistema de administración electrónica conforma una maquinaria perfecta donde el ciudadano interactúa a través de la Sede y la Carpeta, el Registro captura la entrada, la OAMR digitaliza certificadamente, @firma garantiza el valor legal, SIR transporta el dato y el Gestor Documental asegura su preservación perpetua bajo el paraguas del ENI y el ENS.

--------------------------------------

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

### 3.3. Sede Electrónica Asociada (RD 203/2021):

  * Portal vinculado a la Sede matriz para servicios o departamentos específicos.
  * ⚠️ *Clave:* Tiene URL propia, pero mantiene **exactamente las mismas garantías jurídicas y de seguridad** que la sede principal.

### 3.4. Servicios y Contenidos Clave (Obligatorios en un Ayuntamiento):

  * **Tablón de Edictos Electrónico:** Sustituye al tablón de corcho físico. Utiliza **sellos de tiempo** para dar fehaciencia legal a los plazos de exposición de bandos y edictos.
  * **Perfil del Contratante:** Garantiza la transparencia y libre concurrencia en la contratación pública (licitaciones, adjudicaciones).
  * **Carta de Servicios:** Documento público que fija los compromisos de calidad y los derechos del ciudadano.

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

- Obligatorio según **art. 16 Ley 39/2015**.
- Funciona **24×7**.
- Permite presentar documentos dirigidos a cualquier AAPP.
- Emite **recibo electrónico** con fecha, hora, asiento y documentos.
- Los plazos se calculan según la **fecha y hora oficial**.

### 5.2. Interconexión de Registros: SIR y SICRES

- **SIR (Sistema de Interconexión de Registros):** interconecta registros para intercambiar **asientos registrales**.
- **SICRES (Sistema de Información Común de Registros de Entrada y Salida):** Norma técnica que define el **formato y estructura** de los asientos intercambiados. Metadatos obligatorios, documentos, firmas, etc.

### 5.3. Herramientas (GEISER/ORVE) y OAMR

- **GEISER:** gestión del Registro Electrónico General + integración con SIR.
- **ORVE (Oficina de Registro Virtual):** digitalización y remisión de documentos en papel mediante SIR.
- El ciudadano sin medios va a la **OAMR (Oficina de Asistencia en Materia de Registros)**, donde el Ayuntamiento usa ORVE para digitalizar y enviar por SIR.

## 6. Firma y Sellado Electrónico

### 6.1. Firma electrónica: Concepto y tipos

- Garantiza **autenticidad (identificación del firmante) + integridad (el documento no ha sido alterado) + no repudio (el firmante no puede negar la firma)**.
- **Simple:** garantía mínima. Ej. Nombre escrito en email
- **Avanzada:** vinculada al firmante y detecta modificaciones. Ej. Firma con clave privada en dispositivo de usuario
- **Cualificada:** máxima garantía y equivalente legal a la manuscrita. Ej. Certificado FNMT o DNIe

### 6.2. Certificado electrónico X.509 v3

- PKI (infraestructura de clave pública) basada en certificados **X.509 v3**.
- Principales: **FNMT-RCM** y **DNIe** con dos certificados (autenticación y firma).

### 6.3. Sello electrónico de órgano

- Identifica a un **órgano/persona jurídica**, no a una persona física.
- Permite **firma automatizada** de actos administrativos (liquidaciones tributarias, certificados de padrón).

### 6.4. Plataforma @firma

- Plataforma de la AGE para **validar certificados y firmas**.
- Soporta **XAdES, PAdES y CAdES**.

### 6.5. Sellado de tiempo (Timestamp)

### 6.5. Sellado de tiempo (Timestamp) y Funcionario Habilitado:** 
- Acredita que un documento **existía en un momento determinado** y no ha sido modificado.
- Emitido por una Autoridad de Sellado de Tiempo (TSA)
- Importante para la **validez a largo plazo**.
- Si el ciudadano no tiene firma, actúa el **Funcionario Habilitado** firmando por él con consentimiento expreso.

## 7. Digitalización Certificada

### 7.1. Concepto

- Convierte papel en documento electrónico con **validez legal equivalente**.
- Requisitos principales:
  - Imagen fiel (resolución mínima de 200 ppp).
  - **Metadatos obligatorios (NTI de Documento Electrónico del ENI)**.
  - Firma electrónica o sello de órgano.
  - Formato **PDF/A** o equivalente.

### 7.2. Proceso

Escanear → Metadatos obligatorios → Firmar/Sellar → Expediente → Destrucción del papel (si procede)

## 8. Gestor Documental

### 8.1. Concepto en el contexto de la Administración Electrónica

- Gestiona el **ciclo de vida completo** del documento electrónico:
  - Captura + metadatos.
  - Clasificación + expediente.
  - Firma y sellado.
  - Versionado + trazabilidad.
  - Conservación a largo plazo (archivo electrónico).
  - Archivo histórico.
  - Eliminación según calendarios.

### 8.2. Archivo Electrónico Único

- La Ley 40/2015 establece la obligación de un **Archivo Electrónico Único**
- Garantiza **conservación, recuperación y acceso** a documentos y expedientes durante todo su ciclo de vida.
- Incluye conservación a largo plazo y **firmas longevas**.

## 9. Conclusión

La administración electrónica se materializa a través de un ecosistema de instrumentos técnico-jurídicos interconectados:

- **Sede:** acceso jurídico a servicios.
- **Carpeta:** información y trámites del ciudadano.
- **Registro + SIR/SICRES:** presentación e intercambio.
- **Firma/Sello:** autenticidad e integridad.
- **Digitalización:** papel → electrónico con validez legal.
- **Gestor documental:** ciclo de vida del documento conforme al ENI.

Piedra angular de la modernización tecnológica de las Administraciones Públicas.