# Tema 4.- Sistemas de gestión de bases de datos relacionales. El modelo de referencia ANSI.

## 1. Introducción

La gestión eficiente de la información constituye uno de los pilares fundamentales de cualquier organización moderna y, de manera singular, de las Administraciones Públicas. Municipios, diputaciones y organismos públicos manejan volúmenes crecientes de datos —padronales, tributarios, urbanísticos, de recursos humanos— cuya integridad, disponibilidad y confidencialidad resultan críticas para la prestación de servicios al ciudadano y el cumplimiento del marco normativo vigente (Esquema Nacional de Seguridad, RGPD, LOPDGDD).

En las décadas iniciales de la informática, el almacenamiento de datos se basaba en **sistemas de ficheros planos (Flat Files)**, donde cada aplicación gestionaba sus propios archivos de datos de forma independiente. Este modelo presentaba graves limitaciones estructurales:

1.  **Redundancia e inconsistencia:** Un mismo dato (por ejemplo, la dirección de un contribuyente) se almacenaba de forma duplicada en múltiples ficheros independientes (Recaudación, Padrón, Urbanismo). Cualquier actualización debía replicarse manualmente en todos ellos, con el consiguiente riesgo de inconsistencias.
2.  **Dependencia datos-programas:** La estructura física de los datos estaba estrechamente acoplada al código de las aplicaciones. Cualquier modificación en el formato de almacenamiento obligaba a recompilar y adaptar todos los programas que accedían a esos ficheros.
3.  **Ausencia de control de concurrencia y seguridad:** No existían mecanismos nativos para gestionar el acceso simultáneo de múltiples usuarios ni para establecer políticas de permisos granulares sobre los datos.

Para superar estas limitaciones, surgieron los **Sistemas de Gestión de Bases de Datos (SGBD)**, que centralizan la administración de los datos y proporcionan un nivel de abstracción entre las aplicaciones y el almacenamiento físico. A lo largo de este tema se analizará el modelo relacional como paradigma dominante en los SGBD actuales, así como la arquitectura de referencia ANSI/SPARC que establece los principios de independencia de datos.

## 2. Bases de Datos y Sistemas Gestores de Bases de Datos (SGBD)

Es fundamental distinguir con precisión dos conceptos que, aunque estrechamente relacionados, definen realidades diferentes:

*   **Base de Datos (BD):** Es el conjunto organizado y estructurado de datos interrelacionados, almacenados de forma persistente en un soporte físico, que representa una porción del mundo real relevante para la organización. Por ejemplo, la base de datos del Padrón Municipal contiene los registros de todos los habitantes empadronados, con sus datos personales, direcciones y fechas de inscripción.

*   **Sistema Gestor de Bases de Datos (SGBD / DBMS - Database Management System):** Es el software especializado que actúa como intermediario entre los usuarios o aplicaciones y la base de datos. Gestiona todas las operaciones de acceso, manipulación, seguridad y mantenimiento de los datos. Ejemplos representativos incluyen Oracle Database, PostgreSQL, Microsoft SQL Server, MySQL y MariaDB.

### 2.1. Funciones fundamentales de un SGBD

Todo SGBD debe proporcionar un conjunto de funciones esenciales para garantizar la gestión integral de los datos:

1.  **Función de definición (DDL - Data Definition Language):** Permite especificar la estructura lógica de la base de datos: crear, modificar y eliminar tablas, vistas, índices, esquemas y restricciones de integridad. Incluye sentencias como `CREATE TABLE`, `ALTER TABLE` y `DROP TABLE`.

2.  **Función de manipulación (DML - Data Manipulation Language):** Proporciona las operaciones necesarias para interactuar con los datos almacenados: inserción (`INSERT`), consulta (`SELECT`), actualización (`UPDATE`) y eliminación (`DELETE`) de registros.

3.  **Función de control (DCL - Data Control Language):** Gestiona la seguridad y los permisos de acceso a los datos mediante sentencias como `GRANT` (conceder privilegios) y `REVOKE` (revocar privilegios), permitiendo definir políticas de acceso granulares por usuario, rol o grupo.

4.  **Gestión de la integridad:** Garantiza que los datos cumplan en todo momento las reglas de negocio definidas mediante restricciones de integridad: clave primaria (PRIMARY KEY), clave ajena (FOREIGN KEY), unicidad (UNIQUE), no nulidad (NOT NULL) y restricciones de comprobación (CHECK).

5.  **Gestión de la concurrencia:** Permite que múltiples usuarios accedan simultáneamente a la base de datos sin que se produzcan interferencias ni corrupción de datos, mediante mecanismos de bloqueo y control de versiones (MVCC - Multi-Version Concurrency Control).

6.  **Recuperación ante fallos:** Incorpora mecanismos de respaldo y restauración (backup/recovery), registro de transacciones (Write-Ahead Logging) y operaciones de Rollback que garantizan la recuperación del sistema a un estado consistente tras cualquier fallo de hardware, software o energía eléctrica.

## 3. El Modelo Relacional de Bases de Datos

### 3.1. Origen y fundamentos

En 1970, el matemático e investigador de IBM **Edgar Frank Codd** publicó su artículo seminal *"A Relational Model of Data for Large Shared Data Banks"*, que sentó las bases teóricas del **Modelo Relacional**. Este modelo supuso una ruptura radical con los modelos previos —jerárquico (IMS de IBM) y en red (CODASYL)— que obligaban a los programadores a navegar manualmente por estructuras de punteros y árboles para acceder a los datos.

El modelo relacional se fundamenta en la teoría matemática de conjuntos y la lógica de predicados de primer orden. Su concepto central es la **relación** (que en la práctica se implementa como una **tabla**), compuesta por:

*   **Tuplas (filas/registros):** Cada fila representa una instancia concreta de la entidad modelada (por ejemplo, un contribuyente específico).
*   **Atributos (columnas/campos):** Cada columna representa una propiedad de la entidad (nombre, NIF, dirección, teléfono).
*   **Dominio:** El conjunto de valores válidos que un atributo puede tomar (por ejemplo, el dominio del atributo "edad" serían los enteros positivos).
*   **Clave primaria (Primary Key):** Atributo o conjunto mínimo de atributos que identifica de forma unívoca cada tupla de la relación.
*   **Clave ajena (Foreign Key):** Atributo de una relación que referencia la clave primaria de otra relación, estableciendo vínculos lógicos entre tablas.

### 3.2. Reglas de integridad del modelo relacional

Codd definió dos reglas fundamentales de integridad:

*   **Integridad de entidad:** Ningún componente de la clave primaria de una relación puede contener valores nulos (NULL), ya que la clave primaria debe identificar inequívocamente cada tupla.
*   **Integridad referencial:** Toda clave ajena debe contener un valor que exista como clave primaria en la relación referenciada, o bien ser nula (si el atributo lo permite). Esta regla garantiza la coherencia de las referencias entre tablas.

### 3.3. Las 12 reglas de Codd

Para evaluar si un sistema puede considerarse verdaderamente relacional, Codd formuló en 1985 sus célebres **12 reglas** (numeradas de la 0 a la 12). Entre las más relevantes destacan:

*   **Regla 0 (Regla fundamental):** Un SGBD relacional debe gestionar sus datos enteramente mediante sus capacidades relacionales.
*   **Regla 1 (Regla de la información):** Toda la información debe representarse como valores en tablas (relaciones).
*   **Regla 2 (Regla del acceso garantizado):** Todo dato debe ser accesible mediante la combinación del nombre de la tabla, el nombre de la columna y el valor de la clave primaria.
*   **Regla 5 (Sublenguaje completo):** El sistema debe soportar al menos un lenguaje relacional que permita definir datos, manipularlos, establecer restricciones de integridad y gestionar transacciones.
*   **Regla 12 (Regla de la no subversión):** Si el sistema dispone de una interfaz de bajo nivel para operar registro a registro, esta no debe poder utilizarse para eludir las reglas de integridad o las restricciones expresadas en el lenguaje relacional de alto nivel.

### 3.4. Propiedades ACID de las transacciones

Los SGBD relacionales modernos garantizan que las transacciones cumplan las propiedades **ACID**, acrónimo que representa cuatro garantías fundamentales:

*   **Atomicidad (Atomicity):** Una transacción se ejecuta completamente o no se ejecuta en absoluto. Si una transferencia bancaria implica un cargo en una cuenta y un abono en otra, ambas operaciones se consolidan juntas o se deshacen juntas. No existen estados intermedios.

*   **Consistencia (Consistency):** Una transacción transforma la base de datos de un estado válido a otro estado válido, respetando todas las reglas de integridad definidas. Por ejemplo, no puede quedar registrado un saldo negativo si existe una restricción que lo prohíbe.

*   **Aislamiento (Isolation):** Las transacciones concurrentes se ejecutan de forma aislada entre sí, como si se procesaran secuencialmente. Una transacción en curso no puede ver los datos intermedios de otra transacción no confirmada. El estándar SQL define cuatro niveles de aislamiento: Read Uncommitted, Read Committed, Repeatable Read y Serializable.

*   **Durabilidad (Durability):** Una vez confirmada una transacción mediante `COMMIT`, sus efectos persisten de forma permanente en la base de datos, incluso ante caídas del sistema, cortes de energía eléctrica o fallos de hardware. Esta garantía se implementa mediante técnicas como el Write-Ahead Logging (WAL).

## 4. El Modelo de Referencia ANSI/SPARC

### 4.1. Contexto y motivación

En los primeros años de la tecnología de bases de datos, cada fabricante implementaba su propio modelo de gestión con interfaces, lenguajes y arquitecturas propietarias e incompatibles entre sí. Un sistema desarrollado sobre IBM IMS no podía interoperar con uno basado en CODASYL, y la migración entre plataformas resultaba extraordinariamente costosa.

Para abordar esta fragmentación, en 1975 el **ANSI (American National Standards Institute)** constituyó el comité **SPARC (Standards Planning and Requirements Committee)**, que propuso una **arquitectura de referencia de tres niveles** aplicable a cualquier SGBD, independientemente de su implementación interna. El objetivo fundamental de esta arquitectura era lograr la **independencia de los datos**: la capacidad de modificar la definición de los datos en un nivel sin afectar a los niveles adyacentes.

### 4.2. Los tres niveles de la arquitectura ANSI/SPARC

#### 4.2.1. Nivel Interno o Físico (Esquema Interno)

Es el nivel más cercano al almacenamiento físico real. Define **cómo** y **dónde** se almacenan los datos en los dispositivos de almacenamiento.

*   **Contenido:** Especifica las estructuras de almacenamiento físico: formato de los registros en disco, organización de los ficheros (secuencial, indexada, hash), algoritmos de indexación (árboles B, B+, índices bitmap), técnicas de compresión, asignación de espacio en disco (tablespaces, datafiles), estrategias de clustering y particionamiento.
*   **Responsable:** Es gestionado por el administrador del sistema o el DBA (Database Administrator) en su faceta de administración física.
*   **Ejemplo:** En Oracle, este nivel comprende los datafiles, los tablespaces, los redo logs y los segmentos de rollback. En PostgreSQL, se materializa en los ficheros del directorio `PGDATA`, los WAL (Write-Ahead Logs) y la configuración de `postgresql.conf`.

#### 4.2.2. Nivel Lógico o Conceptual (Esquema Conceptual)

Es el nivel intermedio y constituye el **núcleo central** de la arquitectura ANSI/SPARC. Representa la visión global y unificada de toda la base de datos.

*   **Contenido:** Define **qué** datos se almacenan y **qué relaciones** existen entre ellos, con total independencia de su representación física. Incluye la definición de todas las tablas, atributos, tipos de datos, claves primarias, claves ajenas, restricciones de integridad y reglas de negocio.
*   **Responsable:** Es diseñado por el administrador de la base de datos (DBA) y los analistas de datos, generalmente mediante herramientas de modelado Entidad-Relación (E/R) que posteriormente se traducen al esquema relacional.
*   **Ejemplo:** El esquema conceptual de un ayuntamiento podría definir las tablas `CONTRIBUYENTE`, `INMUEBLE`, `LIQUIDACION`, `RECIBO` y sus relaciones, sin especificar en qué disco o con qué formato de indexación se almacenan.

#### 4.2.3. Nivel Externo o de Vistas (Esquema Externo)

Es el nivel más cercano a los usuarios finales y las aplicaciones. Proporciona visiones personalizadas y parciales de la base de datos adaptadas a las necesidades de cada grupo de usuarios.

*   **Contenido:** Define múltiples **vistas (views)** que muestran subconjuntos o transformaciones de los datos del esquema conceptual. Cada vista expone únicamente la información relevante para un perfil de usuario concreto, ocultando la complejidad del esquema global.
*   **Responsable:** Es definido por el DBA en colaboración con los responsables funcionales de cada área.
*   **Ejemplo:** En un ayuntamiento con 500 tablas en su esquema conceptual, la aplicación de Recursos Humanos accedería a una vista que expone únicamente los campos `Nombre`, `Apellidos`, `Puesto`, `Categoría` y `Salario` de los empleados. Mientras tanto, la aplicación de Recaudación accedería a otra vista completamente distinta con datos tributarios. Ninguna de las dos aplicaciones necesita conocer la existencia de las tablas que no le conciernen.

## 5. La Independencia de Datos: objetivo fundamental de la arquitectura ANSI/SPARC

La separación en tres niveles de la arquitectura ANSI/SPARC persigue como objetivo primordial alcanzar la **independencia de datos** en dos dimensiones complementarias:

### 5.1. Independencia física de datos

Se establece entre el **nivel interno** y el **nivel conceptual**. Permite modificar la organización física del almacenamiento sin necesidad de alterar el esquema conceptual ni las aplicaciones que acceden a los datos.

*   **Ejemplo práctico:** El DBA puede migrar la base de datos de discos HDD a discos SSD, cambiar la estrategia de indexación de un árbol B a un índice bitmap, o redistribuir los tablespaces entre diferentes unidades de almacenamiento, sin que las tablas, vistas ni las sentencias SQL de las aplicaciones sufran modificación alguna.

### 5.2. Independencia lógica de datos

Se establece entre el **nivel conceptual** y el **nivel externo**. Permite modificar el esquema conceptual (añadir tablas, renombrar columnas, reorganizar relaciones) sin afectar a las vistas y aplicaciones del nivel externo, siempre que la información que estas consumen siga estando disponible.

*   **Ejemplo práctico:** El DBA puede añadir una nueva columna `correo_electronico` a la tabla `CONTRIBUYENTE` o dividir una tabla en dos por razones de normalización. Las aplicaciones existentes que acceden a la base de datos a través de sus vistas predefinidas continúan funcionando sin necesidad de recompilación ni modificación, ya que sus vistas mantienen la misma estructura que tenían antes del cambio.

### 5.3. Correspondencias entre niveles (Mappings)

Las transformaciones entre niveles se implementan mediante **correspondencias** (mappings) gestionadas automáticamente por el SGBD:

*   **Correspondencia conceptual-interna:** Traduce las estructuras lógicas (tablas, columnas) a estructuras físicas (ficheros, páginas de disco, índices). Cuando cambia la organización física, solo se modifica esta correspondencia.
*   **Correspondencia externa-conceptual:** Traduce cada vista del nivel externo a las tablas y columnas del esquema conceptual. Cuando se reorganiza el esquema conceptual, solo se actualiza esta correspondencia, manteniendo intactas las vistas.

## 6. Conclusión

Los Sistemas Gestores de Bases de Datos Relacionales representan la tecnología de persistencia dominante en las organizaciones públicas y privadas desde hace más de cuatro décadas. Su fundamentación matemática en el modelo relacional de Codd proporciona un marco riguroso para la organización, consulta y protección de los datos, mientras que las propiedades ACID garantizan la fiabilidad transaccional imprescindible en aplicaciones críticas como la gestión tributaria, el padrón municipal o la contabilidad pública.

La arquitectura de referencia ANSI/SPARC, con su separación en tres niveles —interno, conceptual y externo—, constituye uno de los principios arquitectónicos más influyentes de la historia de la informática. La independencia física y lógica de los datos que esta arquitectura posibilita permite que las organizaciones evolucionen su infraestructura tecnológica y su modelo de datos sin desestabilizar las aplicaciones existentes, reduciendo drásticamente los costes de mantenimiento y facilitando la adaptación continua a nuevos requisitos funcionales y normativos.

En el contexto de las Administraciones Públicas, donde la continuidad del servicio, la integridad de la información y el cumplimiento normativo son exigencias ineludibles, el dominio de estas tecnologías y arquitecturas resulta esencial para cualquier profesional responsable de la gestión de los sistemas de información.
