# Tema 23.- Cuadros de Mando. Análisis, implantación y explotación.

## 1. Introducción

Las Administraciones Públicas generan gran volumen de datos transaccionales: altas y bajas en el padrón, liquidaciones tributarias, expedientes de licencias, sanciones de tráfico, consumos de agua). Para que los órganos de gobierno municipal tomen decisiones, los datos brutos deben transformarse en **conocimiento** (patrones, tendencias y predicciones). 
El **Business Intelligence (BI)** y los **Cuadros de Mando (Dashboards)** son las herramientas que materializan esta transformación.

## 2. Business Intelligence (BI): De los Datos al Conocimiento

### 2.1. Concepto

**Business Intelligence (BI)** transformar datos transaccionales brutos (sistemas OLTP) en información estructurada para el análisis y la toma de decisiones (sistemas OLAP).

### 2.2. Arquitectura BI

El flujo de datos:

1.  **Fuentes de datos (OLTP):** Bases de datos operacionales que registran las transacciones diarias.
2.  **Proceso ETL (Extract, Transform, Load):**
3.  **Data Warehouse (Almacén de Datos):** Base de datos dimensional optimizada para consultas analíticas complejas
4.  **Capa de visualización:** Cuadros de Mando (*Dashboards*) e informes.

### 2.3. Modelado dimensional

*   **Esquema en Estrella (Star Schema):** Una tabla central de **hechos** (fact table) que almacena las métricas cuantificables (importe recaudado, número de expedientes) rodeada de tablas de **dimensiones** desnormalizadas (contribuyente, tipo de tributo, distrito).
*   **Esquema en copo de nieve (Snowflake Schema):** Variante del esquema en estrella donde las dimensiones están normalizadas (la dimensión "ubicación" se descompone en tablas separadas de calle, barrio, distrito, municipio).

### 2.4. OLTP vs. OLAP

* **OLTP (Transaccional):** Operaciones diarias, consultas simples/predefinidas, datos actuales al detalle, modelo **normalizado (3FN)**, usuarios operativos. (*Ej: Alta en el padrón*).
* **OLAP (Analítico):** Toma de decisiones, consultas complejas/ad-hoc, datos históricos/agregados, modelo **dimensional**, usuarios directivos/analistas. (*Ej: Evolución de altas por distrito en 5 años*)

## 3. El Cuadro de Mando (Dashboard)

### 3.1. Concepto

Un **Cuadro de Mando (Dashboard)** es una interfaz gráfica que consolida, en una única vista, los indicadores clave de rendimiento (KPI) de la organización, permitiendo evaluar el estado actual de la gestión y detectar tendencias, anomalías y oportunidades de mejora.

### 3.2. Tipología de Cuadros de Mando

#### Cuadro de Mando Operativo (CMO)

*   **Destinatarios:** Personal técnico y jefes de servicio.
*   **Horizonte temporal:** Corto plazo (diario a semanal).
*   **Objetivo:** controlar la **operativa diaria**.

#### Cuadro de Mando Táctico o Directivo (CMD)

*   **Destinatarios:** Directores de área y gerentes de departamento.
*   **Horizonte temporal:** Medio plazo (mensual a trimestral).
*   **Ejemplo:** Dashboard de Recursos Humanos que muestra evolución del absentismo por departamento, tasa de temporalidad, presupuesto ejecutado vs. presupuestado.

#### Cuadro de Mando Integral (CMI) — Balanced Scorecard

*   **Kaplan y Norton (1992)**.
*   **Objetivo:** evaluar la **estrategia** mediante 4 perspectivas:
    1. **Financiera** → ¿Cumplimos los objetivos?. KPI: Porcentaje de ejecución presupuestaria
    2. **Ciudadano/Cliente** → ¿Satisfacemos al ciudadano?. KPI: Índice de satisfacción ciudadana
    3. **Procesos internos** → ¿Somos eficientes?. KPI: Tiempo medio de tramitación de licencias
    4. **Aprendizaje e innovación** → ¿Mejoramos?.  KPI: Horas de formación por empleado o inversión en modernización tecnológica

## 4. Elementos Constructivos del Dashboard

### 4.1. KPI (Key Performance Indicator)

KPI = métrica cuantificable vinculada a un objetivo estratégico.

**Características de un buen KPI:**
- **Estratégico** → vinculado a un objetivo.
- **Medible** → cuantificable objetivamente.
- **Temporal** → periodo definido.
- **Comparable** → con otros periodos/objetivos.
- **Accionable** → permite tomar decisiones.

**Métrica ≠ KPI** → Un KPI siempre está ligado a un objetivo.

### 4.2. Representaciones gráficas (Data Visualization)

- **Gauge / Velocímetro** → KPI vs. objetivo.
- **Semáforo** → estado del indicador.
- **Barras** → comparar categorías (recaudación por barrio).
- **Líneas** → evolución temporal (nº licencias urbanisticas por mes).
- **Circular** → proporciones (tipos de quejas ciudadanas).
- **Heatmap** → distribución/intensidad.
- **Sparklines** → microtendencias.

### 4.3. Interactividad: Drill-Down y Slicers

- **Drill-Down (Desglose)** → pasar de datos generales a mayor detalle.
  - Municipio → distrito → tributo → contribuyente.
- **Slicers** → filtros interactivos (fecha, zona, categoría...)

### 4.4. Seguridad (AAPP):** 

Anonimización de datos sensibles (RGPD) y control de acceso basado en roles (ENS).

## 5. Fases de Implantación de un Cuadro de Mando en Ayuntamientos

1. **Requisitos** → objetivos y necesidades.
2. **Fuentes** → identificar y evaluar datos.
3. **Modelo dimensional** → hechos, dimensiones, ETL y KPIs.
4. **ETL** → extraer, transformar y cargar.
5. **Dashboard** → crear visualizaciones.
6. **Validación** → comprobar con usuarios.
7. **Despliegue** → producción, formación y mantenimiento.

## 6. Herramientas de BI y Visualización

- **Power BI** → Microsoft + DAX (Data Analysis Expressions).
- **Tableau** → visualización avanzada.
- **Qlik** → Motor asociativo in-memory.
- **Oracle Analytics** → ecosistema Oracle.
- **Grafana** → métricas operacionales / infraestructura.
- **Superset** → open source + SQL.

## 7. Conclusión

Los Cuadros de Mando transforman los datos brutos almacenados en las bases de datos transaccionales en herramientas de gestión estratégica. 
Su implantación en las Administraciones Públicas requiere una arquitectura BI completa (fuentes OLTP → ETL → Data Warehouse → Dashboard) adaptándolo a las necesidades de quienes toman las decisiones.