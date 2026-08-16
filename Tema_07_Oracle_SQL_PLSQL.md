# Tema 7.- El SGBDR Oracle. Lenguajes SQL y PL/SQL.

## 1. Introducción

Oracle Database, como se analizó en el tema anterior dedicado a su arquitectura, es un potente motor de gestión de la persistencia. Sin embargo, toda esa maquinaria interna resultaría inaccesible sin un lenguaje de interfaz que permita a los administradores (DBA), desarrolladores y aplicaciones operar sobre los datos. Ese lenguaje es **SQL (Structured Query Language)**, el estándar universal de comunicación con bases de datos relacionales, y su extensión procedural propietaria de Oracle: **PL/SQL (Procedural Language for SQL)**.

Mientras que SQL es un lenguaje declarativo —el usuario describe *qué* datos quiere obtener, no *cómo* obtenerlos—, PL/SQL añade las capacidades imperativas necesarias para construir lógica de programación completa dentro de la propia base de datos: variables, bucles, condicionales, gestión de excepciones y modularización del código.

Oracle ha adoptado plenamente el estándar ANSI/ISO SQL en sus sucesivas versiones (SQL-92, SQL:1999, SQL:2003, SQL:2016) y lo ha enriquecido con extensiones propietarias que explotan al máximo las capacidades de su motor relacional.

## 2. Lenguaje SQL en Oracle

Oracle estructura el lenguaje SQL en subconjuntos funcionales claramente diferenciados, cada uno orientado a un tipo de operación específico sobre la base de datos.

### 2.1. DDL: Lenguaje de Definición de Datos (Data Definition Language)

El DDL comprende las sentencias destinadas a crear, modificar y eliminar la estructura (esquema) de la base de datos. Son comandos de alto impacto estructural que afectan a los metadatos almacenados en el Diccionario de Datos de Oracle.

**Característica fundamental:** Toda sentencia DDL ejecuta un **COMMIT automático implícito**. Cualquier transacción DML pendiente en la sesión se confirma irreversiblemente antes de ejecutar el DDL. Los cambios DDL no son reversibles mediante ROLLBACK.

Las sentencias DDL principales son:

*   **`CREATE`:** Crea nuevos objetos en el esquema de la base de datos.
    *   `CREATE TABLE`: Define una nueva tabla con sus columnas, tipos de datos y restricciones.
    *   `CREATE INDEX`: Crea estructuras de indexación (B-Tree, Bitmap) para acelerar las consultas.
    *   `CREATE VIEW`: Define una vista, que es una consulta almacenada que se comporta como una tabla virtual.
    *   `CREATE SEQUENCE`: Crea un generador de valores numéricos secuenciales, utilizado típicamente para generar claves primarias autoincrementales.
    *   `CREATE SYNONYM`: Define un alias para un objeto de la base de datos.

*   **`ALTER`:** Modifica la estructura de un objeto existente sin destruirlo ni afectar a sus datos.
    *   Ejemplo: `ALTER TABLE Ciudadano ADD (email VARCHAR2(100));` — añade una nueva columna a la tabla.
    *   Otros usos: modificar el tipo de dato de una columna, añadir o eliminar restricciones, habilitar o deshabilitar índices.

*   **`DROP`:** Elimina un objeto del esquema junto con todos sus datos asociados. Libera el espacio de almacenamiento ocupado en el Tablespace correspondiente.
    *   Ejemplo: `DROP TABLE Temporal_Importacion;`
    *   La cláusula `CASCADE CONSTRAINTS` elimina también las restricciones de integridad referencial que dependan del objeto eliminado.

*   **`TRUNCATE`:** Elimina todas las filas de una tabla de forma instantánea, liberando el espacio de almacenamiento, pero conservando la estructura de la tabla (columnas, restricciones, índices). A diferencia de `DELETE`, no genera registros individuales en el Redo Log para cada fila eliminada, lo que la convierte en una operación extremadamente rápida. Es una operación DDL (con COMMIT implícito) y, por tanto, **no reversible** mediante ROLLBACK.

### 2.2. DML: Lenguaje de Manipulación de Datos (Data Manipulation Language)

El DML es el conjunto de sentencias utilizado por los desarrolladores y las aplicaciones para interactuar con los datos almacenados en las tablas. A diferencia del DDL, las operaciones DML son **transaccionales**: no se confirman automáticamente y pueden deshacerse mediante ROLLBACK hasta que se ejecute un COMMIT explícito.

*   **`SELECT`:** Recupera datos de una o más tablas. Es la sentencia más utilizada de SQL y ofrece una amplia gama de cláusulas:
    *   `WHERE`: Filtra las filas según condiciones lógicas.
    *   `GROUP BY`: Agrupa filas con valores comunes para aplicar funciones de agregación (COUNT, SUM, AVG, MAX, MIN).
    *   `HAVING`: Filtra los grupos generados por GROUP BY según condiciones sobre las funciones de agregación.
    *   `ORDER BY`: Ordena el resultado de forma ascendente (ASC) o descendente (DESC).
    *   `DISTINCT`: Elimina las filas duplicadas del resultado.
    *   Funciones analíticas (Window Functions): `ROW_NUMBER()`, `RANK()`, `LAG()`, `LEAD()`, `PARTITION BY`, que permiten cálculos avanzados sobre conjuntos de filas.

*   **`INSERT`:** Añade nuevas filas a una tabla, respetando las restricciones de integridad definidas (NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, CHECK).
    *   Inserción simple: `INSERT INTO Empleado (id, nombre) VALUES (1, 'García');`
    *   Inserción masiva desde otra tabla: `INSERT INTO Historico SELECT * FROM Empleado WHERE activo = 'N';`

*   **`UPDATE`:** Modifica los valores de una o más columnas en filas existentes. Es fundamental incluir siempre una cláusula `WHERE` para limitar el alcance de la actualización; de lo contrario, se modificarán **todas** las filas de la tabla.
    *   Ejemplo: `UPDATE Empleado SET salario = salario * 1.03 WHERE departamento_id = 10;`

*   **`DELETE`:** Elimina filas de una tabla, respetando las restricciones de integridad referencial (no permite eliminar filas referenciadas por claves foráneas de otras tablas, salvo que se defina `ON DELETE CASCADE` o `ON DELETE SET NULL`). Genera registros en el Redo Log para cada fila eliminada y es reversible mediante ROLLBACK.

*   **`MERGE`:** Sentencia avanzada que combina operaciones de INSERT y UPDATE en una sola instrucción, según exista o no una correspondencia entre la tabla origen y la tabla destino. Es especialmente útil en procesos ETL y cargas de datos incrementales.

### 2.3. DCL: Lenguaje de Control de Datos (Data Control Language)

Gestiona los permisos y la seguridad del acceso a los datos:

*   **`GRANT`:** Concede privilegios a usuarios o roles. Puede otorgar privilegios de sistema (CREATE TABLE, CREATE SESSION) o privilegios sobre objetos específicos (SELECT, INSERT, UPDATE sobre una tabla concreta).
    *   Ejemplo: `GRANT SELECT, INSERT ON Empleado TO rol_rrhh;`

*   **`REVOKE`:** Revoca privilegios previamente concedidos.
    *   Ejemplo: `REVOKE INSERT ON Empleado FROM rol_rrhh;`

### 2.4. TCL: Lenguaje de Control de Transacciones (Transaction Control Language)

Gobierna el ciclo de vida de las transacciones, implementando las propiedades ACID:

*   **`COMMIT`:** Confirma permanentemente todos los cambios realizados en la transacción actual. Una vez ejecutado, los cambios son durables y visibles para el resto de sesiones. Activa el proceso LGWR para escribir el Redo Log Buffer en disco.

*   **`ROLLBACK`:** Deshace todos los cambios realizados desde el último COMMIT (o desde el inicio de la sesión). Restaura los datos a su estado previo utilizando los datos de UNDO almacenados en el Tablespace UNDO.

*   **`SAVEPOINT`:** Establece un punto intermedio dentro de una transacción al que se puede retroceder parcialmente con `ROLLBACK TO SAVEPOINT nombre;`, sin deshacer toda la transacción.

### 2.5. Operaciones relacionales avanzadas: JOINs y subconsultas

La potencia de SQL se manifiesta plenamente al combinar datos de múltiples tablas, respetando las relaciones establecidas mediante claves foráneas.

**JOINs (conforme al estándar ANSI SQL-92):**

*   **`INNER JOIN`:** Devuelve solo las filas donde existe correspondencia en ambas tablas. Es el tipo de JOIN más habitual.
    *   Ejemplo: `SELECT e.nombre, d.nombre_dept FROM Empleado e INNER JOIN Departamento d ON e.dept_id = d.id;`

*   **`LEFT OUTER JOIN`:** Devuelve todas las filas de la tabla izquierda, y las filas correspondientes de la tabla derecha. Si no existe correspondencia, las columnas de la tabla derecha aparecen como NULL.

*   **`RIGHT OUTER JOIN`:** Simétrico al LEFT OUTER JOIN: conserva todas las filas de la tabla derecha.

*   **`FULL OUTER JOIN`:** Combina LEFT y RIGHT: conserva todas las filas de ambas tablas, rellenando con NULL donde no exista correspondencia.

*   **`CROSS JOIN`:** Producto cartesiano de ambas tablas. Cada fila de la primera tabla se combina con todas las filas de la segunda. Rara vez se utiliza intencionadamente debido al volumen exponencial de resultados.

**Subconsultas (Subqueries):**

Son consultas SELECT anidadas dentro de otra consulta SQL. Pueden ubicarse en:
*   La cláusula `WHERE`: como filtro dinámico (`WHERE salario > (SELECT AVG(salario) FROM Empleado)`).
*   La cláusula `FROM`: como tabla derivada (inline view).
*   La cláusula `SELECT`: como valor calculado por cada fila.

Las subconsultas pueden ser correlacionadas (dependen de la fila de la consulta externa) o no correlacionadas (se ejecutan una sola vez de forma independiente).

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
