# Tema 5.- El modelo relacional. Normas y estándares para la interoperabilidad entre gestores de bases de datos relacionales.

## 1. Introducción

El modelo relacional constituye el paradigma teórico dominante en la gestión de bases de datos desde su formulación en 1970 por Edgar F. Codd. Su solidez matemática, fundamentada en la teoría de conjuntos y la lógica de predicados de primer orden, lo ha convertido en el estándar sobre el que se construyen los Sistemas Gestores de Bases de Datos Relacionales (SGBDR) que sustentan los sistemas de información de Administraciones Públicas y organizaciones privadas en todo el mundo.

Sin embargo, la coexistencia de múltiples SGBDR de distintos fabricantes (Oracle, PostgreSQL, SQL Server, MySQL) en una misma organización plantea un desafío crítico: la **interoperabilidad**. Las Administraciones Públicas, que frecuentemente heredan sistemas heterogéneos de distintas épocas y proveedores, necesitan que sus aplicaciones accedan de forma transparente a bases de datos de diferentes fabricantes sin reescribir el código para cada plataforma.

En este tema se profundiza en los fundamentos del modelo relacional, sus mecanismos de integridad y normalización, y se analizan los estándares y tecnologías que hacen posible la interoperabilidad entre SGBDR heterogéneos.

## 2. Fundamentos Matemáticos del Modelo Relacional

### 2.1. Estructura lógica y terminología formal

El modelo relacional no es una convención arbitraria, sino que se sustenta sobre la **Teoría Matemática de Conjuntos** y la **lógica de predicados de primer orden**. Aunque el lenguaje comercial SQL ha popularizado una terminología simplificada, la nomenclatura formal establece equivalencias precisas:

1.  **Relación (equivale comercialmente a Tabla):** Es la estructura fundamental del modelo. Formalmente, es un subconjunto del producto cartesiano de una lista finita de dominios. Visualmente se representa como una tabla bidimensional compuesta por tuplas (filas) y atributos (columnas).

2.  **Tupla (equivale a Fila o Registro):** Cada elemento de la relación. Representa una instancia concreta de la entidad modelada (por ejemplo, los datos completos de un contribuyente). El modelo relacional establece que las tuplas no tienen orden intrínseco; el orden de inserción es irrelevante desde el punto de vista formal.

3.  **Atributo (equivale a Columna o Campo):** Cada propiedad que describe a los elementos de la relación (DNI, nombre, fecha de nacimiento, dirección). Al igual que las tuplas, los atributos carecen de un orden predeterminado en el modelo formal.

4.  **Dominio:** Conjunto de valores válidos que un atributo puede tomar. Es un concepto atómico: cada valor del dominio debe ser indivisible. Por ejemplo, el dominio del atributo "edad" sería el conjunto de los números enteros positivos entre 0 y 130. Un valor que no pertenezca al dominio definido será rechazado por el SGBDR.

5.  **Grado:** Número total de atributos (columnas) que componen una relación. Es una propiedad estructural fija del esquema.

6.  **Cardinalidad:** Número total de tuplas (filas) que contiene una relación en un momento determinado. A diferencia del grado, la cardinalidad varía dinámicamente con las operaciones de inserción y eliminación.

### 2.2. El sistema de claves relacionales

En una base de datos con millones de registros, el SGBDR necesita mecanismos inequívocos para identificar cada tupla de forma única. Este requisito se satisface mediante el sistema de claves:

*   **Superclave:** Cualquier conjunto de uno o más atributos que permite identificar de forma unívoca una tupla dentro de una relación. Una superclave puede contener atributos redundantes. Ejemplo: la combinación {DNI, Nombre, Fecha_Nacimiento} identifica unívocamente a un ciudadano, aunque {DNI} sería suficiente.

*   **Clave Candidata:** Es una superclave mínima, es decir, una superclave de la que no se puede eliminar ningún atributo sin perder la capacidad de identificación unívoca. Ejemplo: tanto {DNI} como {Número_SS} pueden ser claves candidatas de la relación CIUDADANO.

*   **Clave Primaria (Primary Key - PK):** Es la clave candidata seleccionada por el diseñador de la base de datos como identificador principal de la relación. No admite valores nulos (NULL) en ninguno de sus atributos componentes. Cada relación debe tener exactamente una clave primaria.

*   **Clave Alternativa:** Cualquier clave candidata que no fue seleccionada como clave primaria. Mantiene la propiedad de unicidad y puede implementarse mediante restricciones UNIQUE.

*   **Clave Foránea o Ajena (Foreign Key - FK):** Es un atributo o conjunto de atributos de una relación cuyos valores deben corresponderse obligatoriamente con los valores de la clave primaria de otra relación (o de la misma). Constituye el mecanismo fundamental del modelo relacional para establecer vínculos lógicos entre tablas. Ejemplo: el atributo `ID_Departamento` en la tabla EMPLEADO referencia la clave primaria de la tabla DEPARTAMENTO, estableciendo la relación "cada empleado pertenece a un departamento".

## 3. Restricciones de Integridad del Modelo Relacional

Un sistema relacional debe garantizar la coherencia de los datos en todo momento. Para ello, el SGBDR actúa como guardián, imponiendo un conjunto de reglas de integridad que no pueden ser vulneradas:

1.  **Integridad de Entidad:** Ningún componente de la clave primaria de una relación puede contener el valor NULL. Si una tupla careciera de identificador primario, sería una entidad indistinguible e inutilizable.

2.  **Integridad Referencial:** Si una relación contiene una clave foránea que referencia a otra relación, cada valor de dicha clave foránea debe coincidir con un valor existente en la clave primaria de la relación referenciada, o bien ser NULL (si la definición del atributo lo permite). El SGBDR nunca permitirá asignar un empleado a un departamento con código '999' si dicho código no existe en la tabla DEPARTAMENTO.

3.  **Integridad de Dominio:** Garantiza que los valores almacenados en cada atributo respeten las restricciones de tipo de dato (INTEGER, VARCHAR, DATE), rango, formato y cualquier otra regla de validación definida en el esquema (restricciones CHECK).

4.  **Integridad definida por el usuario:** Reglas de negocio específicas de la organización que se implementan mediante restricciones CHECK, triggers o procedimientos almacenados. Ejemplo: "el saldo de una cuenta no puede ser negativo" o "la fecha de finalización debe ser posterior a la fecha de inicio".

## 4. Las Doce Reglas de Codd

En 1985, ante la proliferación de productos comerciales que se autoproclamaban "relacionales" sin serlo genuinamente, Edgar F. Codd formuló sus célebres **12 reglas** (numeradas de la 0 a la 12) como criterio de evaluación:

*   **Regla 0 (Regla fundamental):** Un SGBDR debe ser capaz de gestionar bases de datos enteramente mediante sus capacidades relacionales, sin recurrir a mecanismos no relacionales.
*   **Regla 1 (Regla de la información):** Toda la información de la base de datos debe estar representada de una única manera: como valores en posiciones de columnas dentro de filas de tablas.
*   **Regla 2 (Acceso garantizado):** Todo dato atómico debe ser accesible de forma determinista mediante la combinación del nombre de la tabla, el valor de la clave primaria y el nombre del atributo.
*   **Regla 3 (Tratamiento sistemático de valores nulos):** El SGBDR debe soportar la representación de información faltante o inaplicable mediante valores NULL, distintos de cadenas vacías o ceros.
*   **Regla 4 (Catálogo dinámico basado en el modelo relacional):** Los metadatos (el diccionario de datos o catálogo del sistema) deben almacenarse y ser consultables utilizando el mismo modelo relacional y el mismo lenguaje (SQL) que los datos de usuario.
*   **Regla 5 (Sublenguaje completo de datos):** El sistema debe soportar al menos un lenguaje relacional que permita la definición de datos, la manipulación de datos, las restricciones de integridad, la autorización y la gestión de transacciones.
*   **Regla 6 (Actualización de vistas):** Toda vista que sea teóricamente actualizable debe poder ser actualizada por el sistema.
*   **Regla 7 (Inserción, actualización y eliminación de alto nivel):** Las operaciones de modificación deben aplicarse a conjuntos de tuplas, no solo registro a registro.
*   **Regla 8 (Independencia física):** Los cambios en el almacenamiento físico no deben requerir cambios en las aplicaciones.
*   **Regla 9 (Independencia lógica):** Los cambios en el esquema lógico que preserven la información no deben requerir cambios en las aplicaciones.
*   **Regla 10 (Independencia de integridad):** Las restricciones de integridad deben poder definirse y modificarse sin afectar a los programas de aplicación.
*   **Regla 11 (Independencia de distribución):** La distribución de los datos en múltiples ubicaciones debe ser transparente para las aplicaciones.
*   **Regla 12 (Regla de no subversión):** Si el sistema dispone de una interfaz de bajo nivel (registro a registro), esta no debe poder utilizarse para eludir las reglas de integridad expresadas en el lenguaje relacional de alto nivel.

## 5. Normalización de Bases de Datos

Un diseño de base de datos deficiente provoca las denominadas **anomalías de actualización**: anomalías de inserción (no poder añadir datos sin información adicional innecesaria), anomalías de eliminación (perder información no relacionada al borrar registros) y anomalías de modificación (tener que actualizar un mismo dato en múltiples lugares). La **normalización** es el proceso formal que permite corregir estas deficiencias.

La normalización descompone progresivamente las relaciones en estructuras más pequeñas y cohesionadas, vinculadas mediante claves foráneas, eliminando la redundancia y garantizando la consistencia. Las formas normales son acumulativas: cada nivel incorpora las exigencias de los anteriores.

### 5.1. Primera Forma Normal (1FN)

Una relación está en 1FN si y solo si todos sus atributos contienen valores atómicos (indivisibles). No se permiten atributos multivaluados ni grupos repetitivos. Ejemplo: almacenar varios números de teléfono separados por comas en una sola celda viola la 1FN.

### 5.2. Segunda Forma Normal (2FN)

Una relación está en 2FN si está en 1FN y, además, no existen **dependencias funcionales parciales**: todo atributo no clave depende funcionalmente de la totalidad de la clave primaria, no de un subconjunto de ella. Esta forma normal es relevante cuando la clave primaria es compuesta.

### 5.3. Tercera Forma Normal (3FN)

Una relación está en 3FN si está en 2FN y, además, no existen **dependencias funcionales transitivas**: ningún atributo no clave depende de otro atributo no clave. Ejemplo: si en la tabla EMPLEADO el atributo "Ciudad" depende de "Código_Postal", y "Código_Postal" depende de la PK "ID_Empleado", existe una dependencia transitiva. La solución es extraer {Código_Postal, Ciudad} a una tabla independiente.

### 5.4. Forma Normal de Boyce-Codd (FNBC)

Es una versión reforzada de la 3FN. Una relación está en FNBC si y solo si todo determinante es una superclave. Mientras que la 3FN permite que existan determinantes que no sean superclaves cuando el atributo determinado forma parte de alguna clave candidata, la FNBC elimina esta excepción, siendo por tanto más restrictiva.

### 5.5. Formas normales superiores

Existen formas normales adicionales (4FN, 5FN y Forma Normal de Dominio-Clave) que abordan dependencias multivaluadas y dependencias de reunión. En la práctica, la mayoría de los diseños de bases de datos operacionales se normalizan hasta la 3FN o la FNBC, ya que las formas superiores se aplican a situaciones muy específicas.

## 6. Interoperabilidad entre SGBDR: Normas y Estándares

En las organizaciones de cierta envergadura, y especialmente en las Administraciones Públicas, es habitual que coexistan múltiples SGBDR de distintos fabricantes. Un departamento puede utilizar Microsoft SQL Server, mientras que otro opera sobre Oracle y un tercero emplea PostgreSQL. La **interoperabilidad** —la capacidad de que las aplicaciones accedan de forma transparente a bases de datos heterogéneas— es un requisito operativo crítico.

### 6.1. SQL como estándar universal (ANSI/ISO)

El lenguaje **SQL (Structured Query Language)**, originalmente desarrollado por IBM, fue estandarizado por el ANSI en 1986 y posteriormente adoptado por la ISO. Las principales revisiones del estándar incluyen:

| Versión | Año | Aportaciones principales |
|---------|------|--------------------------|
| SQL-86 | 1986 | Primera estandarización formal |
| SQL-89 | 1989 | Integridad referencial |
| SQL-92 | 1992 | Joins explícitos (INNER, OUTER), dominios, esquemas |
| SQL:1999 | 1999 | Triggers, procedimientos almacenados, tipos definidos por el usuario, expresiones regulares |
| SQL:2003 | 2003 | XML, secuencias, columnas de identidad |
| SQL:2008 | 2008 | TRUNCATE, FETCH FIRST |
| SQL:2011 | 2011 | Datos temporales |
| SQL:2016 | 2016 | JSON, row pattern matching |
| SQL:2023 | 2023 | Grafos (SQL/PGQ), tipos de datos JSON mejorados |

Aunque cada fabricante implementa extensiones propietarias (PL/SQL en Oracle, T-SQL en SQL Server, PL/pgSQL en PostgreSQL), todos respetan el núcleo del estándar ANSI/ISO SQL para las operaciones básicas de definición, manipulación y control de datos.

### 6.2. ODBC (Open Database Connectivity)

Desarrollado por Microsoft en 1992, **ODBC** es una API estandarizada que proporciona una interfaz uniforme para que las aplicaciones accedan a cualquier SGBDR, independientemente del fabricante.

*   **Arquitectura:** Se basa en un componente intermediario denominado **ODBC Driver Manager**. La aplicación envía las sentencias SQL al Driver Manager, que las redirige al **driver específico** del SGBDR de destino (driver de Oracle, de PostgreSQL, de SQL Server, etc.). El driver traduce las llamadas al protocolo nativo del SGBDR correspondiente.
*   **Ventaja principal:** La aplicación se codifica una sola vez contra la API ODBC. Para cambiar de SGBDR, basta con sustituir el driver, sin modificar el código fuente de la aplicación.
*   **Limitaciones:** Originalmente vinculado al ecosistema Windows y al lenguaje C/C++, aunque existen implementaciones multiplataforma como unixODBC.

### 6.3. JDBC (Java Database Connectivity)

Dado que ODBC estaba ligado a C/C++ y al entorno Windows, Sun Microsystems (actualmente Oracle Corporation) desarrolló **JDBC** como el estándar de conectividad a bases de datos para el ecosistema Java.

*   **Arquitectura:** JDBC es una API Java alojada en el paquete `java.sql` (y `javax.sql` para funcionalidades avanzadas). Proporciona interfaces como `DriverManager`, `Connection`, `Statement`, `PreparedStatement` y `ResultSet` que abstraen el acceso a datos de forma independiente del SGBDR subyacente.
*   **Tipos de drivers JDBC:**
    *   **Tipo 1 (Puente JDBC-ODBC):** Traduce llamadas JDBC a llamadas ODBC. Obsoleto desde Java 8.
    *   **Tipo 2 (API nativa):** Utiliza bibliotecas nativas del SGBDR. Requiere instalación de software del fabricante en el cliente.
    *   **Tipo 3 (Protocolo de red):** Comunica con un middleware que traduce al protocolo nativo. Independiente de la plataforma.
    *   **Tipo 4 (Protocolo nativo puro Java):** Comunica directamente con el SGBDR mediante su protocolo nativo, implementado íntegramente en Java. Es el tipo más utilizado actualmente por su rendimiento y portabilidad.

### 6.4. Otros estándares y tecnologías de interoperabilidad

*   **OLE DB y ADO.NET:** APIs de acceso a datos en el ecosistema Microsoft. ADO.NET, utilizado en aplicaciones .NET (C#, VB.NET), proporciona proveedores de datos específicos para cada SGBDR con una interfaz unificada.
*   **ORM (Object-Relational Mapping):** Frameworks como Hibernate (Java), Entity Framework (.NET) o SQLAlchemy (Python) proporcionan una capa de abstracción de alto nivel que permite a los desarrolladores trabajar con objetos del lenguaje de programación en lugar de escribir SQL directamente, facilitando la portabilidad entre SGBDR.
*   **SQL/MED (Management of External Data):** Extensión del estándar SQL que permite acceder a datos externos (otros SGBDR, ficheros, servicios web) como si fueran tablas locales, mediante el concepto de **Foreign Data Wrappers (FDW)**. PostgreSQL implementa esta funcionalidad de forma nativa.

## 7. Conclusión

El modelo relacional de Codd sigue siendo, más de cinco décadas después de su formulación, el fundamento teórico sobre el que se construyen los sistemas de gestión de datos más críticos del mundo. Su rigor matemático —basado en la teoría de conjuntos, las dependencias funcionales y la normalización— garantiza la coherencia, integridad y ausencia de redundancia en las bases de datos.

La interoperabilidad entre SGBDR heterogéneos, resuelta mediante la estandarización del lenguaje SQL (ANSI/ISO) y las APIs de conectividad universales (ODBC, JDBC, ADO.NET), permite a las organizaciones —y especialmente a las Administraciones Públicas— integrar sistemas de distintos fabricantes sin dependencia tecnológica de un único proveedor. Esta capacidad resulta esencial para cumplir los principios de neutralidad tecnológica e interoperabilidad contemplados en el Esquema Nacional de Interoperabilidad (ENI) regulado por el Real Decreto 4/2010 y actualizado por el Real Decreto 311/2022.

El dominio de estos fundamentos teóricos y estándares de interoperabilidad constituye una competencia imprescindible para el profesional de las tecnologías de la información al servicio de la Administración Pública.
