# Índice General del Curso de PostgreSQL

## I. Introducción a PostgreSQL y Fundamentos de Bases de Datos

### A. Conceptos básicos
- ¿Qué es una **base de datos**?
- ¿Qué es un **SGBD** (Sistema de Gestión de Bases de Datos)?
- PostgreSQL como **ORDBMS** (sistema de base de datos relacional de objetos).
- Rol de PostgreSQL dentro de una arquitectura de aplicaciones.

### B. Componentes fundamentales de una base de datos relacional
- **Tablas**: estructura básica de almacenamiento (filas y columnas).
- **Campos o columnas**: definición de atributos y tipos de datos.
- **Registros o filas**: conjunto coherente de datos en una tabla.
- **Clave primaria (Primary Key)**:
  - Identificación única de cada registro.
  - Restricciones típicas: `PRIMARY KEY`, `UNIQUE`, `NOT NULL`.
- **Clave foránea (Foreign Key)**:
  - Referencias entre tablas.
  - Integridad referencial.
- **Consultas (queries)**:
  - Operaciones básicas: `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
- **Lenguaje SQL**:
  - Lenguaje estándar para definición y manipulación de datos.
- **Lenguaje PL/pgSQL**:
  - Lenguaje procedural específico de PostgreSQL.
  - Uso en funciones, procedimientos y triggers.

### C. Conceptos avanzados de gestión de datos
- **Relaciones entre tablas**:
  - Tipos de relaciones (1:1, 1:N, N:M).
  - Uso de joins (`INNER JOIN`, `LEFT JOIN`, etc.).
- **Índices**:
  - Objetivo y beneficios.
  - Impacto en rendimiento de lectura y escritura.
- **Normalización**:
  - Reducción de redundancia y mejora de integridad.
  - Formas normales (visión general).
- **Transacciones**:
  - Concepto de unidad atómica “todo o nada”.
  - Propiedades ACID (Atomicidad, Consistencia, Aislamiento, Durabilidad).
- **Backups**:
  - Importancia de las copias de seguridad.
  - Visión general de métodos de backup y restauración.

---

## II. Comparación de Gestores de Bases de Datos (PostgreSQL, SQL Server, MySQL)

### A. Aspectos de licencia y desarrollo
- **PostgreSQL**:
  - Open Source (licencia PostgreSQL, muy permisiva).
  - Desarrollado por PostgreSQL Global Development Group.
- **SQL Server**:
  - Producto propietario de Microsoft.
  - Licenciamiento por núcleo/CAL según edición.
- **MySQL**:
  - Open Source (GPL) con ediciones comerciales.
  - Desarrollado por Oracle Corporation.

### B. Características técnicas comparadas
- **Modelo de datos**:
  - PostgreSQL: objeto-relacional (ORDBMS).
  - SQL Server y MySQL: relacionales.
- **Soporte JSON / NoSQL ligero**:
  - PostgreSQL: `JSON` / `JSONB`, índices GIN, funciones avanzadas.
  - SQL Server: soporte funcional de JSON vía T‑SQL.
  - MySQL: tipo `JSON`, soporte intermedio.
- **Rendimiento**:
  - PostgreSQL: fuerte en consultas complejas y escrituras con alta concurrencia.
  - SQL Server: excelente en entornos corporativos bien dimensionados.
  - MySQL: muy rápido en lecturas simples y patrones típicos web.
- **Escalabilidad**:
  - PostgreSQL: buena escalabilidad vertical y horizontal (replicación, particionado).
  - SQL Server: gran escalabilidad vertical; opciones de HA/DR empresariales.
  - MySQL: escalabilidad moderada, comúnmente usada con réplicas de lectura.
- **Transacciones ACID**:
  - PostgreSQL, SQL Server: soporte completo.
  - MySQL: depende del motor de almacenamiento (InnoDB recomendado).
- **Índices avanzados**:
  - PostgreSQL: GIN, GiST, BRIN, índices parciales, multicolumna, por expresión.
  - SQL Server: B‑Tree, Columnstore, índices filtrados.
  - MySQL: principalmente B‑Tree, otros tipos más limitados.
- **Particionado de tablas**:
  - PostgreSQL: particionado nativo (RANGE, LIST, HASH).
  - SQL Server: particionado maduro y bien integrado.
  - MySQL: particionado disponible con mayor complejidad y restricciones.
- **Seguridad**:
  - PostgreSQL: roles granulares, políticas de fila (RLS), autenticación flexible.
  - SQL Server: integración nativa con Active Directory y herramientas de gobierno.
  - MySQL: seguridad adecuada, menos sofisticada en entornos de gran gobernanza.
- **Uso ideal**:
  - PostgreSQL: aplicaciones complejas, analítica, GIS, finanzas.
  - SQL Server: grandes empresas en entorno Microsoft.
  - MySQL: aplicaciones web ligeras/medias, CMS, hosting compartido.

---

## III. Tipos de Datos en PostgreSQL

### A. Tipos numéricos
- `SMALLINT`, `INTEGER`, `BIGINT`: enteros de distinto rango.
- `DECIMAL(p, s)` / `NUMERIC(p, s)`: decimales de precisión exacta (ej. montos).
- `REAL`, `DOUBLE PRECISION`: números en punto flotante (precisión simple y doble).

### B. Tipos de texto
- `VARCHAR(n)`: longitud variable con límite máximo.
- `CHAR(n)`: longitud fija.
- `TEXT`: longitud prácticamente ilimitada (ideal para descripciones largas).

### C. Tipos lógicos, de fecha y avanzados
- **Lógico**:
  - `BOOLEAN`: `TRUE`, `FALSE`, `NULL`.
- **Fecha y hora**:
  - `DATE`, `TIME`, `TIMESTAMP`, `TIMESTAMPTZ` (con zona horaria), `INTERVAL`.
- **Tipos estructurados / NoSQL**:
  - `JSON` y `JSONB`: almacenamiento de documentos JSON.
  - `ARRAY`: arrays de valores (`integer[]`, `text[]`, etc.).
  - `HSTORE`: pares clave–valor en una sola columna.

*(En el desarrollo del contenido se incluyen ejemplos de creación de tablas usando estos tipos.)*

---

## IV. Instalación y Configuración del Servidor PostgreSQL

### A. Proceso de instalación
- Instalación en:
  - **Windows**: instalador oficial.
  - **Linux**: repositorios de la distribución (`apt`, `yum`, etc.) o Docker.
  - **macOS**: Homebrew o instalador oficial.
- Configuración inicial:
  - Puesto por defecto: `5432`.
  - Creación y prueba de conexión inicial.
  - Verificación del servicio.

### B. Herramientas de interacción
- **pgAdmin 4**:
  - Administración gráfica desde navegador.
  - Creación de servidores, bases de datos, esquemas, tablas y roles.
- **psql**:
  - Consola oficial de PostgreSQL.
  - Ejecución de sentencias SQL y comandos internos (`\l`, `\dt`, etc.).

### C. Primeros pasos y comandos básicos
- Uso del **Query Tool** en pgAdmin.
- Manipulación de datos con SQL:
  - `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
- Consulta de bases de datos predeterminadas:
  - `postgres`, `template1`, `template0`.

*(Ejemplos incluidos: creación de tabla sencilla, inserción y consulta básica.)*

---

## V. Creación de Bases de Datos y Esquemas

### A. Creación y eliminación de bases de datos
- Creación de bases de datos de ejemplo (`tienda_db`):
  - `CREATE DATABASE tienda_db WITH OWNER = ... ENCODING = 'UTF8' TEMPLATE = template0;`
- Eliminación de bases de datos (`DROP DATABASE`):
  - Manejo de conexiones activas con `pg_stat_activity`.
  - Uso de `pg_terminate_backend()` antes de `DROP DATABASE`.

### B. Definición y uso de esquemas
- Esquema por defecto: `public`.
- Buenas prácticas:
  - Crear esquemas propios (`CREATE SCHEMA IF NOT EXISTS tienda;`).
  - Separación lógica por módulos o dominios.
- Configuración de `search_path`:
  - `SET search_path TO tienda;`
  - Ajuste persistente con `ALTER ROLE` / `ALTER DATABASE`.

---

## VI. Creación de Tablas con Características Avanzadas

### A. Extensiones y generación de IDs únicos
- Uso de extensiones:
  - `uuid-ossp` o `pgcrypto` para generar UUIDs (`uuid_generate_v4()`, `gen_random_uuid()`).
- Ventajas de `UUID` frente a `SERIAL`/`BIGSERIAL`:
  - Unicidad global.
  - Ocultación del orden real de inserción.
- Función de utilidad `random_string` en PL/pgSQL para generar textos aleatorios.

*(Se incluye código de ejemplo para `CREATE EXTENSION`, definición de tablas con UUID y funciones auxiliares.)*

### B. Definición de la tabla de usuarios
- Tabla `usuario`:
  - `usuario_id` como `UUID PRIMARY KEY` con `DEFAULT uuid_generate_v4()` o `gen_random_uuid()`.
  - Campos típicos: `nombres`, `apellidos`, `email`, `password`, `estado`, `fecha_creacion`, `ultimo_login`, `es_admin`.
- Validaciones (`constraints`):
  - `NOT NULL` y `UNIQUE` (ej. email).
  - `CHECK` para:
    - Validar formato de email (expresiones regulares).
    - Restringir valores de `estado`.

### C. Tipos personalizados (`ENUM`)
- Creación de tipos enumerados:
  - `CREATE TYPE estado_de_usuario AS ENUM ('activo', 'inactivo', 'suspendido');`
- Uso de `ENUM` en la tabla `usuario`:
  - Columna `estado` tipada como `estado_de_usuario`.
- Ventajas:
  - Mayor claridad semántica.
  - Menos errores de escritura en valores de estado.

---

## VII. Inserción de Registros de Forma Aleatoria

### A. Inserción masiva con `INSERT INTO ... SELECT`
- Generación de datos de prueba usando:
  - `INSERT INTO ... SELECT`.
  - `generate_series(1, N)` para producir N filas.
- Ejemplo básico:
  - Inserción de 100 usuarios de prueba en la tabla `usuario`.

### B. Generación de datos aleatorios y patrones
- Construcción de nombres, apellidos y correos:
  - Concatenación de texto con el índice `i` (`'nombre_' || i`).
- Generación de contraseñas:
  - `MD5(random()::TEXT)` como hash de contraseña de prueba.
- Campos con lógica condicional:
  - Uso de `CASE WHEN` y operador módulo (`%`) para:
    - Asignar teléfonos según patrones.
    - Marcar ciertos usuarios como administradores (`es_admin`) de forma proporcional.

### C. Verificación y reutilización
- Verificación de los datos insertados:
  - `SELECT * FROM usuario;`
  - Conteos y agrupaciones de prueba.
- Limpieza para nuevas pruebas:
  - `TRUNCATE TABLE usuario RESTART IDENTITY;` (según diseño de la PK).

---

> Este índice sirve como guía estructurada para el documento completo del curso, abarcando desde los fundamentos de PostgreSQL hasta técnicas avanzadas de modelado, validación y generación de datos de prueba.