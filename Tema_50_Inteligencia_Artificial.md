# Tema 50. Inteligencia Artificial: Conceptos básicos, tecnologías fundamentales y aplicaciones prácticas. Aprendizaje automático (*Machine Learning*). Aprendizaje supervisado, no supervisado y por refuerzo. *Deep Learning*. Aspectos éticos.

---

## 1. Introducción

La Inteligencia Artificial (IA) se ha consolidado como la tecnología más transformadora de nuestra era, con un impacto especialmente significativo en la Administración Pública. Su capacidad de análisis permite transitar desde un modelo estatal reactivo hacia una gestión predictiva y centrada en el ciudadano.

Delegar decisiones en algoritmos conlleva, no obstante, ineludibles retos técnicos, éticos y jurídicos. Este tema examina los fundamentos tecnológicos de la IA, explora el *Machine Learning* y sus metodologías, presenta el salto cualitativo del *Deep Learning* y detalla el marco normativo europeo —el **AI Act**— que rige su despliegue seguro.

---

## 2. Conceptos Básicos y Tecnologías Fundamentales

### 2.1. Definición y clasificación de la IA

La IA es la disciplina científica que diseña sistemas computacionales capaces de emular capacidades cognitivas humanas: procesar lenguaje, percibir el entorno, extraer conocimiento y aprender para resolver problemas complejos. En el ámbito de la informática, puede definirse como un conjunto de capacidades cognoscitivas e intelectuales expresadas mediante modelos estadísticos o combinaciones de algoritmos, cuya finalidad es la creación de sistemas que imiten la inteligencia humana y que aprendan a medida que recopilan información.

Existen diversas formas de clasificar los sistemas de IA:

- **Por capacidad**: *IA Fuerte o AGI* (modelo teórico aún inexistente, con consciencia e inteligencia polivalente) frente a *IA Débil o Estrecha* (sistemas diseñados para tareas acotadas, como traducir textos o jugar al ajedrez). Toda tecnología actual pertenece a esta segunda categoría.
- **Por enfoque metodológico**: *IA Simbólica* (reglas lógicas preprogramadas, utilizada en sistemas expertos clásicos) frente a *IA Conexionista/Estadística* o *Machine Learning*, que constituye el paradigma vigente.
- **Por comportamiento** (según Norvig y Russell): sistemas reactivos, basados en memoria, basados en metas y basados en modelos, con capacidad creciente de razonamiento y planificación.

### 2.2. Tecnologías Fundamentales

La IA integra varias ramas interconectadas:

- **Procesamiento de Lenguaje Natural (PLN/NLP)**: Permite a las máquinas comprender, interpretar, manipular y generar texto humano natural. Base de los asistentes virtuales y los Grandes Modelos de Lenguaje (LLM).
- **Visión Artificial (*Computer Vision*)**: Adquiere, procesa y comprende imágenes y vídeos del mundo real para producir información numérica o simbólica. Aplicada en reconocimiento facial, diagnóstico médico por imagen y control de calidad industrial.
- **Robótica e IoT Cognitivo**: Integra sensores con capacidades inferenciales autónomas para actuar en el entorno físico en tiempo real.
- **Sistemas Expertos**: Emulan la toma de decisiones de un experto humano mediante una base de conocimientos, una base de hechos y un motor de inferencia.
- **Sistemas Recomendadores**: Filtran y personalizan información para el usuario en función de predicciones sobre su historial de comportamiento.
- **Agentes Inteligentes**: Entidades autónomas con conocimientos propios que interactúan de forma flexible con su entorno y con otros agentes, formando sistemas multiagente.

### 2.3. Aplicaciones Prácticas en el Sector Público

La IA converge en tres grandes frentes para la Administración:

1. **Análisis Predictivo**: Detección de fraude fiscal mediante anomalías en declaraciones, y estimación de la demanda futura en servicios sociales.
2. **Sistemas Conversacionales (*Chatbots*)**: Asistentes en la Sede Electrónica que ofrecen orientación normativa las 24 horas.
3. **Automatización Inteligente (RPA Cognitiva)**: Redirección automatizada de expedientes del registro virtual hacia los técnicos competentes. La AGE cuenta para ello con el **Servicio de Automatización de Procesos (SAI)**, enmarcado en la medida 5 del Plan de Digitalización de las AAPP 2021-2025.

---

## 3. Aprendizaje Automático (*Machine Learning*)

El *Machine Learning* (ML) es el motor basal de la IA moderna. Arthur Samuel lo definió como «el campo que dota a los ordenadores de la capacidad de aprender sin necesidad de ser explícitamente programados».

A diferencia de la programación tradicional —donde un ingeniero codifica reglas estrictas—, el ML aplica un esquema **inductivo**: se proporcionan al sistema grandes volúmenes de datos históricos y sus soluciones asociadas para que el propio software infiera la regla matemática subyacente. La regla descubierta, denominada **modelo predictivo**, se aplica después sobre datos nuevos para deducir resultados futuros.

### 3.1. Componentes clave de un sistema ML

| Componente | Descripción |
|---|---|
| **Dataset** | Conjunto de datos dividido en datos de entrenamiento (para construir el modelo) y datos de prueba (para evaluar su fiabilidad). |
| **Características (*Features*)** | Variables medibles seleccionadas como entrada del modelo (edad, ingresos, cargas familiares, etc.). |
| **Función de coste (*Loss Function*)** | Mide el error del modelo respecto al resultado esperado. |
| **Optimizador** | Rutina matemática (p. ej., *Descenso de Gradiente*) que ajusta los parámetros del modelo para minimizar el error. |

El proceso incluye además etapas de **preprocesado** imprescindibles: limpieza de datos, filtrado/transformación, normalización y reducción de la dimensionalidad (técnicas como PCA o SVD).

---

## 4. Tipos de Aprendizaje

### 4.1. Aprendizaje Supervisado

Es la modalidad más empleada en entornos corporativos y administrativos. Entrena el modelo con un volumen histórico de datos **etiquetados**, es decir, con la respuesta correcta ya definida por expertos humanos (p. ej., expedientes de becas marcados como «Estimada» o «Desestimada»).

Se divide en dos vertientes:

- **Clasificación**: Asigna una etiqueta categórica a nuevos datos («Apelable / No apelable»). Algoritmos destacados: Regresión Logística, *Support Vector Machines* (SVM), *Random Forest*, *Naive Bayes* o *Gradient Boosting*.
- **Regresión**: Predice un valor numérico continuo (p. ej., estimación mensual de ingresos tributarios municipales). La Regresión Lineal Múltiple es el algoritmo de referencia.

### 4.2. Aprendizaje No Supervisado

Se aplica cuando los datos carecen de etiquetas directrices. El modelo explora autónomamente la información para descubrir estructuras, agrupaciones y correlaciones ocultas.

Sus técnicas principales son:

- **Agrupamiento (*Clustering*)**: Divide la población en grupos internamente homogéneos. El algoritmo K-Means es el más utilizado, junto con variantes como K-Medoids, DBSCAN o el *Clustering* jerárquico (con su representación en dendrograma).
- **Reducción de Dimensionalidad**: Simplifica variables redundantes manteniendo la representatividad. El Análisis de Componentes Principales (PCA) y la Descomposición en Valores Singulares (SVD) son las técnicas lineales más habituales.

### 4.3. Aprendizaje por Refuerzo (*Reinforcement Learning*)

Inspirado en el conductismo, prescinde de bases de datos históricas. Un **agente** autónomo interactúa con un **entorno** simulado mediante prueba y error: cada acción recibe una recompensa positiva o negativa, y el agente optimiza su política de comportamiento para maximizar la recompensa acumulada. Se aplica en conducción autónoma, robótica, juegos y optimización de procesos industriales como la semaforización urbana inteligente.

Existen también modalidades adicionales: el **aprendizaje semisupervisado** (combina datos etiquetados y no etiquetados, útil cuando etiquetar es costoso) y el **aprendizaje multi-tarea** (entrena un único modelo para múltiples tareas aprovechando conocimiento compartido).

---

## 5. *Deep Learning* (Aprendizaje Profundo)

El *Deep Learning* es el subconjunto del ML que se apoya en **Redes Neuronales Artificiales** de gran profundidad, con decenas o centenares de capas ocultas que procesan datos de forma secuencial y jerarquizada. Su principal atributo diferencial es el **autodescubrimiento de características**: mientras el ML clásico exige que los ingenieros definan manualmente las variables relevantes, las redes profundas aprenden a extraerlas automáticamente de datos en bruto (imágenes, audio, texto).

### 5.1. Arquitecturas Fundamentales

| Arquitectura | Aplicación principal |
|---|---|
| **Redes Convolucionales (CNN)** | Visión artificial: reconocimiento de matrículas, DNI electrónico, diagnóstico oncológico por imagen, cartografía catastral con dron. |
| **Redes Recurrentes y LSTM** | Series temporales y secuencias: traducción de habla, predicción meteorológica, análisis de deuda pública. |
| **Transformadores (LLM)** | Procesamiento y generación de texto: GPT-4, Gemini, Llama. Utilizan el mecanismo de *self-attention* para captar relaciones contextuales en documentos completos. |

### 5.2. IA Generativa (GenAI)

La IA Generativa facilita la creación de contenido sintético nuevo —texto, imágenes, vídeo, audio— a partir de grandes volúmenes de datos. Los **LLM** se basan en la arquitectura Transformer y procesan el lenguaje mediante *tokens*. Para optimizar su rendimiento sin reentrenamiento completo se utilizan técnicas como la **ingeniería de *prompts*** (diseño cuidadoso de instrucciones, con enfoques *zero-shot*, *one-shot* y *few-shot*) y la **Generación Aumentada por Recuperación (RAG)**, que enriquece la consulta con información recuperada de bases de datos indexadas.

---

## 6. Aspectos Éticos y Marco Legal Europeo

### 6.1. Riesgos Éticos de la IA en la Administración

La automatización de decisiones administrativas plantea desafíos críticos:

- **Sesgos Algorítmicos (*Bias*)**: Los modelos aprenden de datos históricos que pueden contener prejuicios pasados. Si los datos de entrenamiento reflejan denegaciones desproporcionadas a ciertos colectivos, el algoritmo reproducirá y amplificará esa discriminación, vulnerando el principio de igualdad.
- **Opacidad Algorítmica («Caja Negra»)**: Las redes neuronales profundas no permiten explicar con exactitud el fundamento de una decisión denegatoria, lo que atenta contra la motivación jurídica exigida por el ordenamiento y contra el artículo 22 del RGPD.
- **Alucinaciones Generativas**: Los LLM pueden generar resoluciones o textos con coherencia sintáctica perfecta pero basados en hechos o normativas inventados, con riesgo de indefensión para el ciudadano.
- **Ataques de inyección de *prompts***: Manipulación malintencionada de las instrucciones de los LLM para obtener respuestas inseguras (recogido por OWASP como uno de los principales riesgos).
- **Impacto laboral y ambiental**: La automatización exige reestructuración laboral justa, y el entrenamiento de grandes modelos conlleva un alto coste energético con impacto en el efecto invernadero.

### 6.2. El Marco Legal Europeo: AI Act (Reglamento UE 2024/1689)

El **AI Act** es el primer marco normativo mundial que clasifica los sistemas de IA según su nivel de riesgo, imponiendo obligaciones proporcionales:

| Nivel de riesgo | Descripción | Ejemplos |
|---|---|---|
| **Inaceptable (Prohibición total)** | Amenaza clara a la seguridad o derechos europeos. | *Social scoring*, vigilancia biométrica masiva en tiempo real, manipulación del comportamiento. |
| **Alto riesgo** | Permitido con obligaciones estrictas: evaluación de conformidad, auditorías de sesgos y supervisión humana obligatoria. | Clasificación de candidatos en oposiciones, asignación de ayudas sociales, priorización policial. |
| **Riesgo limitado** | Obligación de transparencia: informar al ciudadano de que interactúa con una máquina. | *Chatbots* y asistentes virtuales en Sede Electrónica. |
| **Riesgo mínimo** | Sin regulación restrictiva. | Filtros de spam, videojuegos. |

En España, la autoridad de supervisión designada es la **Agencia Española de Supervisión de la Inteligencia Artificial (AESIA)**, que representa al país en el Comité Europeo de IA y tiene encomendadas las labores de supervisión, minimización de riesgos y fomento de un ecosistema de investigación e innovación en IA.

---

## 7. Conclusión

La adopción de la IA —desde el *Machine Learning* predictivo hasta la revolución del *Deep Learning* generativo— transforma los paradigmas de tramitación estática en el sector público, capacitando a la Administración para predecir, automatizar tareas masivas y ofrecer interacción inmediata al ciudadano.

Sin embargo, esta modernización exige una subordinación ineludible al blindaje ético y legal del AI Act europeo. Solo garantizando la **transparencia**, eliminando los **sesgos discriminatorios** y manteniendo el **control humano** sobre los algoritmos (*human in the loop*), el e-Gobierno podrá construir un Estado digital verdaderamente equitativo, fiable y respetuoso con los derechos fundamentales del ciudadano.
