# Tema 50. Inteligencia Artificial: Conceptos básicos, tecnologías fundamentales y aplicaciones prácticas. Aprendizaje automático (*Machine Learning*). Aprendizaje supervisado, no supervisado y por refuerzo. *Deep Learning*. Aspectos éticos.

## 1. Introducción
- **El cambio de paradigma:** La IA permite a la Administración transitar de un modelo *reactivo* a uno *predictivo* y automatizado.
- **El reto:** Delegar decisiones en algoritmos exige un control riguroso para evitar sesgos discriminatorios y garantizar el estricto cumplimiento del Esquema Nacional de Seguridad (ENS) y el RGPD.

## 2. IA: Conceptos básicos, tecnologías fundamentales y aplicaciones prácticas
- **2.1. Conceptos básicos (Definición y Clasificación):** 
  * Rama de la informática que desarrolla sistemas capaces de **simular la inteligencia humana** (aprender y razonar) para tomar decisiones de forma **autónoma** a partir de **datos**.
  * *Por capacidad:* **IA Estrecha o Débil** (sistemas actuales acotados a tareas específicas) frente a **IA General o Fuerte / AGI** (modelo teórico inexistente capaz de realizar cualquier tarea intelectual humana con la misma eficacia (o superior) que una persona).
  * *Por enfoque:* **IA Simbólica** (reglas lógicas preprogramadas) frente a **IA Conexionista** (basada en datos, origen del Machine Learning).
- **2.2. Tecnologías fundamentales y aplicaciones prácticas:**
  * **Casos de uso municipal (Tecnologías):**
    * *Análisis Predictivo (Machine Learning):* Aprendizaje a partir de datos históricos para anticipar comportamientos o tendencias (Ej: Detección de fraude fiscal mediante anomalías y estimación de demanda en servicios sociales).
    * *Procesamiento de Lenguaje Natural (NLP):* Comprensión y generación de texto/voz (Ej: Chatbots en la Sede Electrónica para orientación normativa 24/7).
    * *Visión Artificial:* Interpretación y análisis de imágenes o vídeos (Ej: OCR de matrículas para control de tráfico o zonas de bajas emisiones).
    * *Automatización Inteligente (RPA Cognitiva):* Ej. Redirección de expedientes en registro al técnico competente. 
  * **Gestión y Gobernanza TIC (Despliegue corporativo):**
    * *Copilotos (M365):* Obligan a auditar estrictamente los permisos del *Active Directory* para evitar que la IA exponga documentos internos a usuarios sin privilegios.
    * *Arquitecturas Seguras (RAG - Retrieval-Augmented Generation):* Uso de IA en nubes soberanas que solo "lee" normativa local, sin enviar datos de ciudadanos a modelos públicos, garantizando privacidad.

## 3. Aprendizaje automático (Machine Learning). Aprendizaje supervisado, no supervisado y por refuerzo.
El sistema aprende infiriendo reglas matemáticas a partir de datos históricos, en lugar de ser programado de forma explícita. Se divide en tres enfoques principales:
- **3.1. Aprendizaje Supervisado:** Entrenado con datos *etiquetados* (respuesta conocida). *Uso local:* Predecir si una beca será aprobada o denegada basándose en el histórico de concesiones.
- **3.2. Aprendizaje No Supervisado:** Usa datos *sin etiquetar* para buscar patrones ocultos (Agrupamiento o Clustering - divide en grupos). *Uso local:* Agrupar perfiles de contribuyentes para detectar anomalías o fraude fiscal.
- **3.3. Aprendizaje por Refuerzo:** Aprende por *prueba y error* (recibiendo recompensas/penalizaciones). *Uso local:* Optimización y sincronización inteligente de la red de semáforos de la ciudad según el tráfico en tiempo real, menos cola de coches 10 puntos y atasco -50 puntos.

## 4. Deep Learning
Subconjunto del ML que utiliza **Redes Neuronales Profundas** (múltiples capas ocultas) para extraer características automáticamente de datos crudos (imágenes, audios).
- **4.1. Arquitecturas principales:**
  * *Redes Convolucionales (CNN - Espacio/Imágenes):* Para visión artificial (ej. lectura de matrículas policiales).
  * *Redes Recurrentes (RNN - Tiempo/Secuencias):* Para análisis de series temporales (ej. predicción meteorológica).
  * *Transformers (Atención/Contexto):* Arquitectura base de la IA Generativa y los LLMs.
    * *Ejemplos LLMs (Texto/Código):* ChatGPT, Gemini, Claude.
    * *Ejemplos GenAI (Multimedia):* Midjourney, DALL-E (generación de imágenes).

## 5. Aspectos éticos (Riesgos y Regulación AI Act)
La implementación de la IA plantea dilemas éticos y de seguridad que el marco normativo actual debe resolver.
- **5.1. Riesgos Éticos y Técnicos:**
  * *Caja Negra (Opacidad):* Imposibilidad de explicar jurídicamente *por qué* la red neuronal tomó una decisión (vulnera la motivación del acto administrativo).
  * *Sesgo (Bias):* Si los datos históricos son discriminatorios, la IA perpetuará y amplificará esa discriminación en sus predicciones.
  * *Alucinaciones (Fiabilidad):* Generación de respuestas gramaticalmente perfectas pero con hechos o normativas inventadas, lo que supone un riesgo jurídico inaceptable en la información al ciudadano si no se mitiga mediante arquitecturas RAG.
  * *Ciberseguridad OWASP:* Vulnerabilidades como el *Prompt Injection* (manipular el chatbot municipal para que revele información privada) o el *Data Poisoning* (envenenar los datos de entrenamiento para alterar el comportamiento futuro del modelo).
- **5.2. El Marco Legal (AI Act - Rgto UE 2024/1689):** Es la respuesta legal a los riesgos éticos, supervisada en España por la **AESIA**. Clasifica el riesgo en:
  * *Inaceptable:* Prohibición total de sistemas que amenacen los derechos europeos, como el *social scoring* (Puntuación ciudadana según comportamiento para denegar servicios) o la vigilancia biométrica masiva en tiempo real
  * *Alto Riesgo:* Exige **supervisión humana** (*Human-in-the-loop*). Aplica a algoritmos municipales de RRHH (decidan entre curriculums en procesos selectivos) o ayudas sociales.
  * *Limitado / Mínimo:* Obligación de transparencia (informar de que hablas con un bot) y filtros de antispam.

## 6. Conclusión
La IA representa el salto definitivo hacia la Administración proactiva. Su despliegue técnico en Ayuntamientos (mediante Machine Learning o arquitecturas RAG) debe estar blindado por el ENS frente a ataques como el *Prompt Injection*, y supeditarse al *AI Act* para asegurar que los aspectos éticos y la garantía jurídica del ciudadano permanezcan siempre bajo supervisión del empleado público.

------------------------------

# Tema 50. Inteligencia Artificial: Conceptos básicos, tecnologías fundamentales y aplicaciones prácticas. Aprendizaje automático (*Machine Learning*). Aprendizaje supervisado, no supervisado y por refuerzo. *Deep Learning*. Aspectos éticos.

## 1. Introducción

- La **IA** tecnología más transformadora de nuestra era que permite automatizar tareas, analizar datos y apoyar la toma de decisiones.
- En la Administración Pública: predicción, automatización y atención al ciudadano.
- Riesgos: **sesgos, falta de transparencia, errores y problemas jurídicos/éticos**.
- Marco europeo principal: **AI Act (Reglamento UE 2024/1689)**.

## 2. Conceptos Básicos y Tecnologías Fundamentales

### 2.1. Definición y clasificación de la IA

- **IA:** sistemas capaces de realizar tareas asociadas a la inteligencia humana.
- **Clasificación por capacidad:**
  - **IA Débil o Estrecha:** especializada en tareas concretas. → IA actual.
  - **IA Fuerte / AGI:** inteligencia general equivalente a la humana. → Teórica.
- **Clasificación por enfoque:**
  - **IA Simbólica:** reglas y lógica preprogramadas. Menos recursos
  - **IA Conexionista/Estadística:** aprendizaje a partir de datos → **Machine Learning**.

### 2.2. Tecnologías Fundamentales

- **Procesamiento de Lenguaje Natural (PLN/NLP):** comprensión y generación de lenguaje natural. Base de los asistentes virtuales y los Grandes Modelos de Lenguaje (LLM).
- **Visión Artificial:** análisis de imágenes y vídeos. Reconocimiento facial, diagnóstico médico por imagen y control de calidad industrial
- **Robótica e IoT:** sensores + IA para actuar sobre el entorno.
- **Sistemas Expertos:** conocimientos + hechos + motor de inferencia. Emulan experto humano
- **Sistemas Recomendadores:** personalización mediante predicciones.
- **Agentes Inteligentes:** sistemas autónomos que interactúan con su entorno.

### 2.3. Aplicaciones Prácticas en el Sector Público

- **Análisis predictivo:** fraude fiscal, demanda de servicios, riesgos.
- **Chatbots:** atención y orientación al ciudadano.
- **Automatización inteligente:** clasificación y distribución automática de expedientes.
- **RPA - Automatización Robótica de Procesos Cognitiva:** automatización de procesos mediante IA. Ej. Procesar facturas.

**La irrupción de los Copilotos*** La integración de IA en el puesto de trabajo (como Microsoft 365 Copilot) obliga al departamento de Sistemas a gobernar estrictamente los permisos del *Active Directory*. El objetivo es evitar que la IA indexe y exponga accidentalmente documentos internos confidenciales a usuarios sin privilegios.

## 3. Aprendizaje Automático (*Machine Learning*)

- **ML:** sistemas que aprenden patrones a partir de datos sin programar explícitamente todas las reglas.
- Proceso:
  - Datos → entrenamiento → modelo predictivo → predicción.

### 3.1. Componentes clave de un sistema ML

- **Dataset:** conjunto de datos de entrenamiento y prueba.
- **Features:** variables de entrada del modelo.
- **Función de coste (Loss Function):** mide el error respecto al resultado esperado.
- **Optimizador:** ajusta el modelo para reducir el error.
- **Preprocesado:** limpieza de datos, transformación, normalización y reducción de dimensionalidad.

## 4. Tipos de Aprendizaje

### 4.1. Aprendizaje Supervisado

- Utiliza **datos etiquetados** con la respuesta correcta.
- **Clasificación:** predice categorías.
  - Ej.: expediente → aprobado/desestimado.
  - Algoritmos: Regresión Logística, *Support Vector Machines* SVM, Random Forest, Naive Bayes.
- **Regresión:** predice valores numéricos.
  - Ej.: ingresos tributarios.
  - Algoritmo: Regresión Lineal.

### 4.2. Aprendizaje No Supervisado

- Utiliza **datos sin etiquetar**.
- Busca patrones y estructuras ocultas.
- **Clustering:** agrupa datos similares.
  - Algoritmos: K-Means, DBSCAN, clustering jerárquico.
- **Reducción de dimensionalidad:** reduce variables manteniendo información.
  - Algoritmos: Análisis de Componentes Principales (PCA), Descomposición en Valores Singulares (SVD).

### 4.3. Aprendizaje por Refuerzo (*Reinforcement Learning*)

- Un **agente** interactúa con un **entorno** mediante prueba y error.
- Recibe **recompensas o penalizaciones**.
- Objetivo: maximizar la recompensa acumulada.
- Aplicaciones: robótica, conducción autónoma, juegos y optimización.
- Otros:
  - **Semisupervisado:** combina datos etiquetados y no etiquetados.
  - **Multi-tarea:** un modelo aprende varias tareas.

## 5. *Deep Learning* (Aprendizaje Profundo)

- Subconjunto del **Machine Learning** basado en **redes neuronales profundas**.
- Aprende automáticamente las características relevantes de los datos.
- Especialmente eficaz con **imágenes, audio y texto**.

### 5.1. Arquitecturas Fundamentales

- **CNN:** visión artificial e imágenes. Ej. reconocimiento de matrículas
- **Redes Recurrentes (RNN) y LSTM:** secuencias y series temporales. Ej. traducción de habla, predicción meteorológica
- **Transformadores:** procesamiento de lenguaje y **LLM (Large Language Model )**.
  - Utilizan **self-attention** para analizar el contexto.

### 5.2. IA Generativa (GenAI)

- Genera contenido nuevo: **texto, imágenes, audio y vídeo**.
- **LLM:** modelos de lenguaje basados principalmente en Transformers.
- Trabajan con **tokens**.
- Técnicas:
  - **Prompt Engineering:** diseño de instrucciones.
    - **Zero-shot:** sin ejemplos.
    - **One-shot:** un ejemplo.
    - **Few-shot:** varios ejemplos.
  - **Generación Aumentada por Recuperación (RAG):** combina el modelo con información externa recuperada de bases de datos/documentos.

* Un Ayuntamiento *nunca* debe introducir datos ciudadanos en LLMs públicos. La solución táctica actual es desplegar arquitecturas **RAG (Generación Aumentada por Recuperación)** en nubes soberanas. Así, el modelo de IA solo tiene permiso para "leer" y generar respuestas basándose exclusivamente en el repositorio documental del propio Ayuntamiento (como sus normativas u ordenanzas), garantizando la total privacidad.

## 6. Aspectos Éticos y Marco Legal Europeo

### 6.1. Riesgos Éticos de la IA en la Administración

- **Sesgo algorítmico (*Bias*):** los modelos pueden reproducir o amplificar sesgos de los datos.
- **Caja negra:** dificultad para explicar determinadas decisiones.
- **Alucinaciones:** generación de información falsa presentada como verdadera.

- **Prompt Injection:** manipulación de las instrucciones de un modelo (recogido por OWASP como uno de los principales riesgos).
- **Data Poisoning:** Ataques donde los ciberdelincuentes "envenenan" los datos de entrenamiento para alterar el comportamiento futuro del modelo.

- **Impacto laboral:** automatización y transformación de puestos de trabajo.
- **Impacto ambiental:** elevado consumo energético de algunos modelos.

### 6.2. El Marco Legal Europeo: AI Act (Reglamento UE 2024/1689)

- Clasifica los sistemas de IA según su **nivel de riesgo**:

- **Riesgo inaceptable:**
  - *Prohibición total* de sistemas que amenacen los derechos europeos.
  - Ej.: *social scoring* (sistemas de crédito social) o la vigilancia biométrica masiva en tiempo real.

- **Alto riesgo:**
  - Permitido con fuertes obligaciones.
  - Exigen evaluación de riesgos, registro, trazabilidad y **supervisión humana obligatoria** (*Human-in-the-loop*)
  - Ej.: asignación de ayudas sociales

- **Riesgo limitado:**
  - Principalmente obligaciones de **transparencia**.
  - Ej.: informar al ciudadano de que interactúa con un chatbot.

- **Riesgo mínimo:**
  - Sin obligaciones específicas relevantes.
  - Ej.: filtros de spam.

- En España destaca la **AESIA (Agencia Española de Supervisión de la Inteligencia Artificial)** como organismo de supervisión.

## 7. Conclusión

La adopción de la IA —desde el *Machine Learning* predictivo hasta la revolución del *Deep Learning* generativo— transforma los paradigmas de tramitación estática en el sector público, capacitando a la Administración para predecir, automatizar tareas masivas y ofrecer interacción inmediata al ciudadano.

- Su aplicación debe garantizar:
  - **Transparencia.**
  - **Ausencia de discriminación.**
  - **Seguridad.**
  - **Supervisión humana.**
  - **Cumplimiento del AI Act.**