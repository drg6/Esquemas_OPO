# Tema 7.- El SGBDR Oracle. Lenguajes SQL y PL/SQL.

## 1. Introducción

**SQL (Structured Query Language)**, estándar universal de comunicación con BD relacionales, Oracle -> **PL/SQL (Procedural Language for SQL)**.

SQL lenguaje declarativo, PL/SQL añade lógica de programación (variables, bucles, condicionales, gestión de excepciones y modularización del código)

## 2. Lenguaje SQL en Oracle

Operaciones sobre BD.

### 2.1. DDL: Lenguaje de Definición de Datos (Data Definition Language)

DDL crear, modificar y eliminar la **estructura** (esquema) de BD. **Auto-COMMIT (Irreversible)**.

*   **`CREATE`:** Crea objetos.
    *   `CREATE TABLE`
    *   `CREATE INDEX`
    *   `CREATE VIEW`
    *   `CREATE SEQUENCE`
    *   `CREATE SYNONYM`: Alias

*   **`ALTER`:** Modifica objetos (añade/elimina columnas, restricciones, indices, modifica tipos de datos, etc)
*   **`DROP`:** Elimina objeto. `CASCADE CONSTRAINTS` elimina también las restricciones de integridad referencial dependientes.
*   **`TRUNCATE`:** Elimina todas las filas de una tabla conservando la estructura. No genera Redo Log, operación extremadamente rápida. 

### 2.2. DML: Lenguaje de Manipulación de Datos (Data Manipulation Language)

DML interactuar con los datos. **Transaccional** (Requiere COMMIT explícito. Reversible con ROLLBACK)

*   **`SELECT`:** Recupera datos:
    *   `WHERE`: Filtra.
    *   `GROUP BY`: Agrupa, funciones de agregación (COUNT, SUM, AVG, MAX, MIN).
    *   `HAVING`: Filtra grupos generados por GROUP BY.
    *   `ORDER BY`: Ordena (ASC, DESC).
    *   `DISTINCT`: Elimina duplicados.
    *   Funciones analíticas (Window Functions): `ROW_NUMBER()`, `RANK()`, `LAG()`, `LEAD()`, `PARTITION BY`.

*   **`INSERT`:** Añade filas. Inserción simple, Inserción masiva desde otra tabla (SELECT).
*   **`UPDATE`:** Modifica valores.
*   **`DELETE`:** Elimina filas.
*   **`MERGE`:** Combina INSERT y UPDATE.

### 2.3. DCL: Lenguaje de Control de Datos (Data Control Language)

Gestiona permisos:

*   **`GRANT xxx ON xxx TO xxx`:** Concede privilegios a usuarios o roles. 
*   **`REVOKE xxx ON xxx TO xxx`:** Revoca privilegios previamente concedidos.

### 2.4. TCL: Lenguaje de Control de Transacciones (Transaction Control Language)

Gobierna el ciclo de vida de las transacciones, implementando las propiedades ACID:

*   **`COMMIT`:** Confirma permanentemente todos los cambios transacción actual. 
*   **`ROLLBACK`:** Deshace todos los cambios realizados desde el último COMMIT.
*   **`SAVEPOINT`:** Establece un punto intermedio dentro de una transacción. Retroceder parcialmente -> `ROLLBACK TO SAVEPOINT nombre;`.

### 2.5. Operaciones relacionales avanzadas: JOINs y subconsultas

**JOINs (conforme al estándar ANSI SQL-92):**
*   **`INNER JOIN`:** solo las filas con correspondencia en ambas tablas.
*   **`LEFT OUTER JOIN`:** todas las filas de la izquierda + las coincidentes de la derecha. Si no hay coincidencia → NULL.
*   **`RIGHT OUTER JOIN`:** todas las filas de la derecha + las coincidentes de la izquierda. Si no hay coincidencia → NULL.
*   **`FULL OUTER JOIN`:** todas las filas de ambas tablas, coincidan o no.
*   **`CROSS JOIN`:** Producto cartesiano de ambas tablas. 

**Subconsultas (Subqueries):**
Son consultas SELECT anidadas dentro de otra consulta SQL (`WHERE`, `FROM`,  `SELECT`).

## 3. PL/SQL: El Lenguaje Procedural de Oracle

### 3.1. Naturaleza y propósito

SQL -> no soporta variables, estructuras de control de flujo (bucles, condicionales), excepciones y no modulariza código.
**PL/SQL (Procedural Language for SQL)**, extiende SQL, se compila y ejecuta dentro del motor de BD (más rápido).

### 3.2. Estructura de un bloque PL/SQL

1.  **DECLARE (opcional):** Definición de variables, constantes, cursores y tipos de datos compuestos. `%TYPE` tipo de datos del campo BD y `%ROWTYPE` variable con tipos de datos de registro tabla.
2.  **BEGIN (obligatoria):** Lógica ejecutable.
3.  **EXCEPTION (opcional):** Gestión de errores (`NO_DATA_FOUND`, `TOO_MANY_ROWS`, `DUP_VAL_ON_INDEX` y `ZERO_DIVIDE`, `WHEN OTHERS`).

### 3.3. Subprogramas almacenados: Procedimientos y Funciones

*   **Procedimiento (Procedure):** Ejecuta acción, no devuelve un valor directamente (solo con parámetros de salida OUT o IN OUT). **CREATE OR REPLACE PROCEDURE () IS BEGIN END;**
*   **Función (Function):** Siempre devuelve valor `RETURN`. **CREATE OR REPLACE FUNCTION () RETURN xx IS BEGIN END;**

### 3.4. Paquetes (Packages)

Agrupan lógicamente procedimientos, funciones, variables, constantes, cursores y tipos relacionados:

*   **Especificación del paquete (Package Specification):** Declaracion.
*   **Cuerpo del paquete (Package Body):** Implementación.

**Ventajas de los paquetes:** Encapsulamiento, Rendimiento, Organización, Sobrecarga.

### 3.5. Cursores

Consultas SQL registro a registro.

*   **Cursor implícito:** Oracle lo crea automáticamente. `SQL%FOUND`, `SQL%NOTFOUND`, `SQL%ROWCOUNT`.
*   **Cursor explícito:** Declarado y gestionado manualmente. **DECLARE (Cursor + Variable) -> OPEN -> FETCH (Bucle + Exit) -> CLOSE**
*   **Cursor FOR LOOP:** Gestión simplificada. **FOR x IN (SELECT) LOOP .. END LOOP** .Apertura, lectura y cierre automáticos.

### 3.6. Triggers (Disparadores)

Los **Triggers** bloques PL/SQL que se ejecutan automáticamente en respuesta a un evento (Auditoría de cambios, reglas de negocio).

*   **Triggers DML:** Operaciones INSERT, UPDATE o DELETE. BEFORE (antes de la operación) o AFTER (después), ROW-LEVEL (por cada fila afectada) o STATEMENT-LEVEL (una vez por sentencia).
    ```sql
    CREATE OR REPLACE TRIGGER trg_auditoria_empleado
    AFTER UPDATE ON Empleado
    FOR EACH ROW
    BEGIN
        INSERT INTO xx
    END;
    /
    ```

*   **Triggers DDL:** Se disparan ante eventos de definición de datos (CREATE, ALTER, DROP).
*   **Triggers de base de datos:** Se disparan ante eventos del sistema (STARTUP, SHUTDOWN, LOGON, LOGOFF).

## 4. Conclusión

SQL (ANSI/ISO) definir, manipular, controlar y consultar datos (DDL, DML, DCL y TCL).
PL/SQL añaden variables, control de flujo, gestión de excepciones, procedimientos y funciones, paquetes, cursores y triggers. 
Ventaja: Minimiza latencia de red, maximiza rendimiento en concurrencia (crucial en sistemas AP).

SQL y PL/SQL esencial para desarrollo, la administración y la optimización de SGBD Oracle.
