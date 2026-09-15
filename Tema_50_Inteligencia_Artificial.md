# Tema 50.- Inteligencia Artificial: Conceptos, Machine Learning, Deep Learning y Ética (AI Act).

## 1. Introducción
* **El cambio de paradigma:** La IA permite a la Administración transitar de un modelo *reactivo* (esperar la solicitud) a uno *predictivo* y automatizado.
* **El reto:** Delegar decisiones en algoritmos exige un control riguroso para evitar sesgos discriminatorios y garantizar el estricto cumplimiento del Esquema Nacional de Seguridad (ENS) y el RGPD.

## 2. Tecnologías Fundamentales y el Puesto de Trabajo TIC
* **2.1. Enfoques:** 
  * *IA Simbólica:* Basada en reglas lógicas preprogramadas (Sistemas Expertos clásicos).
  * *IA Conexionista:* Basada en datos y estadística, que es lo que hoy conocemos como **Machine Learning**.
* **2.2. Tecnologías clave:** El Procesamiento de Lenguaje Natural (NLP), la Visión Artificial y la RPA Cognitiva para la automatización de tareas burocráticas repetitivas.
* **2.3. La irrupción de los Copilotos (Reto de Microinformática):** La integración de IA en el puesto de trabajo (como Microsoft 365 Copilot) obliga al departamento de Sistemas a gobernar estrictamente los permisos del *Active Directory*. El objetivo es evitar que la IA indexe y exponga accidentalmente documentos internos confidenciales a usuarios sin privilegios.

## 3. Machine Learning (El motor estadístico)
En este paradigma, el sistema aprende infiriendo reglas matemáticas a partir de datos históricos, en lugar de ser programado de forma explícita. Se divide en tres enfoques principales:

* **Aprendizaje Supervisado:** El modelo se entrena utilizando datos *etiquetados* que ya contienen la respuesta correcta. En un Ayuntamiento, un caso de uso clásico de clasificación sería predecir si una beca será aprobada o denegada basándose en el histórico de concesiones.
* **Aprendizaje No Supervisado:** Utiliza datos *sin etiquetar* para que el algoritmo busque patrones ocultos por sí mismo (Clustering). A nivel municipal, es ideal para agrupar perfiles de contribuyentes con el fin de detectar anomalías fiscales o fraude.
* **Aprendizaje por Refuerzo:** El sistema aprende por *prueba y error* interactuando con un entorno, recibiendo recompensas o penalizaciones. Su aplicación estrella es la optimización, como la sincronización inteligente de la red de semáforos de la ciudad según el tráfico en tiempo real.

## 4. Deep Learning e IA Generativa (GenAI)
Es el subconjunto de la IA que utiliza **Redes Neuronales Profundas** (con múltiples capas ocultas), capaces de extraer características automáticamente de datos crudos como imágenes o texto.

* **4.1. Arquitecturas principales:** Destacan las Redes Convolucionales (CNN) para visión artificial (como la lectura de matrículas policiales), las Recurrentes (RNN) para series temporales, y los **Transformers**, que son el motor de los grandes modelos de lenguaje (LLMs) modernos.
* **4.2. El entorno corporativo (RAG vs. Nube Pública):** Un Ayuntamiento *nunca* debe introducir datos ciudadanos en LLMs públicos. La solución táctica actual es desplegar arquitecturas **RAG (Generación Aumentada por Recuperación)** en nubes soberanas. Así, el modelo de IA solo tiene permiso para "leer" y generar respuestas basándose exclusivamente en el repositorio documental del propio Ayuntamiento (como sus normativas u ordenanzas), garantizando la total privacidad.

## 5. Riesgos Éticos y Ciberseguridad en la IA
* **5.1. Riesgos Éticos:**
  * **Sesgo algorítmico (Bias):** Si los datos históricos son discriminatorios, la IA perpetuará y amplificará esa discriminación en sus predicciones.
  * **Caja Negra (Opacidad):** Es la imposibilidad de explicar jurídicamente *por qué* una red neuronal ha tomado una decisión, lo que vulnera el principio de motivación de las resoluciones administrativas.
  * **Alucinaciones:** Ocurre cuando el modelo de lenguaje inventa normativas o hechos falsos, pero los presenta con total coherencia gramatical.
* **5.2. Ciberriesgos emergentes (Alertas OWASP / CCN-CERT):**
  * **Prompt Injection:** Consiste en manipular las instrucciones del Chatbot municipal para que revele información privada o se salte sus reglas de seguridad.
  * **Data Poisoning:** Ataques donde los ciberdelincuentes "envenenan" los datos de entrenamiento para alterar el comportamiento futuro del modelo.

## 6. Marco Legal: El AI Act (Reglamento UE 2024/1689)
Es el primer marco mundial que regula la Inteligencia Artificial basándose en niveles de riesgo. En España, su supervisión recae sobre la recién creada **AESIA**. Clasifica los sistemas en cuatro niveles:

* **Riesgo Inaceptable:** Implica la *prohibición total* de sistemas que amenacen los derechos europeos, como el *social scoring* (sistemas de crédito social) o la vigilancia biométrica masiva en tiempo real.
* **Alto Riesgo:** Están permitidos, pero exigen una evaluación estricta, registro, trazabilidad y, fundamentalmente, la **supervisión humana obligatoria** (*Human-in-the-loop*). En un Ayuntamiento, esto aplica a algoritmos que decidan sobre ayudas sociales o que filtren currículums en procesos selectivos de Recursos Humanos.
* **Riesgo Limitado:** Su obligación principal es la *transparencia*; es decir, informar obligatoriamente al ciudadano de que está interactuando con una máquina, como ocurre con un Chatbot de la Sede Electrónica.
* **Riesgo Mínimo:** Engloba herramientas sin obligaciones restrictivas, como los filtros antispam del correo corporativo.

## 7. Conclusión
La Inteligencia Artificial representa el salto definitivo hacia la Administración proactiva y automatizada. No obstante, para que su adopción técnica —mediante algoritmos de Machine Learning o arquitecturas RAG corporativas— sea viable en el Sector Público, el despliegue debe estar blindado por el Esquema Nacional de Seguridad frente a vulnerabilidades como el *Prompt Injection*. Además, debe supeditarse estrictamente al Reglamento *AI Act*, garantizando que las decisiones críticas mantengan en todo momento la supervisión y la garantía jurídica del empleado público.

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