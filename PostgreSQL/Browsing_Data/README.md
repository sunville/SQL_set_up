# Browsing PostgreSQL Data with Navicat

[← Back to PostgreSQL Database Tutorial](../PostgreSQL%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions on how to efficiently browse, navigate, and view data in PostgreSQL databases using Navicat, including basic and advanced data browsing techniques.

## Table of Contents
- [Database Object Navigation](#database-object-navigation)
- [Table Data Viewing](#table-data-viewing)
- [Advanced Filtering and Sorting](#advanced-filtering-and-sorting)
- [Data Visualization](#data-visualization)
- [Data Relationship Browsing](#data-relationship-browsing)
- [Custom Data Views](#custom-data-views)
- [Search and Replace](#search-and-replace)
- [Exporting View Results](#exporting-view-results)
- [Performance Optimization Tips](#performance-optimization-tips)

## Database Object Navigation

### Using Object Browser

After connecting to a PostgreSQL database, Navicat's left panel displays the Object Browser to help you navigate the database structure:

1. **Expand Connection**: Double-click the connection or click the expand icon (+) next to the connection.
   
   ![Expand Connection](images/expand_connection.png)

2. **Browse Object Types**: After expanding, you can see objects organized by type:
   - Databases
   - Schemas (default is public)
   - Tables
   - Views
   - Functions
   - Stored Procedures
   - Sequences
   - Types (PostgreSQL custom types)
   - Rules
   - Triggers
   - Foreign Tables
   - Materialized Views
   - Domains
   
3. **Search for Objects**:
   - Use the search box at the top of the Object Browser to quickly find specific objects
   - Supports wildcards (e.g., "user*" to find all objects starting with "user")

4. **Object Properties**:
   - Right-click any object to view available actions
   - Select "Properties" to view detailed object information
   - Use the "Properties" button on the toolbar (or press F6)

### Managing Database Connections

1. **Create Connection Groups**:
   - Right-click on blank space in the connection window, select "New Group"
   - Create logical groups for different projects or environments
   - Drag and drop connections into appropriate groups

2. **Connection Color Coding**:
   - Right-click a connection, select "Edit Connection"
   - In the General tab, click the color square to set connection color
   - Use different colors for production/development environments to enhance visual distinction

3. **Favorites and Recently Used Objects**:
   - Right-click frequently used objects, select "Add to Favorites"
   - Access them using the "Favorites" in Navicat's top menu
   - Use "Recently Opened Objects" to quickly return to previously viewed items

## Table Data Viewing

### Basic Table Data Viewing

1. **Open Table Data**:
   - In the Object Browser, expand the "Tables" node
   - Double-click a table name or right-click the table name and select "Open Table"
   - Or select a table and click the "Table" button in the toolbar

2. **Table Design**:
   - Switch to the "Design" tab to view table structure
   - View column names, data types, default values, and constraints
   - View indexes, foreign keys, and other table properties

3. **Table Data Grid**:
   - View table data in the "Data" tab
   - Data is displayed as an editable grid
   - View record count and other statistics at the bottom of the grid

   ![Table Data View](images/table_data_view.png)

4. **Customize Grid Display**:
   - Right-click column headers to adjust display options
   - Reorder columns (drag and drop column headers)
   - Adjust column widths (drag column boundaries)
   - Hide/show columns (right-click column header and select "Hide Column")

### Data Navigation Techniques

1. **Pagination Browsing**:
   - Use pagination controls at the bottom to browse large tables
   - Set page size (rows per page)
   - Jump to specific page numbers

2. **Limit Result Sets**:
   - Use the "Limit" option to control the number of records displayed
   - For large tables, appropriate limits can improve performance
   - Select the "Maximum Rows" option when opening table data

3. **Quickly Locate Records**:
   - Ctrl+G: Jump to a specific row
   - Use the "Find" function (Ctrl+F) to search for values
   - Use the "Locate" function to quickly find matching rows

4. **Data Column Type Recognition**:
   - Navicat provides different column editors based on data types
   - Display formats for different data types can be customized
   - Right-click column header, select "Display Format" to customize display method

## Advanced Filtering and Sorting

### Data Filtering

1. **Using Filters**:
   - Click the filter icon in the toolbar (funnel shape)
   - Or right-click the table area and select "Filter"
   - Enter filter conditions

   ![Filter Data](images/filter_data.png)

2. **Custom Filter Conditions**:
   - Simple filtering: directly enter values in the filter row
   - Advanced filtering: click the filter button (arrow) on columns
   - Select predefined filter conditions (equals, contains, begins with, etc.)
   - Combine multiple column filters

3. **Using SQL Filtering**:
   - Click the "SQL Filter" button
   - Enter a custom WHERE clause (no need to include the WHERE keyword)
   - Example: `column1 = 'value' AND column2 > 100`

4. **Save Filters**:
   - Save frequently used filters after creation
   - Click the "Save Current Filter" button
   - Name and save the current filter conditions
   - Use the "Load Filter" button to apply saved filters

### Data Sorting

1. **Basic Sorting**:
   - Click a column header for ascending sort
   - Click again to toggle to descending
   - Arrows on column headers indicate sort direction

2. **Multi-column Sorting**:
   - Hold the Shift key and click multiple column headers
   - The first column clicked becomes the primary sort column
   - Or use the "Sort" dialog to set complex sorting

3. **Custom Sorting**:
   - Click the Sort button in the toolbar
   - Select columns and order (ascending/descending)
   - Add multiple sort rules
   - Set sort priority (drag rules to adjust order)

4. **Save Sort Settings**:
   - Save frequently used sort configurations
   - Click the "Save Current Sort" button
   - Name your sort configuration
   - Use "Load Sort" to apply saved sort settings

## Data Visualization

### Chart View

Navicat provides charting functionality to help visualize table data:

1. **Create Charts**:
   - In the table's "Data" tab, click the "Chart" button in the bottom right
   - Or right-click the table and select "Chart"
   - Select chart type (bar chart, line chart, pie chart, etc.)

   ![Chart View](images/chart_view.png)

2. **Configure Chart Data**:
   - Select fields to use in the chart
   - Set dimensions (X-axis) and measures (Y-axis)
   - Configure filter conditions
   - Set aggregation functions (count, sum, average, etc.)

3. **Customize Chart Appearance**:
   - Adjust chart title and style
   - Change color theme
   - Set legend position
   - Configure axis labels and formats

4. **Interactive Charts**:
   - Hover to view data point details
   - Zoom and pan charts
   - Highlight specific data series
   - Export charts as images

### JSON and XML Data Visualization

PostgreSQL provides good support for JSON and XML types, and Navicat offers special viewing tools:

1. **JSON Data Viewing**:
   - JSON column values are displayed as collapsible tree structures
   - Click to expand/collapse different levels of JSON structure
   - Right-click JSON data and select "Open with JSON Editor" for more powerful features

2. **XML Data Viewing**:
   - XML data also has a dedicated viewer
   - Supports syntax highlighting and formatting
   - Provides tree view navigation

## Data Relationship Browsing

### Navigating with Foreign Keys

Leverage foreign key relationships defined in PostgreSQL to navigate between related tables:

1. **View Related Records**:
   - In the table data view, right-click a cell containing a foreign key
   - Select "View Foreign Key"
   - Navicat automatically opens the related table and displays the associated records

2. **ER Diagram Navigation**:
   - Click the "ER Diagram" button on the toolbar
   - Create or open an ER diagram containing related tables
   - Visually understand relationships between tables through relationship lines
   - Double-click tables to view data directly in the diagram

### Using Master-Detail View

1. **Set Up Master-Detail View**:
   - Select two or more tables with relationships
   - Right-click and select "Master-Detail View"
   - Set up master table and detail table relationships

2. **Browse Data with Master-Detail View**:
   - After selecting a master table record, related detail table records are automatically displayed below
   - Supports multi-level master-detail relationships
   - Convenient for viewing and editing data with parent-child relationships

## Custom Data Views

### Creating Custom Views with Queries

1. **Create New Query**:
   - Click the "Query" button in the toolbar
   - Write an SQL query, for example:
   ```sql
   SELECT 
       c.customer_id,
       c.name AS customer_name,
       o.order_id,
       o.order_date,
       p.product_name,
       oi.quantity,
       oi.price
   FROM 
       customers c
   JOIN 
       orders o ON c.customer_id = o.customer_id
   JOIN 
       order_items oi ON o.order_id = oi.order_id
   JOIN 
       products p ON oi.product_id = p.product_id
   ORDER BY 
       c.name, o.order_date DESC;
   ```
   - Execute the query to view results

2. **Save Query**:
   - Click the "Save" button
   - Enter a name and description
   - Save the query under the appropriate connection

3. **Create View**:
   - Right-click on query results and select "Create View"
   - Or directly create a view in the database:
   ```sql
   CREATE VIEW customer_orders AS
   SELECT /* query content */;
   ```
   - The view will appear under the "Views" node in the Object Browser

### Using Custom Field Settings

1. **Customize Data Grid**:
   - Right-click the data grid area and select "Settings"
   - Configure grid properties, row height, and column width

2. **Set Field Display Format**:
   - Right-click column header and select "Display Format"
   - Set formats for different data types:
     - Date/Time: Set date format (e.g., YYYY-MM-DD)
     - Numbers: Set decimal places, thousand separators, etc.
     - Boolean: Set display text (e.g., "Yes/No" instead of 1/0)

3. **Set Conditional Formatting**:
   - Right-click the data grid and select "Conditional Format"
   - Set rules to apply different formats based on cell values
   - For example, set a red background for negative values

## Search and Replace

### Finding Data

1. **Basic Find**:
   - Press Ctrl+F to open the find dialog
   - Enter search text
   - Set search options (e.g., case sensitive, whole word match)
   - Click "Find Next" to search one by one

2. **Advanced Find**:
   - Use regular expressions for complex searches
   - Select specific columns to search
   - Limit search scope (current selection, current page, or all records)

### Replacing Data

1. **Search and Replace**:
   - Press Ctrl+H to open the replace dialog
   - Enter search text and replacement text
   - Configure search options
   - Choose "Replace All" or "Replace One by One"

2. **View Modifications**:
   - Replaced cells are highlighted to show modifications
   - Use the "Commit" button to save changes
   - Or use "Undo" to cancel modifications

## Exporting View Results

### Exporting Table Data

1. **Export Data**:
   - Right-click the table or query results and select "Export Wizard"
   - Select export format (CSV, Excel, JSON, HTML, etc.)
   - Configure export options

2. **Quick Export of Selected Data**:
   - Select rows and columns to export
   - Right-click and select "Export Selected Data"
   - Choose export format and destination

3. **Copy to Clipboard**:
   - Select cells to copy
   - Press Ctrl+C to copy to clipboard
   - Paste directly into Excel or other applications

### Printing Table Data

1. **Print Preview**:
   - Right-click the data grid and select "Print"
   - Or press Ctrl+P
   - Check the print preview

2. **Custom Print Settings**:
   - Configure page orientation and margins
   - Add headers and footers
   - Set print options (such as grid lines)

## Performance Optimization Tips

### Optimizing Browsing for Large Tables

1. **Pagination Loading**:
   - Use pagination controls to browse large tables
   - Increase "Records per Page" to reduce page switching
   - Avoid loading all data at once

2. **Limit Initial Loading**:
   - Set the "Limit" option when opening tables
   - For example, only load the first 1000 rows
   - Use filters to narrow down result sets

3. **Filter Using Indexed Columns**:
   - Prioritize filtering on columns with indexes
   - PostgreSQL query execution will be faster
   - View the table's "Indexes" tab to understand available indexes

4. **Disable Auto-Refresh**:
   - Right-click the data grid and select "Settings"
   - Disable the auto-refresh option
   - Reduce unnecessary data reloading

### Query Execution Optimization

1. **Using EXPLAIN**:
   - Add EXPLAIN ANALYZE before your query
   - Analyze the query execution plan
   - Identify potential performance bottlenecks

   ```sql
   EXPLAIN ANALYZE
   SELECT * FROM large_table WHERE column = 'value';
   ```

2. **Create Views**:
   - Create views for complex, frequently used queries
   - Reduce the need to repeatedly write complex queries
   - Improve efficiency when browsing complex data structures

## Advanced Browsing Techniques

### Using Data Printer

1. **Custom Reports**:
   - Select "Tools" > "Data Printer"
   - Design custom tabular reports
   - Add fields, calculations, and formatting

2. **Generate Reports**:
   - Run Data Printer reports
   - Preview generated reports
   - Export to PDF or print reports

### Data Comparison and Synchronization

1. **Compare Table Data**:
   - Select two tables, right-click and select "Compare Data"
   - Set corresponding fields
   - View differences

2. **Synchronize Data**:
   - After comparison, select differences to synchronize
   - Generate synchronization script
   - Execute synchronization operation

## Next Steps

Now that you've mastered how to browse and view PostgreSQL data, you may want to learn how to:

- [Run SQL Queries](../Running_SQL_Queries/README.md)
- [Export Data](../Exporting_Data/README.md)
- [Database Administration](../Database_Administration/README.md) 