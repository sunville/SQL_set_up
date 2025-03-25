# Running SQLite Databases

[← Back to SQLite Database Tutorial](../SQLite%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions for running and managing SQLite databases. Unlike traditional database systems that require a server process, SQLite is serverless and runs directly within your application or through the SQLite command-line interface.

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

SQLite uses a unique architecture compared to other database systems:

### Serverless Design

- **No separate server process**: Unlike MySQL or PostgreSQL, SQLite doesn't require a database server
- **Direct disk access**: SQLite reads and writes directly to ordinary disk files
- **Self-contained**: The entire database (definitions, tables, indices, data) is stored in a single cross-platform file

### SQLite Database Files

- **Single file structure**: A complete database is stored in a single file, typically with a `.db`, `.sqlite`, or `.sqlite3` extension
- **Cross-platform compatibility**: The same database file works across operating systems
- **Size limitations**: Theoretically supports databases up to 140 terabytes, though practical limits are often lower
- **File locking**: SQLite uses file-level locking for write operations

## SQLite Command-Line Interface

The SQLite command-line tool (`sqlite3`) is a simple program that allows you to manually enter and execute SQL statements against an SQLite database.

### Accessing the Command Line

**Windows (WSL)**:
```bash
wsl sqlite3
```

**macOS/Linux**:
```bash
sqlite3
```

### Opening a Database

```bash
# Create/open a database file
wsl sqlite3 mydatabase.db

# Create/open database in memory (temporary)
wsl sqlite3 :memory:
```

### Command Line Options

```bash
# Open with column headers displayed
wsl sqlite3 -header mydatabase.db

# Set column mode for better readability
wsl sqlite3 -column mydatabase.db

# Combine options
wsl sqlite3 -header -column mydatabase.db

# Run SQL directly from command line
wsl sqlite3 mydatabase.db "SELECT * FROM users;"

# Run SQL from file
wsl sqlite3 mydatabase.db < queries.sql
```

## Creating and Opening Databases

### Creating a New Database

1. **Using command line**:
   ```bash
   wsl sqlite3 new_database.db
   sqlite> CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT);
   sqlite> .quit
   ```

2. **Creating in-memory database** (temporary, for testing):
   ```bash
   wsl sqlite3 :memory:
   ```

### Opening Existing Databases

1. **From command line**:
   ```bash
   wsl sqlite3 mydatabase.db
   ```

2. **From Navicat**:
   - Launch Navicat
   - Create a new SQLite connection
   - Browse to your database file

### Attaching Multiple Databases

SQLite allows you to work with multiple databases in a single session:

```sql
-- Attach another database file
ATTACH DATABASE 'another.db' AS other;

-- Now you can query across both
SELECT * FROM main.users, other.products;

-- Detach when done
DETACH DATABASE other;
```

## Basic SQLite Commands

SQLite command-line interface includes two types of commands:
1. Standard SQL statements
2. Special "dot" commands that control the SQLite program itself

### Special Dot Commands

```
.help           Show help on commands
.tables         List all tables
.schema         Show the CREATE statements for all tables
.schema TABLE   Show the CREATE statement for TABLE
.mode column    Set output mode to columnar
.headers on     Turn display of headers on
.width NUM NUM  Set column widths for columnar output
.output FILE    Send output to FILE
.output stdout  Send output back to screen
.dump           Dump database as SQL text
.exit or .quit  Exit SQLite
```

### SQL Statement Examples

```sql
-- Create a table
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    price REAL,
    description TEXT
);

-- Insert data
INSERT INTO products (name, price, description) 
VALUES ('Product 1', 29.99, 'This is product 1');

-- Query data
SELECT * FROM products;

-- Update data
UPDATE products SET price = 24.99 WHERE id = 1;

-- Delete data
DELETE FROM products WHERE id = 1;
```

### Transaction Management

SQLite supports transactions for grouping operations:

```sql
-- Begin a transaction
BEGIN TRANSACTION;

-- Perform operations
INSERT INTO products (name, price) VALUES ('Product 2', 19.99);
INSERT INTO products (name, price) VALUES ('Product 3', 39.99);

-- Commit changes
COMMIT;

-- Or roll back if needed
-- ROLLBACK;
```

## SQLite Configuration

SQLite requires minimal configuration, but there are some settings you can adjust:

### Pragmas

SQLite uses PRAGMA statements to modify its behavior:

```sql
-- Check current journal mode
PRAGMA journal_mode;

-- Set WAL journal mode (better for concurrent access)
PRAGMA journal_mode = WAL;

-- Enable foreign key constraints (off by default)
PRAGMA foreign_keys = ON;

-- Set cache size (in pages)
PRAGMA cache_size = 10000;

-- Set page size (before creating any tables)
PRAGMA page_size = 4096;
```

### Common Pragmas and Their Use

| Pragma | Purpose |
|--------|---------|
| journal_mode | Controls how journal files are stored (DELETE, TRUNCATE, PERSIST, MEMORY, WAL) |
| foreign_keys | Enables/disables foreign key constraint enforcement |
| cache_size | Number of database pages to keep in memory |
| synchronous | Level of disk synchronization (OFF, NORMAL, FULL) |
| temp_store | Where temporary tables and indices are stored |
| page_size | Size of database pages (512-65536 bytes, power of 2) |

### Configuration Best Practices

1. **Enable foreign key constraints**:
   ```sql
   PRAGMA foreign_keys = ON;
   ```

2. **Use WAL mode for better concurrency**:
   ```sql
   PRAGMA journal_mode = WAL;
   ```

3. **Adjust synchronous setting based on need**:
   ```sql
   -- Normal safety (balance of safety and speed)
   PRAGMA synchronous = NORMAL;
   
   -- Full safety (slower but safer)
   PRAGMA synchronous = FULL;
   
   -- Maximum speed, risk of corruption on power loss
   PRAGMA synchronous = OFF;
   ```

## Maintaining SQLite Databases

Regular maintenance keeps your SQLite databases running optimally.

### Vacuuming the Database

When you delete data, the database file doesn't automatically shrink. Use VACUUM to reclaim space:

```sql
-- Simple vacuum
VACUUM;

-- Vacuum into a new file (requires SQLite 3.27.0+)
VACUUM INTO 'compacted.db';
```

### Analyzing Tables

Update statistics to improve query planning:

```sql
-- Analyze all tables
ANALYZE;

-- Analyze specific table
ANALYZE users;
```

### Integrity Check

Check database for corruption:

```sql
PRAGMA integrity_check;
```

### Backing Up Databases

1. **Using the .backup command**:
   ```
   sqlite> .backup 'backup.db'
   ```

2. **Using SQL dump**:
   ```
   sqlite> .output backup.sql
   sqlite> .dump
   sqlite> .output stdout
   ```

3. **Simple file copy** (when database is not in use):
   ```bash
   wsl cp mydatabase.db backup.db
   ```

## SQLite in Various Environments

SQLite can be used in many different contexts:

### Command Line

As we've seen, the `sqlite3` command-line tool provides direct access to SQLite databases.

### Programming Languages

Most programming languages have SQLite support:

**Python**:
```python
import sqlite3
conn = sqlite3.connect('example.db')
c = conn.cursor()
c.execute('SELECT * FROM users')
```

**Node.js**:
```javascript
const sqlite3 = require('sqlite3').verbose();
let db = new sqlite3.Database('example.db');
db.all("SELECT * FROM users", (err, rows) => {
  console.log(rows);
});
```

**PHP**:
```php
$db = new SQLite3('example.db');
$results = $db->query('SELECT * FROM users');
while ($row = $results->fetchArray()) {
    echo $row['name'] . "\n";
}
```

### Mobile Applications

SQLite is often used in mobile apps:

- **Android**: Using Room persistence library or SQLiteOpenHelper
- **iOS**: Using Core Data or SQLite directly

### Desktop Applications

Many desktop applications use SQLite for data storage, including:
- Firefox (browser data)
- Thunderbird (email storage)
- Skype (chat history)
- Many games and productivity applications

## Troubleshooting

Common SQLite issues and their solutions:

### Database Locked

**Problem**: "database is locked" error when trying to write to the database.

**Solution**:
1. Ensure all other connections are closed
2. Check if another process is using the database
3. Increase timeout:
   ```sql
   PRAGMA busy_timeout = 5000;  -- 5 seconds
   ```

### Corruption Issues

**Problem**: Database corruption after system crash or power failure.

**Solution**:
1. Run integrity check:
   ```sql
   PRAGMA integrity_check;
   ```
2. Restore from a backup if available
3. Use SQLite recovery tools (like `.recover` in newer versions)
4. Consider using WAL mode to reduce corruption risk

### Performance Issues

**Problem**: Slow queries or operations.

**Solution**:
1. Create proper indexes:
   ```sql
   CREATE INDEX idx_users_email ON users(email);
   ```
2. Use EXPLAIN QUERY PLAN to analyze queries:
   ```sql
   EXPLAIN QUERY PLAN SELECT * FROM users WHERE email = 'test@example.com';
   ```
3. Enable the cache:
   ```sql
   PRAGMA cache_size = 10000;
   ```
4. Consider using prepared statements for repeated queries
5. Use transactions for multiple inserts/updates

### Concurrency Limitations

**Problem**: Multiple processes trying to write to the database.

**Solution**:
1. Use WAL mode for better concurrency:
   ```sql
   PRAGMA journal_mode = WAL;
   ```
2. Keep transactions short
3. Consider a client-server database if you need high concurrency

## Next Steps

Now that you understand how to run and manage SQLite databases, you can:

1. [Connect to your SQLite database using Navicat](../Connecting_with_Navicat/README.md)
2. [Load data into your SQLite database](../Loading_Data/README.md)
3. [Browse and explore your database](../Browsing_Data/README.md)

For more information, refer to the [official SQLite documentation](https://www.sqlite.org/docs.html). 