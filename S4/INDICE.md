# Esquema de Contenidos: Administración Avanzada en PostgreSQL

## I. Introducción y contexto del curso

1. **Cuarta sesión del curso**
   - Enfoque en administración avanzada de PostgreSQL.
   - Relación con sesiones previas (modelado, consultas, fundamentos).

2. **Inicio de la sesión y conexión al servidor**
   - Verificación de audio, video y grabación.
   - Conexión a la instancia de PostgreSQL.
   - Selección de la base de datos de trabajo (`tienda`).

---

## II. Administración y herramientas avanzadas: copias de seguridad y restauración

### A. Copias de seguridad (backups)

1. **Métodos principales de backup**
   - `pg_dump` / `pg_dumpall` (backups lógicos).
   - `pg_basebackup` (backups físicos).

2. **`pg_dump`: script de base de datos (estructura + datos)**
   - Generación de scripts SQL (`.sql`).
   - Formato personalizado (`-Fc`, `.backup`) para uso con `pg_restore`.
   - Uso típico: migraciones, respaldos lógicos portables.

3. **`pg_basebackup`: backup físico del clúster**
   - Generación de copia completa del *cluster*.
   - Adecuado para recuperación ante desastres y servidores en *standby*.

4. **Comparación y uso recomendado**
   - `pg_dump`:
     - Respaldo a nivel de base de datos.
     - Alta portabilidad entre versiones.
     - Ideal para migraciones y respaldos ligeros.
   - `pg_basebackup`:
     - Respaldo de todo el clúster.
     - Portabilidad limitada a versiones/arquitecturas compatibles.
     - Recomendado para *Disaster Recovery* y réplicas.

### B. Restauración de bases de datos

1. **Herramientas de restauración**
   - `psql` para scripts SQL (`.sql`).
   - `pg_restore` para backups en formato personalizado (`.backup`).

2. **Restauración desde `pg_basebackup`**
   - Copia de directorios de datos.
   - Arranque/reinicio del motor con el backup físico.

3. **Tipos de archivos de respaldo**
   - `.sql` → script plano (salida de `pg_dump` / `pg_dumpall`).
   - Formato personalizado (`.backup`, `.pgdump`, etc.) → `pg_dump -Fc`.
   - Backup físico → estructura de directorios generada por `pg_basebackup`.

4. **Métodos de ejecución**
   - Restauración lógica:
     - `pg_restore` o interfaz gráfica de **pgAdmin** para archivos `.backup`.
     - `psql` para archivos `.sql`.
   - Restauración física:
     - Uso de línea de comandos (CMD/terminal) para copiar y reemplazar el *data directory*.

---

## III. Optimización de bases de datos

### A. Particionado de tablas

1. **Concepto**
   - División de tablas grandes en **particiones** (subtablas) más pequeñas.

2. **Beneficios**
   - Mejor gestión de grandes volúmenes de datos.
   - Consultas más rápidas (pruning de particiones).
   - Eliminación eficiente de datos antiguos por partición.
   - Optimización de índices y análisis.

3. **Tipos de particionado en PostgreSQL**
   - `RANGE` (rango): por fechas, números, etc.
   - `LIST` (lista de valores).
   - `HASH`: distribución uniforme basada en hash.
   - Compuesto (*composite*): combinación multinivel (p. ej. rango + hash).

### B. Uso de tablespaces

1. **Concepto**
   - Ubicación física en el sistema de archivos donde PostgreSQL almacena datos (tablas, índices, etc.).

2. **Beneficios**
   - Optimización de rendimiento al separar tablas e índices en distintos discos.
   - Distribución de carga de E/S entre varios volúmenes.

3. **Gestión del espacio**
   - Facilita la administración del almacenamiento.
   - Permite segmentar datos por velocidad, costo o criticidad.

---

## IV. Monitoreo y mantenimiento

### A. Comandos de mantenimiento

1. **Objetivo**
   - Mantener el **rendimiento** y la **salud** de la base de datos.
   - Evitar bloat y problemas de estadísticas desactualizadas.

2. **Comandos principales**
   - `VACUUM`
   - `VACUUM FULL`
   - `VACUUM ANALYZE`
   - `ANALYZE`

3. **Tipos de `VACUUM`**
   - `VACUUM`: limpia **tuplas muertas** y marca espacio reutilizable.
   - `VACUUM FULL`: limpia y **compacta** tablas; libera espacio a nivel de sistema de archivos.
   - `VACUUM ANALYZE`: combina limpieza con actualización de estadísticas.

4. **`ANALYZE`**
   - Actualiza exclusivamente las **estadísticas** de la base de datos para el planificador de consultas.

### B. Herramientas de monitoreo

1. **Objetivo**
   - Analizar rendimiento.
   - Detectar consultas lentas o bloqueadas.
   - Optimizar uso de recursos.

2. **`pg_stat_activity`**
   - Monitoreo en tiempo real de:
     - Conexiones activas.
     - Consultas en ejecución.
     - Estados de sesión y bloqueos.

3. **`pg_stat_statements`**
   - Estadísticas agregadas de consultas ejecutadas:
     - Número de llamadas.
     - Tiempo total y promedio.
     - Identificación de consultas más costosas a largo plazo.

---

## V. Replicación y alta disponibilidad

### A. Concepto de replicación

- Proceso de copiar datos desde una base de datos principal (*primary*) hacia una o varias réplicas (*standby*), normalmente como **espejos** para:
  - Alta disponibilidad.
  - Balanceo de lectura.
  - Recuperación ante desastres.

### B. Tipos de replicación

1. **Streaming replication (tiempo real)**
   - Replica cambios en tiempo (casi) real usando registros WAL.
   - Ventajas:
     - Alta disponibilidad.
     - Bajo impacto en el rendimiento del *primary*.
     - Distribución de carga de lectura.
   - Puede ser síncrona o asíncrona.

2. **Replicación síncrona**
   - El *primary* **espera confirmación** de al menos una réplica antes de confirmar una transacción.
   - Minimiza riesgo de pérdida de datos.

3. **Replicación asíncrona**
   - El *primary* **no espera** confirmación de réplicas.
   - Mejor rendimiento en escrituras, con posible pérdida de los cambios más recientes si el *primary* falla.

4. **Replicación lógica**
   - Replica solo **partes específicas** de la base de datos (tablas, esquemas).
   - Útil para migraciones, integraciones o sincronización selectiva.

### C. Configuración y verificación de la replicación

- Configuración del servidor principal:
  - Modificación de `postgresql.conf` (parámetros de WAL, `wal_level`, `max_wal_senders`, etc.).
  - Configuración de `pg_hba.conf` para permitir conexiones de replicación.
- Creación de copia base hacia la réplica (p. ej. `pg_basebackup`).
- Configuración del servidor réplica:
  - Parámetros de conexión al *primary*.
  - Archivos de señalización para modo *standby* (según versión).
- Verificación:
  - Uso de vistas como `pg_stat_replication` en el *primary*.
  - Comprobación de estado de recuperación en la réplica.

### D. Herramientas de alta disponibilidad y *failover*

1. **Patroni**
   - Orquestador de alta disponibilidad para PostgreSQL.
   - Failover automático.
   - Supervisión de nodos y elección de líder.

2. **pgBouncer**
   - Pool de conexiones ligero.
   - Ayuda a balancear carga y reducir overhead de conexión.

3. **Pgpool-II**
   - Middleware de *clustering*:
     - Failover.
     - Balanceo de carga.
     - Pooling de conexiones.

---

## VI. Migración y compatibilidad

### A. Desafíos generales de migración

1. **Características específicas de cada SGBD**
   - Diferencias en funciones, sintaxis, características de sistema.

2. **Tipos de datos**
   - Adaptación de tipos sin equivalencia directa:
     - Ejemplo: `uniqueidentifier` (SQL Server) → `UUID` (PostgreSQL).

3. **Procedimientos almacenados**
   - Distintos lenguajes:
     - SQL Server → T‑SQL.
     - PostgreSQL → PL/pgSQL.
   - Requiere reescritura de lógica y adaptación de funciones nativas.

### B. Migración desde SQL Server

1. **Herramientas**
   - SQL to PostgreSQL Migration Toolkit.
   - pgAdmin (importación vía CSV + ejecución de scripts).
   - AWS Schema Conversion Tool (SCT).
   - Data Migration Assistant (DMA, Microsoft).
   - DBConvert / DBSync (herramientas comerciales).

### C. Migración desde MySQL

1. **Desafíos**
   - Tipos de datos sin equivalencia directa:
     - `ENUM`, `TINYINT`, tipos `UNSIGNED`, etc.
   - Diferencias de sintaxis:
     - Creación de índices.
     - Funciones de agregación y operadores específicos.

2. **Herramientas**
   - `pgLoader` (código abierto).
   - AWS SCT.
   - MySQL Workbench (exportaciones SQL/CSV).
   - DBConvert / DBSync.

---

## VII. Manejo de imágenes en formato binario

### A. Creación de tabla para imágenes (`BYTEA`)

- Definición de tabla con:
  - Identificador (`SERIAL` / `UUID`).
  - `nombre_archivo`, `tipo_imagen`.
  - Campo binario `BYTEA` para los datos.
  - Metadatos: fecha de subida, tamaño, etc.

### B. Inserción de imágenes

- Lectura de archivos binarios desde sistema de archivos:
  - Uso de funciones como `pg_read_binary_file()` (en contextos permitidos).
  - Inserción desde aplicaciones (Python, Java, etc.) usando parámetros binarios.
- Verificación del almacenamiento binario y tamaño.

---

## VIII. Triggers, procedimientos almacenados y auditoría (práctica)

### A. Estructuras de auditoría

- Tablas de auditoría:
  - `auditoria_log`: registro general de operaciones (tabla, operación, usuario, timestamp, datos antes/después).
  - `auditoria_cambios`: detalle por columna (valores anteriores/nuevos).

### B. Índices en tablas de auditoría

- Creación de índices sobre:
  - `tabla_afectada`
  - `operacion`
  - `usuario_id`
  - `fecha_hora`
- Objetivo: mejorar el rendimiento de consultas de auditoría.

### C. Función general de auditoría (PL/pgSQL)

- Intento de función genérica para:
  - Registrar `INSERT`, `UPDATE`, `DELETE` en múltiples tablas.
  - Usar `TG_OP`, `TG_TABLE_NAME`, `NEW`, `OLD`.
- Manejo de:
  - Captura de usuario e IP de origen (según contexto).
  - Inserción en tablas de auditoría.
- Problemas detectados en la práctica:
  - Errores de referencia de campos.
  - Ajustes pendientes para próxima sesión.

### D. Procedimiento almacenado para insertar usuarios

1. **Lógica de negocio**
   - Verificar previamente si el email ya existe.
   - Evitar violar restricciones de unicidad.

2. **Inserción y retorno de ID**
   - `INSERT INTO ... RETURNING usuario_id`.
   - Devolver el ID del nuevo usuario para su uso por la aplicación.

---

## IX. Gestión de roles y privilegios (usuarios)

### A. Creación de roles

- Uso de `CREATE ROLE` / `CREATE USER`:
  - `admin_tienda` (superusuario).
  - `manager_tienda` (usuario estándar con contraseña, sin SUPERUSER).

### B. Asignación de privilegios

1. **Pruebas de acceso**
   - Intento de conexión con `manager_tienda`.
   - `SELECT` sobre tablas sin permisos → errores de “permission denied”.

2. **Permisos granulares (`GRANT`)**
   - Concesión de:
     - `CONNECT` en base de datos.
     - `USAGE` en esquema.
     - `SELECT` en tablas específicas o en todas las tablas del esquema.
   - Observación de problemas de configuración pendientes para refinar en sesiones posteriores.

---

## X. Realización y restauración de backups (práctica detallada)

### A. Backup mediante interfaz gráfica (pgAdmin)

- Uso de la opción **Backup**:
  - Clic derecho en base de datos → *Backup…*
  - Selección de:
    - Ruta y nombre de archivo.
    - Formato (`Plain`, `Custom`).
    - Objetos a incluir (schemas, tablas, funciones, etc.).

### B. Backup y restauración mediante línea de comandos

1. **Configuración de entorno**
   - Agregar el directorio `PostgreSQL/bin` al `PATH` del sistema para usar:
     - `pg_dump`, `pg_restore`, `psql`, etc.

2. **Ejecución de `pg_dump`**
   - Creación de archivo de respaldo (`.backup` o `.sql`) indicando:
     - Usuario (`-U`).
     - Host (`-h`).
     - Base de datos (`-d`).
     - Formato (`-Fc` para personalizado).
     - Archivo de salida (`-f`).

3. **Restauración con `pg_restore`**
   - Creación previa de base de datos destino (`createdb` / `CREATE DATABASE`).
   - Uso de `pg_restore` con:
     - `-U` (usuario).
     - `-d` (base de datos destino).
     - Ruta del archivo `.backup`.

4. **Verificación**
   - Revisión en pgAdmin:
     - Existencia de la nueva base de datos.
     - Presencia de esquemas, tablas e índices.
   - Ejecución de consultas simples para confirmar que los datos se restauraron correctamente.