# Tema 2.- Metodologías predictivas para la gestión de proyectos: GANTT, PERT. Metodologías ágiles para la gestión de proyectos. Metodologías lean. Herramientas digitales de colaboración y productividad. Herramientas de gestión de proyectos.

## 1. Introducción a la Dirección de Proyectos Tecnológicos
Necesidad de metodologías formales para asegurar: Cumplimiento de plazos, Presupuesto, Viabilidad técnica y Satisfacción ciudadana.
Evolución desde  Metodologías Predictivas (clásicas): lineales e industriales hacia Metodologías Ágiles y filosofías Lean: marcos flexibles, iterativos y adaptativos.

## 2. Metodologías Predictivas para la Gestión de Proyectos
Las metodologías predictivas o modelo "en cascada" (Waterfall): Requisitos estables, predecibles y y definidos con un costo elevadísimo de cambio.

La planificación exhaustiva antes de programar. Necesario uso herramientas: el diagrama de GANTT y el método PERT.

### 2.1. El Diagrama de GANTT
Henry L. Gantt (principios del siglo XX), herramienta gráfica más usada. 
Cronograma de un proyecto en eje temporal:
*   **Eje Horizontal (X):** Calendario del proyecto.
*   **Eje Vertical (Y):** Desglose de Trabajo (WBS - Work Breakdown Structure) / tareas.

**Componentes esenciales del GANTT:**
1.  **Barras de tarea:** Fecha de inicio, Duración y Fecha Fin.
2.  **Dependencias lógicas:** Líneas que conectan tareas y definen requisitos y dependencias
3.  **Hitos (Milestones):** Rombos con fechas límite de entrega o eventos importantes. 
4.  **Porcentaje de completitud:** Progreso real frente al planificado.

### 2.2. Técnicas PERT (Program Evaluation and Review Technique)
Gantt problemas al plasmar dependencias -> PERT (Armada EEUU en 1958 para misil Polaris)
Uso: Analizar tareas involucradas en proyecto mediante un "Diagrama de Red".

Estimación probabilística:
1.  **Tiempo Optimista ($T_o$):** Tiempo mínimo ejecución tarea en perfectas condiciones.
2.  **Tiempo Pesimista ($T_p$):** Tiempo máximo ejecución tarea retrasos razonables.
3.  **Tiempo Más Probable ($T_m$):** Tiempo de ejecución más probable.
Tiempo Esperado $T_e = \frac{T_o + 4(T_m) + T_p}{6}$.

**CPM (Critical Path Method - Método de la Ruta Crítica)**. El Camino Crítico: secuencia que marca duración mínima total. "Holgura" (Slack o Float) de tareas igual a cero.

## 3. Metodologías Ágiles para la Gestión de Proyectos
Fracasos usando metodología predictiva: Software voluble, abstracto y sujeto a muchos cambios -> producto obsoleto.
**Manifiesto Ágil (Agile Manifesto)** (2001) primando:
*   Individuos / Interacciones.
*   Software funcional.
*   Colaboración cliente.
*   Respuesta inmediata / Adaptación al cambio.

**Desarrollo Iterativo e Incremental**. Proyecto dividido en ciclo (iteración) se produce, inspecciona y entrega software vivo y funcional (incremento) -> Reduce riesgo  y permite modificación de requisitos en vuelo.

### 3.1. El Marco de Trabajo Scrum
Marco ágil más usado. Empírico y ligero para entornos de alta incertidumbre.
Fundamentado: Transparencia, Inspección y Adaptación.

**Roles oficiales en Scrum:**
1.  **Product Owner (Propietario del Producto):** Cliente que gestiona el *Product Backlog*.
2.  **Scrum Master:** Líder o facilitador. Eliminar impedimentos burocráticos.
3.  **Developers (Desarrolladores):** Equipo autogestionado. Responsables crear incrementos de software.

**Eventos y Artefactos en Scrum:**
*   **El Sprint:** Creación de incrementos de software (generalmente de 2 a 4 semanas).
*   **Product Backlog:** Inventario historias de usuario priorizadas y ordenadas. Mantenido por Product Owner.
*   **Sprint Planning:** Reunión Equipo inicio Sprint -> decidir elementos del Backlog. *Sprint Backlog* (Plan táctico Sprint en curso).
*   **Daily Scrum:** Reunión diaria, rápida (15 min), ver progresos.
*   **Sprint Review y Sprint Retrospective:** Software al cliente (Review) y auditar mejoras equipo (Retrospective).

### 3.2. Kanban

Gestión visual del flujo de trabajo. Propósito -> optimizar entrega tareas limitando cantidad de trabajo simultáneo.

#### Orígenes y Principios Fundamentales

Basado en **filosofía Lean** y *Just-in-Time*, con **flujo continuo** (*sistema Pull*), tareas avanzan con el equipo sin plazos fijos de Scrum.

Los principios básicos:

* **Visualizar el trabajo:** **tablero** en columnas con estados tareas (*Por hacer, En proceso, Hecho*).
* **Limitar el trabajo en curso (*WIP - Work in Progress*):** Restricción estricta tareas abiertas -> evitar multitarea
* **Gestionar el flujo:** Seguimiento para detectar bloqueos y medir tiempos de entrega (*Cycle Time*).
* **Políticas explícitas:** Definición reglas para pasar una tarea de una fase a otra.
* **Mejora continua (*Kaizen*):** Revisión periódica para evolucionar y optimizar la eficiencia.

#### Funcionamiento Operativo

1. **Tarjetas:** Unidades de trabajo (historias de usuario, tareas) con descripción, estado y responsable.
2. **Columnas:** Ciclo de vida tareas.
3. **Límites WIP:** Asignación limite de tareas -> previenen saturación y garantizar entrega sostenible.

## 4. Metodologías Lean
Nace manufactura automovilística japonesa -> "Toyota Production System (TPS)". Objetivo: **Eliminar el Desperdicio (Muda)** y Maximizar valor entregado.
2000 se implementó en software corporativo "Lean Software Development".

### 4.1. Principios de Lean en el Software
1.  **Eliminar el Desperdicio:** No aporta valor directo al desarrollo.
2.  **Integrar la Calidad (Built-in Quality):**  TDD (Test Driven Development) en el desarollo -> prevenir vulnerabilidades.
3.  **Amplificar el Aprendizaje continuo:** Refactorizaciones iterativas de código e Integración Continua (CI)
4.  **Decidir lo más Tarde Posible y Entregar lo más Rápido Posible:** Retrasar decisiones hasta disponer información técnica / Entregar código rapida e ininterrumpidamente.
5.  **Optimizar el Todo (Holismo):** Proyecto como cadena valor.

## 5. Herramientas Digitales de Colaboración y Productividad
Teletrabajo -> reemplazo interacciones físicas por colaboración digital grupal.

**Plataformas de Comunicación y Hubs Corporativos:**
*   **Microsoft Teams:** Estándar. Integración AD. Llamadas VoIP canales temáticos, pizarras compartidas (Whiteboard) y edición colaborativa de documentos.
*   **Slack:** Mensajería asincrónica. Canales, Webhooks y flujos DevOps (Desarrollo y Operaciones).

**Gestión de Conocimiento y Bases Documentales Colaborativas institucionales:**
*   **Atlassian Confluence, Microsoft SharePoint o Notion** Intranets tipo Wiki. Documentación electrónica y actas de reuniones.

## 6. Herramientas de Gestión de Proyectos (Software PPM)

1.  **Herramientas para Metodologías Predictivas (Gantt/PERT):**
    *   **Microsoft Project:** Estándar. Gantt.
    *   **Primavera P6 (Oracle):** Mega-proyectos de alta complejidad.
2.  **Herramientas para Metodologías Ágiles y Lean (Scrum/Kanban):**
    *   **Jira Software (Atlassian):** Integra tableros Kanban, límites de Trabajo en Curso (WIP Limits). Facilita Sprints, Burn-down charts y gestión de incidencias.
    *   **Trello / Asana / Monday.com:** Sistemas ligeros visuales, basados en Kanban. Orientados departamentos no puramente tecnológicos.
    *   **GitLab / GitHub / Azure DevOps:** Integración mediante integración continua (CI/CD).

## 7. Conclusión Normativa y Técnica Institucional
Coexistencia entre lo Predictivo / Cascada (Proyectos hardware físico)  y lo Empírico / Agile (Proyectos software lógico).
