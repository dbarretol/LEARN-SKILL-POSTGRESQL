# I. Introducción y contexto del curso

## 1. Modelo de base de datos
- Enfoque en **PostgreSQL** y el dialecto **SQL estándar** (con particularidades de PG).
- Ejes principales:
  - **Modelado de datos relacional**.
  - **Consultas avanzadas** y optimización.

---

# II. Modelado de datos

## 1. Normalización
- **Concepto general**  
  Proceso de organizar los datos para:
  - Reducir **redundancia**.
  - Evitar **anomalías** en inserciones, actualizaciones y eliminaciones.
  - Mejorar la **integridad** y **consistencia**.

- **Primera Forma Normal (1NF)**  
  - Cada columna contiene **valores atómicos** (un solo valor por celda).
  - No se permiten **listas** o **repeticiones** en una misma columna.
  - No existen filas duplicadas.

- **Segunda Forma Normal (2NF)**  
  - Cumple 1NF.
  - No hay **dependencias parciales** respecto a una clave primaria compuesta:
    - Todos los atributos no clave dependen **por completo** de la clave primaria.

- **Tercera Forma Normal (3NF)**  
  - Cumple 2NF.
  - No existen **dependencias transitivas**:
    - Ningún atributo no clave depende de otro atributo no clave.

## 2. Desnormalización
- **Concepto**  
  Proceso de **relajar la normalización** (introducir cierta redundancia) para mejorar el **rendimiento de lectura**.

- **Ventajas**  
  - Mejora el **tiempo de respuesta** en consultas frecuentes.
  - Ideal para sistemas de **lectura intensiva** (reportes, analítica, dashboards).
  - Puede reducir la complejidad de consultas (menos `JOIN`).

- **Desventajas**  
  - Introduce **datos duplicados**.
  - Aumenta el riesgo de **inconsistencia** si no se actualiza correctamente.
  - Complica operaciones de **escritura** (INSERT/UPDATE/DELETE) al requerir cambios en múltiples lugares.

---

# III. Estructura y relaciones de tablas

## 1. Claves y creación de tablas
- Creación de tablas con `CREATE TABLE`.
- **Clave primaria (Primary Key)**  
  - Identifica de forma **única** cada registro.
  - No admite valores `NULL`.
  - Solo puede haber **una PK por tabla** (puede ser simple o compuesta).
- **Clave foránea (Foreign Key)**  
  - Campo(s) que referencian la **PK de otra tabla**.
  - Mantiene la **integridad referencial** entre tablas.

## 2. Tipos de relaciones entre tablas
- **Uno a uno (1:1)**  
  - Un registro en la tabla A se asocia con **un solo** registro en la tabla B.
- **Uno a muchos (1:N)**  
  - Un registro en la tabla padre se relaciona con **varios** registros en la tabla hija.
  - La **FK** vive en la tabla hija apuntando a la PK de la tabla padre.
- **Muchos a muchos (N:M)**  
  - Varios registros en A se relacionan con varios registros en B.
  - Se utiliza una **tabla intermedia** con dos FKs (hacia A y hacia B).

---

# IV. Consultas avanzadas

## 1. Operaciones `JOIN` (uniones)
- **`INNER JOIN`**  
  - Devuelve solo las filas con **coincidencia en ambas tablas**.
- **`LEFT JOIN` / `LEFT OUTER JOIN`**  
  - Todas las filas de la tabla izquierda + coincidencias de la derecha.
  - Donde no hay coincidencia, las columnas de la derecha son `NULL`.
- **`RIGHT JOIN` / `RIGHT OUTER JOIN`**  
  - Todas las filas de la tabla derecha + coincidencias de la izquierda.
  - Donde no hay coincidencia, las columnas de la izquierda son `NULL`.
- **`FULL OUTER JOIN`**  
  - Devuelve **todas las filas** que existan en cualquiera de las tablas.
  - Donde no hay coincidencia en una tabla, sus columnas aparecen como `NULL`.

## 2. Expresiones de tablas comunes (CTEs)
- Definición de **subconsultas nombradas** usando `WITH`.
- Útiles para:
  - Dividir consultas complejas en pasos intermedios.
  - Reutilizar resultados parciales en la misma consulta.
- **CTEs recursivas**:
  - Adecuadas para estructuras **jerárquicas** (árboles, organigramas, etc.).

## 3. Agrupación de datos (`GROUP BY`)
- Se utiliza para agrupar filas y calcular:
  - **Contadores** (`COUNT`).
  - **Sumas** (`SUM`).
  - **Promedios** (`AVG`).
  - **Mínimos y máximos** (`MIN`, `MAX`).
- Base para construir **indicadores**, reportes y estadísticas.

## 4. Consultas de ventana (*window functions*)
- Uso de la cláusula `OVER` (con/ sin `PARTITION BY` y `ORDER BY`).
- Permiten calcular valores **sobre un conjunto de filas relacionado**, sin colapsarlas en una sola fila.
- Funciones de ventana frecuentes:
  - `ROW_NUMBER()` → numera filas de un particionado/orden.
  - `RANK()` → asigna rangos con **saltos** en caso de empates.
  - `DENSE_RANK()` → rangos **sin saltos** en empates.
  - `SUM()` → suma acumulada sobre una ventana.
  - `AVG()` → promedio móvil / promedio sobre una ventana.

---

# V. Índices y optimización de consultas

## 1. Tipos de índices en PostgreSQL
- **B‑Tree (por defecto)**  
  - Ideal para búsquedas por **igualdad** y **rango**.
- **Hash**  
  - Especializado en búsquedas por **igualdad** (uso más limitado).
- **GiST y GIN**  
  - Adecuados para:
    - Datos complejos (espaciales, rangos, etc.).
    - Búsqueda en `JSONB` o texto completo.

## 2. Análisis y optimización de rendimiento
- Crear índices en:
  - Columnas usadas en **filtros** (`WHERE`).
  - Columnas de **uniones** (`JOIN`).
  - Columnas empleadas frecuentemente en `ORDER BY` y `GROUP BY`.
- Evitar **escaneos secuenciales innecesarios** asegurando índices adecuados.
- Optimizar `JOIN`:
  - Asegurar que las columnas de unión estén indexadas.
  - Revisar planes de ejecución (`EXPLAIN`, `EXPLAIN ANALYZE`).
- Mantener estadísticas actualizadas:
  - Uso de `ANALYZE` o autovacuum para que el planificador elija buenos planes.
- Considerar:
  - **Particionado** de tablas grandes.
  - Simplificación/re-escritura de consultas complejas (subconsultas, CTEs, etc.) basándose en el plan de ejecución, no en reglas genéricas.

---

# VI. Aplicación práctica: creación y manejo de tablas

## 1. Manejo de esquemas
- Reorganización de tablas:
  - Mover tablas desde `public` a un esquema de aplicación (ej. `tienda`) con `ALTER TABLE ... SET SCHEMA`.
- Definir esquema por defecto:
  - Uso de `SET search_path TO tienda;` para:
    - Crear nuevas tablas directamente en el esquema deseado.
    - Evitar prefijos constantes (`tienda.tabla`).

## 2. Creación de tablas con integridad referencial
- Diseño de:
  - Tablas **maestras** (ej. `categoria`).
  - Tablas **transaccionales** (ej. `productos`, `imagenes_productos`, `clientes`).
- Uso de claves foráneas con distintas políticas:
  - `ON DELETE RESTRICT` → impide borrar padres con hijos existentes.
  - `ON DELETE CASCADE` → elimina automáticamente los hijos al borrar el padre.

## 3. Inserción de datos aleatorios
- Uso de `GENERATE_SERIES` y funciones aleatorias para:
  - Generar datos de prueba en masa.
  - Poblar tablas como `usuarios`, `productos`, etc.

## 4. Almacenamiento de registros complejos
- Uso de columnas `JSONB` (ej. tabla `skill_json`) para:
  - Guardar datos semiestructurados.
  - Combinar flexibilidad NoSQL con la potencia de PostgreSQL.

## 5. Creación de índices para rendimiento
- Aplicación de `CREATE INDEX` en tablas grandes (ej. `productos`):
  - Índices sobre claves foráneas (ej. `categoria_id`) para acelerar:
    - `JOIN`.
    - Filtros por categoría.

## 6. Visualización del modelo
- Generación de un **Diagrama Entidad‑Relación (DER)**:
  - Visualización de relaciones entre tablas como:
    - `usuarios`, `clientes`, `productos`, etc.
  - Apoyo al análisis y documentación del modelo.

## 7. Eliminación de tablas relacionadas
- Uso de:
  - `DROP TABLE nombre_tabla;` → si no hay dependencias.
  - `DROP TABLE nombre_tabla CASCADE;` → para eliminar tablas con FKs y objetos dependientes.
- Consideraciones:
  - Impacto en integridad referencial.
  - Necesidad de backups previos en entornos reales.