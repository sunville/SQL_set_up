# Loading Data into PostgreSQL Database

[← Back to PostgreSQL Database Tutorial](../PostgreSQL%20Database%20Tutorial%20with%20Navicat.md)

This guide will provide detailed instructions on how to use Navicat to import data from various formats into PostgreSQL databases, including importing table data, performing bulk imports, data migration, and handling various import challenges.

## Table of Contents
- [Overview of Import Methods](#overview-of-import-methods)
- [Importing Data from Files](#importing-data-from-files)
- [Importing from Other Databases](#importing-from-other-databases)
- [Using the Import Wizard](#using-the-import-wizard)
- [Executing Bulk Inserts](#executing-bulk-inserts)
- [Using the COPY Command](#using-the-copy-command)
- [Importing JSON and XML Data](#importing-json-and-xml-data)
- [Importing Geospatial Data](#importing-geospatial-data)
- [Data Import Strategies](#data-import-strategies)
- [Troubleshooting Import Errors](#troubleshooting-import-errors)
- [Performance Optimization Tips](#performance-optimization-tips)

## Overview of Import Methods

PostgreSQL provides multiple methods for importing data into databases, and Navicat makes this process more intuitive and efficient. Here's a comparison of the main import methods:

| Import Method | Suitable For | Advantages | Limitations |
|----------|----------|------|------|
| Import Wizard | Structured file imports | User-friendly, supports multiple formats | May be slow for large datasets |
| SQL Scripts | Predefined data imports | Highly customizable, reusable | Requires SQL knowledge |
| COPY Command | High-performance bulk imports | Fastest speed, server-side processing | Files must be accessible on the server |
| Bulk Insert | Medium-sized datasets | Client-friendly, transaction control | Not as fast as COPY command |
| Foreign Data Wrappers (FDW) | Connecting to external data sources | Real-time data access, no copying needed | Performance overhead, complex configuration |

## Importing Data from Files

Navicat supports importing data from various file formats into PostgreSQL tables.

### Importing from CSV/TSV Files

1. **Prepare for Import**:
   - Connect to your PostgreSQL database in Navicat
   - Right-click on the target table, select **Import Wizard**
   
   ![Import Wizard](images/import_wizard.png)

2. **Select File Format and Location**:
   - Choose **File** as the source
   - Select **CSV** or **TXT** (for TSV) as the format
   - Browse and select your data file

3. **Configure Import Options**:
   - **Field Delimiter**: Comma (,) for CSV, Tab (\t) for TSV
   - **Text Delimiter**: Usually double quotes (")
   - **First Row Contains Field Names**: Check this if the first row of your CSV file contains column names
   - **Encoding**: Select the character encoding of your file (e.g., UTF-8)

4. **Field Mapping**:
   - Map CSV columns to the corresponding columns in your table
   - You can skip columns that don't need to be imported
   - Set data type conversion rules

5. **Import Settings**:
   - **Insert Records**: Add the data as new records
   - **Empty Table if Exists**: Clear the table before importing
   - **Ignore Import Errors**: Continue importing even if errors occur
   - **Enable Transactions**: Treat the entire import as a single transaction

6. **Execute Import**:
   - Click the **Start** button to execute the import
   - View the import progress and result summary

### Importing from Excel Files

1. Right-click on the target table, select **Import Wizard**
2. Choose **File** as the source, with **Excel** as the format
3. Browse and select your Excel file
4. Select the worksheet to import
5. Configure mapping and import options (similar to CSV import)
6. Execute the import

### Importing from JSON Files

1. Right-click on the target table, select **Import Wizard**
2. Choose **File** as the source, with **JSON** as the format
3. Browse and select your JSON file
4. Set the JSON path and array path (if importing array data)
5. Map JSON fields to table columns
6. Configure special field handling (like nested objects)
7. Execute the import

## Importing from Other Databases

Navicat allows you to import data directly from other databases into PostgreSQL.

### Importing from Other PostgreSQL Databases

1. **Prepare for Import**:
   - Ensure you have both source and target PostgreSQL databases connected in Navicat
   - Right-click on the target table, select **Import Wizard**

2. **Select Source Database**:
   - Choose **Database** as the source
   - Select the source connection and database from the dropdown list

3. **Select Source Tables and Fields**:
   - Choose the source table to import data from
   - Select specific fields to import

4. **Configure Import Options**:
   - Set filter conditions to import only specific records
   - Configure advanced options like batch size and timeout settings

5. **Execute Import**:
   - Click the **Start** button to execute the import
   - View the import progress and result summary

### Importing from MySQL/SQL Server/Oracle

1. Create a connection to the corresponding database type in Navicat
2. Follow steps similar to those above
3. Pay attention to data type differences and special characters

## Using the Import Wizard

Navicat's Import Wizard provides advanced features to enhance the data import process.

### Custom Data Transformations

During the import process, you can apply the following transformations:

1. **Set Target Values**:
   - **NULL Value Handling**: Set default values for empty values
   - **Date/Time Format**: Specify the input format for dates and times
   - **Boolean Representation**: Define which values should be converted to true/false

2. **Using Expressions**:
   - Use SQL expressions in your import mapping
   - For example: `UPPER([FieldName])` to convert text to uppercase

### Using the Preview Feature

The Import Wizard's preview feature helps you validate import settings before executing the import:

1. Click the **Preview** button to see how the data will be parsed
2. Check field mapping and data type conversions
3. Verify that special characters and encoding are handled correctly

### Saving and Reusing Import Configurations

Frequently performing similar imports? Save your import configuration to save time:

1. After configuring import settings, click **Save Profile**
2. Provide a name and description for the configuration
3. For future imports, use the **Load Profile** button to reuse your settings

## Executing Bulk Inserts

For large datasets, bulk inserts can significantly improve performance.

### Using Navicat for Bulk Inserts

1. When configuring settings in the Import Wizard:
   - Check the **Bulk Insert** option
   - Set the **Records per Batch** (typically 1000-5000 is recommended)

2. Benefits of bulk inserting:
   - Reduces the number of communications with the server
   - Speeds up imports of large datasets
   - Reduces server load

### Executing Bulk Inserts through SQL Scripts

For more advanced control, you can manually create bulk insert scripts:

```sql
BEGIN;

INSERT INTO target_table (column1, column2, column3)
VALUES
  ('value1_1', 'value1_2', 'value1_3'),
  ('value2_1', 'value2_2', 'value2_3'),
  ('value3_1', 'value3_2', 'value3_3'),
  /* You can add more rows */;

COMMIT;
```

To execute this script:
1. Open the query editor in Navicat
2. Paste the script and execute it
3. For very large datasets, split it into multiple transactions

## Using the COPY Command

The COPY command in PostgreSQL is the fastest method for importing and exporting data.

### Using the COPY Command through Navicat

1. In Navicat's query editor, execute the COPY command:

```sql
COPY target_table FROM '/path/to/your/file.csv' 
WITH (FORMAT csv, HEADER true, DELIMITER ',', QUOTE '"');
```

2. Important notes:
   - The file path must be a path **on the server**, not a local path
   - If using Windows WSL, ensure the path format is correct

### Client-Side COPY Command (\\copy)

If the file is on your local machine, you can use psql's \copy command:

```bash
wsl psql -U postgres -d your_database -c "\copy target_table FROM '/local/path/to/file.csv' WITH (FORMAT csv, HEADER true)"
```

In Navicat, you can:
1. Open the terminal/command line tool
2. Execute the above command (adjust connection parameters and file path)
3. Return to Navicat and refresh the table to see the imported data

## Importing JSON and XML Data

PostgreSQL has excellent support for JSON and XML data types.

### Importing JSON Data

1. **Create a Table with JSON Columns**:

```sql
CREATE TABLE json_data (
    id SERIAL PRIMARY KEY,
    data JSONB
);
```

2. **Import JSON Files Using Navicat**:
   - Follow the JSON import steps mentioned earlier
   - Ensure you map JSON data to columns of JSONB type

3. **Importing Nested JSON Data**:
   - Use JSON path expressions to parse complex structures
   - Or preserve complete JSON objects and query them later using PostgreSQL's JSON functions

### Importing XML Data

1. **Create a Table with XML Columns**:

```sql
CREATE TABLE xml_data (
    id SERIAL PRIMARY KEY,
    data XML
);
```

2. **Import XML Files**:
   - Use the Import Wizard, selecting the appropriate source format
   - Or use SQL to directly import XML file contents:

```sql
INSERT INTO xml_data (data)
SELECT XMLPARSE(DOCUMENT convert_from(pg_read_binary_file('/path/to/file.xml'), 'UTF8'));
```

## Importing Geospatial Data

PostgreSQL provides powerful geospatial data support through the PostGIS extension.

### Preparing PostGIS

1. **Install the PostGIS Extension**:

```sql
CREATE EXTENSION postgis;
```

2. **Create a Geospatial Table**:

```sql
CREATE TABLE spatial_data (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    geom GEOMETRY(Point, 4326)
);
```

### Importing Shapefile Data

1. **Using the shp2pgsql Tool** (command line):

```bash
wsl shp2pgsql -s 4326 -I input_file.shp spatial_data > spatial_data.sql
wsl psql -U postgres -d your_database -f spatial_data.sql
```

2. **Execute the Generated SQL through Navicat**:
   - Open Navicat's query editor
   - Load and execute the SQL file generated above

### Importing GeoJSON Data

Use PostgreSQL's JSON features and PostGIS to import GeoJSON:

```sql
-- Assuming you already have a table to store GeoJSON
INSERT INTO spatial_data (name, geom)
SELECT 
    properties->>'name',
    ST_GeomFromGeoJSON(geometry)
FROM (
    SELECT json_array_elements(
        (json_extract_path(
            (convert_from(pg_read_binary_file('/path/to/file.geojson'), 'UTF8')::json),
            'features'
        ))
    ) AS feature
) AS features,
LATERAL json_extract_path(feature, 'properties') AS properties,
LATERAL json_extract_path(feature, 'geometry') AS geometry;
```

## Data Import Strategies

### Strategies for Large Dataset Imports

For datasets with millions of rows:

1. **Disable Constraints and Triggers**:
   - Disable triggers, foreign keys, and indexes before importing
   - Re-enable them after import is complete

```sql
-- Disable triggers on the table
ALTER TABLE your_table DISABLE TRIGGER ALL;

-- Import data...

-- Re-enable triggers
ALTER TABLE your_table ENABLE TRIGGER ALL;
```

2. **Batch Import**:
   - Split large datasets into smaller batches
   - Commit transactions after each batch

3. **Optimize Server Settings**:
   - Temporarily increase maintenance work memory:
   ```sql
   SET maintenance_work_mem = '1GB';
   ```
   - Adjust autovacuum settings:
   ```sql
   SET autovacuum_vacuum_threshold = 5000;
   SET autovacuum_analyze_threshold = 5000;
   ```

### Data Quality Strategies

Ensure the quality of imported data:

1. **Preprocess Data**:
   - Clean inconsistencies and errors in source data
   - Normalize formats (dates, numbers, etc.)

2. **Use Temporary Tables**:
   - First import into a temporary table
   - Validate and clean data
   - Move cleaned data to the target table

```sql
-- Create temporary table
CREATE TEMP TABLE temp_import AS
SELECT * FROM target_table WITH NO DATA;

-- Import data into temporary table...

-- Validate and clean
DELETE FROM temp_import WHERE column1 IS NULL;
UPDATE temp_import SET column2 = UPPER(column2);

-- Move data to target table
INSERT INTO target_table SELECT * FROM temp_import;

-- Cleanup
DROP TABLE temp_import;
```

## Troubleshooting Import Errors

### Common Import Errors and Solutions

| Error Type | Possible Causes | Solutions |
|----------|----------|----------|
| Field Mismatch | Number of columns in source data differs from target table | Check field mapping, adjust import settings or modify table structure |
| Data Type Conflicts | Source data values incompatible with target column types | Use data type conversions, or clean data before importing |
| Unique Constraint Violations | Import data violates unique key/primary key constraints | Check for duplicate values in source data, decide whether to update existing records |
| Foreign Key Constraint Failures | Import data references non-existent foreign key values | Import reference tables first, or temporarily disable foreign key constraints |
| Encoding Issues | Character encoding mismatch | Ensure correct file encoding settings, consider preprocessing conversions |

### Diagnosing Import Problems

1. **Enable Detailed Logging**:
   - Choose verbose logging options in the Import Wizard
   - Or set more detailed logging in postgresql.conf

2. **Check PostgreSQL Server Logs**:
   - Right-click the connection in Navicat and select "Server Logs"
   - Or check the server log file location:
     - Windows: `C:\Program Files\PostgreSQL\15\data\log\`
     - Linux: `/var/log/postgresql/`

3. **Try Importing a Smaller Data Sample**:
   - Extract a small number of records from your source data (e.g., 10-100 rows)
   - Try importing to identify specific issues

## Performance Optimization Tips

### Tips to Increase Import Speed

1. **Optimize Indexes**:
   - Remove indexes before importing, recreate them afterward:
   ```sql
   -- Record existing indexes (needs to be created first)
   SELECT 'CREATE INDEX ' || indexname || ' ON ' || tablename || ' USING ' || indexdef || ';'
   FROM pg_indexes
   WHERE tablename = 'your_table';
   
   -- Drop indexes
   DROP INDEX index_name;
   
   -- Import data...
   
   -- Recreate indexes
   CREATE INDEX index_name ON your_table(column);
   ```

2. **Use UNLOGGED Tables**:
   - For temporary imports, use UNLOGGED tables to reduce WAL generation:
   ```sql
   CREATE UNLOGGED TABLE staging_table (LIKE target_table);
   
   -- Import into staging_table...
   
   -- Move data when complete
   INSERT INTO target_table SELECT * FROM staging_table;
   ```

3. **Adjust Server Parameters**:
   - Increase work memory:
   ```sql
   SET work_mem = '256MB';
   ```
   - Disable WAL archiving (test environments only):
   ```sql
   SET wal_level = 'minimal';
   ```

### Parallel Processing

Leverage PostgreSQL's parallel capabilities:

1. **Parallel Data Loading**:
   - Partition your dataset
   - Use multiple connections to import different partitions in parallel
   - Use the WORKERS option with COPY command (PostgreSQL 14+):
   ```sql
   COPY large_table FROM 'large_file.csv' WITH (FORMAT csv, WORKERS 4);
   ```

2. **Using Foreign Data Wrappers for Advanced Imports**:
   - Set up file_fdw:
   ```sql
   CREATE EXTENSION file_fdw;
   CREATE SERVER file_server FOREIGN DATA WRAPPER file_fdw;
   ```
   - Create a foreign table:
   ```sql
   CREATE FOREIGN TABLE ext_table (
     /* column definitions */
   ) SERVER file_server
   OPTIONS (filename '/path/to/file.csv', format 'csv', header 'true');
   ```
   - Import from the foreign table:
   ```sql
   INSERT INTO target_table SELECT * FROM ext_table;
   ```

## Next Steps

After successfully importing data, it's recommended to:

1. **Verify the Imported Data**:
   - Check if the record count matches expectations
   - Verify data integrity and consistency

2. **Optimize the Database**:
   - Rebuild indexes
   - Run ANALYZE to update statistics
   - Perform VACUUM operations as needed

3. **Continue Learning Other Features**:
   - [Browse the Imported Data](../Browsing_Data/README.md)
   - [Run SQL Queries](../Running_SQL_Queries/README.md)
   - [Export Data](../Exporting_Data/README.md)
   - [Database Administration](../Database_Administration/README.md) 