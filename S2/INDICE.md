# I. Introducción y Contexto del Curso
1.  **Modelo de Base de Datos**
    *   Énfasis en SQL PG y PostgreSQL.
    *   Temas principales: Modelado de datos y consultas avanzadas.

# II. Modelado de Datos
1.  **Normalización**
    *   **Concepto:** Proceso de organizar los datos en una base de datos para reducir la redundancia y evitar anomalías de actualización.
    *   Implica dividir tablas grandes en tablas más pequeñas.
    *   **Primera Forma Normal (1NF):** Eliminar los valores duplicados en una columna. Asegurar que cada columna tenga valores atómicos (un solo valor por celda).
    *   **Segunda Forma Normal (2NF):** Debe cumplir con 1NF y eliminar dependencias parciales. Todos los atributos deben depender completamente de la clave primaria.
    *   **Tercera Forma Normal (3NF):** Debe cumplir con 2NF y eliminar dependencia transitiva. No deben existir dependencias directas entre atributos no clave.
2.  **Desnormalización**
    *   **Concepto:** Proceso de modificar tablas previamente normalizadas para mejorar el rendimiento de las consultas.
    *   Implica introducir redundancia en los datos.
    *   **Ventajas:** Mejora la velocidad de las consultas. Es ideal para bases de datos de lectura intensiva (como sistemas de análisis o *reporting*). Reduce la complejidad de las consultas al evitar múltiples uniones (*joins*).
    *   **Desventajas:** Puede introducir datos duplicados. Aumenta el riesgo de inconsistencia debido a la redundancia. Hace más complejas las operaciones de escritura (insertar, actualizar, eliminar), ya que los cambios deben aplicarse en varios lugares.

# III. Estructura y Relaciones de Tablas
1.  **Claves y Creación de Tablas**
    *   La creación de tablas se realiza con la instrucción `CREATE`.
    *   **Clave Primaria (*Primary Key*):** Identifica de forma única cada registro en una tabla. Los valores no deben ser nulos y debe haber solo una clave primaria por tabla. Puede consistir en un solo campo o varios campos (claves primarias compuestas).
    *   **Clave Foránea (*Foreign Key*):** Conjunto de campos en una tabla que se usa para establecer una relación con la clave primaria de otra tabla. Permiten la integridad referencial.
2.  **Relaciones entre Tablas**
    *   **Uno a Uno (1:1):** Un registro en una tabla se relaciona exactamente con un registro en otra tabla.
    *   **Uno a Muchos (1:N):** Un registro en una tabla puede estar relacionado con varios registros en otra tabla. La clave foránea se coloca en la tabla hija, apuntando a la tabla padre.
    *   **Muchos a Muchos (N:M):** Varios registros en una tabla se relacionan con varios registros en otra. Se utiliza una tabla intermedia con dos claves foráneas que refieren a las tablas principales.

# IV. Consultas Avanzadas
1.  **Operaciones JOIN (Uniones)**
    *   **INNER JOIN:** Devuelve las filas que tienen coincidencia en ambas tablas.
    *   **LEFT JOIN (o LEFT OUTER JOIN):** Devuelve todas las filas de la tabla izquierda y las coincidencias de la tabla derecha; devuelve nulo si no hay coincidencia a la derecha.
    *   **RIGHT JOIN (o RIGHT OUTER JOIN):** Devuelve todas las filas de la tabla derecha y las coincidencias de la tabla izquierda; devuelve nulo si no hay coincidencia a la izquierda.
    *   **FULL OUTER JOIN:** Devuelve todas las filas cuando hay una coincidencia en cualquiera de las tablas; devuelve nulo en la tabla sin coincidencia.
2.  **Expresiones de Tablas Comunes (CTEs)**
    *   Son consultas anidadas dentro de otra consulta que trabajan de forma temporal.
    *   Son útiles para operaciones intermedias, como cálculos o filtrado.
    *   Pueden ser recursivas, lo que es útil para estructuras jerárquicas.
3.  **Agrupación de Datos (GROUP BY)**
    *   Se utiliza para generar indicadores, sumas o contabilizaciones.
    *   Se usa con funciones agregadas como `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.
4.  **Consultas de Ventana (*Window Queries*)**
    *   Utilizan las cláusulas `OVER` y `PARTITION BY`.
    *   Funciones de ventana: `ROW NUMBER` (asigna un número único a cada fila), `RANK` (asigna un rango, permitiendo saltos), `DENSE RANK` (asigna un rango sin saltar números en caso de empates), `SUM` (calcula la suma acumulada), y `AVG` (calcula un promedio acumulado).

# V. Índices y Optimización de Consultas
1.  **Tipos de Índices**
    *   **B-Tree:** Índice predeterminado y eficiente para búsquedas de igualdad y rango.
    *   **Hash:** Útil solo para comparación de igualdad.
    *   **GIST y GIN:** Útiles para datos complejos, como JSONB o texto completo.
2.  **Análisis y Optimización de Rendimiento**
    *   Crear índices en columnas que se usan constantemente para búsquedas.
    *   Evitar escaneos secuenciales asegurando que las columnas filtradas estén indexadas.
    *   Optimizar *joins* usando el tipo adecuado y asegurando que las columnas de unión estén indexadas.
    *   Crear índices en columnas de agrupación para consultas con `GROUP BY`.
    *   Actualizar estadísticas después de cambios significativos en los datos.
    *   Particionamiento de tablas grandes para mejorar el rendimiento.
    *   Reemplazar consultas con *join* por subconsultas cuando sea posible para optimizar.

# VI. Aplicación Práctica (Creación y Manejo de Tablas)
1.  **Manejo de Esquemas**
    *   Traslado de tablas desde el esquema `public` al esquema correcto (e.g., `tienda`) usando `ALTER TABLE`.
    *   Establecer un esquema por defecto (`SET search_path`) para la creación automática de nuevas tablas.
2.  **Creación de Tablas con Integridad Referencial**
    *   Creación de tablas maestras (`categoría`) y transaccionales (`productos`, `imágenes_productos`, `clientes`),,,.
    *   Uso de restricciones `ON DELETE RESTRICT` (para mantener la integridad) o `ON DELETE CASCADE` (para eliminar registros dependientes) en claves foráneas,.
3.  **Inserción de Datos Aleatorios**
    *   Utilización de la función `GENERATE_SERIES` para generar registros en masa y de forma aleatoria en las tablas.
4.  **Almacenamiento de Registros Complejos**
    *   Creación de una tabla (`skill_json`) para guardar registros usando el tipo de dato `JSONB`, que es una versión mejorada para PostgreSQL.
5.  **Creación de Índices para Rendimiento**
    *   Uso de `CREATE INDEX` en tablas grandes como `productos` para optimizar consultas, enfocándose en campos de relación como `categoría_ID`,.
6.  **Visualización del Modelo**
    *   Generación del Diagrama Entidad-Relación (DER) de la base de datos para visualizar las relaciones creadas (ejemplo: la relación entre `usuarios` y `clientes`),.
7.  **Eliminación de Tablas Relacionadas**
    *   Uso de la instrucción `DROP TABLE [nombre_tabla] CASCADE` para eliminar tablas que tienen relaciones referenciales, forzando la eliminación de objetos dependientes.