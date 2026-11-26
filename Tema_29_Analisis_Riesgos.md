# Tema 29.- Análisis y gestión de riesgos. Herramientas.

## 1. Introducción
- **Seguridad de la Información (ENS):** Capacidad de los sistemas para resistir, de manera fiable, acciones que comprometan su Disponibilidad, Autenticidad, Integridad y Confidencialidad.
- **Gestión de Riesgos:** Principio básico del ENS (Art. 7) y proceso continuo que permite identificar qué proteger (activos), de qué protegerlo (amenazas) y equilibrar el riesgo asumido con el coste de protección. Se divide en dos fases: **Análisis** y **Tratamiento**.

## 2. Análisis de Riesgos: Metodología MAGERIT v3
Fase diagnóstica destinada a identificar activos, amenazas y calcular el nivel de exposición (riesgo residual). En las AAPP españolas, la metodología oficial es **MAGERIT v3**, estructurada en 6 fases secuenciales:
1. **Caracterización de Activos:** Identificar qué tiene valor (datos, servicios como BD Oracle o Padrón), valorarlos en las 5 dimensiones (**D-I-C-A-T**, de 0 a 5) y trazar sus *dependencias* (el servicio X depende del servidor Y).
2. **Caracterización de Amenazas:** Identificar qué puede causar daño (ransomware, error humano, inundación) explotando una **vulnerabilidad** (debilidad del sistema, ej. falta de parches). Se estima su *Frecuencia* y *Degradación*.
3. **Determinación del Impacto:** Medir el daño real producido sobre los activos si se materializa la amenaza.
4. **Determinación del Riesgo Potencial:** Probabilidad de que la amenaza explote la vulnerabilidad en escenario de "desprotección total" (*Riesgo = Probabilidad × Impacto*).
5. **Caracterización de Salvaguardas:** Identificar las medidas de protección ya existentes y evaluar su eficacia real.
6. **Estado de Riesgo:** Cálculo del **Riesgo Residual** (el riesgo que permanece *después* de aplicar las salvaguardas).
- **Informes resultantes:** Modelo de valor (activos y dependencias), Mapa de riesgos (amenazas y frecuencia), Declaración de Aplicabilidad / SoA (contramedidas) e Informe de Insuficiencias.
- **Marcos de Referencia:**
  * **ISO 27005:** Gestión de riesgos de seguridad TI (dentro del SGSI de ISO 27001).
  * **ISO 31000:** Gestión de riesgos global (no solo TI).

## 3. Gestión / Tratamiento del Riesgo (MATE)
Objetivo → reducir el riesgo hasta un nivel **aceptable** (riesgo residual). Estrategias **MATE**:
* **M**itigar (Reducir): Aplicar más salvaguardas (Ej: poner un firewall) para bajar el riesgo. Tipología: 
  * **Preventivos:** Evitan el incidente (Hardening, cifrado, formación, control de acceso, firewall / IPS Intrusion Prevention System).
  * **Detectivos:** Alertan del incidente (IDS - Intrusion Detection System, SIEM, auditoría de logs).
  * **Correctivos:** Recuperan la normalidad (Planes de contingencia, restauración de backups).
* **A**ceptar: Asumirlo (El coste del firewall es mayor que el daño).
* **T**ransferir: Pasarlo a un tercero (Ej: contratar un ciberseguro o un proveedor con SLA - Acuerdo de Nivel de Servicio).
* **E**vitar: Eliminar la causa (Ej: apagar un servidor obsoleto).
* **Nivel de exigencia ENS [op.pl.1]:** Básica (Informal), Media (Semiformal), Alta (Formal/Matemático).

## 4. Herramientas
El CCN-CERT provee la **Familia EAR** (*Entorno de Análisis de Riesgos*), estándar de facto en las AAPP:
  * **PILAR:** Versión completa. Automatiza MAGERIT y genera la SoA.
  * **PILAR Basic / μPILAR:** Orientadas a Administración Local. 
  * ⚠️ *Detalle de oro AAPP:* Permiten importar el **PCE-EELL** (Perfil de Cumplimiento Específico para Entidades Locales) y tienen módulos para hacer la **EIPD** exigida por el RGPD.

## 5. Conclusión
La gestión de riesgos es el motor de la seguridad. MAGERIT y PILAR transforman el miedo a las ciberamenazas en una toma de decisiones matemática y presupuestaria (MATE), garantizando el cumplimiento simultáneo del ENS y del RGPD en la Administración Local.

--------------

# Tema 29.- Análisis y gestión de riesgos. Herramientas.

## 1. Introducción

- ENS **Seguridad de la información** → capacidad de los sistemas para resistir, de manera fiable, acciones que comprometan:
  - Disponibilidad
  - Autenticidad
  - Integridad
  - Confidencialidad

- Las organizaciones están expuestas a **amenazas y vulnerabilidades**. El **análisis y gestión de riesgos** permite:
  - Identificar **qué proteger**.
  - Identificar **de qué protegerlo**.
  - Determinar **cuánto invertir** en protección.
- Es un proceso **continuo** y un principio básico del **ENS (art. 7)**.

## 2. Análisis y Gestión de Riesgos

### 2.1. Concepto

- **Gestión de riesgos** → identificar, analizar, evaluar y modificar los riesgos.
- Busca equilibrar:
    - **Riesgo asumido**
    - **Coste de las medidas de protección**

Se divide en:
    - **Análisis de riesgos**
    - **Tratamiento de riesgos**

### 2.2. Definiciones clave

- **Activo** → elemento que debe protegerse (información y servicios)
- **Amenaza** → causa potencial de daño (desastres naturales, errores intencionados y no intencionados)
- **Vulnerabilidad** → debilidad explotable por una amenaza (Software sin parchear, contraseñas débiles, falta de cifrado)
- **Riesgo** → probabilidad de que una amenaza cause daño.
  - **Riesgo = Probabilidad × Impacto**
- **Impacto** → daño producido (Pérdida económica, daño reputacional, sanción legal, interrupción del servicio)
- **Salvaguarda** → medida que reduce el riesgo (diminuyendo posibilidad o limitando el daño).
- **Riesgo residual** → riesgo que queda después de aplicar salvaguardas.

**Relación entre conceptos:** Los sistemas (activos) pueden tener vulnerabilidades que son explotables porque estamos expuestos a amenazas. La probabilidad de que esto ocurra y tenga efecto es el riesgo. El impacto es el daño que produce en la organización. Para minimizar el riesgo implantamos salvaguardas. El riesgo que queda tras aplicarlas es el riesgo residual.

### 2.3. Análisis de riesgos

El **análisis de riesgos** es el proceso sistemático para estimar la magnitud de los riesgos a que está expuesta una organización, **activos, amenazas, impactos y salvaguardas**. 
**MAGERIT** (metodología creada para las Administraciones Públicas), fases:

1. **Caracterización de los activos:**
    - Identificación (información, servicios, software, hardware, soportes, redes, instalaciones, personas) y dependencias entre activos.
    - Valoración en **D-I-C-A-T** para cada activo (0 Despreciable - 5 Muy Alto).
    - Identificación de los activos: esenciales (información y servicios) y de soporte (software, hardware, soportes de información, equipamiento auxiliar, redes de comunicación, instalaciones, personas).

2. **Caracterización de las amenazas:**
    - Naturales.
    - Industriales.
    - Errores/fallos.
    - Ataques deliberados.
    - Valorar **frecuencia + degradación**.

3. **Determinación del impacto:** Daño producido por la amenaza.

4. **Determinación del riesgo potencial:** Riesgo **sin salvaguardas**.

5. **Caracterización de las salvaguardas:**
    - Identificación y eficacia.

6. **Estimación del estado de riesgo:** 
    - **Impacto y riesgo residual** tras las salvaguardas.

### 2.4. Gestión de riesgos

Objetivo → reducir el riesgo hasta un nivel **aceptable**.

**Estrategias (MATE):**
- **Aceptar** → asumir el riesgo. Coste supera daño
- **Mitigar/Reducir** → aplicar salvaguardas.
- **Transferir** → delegar el riesgo a un tercer
- **Evitar** → eliminar la actividad o activo.

**Principio coste-beneficio:** El coste de las salvaguardas nunca debe superar el coste potencial de la materialización de las amenazas que mitigan.

**Controles:**
- **Preventivos** → evitan (hardening, formación, control de acceso).
- **Detectivos** → detectan (IDS/IPS, SIEM, auditoría de logs).
- **Correctivos** → corrigen/recuperan (planes de contingencia, restauración de backups, parcheo).

### 2.5. Análisis de riesgos en el ENS

**Art. 7 ENS** → gestión de riesgos **permanente y actualizada**.
**[op.pl.1] → formalización según categoría del sistema:**

- **BÁSICA** → análisis **informal**.
- **MEDIA** → análisis **semiformal**.
- **ALTA** → análisis **formal/cuanti­tativo**.

## 3. Herramientas

### 3.1. Marcos de referencia

- **ISO 27000** → conceptos y vocabulario.
- **ISO 27001** → Requisitos gestión **SGSI**, certificable.
- **ISO 27005** → gestión de riesgos de seguridad. Fases
- **ISO 31000** → gestión general de riesgos. No limitado a TI
- **COBIT** → gobierno y gestión de TI.
  - **EDM03** → optimización del riesgo.
  - **APO12** → gestión del riesgo.
- **COSO ERM** → gestión del riesgo empresarial y control interno. Orientado a la alta dirección.

### 3.2. MAGERIT v3 (Metodología de Análisis y Gestión de Riesgos de los Sistemas de Información)

- Metodología de referencia para las **AAPP españolas**. promovida por el **CSAE** (Comisión Sectorial de Administración Electrónica). Su versión actual es MAGERIT v3.
- Analiza y gestiona riesgos de sistemas de información.
- Base para:
  - Evaluación.
  - Certificación.
  - Auditoría.
  - Acreditación. Legitimar al sistema para integrarse en sistemas más amplios.

**Informes resultado del análisis de riesgos con MAGERIT:**
- **Modelo de valor** (Activos, dependencias, valor)
- **Mapa de riesgos** (Amenazas, frencuencia)
- **Declaración de aplicabilidad** (Contramedidas)
- **Evaluación de salvaguardas**.
- **Informe de insuficiencias**.
- **Estado de riesgo**. Potencial y residual por activo y amenaza

**PILAR** → herramienta del **CCN** que automatiza MAGERIT.

### 3.3. Técnicas para la gestión de riesgos

MAGERIT v3 propone las siguientes técnicas:

**Técnicas específicas del análisis de riesgos:**
- **Tablas de impacto y riesgo.**
- **Análisis algorítmico.** Modelos de valoración del riesgo.
- **Árboles de ataque.**

**Técnicas generales:**
- **Técnicas gráficas:** 
- **Sesiones de trabajo:** Participación de los usuarios.
- **Valoración Delphi:** Cuestionarios para identificar problemas y desarrollar estrategias.

### 3.4. Herramientas EAR (Entorno de Análisis de Riesgos)

Análisis y la gestión de riesgos siguiendo la metodología MAGERIT (**CCN**):

- **PILAR** → completa. Automatiza MAGERIT y genera la SoA (Statement of Applicability, Declaración de aplicabilidad)
- **PILAR Basic** → simplificada. Orientadas a Administración Local.
- **μPILAR** → análisis rápido.
- **RMAT** → Risk Management Additional Tools. personalización/extensión.

Permiten importar el **PCE-EELL** (Perfil para Entidades Locales) y tienen módulos para hacer la **Evaluación de Impacto en Protección de Datos (EIPD)** exigida por el RGPD.

## 4. Conclusión

El análisis y gestión de riesgos es el núcleo de la seguridad y un principio básico del ENS (art. 7). Consiste en identificar y valorar activos, amenazas, vulnerabilidades e impactos y decidir cómo tratar los riesgos: evitarlos, mitigarlos, transferirlos o aceptarlos, aplicando controles preventivos, detectivos o correctivos.

MAGERIT v3 es la metodología de referencia en las AAPP españolas y PILAR es la herramienta del CCN que facilita y automatiza su aplicación. Se complementa con normas como ISO 27001/27005 e ISO 31000, y marcos como COBIT y COSO.

Lo más importante: la gestión de riesgos no se hace una sola vez, sino que es continua y debe mantenerse actualizada. Es fundamental para cumplir el ENS, implantar un SGSI (ISO 27001) y proteger los datos conforme al RGPD.

Para memorizar:
👉 ENS exige → MAGERIT analiza → PILAR ayuda → gestión continua → seguridad y cumplimiento.