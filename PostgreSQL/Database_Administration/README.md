# PostgreSQL Database Administration

[← Back to PostgreSQL Database Tutorial](../PostgreSQL%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions on PostgreSQL database administration using Navicat, including user management, backup and recovery, performance monitoring, maintenance tasks, and other administrator daily operations.

## Table of Contents
- [Database Administration Basics](#database-administration-basics)
- [User and Permission Management](#user-and-permission-management)
- [Backup and Recovery](#backup-and-recovery)
- [Database Maintenance Tasks](#database-maintenance-tasks)
- [Performance Monitoring and Optimization](#performance-monitoring-and-optimization)
- [Security Management](#security-management)
- [Advanced Administration Tasks](#advanced-administration-tasks)
- [Troubleshooting](#troubleshooting)

## Database Administration Basics

### Managing Database Objects

1. **Creating a New Database**:
   - Right-click on the **Databases** node in the Navicat object browser
   - Select **New Database**
   - Enter the database name and owner
   - Set encoding (recommended: UTF-8) and collation
   
   ![Create Database](images/create_database.png)

2. **Managing Schemas**:
   - Expand the database, right-click on the **Schemas** node
   - Select **New Schema**
   - Set the schema name and owner
   - Configure permissions (if needed)

3. **Creating and Managing Tables**:
   - Right-click on the **Tables** node in Navicat
   - Select **New Table**
   - Use the table designer to define columns, data types, constraints, and indexes
   - Or use SQL Editor to enter CREATE TABLE statements

4. **Index Management**:
   - In the table designer, switch to the **Indexes** tab
   - Select **Add** to create a new index
   - Set the index type (B-tree, Hash, GiST, GIN, etc.)
   - Select index columns and sort order

### Server Settings

1. **Viewing Server Settings**:
   - Right-click on the PostgreSQL connection
   - Select **Server Status**
   - View parameter settings, process list, and other server information
   
   ![Server Settings](images/server_settings.png)

2. **Modifying PostgreSQL Configuration**:
   - Edit the `postgresql.conf` file directly
   - Use the `ALTER SYSTEM` command in SQL queries:
   ```sql
   -- Set maximum connections
   ALTER SYSTEM SET max_connections = '100';
   
   -- Set shared buffer size
   ALTER SYSTEM SET shared_buffers = '1GB';
   
   -- Reload configuration
   SELECT pg_reload_conf();
   ```

### Basic Administration Queries

1. **View Server Version**:
   ```sql
   SELECT version();
   ```

2. **View Current Database Size**:
   ```sql
   SELECT pg_size_pretty(pg_database_size(current_database()));
   ```

3. **View Table Sizes**:
   ```sql
   SELECT 
       tablename, 
       pg_size_pretty(pg_table_size(tablename)) AS table_size,
       pg_size_pretty(pg_indexes_size(tablename)) AS index_size,
       pg_size_pretty(pg_total_relation_size(tablename)) AS total_size
   FROM 
       pg_tables
   WHERE 
       schemaname = 'public'
   ORDER BY 
       pg_total_relation_size(tablename) DESC;
   ```

4. **List Active Connections**:
   ```sql
   SELECT 
       pid, 
       usename, 
       application_name,
       client_addr, 
       backend_start,
       state, 
       query
   FROM 
       pg_stat_activity
   WHERE 
       state != 'idle'
   ORDER BY 
       backend_start;
   ```

## User and Permission Management

### Managing Users with Navicat

1. **Creating a New User**:
   - Expand the PostgreSQL connection
   - Right-click on the **Users** node
   - Select **New User**
   - Set username, password, and attributes
   
   ![Create User](images/create_user.png)

2. **User Attributes**:
   - **Superuser**: Whether the user has all privileges
   - **Create DB**: Whether the user can create new databases
   - **Create Role**: Whether the user can create new roles
   - **Login**: Whether the user can log in to the database
   - **Valid Until**: Set password expiration date

3. **Editing Existing Users**:
   - Right-click on the username
   - Select **Edit User**
   - Modify password, privileges, or other attributes

### Role and Group Management

1. **Creating Roles**:
   
   In PostgreSQL, roles can be users (can login) or groups (cannot login but can contain other roles):
   
   ```sql
   -- Create a user role with login capability
   CREATE ROLE app_user WITH LOGIN PASSWORD 'secure_password';
   
   -- Create a group role (cannot login)
   CREATE ROLE developers NOLOGIN;
   
   -- Add a user to a group
   GRANT developers TO app_user;
   ```

2. **Managing Group Members**:
   - In Navicat, right-click on a role
   - Select **Edit Role**
   - In the **Members** tab, add or remove members

### Permission Management

1. **Granting Object Permissions**:
   ```sql
   -- Grant table permissions
   GRANT SELECT, INSERT, UPDATE ON table_name TO role_name;
   
   -- Grant all table permissions
   GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO role_name;
   
   -- Grant sequence permissions
   GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO role_name;
   ```

2. **Managing Permissions using Navicat**:
   - Right-click on a database object (like a table)
   - Select **Manage Privileges**
   - Set permissions for specific users or roles
   
   ![Manage Privileges](images/manage_privileges.png)

3. **Schema Permissions**:
   ```sql
   -- Grant schema usage permission
   GRANT USAGE ON SCHEMA schema_name TO role_name;
   
   -- Grant object creation permission
   GRANT CREATE ON SCHEMA schema_name TO role_name;
   
   -- Set default permissions for future objects
   ALTER DEFAULT PRIVILEGES IN SCHEMA schema_name
   GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO role_name;
   ```

4. **Revoking Permissions**:
   ```sql
   -- Revoke specific permissions
   REVOKE INSERT, UPDATE ON table_name FROM role_name;
   
   -- Revoke all permissions
   REVOKE ALL PRIVILEGES ON table_name FROM role_name;
   ```

### Client Authentication Configuration

1. **Editing pg_hba.conf**:

   PostgreSQL controls client authentication through the `pg_hba.conf` file:
   
   ```
   # TYPE  DATABASE        USER            ADDRESS                 METHOD
   local   all             postgres                                peer
   host    all             all             127.0.0.1/32            md5
   host    all             all             ::1/128                 md5
   host    production      app_user        192.168.1.0/24          md5
   ```

2. **Authentication Methods**:
   - **trust**: Connect without password
   - **md5**: Require MD5 encrypted password
   - **password**: Plaintext password (not recommended)
   - **peer**: Use operating system username (local connections only)
   - **ident**: Use operating system username (remote connections)
   - **scram-sha-256**: SCRAM-SHA-256 encryption (PostgreSQL 10+)

3. **Reload Configuration**:
   ```sql
   SELECT pg_reload_conf();
   ```
   Or using command line:
   ```bash
   wsl pg_ctl reload -D /path/to/data/directory
   ```

## Backup and Recovery

### Using Navicat for Backup

1. **Backing up a Single Database**:
   - Right-click on the database name
   - Select **Dump SQL File**
   - Configure backup options (structure, data, or both)
   - Specify output file
   
   ![Backup Database](images/dump_database.png)

2. **Backing up Specific Objects**:
   - Select specific tables, views, or other objects
   - Right-click and select **Dump SQL File**
   - Configure backup options

3. **Backup Options**:
   - **Structure Only**: Only export CREATE statements
   - **Data Only**: Only export INSERT statements
   - **Structure and Data**: Export both
   - **Use Transaction**: Execute recovery in a transaction
   - **IF NOT EXISTS**: Add IF NOT EXISTS clauses
   - **Use DROP Statements**: Include DROP statements before creation

### Using Command Line Tools for Backup

1. **Using pg_dump**:
   ```bash
   # Backup a single database as SQL file
   wsl pg_dump -h localhost -U postgres -F p -f database_backup.sql database_name
   
   # Backup in custom format (for restoration)
   wsl pg_dump -h localhost -U postgres -F c -f database_backup.dump database_name
   
   # Backup structure only
   wsl pg_dump -h localhost -U postgres -F p -s -f structure_backup.sql database_name
   
   # Backup data only
   wsl pg_dump -h localhost -U postgres -F p -a -f data_backup.sql database_name
   ```

2. **Backing up All Databases (pg_dumpall)**:
   ```bash
   wsl pg_dumpall -h localhost -U postgres -f all_databases.sql
   ```

3. **Schedule Regular Backups**:
   - Use Navicat's **Scheduled Tasks** feature to set up automatic backups
   - Configure backup frequency, retention policy, and notification settings

### Restoring Databases

1. **Restoring with Navicat**:
   - Create a new database (if needed)
   - Right-click on the database
   - Select **Execute SQL File**
   - Select the previously created backup file

2. **Restoring from Command Line**:
   ```bash
   # Restore from SQL file
   wsl psql -h localhost -U postgres -d database_name -f database_backup.sql
   
   # Restore from custom format backup
   wsl pg_restore -h localhost -U postgres -d database_name -F c database_backup.dump
   
   # Parallel restore (speeds up large database restoration)
   wsl pg_restore -h localhost -U postgres -d database_name -F c -j 4 database_backup.dump
   ```

3. **pg_restore Options**:
   - `-j num`: Number of threads for parallel restore
   - `-c`: Clean (drop) objects before recreating
   - `-C`: Create database, then restore
   - `--no-owner`: Do not set ownership of objects
   - `--role=rolename`: Execute restore as specified role

### Point-in-Time Recovery (PITR)

1. **Configure Continuous Archiving**:
   
   Edit postgresql.conf:
   ```
   wal_level = replica
   archive_mode = on
   archive_command = 'cp %p /path/to/archive/%f'
   ```

2. **Create Base Backup**:
   ```bash
   wsl pg_basebackup -D /path/to/backup -F tar -z -P
   ```

3. **Perform Recovery**:
   
   Create recovery.conf file:
   ```
   restore_command = 'cp /path/to/archive/%f %p'
   recovery_target_time = '2023-06-15 14:30:00'
   ```
   
   Then start the PostgreSQL server to begin recovery.

## Database Maintenance Tasks

### VACUUM Operations

1. **Why VACUUM is Needed**:
   - PostgreSQL uses MVCC (Multi-Version Concurrency Control)
   - Updates and deletes don't immediately clear old version data
   - VACUUM reclaims space occupied by dead tuples

2. **Performing VACUUM using Navicat**:
   - Right-click on a database or table
   - Select **Maintenance** > **VACUUM**
   - Choose VACUUM type
   
   ![VACUUM Operation](images/vacuum_operation.png)

3. **VACUUM Types**:
   ```sql
   -- Basic VACUUM (doesn't block operations)
   VACUUM table_name;
   
   -- Full VACUUM (requires exclusive lock, reclaims more space)
   VACUUM FULL table_name;
   
   -- VACUUM with analysis (updates statistics)
   VACUUM ANALYZE table_name;
   ```

4. **Autovacuum Configuration**:
   
   Edit parameters in postgresql.conf:
   ```
   autovacuum = on
   autovacuum_vacuum_threshold = 50
   autovacuum_analyze_threshold = 50
   autovacuum_vacuum_scale_factor = 0.2
   autovacuum_analyze_scale_factor = 0.1
   ```

### Analyzing Tables

1. **Update Statistics**:
   - Right-click on a table
   - Select **Maintenance** > **Analyze**

2. **SQL Commands**:
   ```sql
   -- Analyze a single table
   ANALYZE table_name;
   
   -- Analyze the entire database
   ANALYZE;
   ```

3. **Automatic Analysis**:
   
   PostgreSQL's autovacuum process automatically analyzes tables. Adjust with these parameters:
   ```
   autovacuum_analyze_threshold = 50
   autovacuum_analyze_scale_factor = 0.1
   ```

### Rebuilding Indexes

1. **Rebuild Indexes using Navicat**:
   - Right-click on a table or index
   - Select **Maintenance** > **Rebuild Index**

2. **SQL Commands**:
   ```sql
   -- Rebuild a single index
   REINDEX INDEX index_name;
   
   -- Rebuild all indexes on a table
   REINDEX TABLE table_name;
   
   -- Rebuild all indexes in a database
   REINDEX DATABASE database_name;
   ```

3. **Concurrent Index Rebuilding (PostgreSQL 12+)**:
   ```sql
   -- Index rebuild without blocking write operations
   REINDEX INDEX CONCURRENTLY index_name;
   ```

### Database Cleanup

1. **Clean Up Unused Objects**:
   - Delete unused tables, views, and indexes
   - Remove redundant or outdated data

2. **Find Unused Indexes**:
   ```sql
   SELECT
       s.schemaname,
       s.relname AS tablename,
       s.indexrelname AS indexname,
       pg_relation_size(s.indexrelid) AS index_size,
       s.idx_scan
   FROM
       pg_stat_user_indexes s
   JOIN
       pg_index i ON s.indexrelid = i.indexrelid
   WHERE
       s.idx_scan = 0 AND
       NOT i.indisprimary AND
       NOT i.indisunique
   ORDER BY
       pg_relation_size(s.indexrelid) DESC;
   ```

3. **Find Unused Tables**:
   ```sql
   SELECT
       schemaname,
       relname,
       last_vacuum,
       last_autovacuum,
       last_analyze,
       last_autoanalyze
   FROM
       pg_stat_user_tables
   WHERE
       (n_tup_ins + n_tup_upd + n_tup_del) = 0
   ORDER BY
       pg_total_relation_size(schemaname || '.' || relname) DESC;
   ```

## Performance Monitoring and Optimization

### Monitoring Performance with Navicat

1. **Server Monitoring**:
   - Right-click on the PostgreSQL connection
   - Select **Server Monitor**
   - View CPU, memory, disk and other resource usage
   
   ![Server Monitor](images/server_monitor.png)

2. **Process List**:
   - Right-click on the PostgreSQL connection
   - Select **Process List**
   - View all active connections and queries

3. **Status Variables**:
   - Right-click on the PostgreSQL connection
   - Select **Server Status**
   - View status variables, cache hit ratios, locks, and other information

### Query Performance Analysis

1. **Using EXPLAIN**:
   ```sql
   -- View execution plan
   EXPLAIN SELECT * FROM large_table WHERE column1 = 'value';
   
   -- View actual execution statistics
   EXPLAIN ANALYZE SELECT * FROM large_table WHERE column1 = 'value';
   ```

2. **Interpreting Execution Plans**:
   - **Seq Scan**: Full table scan (may need an index)
   - **Index Scan**: Uses an index to find matching rows
   - **Bitmap Heap Scan**: Two-phase scan, first building a bitmap of matching rows
   - **Hash Join**: Uses a hash table to perform joins
   - **Nested Loop**: Nested loop join
   - **Cost**: Estimated cost (startup cost and total cost)
   - **Rows**: Estimated row count
   - **Actual time**: Actual execution time (milliseconds)

3. **Identifying Slow Queries**:
   
   Edit postgresql.conf to enable slow query logging:
   ```
   log_min_duration_statement = 500   # Log queries taking more than 500ms
   ```

4. **Using pg_stat_statements Extension**:
   ```sql
   -- Create extension
   CREATE EXTENSION pg_stat_statements;
   
   -- Query most time-consuming SQL
   SELECT 
       query,
       calls,
       total_exec_time / 1000 AS total_time_s,
       total_exec_time / calls / 1000 AS avg_time_s,
       rows / calls AS avg_rows,
       100 * shared_blks_hit / nullif(shared_blks_hit + shared_blks_read, 0) AS hit_percent
   FROM 
       pg_stat_statements
   ORDER BY 
       total_exec_time DESC
   LIMIT 10;
   ```

### Query Performance Optimization

1. **Index Optimization**:
   - Create indexes for columns frequently used in WHERE clauses, JOIN conditions
   - Consider creating composite indexes, partial indexes, or expression indexes
   - Remove unused indexes

2. **Creating Indexes**:
   ```sql
   -- Create B-tree index (default type)
   CREATE INDEX idx_table_column ON table_name(column_name);
   
   -- Create composite index
   CREATE INDEX idx_table_multi ON table_name(column1, column2);
   
   -- Create partial index
   CREATE INDEX idx_table_partial ON table_name(column_name) WHERE condition;
   
   -- Create expression index
   CREATE INDEX idx_table_expr ON table_name(lower(column_name));
   ```

3. **Table Structure Optimization**:
   - Choose correct column data types
   - Consider table partitioning (for large tables)
   - Use proper constraints (primary keys, foreign keys, unique constraints)

4. **Table Partitioning**:
   ```sql
   -- Create partitioned table (by range)
   CREATE TABLE measurements (
       logdate         date not null,
       peaktemp        int,
       unitsales       int
   ) PARTITION BY RANGE (logdate);
   
   -- Create partitions
   CREATE TABLE measurements_y2021 PARTITION OF measurements
       FOR VALUES FROM ('2021-01-01') TO ('2022-01-01');
   
   CREATE TABLE measurements_y2022 PARTITION OF measurements
       FOR VALUES FROM ('2022-01-01') TO ('2023-01-01');
   ```

### Tuning PostgreSQL Configuration

Here are some key performance-related configuration parameters:

1. **Memory Configuration**:
   ```
   shared_buffers = 25% of RAM      # Recommend 25% of system memory
   work_mem = 32MB                  # Memory for internal sort and hash operations
   maintenance_work_mem = 256MB     # Memory for maintenance operations (like VACUUM)
   effective_cache_size = 75% of RAM  # Estimated size of OS cache
   ```

2. **Query Planner Configuration**:
   ```
   random_page_cost = 1.1          # Lower for SSD storage
   seq_page_cost = 1.0             # Cost of sequential scan
   effective_io_concurrency = 200  # Higher value for SSDs
   ```

3. **WAL and Checkpoint Configuration**:
   ```
   checkpoint_timeout = 15min      # Maximum time between checkpoints
   checkpoint_completion_target = 0.9  # Spread checkpoint completion
   max_wal_size = 2GB              # Maximum WAL file size
   min_wal_size = 1GB              # Minimum WAL file retention
   ```

4. **Parallel Query Configuration**:
   ```
   max_worker_processes = 8        # Maximum number of background worker processes
   max_parallel_workers_per_gather = 4  # Workers for parallel query
   max_parallel_workers = 8        # Maximum parallel workers
   ```

## Security Management

### Encryption and Security Configuration

1. **Enable SSL Connections**:
   
   Edit postgresql.conf:
   ```
   ssl = on
   ssl_cert_file = 'server.crt'
   ssl_key_file = 'server.key'
   ```

2. **Password Policy**:
   - Use strong passwords
   - Change passwords regularly
   - Enable password expiration

3. **Limit Network Access**:
   
   Edit postgresql.conf:
   ```
   listen_addresses = 'localhost, 192.168.1.100'  # Limit listening addresses
   ```
   
   Edit pg_hba.conf to limit IP access:
   ```
   host    all     all     192.168.1.0/24    md5
   ```

### Auditing

1. **Enable SQL Logging**:
   
   Edit postgresql.conf:
   ```
   log_statement = 'mod'        # Log all modification statements
   log_min_duration_statement = 1000  # Log queries taking longer than 1 second
   log_connections = on         # Log connection attempts
   log_disconnections = on      # Log session terminations
   ```

2. **Using Audit Extensions**:
   - pgAudit: Provides detailed session and object audit logging
   
   ```sql
   -- Install pgAudit extension
   CREATE EXTENSION pgaudit;
   
   -- Configure pgAudit
   ALTER SYSTEM SET pgaudit.log = 'write, ddl';
   SELECT pg_reload_conf();
   ```

3. **Monitoring Who Did What**:
   - Check log files
   - Use triggers to record table modifications
   
   ```sql
   -- Create audit table
   CREATE TABLE audit_log (
       id SERIAL PRIMARY KEY,
       table_name TEXT NOT NULL,
       operation TEXT NOT NULL,
       old_data JSONB,
       new_data JSONB,
       changed_by TEXT NOT NULL,
       changed_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP
   );
   
   -- Create audit function
   CREATE OR REPLACE FUNCTION audit_trigger_func()
   RETURNS TRIGGER AS $$
   BEGIN
       IF (TG_OP = 'DELETE') THEN
           INSERT INTO audit_log(table_name, operation, old_data, changed_by)
           VALUES (TG_TABLE_NAME, TG_OP, row_to_json(OLD), current_user);
           RETURN OLD;
       ELSIF (TG_OP = 'UPDATE') THEN
           INSERT INTO audit_log(table_name, operation, old_data, new_data, changed_by)
           VALUES (TG_TABLE_NAME, TG_OP, row_to_json(OLD), row_to_json(NEW), current_user);
           RETURN NEW;
       ELSIF (TG_OP = 'INSERT') THEN
           INSERT INTO audit_log(table_name, operation, new_data, changed_by)
           VALUES (TG_TABLE_NAME, TG_OP, row_to_json(NEW), current_user);
           RETURN NEW;
       END IF;
       RETURN NULL;
   END;
   $$ LANGUAGE plpgsql;
   
   -- Add audit trigger to a table
   CREATE TRIGGER users_audit
   AFTER INSERT OR UPDATE OR DELETE ON users
   FOR EACH ROW EXECUTE FUNCTION audit_trigger_func();
   ```

## Advanced Administration Tasks

### Replication and High Availability

1. **Setting Up Streaming Replication**:
   
   On the primary server in postgresql.conf:
   ```
   wal_level = replica
   max_wal_senders = 10
   wal_keep_segments = 32
   ```
   
   On the primary server in pg_hba.conf:
   ```
   host replication replicator 192.168.1.0/24 md5
   ```
   
   Setup recovery.conf on the standby server:
   ```
   standby_mode = 'on'
   primary_conninfo = 'host=192.168.1.100 port=5432 user=replicator password=password'
   trigger_file = '/tmp/pg_failover_trigger'
   ```

2. **Using pgBouncer Connection Pooling**:
   
   pgbouncer.ini configuration:
   ```
   [databases]
   mydb = host=localhost port=5432 dbname=mydb
   
   [pgbouncer]
   listen_addr = *
   listen_port = 6432
   auth_type = md5
   auth_file = userlist.txt
   pool_mode = transaction
   max_client_conn = 1000
   default_pool_size = 20
   ```

3. **Load Balancing**:
   - Use HAProxy or Pgpool-II for connection load balancing
   - Set up read-write splitting (primary for write operations, standby for read operations)

### Large Database Management

1. **Table Partitioning**:
   - Partition by date, range, or list of values
   - Use declarative partitioning or inheritance

2. **Managing Large Tables**:
   - Use parallel queries
   - Regularly reorganize and clean up
   - Consider archiving old data

3. **Upgrade Strategies**:
   - Use pg_upgrade for in-place upgrades
   - Use logical replication for zero-downtime upgrades
   - Maintain compatibility and test applications

## Troubleshooting

### Common Issues and Solutions

1. **Connection Problems**:
   - Check pg_hba.conf settings
   - Verify listening address and port configuration
   - Confirm the server is running
   - Check network connectivity and firewall settings

2. **Performance Issues**:
   - Find long-running queries
   - Check for missing or inefficient indexes
   - Look for table bloat
   - Analyze locks and blocking

3. **Disk Space Issues**:
   - Use VACUUM FULL to reclaim space
   - Clean up old WAL files
   - Check temporary file usage
   - Consider adding storage or partitioning data

### Identifying and Resolving Lock Issues

1. **Finding Locks**:
   ```sql
   SELECT 
       a.datname,
       l.relation::regclass,
       l.transactionid,
       l.mode,
       l.granted,
       a.usename,
       a.query,
       a.query_start,
       age(now(), a.query_start) AS "age",
       a.pid
   FROM 
       pg_stat_activity a
   JOIN 
       pg_locks l ON l.pid = a.pid
   WHERE 
       NOT l.granted
   ORDER BY 
       a.query_start;
   ```

2. **Finding Blocking Queries**:
   ```sql
   SELECT 
       blocked_activity.pid AS blocked_pid,
       blocked_activity.usename AS blocked_user,
       blocked_activity.query AS blocked_query,
       blocking_activity.pid AS blocking_pid,
       blocking_activity.usename AS blocking_user,
       blocking_activity.query AS blocking_query,
       age(now(), blocked_activity.query_start) AS blocked_duration
   FROM 
       pg_catalog.pg_locks blocked_locks
   JOIN 
       pg_catalog.pg_stat_activity blocked_activity ON blocked_activity.pid = blocked_locks.pid
   JOIN 
       pg_catalog.pg_locks blocking_locks 
       ON blocking_locks.locktype = blocked_locks.locktype
       AND blocking_locks.database IS NOT DISTINCT FROM blocked_locks.database
       AND blocking_locks.relation IS NOT DISTINCT FROM blocked_locks.relation
       AND blocking_locks.page IS NOT DISTINCT FROM blocked_locks.page
       AND blocking_locks.tuple IS NOT DISTINCT FROM blocked_locks.tuple
       AND blocking_locks.virtualxid IS NOT DISTINCT FROM blocked_locks.virtualxid
       AND blocking_locks.transactionid IS NOT DISTINCT FROM blocked_locks.transactionid
       AND blocking_locks.classid IS NOT DISTINCT FROM blocked_locks.classid
       AND blocking_locks.objid IS NOT DISTINCT FROM blocked_locks.objid
       AND blocking_locks.objsubid IS NOT DISTINCT FROM blocked_locks.objsubid
       AND blocking_locks.pid != blocked_locks.pid
   JOIN 
       pg_catalog.pg_stat_activity blocking_activity ON blocking_activity.pid = blocking_locks.pid
   WHERE 
       NOT blocked_locks.granted;
   ```

3. **Canceling or Terminating Processes**:
   ```sql
   -- Cancel a query (allows transaction cleanup)
   SELECT pg_cancel_backend(pid);
   
   -- Terminate a process (forceful interruption, may require recovery)
   SELECT pg_terminate_backend(pid);
   ```

### Using Logs for Troubleshooting

1. **Configuring Logging**:
   
   Edit postgresql.conf:
   ```
   log_destination = 'stderr'
   logging_collector = on
   log_directory = 'pg_log'
   log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'
   log_min_messages = warning
   log_min_error_statement = error
   log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a '
   ```

2. **Viewing Logs in Navicat**:
   - Right-click on the PostgreSQL connection
   - Select **Server Log**

3. **Analyzing Common Problems in Logs**:
   - Connection rejections
   - Permission errors
   - Syntax errors
   - Deadlocks
   - Out of memory errors

### Data Recovery and Repair

1. **Recovering Accidentally Deleted Data**:
   - Use PITR (Point-in-Time Recovery)
   - Restore from recent backup
   - Consider using log mining tools

2. **Handling Data Corruption**:
   ```sql
   -- Run database consistency check
   SELECT * FROM pg_consistency_check('database_name');
   
   -- Fix corrupted indexes
   REINDEX TABLE table_name;
   
   -- Check table corruption
   SELECT * FROM pgstattuple('table_name');
   ```

3. **Repairing System Catalogs**:
   - Use pg_resetwal to reset database control information
   - Note: Only use when recovery is not possible

## Next Steps

- [PostgreSQL Cluster Deployment](../postgresql_cluster/README.md)
- [PostgreSQL Application Integration](../postgresql_integration/README.md) 