# Tema 4.- Sistemas de gestión de bases de datos relacionales. El modelo de referencia ANSI.

## 1. Introducción

Gestión eficiente de la información -> pilar fundamental para organización. Contexto en la Administración: Gestión crítica de padrones y tributos
Integridad, disponibilidad y confidencialidad de los datos -> cumplir exigencias normativas (ENS, RGPD, LOPDGDD).

El problema original **Sistemas de ficheros planos (Flat Files)**:

1.  **Redundancia e inconsistencia:** 
2.  **Dependencia datos-programas:** 
3.  **Ausencia de control de concurrencia y seguridad:** 

La solución: Aparición de los SGBD para centralizar y abstraer el almacenamiento.

## 2. Bases de Datos y Sistemas Gestores de Bases de Datos (SGBD)

Definiciones: BD (conjunto estructurado que modela una realidad) vs SGBD (software intermediario).

### 2.1. Funciones fundamentales de un SGBD

Objetivo garantizar gestión integral de datos.

1.  **Función de definición (DDL - Data Definition Language):** Crear, modificar y eliminar tablas, vistas, índices, esquemas y restricciones de integridad. 

2.  **Función de manipulación (DML - Data Manipulation Language):** Interactuar con los datos almacenados: inserción (`INSERT`), consulta (`SELECT`), actualización (`UPDATE`) y eliminación (`DELETE`) de registros.

3.  **Función de control (DCL - Data Control Language):** Seguridad y permisos de acceso a los datos: `GRANT` (conceder privilegios) y `REVOKE` (revocar privilegios), permite definir políticas de acceso granulares por usuario, rol o grupo.

4.  **Gestión de la integridad:** Garantiza restricciones de integridad: clave primaria (PRIMARY KEY), clave ajena (FOREIGN KEY), unicidad (UNIQUE), no nulidad (NOT NULL) y restricciones de comprobación (CHECK).

5.  **Gestión de la concurrencia:** Acceso simultáneo de usuariosa BD sin interferencias ni corrupción de datos -> bloqueo y control de versiones (MVCC - Multi-Version Concurrency Control).

6.  **Recuperación ante fallos:** Respaldo y restauración (backup/recovery), registro de transacciones (Write-Ahead Logging) y Rollback (recuperación del sistema).

## 3. El Modelo Relacional de Bases de Datos

### 3.1. Origen y fundamentos

Artículo *"A Relational Model of Data for Large Shared Data Banks"* (1970), **Edgar Frank Codd** -> bases teóricas **Modelo Relacional**. Ruptura modelos previos —jerárquico (IMS de IBM) y en red (CODASYL).

Se fundamenta en teoría matemática de conjuntos y la lógica de predicados de primer orden. 
Concepto central de **relación** (**tabla**), compuesta por:

*   **Tuplas (filas/registros):** Instancia concreta de la relación.
*   **Atributos (columnas/campos):** Propiedad de la relación.
*   **Dominio:** Valores válidos del atributo.
*   **Clave primaria (Primary Key):** Atributo o conjunto de atributos que identifica de forma unívoca cada tupla de la relación.
*   **Clave ajena (Foreign Key):** Atributo relación que referencia la clave primaria de otra relación.

### 3.2. Reglas de integridad del modelo relacional

*   **Integridad de entidad:** Ningún componente de clave primaria puede contener valores nulos (NULL), clave primaria identifica inequívocamente cada tupla.
*   **Integridad referencial:** Toda clave ajena contiene valor que exista como clave primaria en la relación referenciada o es nulo. Coherencia entre tablas.

### 3.3. Las 12 reglas de Codd

Fijan los requisitos para que un SGBD sea estrictamente relacional (independencia, acceso por valor, catálogo dinámico)

### 3.4. Propiedades ACID de las transacciones

SGBD garantizan transacciones cumplan propiedades **ACID**:

*   **Atomicidad (Atomicity):** Transacción se ejecuta completamente o no se ejecuta.

*   **Consistencia (Consistency):** Una transacción lleva BD de un estado válido a otro, garantizando cumplir reglas de integridad.

*   **Aislamiento (Isolation):** Las transacciones concurrentes ejecucción aislada, secuencialmente. 4 niveles de aislamiento: Read Uncommitted, Read Committed, Repeatable Read y Serializable.

*   **Durabilidad (Durability):** Confirmada transacción `COMMIT`, efectos persisten de forma permanente en BD ante caidas. Garantiza Write-Ahead Logging (WAL), registra en log antes de en BD.

### 3.5 Normalización de Bases de Datos

Objetivo: Proceso formal para minimizar redundancia y evitar anomalías de modificación.
Formas Normales principales:

*   **1FN**: Atributos atómicos, sin grupos repetitivos.

*   **2FN**: Cumple 1FN + atributos no clave dependen de la clave primaria completa.

*   **3FN**: Cumple 2FN + sin dependencias transitivas.

*   **BCNF (Boyce-Codd)**: Todo determinante es una clave candidata.

## 4. El Modelo de Referencia ANSI/SPARC

### 4.1. Contexto y motivación

Implementaciones propietarias sin poder interoperar entre si, migración muy costosa.

**ANSI (American National Standards Institute)** (1975) crea comité **SPARC (Standards Planning and Requirements Committee)**, propuso **arquitectura de referencia de tres niveles** aplicable a cualquier SGBD. 
Objetivo lograr **independencia de los datos**.

### 4.2. Los tres niveles de la arquitectura ANSI/SPARC

#### 4.2.1. Nivel Interno o Físico (Esquema Interno)

**Cómo** y **Dónde** se almacenan los datos.

*   **Contenido:** Estructuras de almacenamiento físico: formato de los registros en disco, organización de ficheros (secuencial, indexada, hash), algoritmos de indexación (árboles B, B+, índices bitmap), técnicas de compresión, asignación de espacio en disco (tablespaces, datafiles), estrategias de clustering y particionamiento.
*   **Responsable:** Administrador del sistema o el DBA (Database Administrator).

#### 4.2.2. Nivel Lógico o Conceptual (Esquema Conceptual)

Visión global y unificada de toda la BD.

*   **Contenido:** Define **qué** datos se almacenan y **qué relaciones** existen entre ellos, independencia representación física. Tablas, atributos, tipos de datos, claves primarias, claves ajenas, restricciones de integridad y reglas de negocio.
*   **Responsable:** Administrador de la base de datos (DBA) y los analistas de datos, modelado mediante Entidad-Relación (E/R).

#### 4.2.3. Nivel Externo o de Vistas (Esquema Externo)

Vistas personalizadas y parciales BD adaptada a usuarios.

*   **Contenido:** Múltiples **vistas (views)** con información relevante para un perfil de usuario concreto, ocultando complejidad del esquema global.
*   **Responsable:** Definido por DBA junto con responsables funcionales de cada área.

## 5. La Independencia de Datos: objetivo fundamental de la arquitectura ANSI/SPARC

Objetivo: Modificar un nivel sin afectar a los adyacentes.

### 5.1. Independencia física de datos

Entre el **nivel interno** y el **nivel conceptual**. Cambios en hardware/índices (Nivel Interno) no afectan al esquema lógico.

### 5.2. Independencia lógica de datos

Entre el **nivel conceptual** y el **nivel externo**. Cambios en tablas (Nivel Conceptual) no rompen las vistas de los usuarios (Nivel Externo).

### 5.3. Correspondencias entre niveles (Mappings)

Implementación: Mediante correspondencias (mappings) automáticas que traducen las peticiones entre niveles.

## 6. Conclusión

Los SGBD Relacionales -> tecnología de persistencia dominante. Se fundamenta en modelo relacional de Codd, las propiedades ACID garantizan la fiabilidad transaccional.

ANSI/SPARC, tres niveles independientes —interno, conceptual y externo permite reducir costes de mantenimiento y facilita adaptación a nuevos requisitos.

Administraciones Públicas -> continuidad del servicio, la integridad de la información y el cumplimiento normativo son exigencias ineludibles. Dominio de estas tecnologías y arquitecturas resulta esencial.
