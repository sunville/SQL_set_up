# Loading Data into MySQL with Navicat

[← Back to MySQL Database Tutorial](../MySQL%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions for importing and loading data into MySQL databases using Navicat. Efficiently loading data is a critical skill for database management, and Navicat offers multiple methods for importing data from various sources.

## Table of Contents
- [Import Methods Overview](#import-methods-overview)
- [Importing from CSV Files](#importing-from-csv-files)
- [Importing from Excel Files](#importing-from-excel-files)
- [Importing from SQL Files](#importing-from-sql-files)
- [Importing from Other Databases](#importing-from-other-databases)
- [Using the Data Transfer Tool](#using-the-data-transfer-tool)
- [Handling Large Datasets](#handling-large-datasets)
- [Troubleshooting Import Issues](#troubleshooting-import-issues)

## Import Methods Overview

Navicat provides several methods to import data:

1. **Import Wizard**: A step-by-step guide for importing from files
2. **Execute SQL File**: Run SQL scripts containing data insertion commands
3. **Data Transfer**: Copy data between different database connections
4. **Batch Import**: Import multiple files at once

The method you choose depends on:
- Data source format (CSV, Excel, SQL, etc.)
- Data volume
- Need for data transformation during import
- Whether the target table already exists

## Importing from CSV Files

CSV (Comma-Separated Values) files are one of the most common formats for data exchange.

### Step 1: Prepare Your CSV File

For best results:
- Ensure the first row contains column names
- Check that delimiters are consistent (commas, semicolons, or tabs)
- Verify date format consistency
- Remove any extraneous data or comments

### Step 2: Launch Import Wizard

1. Connect to your MySQL server in Navicat
2. Right-click on the target database
3. Select "Import Wizard" from the context menu

![Import Wizard Menu](images/import_wizard_menu.png)

### Step 3: Select Import Mode and File

1. Choose "Import from a file"
2. Select "CSV" as the file format
3. Click "Browse" and locate your CSV file
4. Click "Next"

![CSV Import Source](images/csv_import_source.png)

### Step 4: Configure Import Settings

1. **General Options**:
   - Select field delimiter (comma, tab, semicolon, etc.)
   - Choose text qualifier (quote character)
   - Select file encoding (UTF-8, ASCII, etc.)
   - Check "First Line Contains Field Names" if applicable

2. **Additional Options**:
   - Date Format: Specify date format in your data
   - Decimal Symbol: Period or comma
   - Trim Spaces: Remove leading/trailing spaces from fields

![CSV Import Settings](images/csv_import_settings.png)

### Step 5: Select Destination Table

1. Choose one of the following options:
   - **Create new table**: Create a new table using the imported data
   - **Append data to an existing table**: Add records to an existing table
   - **Replace existing table**: Drop and recreate the table

2. If creating a new table:
   - Enter a name for the new table
   - Modify field names if needed
   - Set appropriate data types for each column
   - Define primary key(s)

![CSV Destination Table](images/csv_destination_table.png)

### Step 6: Map Fields

1. Review the field mapping between source and destination
2. Adjust mappings if needed:
   - Exclude columns by unchecking them
   - Change target field names
   - Modify data types or column properties

![CSV Field Mapping](images/csv_field_mapping.png)

### Step 7: Start Import

1. Review the import summary
2. Click "Start" to begin the import process
3. Monitor the progress bar
4. Review results and any error messages

![CSV Import Progress](images/csv_import_progress.png)

## Importing from Excel Files

Navicat supports direct import from Microsoft Excel files (XLS and XLSX formats).

### Step 1: Prepare Your Excel File

For optimal results:
- Format data as a proper table
- Include column headers in the first row
- Remove any charts, images, or merged cells
- Ensure data consistency in each column

### Step 2: Launch Import Wizard

1. Right-click on the target database
2. Select "Import Wizard"
3. Choose "Import from a file"
4. Select "Excel" as the file format
5. Browse to your Excel file
6. Click "Next"

### Step 3: Configure Excel Import Settings

1. Select the worksheet to import from the dropdown
2. Choose whether the first row contains field names
3. Specify the cell range (optional)
4. Click "Next"

![Excel Import Settings](images/excel_import_settings.png)

### Step 4: Follow Table Creation and Field Mapping

Follow the same process as CSV imports for:
- Creating or selecting the destination table
- Mapping fields between source and destination
- Setting data types and column properties
- Starting the import process

## Importing from SQL Files

SQL files containing INSERT, CREATE TABLE, or other statements can be imported directly.

### Method 1: Execute SQL File

For SQL dump files or script files:

1. Connect to your target database
2. Click the "Query" button in the toolbar
3. In the Query window, click "File" > "Open SQL File"
4. Navigate to and select your SQL file
5. Click "Run" to execute the entire script

![Execute SQL File](images/execute_sql_file.png)

### Method 2: Using Import Wizard for SQL Files

1. Right-click on the database
2. Select "Import Wizard"
3. Choose "Import from a file"
4. Select "SQL" as the file format
5. Browse to your SQL file
6. Configure execution options:
   - Continue on error
   - Run multiple queries
   - Set client character set
7. Click "Start" to execute the SQL file

## Importing from Other Databases

Navicat allows direct data transfer between different database connections.

### Using Data Transfer Tool

1. In the main menu, click "Tools" > "Data Transfer"
2. Configure the source connection and objects:
   - Select source connection
   - Choose database objects to transfer (tables, views, etc.)
   
3. Configure the target connection:
   - Select target connection
   - Choose destination database
   
4. Set transfer options:
   - Table creation options (Create, Create if not exists, etc.)
   - Index creation options
   - Include foreign keys
   - Transfer data options (Insert, Update if exists, etc.)
   
5. Click "Start" to begin the transfer

![Data Transfer Tool](images/data_transfer_tool.png)

## Using the Batch Import Tool

For importing multiple files at once:

1. In the main menu, select "Tools" > "Batch Import"
2. Click "Add Files" to select multiple CSV or Excel files
3. Configure import settings for each file:
   - Source file format and settings
   - Target database and table
   - Field mappings
4. Click "Start" to process all imports sequentially

## Handling Large Datasets

When importing large volumes of data:

### Performance Tips

1. **Disable Indexes Before Import**:
   ```sql
   ALTER TABLE your_table DISABLE KEYS;
   -- Import data
   ALTER TABLE your_table ENABLE KEYS;
   ```

2. **Use Batch Import**:
   - Configure a larger "Commit records" value (5000-10000)
   - This reduces transaction overhead

3. **Disable Foreign Key Checks**:
   ```sql
   SET FOREIGN_KEY_CHECKS=0;
   -- Import data
   SET FOREIGN_KEY_CHECKS=1;
   ```

4. **Consider Server Load**:
   - Schedule large imports during off-peak hours
   - Monitor server resources during import

### Import in Chunks

For extremely large datasets:

1. Split your data file into smaller chunks
2. Import each chunk separately
3. Use transactions to ensure consistency

## Troubleshooting Import Issues

### Common Import Errors

1. **Data Type Mismatches**:
   - Error: "Incorrect integer value", "Incorrect datetime value"
   - Solution: Review data types in mapping step, clean source data

2. **Duplicate Primary Keys**:
   - Error: "Duplicate entry for key 'PRIMARY'"
   - Solution: Use REPLACE instead of INSERT, or handle duplicates

3. **Character Set Issues**:
   - Error: "Incorrect string value"
   - Solution: Ensure correct encoding selected, check for special characters

4. **Field Length Exceeded**:
   - Error: "Data too long for column"
   - Solution: Increase column size or truncate data during import

### Import Validation

After importing data:

1. **Verify Record Count**:
   ```sql
   SELECT COUNT(*) FROM imported_table;
   ```

2. **Check Sample Records**:
   ```sql
   SELECT * FROM imported_table LIMIT 10;
   ```

3. **Validate Critical Fields**:
   ```sql
   SELECT MIN(field), MAX(field), AVG(field) FROM imported_table;
   ```

4. **Look for Unexpected NULL Values**:
   ```sql
   SELECT COUNT(*) FROM imported_table WHERE important_field IS NULL;
   ```

## Next Steps

Now that you've successfully imported data into your MySQL database, you can:

- [Browse and explore your data](../Browsing_Data/README.md)
- [Run SQL queries on your imported data](../Running_SQL_Queries/README.md)
- [Export data to various formats](../Exporting_Data/README.md)
- [Perform database administration tasks](../Database_Administration/README.md)

For more information, refer to the [official Navicat documentation](https://www.navicat.com/en/support/navicat-for-mysql-manual). 