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

## 3. Restricciones de Integridad del Modelo Relacional

1.  **Integridad de entidad:** Ningún componente de clave primaria puede contener valores nulos (NULL), clave primaria identifica inequívocamente cada tupla.
2.  **Integridad referencial:** Toda clave ajena contiene valor que exista como clave primaria en la relación referenciada o es nulo. Coherencia entre tablas.
3.  **Integridad de Dominio:** Valores de atributo respeten las restricciones de tipo de dato, rango, formato o restricciones CHECK.
4.  **Integridad definida por el usuario:** Reglas de negocio.

## 4. Las Doce Reglas de Codd

Fijan los requisitos para que un SGBD sea estrictamente relacional (independencia, acceso por valor, catálogo dinámico)

## 5. Normalización de Bases de Datos

Objetivo: Proceso formal para minimizar redundancia y evitar anomalías de modificación.
Formas Normales principales:

*   **1FN**: Atributos atómicos, sin grupos repetitivos.

*   **2FN**: Cumple 1FN + atributos no clave dependen de la clave primaria completa.

*   **3FN**: Cumple 2FN + sin dependencias transitivas.

*   **BCNF (Boyce-Codd)**: Todo determinante es una clave candidata.

## 6. Interoperabilidad entre SGBDR: Normas y Estándares

**Interoperabilidad** —> capacidad que aplicaciones accedan de forma transparente a BD heterogéneas.

### 6.1. SQL como estándar universal (ANSI/ISO)

**SQL (Structured Query Language)**, IBM, estandar ANSI en 1986 y adoptado por la ISO. 

(PL/SQL en Oracle, T-SQL en SQL Server, PL/pgSQL en PostgreSQL), pero todos respetan núcleo del estándar ANSI/ISO SQL definición, manipulación y control de datos.

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
