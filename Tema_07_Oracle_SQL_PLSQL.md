# Tema 7.- El SGBDR Oracle. Lenguajes SQL y PL/SQL.

## 1. Introducción

**SQL (Structured Query Language)**, estándar universal de comunicación con BD relacionales, Oracle -> **PL/SQL (Procedural Language for SQL)**.

SQL lenguaje declarativo, PL/SQL añade lógica de programación (variables, bucles, condicionales, gestión de excepciones y modularización del código)

## 2. Lenguaje SQL en Oracle

Operaciones sobre BD.

### 2.1. DDL: Lenguaje de Definición de Datos (Data Definition Language)

DDL crear, modificar y eliminar la estructura (esquema) de BD -> ejecuta un **COMMIT automático implícito**. Reversibles mediante ROLLBACK.

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

DML interactuar con los datos. **Transaccionales**: no se confirman automáticamente.

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

El estándar SQL es extraordinariamente eficaz como lenguaje declarativo de consulta y manipulación de datos. Sin embargo, presenta limitaciones inherentes: no soporta variables, no dispone de estructuras de control de flujo (bucles, condicionales), no gestiona excepciones y no permite modularizar el código en subprogramas reutilizables.

Para cubrir estas necesidades, Oracle desarrolló **PL/SQL (Procedural Language for SQL)**, un lenguaje de programación procedural que extiende SQL incorporando todas las capacidades de un lenguaje de programación imperativo. PL/SQL se compila y ejecuta directamente dentro del motor de la base de datos, lo que elimina la latencia de red entre la aplicación y el servidor y reduce drásticamente el tráfico de datos.

### 3.2. Estructura de un bloque PL/SQL

Todo código PL/SQL se organiza en **bloques** con una estructura fija de tres secciones:

```sql
DECLARE
    -- Sección declarativa (opcional)
    -- Definición de variables, constantes, cursores y tipos
    v_nombre VARCHAR2(100);
    v_total  NUMBER := 0;
BEGIN
    -- Sección ejecutable (obligatoria)
    -- Lógica del programa: sentencias SQL, control de flujo
    SELECT nombre INTO v_nombre FROM Empleado WHERE id = 1;
    v_total := v_total + 1;
EXCEPTION
    -- Sección de excepciones (opcional)
    -- Gestión de errores y situaciones excepcionales
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Empleado no encontrado');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
```

**Las tres secciones:**

1.  **DECLARE (opcional):** Se definen las variables locales (`v_total NUMBER`), constantes (`c_iva CONSTANT NUMBER := 0.21`), cursores explícitos y tipos de datos compuestos (registros, tablas PL/SQL). Se utiliza `%TYPE` para vincular el tipo de una variable al tipo de una columna (`v_nombre Empleado.nombre%TYPE`), y `%ROWTYPE` para declarar una variable que almacene una fila completa de una tabla.

2.  **BEGIN (obligatoria):** Contiene la lógica ejecutable del programa. Incluye sentencias SQL (SELECT INTO, INSERT, UPDATE, DELETE), estructuras de control de flujo (`IF-THEN-ELSIF-ELSE`, `CASE`, `FOR LOOP`, `WHILE LOOP`, `LOOP-EXIT WHEN`), llamadas a subprogramas y asignaciones de variables.

3.  **EXCEPTION (opcional):** Gestiona los errores que pueden producirse durante la ejecución del bloque BEGIN. Oracle proporciona excepciones predefinidas como `NO_DATA_FOUND` (la consulta no devolvió filas), `TOO_MANY_ROWS` (la consulta devolvió más de una fila), `DUP_VAL_ON_INDEX` (violación de restricción UNIQUE) y `ZERO_DIVIDE`. También permite definir excepciones personalizadas y capturar cualquier error con el manejador genérico `WHEN OTHERS`.

### 3.3. Subprogramas almacenados: Procedimientos y Funciones

PL/SQL permite crear bloques de código con nombre que se compilan y almacenan permanentemente en la base de datos, listos para ser invocados repetidamente:

*   **Procedimiento (Procedure):** Subprograma que ejecuta una acción pero no devuelve un valor directamente (aunque puede devolver valores mediante parámetros de salida OUT o IN OUT).
    ```sql
    CREATE OR REPLACE PROCEDURE actualizar_salario (
        p_empleado_id IN NUMBER,
        p_incremento  IN NUMBER
    ) IS
    BEGIN
        UPDATE Empleado SET salario = salario + p_incremento
        WHERE id = p_empleado_id;
        COMMIT;
    END;
    /
    ```

*   **Función (Function):** Subprograma que siempre devuelve un valor mediante la cláusula `RETURN`. Puede utilizarse dentro de sentencias SQL.
    ```sql
    CREATE OR REPLACE FUNCTION obtener_salario (
        p_empleado_id IN NUMBER
    ) RETURN NUMBER IS
        v_salario NUMBER;
    BEGIN
        SELECT salario INTO v_salario FROM Empleado WHERE id = p_empleado_id;
        RETURN v_salario;
    END;
    /
    ```

### 3.4. Paquetes (Packages)

Los **Paquetes** son la unidad de modularización más potente de PL/SQL. Agrupan lógicamente procedimientos, funciones, variables, constantes, cursores y tipos relacionados en una única estructura con dos componentes:

*   **Especificación del paquete (Package Specification):** Define la interfaz pública: declara los subprogramas, tipos y variables accesibles desde fuera del paquete. Actúa como un contrato o API.
*   **Cuerpo del paquete (Package Body):** Contiene la implementación de los subprogramas declarados en la especificación, así como subprogramas y variables privados que solo son accesibles internamente.

**Ventajas de los paquetes:**
- **Encapsulamiento:** Ocultan la implementación interna, exponiendo solo la interfaz pública.
- **Rendimiento:** Al cargar un paquete en memoria, todos sus componentes quedan disponibles, reduciendo las llamadas a disco.
- **Organización:** Facilitan la gestión de cientos de subprogramas agrupándolos por funcionalidad.
- **Sobrecarga:** Permiten definir múltiples subprogramas con el mismo nombre pero diferentes parámetros.

### 3.5. Cursores

PL/SQL procesa los resultados de las consultas SQL registro a registro mediante **cursores**:

*   **Cursor implícito:** Oracle lo crea automáticamente para cada sentencia SQL ejecutada en un bloque PL/SQL. Se accede a sus atributos mediante `SQL%FOUND`, `SQL%NOTFOUND`, `SQL%ROWCOUNT`.

*   **Cursor explícito:** Declarado y gestionado manualmente por el programador cuando necesita recorrer un conjunto de filas resultado de un SELECT:
    ```sql
    DECLARE
        CURSOR c_empleados IS SELECT id, nombre FROM Empleado WHERE dept_id = 10;
        v_emp c_empleados%ROWTYPE;
    BEGIN
        OPEN c_empleados;
        LOOP
            FETCH c_empleados INTO v_emp;
            EXIT WHEN c_empleados%NOTFOUND;
            DBMS_OUTPUT.PUT_LINE(v_emp.nombre);
        END LOOP;
        CLOSE c_empleados;
    END;
    /
    ```

*   **Cursor FOR LOOP:** Forma simplificada que automatiza la apertura, el fetch y el cierre del cursor:
    ```sql
    FOR r IN (SELECT id, nombre FROM Empleado WHERE dept_id = 10) LOOP
        DBMS_OUTPUT.PUT_LINE(r.nombre);
    END LOOP;
    ```

### 3.6. Triggers (Disparadores)

Los **Triggers** son bloques PL/SQL que se ejecutan automáticamente en respuesta a un evento específico de la base de datos, sin intervención explícita del usuario:

*   **Triggers DML:** Se disparan ante operaciones INSERT, UPDATE o DELETE sobre una tabla. Pueden configurarse como BEFORE (antes de la operación) o AFTER (después), y como ROW-LEVEL (se ejecutan por cada fila afectada) o STATEMENT-LEVEL (se ejecutan una vez por sentencia).
    ```sql
    CREATE OR REPLACE TRIGGER trg_auditoria_empleado
    AFTER UPDATE ON Empleado
    FOR EACH ROW
    BEGIN
        INSERT INTO Log_Auditoria (tabla, operacion, fecha, usuario)
        VALUES ('EMPLEADO', 'UPDATE', SYSDATE, USER);
    END;
    /
    ```

*   **Triggers DDL:** Se disparan ante eventos de definición de datos (CREATE, ALTER, DROP).
*   **Triggers de base de datos:** Se disparan ante eventos del sistema (STARTUP, SHUTDOWN, LOGON, LOGOFF).

**Usos habituales:** Auditoría de cambios, validación de reglas de negocio complejas, generación automática de valores derivados, replicación de datos y control de acceso contextual.

## 4. Conclusión

SQL y PL/SQL conforman el ecosistema lingüístico completo de Oracle Database. SQL, respaldado por los estándares ANSI/ISO, proporciona la capacidad declarativa necesaria para definir, manipular, controlar y consultar datos mediante los sublenguajes DDL, DML, DCL y TCL. Su modelo de JOINs, subconsultas y funciones analíticas permite abordar las consultas más complejas del ámbito de la gestión pública.

PL/SQL complementa a SQL añadiendo las capacidades procedurales imprescindibles para la construcción de lógica de negocio dentro de la base de datos: variables, control de flujo, gestión de excepciones, subprogramas almacenados (procedimientos y funciones), paquetes, cursores y triggers. La ejecución del código PL/SQL directamente en el motor de la base de datos minimiza la latencia de red y maximiza el rendimiento en entornos transaccionales de alta concurrencia, característicos de los sistemas de información de las Administraciones Públicas.

El dominio combinado de SQL y PL/SQL constituye una competencia técnica esencial para el desarrollo, la administración y la optimización de los sistemas de bases de datos Oracle que sustentan los servicios digitales del sector público.
