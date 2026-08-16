# Tema 25.- Protección de datos de carácter personal. Normativa. Adaptación de aplicaciones y entornos a los requisitos de la normativa legal. La Agencia Española de Protección de Datos.

## 1. Introducción: Protección de Datos de Carácter Personal

La protección de datos de carácter personal constituye un derecho fundamental reconocido en el artículo 18.4 de la Constitución Española, que encomienda al legislador limitar el uso de la informática para garantizar el honor y la intimidad de los ciudadanos. En la era digital, donde las Administraciones Públicas y las empresas tratan volúmenes masivos de información personal, disponer de un marco normativo robusto es esencial para preservar la privacidad de los individuos y garantizar sus derechos.

La evolución normativa en España ha sido progresiva: la Ley Orgánica 5/1992 (LORTAD) fue la primera regulación específica para el tratamiento automatizado de datos personales; la Ley Orgánica 15/1999 (LOPD) amplió su alcance a ficheros no automatizados y transpuso la Directiva 95/46/CE; y, finalmente, con la entrada en vigor del Reglamento General de Protección de Datos (RGPD) en 2018, la Unión Europea estableció un marco normativo único y directamente aplicable. España adaptó su legislación mediante la Ley Orgánica 3/2018, de Protección de Datos Personales y garantía de los derechos digitales (LOPDGDD).

## 2. Normativa

### 2.1. Reglamento General de Protección de Datos (RGPD)

El Reglamento (UE) 2016/679 (RGPD) es directamente aplicable en todos los Estados miembros desde el 25 de mayo de 2018. Sus principales novedades respecto a la normativa anterior fueron:

- Enfoque basado en el **riesgo** y en la **responsabilidad proactiva** (accountability).
- Ampliación de los derechos de los interesados (portabilidad, limitación, supresión).
- Obligación de notificar las brechas de seguridad en 72 horas.
- Régimen sancionador reforzado (multas de hasta 20 millones de euros o el 4% del volumen de negocio anual global).
- Figura del Delegado de Protección de Datos (DPO).

### 2.2. LOPDGDD (Ley Orgánica 3/2018)

La LOPDGDD adapta el ordenamiento jurídico español al RGPD y garantiza los derechos digitales de los ciudadanos (Título X). Entre sus aportaciones específicas destacan:

- Regulación de los datos de personas fallecidas (derecho de acceso por herederos).
- Edad mínima de consentimiento digital del menor: **14 años** (el RGPD permite entre 13 y 16).
- Garantías de los derechos digitales: desconexión digital laboral, educación digital, protección de menores en Internet, testamento digital.

### 2.3. Principios fundamentales del tratamiento

El artículo 5 del RGPD establece los principios que rigen todo tratamiento de datos personales:

1. **Licitud, lealtad y transparencia:** El tratamiento debe basarse en una causa legítima, ser justo para el interesado e informarle de manera clara y accesible.
2. **Limitación de la finalidad:** Los datos solo pueden recogerse para fines determinados, explícitos y legítimos.
3. **Minimización de datos:** Solo deben recogerse los datos adecuados, pertinentes y estrictamente necesarios.
4. **Exactitud:** Los datos deben ser exactos y estar actualizados.
5. **Limitación del plazo de conservación:** Los datos no deben mantenerse más tiempo del necesario.
6. **Integridad y confidencialidad:** Los datos deben tratarse garantizando una seguridad adecuada.
7. **Responsabilidad proactiva (accountability):** El responsable debe ser capaz de demostrar el cumplimiento de todos los principios anteriores.

### 2.4. Bases de legitimación del tratamiento

El artículo 6 del RGPD enumera las seis bases legales que legitiman el tratamiento:

- **Consentimiento** del interesado (libre, específico, informado e inequívoco).
- **Ejecución de un contrato** o medidas precontractuales.
- **Cumplimiento de una obligación legal** del responsable.
- Protección de **intereses vitales** del interesado o de otra persona.
- Cumplimiento de una misión de **interés público** o ejercicio de poderes públicos (base habitual en las AAPP).
- Satisfacción de **intereses legítimos** del responsable (no aplicable a las AAPP en el ejercicio de sus funciones).

### 2.5. Derechos de los interesados: SOPLAR

La LOPDGDD reconoce los siguientes derechos, que pueden recordarse con la regla mnemotécnica **SOPLAR**:

| Letra | Derecho | Contenido | Plazo |
|-------|---------|-----------|-------|
| **S** | **Supresión** ("derecho al olvido") | Solicitar la eliminación de los datos cuando ya no sean necesarios, se retire el consentimiento o el tratamiento sea ilícito | 1 mes |
| **O** | **Oposición** | Oponerse al tratamiento en determinadas circunstancias, como el marketing directo (cese inmediato y sin excepciones) | 1 mes |
| **P** | **Portabilidad** | Recibir los datos en formato estructurado y de uso común (CSV, JSON) y transmitirlos a otro responsable | 1 mes |
| **L** | **Limitación** | Restringir el tratamiento (los datos se conservan pero no se tratan) cuando se impugne la exactitud o el tratamiento sea ilícito | 1 mes |
| **A** | **Acceso** | Conocer si se tratan sus datos, con qué finalidad, a quién se comunican y obtener copia. Ampliable a 2 meses más si la complejidad lo justifica. Si se reitera en 6 meses sin cambio sustancial, se puede cobrar canon o denegar | 1 mes (+2) |
| **R** | **Rectificación** | Corregir datos inexactos o completar datos incompletos. Ejercitable en cualquier momento | 1 mes |

El ejercicio de estos derechos es **gratuito** y, en caso de denegación, el responsable debe motivar su decisión e informar al interesado de su derecho a reclamar ante la AEPD.

### 2.6. Obligaciones de los responsables y encargados del tratamiento

**Delegado de Protección de Datos (DPO):** Su designación es obligatoria en todas las Administraciones Públicas, en entidades que realicen observación habitual y sistemática de interesados a gran escala, y en las que traten categorías especiales de datos a gran escala. El DPO informa, asesora, supervisa el cumplimiento y coopera con la AEPD.

**Evaluación de Impacto en Protección de Datos (EIPD):** Obligatoria cuando un tratamiento suponga un alto riesgo para los derechos de las personas: decisiones automatizadas, elaboración de perfiles, tratamiento a gran escala de datos especiales o videovigilancia de zonas de acceso público.

**Notificación de brechas de seguridad:** El responsable debe notificar a la AEPD en un plazo máximo de **72 horas** desde que tenga conocimiento de la brecha. Si supone un alto riesgo para los interesados, debe notificárseles también sin dilación indebida.

**Registro de actividades de tratamiento:** Documento que detalla la finalidad, categorías de datos, destinatarios, transferencias internacionales, plazos de supresión y medidas de seguridad de cada tratamiento (art. 30 RGPD).

### 2.7. Conservación de datos de comunicaciones electrónicas

La Ley 25/2007 obliga a los operadores de telecomunicaciones a conservar los metadatos de las comunicaciones (no el contenido):

- **Datos conservados:** Origen y destino de la comunicación, fecha, hora, duración, tipo de servicio y ubicación geográfica del dispositivo.
- **Plazo:** Máximo de **12 meses**, reducible a 6 si se justifica.
- **Finalidad:** Exclusivamente para la investigación y persecución de **delitos graves** y la seguridad nacional.
- **Acceso:** Restringido a autoridades judiciales y Fuerzas y Cuerpos de Seguridad del Estado, siempre con autorización judicial previa.

## 3. Adaptación de Aplicaciones y Entornos a los Requisitos de la Normativa Legal

Las Administraciones Públicas deben adaptar sus sistemas de información al cumplimiento del RGPD y la LOPDGDD, implementando medidas técnicas y organizativas adecuadas al nivel de riesgo:

- **Privacidad desde el diseño (Privacy by Design):** Incorporar la protección de datos desde la fase de diseño de las aplicaciones: minimización de datos recogidos, seudonimización de campos identificativos y cifrado de datos en la base de datos.
- **Privacidad por defecto (Privacy by Default):** Las opciones más restrictivas de privacidad deben estar activadas por defecto. Un formulario web no debe tener casillas de consentimiento premarcadas.
- **Control de acceso basado en roles (RBAC):** Solo el personal autorizado accede a los datos necesarios para su función. Un técnico de urbanismo no debe poder consultar datos tributarios.
- **Cifrado de datos:** En reposo (AES-256, BitLocker, TDE en Oracle) y en tránsito (TLS 1.2 o superior).
- **Seudonimización y anonimización:** Sustituir los datos identificativos por códigos reversibles (seudonimización) o irreversibles (anonimización) para reducir el riesgo de identificación del interesado, especialmente en entornos de desarrollo y pruebas.
- **Auditoría y trazabilidad:** Registro de accesos que permita saber quién accedió a qué datos, cuándo y con qué finalidad. Esencial para detectar accesos indebidos y cumplir con las obligaciones de rendición de cuentas.
- **Gestión del consentimiento:** Los sistemas deben recoger el consentimiento de forma granular (finalidad por finalidad), registrando el momento y el contenido exacto del consentimiento otorgado, y permitiendo su revocación.
- **Derecho al olvido técnico:** Las aplicaciones deben implementar mecanismos de supresión efectiva de datos, incluyendo la eliminación en copias de seguridad, índices de búsqueda y sistemas secundarios.
- **Evaluaciones de impacto automatizadas:** Uso de herramientas como **PILAR** (del CCN) o **GESTIONA EIPD** (de la AEPD) para facilitar la realización de evaluaciones de impacto en protección de datos.

## 4. La Agencia Española de Protección de Datos (AEPD)

### 4.1. Naturaleza y funciones

La **AEPD** es un ente de derecho público con personalidad jurídica propia que actúa con plena independencia de las Administraciones Públicas. Es la autoridad de control española conforme al artículo 51 del RGPD.

**Funciones principales:**

- **Supervisión y control:** Velar por el cumplimiento del RGPD y la LOPDGDD mediante inspecciones, auditorías y planes sectoriales.
- **Atención ciudadana:** Informar y asesorar sobre derechos en materia de protección de datos.
- **Resolución de reclamaciones:** Tramitar y resolver las reclamaciones presentadas por los interesados.
- **Potestad sancionadora:** Imponer sanciones por infracciones de la normativa.
- **Promoción y divulgación:** Publicación de guías, recomendaciones y códigos de conducta.
- **Canal prioritario:** Para denuncias de difusión de contenidos sensibles (violencia digital y pornografía no consentida).

### 4.2. Régimen sancionador

El RGPD establece dos niveles de sanciones:

| Nivel | Cuantía máxima | Infracciones |
|-------|---------------|-------------|
| **Inferior** | 10 millones € o 2% del volumen de negocio anual | Obligaciones del responsable/encargado, DPO, certificación |
| **Superior** | 20 millones € o 4% del volumen de negocio anual | Principios del tratamiento, derechos de los interesados, transferencias internacionales |

La LOPDGDD clasifica las infracciones en **leves** (prescripción 1 año), **graves** (2 años) y **muy graves** (3 años). Para las Administraciones Públicas, la AEPD no impone multas económicas, sino un **apercibimiento** y, en su caso, comunicación al Defensor del Pueblo.

### 4.3. Cooperación internacional

La AEPD participa en el **Comité Europeo de Protección de Datos (CEPD/EDPB)**, órgano independiente que garantiza la aplicación coherente del RGPD en todos los Estados miembros, emite directrices vinculantes y resuelve controversias entre autoridades nacionales.

## 5. Conclusión

La protección de datos de carácter personal es un derecho fundamental sustentado por un marco normativo robusto: el RGPD como pilar europeo directamente aplicable y la LOPDGDD como adaptación nacional. La normativa establece principios rectores (licitud, minimización, exactitud, responsabilidad proactiva), derechos de los interesados (SOPLAR: Supresión, Oposición, Portabilidad, Limitación, Acceso y Rectificación) y obligaciones para los responsables (DPO, EIPD, notificación de brechas en 72 horas). La adaptación de las aplicaciones y entornos de las Administraciones Públicas exige implementar medidas técnicas concretas — privacidad desde el diseño, cifrado, control de acceso, auditoría y gestión del consentimiento — que garanticen el cumplimiento efectivo de la normativa. La AEPD, como autoridad de control independiente con potestad sancionadora, supervisa este cumplimiento y coopera a nivel europeo a través del CEPD.
