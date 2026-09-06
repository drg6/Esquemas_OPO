# Tema 31.- Interoperabilidad y sistemas de cooperación entre AAPP. Políticas UE, ENI, NTI e Intercambio de datos.

## 1. Introducción
* **Contexto:** 8.100 AAPP con sistemas heterogéneos y aislados.
* **Concepto:** Interoperabilidad es la capacidad de intercambiar datos y procesos de forma automática y con valor jurídico.
* **El Motor Legal:** **Art. 28.2 Ley 39/2015** (Principio *Once Only*). El ciudadano tiene derecho a NO aportar documentos que ya tenga la Administración.

## 2. Política de la UE en Interoperabilidad
* **2.1. EIF (Marco Europeo de Interoperabilidad):** 47 recomendaciones en 4 capas (Jurídica, Organizativa, Semántica, Técnica).
* **2.2. Normativa Europea:**
  * **Reglamento eIDAS (910/2014):** Identidad y servicios de confianza.
  * **Reglamento Pasarela Digital Única (2018/1724):** *Your Europe* (Sistema Técnico Once Only - OOTS).
  * **Reglamento Europa Interoperable (2024/903):** Crea el Comité de Europa Interoperable y obliga a realizar *Evaluaciones de Interoperabilidad*.
  * **Directiva Open Data (2019/1024):** Reutilización de información.
* **2.3. Programas de financiación:** ISA² → relevado por **Digital Europe**.

## 3. Normativa Nacional (ENI vs ENS)
* **ENI (RD 4/2010):** Regula el intercambio de datos. (Garantiza que *se puedan* comunicar).
* **ENS (RD 311/2022):** Regula la seguridad. (Garantiza que se comuniquen *sin riesgos*).
* Ambos son obligatorios (AGE, CCAA, Entidades Locales).

## 4. Dimensiones de la Interoperabilidad (Las 4 Capas)
1. **Jurídica:** Leyes, convenios de cesión de datos y RGPD (minimización).
2. **Organizativa:** Acuerdos de Nivel de Servicio (SLA), procesos de negocio y responsables.
3. **Semántica (El gran reto):** Que los datos signifiquen lo mismo. 
   * *Soluciones:* Vocabularios comunes y código unívoco de oficina **DIR3**.
4. **Técnica:** Conectividad física y lógica (REST, XML, Red SARA).

## 5. Normas Técnicas de Interoperabilidad (NTI)
Desarrollan técnicamente el ENI mediante especificaciones concretas.
* *Mnemotecnia:* **C**uando **D**escargas **E**xpedientes **D**igitales **F**irmados, **I**ntenta **M**antener **G**ran **R**igor **C**on **R**esponsabilidad.
* (Catálogo de estándares, Documento, Expediente, Digitalización, Firma, Intermediación PID/SCSP, Modelos de datos, Gestión documental, Red SARA, Copiado auténtico, Reutilización).

## 6. Intercambio de Datos y Sistemas de Cooperación
La SGAD provee las infraestructuras que los Ayuntamientos consumen:
* **6.1. Red SARA:** Intranet cifrada de las AAPP (Tecnología MPLS). 
* **6.2. PID (Plataforma de Intermediación de Datos):**
  * Materializa el *Once Only*.
  * Usa el protocolo **SCSP** (Sustitución de Certificados en Soporte Papel) en formato XML.
  * *Servicios top:* Identidad (DGP), Padrón (INE), Estar al corriente de AEAT y Seguridad Social.
* **6.3. Servicios Comunes Habilitadores:**
  * **Cl@ve:** Identidad (PIN, Permanente, DNIe).
  * **@firma:** Validación de firmas (XAdES, PAdES) y sellado de tiempo.
  * **SIR / GEISER:** Interconexión de Registros.
  * **Notific@ / DEHú:** Notificaciones electrónicas.
  * **Archive / INSIDE:** Archivo y gestión de expedientes.

## 7. Conclusión
La interoperabilidad ha destruido la barrera física de la ventanilla. Apoyada por el marco EIF europeo y orquestada en España por el ENI y las NTI, exige que los Ayuntamientos modernicen sus arquitecturas para integrarse en la Red SARA. Consumir servicios como la PID, SIR o Cl@ve no es un lujo técnico, es la única vía legal para garantizar el derecho del ciudadano a no ser el mensajero de sus propios papeles.

-----------------------

# Tema 31.- Interoperabilidad y sistemas de cooperación entre Administraciones Públicas. Política de la Unión Europea y normativa al respecto. El Esquema Nacional de Interoperabilidad. Dimensiones de la interoperabilidad. Las Normas Técnicas de Interoperabilidad. Intercambio de datos entre Administraciones Públicas.

## 1. Introducción

- La Administración Pública española está formada por múltiples administraciones con sistemas de información heterogéneos.
- **Interoperabilidad:** capacidad de las AAPP para **intercambiar datos y procesos** de forma automática, transparente y con valor jurídico.
- Es un **mandato legal** → art. 28.2 Ley 39/2015: el ciudadano no debe aportar datos que ya obren en poder de una Administración.

## 2. Política de la Unión Europea en Materia de Interoperabilidad

### 2.1. El Marco Europeo de Interoperabilidad (EIF)

- **EIF:** marco europeo para la interoperabilidad de las AAPP.
- **47 recomendaciones** y 4 capas:
  - Jurídica.
  - Organizativa.
  - Semántica.
  - Técnica.
- Principios: **reutilización, apertura, transparencia y orientación al ciudadano**.

### 2.2. Directivas y Reglamentos europeos relevantes

- **Reglamento 2018/1724** → Pasarela Digital Única / **Your Europe** → principio **once only**.
- **eIDAS 910/2014** → identificación electrónica y servicios de confianza (Firma electrónica, Sello electrónico, Sellado de tiempo)) entre estados miembros.
- **Directiva 2019/1024** → datos abiertos y reutilización.
- **Reglamento 2024/903** → Crea el **Comité de Europa Interoperable**  y obliga a realizar *Evaluaciones de Interoperabilidad*.

### 2.3. Programa ISA² y su sucesor

- **ISA²** → soluciones reutilizables de interoperabilidad.
- Sucesor → **Digital Europe (2021-2027)**.

## 3. Normativa Nacional sobre Interoperabilidad

### 3.1. Marco jurídico general

- **Ley 40/2015, art. 156** → establece el **ENI**.
- **Ley 39/2015, art. 28.2** → derecho a no aportar documentos ya disponibles.
- **RD 4/2010** → regula el ENI. Actualizado por el **RD 311/2022**, lo alinea con el EIF europeo.

### 3.2. Relación con el Esquema Nacional de Seguridad (ENS)

- **ENI → interoperabilidad**.
- **ENS → seguridad** del intercambio (DICAT).
- Son **complementarios**.

## 4. El Esquema Nacional de Interoperabilidad (ENI)

### 4.1. Definición y objeto

- Marco que establece **principios, criterios y recomendaciones** para garantizar la interoperabilidad de los sistemas de las AAPP.

### 4.2. Principios del ENI

- **Interoperabilidad integral** → no solo técnica.
- **Multidimensional**.
- **Soluciones multilaterales y reutilizables**.
- Evitar desarrollos duplicados.

### 4.3. Ámbito de aplicación

- **AGE**.
- **CCAA**.
- **Administración Local**.
- Organismos públicos y entidades de Derecho Público vinculados.

## 5. Dimensiones de la Interoperabilidad

### 5.1. Interoperabilidad Jurídica (Legal Interoperability)

- Garantiza el **valor jurídico** del intercambio.
- Base legal + protección de datos + reconocimiento de documentos y firmas.

### 5.2. Interoperabilidad Organizativa (Organisational Interoperability)

- Coordina **procesos (qué, quién, a quién), responsabilidades, plazos y niveles de servicio (SLA)** entre Administraciones.

### 5.3. Interoperabilidad Semántica (Semantic Interoperability)

- Garantiza que los datos tengan el **mismo significado** para todos.
- Modelos de datos, vocabularios comunes y **DIR3**. Ej. dirección vivienda
- Detalle Local: Los Ayuntamientos utilizan un Nodo de Interoperabilidad Local para traducir sus bases de datos antiguas (legacy) al modelo de datos estándar que exige el Estado.

### 5.4. Interoperabilidad Técnica (Technical Interoperability)

- Garantiza la **conectividad y comunicación** entre sistemas.
- Protocolos (HTTPS, REST), formatos (JSON, CSV), firmas electrónicas, **Red SARA** y servicios (Dehú, PID).

## 6. Las Normas Técnicas de Interoperabilidad (NTI)

### 6.1. Concepto

- Desarrollan técnicamente el **ENI**.
- Establecen especificaciones concretas para documentos, expedientes, firmas, datos, formatos, etc.

### 6.2. Catálogo de NTI vigentes

- **Catálogo de estándares**.
- **Documento electrónico**.
- **Expediente electrónico**.
- **Digitalización de documentos**.
- **Firma electrónica y certificados**.
- **Intermediación de datos (PID)** Especificaciones técnicas (SCSP).
- **Modelos de datos**.
- **Gestión de documentos electrónicos**.
- **Red SARA**.
- **Copiado auténtico y conversión**.
- **Reutilización de información**.

"Cuando Descargas Expedientes Digitales Firmados, Intenta Mantener Gran Rigor Con Responsabilidad." (C-D-E-D-F-I-M-G-R-C-R)

## 7. Intercambio de Datos entre Administraciones Públicas

### 7.1. Red SARA (Sistema de Aplicaciones y Redes para las Administraciones)

- Red que **interconecta las AAPP**.
- Proporciona comunicación segura y acceso a servicios comunes (Cl@ve, @firma, GEISER, FACE, PID).

### 7.2. Plataforma de Intermediación de Datos (PID)

- Permite consultar datos de otras Administraciones.
- Evita que el ciudadano aporte documentos ya disponibles.
- Utiliza **SCSP (Sustitución de Certificados en Soporte Papel)**.
- Garantiza la **trazabilidad** de las consultas.
- Detalle Local: Para no programar XML a mano, el Ayuntamiento despliega un Bus de Integración que conecta automáticamente su Gestor de Expedientes con la PID.
- Servicios de la PID: verificación de identidad, residencia, obligaciones tributarias y con la Seguridad Social, datos catastrales, títulos universitarios, prestaciones por desempleo y grado de discapacidad.

### 7.3. Cl@ve: Sistema de Identificación Electrónica

- Identificación, autenticación y firma electrónica.
- **Cl@ve PIN, Cl@ve Permanente y certificado/DNIe**.

### 7.4. @firma: Plataforma de Validación de Firma Electrónica

- Valida certificados y firmas electrónicas.
- Formatos: **XAdES, CAdES y PAdES**.
- Permite **sellado de tiempo**.

### 7.5. Otros servicios comunes de interoperabilidad

- **SIR (Sistema de Interconexión de Registros)** → intercambio de registros.
- **GEISER/ORVE** → registro electrónico.
- **FACE** → facturación electrónica.
- **INSIDE** → documentos y expedientes.
- **Archive** → archivo electrónico.
- **Notific@** → notificaciones.
- **DIR3** → directorio de unidades y oficinas.
- **PAe** → portal de administración electrónica.

### 7.6. Estándares de formatos documentales (NTI de Catálogo de Estándares)

- **Texto:** PDF/A, ODF, OOXML.
- **Imagen:** JPEG, PNG, TIFF, SVG.
- **Datos:** XML, JSON, CSV.
- **Firma:** XAdES, CAdES, PAdES.

## 8. Conclusión

- **ENI → garantiza la interoperabilidad.**
- **NTI → concretan cómo aplicarla.**
- **Red SARA → conecta las Administraciones.**
- **PID + SCSP → permiten intercambiar datos.**
- **Servicios comunes → identificación, firma, registro, notificación, etc.**
- Objetivo final → **simplificar la Administración y evitar que el ciudadano aporte datos que ya posee otra Administración**.