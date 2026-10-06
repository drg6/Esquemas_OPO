# Tema 17.- Accesibilidad y usabilidad de las tecnologías, productos y servicios relacionados con la sociedad de la información: W3C. Diseño universal; conceptos de UX (user experience) y UI (user interface).

## 1. Introducción
* **Propósito en la AP:** La digitalización de trámites exige que las interfaces sean universalmente utilizables, mitigando cualquier barrera por discapacidad o brecha digital.
* **Obligatoriedad:** La usabilidad y accesibilidad han dejado de ser recomendaciones estéticas para convertirse en mandatos legales estrictos.

## 2. Accesibilidad y usabilidad de las tecnologías, productos y servicios de la sociedad de la información
* **Marco Normativo (Crítico para AAPP):** El **Real Decreto 1112/2018** obliga al sector público a cumplir el nivel **WCAG 2.1 AA** en webs y apps móviles. 
* **Obligaciones derivadas:** Publicación obligatoria de una **Declaración de Accesibilidad** y habilitación de un mecanismo de comunicación para reporte de barreras.
* **Norma y Auditoría:** Norma técnica europea **UNE-EN 301549**. El **OBSAE** (Observatorio de Accesibilidad Web) audita y emite informes periódicos sobre el cumplimiento de los portales públicos.
* **Adaptabilidad (RWD):** La usabilidad multidispositivo se garantiza mediante el **Responsive Web Design** (uso de CSS Media Queries y *Mobile First*), desplazando al costoso modelo adaptativo (AWD).

## 3. W3C (World Wide Web Consortium) y estándares
* **El W3C:** Organismo fundado por Tim Berners-Lee (1994) que gobierna los estándares abiertos de la Web (HTML5, CSS3).
* **WAI (Web Accessibility Initiative):** Grupo interno del W3C dedicado a desarrollar pautas para la accesibilidad web.
* **WCAG 2.1 / 2.2 (*Web Content Accessibility Guidelines*):** Estándar internacional basado en 4 principios fundamentales (Acrónimo **POUR**):
  1. **Perceptible:** Textos alternativos (`alt`) en imágenes, subtítulos en vídeo y contraste de color mínimo (4.5:1 para texto normal y **3:1** para texto grande).
  2. **Operable:** Navegable enteramente por teclado (sin "trampas"), tiempo suficiente para leer/actuar y control de parpadeos (prevención de convulsiones).
  3. **Comprensible:** Idioma declarado (`<html lang="es">`), navegación predecible y campos de formulario con etiquetas (`<label>`) y prevención de errores.
  4. **Robusto:** HTML válido y compatibilidad con tecnologías de asistencia.
* **Niveles de Conformidad:** **A** (Mínimo), **AA** (Intermedio y **Obligatorio legalmente en España**) y **AAA** (Máximo).
* **WAI-ARIA (*Accessible Rich Internet Applications*):** Atributos inyectados en el HTML (`role="..."`, `aria-label="..."`, `aria-hidden="..."`) para dar significado a componentes dinámicos de JavaScript (ej. menús desplegables) de cara a los lectores de pantalla. *Regla de oro:* Usar HTML semántico nativo siempre que sea posible antes de recurrir a ARIA.

## 4. Diseño universal
* **Definición:** Filosofía (Ronald Mace) para diseñar productos, entornos y servicios utilizables por **todas las personas** en la mayor medida posible, sin adaptaciones ni diseños especializados.
* **Aplicación técnica web:** Garantizar la navegación sin ratón, asegurar la interpretación mediante lectores de pantalla (*Screen Readers* como NVDA o JAWS) y utilizar paletas de color aptas para usuarios con daltonismo.

## 5. Conceptos de UX (user experience) y UI (user interface)
* **UX (*User Experience*):**
  * Lo que el usuario **siente y experimenta** de forma global (eficiencia, satisfacción, tolerancia a errores, arquitectura de la información).
  * *Metodologías:* *Design Thinking* (Empatizar $\rightarrow$ Definir $\rightarrow$ Idear $\rightarrow$ Prototipar $\rightarrow$ Testear), Diseño Centrado en el Usuario (UCD) y Mapas de Experiencia (*Customer Journey*).
* **UI (*User Interface*):**
  * Lo que el usuario **ve y con lo que interactúa** (botones, menús, tipografía).
  * *Principios:* Consistencia, Jerarquía visual, *Feedback* inmediato y *Affordance* (el diseño de un elemento debe sugerir intuitivamente cómo usarlo).
* **Herramientas de prototipado:** Software como Figma o Adobe XD permite validar los flujos de UX y el contraste visual de UI antes de escribir código.

## 6. Conclusión
La accesibilidad web y la usabilidad son un imperativo legal (RD 1112/2018) en la Administración Pública. La aplicación del Diseño Universal, articulada mediante los estándares del W3C (WCAG AA y WAI-ARIA) y apoyada en metodologías rigurosas de UX/UI, garantiza que los servicios digitales sean equitativos y no generen barreras de exclusión ciudadana.

---------------------

# Tema 17.- Accesibilidad y usabilidad de las tecnologías, productos y servicios relacionados con la sociedad de la información: W3C. Diseño universal; conceptos de UX (user experience) y UI (user interface).

## 1. Introducción

La transformación digital de las AP ha trasladado al entorno web un volumen creciente de trámites y servicios que anteriormente requerían presencia física: empadronamiento, liquidación de tributos... Ciudadanos discapacidad visual, auditiva, motora o cognitiva, personas mayores necesitan que los servicios digitales estén diseñados para ser universalmente utilizables.

La **usabilidad** y la **accesibilidad web** en las AP española, constituyen **obligaciones legales** reguladas por un marco normativo específico. Este tema analiza los conceptos de UX/UI, el Diseño Universal, las pautas de accesibilidad del W3C y su marco normativo aplicable.

## 2. El W3C y la Gobernanza de los Estándares Web

### 2.1. El World Wide Web Consortium (W3C)

El **W3C** es el organismo internacional de estandarización, 1994 Tim Berners-Lee (creador de la World Wide Web), estándares más relevantes del W3C incluyen:

*   **HTML5:** Lenguaje de marcado para la estructura de las páginas web.
*   **CSS3:** Hojas de estilo para la presentación visual.
*   **WAI-ARIA:** Especificación de accesibilidad para aplicaciones web dinámicas.
*   **WCAG 2.1/2.2:** Pautas de accesibilidad para el contenido web.

### 2.2. WAI (Web Accessibility Initiative)

Dentro del W3C, **WAI (Web Accessibility Initiative)** es el grupo de trabajo dedicado a desarrollar normas que hagan la Web accesible para personas con discapacidad. Responsable de las pautas WCAG.

## 3. Conceptos de UI (User Interface) y UX (User Experience)

### 3.1. UI: Interfaz de Usuario

Elementos visuales e interactivos que componen la interfaz: botones, menús, formularios, colores, tipografías, iconos. Estética, la coherencia visual y la claridad de la presentación. Usuario *ve*.

**Principios de diseño UI:**
*   **Consistencia:** Misma visualización y comportamiento.
*   **Jerarquía visual:** Elementos importantes deben destacar.
*   **Feedback inmediato:** Acción = respuesta visual (Cambio de color de un botón al pulsarle).
*   **Affordance:** Los elementos deben sugerir cómo se usan (botón debe parecer pulsable).

### 3.2. UX: Experiencia de Usuario

La experiencia global del usuario al interactuar con el sistema: facilidad de uso, eficiencia, satisfacción, capacidad de aprendizaje.. Usuario *siente*. La UX abarca la UI.

**Metodologías de diseño UX:**
*   **Design Thinking:**  Empatizar → Definir → Idear → Prototipar → Testear (TIPED)
*   **Diseño Centrado en el Usuario (UCD):** Diseño con usuarios reales.
*   **Personas:** Usuarios ficticios.
*   **Customer Journey Map:** Recorrido del usuario en etapas de interacción.

### 3.3. Herramientas de prototipado

Figma, Adobe XD, Sketch. Permiten simular la navegación y evaluar el contraste y tamaños mínimos (accesibilidad desde el diseño) antes del desarrollo.

## 4. Diseño Universal (Design for All)

### 4.1. Definición

El **Diseño Universal** (Design for All) crear productos / servicios utilizables por **todas las personas** sin necesidad de adaptaciones o diseños especializados. Principios:

1.  **Uso equitativo:** Útil para personas con diversas capacidades.
2.  **Flexibilidad en el uso:** Interfaces que funcionen con teclado, ratón, voz y pantalla táctil
3.  **Uso simple e intuitivo:** 
4.  **Información perceptible:** Ej. Contraste de color suficiente para usuarios con baja visión o daltonismo, Alternativas textuales para contenido multimedia.
5.  **Tolerancia al error:** Minimiza consecuencias acciones accidentales, Ej. Confirmación antes de eliminar un archivo
6.  **Bajo esfuerzo físico:** 
7.  **Tamaño y espacio adecuados:** Ej. Botones suficientemente grandes

[Memorizar 3 o 4 con ejemplos, el resto agruparlos]

## 5. Accesibilidad Web: Pautas WCAG

### 5.1. Las Pautas WCAG (Web Content Accessibility Guidelines)

Las **WCAG** estándar internacional de accesibilidad web desarrollado WAI del W3C. **WCAG 2.1** (2018), **WCAG 2.2** (actualización 2023). 4 principios **POUR**:

#### Principio 1: Perceptible

*   **Alternativas textuales:** Imagenes con `alt`. Los vídeos con subtítulos y audiodescripción.
*   **Contenido adaptable:** Diseño simplificado sin perder contenido ni estructura.
*   **Distinguible:** Contraste mínimo **4.5:1**  texto normal y **3:1** para texto grande. No solo color para transmitir información.

#### Principio 2: Operable

*   **Accesible mediante teclado:** Toda funcionalidad debe poder realizarse con teclado. Uso tecla Tab.
*   **Tiempo suficiente:** Límites de tiempo ampliables o desactivables.
*   **No provocar convulsiones:** No parpadear más de tres veces por segundo.
*   **Navegable:** Mecanismos claros de navegación, encabezados descriptivos, foco visible.

#### Principio 3: Comprensible

*   **Legible:** El idioma página declarado y términos técnicos explicados.
*   **Predecible:** No deben cambiar de comportamiento inesperadamente.
*   **Ayuda para la entrada:** Etiquetas `<label>`. Errores identificados.

#### Principio 4: Robusto

*   **Compatible:** HTML sintácticamente correcto y utilizar atributo ARIA (Accessible Rich Internet Applications) para etiquetar componentes dinámicos (div o span).

### 5.2. Niveles de conformidad WCAG

Las WCAG definen tres niveles de conformidad acumulativos:

**A**: Requisitos mínimos de accesibilidad. Obligatorio.
**AA**: Nivel intermedio, elimina barreras significativas. **Obligatorio** (nivel exigido por la normativa española)
**AAA**: Nivel máximo de accesibilidad. Recomendado.

### 5.3. WAI-ARIA (Accessible Rich Internet Applications)

HTML no estándar -> **WAI-ARIA** (atributos adicionales para que los lectores de pantalla puedan interpretar su función):

*   **Roles:** `role="dialog"`.
*   **Propiedades:** `aria-label="Menú principal"`.
*   **Estados:** `aria-hidden="false"`.

## 6. Responsive Web Design vs. Adaptive Web Design

### 6.1. Responsive Web Design (RWD)

**CSS Media Queries** adaptan la presentación según el ancho de pantalla, la orientación y las capacidades del dispositivo.
Ej. @media (min-width: 768px) {...}

**Ventajas:** Un solo código fuente, mantenimiento simplificado, SEO optimizado (una sola URL).

RWD con enfoque Mobile First es el estándar predominante.

### 6.2. Adaptive Web Design (AWD)

El servidor detecta el tipo de dispositivo (mediante el User-Agent de HTTP) y entrega **versiones diferentes** del sitio web para cada tipo de dispositivo (escritorio, tablet, móvil).

**Desventaja principal:** Requiere mantener múltiples versiones del sitio web en paralelo (mas desarrollo y mantenimiento).

## 7. Marco Normativo de Accesibilidad en España

*   **Real Decreto 1112/2018:** Sobre accesibilidad de los sitios web y aplicaciones para dispositivos móviles del sector público. Transpone la Directiva europea 2016/2102 y exige el cumplimiento del nivel **AA de las WCAG 2.1**.
*   **Obligaciones derivadas del RD 1112/2018:**
    *   **Declaración de Accesibilidad:** Página obligatoria detallando el nivel de cumplimiento.
    *   **Mecanismo de comunicación:** Canal para que los ciudadanos reporten incumplimientos o pidan formatos accesibles.
*   **OBSAE (Observatorio de Accesibilidad Web):** Iniciativa estatal que realiza evaluaciones periódicas y consolida informes de cumplimiento de las AP para enviar a la Comisión Europea.
*   **Norma UNE-EN 301549:** Norma técnica europea que establece los requisitos de accesibilidad para productos y servicios TIC, incluyendo sitios web y aplicaciones móviles.

## 8. Conclusión

La accesibilidad y la usabilidad web constituyen requisitos legales y éticos ineludibles para las Administraciones Públicas. Las pautas WCAG 2.1 del W3C y el nivel de conformidad AA exigido por el Real Decreto 1112/2018, garantizan que los servicios digitales públicos sean universalmente accesibles.

En un contexto de creciente digitalización de los servicios públicos, el dominio de estos estándares y principios resulta imprescindible para garantizar que la transformación digital no genere nuevas formas de exclusión.
