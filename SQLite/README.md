# SQLite Database Tutorial with Navicat

## Table of Contents
- [Introduction to SQLite](#introduction-to-sqlite)
- [Installation](#installation)
- [Creating and Using SQLite Databases](#creating-and-using-sqlite-databases)
- [Loading Data](#loading-data)
- [User Access Control](#user-access-control)
- [Connecting with Navicat](#connecting-with-navicat)
- [Common Operations in Navicat](#common-operations-in-navicat)

## Introduction to SQLite

SQLite is a self-contained, serverless, zero-configuration, transactional SQL database engine. Unlike most other SQL databases, SQLite does not have a separate server process. SQLite reads and writes directly to ordinary disk files. A complete SQL database with multiple tables, indices, triggers, and views is contained in a single disk file.

Key features of SQLite:
- Serverless - No separate server process needed
- Zero-configuration - No setup or administration required
- Portable - The entire database is stored in a single file
- Small footprint - Minimal memory and disk space requirements
- Cross-platform - Works on virtually all operating systems

## Installation

SQLite is often pre-installed on many operating systems (including macOS and many Linux distributions). However, if you need to install it manually:

### Windows
1. Visit the [SQLite Download Page](https://www.sqlite.org/download.html)
2. Download the precompiled binaries for Windows
   - "sqlite-tools-win32-x86-*.zip" (command line tools)
3. Extract the ZIP file to a folder (e.g., `C:\sqlite`)
4. Add this folder to your PATH environment variable:
   - Right-click on "This PC" or "Computer" > Properties > Advanced system settings
   - Click "Environment Variables"
   - Select "Path" from system variables and click "Edit"
   - Add the path to your SQLite folder (e.g., `C:\sqlite`)
   - Click "OK" to confirm changes

### macOS
If SQLite is not already installed, use Homebrew:
```bash
brew install sqlite
```

### Linux
For Debian/Ubuntu:
```bash
sudo apt update
sudo apt install sqlite3
```

For Red Hat/Fedora:
```bash
sudo dnf install sqlite
```

## Creating and Using SQLite Databases

Unlike other database systems, SQLite doesn't require you to set up a server. You simply create or connect to a database file.

### Creating a New Database

Using the SQLite command line:

```bash
# Open SQLite with a new or existing database file
sqlite3 mydatabase.db

# You'll see the SQLite prompt
sqlite>
```

### Basic Commands

```sql
-- Create a new table
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert data
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
INSERT INTO users (name, email) VALUES ('Jane Smith', 'jane@example.com');

-- Query data
SELECT * FROM users;

-- Exit SQLite
.quit
```

### SQLite Special Commands

SQLite has special dot commands for various functions:

```
.help           Show help on commands
.tables         List all tables
.schema TABLE   Show the CREATE statement for TABLE
.mode column    Set output mode to columnar
.headers on     Turn display of headers on
.exit or .quit  Exit SQLite
```

## Loading Data

### Creating a Database and Table
```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    price REAL,
    description TEXT
);
```

### Loading Data from CSV

1. Open your database:
```bash
sqlite3 mydatabase.db
```

2. Configure import settings and import data:
```
.mode csv
.headers on
.import /path/to/data.csv products
```

### Importing SQL Dump File
```bash
sqlite3 mydatabase.db < dump.sql
```

### Exporting Data
```
-- Export to CSV
.headers on
.mode csv
.output data.csv
SELECT * FROM products;
.output stdout

-- Export schema
.output schema.sql
.schema
.output stdout
```

## User Access Control

SQLite has limited built-in user authentication and authorization. It primarily relies on file system permissions for access control.

### File Permissions
- Set appropriate file permissions on your SQLite database file:
  
  **Unix/Linux/macOS**:
  ```bash
  # Give read/write access to owner and group only
  chmod 660 mydatabase.db
  
  # Change the owner/group
  chown user:group mydatabase.db
  ```
  
  **Windows**:
  - Right-click on the database file > Properties > Security
  - Adjust permissions for users and groups

### Encryption
SQLite doesn't have built-in encryption. For sensitive data, consider:

1. **SQLCipher** - An SQLite extension that provides transparent 256-bit AES encryption
   - [SQLCipher Website](https://www.zetetic.net/sqlcipher/)
   - Available as a commercial product with open-source community edition

2. **Full Disk Encryption** - Encrypt the entire drive containing your SQLite databases

3. **Application-Level Encryption** - Encrypt sensitive data before storing it in the database

## Connecting with Navicat

1. **Install Navicat**:
   - Download and install Navicat from the [official website](https://www.navicat.com/en/download/navicat-for-sqlite).
   - Choose the edition that suits your needs (there's a free trial available).

2. **Create a New Connection**:
   - Launch Navicat
   - Click the "Connection" button in the top-left corner
   - Select "SQLite" as the connection type
   - Fill in the connection details:
     - Connection Name: Choose a meaningful name (e.g., "My SQLite Database")
     - Database File: Browse to select your SQLite database file (e.g., mydatabase.db)
     - For encrypted databases (if using SQLCipher), enter the password
   - Test the connection using the "Test Connection" button
   - Click "OK" to save the connection

3. **Connect to the Database**:
   - Double-click on your newly created connection in the left sidebar
   - Navicat will connect to the SQLite database and display all tables

## Common Operations in Navicat

### Creating a New Database
1. Click "Connection" and select "SQLite"
2. Enter a connection name
3. Click the "..." button next to Database File
4. Choose a location and name for your new database
5. Click "Save" and "OK"

### Browsing Data
- Expand your connection in the left sidebar
- Double-click any table to see its data
- Use the filter and sort options to organize your view

### Running SQL Queries
1. Click the "Query" button on the toolbar
2. Select your connection
3. Write your SQL query in the editor
4. Click "Run" to execute the query

### Importing Data
1. Right-click on a table and select "Import Wizard"
2. Choose the file format (CSV, Excel, JSON, etc.)
3. Select the file to import
4. Configure import settings (delimiters, encoding, etc.)
5. Map columns to table fields
6. Click "Start" to import the data

### Exporting Data
1. Right-click on a table and select "Export Wizard"
2. Choose the export format
3. Configure export settings
4. Choose the destination file
5. Click "Start" to export the data

### Creating and Modifying Tables
1. Right-click on "Tables" and select "New Table"
2. Define table structure (columns, types, constraints)
3. Click "Save" to create the table

### Database Design and Modeling
1. Click on "Model" in the toolbar
2. Create a new model
3. Add tables and define relationships
4. Synchronize the model with the database

This tutorial covers the basics of setting up and using SQLite with Navicat. For more detailed information, refer to the [SQLite documentation](https://www.sqlite.org/docs.html) and [Navicat documentation](https://www.navicat.com/en/support/navicat-for-sqlite-manual). 