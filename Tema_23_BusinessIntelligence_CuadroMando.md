# Tema 23.- Cuadros de Mando. Análisis, implantación y explotación.

## 1. Introducción

Las Administraciones Públicas generan diariamente volúmenes ingentes de datos transaccionales: altas y bajas en el padrón, liquidaciones tributarias, expedientes de licencias, sanciones de tráfico, consumos de agua. Sin embargo, un registro individual en la base de datos constituye un **dato bruto**: un hecho atómico sin contexto por sí mismo.

Para que los órganos de gobierno municipal (Alcaldía, Pleno, Intervención, Jefaturas de Servicio) tomen decisiones informadas y estratégicas, los datos brutos deben transformarse en **información** (datos contextualizados y relacionados) y, a su vez, en **conocimiento** (patrones, tendencias y predicciones que orientan la acción). El **Business Intelligence (BI)** y los **Cuadros de Mando (Dashboards)** son las herramientas que materializan esta transformación.

## 2. Business Intelligence (BI): De los Datos al Conocimiento

### 2.1. Concepto

**Business Intelligence (BI)** es el conjunto de estrategias, tecnologías, arquitecturas y metodologías que permiten transformar datos transaccionales brutos (sistemas OLTP) en información estructurada para el análisis y la toma de decisiones (sistemas OLAP).

### 2.2. Arquitectura BI

El flujo de datos en una arquitectura BI sigue el siguiente recorrido:

1.  **Fuentes de datos (OLTP):** Bases de datos operacionales (Oracle del padrón, Oracle de tributos, sistemas de gestión de expedientes) que registran las transacciones diarias.

2.  **Proceso ETL (Extract, Transform, Load):**
    *   **Extract:** Extracción de datos de las diversas fuentes operacionales.
    *   **Transform:** Limpieza, normalización, integración y enriquecimiento de los datos (corregir formatos de fechas, unificar codificaciones de calles, calcular métricas derivadas).
    *   **Load:** Carga de los datos transformados en el almacén analítico.

3.  **Data Warehouse (Almacén de Datos):** Base de datos dimensional optimizada para consultas analíticas complejas, no para transacciones individuales.

4.  **Capa de visualización:** Cuadros de Mando, informes, herramientas de exploración de datos.

### 2.3. Modelado dimensional: Estrella y Copo de Nieve

Los Data Warehouses utilizan modelos dimensionales en lugar del modelo relacional normalizado de los sistemas OLTP:

*   **Esquema en Estrella (Star Schema):** Una tabla central de **hechos** (fact table) que almacena las métricas cuantificables (importe recaudado, número de expedientes) rodeada de tablas de **dimensiones** desnormalizadas (tiempo, contribuyente, tipo de tributo, distrito).

*   **Esquema en copo de nieve (Snowflake Schema):** Variante del esquema en estrella donde las dimensiones están normalizadas (la dimensión "ubicación" se descompone en tablas separadas de calle, barrio, distrito, municipio).

### 2.4. OLTP vs. OLAP

| Característica | OLTP (Transaccional) | OLAP (Analítico) |
|---------------|---------------------|-------------------|
| Propósito | Operaciones diarias | Análisis y decisiones |
| Consultas | Simples, predefinidas | Complejas, ad-hoc |
| Datos | Actuales, detallados | Históricos, agregados |
| Modelo | Normalizado (3FN) | Dimensional (estrella) |
| Usuarios | Operadores, funcionarios | Directivos, analistas |
| Ejemplo | INSERT INTO padron... | Total de altas por distrito y año |

## 3. El Cuadro de Mando (Dashboard)

### 3.1. Concepto

Un **Cuadro de Mando (Dashboard)** es una interfaz gráfica que consolida, en una única vista, los indicadores clave de rendimiento (KPI) de la organización, permitiendo a los responsables evaluar de un vistazo el estado actual de la gestión y detectar tendencias, anomalías y oportunidades de mejora.

### 3.2. Tipología de Cuadros de Mando

#### Cuadro de Mando Operativo (CMO)

*   **Destinatarios:** Personal técnico y jefes de servicio.
*   **Horizonte temporal:** Corto plazo (diario a semanal).
*   **Actualización:** Frecuente (en tiempo real o diaria).
*   **Ejemplo:** Dashboard del servicio de Policía Local que muestra patrullas activas, incidencias abiertas, tiempos de respuesta y niveles de ocupación del calabozo.

#### Cuadro de Mando Táctico o Directivo (CMD)

*   **Destinatarios:** Directores de área y gerentes de departamento.
*   **Horizonte temporal:** Medio plazo (mensual a trimestral).
*   **Actualización:** Semanal o mensual.
*   **Ejemplo:** Dashboard de Recursos Humanos que muestra evolución del absentismo por departamento, tasa de temporalidad, presupuesto ejecutado vs. presupuestado.

#### Cuadro de Mando Integral (CMI) — Balanced Scorecard

Desarrollado por **Robert Kaplan y David Norton** (1992), el CMI trasciende la visión puramente financiera y evalúa la estrategia de la organización a través de **cuatro perspectivas** equilibradas:

1.  **Perspectiva Financiera:** ¿Cumplimos los objetivos presupuestarios?
    *   KPI: Porcentaje de ejecución presupuestaria, ratio de recaudación voluntaria, ahorro neto.

2.  **Perspectiva del Ciudadano (Cliente):** ¿Satisfacemos las expectativas ciudadanas?
    *   KPI: Índice de satisfacción ciudadana, tiempo medio de resolución de quejas, porcentaje de trámites completados online.

3.  **Perspectiva de Procesos Internos:** ¿Son eficientes nuestros procesos?
    *   KPI: Tiempo medio de tramitación de licencias, porcentaje de expedientes resueltos en plazo, tasa de automatización de procedimientos.

4.  **Perspectiva de Aprendizaje e Innovación:** ¿Mejora la organización?
    *   KPI: Horas de formación por empleado, porcentaje de servicios digitalizados, inversión en modernización tecnológica.

## 4. Elementos Constructivos del Dashboard

### 4.1. KPI (Key Performance Indicator)

Un **KPI** es una métrica cuantificable, directamente vinculada a un objetivo estratégico de la organización. No toda métrica es un KPI: el número total de registros en la base de datos es una métrica; el porcentaje de recaudación tributaria sobre el presupuestado es un KPI.

**Características de un buen KPI:**
*   Vinculado a un objetivo estratégico concreto.
*   Cuantificable y medible objetivamente.
*   Acotado en el tiempo (trimestral, anual).
*   Comparable (respecto al periodo anterior o a un objetivo).
*   Accionable (si el KPI cae, se sabe qué acción tomar).

### 4.2. Representaciones gráficas (Data Visualization)

| Tipo de gráfico | Uso adecuado |
|-----------------|-------------|
| **Velocímetro / Gauge** | KPI con objetivo claro (recaudación vs. presupuesto) |
| **Semáforo (Rojo/Ámbar/Verde)** | Estado rápido de indicadores críticos |
| **Gráfico de barras** | Comparación entre categorías (recaudación por distrito) |
| **Gráfico de líneas** | Evolución temporal (tendencia de licencias por mes) |
| **Gráfico circular** | Distribución proporcional (tipos de quejas ciudadanas) |
| **Mapa de calor (Heatmap)** | Distribución geográfica de incidencias o datos |
| **Tabla con sparklines** | Datos detallados con microtendencias incrustadas |

### 4.3. Interactividad: Drill-Down y Slicers

*   **Drill-Down (Desglose):** Capacidad de navegar desde un nivel agregado hacia niveles de detalle progresivamente más granulares. Ejemplo: de "Recaudación total del municipio" → "Recaudación por distrito" → "Recaudación por tipo de tributo en el distrito 3" → "Detalle de contribuyentes morosos en IBI del distrito 3".

*   **Slicers (Filtros interactivos):** Controles que permiten al usuario filtrar dinámicamente los datos del dashboard por dimensiones temporales (año, trimestre, mes), geográficas (distrito, barrio) o categoriales (tipo de tributo, estado del expediente).

## 5. Fases de Implantación de un Cuadro de Mando en Ayuntamientos

1.  **Auditoría de requisitos estratégicos:** Identificar los objetivos estratégicos del Plan de Gobierno Municipal y las necesidades de información de cada nivel directivo.

2.  **Inventario de fuentes de datos:** Catalogar las bases de datos operacionales disponibles (padrón, tributos, expedientes, contabilidad, recursos humanos) y evaluar la calidad de los datos.

3.  **Diseño del modelo dimensional:** Definir las tablas de hechos y dimensiones del Data Warehouse, los procesos ETL y las métricas y KPIs a calcular.

4.  **Desarrollo ETL:** Implementar los procesos de extracción, transformación y carga mediante herramientas como Oracle Data Integrator (ODI), Talend, Apache NiFi o SSIS.

5.  **Construcción del Dashboard:** Diseñar y desarrollar las visualizaciones interactivas mediante herramientas como Power BI, Tableau, QlikView/Qlik Sense, Grafana o Oracle Analytics.

6.  **Validación con usuarios finales:** Testear el dashboard con los destinatarios reales (concejales, directores de área) para verificar que las métricas son comprensibles y útiles.

7.  **Despliegue, formación y mantenimiento:** Poner en producción, formar a los usuarios y establecer un proceso de mejora continua del dashboard.

## 6. Herramientas de BI y Visualización

| Herramienta | Características |
|-------------|----------------|
| **Microsoft Power BI** | Integración con el ecosistema Microsoft, DAX para cálculos, licenciamiento asequible |
| **Tableau** | Visualización avanzada, drag-and-drop, amplia comunidad |
| **QlikView / Qlik Sense** | Motor asociativo in-memory, análisis exploratorio |
| **Oracle Analytics Cloud** | Integración nativa con Oracle Database, análisis predictivo |
| **Grafana** | Open source, excelente para métricas operacionales y de infraestructura |
| **Apache Superset** | Open source, consultas SQL, dashboards interactivos |

## 7. Conclusión

Los Cuadros de Mando transforman los datos brutos almacenados en las bases de datos transaccionales en herramientas de gestión estratégica. El Cuadro de Mando Integral (Balanced Scorecard), con sus cuatro perspectivas (financiera, ciudadano, procesos internos, aprendizaje), proporciona una visión holística del rendimiento de la organización que trasciende la mera supervisión financiera.

Su implantación en las Administraciones Públicas requiere una arquitectura BI completa (fuentes OLTP → ETL → Data Warehouse → Dashboard) sustentada por herramientas de visualización modernas y un proceso iterativo de diseño centrado en las necesidades reales de los tomadores de decisiones municipales.
