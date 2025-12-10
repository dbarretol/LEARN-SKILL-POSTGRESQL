A continuación, se presenta un esquema de títulos y subtítulos (índice) basado en los temas discutidos en los documentos proporcionados:

**I. Introducción y Contexto del Curso**
A. La Cuarta Sesión del Curso,
B. Inicio de la Sesión y Conexión al Servidor,,

**II. Administración y Herramientas Avanzadas: Copias de Seguridad y Restauración**
A. Copias de Seguridad (Backups)
1. Tipos de Métodos de Backup
2. PG Dump: Genera un script de la base de datos, incluyendo registros.
3. PG Base Backup: Genera un archivo `.backup`.
4. Comparación y Uso Recomendado
    a. PG Dump: Respaldo individual, alta portabilidad, recomendado para migraciones y respaldos ligeros.
    b. PG Base Backup: Respaldo de todo el *cluster*, portabilidad limitada a versiones iguales, recomendado para *Disaster Recovery* o *standby*.
B. Restauración de Bases de Datos
1. Herramientas de Restauración: PSQL y PG Restore (para archivos PG Dump).
2. Restauración de PG Base Backup: Copiar y arrancar el motor (PE).
3. Tipos de Datos de Respaldo: Archivos que terminan en `.SQL` (script) o `.PG Dump FC`, y físico (PG Base Backup),.
4. Métodos de Ejecución: PG Restore o PG Admin para `.backup`, CMD para el físico.

**III. Optimización de Bases de Datos**
A. Particionado de Tablas
1. Concepto: Divide tablas grandes en subtablas pequeñas.
2. Beneficios: Mejora la gestión, acelera consultas, facilita la eliminación de datos antiguos, y optimiza índices y análisis.
3. Tipos de Particionado,
    a. Range (Rango): Según rango de valores (fechas, números).
    b. List (Lista).
    c. Hash: Según valor hash en una columna para distribución uniforme.
    d. Composite: Combinación de los anteriores (multinivel).
B. Uso de Tablespaces
1. Concepto: Lugar en el sistema donde se almacenan las tablas (similar a Oracle).
2. Beneficios: Optimiza el rendimiento (almacenando tablas e índices en discos separados) y distribuye la carga.
3. Gestión del Espacio: Facilita la administración de sistemas de archivos.

**IV. Monitoreo y Mantenimiento**
A. Comandos de Mantenimiento
1. Objetivo: Garantizar el buen rendimiento y mantener la base de datos actualizada (refrescada),.
2. Comandos Principales: Vacuum y Analyze.
3. Tipos de Vacuum:
    a. Vacuum: Limpia tuplas muertas y libera espacio.
    b. Vacuum Full: Limpia y compacta tablas, liberando espacio a nivel del sistema de archivos.
    c. Vacuum Analyze: Limpia y actualiza las estadísticas de la base de datos.
4. Analyze: Solo actualiza estadísticas de la base de datos.
B. Herramientas de Monitoreo
1. Objetivo: Analizar el rendimiento, detectar y optimizar consultas lentas,.
2. PG Stat Activity: Monitoreo en tiempo real de conexiones activas e información sobre sesiones activas,.
3. PG Stat Statement: Estadística de consultas ejecutadas y agregadas a largo plazo,.

**V. Replicación y Alta Disponibilidad**,
A. Concepto de Replicación: Proceso de copiar datos a otra base de datos, a menudo como un espejo.
B. Tipos de Replicación,,
1. Streaming Replication (Tiempo Real): Replica cambios en tiempo real usando registros WAL.
    a. Ventajas: Alta disponibilidad, bajo impacto en rendimiento y desempeño de lectura distribuida.
2. Replicación Síncrona: La base de datos principal espera la confirmación de al menos una réplica antes de completar la transacción.
3. Replicación Asíncrona: La base de datos principal no espera la confirmación de las réplicas.
4. Replicación Lógica: Permite replicar solo una parte de la base de datos (tablas, esquemas).
C. Configuración y Verificación: Involucra la configuración del servidor principal (modificar `postgres.conf` y `pg_hda.conf`), el reinicio, la realización del respaldo entre bases, y la configuración del servidor de réplica (`recovery.conf` o `stamping`),.
D. Herramientas de Alta Disponibilidad a Fallos (Failover),
1. Patroni: Gestor de alta disponibilidad para fallos automáticos y supervisión de nodos.
2. PG Bouncer: Pool de conexiones para balancear la carga.
3. PG Pool 2: Herramienta de *clustering* que soporta failover automático y balanceo de carga.

**VI. Migración y Compatibilidad**,
A. Desafíos de Migración (General)
1. Características Específicas: Cada gestor de base de datos tiene características propias que requieren adaptación.
2. Tipos de Datos: Necesidad de adaptar tipos de datos comunes sin correspondencia directa (ej. `uniqueidentifier` de SQL Server).
3. Procedimientos Almacenados: Diferencias de lenguaje (Transact-SQL vs. PL/pgSQL), requiriendo reescritura.
B. Migración desde SQL Server
1. Herramientas: SQL to PostgreSQL Migration Toolkit, PG Admin (para importar CSV), AWS Schema Conversion Tool (SCT), Data Migration Assistant, y DB Conver.
C. Migración desde MySQL,
1. Desafíos: Incompatibilidad de tipos de datos como `Enum` o `TinyInt`.
2. Sintaxis: Diferencias en la creación de índices y funciones agregadas.
3. Herramientas: PG Loader (código abierto), AWS SCT, MySQL Workbench y DB Conver,.

**VII. Manejo de Imágenes en Formato Binario**
A. Creación de Tabla para Almacenar Imágenes Binarias (`bytea`),
B. Proceso de Inserción de Imágenes: Uso de la función `PG Read Binary` para leer el archivo binario y almacenarlo en la base de datos,.

**VIII. Triggers, Procedimientos Almacenados y Auditoría (Práctica)**
A. Creación de Estructuras para Auditoría (Auditoría Log y Auditoría Cambios),
B. Creación de Índices para Mejorar el Rendimiento de Consultas en Tablas de Auditoría,,
C. Desarrollo de Función General de Auditoría: Intento de crear una función `PL/pgSQL` para manejar automáticamente operaciones (Insert, Update, Delete) en múltiples tablas,,.
D. Procedimiento Almacenado para Insertar Usuarios
1. Lógica: Verificar existencia de email único antes de la inserción.
2. Inserción de datos y retorno del ID de usuario insertado,.

**IX. Gestión de Roles y Privilegios (Usuarios)**
A. Creación de Roles: Uso de `CREATE ROLE` para crear usuarios como `admin tienda` (superusuario) y `manager tienda`,.
B. Asignación de Privilegios
1. Pruebas de Acceso: Demostración de permiso denegado para un usuario sin privilegios explícitos.
2. Asignación de Permisos Granulares: Uso de `GRANT` para otorgar privilegios específicos (SELECT) a tablas, esquemas o bases de datos,.

**X. Realización y Restauración de Backups (Práctica Detallada)**
A. Backup mediante Interfaz Gráfica (PG Admin): Uso de la opción *Backup* en el menú contextual de la base de datos, configurando formato y objetos a respaldar,.
B. Backup y Restauración mediante Línea de Comandos (CMD)
1. Configuración de Entorno: Agregar la ruta de `PostgreSQL/bin` a la variable PATH para ejecutar comandos desde CMD,.
2. Ejecución de PG Dump: Uso de la herramienta `PG Dump` para crear un archivo `.backup`,,.
3. Restauración: Creación de una nueva base de datos (`CREATE DB`) y uso de `PG Restore` para cargar el backup,,,.
4. Verificación: Confirmación de que las tablas se han restaurado correctamente.