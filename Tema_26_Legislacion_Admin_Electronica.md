# Tema 26.- La legislación en materia de sociedad de la información y administración electrónica. Impacto en los Sistemas de Información.

## 1. Introducción

La e-Administración no es opcional, es un mandato legal. El marco normativo (europeo y nacional) impone el deber de relacionarse electrónicamente con ciudadanos y entre organismos.
El impacto TIC es transversal: obliga a digitalizar expedientes, crear Sedes Electrónicas, garantizar interoperabilidad (ENI), accesibilidad y seguridad (ENS).

## 2. Marco Legislativo de la e-Administración
### 2.1. Normativa Europea
* **Reglamento eIDAS (UE 910/2014):** Marco europeo de Identificación y Servicios de Confianza. Equipara jurídicamente la firma electrónica cualificada a la manuscrita.
* **Directiva de Accesibilidad (Transpuesta por RD 1112/2018):** Exige que webs/apps públicas cumplan el **Nivel AA de WCAG 2.1**.

### 2.2. Normativa Nacional (El Ecosistema Legal TIC)
* **Ley 34/2002 (LSSICE):** Ley base de la sociedad de la información (Obligaciones, contratos web, cookies, spam/opt-in).
* **Ley 6/2020 de Servicios Electrónicos de Confianza:** Complementa a eIDAS en España (Prestadores cualificados como FNMT, ACCV).

### 2.3. El Núcleo de la Administración Electrónica (Las Leyes de 2015)
* **Ley 39/2015 (Procedimiento Administrativo Común - Relación *Hacia Afuera*):**
  * **Sujetos Obligados:** Empresas, profesionales colegiados y empleados públicos obligados a relacionarse electrónicamente. Para ciudadanos (físicos) es voluntario.
  * **Expediente Electrónico:** Formato nativo de tramitación.
  * **Registro Electrónico:** 24x7x365.
  * **Notificaciones Electrónicas:** Obligatorias vía DEHú.
* **Ley 40/2015 (Régimen Jurídico del Sector Público - Relación *Hacia Adentro*):**
  * **Sede Electrónica:** Portal web oficial con garantías de integridad y autenticidad.
  * **Principio *Once Only*:** Prohibición de pedir al ciudadano datos que ya obren en poder de la AAPP.
  * **Sello Electrónico y CSV:** Para actuaciones administrativas automatizadas.
* **RD 203/2021 (Reglamento de actuación electrónica):** Desarrolla las leyes de 2015 (PAGe, Carpeta Ciudadana, SIR).

### 2.4. Los Esquemas Nacionales (Reales Decretos)
* **RD 4/2010 (ENI - Interoperabilidad):** Condiciones (formatos, metadatos) para intercambiar información entre administraciones.
* **RD 311/2022 (ENS - Seguridad):** Controles obligatorios para proteger la información.

## 3. Impacto de su Aplicación en los Sistemas de Información (Arquitectura AAPP)
El marco legal obliga a rediseñar la arquitectura de los Ayuntamientos integrando las plataformas habilitadoras del Estado:

* **3.1. Identificación y Firma Electrónica (Ley 39 y eIDAS):**
  * Los portales deben integrar la plataforma **Cl@ve** (PIN, Permanente, DNIe) y **@firma** para validación de firmas.
  * Uso de firmas longevas (XAdES, PAdES) para preservar validez en el tiempo.
* **3.2. Interoperabilidad de Registros y Datos (Ley 40 y ENI):**
  * **Red SARA:** Intranet de las Administraciones para comunicación segura.
  * **SIR (Sistema de Interconexión de Registros):** Mediante norma SICRES 4.0.
  * **PID (Plataforma de Intermediación de Datos):** Para cumplir el *Once Only* (consultas de padrón, IRPF, estar al corriente de pago) en lugar de pedir papeles al ciudadano.
* **3.3. Notificaciones Electrónicas:**
  * Integración automatizada del back-office municipal con la **DEHú** (Dirección Electrónica Habilitada Única) y el servicio **Notifica**. Gestión del rechazo automático a los 10 días.
* **3.4. Archivo Electrónico Único:**
  * Implementación de sistemas (ej. proyecto *Archive*) para preservar expedientes a largo plazo (metadatos del ENI y resellados de firma).
* **3.5. Accesibilidad Universal (RD 1112/2018):**
  * Los portales deben pasar auditorías de la metodología IRA (Informe de Revisión de Accesibilidad) garantizando lectura por pantalla, contraste y navegación por teclado.

## 4. Conclusión
El conjunto normativo (Leyes 39/40, ENI, ENS, eIDAS) ha enterrado el expediente de papel. Su impacto en los sistemas de información locales es radical: el Ayuntamiento ya no es una isla, sino un nodo interconectado que debe consumir plataformas estatales (SIR, PID, Cl@ve) para ofrecer un servicio 24/7, accesible, seguro y centrado en el ciudadano (Once Only). No adoptar estos sistemas no es un mero retraso tecnológico, es un incumplimiento legal.