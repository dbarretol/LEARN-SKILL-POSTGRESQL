**Esquema de Títulos y Subtítulos Basado en la Gestión Avanzada de Bases de Datos PostgreSQL**

**I. Introducción a Funcionalidades Avanzadas de PostgreSQL**

*   Tercera sesión del curso de gestión de base de datos PostgreSQL.
*   Necesidad de implementar transacciones y control de concurrencia para una ejecución correcta.

**II. Transacciones y Propiedades ACID**

*   **Concepto de Transacción:** Conjunto de operaciones en la base de datos realizado como una unidad invisible.
*   **Propiedades Fundamentales (ACID)**,:
    *   **Atomicidad:** Todas las operaciones dentro de una transacción deben completarse exitosamente; si alguna falla, ninguna modificación debe reflejarse en la base de datos (principio de "todo o nada"),,.
    *   **Consistencia:** La base de datos debe pasar de un estado válido a otro, garantizando siempre la integridad de los datos antes y después de la transacción.
    *   **Aislamiento (Isolation):** Las transacciones deben ejecutarse de manera aislada de otras transacciones concurrentes.
        *   Niveles de Aislamiento: Lectura de no confirmados (`Read uncommitted`), Lectura de confirmados (`Read committed`), Repetir lecturas (`Repeatable read`), y Serialización (`Serializable`),. La serialización es el nivel más alto, donde las transacciones se ejecutan como si fueran secuenciales.
    *   **Durabilidad:** Los cambios realizados por una transacción confirmada deben persistir y no perderse, incluso si ocurre un fallo en el sistema (como un apagado del servidor),,.

*   **Control de Concurrencia Multiversión (MVCC)**:
    *   Definición: Técnica que gestiona el acceso concurrente a los datos permitiendo que múltiples transacciones operen sin interferir entre sí.
    *   Características Principales: Versionado de datos (crea nuevas versiones de registros sin sobrescribir los antiguos) y la no existencia de bloqueos de lectura.
    *   Ventajas: Mejora la concurrencia y el rendimiento al permitir transacciones rápidas sin esperar a que otras terminen.
    *   Implementación en PostgreSQL: Uso de marcas de tiempo de transacciones, donde cada `UPDATE` crea una nueva versión del dato. La limpieza de versiones antiguas se realiza mediante comandos `VACUUM`.

**III. Funciones, Procedimientos Almacenados y Triggers**

*   **Funciones y Procedimientos Almacenados (PL/pgSQL):** Permiten crear lógica compleja, incluyendo bucles, condicionales y manejo de excepciones, dentro de PostgreSQL.
    *   Declaración de Funciones: Uso de `CREATE FUNCTION`, definición de parámetros y tipo de valor de retorno (`RETURNS`), y el cuerpo de la función delimitado por `BEGIN` y `END`,.
*   **Diferencias Clave entre Funciones y Procedimientos Almacenados**,:
    *   Retorno de Valor: Las funciones devuelven un valor directamente con `RETURN`, mientras que los procedimientos almacenados no lo hacen,.
    *   Invocación: Las funciones se utilizan en consultas (`SELECT`), mientras que los procedimientos se ejecutan con el comando `CALL`.
    *   Transacciones: Las funciones no pueden inicializar una transacción, pero un procedimiento almacenado sí.
*   **Triggers (Disparadores):** Mecanismo para ejecutar automáticamente funciones definidas por el usuario antes o después de un evento en una tabla.
    *   Eventos Capturados: `INSERT`, `UPDATE`, `DELETE`, o `TRUNCATE`.

**IV. Vistas y Vistas Materializadas**

*   **Vistas Comunes:** Consultas almacenadas que actúan como tablas virtuales sin almacenar datos físicos.
    *   Usos: Extracción, ocultación de la complejidad de la consulta (ej. convertir 1000 líneas de código en una sola), seguridad (restricción de columnas/filas sensibles), y reutilización.
    *   Actualización: Se actualizan automáticamente, reflejando cualquier modificación en la tabla base de forma inmediata,,.
*   **Vistas Materializadas:** Similares a las vistas comunes, pero estas sí almacenan físicamente los datos en la base de datos,.
    *   Usos Recomendados: Necesidad de consultas rápidas con gran volumen de datos, cuando los datos no cambian frecuentemente, y para evitar cálculos costosos y repetitivos,.
    *   Actualización: No se actualizan automáticamente cuando la tabla base cambia,. La actualización debe realizarse manualmente usando el comando `REFRESH MATERIALIZED VIEW`,,.

**V. Integridad de Datos, Restricciones y Particionamiento**

*   **Integridad de Datos:** Verificación de la consistencia en las relaciones entre tablas (ej. asegurando que tablas relacionadas contengan el mismo número de registros, si aplica),.
*   **Verificación de Restricciones:** Inspección de las restricciones existentes (como *foreign keys*) entre tablas utilizando consultas al *information schema*,.
*   **Particionamiento de Tablas:** Técnica utilizada para gestionar grandes volúmenes de datos.
    *   Particionamiento por Rango (`PARTITION BY RANGE`): Creación de particiones basado en un rango de valores, como fechas,.

**VI. Seguridad y Control de Acceso**

*   **Roles y Permisos:** Fundamental para manejar la seguridad y generar usuarios con acceso restringido.
    *   Control de Acceso: PostgreSQL permite controlar el acceso desde el nivel de base de datos hasta columnas específicas.
*   **Seguridad de Conexiones y Encriptación:** Soporte para encriptación (SSL) y la capacidad de definir métodos de autenticación,.

**VII. Ejemplos de Consultas SQL Avanzadas**

*   **Consultas de Agregación:** Uso de funciones de agregación como `SUM` y cláusulas `GROUP BY` para obtener totales (ej. suma de precios de productos por categoría),,.
*   **Subconsultas:** Uso de consultas internas (ej. obtener productos con precios mayores al promedio),.