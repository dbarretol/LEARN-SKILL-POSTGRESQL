# Esquema de Contenidos: Gestión Avanzada de Bases de Datos con PostgreSQL

## I. Introducción a Funcionalidades Avanzadas de PostgreSQL

- Contexto de la **tercera sesión** del curso de PostgreSQL.
- Motivación:
  - Necesidad de **transacciones** para garantizar operaciones consistentes.
  - Importancia del **control de concurrencia** en entornos multiusuario.
  - Relación con temas previos: modelo de datos y consultas SQL básicas/medias.

---

## II. Transacciones y Propiedades ACID

### 1. Concepto de transacción

- Conjunto de operaciones que se ejecutan como una **unidad lógica**:
  - Se confirma todo (**COMMIT**) o se deshace por completo (**ROLLBACK**).
- Uso típico:
  - Transferencias bancarias.
  - Operaciones de inventario.
  - Procesos de facturación.

*(En el desarrollo del módulo se mostrarán ejemplos con `BEGIN`, `COMMIT`, `ROLLBACK`.)*

### 2. Propiedades ACID

- **Atomicidad**  
  - “Todo o nada”: si una parte de la transacción falla, **no se aplica ningún cambio**.
- **Consistencia**  
  - La BD pasa de un **estado válido** a otro, cumpliendo todas las **restricciones** (PK, FK, CHECK, etc.).
- **Aislamiento (Isolation)**  
  - Las transacciones concurrentes no deben interferir de forma incorrecta entre sí.
  - **Niveles de aislamiento**:
    - `READ UNCOMMITTED`
    - `READ COMMITTED` (por defecto en PostgreSQL)
    - `REPEATABLE READ`
    - `SERIALIZABLE` (máximo aislamiento: simula ejecución secuencial).
- **Durabilidad**  
  - Una vez hecho `COMMIT`, los cambios **permanecen**, incluso ante fallos del sistema o reinicios.

### 3. Control de Concurrencia Multiversión (MVCC)

- **Definición**  
  - Técnica en la que cada transacción ve una **“fotografía lógica”** de los datos, sin bloquear lecturas.
- **Características principales**:
  - Versionado de filas: cada `UPDATE`/`DELETE` crea una **nueva versión**, sin sobrescribir inmediatamente.
  - Lecturas sin bloqueos: lecturas no bloquean escrituras y viceversa (en la mayoría de casos).
- **Ventajas**:
  - Alta **concurrencia**.
  - Mejor **rendimiento** bajo alta carga.
- **Implementación en PostgreSQL**:
  - Uso de IDs de transacción y marcas de visibilidad.
  - Limpieza de versiones antiguas mediante `VACUUM` / `AUTOVACUUM`.

---

## III. Funciones, Procedimientos Almacenados y Triggers

### 1. Funciones y procedimientos en PL/pgSQL

- **Funciones (`CREATE FUNCTION`)**:
  - Permiten encapsular lógica compleja:
    - Variables, bucles, condiciones, manejo de errores.
  - Devuelven un **valor** (`RETURNS ...`) o un conjunto de filas (`RETURNS SETOF ...`).
  - Se pueden invocar en:
    - `SELECT`, `INSERT`, `UPDATE`, `DELETE`, expresiones, etc.
- **Procedimientos (`CREATE PROCEDURE`)**:
  - Se ejecutan con `CALL`.
  - No devuelven un valor directo (pueden usar parámetros `IN/OUT`).
  - Pueden **iniciar y controlar transacciones internas**.

*(En el desarrollo se mostrarán ejemplos de funciones y procedimientos con `BEGIN ... END` y `EXCEPTION`.)*

### 2. Diferencias clave entre funciones y procedimientos

- **Retorno**:
  - Función → siempre devuelve algo con `RETURN`.
  - Procedimiento → no devuelve un valor directo (solo por parámetros de salida).
- **Invocación**:
  - Función → en una consulta (`SELECT mi_funcion(...)`).
  - Procedimiento → con `CALL mi_procedimiento(...);`
- **Transacciones**:
  - Función → no puede usar `COMMIT`/`ROLLBACK` internos.
  - Procedimiento → puede abrir/confirmar/deshacer transacciones internas.

### 3. Triggers (disparadores)

- **Concepto**:
  - Mecanismos que ejecutan **automáticamente** una función cuando ocurre un evento en una tabla.
- **Eventos soportados**:
  - `INSERT`
  - `UPDATE`
  - `DELETE`
  - `TRUNCATE`
- **Momento de ejecución**:
  - `BEFORE` → antes de la operación.
  - `AFTER` → después de la operación.
- Casos de uso:
  - Auditoría (log de cambios).
  - Validaciones avanzadas.
  - Sincronización entre tablas.

---

## IV. Vistas y Vistas Materializadas

### 1. Vistas (views) normales

- **Definición**:
  - Consultas almacenadas que se exponen como **tablas virtuales**.
  - No almacenan datos, solo la **definición de la consulta**.
- **Usos principales**:
  - Simplificar consultas muy complejas.
  - Dar una “vista lógica” según roles (seguridad, ocultar columnas).
  - Reutilizar lógica de negocio SQL.
- **Actualización**:
  - Siempre muestran los **datos actuales** de las tablas base.

### 2. Vistas materializadas

- **Definición**:
  - Vistas que **almacenan físicamente** el resultado de la consulta.
- **Ventajas**:
  - Mejoran el rendimiento en:
    - Consultas pesadas.
    - Agregaciones sobre grandes volúmenes.
  - Ideales cuando los datos subyacentes **cambian poco**.
- **Actualización**:
  - No se actualizan automáticamente al cambiar las tablas base.
  - Requieren `REFRESH MATERIALIZED VIEW nombre_vista;`.

---

## V. Integridad de Datos, Restricciones y Particionamiento

### 1. Integridad de datos

- Garantiza que las relaciones y reglas del modelo se cumplan:
  - Correspondencia entre tablas relacionadas (FK).
  - Restricciones de negocio (`CHECK`, `UNIQUE`, etc.).
- Verificación mediante:
  - Consultas a `information_schema` y vistas del catálogo del sistema.

### 2. Restricciones (constraints) y validación

- Tipos principales:
  - `PRIMARY KEY`
  - `FOREIGN KEY`
  - `UNIQUE`
  - `CHECK`
  - `NOT NULL`
- Revisión de restricciones existentes:
  - Uso de `information_schema.table_constraints`, `key_column_usage`, etc.

### 3. Particionamiento de tablas

- **Objetivo**:
  - Mejorar rendimiento y manejabilidad de **tablas muy grandes**.
- **Particionamiento por rango (`PARTITION BY RANGE`)**:
  - Dividir datos por:
    - Rango de fechas (mensual, anual, etc.).
    - Rango numérico.
- Otros tipos (en el contenido extendido):
  - Particionado por **lista**.
  - Particionado por **hash**.
  - Particionado **compuesto**.

---

## VI. Seguridad y Control de Acceso

### 1. Roles y permisos

- PostgreSQL usa **roles** para:
  - Representar usuarios y grupos.
  - Asignar permisos a múltiples usuarios de forma centralizada.
- Alcance del control de acceso:
  - Servidor.
  - Base de datos.
  - Esquema.
  - Tabla, vista, función.
  - Incluso **columna** (column-level privileges).

*(En el desarrollo se ejemplifican `CREATE ROLE`, `GRANT`, `REVOKE`.)*

### 2. Seguridad de conexiones y cifrado

- Soporte para:
  - Conexiones cifradas mediante **SSL/TLS**.
  - Diferentes métodos de autenticación (`md5`, `scram-sha-256`, certificados, LDAP, etc.).
- Configuración en:
  - `pg_hba.conf` → reglas de acceso.
  - `postgresql.conf` → parámetros de SSL y seguridad.

---

## VII. Ejemplos de Consultas SQL Avanzadas

### 1. Consultas de agregación

- Uso de funciones:
  - `SUM`, `COUNT`, `AVG`, `MIN`, `MAX`.
- Combinación con `GROUP BY` y `HAVING` para:
  - Totales por categoría (ej. suma de precios por categoría).
  - Indicadores de negocio: ventas por cliente, por mes, etc.

### 2. Subconsultas y patrones avanzados

- **Subconsultas en `WHERE` y `FROM`**:
  - Ejemplo: productos con precio mayor al **promedio** de su categoría.
- Patrones:
  - Subconsultas correlacionadas.
  - Subconsultas para filtros complejos.
  - Combinación con CTEs para mayor legibilidad.

*(A lo largo del módulo se incluirán ejemplos de código SQL reales para cada uno de estos patrones.)*