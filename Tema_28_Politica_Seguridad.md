# Tema 28.- Política de Seguridad. Diseño, aprobación y aplicación.

## 1. Introducción
La seguridad no es un problema puramente tecnológico del departamento de informática, sino un instrumento de **gobierno organizacional** que debe emanar de la máxima autoridad de la entidad y vincular a todo su personal.

## 2. Diseño de la Política de Seguridad
- **2.1. Naturaleza:** Documento fundacional de máximo nivel que articula las directrices de protección de los sistemas, exigido por el ENS (RD 311/2022). Define el "qué" y el "por qué", pero no el "cómo".
- **2.2. Contenido obligatorio (Estricto Art. 12.1 ENS):** 
  1. Misión y objetivos institucionales.
  2. Marco regulatorio (ENS, RGPD, LOPDGDD, Ley 39/2015, Ley 40/2015).
  3. Roles de seguridad (deberes, designación y renovación).
  4. Comités de seguridad (estructura y responsabilidades).
  5. Documentación (directrices de gestión y acceso).
  6. Riesgos de datos personales.
- **2.3. Organización y Roles (Art. 11 y 13):**
  * *Responsables:* Información, Servicio, Seguridad (CISO) y Sistema.
  * *Comité de Seguridad:* Órgano colegiado de coordinación directiva.
  * ⚠️ *Diferenciación de responsabilidades:* CISO ≠ Responsable del Sistema (sin dependencia jerárquica).
  * *DPD:* Supervisión RGPD (colaborador, no es rol estructural ENS).
- **2.4. Desarrollo (Art. 12.6 y Guía 805):** Aplicación de gestión de riesgos (MAGERIT), plan de concienciación, gestión de incidentes y revisión periódica.
- **2.5. Proceso de diseño:** Análisis de contexto → Definición de objetivos → Asignación de roles → Redacción clara → Revisión por el Comité.

## 3. Aprobación de la Política de Seguridad

* **3.1. Requisito de Aprobación al máximo nivel:** 
  * Un documento firmado por el Jefe de Informática no tiene validez legal frente a terceros o empleados.
  * En la Administración Local, debe aprobarse por la máxima autoridad: el **Pleno del Ayuntamiento**, la **Junta de Gobierno Local** o **Alcaldía** (según delegación). En la AGE (Subsecretaría, Secretaría General).

* **3.2. Publicación y Difusión:**
  * Publicación formal (Boletín Oficial de la Provincia - BOP, Portal de Transparencia).
  * Comunicación interna obligatoria a empleados públicos y empresas proveedoras (cláusulas contractuales).

* **3.3. Revisión y Actualización:** Ordinaria (anual) o Extraordinaria (cambios normativos, migración tecnológica grave, ciberataque).

## 4. Aplicación: La Pirámide Documental (El SGSI)
Para que la Política (abstracta) se aplique en el día a día, se despliega un cuerpo documental jerárquico:

* **Nivel 1: La Política de Seguridad.** (Directrices generales aprobadas por el Pleno/Alcaldía).
* **Nivel 2: Normativas de Seguridad.** Reglas de obligado cumplimiento aprobadas por el CISO. 
  * *Ejemplos:* Teletrabajo, Uso de Medios Informáticos, Escritorio Limpio, Control de Accesos (Contraseñas).
* **Nivel 3: Procedimientos (SOPs - Standard Operating Procedure).** Instrucciones técnicas paso a paso ejecutadas por el personal TIC.
  * *Ejemplos:* Procedimiento de alta/baja en Active Directory, Políticas de Backups.
* **Nivel 4: Evidencias y Registros.** Pruebas de cumplimiento auditable (Logs de firewall, actas de formación, contratos).

## 5. Aplicación: Declaración de Aplicabilidad (SoA)
* **Concepto:** Documento maestro que lista los 73 controles del Anexo II del ENS.
* **Función:** Define para cada medida si aplica o no aplica (justificándolo) y cómo se implementa. Es el mapa para las auditorías externas y la base para reportar anualmente el **INES (Informe Nacional del Estado de Seguridad)** al CCN.

## 6. Aplicación: Concienciación y Cultura
* La política fracasa si el usuario (el eslabón más débil) no la conoce.
* El ENS exige planes de formación continua, campañas de concienciación (phishing ético) y firma de compromisos de confidencialidad de los empleados.

## 7. Conclusión
El éxito de una Política de Seguridad municipal no se mide por su complejidad técnica, sino por su respaldo institucional. Un **diseño** alineado con el ENS (roles segregados), una **aprobación** formal que garantice su autoridad, y una **aplicación** estructurada mediante la pirámide documental y la concienciación, son los pilares que transforman un papel burocrático en un escudo real para los datos del ciudadano.

------------

# Tema 28.- Política de Seguridad. Diseño, aprobación y aplicación.

## 1. Introducción
La seguridad no es un problema puramente tecnológico del departamento de informática, sino un instrumento de **gobierno organizacional** que debe emanar de la máxima autoridad (Pleno) de la organización y vincular a todo su personal.

## 2. Diseño de la Política de Seguridad
La Política de Seguridad es el documento fundacional de máximo nivel que articula las directrices de protección de los sistemas de información, exigido por el ENS (RD 311/2022). Define el "qué" y el "por qué", pero no el "cómo".

* **2.1. Contenido obligatorio (Art. 12 ENS y la guía CCN-STIC 805):**
  * Misión y objetivos institucionales.
  * Marco regulatorio (ENS, RGPD, LOPDGDD, Ley 39/2015 (LPACAP) y Ley 40/2015 (LRJSP)).
  * Gestión de riesgos (MAGERIT).
  * Directrices de formación y concienciación.
  * Gestión de incidentes de seguridad
  * Proceso de revisión y actualización

* **2.2. Estructura de Roles (La clave del diseño):**
  * *Responsable de Información / Responsable del Servicio* (Negocio).
  * *Responsable de Seguridad (CISO)* y *Comité de Seguridad*.
  * *Responsable del Sistema* (Informática).
  * *Delegado de Protección de Datos (DPD/DPO)*

  ⚠️ *Principio de segregación de funciones:* Evitar ser juez y parte.
  
* **2.3. Proceso de elaboración:** Liderado por el Comité de Seguridad, requiere un análisis de contexto, definición de objetivos e identificación de roles y responsabilidades.

## 3. Aprobación de la Política de Seguridad

* **3.1. Requisito de Aprobación al máximo nivel:** 
  * Un documento firmado por el Responsable de Informática no tiene validez legal frente a terceros o empleados.
  * En la Administración Local, debe aprobarse por la máxima autoridad: el **Pleno del Ayuntamiento**, la **Junta de Gobierno Local** o **Alcaldía** (según delegación de competencias). En la AGE (Subsecretaría, Secretaría General)

* **3.2. Publicación y Difusión:**
  * Publicación formal (Boletín Oficial de la Provincia - BOP, Portal de Transparencia).
  * Comunicación interna obligatoria a todos los empleados públicos y empresas proveedoras.

* **3.3. Revisión y Actualización:** Ordinaria (anual) o Extraordinaria (cambios normativos, migración tecnológica grave, ciberataque).

## 4. Aplicación: La Pirámide Documental (El SGSI)
Para que la Política (abstracta) se aplique en el día a día, se despliega un cuerpo documental jerárquico:

* **Nivel 1: La Política de Seguridad.** (Directrices generales).

* **Nivel 2: Normativas de Seguridad.** Reglas de obligado cumplimiento aprobadas por el CISO. 
  * *Ejemplos:* Normativa de Teletrabajo, Normativa de Uso de Medios Informáticos, Política de Escritorio Limpio, Control de Accesos (Contraseñas).

* **Nivel 3: Procedimientos (SOPs).** Instrucciones técnicas paso a paso para el personal TIC.
  * *Ejemplos:* Procedimiento de alta/baja de usuarios en Active Directory, Procedimiento de copias de seguridad (Backups).

* **Nivel 4: Evidencias y Registros.** Pruebas de cumplimiento (Logs, actas de formación, contratos de confidencialidad).

## 5. Aplicación: Declaración de Aplicabilidad (SoA)
* **Concepto:** Documento maestro que lista los 73 controles del Anexo II del ENS.
* **Función:** Define para cada medida si aplica o no aplica (justificándolo) y cómo se implementa. Es el mapa para las auditorías externas y la base para reportar anualmente el **INES (Informe Nacional del Estado de Seguridad)** al CCN.

## 6. Aplicación: Concienciación y Cultura
* La política fracasa si el usuario (el eslabón más débil) no la conoce.
* El ENS exige planes de formación continua, campañas de concienciación (phishing ético) y firma de compromisos de confidencialidad por parte de los funcionarios.

## 7. Conclusión
El éxito de una Política de Seguridad en un Ayuntamiento no se mide por su complejidad técnica, sino por su respaldo institucional. Un **diseño** alineado con el ENS, una **aprobación** formal por el Pleno que garantice su autoridad, y una **aplicación** estructurada mediante normativas y concienciación, son los pilares que transforman un documento burocrático en un escudo real para los datos del ciudadano.