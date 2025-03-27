# Browsing Data in SQLite with Navicat

[← Back to SQLite Database Tutorial](../SQLite%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions on browsing, navigating, and visualizing data in SQLite databases using Navicat, including customizing data views, filtering, and data exploration techniques.

## Table of Contents
- [Database Object Navigation](#database-object-navigation)
- [Table Data Viewing](#table-data-viewing)
- [Filtering and Sorting Data](#filtering-and-sorting-data)
- [Data Visualization](#data-visualization)
- [Working with Special Data Types](#working-with-special-data-types)
- [Custom Data Views](#custom-data-views)
- [Data Grid Customization](#data-grid-customization)
- [Searching and Finding Data](#searching-and-finding-data)
- [Data Relationships and Navigation](#data-relationships-and-navigation)
- [Performance Optimization for Large Tables](#performance-optimization-for-large-tables)

## Database Object Navigation

### Using the Navicat Object Browser

The Object Browser in Navicat is your primary tool for navigating database objects:

1. **Connecting to Your Database**:
   - Double-click your SQLite connection in the left panel
   - View all database objects in the expanded connection tree
   - Objects are organized by type (Tables, Views, Functions, etc.)
   
   ![Object Browser](images/object_browser.png)

2. **Browsing Object Types**:
   - **Tables**: Core data storage objects
   - **Views**: Stored queries that function as virtual tables
   - **Triggers**: Automated procedures that run on data changes
   - **Indexes**: Data structures that improve query performance
   - **Functions**: Custom functions (if supported)

3. **Using Search and Filters**:
   - Use the search box at the top of the Object Browser
   - Filter objects by name or type
   - Use wildcards for more flexible searching (e.g., "user*" for all objects starting with "user")

4. **Object Properties**:
   - Right-click any object to access its properties menu
   - View creation SQL, design information, and metadata
   - Access object-specific actions (open, design, etc.)

### Object Shortcuts and Navigation Tips

1. **Keyboard Navigation**:
   - Use arrow keys to navigate the object tree
   - Press Enter to open the selected object
   - Use keyboard shortcuts for common actions
     - F6: Object Properties
     - F7: Object Designer
     - Ctrl+T: New Query
     - Ctrl+N: New Table

2. **Recently Used Objects**:
   - Find recently accessed objects in the **History** panel
   - View connection history and recently executed queries
   - Pin frequently used objects for persistent access

3. **Creating Object Groups**:
   - Organize related tables into logical groups
   - Right-click in the Object Browser and select **New Group**
   - Drag and drop objects into groups for better organization

## Table Data Viewing

### Basic Table Data Viewing

1. **Opening Tables**:
   - Double-click on any table in the Object Browser
   - Or right-click the table and select **Open Table**
   - The table data appears in the data grid on the right
   
   ![Table Data View](images/table_data_view.png)

2. **Understanding the Data Grid**:
   - Rows represent individual records
   - Columns represent fields/attributes
   - Primary key columns often highlighted
   - NULL values displayed distinctively

3. **Grid Navigation**:
   - Use scrollbars to navigate through data
   - Use arrow keys to move between cells
   - Click on column headers to sort
   - Use Tab/Shift+Tab to move between editable cells

4. **Data Grid Information**:
   - View row count at the bottom of the grid
   - See current record position and navigation controls
   - Record and field selection information

### Table Structure and Design

1. **Viewing Table Structure**:
   - Click on the **Table Designer** tab
   - View column definitions, data types, and constraints
   - See index information and relationships

2. **Examining Table Properties**:
   - Right-click the table and select **Properties**
   - View creation SQL
   - Get table statistics (size, row count, etc.)
   - Check storage information

## Filtering and Sorting Data

### Basic Filtering

1. **Quick Filters**:
   - Click the filter icon in a column header
   - Choose from predefined filter conditions
   - Select values from the dropdown list
   
   ![Quick Filter](images/quick_filter.png)

2. **Custom Filters**:
   - Choose "Custom" from the filter dropdown
   - Build complex filter conditions
   - Combine multiple criteria using AND/OR operators
   
   ```
   (age > 30 AND department = 'Sales') OR (status = 'VIP')
   ```

3. **Text Filters**:
   - Contains, Begins with, Ends with
   - Equals, Not equals
   - Regular expression matching (if supported)

4. **Numeric and Date Filters**:
   - Equal to, Greater than, Less than
   - Between two values
   - In the last X days/months
   - Custom date ranges

### Advanced Filtering

1. **SQL Filter**:
   - Click the SQL Filter button in the toolbar
   - Write a custom WHERE clause
   - Apply complex filtering logic
   
   ```sql
   (salary BETWEEN 50000 AND 100000) AND 
   (hire_date > date('now', '-5 years')) AND
   (department_id IN (2, 3, 5))
   ```

2. **Saving Filters**:
   - Save frequently used filters
   - Load saved filters from the dropdown menu
   - Apply consistent filtering across sessions

3. **Combining Filters and Sorting**:
   - Apply multiple column filters simultaneously
   - Sort the filtered results by clicking column headers
   - Create multi-level sorting by holding Shift while clicking columns

### Sorting Data

1. **Simple Sorting**:
   - Click any column header to sort ascending
   - Click again to sort descending
   - Current sort column and direction is indicated

2. **Multi-column Sorting**:
   - Hold Shift and click additional column headers
   - Establish sort priority (primary, secondary, tertiary)
   - View sort order indicators on column headers

3. **Custom Sort Order**:
   - Right-click and choose "Sort" or click the Sort button
   - Add multiple columns with ascending/descending directions
   - Set exact sort priority and save custom sort settings

## Data Visualization

### Visual Data Presentation

1. **Formatting the Data Grid**:
   - Right-click and select "Grid View"
   - Customize colors, fonts, and styles
   - Format specific columns based on data type
   - Use conditional formatting for data highlighting

2. **Data Formatting Options**:
   - Numeric: Currency, percentage, decimal places
   - Dates: Various date/time formats
   - Text: Case, trimming, substring display
   - Boolean: Custom true/false representations

### Charts and Graphs

1. **Creating Charts**:
   - Select "Create Chart" from the toolbar
   - Choose chart type (bar, line, pie, etc.)
   - Configure data source columns
   
   ![Chart View](images/chart_view.png)

2. **Chart Configuration**:
   - Set X and Y axis data
   - Configure grouping and aggregation
   - Customize chart appearance and labels
   - Apply filters to chart data

3. **Saving and Exporting Charts**:
   - Save chart definitions for future use
   - Export charts as images
   - Print chart views
   - Include in reports or documentation

## Working with Special Data Types

### BLOB Data

SQLite supports Binary Large Objects (BLOBs) for storing binary data:

1. **Viewing BLOB Data**:
   - BLOB cells display a special indicator
   - Double-click to open the BLOB viewer
   - View as hex, text, image, or other formats

2. **BLOB Operations**:
   - Import data from files
   - Export BLOBs to files
   - View and edit binary content
   - Convert between representations

### JSON Data

While SQLite doesn't have a native JSON type, Navicat provides tools for working with JSON stored in TEXT fields:

1. **JSON Viewing**:
   - Right-click on JSON data and select "Open with JSON Viewer"
   - View formatted JSON with collapsible nodes
   - Navigate complex JSON structures

2. **JSON Path Expressions**:
   - Query JSON content using JSON path
   - Extract specific elements from JSON
   - Filter based on JSON content when the JSON1 extension is enabled:
   
   ```sql
   SELECT * FROM products 
   WHERE json_extract(attributes, '$.color') = 'red'
   ```

### Date and Time Data

SQLite's date and time handling has some unique aspects:

1. **Date/Time Viewing**:
   - View dates in various formats
   - Configure date display preferences
   - Convert between different date formats

2. **Date Calculations**:
   - Use SQLite's date functions in filters
   - Filter by date ranges
   - Calculate date differences

## Custom Data Views

### Creating Custom Views

1. **SQL Views**:
   - Create a new view using the "New View" option
   - Define SQL SELECT statement for the view
   - Save as a permanent database object
   
   ```sql
   CREATE VIEW active_customers AS
   SELECT * FROM customers 
   WHERE last_order_date > date('now', '-1 year')
   AND status = 'active';
   ```

2. **Using Views for Data Browsing**:
   - Open views just like tables
   - Apply filters and sorting to view data
   - Views automatically reflect current data

### Query Results as Data Views

1. **Saved Queries**:
   - Create a new query
   - Write a SELECT statement with joins, filtering, etc.
   - Save the query for future use

2. **Working with Query Results**:
   - Run the query to view results in data grid
   - Apply additional sorting and filtering to results
   - Export or print result sets

### Master-Detail Views

1. **Setting Up Master-Detail View**:
   - Right-click on a table and select "New Master-Detail View"
   - Configure master table and related detail table(s)
   - Set up the relationship between tables
   
   ![Master-Detail View](images/master_detail_view.png)

2. **Using Master-Detail View**:
   - Select a record in the master table
   - View related records in the detail pane
   - Navigate through parent records to see children
   - Edit data in either master or detail panes

## Data Grid Customization

### Column Layout and Visibility

1. **Adjusting Column Width**:
   - Drag column dividers to resize
   - Double-click divider for "autofit"
   - Right-click header and select "Column Width"

2. **Column Visibility**:
   - Right-click on column header and select "Hide Column"
   - Use "Show Columns" to control multiple column visibility
   - Restore hidden columns when needed

3. **Column Order**:
   - Drag and drop column headers to reorder
   - Right-click and use "Set Column Order"
   - Save custom column arrangements

### Display Formatting

1. **Grid Display Options**:
   - Right-click grid and select "Grid Display Options"
   - Configure grid lines, alternating row colors
   - Set text alignment and display characteristics

2. **Value Formatting**:
   - Format numeric values (decimal places, thousands separators)
   - Format dates according to regional settings
   - Set display format for specific data types

3. **Conditional Formatting**:
   - Highlight cells based on values
   - Set color rules for different conditions
   - Create visual indicators for important data
   
   Example: Highlight all values greater than 1000 in red

### Saving Grid Configurations

1. **Saving Layouts**:
   - Save column widths, order, and visibility
   - Store filter and sort settings
   - Create named layouts for different purposes

2. **Loading Layouts**:
   - Apply previously saved layouts
   - Quickly switch between different views
   - Share layouts with team members

## Searching and Finding Data

### Quick Search

1. **Using Quick Search**:
   - Press Ctrl+F to open Quick Search
   - Type search term to highlight matches
   - Navigate between matches with next/previous buttons
   
   ![Quick Search](images/quick_search.png)

2. **Search Options**:
   - Case sensitive matching
   - Whole word only
   - Regular expression support
   - Search direction (up/down)

### Find and Replace

1. **Finding Data**:
   - Open Find and Replace dialog (Ctrl+H)
   - Enter search criteria and options
   - Find all occurrences in the table

2. **Replacing Data**:
   - Enter replacement text
   - Preview changes before applying
   - Replace individual or all matches
   - Track replacement history

### Advanced Find

1. **Multi-column Search**:
   - Search across multiple columns
   - Specify which columns to include
   - Apply different criteria per column

2. **Results Management**:
   - View all matches in a separate grid
   - Export search results
   - Apply additional filters to found data

## Data Relationships and Navigation

### Navigating Related Data

1. **Using Foreign Key Information**:
   - Right-click on a foreign key cell
   - Select "View Reference Data"
   - Jump directly to the related record
   
   ![View References](images/view_references.png)

2. **Viewing Foreign Key Data**:
   - View referenced table data alongside the current table
   - See parent-child relationships
   - Navigate back and forth between related records

### ER Diagrams for Visual Navigation

1. **Creating ER Diagrams**:
   - Create a new ER Diagram
   - Add tables to the diagram
   - View relationships visually

2. **Navigating via ER Diagram**:
   - Click on tables in the diagram to view data
   - Follow relationship lines to related tables
   - Understand database structure visually

### Tracking Data Relationships

1. **View Dependencies**:
   - Right-click table and select "Dependencies"
   - See all objects dependent on the selected table
   - View impact of potential changes

2. **Reverse Engineer Database**:
   - Create a complete model of existing database
   - Visualize all tables and relationships
   - Document database structure

## Performance Optimization for Large Tables

### Handling Large Datasets

1. **Limiting Data Loading**:
   - Set row limit in Preferences
   - Use "Load X Records" option
   - Apply filters before loading data

2. **Paged Data Browsing**:
   - Use the pager controls to navigate large datasets
   - Jump to specific page or record
   - Set page size based on performance needs

### Query Optimization for Browsing

1. **Optimized Table Opening**:
   - Create targeted queries instead of "SELECT *"
   - Include only necessary columns
   - Apply essential filters at query time

2. **Indexed Searches**:
   - Take advantage of table indexes
   - Create indexes for frequently searched columns
   - Check query execution plans for performance

### Memory Management

1. **Efficient Data Loading**:
   - Load data in smaller chunks
   - Close unused tables and queries
   - Monitor memory usage in large operations

2. **Cache Management**:
   - Configure Navicat cache settings
   - Adjust SQLite cache size for performance
   - Clear cache when necessary

## Next Steps

Now that you're comfortable with browsing data in your SQLite database, you can:

- [Run SQL queries to analyze your data](../Running_SQL_Queries/README.md)
- [Export your data in various formats](../Exporting_Data/README.md)
- [Manage and administer your database](../Database_Administration/README.md)

For more information about SQLite and Navicat, refer to the [official SQLite documentation](https://www.sqlite.org/docs.html) and [Navicat documentation](https://www.navicat.com/en/support/navicat-for-sqlite-manual). 