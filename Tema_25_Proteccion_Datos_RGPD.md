# Tema 25.- Protección de datos de carácter personal. Normativa. Adaptación de aplicaciones y entornos a los requisitos de la normativa legal. La Agencia Española de Protección de Datos.

## 1. Introducción: Protección de Datos de Carácter Personal

La protección de datos de carácter personal -> derecho fundamental (art. 18.4 Constitución Española), que encomienda al legislador limitar el uso de la informática para garantizar el honor y la intimidad de los ciudadanos. 
España con la Ley Orgánica 3/2018, de Protección de Datos Personales y garantía de los derechos digitales (LOPDGDD) adapta y completa el Reglamento General de Protección de Datos (RGPD) en 2018 de la Unión Europea.

AP y las empresas volúmenes masivos de información personal -> esencial marco normativo para preservar la privacidad de los individuos y garantizar sus derechos.

## 2. Normativa

### 2.1. Reglamento General de Protección de Datos (RGPD)

El Reglamento (UE) 2016/679 (RGPD) aplicable Estados miembros desde el 25 de mayo de 2018. 

- Basado en el **riesgo** y en la **responsabilidad proactiva** (accountability).
- Ampliación de los derechos de los interesados (portabilidad, limitación, supresión).
- Obligación de notificar las brechas de seguridad en 72 horas.
- Régimen sancionador reforzado (multas de hasta 20 millones de euros o el 4% del volumen de negocio anual).
- Delegado de Protección de Datos (DPO).

### 2.2. LOPDGDD (Ley Orgánica 3/2018)

La LOPDGDD adapta el ordenamiento jurídico español al RGPD y garantiza los derechos digitales de los ciudadanos (Título X). Aportaciones:

- Regulación de los datos de personas fallecidas (derecho de acceso por herederos).
- Edad mínima de consentimiento digital del menor: **14 años** (el RGPD permite entre 13 y 16).
- Derechos digitales: desconexión digital, educación digital, protección de menores en Internet, testamento digital.

### 2.3. Principios fundamentales del tratamiento (art. 5)

1. **Licitud, lealtad y transparencia** → tratamiento legal e información clara.
2. **Limitación de finalidad** → datos para fines concretos y legítimos.
3. **Minimización** → solo los datos necesarios.
4. **Exactitud** → datos correctos y actualizados.
5. **Conservación** → no conservar más tiempo del necesario.
6. **Integridad y confidencialidad** → garantizar la seguridad.
7. **Responsabilidad proactiva** → demostrar el cumplimiento.

**Mnemotecnia:**  
**Legal → Finalidad → Mínimos → Exactos → Tiempo → Seguridad → Demostrar**
"Legalmente, para una finalidad, los mínimos y exactos, durante el tiempo necesario, seguros y demostrables."

### 2.4. Bases de legitimación del tratamiento (art. 6)

- **Consentimiento** → el interesado acepta.
- **Contrato** → necesario para ejecutar un contrato.
- **Obligación legal** → exigido por una ley.
- **Intereses vitales** → proteger vida o integridad.
- **Interés público / poderes públicos** → función pública (**habitual en AAPP**).
- **Interés legítimo** → interés del responsable (**no AAPP en sus funciones**).

**AAPP:** Se basan casi exclusivamente en **Obligación Legal** e **Interés Público**, debido al desequilibrio de poder (el ciudadano no consiente, está obligado).

🧠 **Mnemotecnia:**  
**Consentimiento → Contrato → Ley → Vida → Público → Legítimo**

### 2.5. Derechos de los interesados: SOPLAR 

- **S → Supresión** → eliminar datos → «derecho al olvido».
- **O → Oposición** → oponerse al tratamiento.
- **P → Portabilidad** → recibir y trasladar los datos (CSV, JSON).
- **L → Limitación** → restringir el tratamiento.
- **A → Acceso** → saber qué datos se tratan y obtener copia.
- **R → Rectificación** → corregir datos incorrectos.

**Ejercicio gratuito.**

### Plazos
- **Regla general:** 1 mes.
- **Acceso:** puede ampliarse **2 meses más** por complejidad.

### 2.6. Obligaciones de los responsables y encargados del tratamiento

- **Delegado de Protección de Datos (DPO)** → obligatorio en las **AAPP**.
  - Informa, asesora, supervisa y coopera con la AEPD.

- **Evaluación de Impacto en Protección de Datos (EIPD)** → cuando existe **alto riesgo** para los derechos.
  - Ej.: perfiles, decisiones automatizadas, datos especiales a gran escala o videovigilancia.

- **Brechas de seguridad** → notificar a la **AEPD en ≤ 72 h**.
  - Alto riesgo → informar también a los afectados.

- **Registro de actividades** → documenta **qué datos, finalidad, destinatarios, plazos y seguridad**.

### 2.7. Conservación de datos de comunicaciones electrónicas — Ley 25/2007

- **Qué:** metadatos, **no contenido**.
- **Plazo:** máximo **12 meses**.
- **Finalidad:** delitos graves y seguridad nacional.
- **Acceso:** autoridades competentes + **autorización judicial**.

## 3. Adaptación de Aplicaciones y Entornos a los Requisitos de la Normativa Legal (RGPD/LOPDGDD)

Por mandato de la Disp. Adicional 1ª de la LOPDGDD, las AAPP cumplen la seguridad del RGPD implantando el **Esquema Nacional de Seguridad (ENS)**

- **Privacidad desde el diseño** → protección desde el desarrollo.
- **Privacidad por defecto** → máxima privacidad activada por defecto.
- **Control de acceso basado en roles (RBAC):** → acceso según el rol y función.
- **Cifrado** → En reposo (AES-256, BitLocker, TDE - Transparent Data Encryption o Cifrado de Datos Transparente en Oracle) y en tránsito (TLS 1.2 o superior).
- **Seudonimización / anonimización** → reducir la identificación de personas. Códigos reversibles (seudonimización) o irreversibles (anonimización)
- **Auditoría y trazabilidad** → registrar quién, qué y cuándo accede.
- **Gestión del consentimiento** → registrar y permitir revocarlo.
- **Derecho al olvido** → eliminar los datos de forma efectiva.
- **Evaluación de impacto** → herramientas como PILAR y GESTIONA EIPD.

## 4. La Agencia Española de Protección de Datos (AEPD)

### 4.1. Naturaleza y funciones

La **AEPD** es un ente de derecho público con personalidad jurídica propia que actúa con plena independencia de las Administraciones Públicas. Es la autoridad de control española conforme al artículo 51 del RGPD.

**Funciones principales:**
- **Supervisar y controlar** → inspecciones y auditorías. Cumplimiento del RGPD y la LOPDGDD
- **Asesorar** → ciudadanos y organizaciones.
- **Resolver reclamaciones**.
- **Sancionar** infracciones.
- **Divulgar** → guías y recomendaciones.
- **Canal prioritario** → difusión de contenidos sensibles (violencia digital y pornografía no consentida).

### 4.2. Régimen sancionador

- **Inferior** → hasta **10 M€ o 2%** del volumen de negocio (Obligaciones del responsable/encargado, DPO, certificación)
- **Superior** → hasta **20 M€ o 4%** del volumen de negocio (Principios del tratamiento, derechos de los interesados, transferencias internacionales)

**LOPDGDD:** infracciones → **leves (prescripción 1 año) | graves (2 años) | muy graves (3 años)**.
⚠️ **AAPP:** no multa económica → **apercibimiento** y posibles medidas adicionales.


### 4.3. Cooperación internacional

- **Comité Europeo de Protección de Datos (CEPD / EDPB)** → coordina la aplicación del RGPD en la UE.
- La **AEPD participa** en este organismo.

## 5. Conclusión

La protección de datos de carácter personal es un derecho fundamental sustentado por un marco normativo robusto: el RGPD como pilar europeo directamente aplicable y la LOPDGDD como adaptación nacional. 
Las AAPP deben aplicar medidas como privacidad desde el diseño, cifrado, control de acceso y auditoría. La AEPD supervisa el cumplimiento y, junto con el CEPD, garantiza la aplicación de la normativa en España y la UE.