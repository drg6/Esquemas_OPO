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
