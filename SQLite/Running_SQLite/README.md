# Running SQLite Databases

[← Back to SQLite Database Tutorial](../SQLite%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions on running and managing SQLite databases, including working with the SQLite command-line interface, executing common operations, and understanding SQLite's architecture.

## Table of Contents
- [Understanding SQLite Architecture](#understanding-sqlite-architecture)
- [SQLite Command-Line Interface](#sqlite-command-line-interface)
- [Creating and Opening Databases](#creating-and-opening-databases)
- [Basic SQLite Commands](#basic-sqlite-commands)
- [SQLite Configuration](#sqlite-configuration)
- [Maintaining SQLite Databases](#maintaining-sqlite-databases)
- [SQLite in Various Environments](#sqlite-in-various-environments)
- [Troubleshooting](#troubleshooting)

## Understanding SQLite Architecture

Before working with SQLite, it's helpful to understand its unique architecture:

### Serverless Design

Unlike traditional client-server database systems (MySQL, PostgreSQL, etc.), SQLite:
- Does not require a separate server process
- Integrates directly into the application
- Reads/writes directly to disk files
- Does not require installation or setup

### Single File Structure

A SQLite database is contained in a single cross-platform disk file:
- The entire database (tables, indexes, triggers, views) is in one file
- File format is stable, cross-platform, and backward compatible
- Can be copied, moved, or shared between systems easily

### Cross-Platform Compatibility

SQLite databases can be used across different operating systems:
- The same database file works on Windows, macOS, Linux, etc.
- Byte order is normalized within the file format
- No endianness issues between platforms

### Size Limitations

While SQLite is "lite", it can handle substantial data:
- Maximum database size: 140 terabytes (theoretical)
- Maximum row size: 1 gigabyte
- Practical limits depend on operating system and storage
- Optimal for databases up to several gigabytes

### File Locking Mechanism

SQLite uses file-based locking for concurrency control:
- Multiple readers allowed simultaneously
- Only one writer allowed at any time
- Writers receive exclusive access to the database
- Lock escalation from shared to exclusive as needed

## SQLite Command-Line Interface

The SQLite command-line interface (CLI) is a powerful tool for managing SQLite databases.

### Accessing the Command Line

**On Windows (using WSL):**
```bash
wsl sqlite3
```

**On macOS/Linux:**
```bash
sqlite3
```

### Starting SQLite with a Database

To open or create a database:
```bash
wsl sqlite3 mydatabase.db
```

This will:
- Create a new database file if it doesn't exist
- Open the existing database if it does exist
- Present you with the `sqlite>` prompt

### SQLite Shell Commands

The SQLite shell has special "dot" commands that begin with a period:

```
.help           Show help text with all commands
.open FILENAME  Open a database file
.databases      List attached databases
.tables         List tables in the database
.schema [TABLE] Show the CREATE statements for tables
.mode MODE      Set output mode (column, csv, html, etc.)
.headers on|off Turn display of headers on or off
.quit           Exit the SQLite command-line
```

### Customizing the Shell Environment

For better readability:
```
sqlite> .mode column
sqlite> .headers on
sqlite> .nullvalue NULL
```

For saving results:
```
sqlite> .output results.txt
sqlite> SELECT * FROM mytable;
sqlite> .output stdout  -- Return to console output
```

## Creating and Opening Databases

### Creating a New Database

**Using the command line:**
```bash
wsl sqlite3 new_database.db
```

**Within SQLite shell:**
```
sqlite> .open new_database.db
```

### In-Memory Databases

For temporary operations, create an in-memory database:
```bash
wsl sqlite3 :memory:
```

In-memory databases are useful for:
- Testing
- Temporary data processing
- Performance-critical applications

### Attaching Multiple Databases

SQLite can work with multiple databases simultaneously:

```sql
-- Attach another database
ATTACH DATABASE 'another.db' AS another;

-- Query from both databases
SELECT * FROM main.users, another.products;

-- Detach when finished
DETACH DATABASE another;
```

## Basic SQLite Commands

### Table Operations

**Creating tables:**
```sql
CREATE TABLE users (
    user_id INTEGER PRIMARY KEY,
    username TEXT NOT NULL UNIQUE,
    email TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Altering tables:**
```sql
-- Add a column
ALTER TABLE users ADD COLUMN last_login TIMESTAMP;

-- Rename a table
ALTER TABLE users RENAME TO app_users;
```

**Dropping tables:**
```sql
DROP TABLE IF EXISTS temp_data;
```

### Data Manipulation

**Inserting data:**
```sql
-- Single row
INSERT INTO users (username, email) VALUES ('john_doe', 'john@example.com');

-- Multiple rows
INSERT INTO users (username, email) VALUES 
    ('jane_smith', 'jane@example.com'),
    ('bob_jones', 'bob@example.com');
```

**Updating data:**
```sql
UPDATE users 
SET email = 'new_email@example.com' 
WHERE username = 'john_doe';
```

**Deleting data:**
```sql
-- Delete specific records
DELETE FROM users WHERE user_id = 5;

-- Delete all records
DELETE FROM users;
```

### Querying Data

**Basic queries:**
```sql
-- Select all columns
SELECT * FROM users;

-- Select specific columns
SELECT username, email FROM users;

-- Filter with WHERE
SELECT * FROM users WHERE username LIKE 'j%';
```

**Sorting and limiting:**
```sql
-- Sort results
SELECT * FROM users ORDER BY created_at DESC;

-- Limit results
SELECT * FROM users LIMIT 10;

-- Pagination
SELECT * FROM users LIMIT 10 OFFSET 20;
```

### Transactions

SQLite is ACID-compliant and supports transactions:

```sql
-- Begin a transaction
BEGIN TRANSACTION;

-- Perform operations
INSERT INTO users (username) VALUES ('user1');
UPDATE products SET stock = stock - 1 WHERE product_id = 100;

-- Commit changes
COMMIT;

-- Or roll back if there's an issue
-- ROLLBACK;
```

## SQLite Configuration

SQLite has minimal configuration, mainly through PRAGMA statements.

### Common PRAGMA Commands

```sql
-- Enable foreign keys (off by default)
PRAGMA foreign_keys = ON;

-- Set journal mode (options: DELETE, TRUNCATE, PERSIST, MEMORY, WAL, OFF)
PRAGMA journal_mode = WAL;

-- Set cache size (in pages)
PRAGMA cache_size = 5000;

-- Set synchronous mode (options: OFF, NORMAL, FULL, EXTRA)
PRAGMA synchronous = NORMAL;

-- Check database integrity
PRAGMA integrity_check;
```

### Best Practices for Configuration

For optimal performance:
- Enable WAL (Write-Ahead Logging) for most applications:
  ```sql
  PRAGMA journal_mode = WAL;
  ```
- Adjust cache size based on available memory:
  ```sql
  PRAGMA cache_size = -10000;  -- Negative values are kilobytes, positive are pages
  ```
- Consider synchronous settings for speed vs. safety tradeoff:
  ```sql
  PRAGMA synchronous = NORMAL;  -- Default, good balance
  ```

## Maintaining SQLite Databases

### Vacuuming Databases

SQLite does not automatically reclaim space from deleted data. Use VACUUM to compact the database:

```sql
-- Vacuum the entire database
VACUUM;

-- Vacuum into a new file
VACUUM INTO 'compacted.db';
```

### Analyzing Tables

For query optimization:

```sql
-- Analyze a specific table
ANALYZE users;

-- Analyze all tables
ANALYZE;
```

### Integrity Check

Verify database integrity:

```sql
PRAGMA integrity_check;
```

### Backup and Restore

**Creating backups:**

Using SQL:
```sql
-- Backup to SQL
.output backup.sql
.dump
.output stdout
```

Using the command line:
```bash
wsl sqlite3 mydatabase.db .dump > backup.sql
```

**Restoring from backups:**

```bash
wsl sqlite3 restored.db < backup.sql
```

Or within SQLite:
```sql
.read backup.sql
```

## SQLite in Various Environments

### SQLite in Command Line

As demonstrated above, using the `sqlite3` command.

### SQLite in Programming Languages

**Python:**
```python
import sqlite3
conn = sqlite3.connect('example.db')
cursor = conn.cursor()
cursor.execute("SELECT * FROM users")
rows = cursor.fetchall()
conn.close()
```

**Node.js:**
```javascript
const sqlite3 = require('sqlite3').verbose();
let db = new sqlite3.Database('example.db');
db.all("SELECT * FROM users", [], (err, rows) => {
  console.log(rows);
});
db.close();
```

**PHP:**
```php
$db = new SQLite3('example.db');
$results = $db->query('SELECT * FROM users');
while ($row = $results->fetchArray()) {
    print_r($row);
}
$db->close();
```

### SQLite in Mobile Applications

SQLite is widely used in mobile app development:

- **Android**: Uses SQLite for local data storage, accessed via Room persistence library or SQLiteOpenHelper
- **iOS**: Core Data framework often uses SQLite as its persistent store

### SQLite in Desktop Applications

Many desktop applications use SQLite for data storage:
- Web browsers (Firefox, Chrome) for bookmarks, history, etc.
- Email clients for local message storage
- Media players for library management

## Troubleshooting

### Database Locking

**Problem**: "database is locked" error

**Solutions**:
- Ensure all connections are properly closed
- Use transactions appropriately
- Configure a busy timeout:
  ```sql
  PRAGMA busy_timeout = 5000;  -- Wait up to 5 seconds
  ```
- Check for other processes accessing the database
- Consider using WAL mode for better concurrency

### Database Corruption

**Problem**: Database file is corrupted

**Solutions**:
- Run integrity check:
  ```sql
  PRAGMA integrity_check;
  ```
- Restore from a backup
- Dump recoverable data:
  ```sql
  .output recovered.sql
  .dump
  ```
- Rebuild the database from the dump

### Performance Issues

**Problem**: Slow query execution

**Solutions**:
- Create indexes for frequently queried columns:
  ```sql
  CREATE INDEX idx_username ON users(username);
  ```
- Analyze tables to improve query planning:
  ```sql
  ANALYZE;
  ```
- Use EXPLAIN QUERY PLAN to understand query execution:
  ```sql
  EXPLAIN QUERY PLAN SELECT * FROM users WHERE username = 'john';
  ```
- Enable WAL mode for concurrent access patterns
- Optimize queries to avoid full table scans

### Concurrency Limitations

**Problem**: Limited support for concurrent write operations

**Solutions**:
- Use transactions to minimize lock duration
- Implement application-level queuing for write operations
- Consider WAL mode for better concurrency:
  ```sql
  PRAGMA journal_mode = WAL;
  ```
- For high concurrency needs, consider a client-server database instead

## Next Steps

Now that you understand how to run SQLite databases, you can proceed to:
- [Connect to SQLite using Navicat](../Connecting_with_Navicat/README.md)
- [Load data into your SQLite database](../Loading_Data/README.md)
- [Run SQL queries with Navicat](../Running_SQL_Queries/README.md)

For more information, refer to the [official SQLite documentation](https://www.sqlite.org/docs.html). 