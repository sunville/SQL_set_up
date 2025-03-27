# Loading Data into SQLite

[← Back to SQLite Database Tutorial](../SQLite%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions on importing data into SQLite databases using Navicat, covering various data sources, import methods, and optimization techniques.

## Table of Contents
- [Import Methods Overview](#import-methods-overview)
- [Using the Navicat Import Wizard](#using-the-navicat-import-wizard)
- [Importing from Different File Formats](#importing-from-different-file-formats)
- [Importing from Other Databases](#importing-from-other-databases)
- [Executing SQL Scripts](#executing-sql-scripts)
- [Bulk Insert Operations](#bulk-insert-operations)
- [Working with JSON Data](#working-with-json-data)
- [Data Transformation During Import](#data-transformation-during-import)
- [Import Performance Optimization](#import-performance-optimization)
- [Common Import Issues](#common-import-issues)

## Import Methods Overview

Navicat offers several methods for loading data into SQLite databases:

| Method | Best for | Advantages | Limitations |
|--------|----------|------------|-------------|
| Import Wizard | Most imports | User-friendly, visual mapping | Not fully customizable |
| Execute SQL File | SQL scripts | Full control, complex operations | Requires SQL knowledge |
| Table Data Import | Specific tables | Simple, direct table mapping | Limited transformation options |
| Query Results Import | Filtered data sets | Precise data selection | Requires query writing |
| External Data Import | External sources | Connect directly to other data | May require additional drivers |

## Using the Navicat Import Wizard

The Import Wizard provides a step-by-step interface for importing data:

1. **Launch the Import Wizard**:
   - Right-click on a database connection in the Navigation Pane
   - Select **Import Wizard**
   - Or select **Tools** → **Import Wizard** from the menu bar
   
   ![Import Wizard](images/import_wizard.png)

2. **Select Import Source**:
   - Choose the file or data source format (CSV, Excel, XML, etc.)
   - Browse to select your source file
   - Configure file-specific options (e.g., delimiters for CSV)

3. **Import Mode Selection**:
   - **Create new table**: Create a new table to hold the imported data
   - **Append data to table**: Add data to an existing table
   - **Replace table data**: Clear and replace existing table data

4. **Field Mapping**:
   - Map source fields to destination fields
   - Select data types for each field
   - Configure transformation options
   - Set constraints (primary key, uniqueness, etc.)

5. **Import Settings**:
   - Set import options (commit intervals, error handling)
   - Configure additional settings specific to your data source
   - Preview the data to be imported

6. **Execute Import**:
   - Start the import process
   - View progress and any errors
   - Save import profile for future use (optional)

## Importing from Different File Formats

### CSV/Text Files

1. **Import from CSV**:
   - In the Import Wizard, select **CSV** as the source
   - Configure CSV-specific options:
     - Field delimiter (comma, tab, semicolon, etc.)
     - Text qualifier (quotes)
     - First row contains field names option
   
   ```
   id,name,email,created_date
   1,"John Smith",john@example.com,2023-01-15
   2,"Jane Doe",jane@example.com,2023-01-16
   ```

2. **Advanced CSV Options**:
   - Character set selection (UTF-8 recommended)
   - Line ending format (Windows, Unix, or Mac)
   - NULL value representation (empty or specific text)
   - Date and time format specification

### Excel Files

1. **Import from Excel**:
   - Select **Excel** as the source type
   - Navigate and select your Excel file (.xlsx or .xls)
   - Choose which worksheet to import from
   - Specify whether first row contains field names

2. **Excel-specific Settings**:
   - Select cell range (optional)
   - Handle merged cells
   - Configure data type recognition
   - Set column formats (date, numeric, text)

### XML Data

1. **Import from XML**:
   - Select **XML** as the source type
   - Specify root element and row element names
   - Configure field element mappings
   - Set attribute and value mapping options

2. **Sample XML Structure**:
   ```xml
   <users>
     <user id="1" name="John Smith" email="john@example.com" />
     <user id="2" name="Jane Doe" email="jane@example.com" />
   </users>
   ```

### JSON Data

1. **Import from JSON**:
   - Select **JSON** as the source type
   - Choose between array format or object format
   - Configure field mapping from JSON properties
   - Set nested object handling options

2. **Sample JSON Structure**:
   ```json
   [
     {"id": 1, "name": "John Smith", "email": "john@example.com"},
     {"id": 2, "name": "Jane Doe", "email": "jane@example.com"}
   ]
   ```

## Importing from Other Databases

### Database to Database Import

1. **Using Navicat connection**:
   - Create connections to both source and target databases
   - Right-click on the source table
   - Select **Copy to Another Database**
   - Choose target database and import options

2. **Cross-database type import**:
   - Works between different database types (MySQL, PostgreSQL, etc.)
   - Automatically maps data types between systems
   - Handles primary keys and indexes conversion

### SQLite Database Files

1. **Direct file import**:
   - In Navicat, right-click on the database
   - Select **Execute SQL File**
   - Choose the source SQLite database file
   - Use `.dump` command output for direct SQL import

2. **Using ATTACH DATABASE**:
   - Attach the source database to current connection:
     ```sql
     ATTACH DATABASE 'source.db' AS source;
     
     -- Copy table structure and data
     CREATE TABLE target_table AS 
     SELECT * FROM source.source_table;
     
     DETACH DATABASE source;
     ```

## Executing SQL Scripts

### Import via SQL Scripts

1. **Run SQL Scripts**:
   - Right-click on the database
   - Select **Execute SQL File**
   - Choose your SQL script file
   - Set execution options (stop on error, etc.)

2. **Writing Effective Import Scripts**:
   ```sql
   BEGIN TRANSACTION;
   
   -- Create table if needed
   CREATE TABLE IF NOT EXISTS users (
       id INTEGER PRIMARY KEY,
       name TEXT NOT NULL,
       email TEXT UNIQUE,
       created_date TEXT
   );
   
   -- Insert data
   INSERT INTO users (id, name, email, created_date) VALUES
   (1, 'John Smith', 'john@example.com', '2023-01-15'),
   (2, 'Jane Doe', 'jane@example.com', '2023-01-16');
   
   COMMIT;
   ```

3. **Using SQL Variables**:
   ```sql
   -- Set variables in SQL script
   PRAGMA foreign_keys = ON;
   
   -- Use parameters (when supported)
   INSERT INTO users (name, email) VALUES (?, ?);
   ```

## Bulk Insert Operations

### Efficient Bulk Inserts

1. **Transaction Wrapping**:
   ```sql
   BEGIN TRANSACTION;
   
   -- Multiple INSERT statements
   INSERT INTO table_name VALUES (...);
   INSERT INTO table_name VALUES (...);
   -- ...many more inserts...
   
   COMMIT;
   ```

2. **Multi-row INSERT**:
   ```sql
   INSERT INTO users (name, email) VALUES 
   ('User 1', 'user1@example.com'),
   ('User 2', 'user2@example.com'),
   ('User 3', 'user3@example.com'),
   ('User 4', 'user4@example.com'),
   ('User 5', 'user5@example.com');
   ```

3. **Using prepared statements**:
   - Prepare your statement once and execute multiple times
   - Reduces parsing overhead
   - Better for programmatic imports

### Handling Large Datasets

1. **Chunking large imports**:
   - Split large files into smaller batches
   - Import each batch with separate transactions
   - Reduces memory usage and improves error recovery

2. **Progress tracking**:
   - Use Navicat's progress indicator
   - For custom scripts, add progress reporting
   - Consider logging import status

## Working with JSON Data

SQLite has limited native JSON support compared to some other databases, but Navicat helps bridge the gap:

1. **Import JSON into TEXT columns**:
   - Store JSON as text in SQLite
   - Use JSON functions for extraction when needed
   - Validate JSON format during import

2. **Using JSON1 Extension**:
   ```sql
   -- With JSON1 extension loaded
   SELECT json_extract(json_column, '$.name') AS name
   FROM json_table;
   ```

3. **Transforming JSON to Regular Columns**:
   - Flatten JSON structure during import
   - Map JSON properties to individual columns
   - Use custom transformations for complex structures

## Data Transformation During Import

### Field Transformations

1. **Field type conversion**:
   - Convert text to numbers, dates, etc.
   - Format date strings to SQLite date format
   - Set appropriate data types during import

2. **Using import-time expressions**:
   - Apply calculations to imported data
   - Combine fields (e.g., first name + last name)
   - Convert units or formats

3. **Advanced transformations with SQL**:
   - Import to temporary table first
   - Use SQL to transform and move to final table:
     ```sql
     INSERT INTO final_table (name, dob, age)
     SELECT 
       name,
       date(dob_string),
       (julianday('now') - julianday(dob_string))/365.25
     FROM temp_import_table;
     ```

### Data Cleansing

1. **Handling missing values**:
   - Set default values for NULL or empty fields
   - Skip records with critical missing data
   - Apply custom values for special cases

2. **Text data cleaning**:
   - Trim whitespace from text fields
   - Standardize case (upper/lower/proper)
   - Replace or remove special characters

3. **Filtering during import**:
   - Import only records meeting specific criteria
   - Use WHERE clauses in SQL-based imports
   - Set filters in the import wizard

## Import Performance Optimization

### Speeding Up Imports

1. **Database configuration for imports**:
   ```sql
   -- Optimize for bulk insert operations
   PRAGMA synchronous = OFF;
   PRAGMA journal_mode = MEMORY;
   PRAGMA cache_size = 10000;
   PRAGMA temp_store = MEMORY;
   
   -- Don't forget to reset after import
   -- PRAGMA synchronous = NORMAL;
   -- PRAGMA journal_mode = WAL;
   ```

2. **Disable indexes during import**:
   - Drop indexes before import (or defer creation)
   - Create indexes after data is loaded
   - Significantly faster for large imports

3. **Optimize transaction size**:
   - Very large transactions: memory usage issues
   - Very small transactions: performance overhead
   - Aim for balanced transaction size (1,000-10,000 rows)

### Memory Usage Management

1. **Import in batches**:
   - Split large files into manageable chunks
   - Process one chunk at a time
   - Reduces memory footprint

2. **Clean up temporary files**:
   - Remove temporary files after import
   - Manage disk space during large imports
   - Use VACUUM after large operations

## Common Import Issues

### Error Handling

1. **Dealing with duplicate keys**:
   - Use INSERT OR IGNORE for soft failure
   - Set unique constraint handling in import wizard
   - Use temporary tables and filtering to remove duplicates

2. **Data type mismatches**:
   - Validate data before import
   - Use explicit type conversion
   - Create tables with appropriate column types

3. **Character encoding issues**:
   - Use UTF-8 encoding when possible
   - Set correct character set in import settings
   - Convert source files if necessary

### Troubleshooting Import Failures

1. **Check error logs**:
   - Navicat provides error details during import
   - Review SQL error messages
   - Look for pattern in failures

2. **Common causes of failure**:
   - Invalid data formats
   - Constraint violations (unique, foreign key)
   - File access or permission issues
   - Memory or disk space limitations

3. **Recovery strategies**:
   - Import to temporary table first
   - Fix issues, then transfer clean data
   - Consider partial imports of valid data

## Next Steps

Now that you've learned how to load data into your SQLite database, you can:

- [Browse and view your data](../Browsing_Data/README.md)
- [Run SQL queries to analyze the data](../Running_SQL_Queries/README.md)
- [Export your data in various formats](../Exporting_Data/README.md)
- [Manage and administer your database](../Database_Administration/README.md)

For more information about SQLite and Navicat, refer to the [official SQLite documentation](https://www.sqlite.org/docs.html) and [Navicat documentation](https://www.navicat.com/en/support/navicat-for-sqlite-manual). 