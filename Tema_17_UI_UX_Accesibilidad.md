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

*  **Figma:** Diseño de interfaces y prototipos interactivos.
*  **Sketch:** Diseño UI para macOS.
*  **Adobe XD** Prototipado y diseño de experiencias.
*  **Wireframe.cc** Wireframes (esquema estructurales) de baja fidelidad. 

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

## 5. Accesibilidad Web: Pautas WCAG

### 5.1. Las Pautas WCAG (Web Content Accessibility Guidelines)

Las **WCAG** son el estándar internacional de accesibilidad web desarrollado por la WAI del W3C. La versión vigente es **WCAG 2.1** (2018), con la versión **WCAG 2.2** publicada en 2023 como actualización incremental. Se organizan en torno a cuatro principios fundamentales conocidos por el acrónimo **POUR**:

#### Principio 1: Perceptible

El contenido y los componentes de la interfaz deben poder ser percibidos por todos los usuarios:

*   **Alternativas textuales:** Toda imagen debe tener un atributo `alt` descriptivo (`<img alt="Escudo del Ayuntamiento de Alicante">`). Los vídeos deben incluir subtítulos y audiodescripción.
*   **Contenido adaptable:** La información debe poder presentarse de diferentes formas (por ejemplo, diseño simplificado) sin perder contenido ni estructura.
*   **Distinguible:** Contraste mínimo de color de **4.5:1** para texto normal y **3:1** para texto grande. El color no debe ser el único medio de transmitir información.

#### Principio 2: Operable

Los componentes de la interfaz y la navegación deben ser operables por todos los usuarios:

*   **Accesible mediante teclado:** Toda funcionalidad debe poder realizarse con teclado. No debe haber trampas de teclado (elementos de los que el usuario no pueda salir con la tecla Tab).
*   **Tiempo suficiente:** Si hay límites de tiempo, el usuario debe poder ampliarlos, desactivarlos o ajustarlos.
*   **No provocar convulsiones:** El contenido no debe parpadear más de tres veces por segundo.
*   **Navegable:** Mecanismos claros de navegación, encabezados descriptivos, foco visible.

#### Principio 3: Comprensible

La información y el manejo de la interfaz deben ser comprensibles:

*   **Legible:** El idioma de la página debe estar declarado (`<html lang="es">`). Los términos técnicos deben explicarse.
*   **Predecible:** La navegación debe ser consistente. Los componentes no deben cambiar de comportamiento inesperadamente.
*   **Ayuda para la entrada:** Los campos de formulario deben tener etiquetas `<label>` asociadas. Los errores deben identificarse claramente y ofrecer sugerencias de corrección.

#### Principio 4: Robusto

El contenido debe ser lo suficientemente robusto como para funcionar con una amplia gama de tecnologías de asistencia:

*   **Compatible:** El código HTML debe ser sintácticamente correcto y utilizar ARIA (Accessible Rich Internet Applications) cuando sea necesario para etiquetar componentes dinámicos.

### 5.2. Niveles de conformidad WCAG

Las WCAG definen tres niveles de conformidad acumulativos:

| Nivel | Descripción | Requisito legal en España |
|-------|-------------|--------------------------|
| **A** | Requisitos mínimos de accesibilidad | Obligatorio |
| **AA** | Nivel intermedio, elimina barreras significativas | **Obligatorio** (nivel exigido por la normativa española) |
| **AAA** | Nivel máximo de accesibilidad | Recomendado (no exigido como requisito general) |

### 5.3. WAI-ARIA (Accessible Rich Internet Applications)

Las aplicaciones web modernas (SPA con React, Angular, Vue.js) utilizan componentes dinámicos (menús desplegables, pestañas, modales, carruseles) que no están contemplados en las etiquetas HTML estándar. **WAI-ARIA** es una especificación del W3C que define atributos adicionales para etiquetar estos componentes de forma que los lectores de pantalla puedan interpretar su función:

*   **Roles:** `role="navigation"`, `role="dialog"`, `role="tablist"`, `role="alert"`.
*   **Propiedades:** `aria-label="Menú principal"`, `aria-describedby="texto-ayuda"`.
*   **Estados:** `aria-expanded="true"`, `aria-hidden="false"`, `aria-selected="true"`.

## 6. Responsive Web Design vs. Adaptive Web Design

### 6.1. Responsive Web Design (RWD)

El servidor entrega **un único documento HTML** y un único conjunto de CSS a todos los dispositivos. Las **CSS Media Queries** adaptan la presentación según el ancho de pantalla, la orientación y las capacidades del dispositivo:

```css
/* Diseño base (Mobile First) */
.contenedor { width: 100%; padding: 10px; }

/* Tablets (768px o más) */
@media (min-width: 768px) {
    .contenedor { width: 750px; margin: 0 auto; }
}

/* Escritorio (1200px o más) */
@media (min-width: 1200px) {
    .contenedor { width: 1140px; }
}
```

**Ventajas:** Un solo código fuente, mantenimiento simplificado, SEO optimizado (una sola URL).

### 6.2. Adaptive Web Design (AWD)

El servidor detecta el tipo de dispositivo (mediante el User-Agent de la petición HTTP) y entrega **versiones diferentes** del sitio web para cada tipo de dispositivo (escritorio, tablet, móvil).

**Desventaja principal:** Requiere mantener múltiples versiones del sitio web en paralelo, aumentando significativamente los costes de desarrollo y mantenimiento.

**Tendencia actual:** RWD con enfoque Mobile First es el estándar predominante.

## 7. Marco Normativo de Accesibilidad en España

*   **Real Decreto 1112/2018:** Sobre accesibilidad de los sitios web y aplicaciones para dispositivos móviles del sector público. Transpone la Directiva europea 2016/2102 y exige el cumplimiento del nivel **AA de las WCAG 2.1**.
*   **Norma UNE-EN 301549:** Norma técnica europea que establece los requisitos de accesibilidad para productos y servicios TIC, incluyendo sitios web y aplicaciones móviles.
*   **Ley 11/2023** y **Real Decreto 193/2023:** Actualizaciones normativas que refuerzan las obligaciones de accesibilidad digital en el sector público.

## 8. Conclusión

La accesibilidad y la usabilidad web constituyen requisitos legales y éticos ineludibles para las Administraciones Públicas. Las pautas WCAG 2.1 del W3C, con sus cuatro principios (Perceptible, Operable, Comprensible, Robusto) y el nivel de conformidad AA exigido por el Real Decreto 1112/2018, garantizan que los servicios digitales públicos sean universalmente accesibles.

El Diseño Universal, los conceptos de UX y UI, y las técnicas de Responsive Web Design proporcionan el marco metodológico y técnico para construir interfaces que no discriminen a ningún ciudadano por sus capacidades o por el dispositivo que utilice. En un contexto de creciente digitalización de los servicios públicos, el dominio de estos estándares y principios resulta imprescindible para garantizar que la transformación digital no genere nuevas formas de exclusión.
