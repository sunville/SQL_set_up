# Running SQL Queries in PostgreSQL

[← Back to PostgreSQL Database Tutorial](../PostgreSQL%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions on how to write and execute SQL queries in a PostgreSQL database using Navicat, from basic queries to advanced techniques and performance optimization.

## Table of Contents
- [Navicat Query Interface](#navicat-query-interface)
- [Basic SQL Queries](#basic-sql-queries)
- [Advanced Query Techniques](#advanced-query-techniques)
- [Query Performance Optimization](#query-performance-optimization)
- [Query Management and Organization](#query-management-and-organization)
- [Stored Procedures and Functions](#stored-procedures-and-functions)
- [PL/pgSQL Script Writing](#plpgsql-script-writing)
- [Query Troubleshooting](#query-troubleshooting)

## Navicat Query Interface

### Creating and Opening Queries

1. **Create a New Query**:
   - Click the "Query" button on the toolbar
   - Or select from the menu: Query → New Query
   - Or use the shortcut Ctrl+Q
   
   ![New Query](images/new_query.png)

2. **Query Editor Interface**:
   - **Editing Area**: The main area for writing SQL code
   - **Result Grid**: The area displaying query results
   - **Information Panel**: Displays query execution information and statistics
   - **Toolbar**: Buttons for common functions (execute, stop, format, etc.)

3. **Connection Selection**:
   - Ensure the correct PostgreSQL connection is selected from the dropdown menu
   - You can switch the connection and database in use

### Editor Features

1. **Code Editing Features**:
   - **Syntax Highlighting**: Different colors for SQL keywords, table names, functions, etc.
   - **Auto-completion**: Suggestions for table names, column names, and keywords as you type
   - **Code Folding**: Collapse SQL statement blocks for easier management of long queries
   - **Bracket Matching**: Highlight matching brackets
   - **Line Numbers**: Display line numbers for easy reference

2. **Code Assistance**:
   - **Table Designer Integration**: Right-click a table name and select "Design Table" to view its structure
   - **SQL Snippets**: Save and insert frequently used code snippets
   - **SQL Formatting**: Click the format button to automatically beautify SQL code
   - **SQL Builder**: Use a visual interface to build queries

## Basic SQL Queries

### SELECT Queries

1. **Simple SELECT Queries**:
