# Tema 5.- El modelo relacional. Normas y estándares para la interoperabilidad entre gestores de bases de datos relacionales.

## 1. Introducción
* **Origen y vigencia:** Formulado por E. F. Codd (1970) bajo teoría matemática de conjuntos y lógica de predicados de primer orden.
* **El reto en AAPP:** Heterogeneidad histórica de motores (Oracle, PostgreSQL, SQL Server).
* **Imperativo legal (Detalle de Oro):** La interoperabilidad y la neutralidad tecnológica son mandatos del **Esquema Nacional de Interoperabilidad (ENI - RD 4/2010)** y el **ENS (RD 311/2022)** para evitar el bloqueo de proveedor (*vendor lock-in*).

## 2. Fundamentos Lógicos y Estructura Formal
* **Correspondencias formales:**
  * Relación = Tabla.
  * Tupla = Fila / Registro.
  * Atributo = Columna / Campo.
  * Dominio = Conjunto atómico de valores válidos.
* **Métricas:** **Grado** (n.º de columnas, estático) vs. **Cardinalidad** (n.º de filas, dinámico).
* **Sistema de Claves:**
  * *Superclave:* Conjunto de atributos que identifican unívocamente una tupla.
  * *Clave Candidata:* Superclave mínima (sin redundancias).
  * *Clave Primaria (PK):* Candidata elegida para indexar e identificar la relación.
  * *Clave Foránea (FK):* Atributo que referencia la PK de otra relación.

## 3. Manipulación: Álgebra Relacional (Sistema Cerrado)
Tanto las entradas como las salidas de cualquier operación son siempre relaciones.
* **Operadores Primitivos:**
  * $\sigma$ (*Selección*): Filtra tuplas en horizontal (filas).
  * $\pi$ (*Proyección*): Extrae atributos en vertical (columnas, eliminando duplicados).
  * $\cup$ (*Unión*), $-$ (*Diferencia*) y $\times$ (*Producto Cartesiano*).
* **Operadores Derivados:** $\cap$ (*Intersección*) y $\bowtie$ (*Join Natural* / Combinación).

## 4. Reglas de Integridad y Reglas de Codd
* **Reglas de Integridad Básicas:**
  * *Entidad:* Ningún componente de la PK puede ser `NULL`.
  * *Referencial:* Toda FK debe coincidir con una PK existente o ser `NULL`.
  * *Dominio:* Tipo de dato, longitud y restricciones `CHECK`.
  * *Usuario:* Triggers y procedimientos para reglas de negocio específicas.
* **Las 12 Reglas de Codd (1985):** Espíritu formal para evaluar SGBD relacionales puros. Exigen representación única en tablas, acceso exclusivo mediante sublenguaje relacional, tratamiento sistemático de `NULL` e independencia física, lógica y de distribución.

## 5. Normalización (Eliminación de Anomalías)
Descomposición sistemática para erradicar redundancias y evitar anomalías de inserción, borrado y actualización:
* **1FN:** Atributos atómicos; ausencia de grupos repetitivos.
* **2FN:** 1FN + dependencia funcional completa de la PK (sin dependencias parciales).
* **3FN:** 2FN + eliminación de dependencias transitivas entre campos no clave.
* **FNBC (Boyce-Codd):** Versión estricta de 3FN donde todo determinante debe ser clave candidata.

## 6. El Estándar SQL (ANSI / ISO)
Lenguaje declarativo universal estructurado en cuatro sublenguajes:
* **DDL** (`CREATE`, `ALTER`, `DROP`) | **DML** (`SELECT`, `INSERT`, `UPDATE`, `DELETE`).
* **DCL** (`GRANT`, `REVOKE`) | **TCL** (`COMMIT`, `ROLLBACK`, `SAVEPOINT`).
* **Dialectos propietarios:** Extensiones procedimentales de fabricantes: **PL/SQL** (Oracle), **T-SQL** (SQL Server), **PL/pgSQL** (PostgreSQL).

## 7. Estándares y Mecanismos de Interoperabilidad
* **ODBC (Open Database Connectivity - Microsoft/SQL Access Group):**
  * API en C/C++ desacoplada mediante un **ODBC Driver Manager** que traduce las llamadas al driver específico de cada motor.
* **JDBC (Java Database Connectivity):**
  * API en `java.sql` / `javax.sql` con interfaces estándar (`Connection`, `PreparedStatement`, `ResultSet`).
  * **Driver Tipo 4 (100% Java puro):** Estándar de facto moderno; convierte llamadas JDBC directamente al protocolo de red nativo del SGBD (sin puentes intermedios ni librerías C cliente).
* **Otras capas de abstracción:**
  * *ADO.NET:* Interfaz desacoplada en el ecosistema Microsoft (.NET).
  * *ORM (Object-Relational Mapping):* Hibernate, JPA, Entity Framework (mapean objetos a tablas, aislando el dialecto SQL).
  * *SQL/MED (Management of External Data):* Integración de fuentes remotas mediante **Foreign Data Wrappers (FDW)** (estándar nativo en PostgreSQL).

## 8. Conclusión
El modelo relacional permanece como el pilar transaccional de las Administraciones Públicas por su rigor formal y garantías de consistencia. La adopción de estándares abiertos como SQL ANSI/ISO, drivers universales (ODBC/JDBC) y capas ORM permite desacoplar las aplicaciones de los motores subyacentes, materializando el principio de neutralidad tecnológica exigido por el ENI y garantizando que los datos públicos no queden cautivos de ningún proveedor.

-------------

# Tema 5.- El modelo relacional. Normas y estándares para la interoperabilidad entre gestores de bases de datos relacionales.

## 1. Introducción

Estándar SGBD **Modelo Relacional** en AP y privadas, **Edgar Frank Codd** (1970). Se fundamenta en teoría matemática de conjuntos y la lógica de predicados de primer orden.

SGBDR de distintos fabricantes (Oracle, PostgreSQL, SQL Server, MySQL) desafío **interoperabilidad**. 

## 2. Fundamentos Matemáticos del Modelo Relacional

### 2.1. Estructura lógica y terminología formal

Se fundamenta en teoría matemática de conjuntos y la lógica de predicados de primer orden. 
Concepto central de **relación** (**tabla**), compuesta por:

*   **Tuplas (filas/registros):** Instancia concreta de la relación.
*   **Atributos (columnas/campos):** Propiedad de la relación.
*   **Dominio:** Valores válidos del atributo.
*   **Grado:** Número total de atributos de la relación. 
*   **Cardinalidad:** Número total de tuplas (filas) de la relación. Varía con operaciones de inserción y eliminación.

### 2.2. El sistema de claves relacionales

*   **Superclave:** Cualquier conjunto de atributos identificar de forma unívoca una tupla de la relación. Puede contener atributos redundantes. 
*   **Clave Candidata:** Superclave mínima. 
*   **Clave Primaria (Primary Key - PK):** Clave candidata seleccionada por DBA. No admite valores nulos (NULL). Cada relación una clave primaria.
*   **Clave Alternativa:** Cualquier clave candidata que no es clave primaria. Necesita restricción UNIQUE.
*   **Clave Foránea o Ajena (Foreign Key - FK):** Atributo relación que referencia la clave primaria de otra relación.

## 3. Manipulación: El Álgebra Relacional

Operaciones definidas sobre relaciones, operandos y resultados son siempre relaciones. 

2 categorías de operadores:

*   **Operadores primitivos:**
    *   **Selección:** Relación formada por el subconjunto de tuplas que satisface dicha expresión
    *   **Proyección:** Relación definida sobre los atributos donde se aplica, eliminando las filas repetidas.
    *   **Unión:** Combina las tuplas que pertenecen a una relación, a otra, o a ambas.
    *   **Diferencia:** Tuplas pertenecen a la primera relación pero no a la segunda.
    *   **Producto Cartesiano:** Concatena cada tupla de la primera relación con todas las tuplas de la segunda.
*   **Operadores derivados:**
    *   **Intersección:** Tuplas presentes simultáneamente en ambas relaciones.
    *   **Combinación o Join Natural:** Relación formada por todos los pares de tuplas que, en el producto cartesiano de ambas, cumplen una condición especificada.

## 4. Restricciones de Integridad del Modelo Relacional

1.  **Integridad de entidad:** Ningún componente de clave primaria puede contener valores nulos (NULL), clave primaria identifica inequívocamente cada tupla.
2.  **Integridad referencial:** Toda clave ajena contiene valor que exista como clave primaria en la relación referenciada o es nulo. Coherencia entre tablas.
3.  **Integridad de Dominio:** Valores de atributo respeten las restricciones de tipo de dato, rango, formato o restricciones CHECK.
4.  **Integridad definida por el usuario:** Reglas de negocio.

## 5. Las Doce Reglas de Codd

Fijan los requisitos para que un SGBD sea estrictamente relacional (independencia, acceso por valor, catálogo dinámico)

## 6. Normalización de Bases de Datos

Objetivo: Proceso formal para minimizar redundancia y evitar anomalías de modificación.
Formas Normales principales:

*   **1FN**: Atomicidad.

*   **2FN**: Cumple 1FN + Dependencia Completa,.

*   **3FN**: Cumple 2FN + Dependencia Transitiva.

*   **BCNF (Boyce-Codd)**: Todo determinante es una clave candidata.

## 7. El Lenguaje SQL como estándar 

*   **DDL (Definición):** Crea y modifica objetos (`CREATE`, `DROP`, `ALTER`).
*   **DML (Manipulación):** Interactúa con los registros (`SELECT`, `INSERT`, `UPDATE`, `DELETE`).
*   **DCL (Control):** Gestiona permisos de usuarios (`GRANT`, `REVOKE`).
*   **TCL (Transacciones):** Mantiene la integridad aislando operaciones (`BEGIN`, `COMMIT`, `ROLLBACK`).

## 8. Interoperabilidad entre SGBDR: Normas y Estándares

**Interoperabilidad** —> capacidad que aplicaciones accedan de forma transparente a BD heterogéneas.

### 8.1. SQL como estándar universal (ANSI/ISO)

**SQL (Structured Query Language)**, IBM, estandar ANSI en 1986 y adoptado por la ISO. 

(PL/SQL en Oracle, T-SQL en SQL Server, PL/pgSQL en PostgreSQL), pero todos respetan núcleo del estándar ANSI/ISO SQL definición, manipulación y control de datos.

### 8.2. ODBC (Open Database Connectivity)

**ODBC**, Microsoft en 1992, API estandarizada acceder a cualquier SGBDR, independientemente del fabricante.

*   **Arquitectura:** Basado en **ODBC Driver Manager**. Redirige sentencias SQL al **driver específico** del SGBDR de destino 
*   **Ventaja principal:** Cambio de SGBDR, sustituir el driver, sin modificar el código fuente de la aplicación.
*   **Limitaciones:** Windows, aunque existe unixODBC.

### 8.3. JDBC (Java Database Connectivity)

Conectividad ecosistema Java, Sun Microsystems (actualmente Oracle).

*   **Arquitectura:** API Java alojada en `java.sql`. Interfaces para acceso a datos de forma independiente del SGBDR.
*   **Tipos de drivers JDBC:** Estándar Tipo 4 (nativo puro Java), alto rendimiento y portabilidad. Sustituye tipos (1, 2 y 3), que dependian de software del fabricante, puentes externos o intermediarios de red.

### 8.4. Otros estándares y tecnologías de interoperabilidad

*   **OLE DB y ADO.NET:** APIs ecosistema Microsoft. ADO.NET, aplicaciones .NET.
*   **ORM (Object-Relational Mapping):** Frameworks como Hibernate (Java), Entity Framework (.NET) o SQLAlchemy (Python) permiten trabajar con objetos del lenguaje de programación en vez de escribir SQL, facilita portabilidad entre SGBDR.
*   **SQL/MED (Management of External Data):** Extensión de SQL que permite acceder a datos externos (otros SGBDR, ficheros, servicios web) como si fueran tablas locales, mediante  **Foreign Data Wrappers (FDW)**. PostgreSQL usa funcionalidad de forma nativa.

## 9. Conclusión

El modelo relacional base que se construyen los SGBD al garantizarse la coherencia, integridad y ausencia de redundancia en BDs.

Interoperabilidad entre SGBDR, resuelta mediante SQL (ANSI/ISO) y APIs de conectividad universales (ODBC, JDBC, ADO.NET). Esencial cumplir principios de neutralidad tecnológica e interoperabilidad del ENI.
