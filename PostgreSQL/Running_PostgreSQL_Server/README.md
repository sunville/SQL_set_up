# PostgreSQL Server Operation and Management Guide

[← Back to PostgreSQL Database Tutorial](../PostgreSQL%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions on how to start, stop, configure, and monitor a PostgreSQL server to ensure efficient and stable database system operation.

## Table of Contents
- [Starting and Stopping PostgreSQL Server](#starting-and-stopping-postgresql-server)
- [Server Configuration](#server-configuration)
- [Monitoring Server Status](#monitoring-server-status)
- [Performance Tuning](#performance-tuning)
- [Backup and Restore](#backup-and-restore)
- [Log Management](#log-management)
- [Common Troubleshooting](#common-troubleshooting)

## Starting and Stopping PostgreSQL Server

### Windows Environment

On Windows, PostgreSQL usually runs as a system service after installation.

**Using Service Manager:**
1. Open Service Manager: Press Win+R, type `services.msc`
2. Find "postgresql-x64-XX" (XX is the version number, e.g., 15)
3. Right-click the service and select "Start", "Stop", or "Restart"

**Using Command Line:**

### JSON Queries

1. **Basic JSON Operations**:

```sql
-- Create a table containing JSON data
CREATE TABLE product_details (
    product_id integer PRIMARY KEY,
    details jsonb
);

-- Insert JSON data
INSERT INTO product_details (product_id, details)
VALUES (
    1, 
    '{"name": "Laptop", "specs": {"ram": "16GB", "cpu": "i7", "storage": "512GB SSD"}, "tags": ["electronics", "computer"]}'
);

-- Access JSON object fields (using -> operator to get JSON value)
SELECT 
    product_id,
    details->'name' AS product_name,
    details->'specs'->'ram' AS ram
FROM 
    product_details;

-- Access JSON object fields (using ->> operator to get text value)
SELECT 
    product_id,
    details->>'name' AS product_name,
    details->'specs'->>'ram' AS ram
FROM 
    product_details;
```

2. **Complex JSON Queries**:

```sql
-- Filter by JSON property
SELECT * FROM product_details
WHERE details->>'name' = 'Laptop';

-- Array element queries
SELECT * FROM product_details 
WHERE details->'tags' ? 'electronics';

-- Use jsonb_array_elements to expand JSON arrays
SELECT 
    product_id,
    details->>'name' AS product_name,
    jsonb_array_elements_text(details->'tags') AS tag
FROM 
    product_details;

-- Use jsonb_each to iterate through key-value pairs
SELECT 
    product_id,
    key AS spec_name,
    value AS spec_value
FROM 
    product_details,
    jsonb_each(details->'specs') AS specs;
```

### Full-Text Search

```sql
-- Create a table with full-text search vector
CREATE TABLE articles (
    article_id serial PRIMARY KEY,
    title text,
    content text,
    search_vector tsvector
);

-- Insert sample data
INSERT INTO articles (title, content) VALUES
('PostgreSQL Tutorial', 'Learn how to use PostgreSQL with practical examples'),
('SQL Basics', 'Introduction to SQL syntax and database concepts');

-- Update full-text search vector
UPDATE articles SET
search_vector = to_tsvector('english', title || ' ' || content);

-- Create GIN index to speed up full-text search
CREATE INDEX article_search_idx ON articles USING gin(search_vector);

-- Execute full-text search query
SELECT 
    article_id,
    title,
    ts_rank(search_vector, query) AS rank
FROM 
    articles,
    to_tsquery('english', 'postgresql & tutorial') AS query
WHERE 
    search_vector @@ query
ORDER BY 
    rank DESC;
```

## Next Steps

After mastering how to run SQL queries using Navicat, you may want to learn about:

- [Exporting PostgreSQL Data](../Exporting_Data/README.md)
- [PostgreSQL Database Administration](../Database_Administration/README.md)

### Using Functions and Stored Procedures in Navicat

1. **View Existing Functions and Procedures**:
   - Expand the "Functions" node in the object browser
   - Double-click a function to view its definition

2. **Create New Functions**:
   - Right-click the "Functions" node
   - Select "New Function"
   - Use the function designer to fill in function details

3. **Test Functions**:
   - Right-click the function
   - Select "Run"
   - Provide any required parameter values

## PL/pgSQL Script Writing

### Basic PL/pgSQL Syntax

1. **Variable Declaration and Assignment**:

```sql
DO $$
DECLARE
    v_customer_id integer := 100;
    v_customer_name varchar(100);
    v_order_count integer;
BEGIN
    -- Assign from query
    SELECT customer_name INTO v_customer_name
    FROM customers
    WHERE customer_id = v_customer_id;
    
    -- Calculate values
    SELECT COUNT(*) INTO v_order_count
    FROM orders
    WHERE customer_id = v_customer_id;
    
    -- Output information
    RAISE NOTICE 'Customer: %, Order Count: %', v_customer_name, v_order_count;
END;
$$;
```

2. **Conditional Statements**:

```sql
DO $$
DECLARE
    v_price numeric := 45.99;
    v_price_category varchar(20);
BEGIN
    -- IF-THEN-ELSIF-ELSE
    IF v_price < 10 THEN
        v_price_category := 'Budget';
    ELSIF v_price < 50 THEN
        v_price_category := 'Mid-range';
    ELSIF v_price < 100 THEN
        v_price_category := 'Premium';
    ELSE
        v_price_category := 'Luxury';
    END IF;
    
    RAISE NOTICE 'Price category: %', v_price_category;
    
    -- CASE statement
    v_price_category := CASE
        WHEN v_price < 10 THEN 'Budget'
        WHEN v_price < 50 THEN 'Mid-range'
        WHEN v_price < 100 THEN 'Premium'
        ELSE 'Luxury'
    END;
    
    RAISE NOTICE 'Price category (from CASE): %', v_price_category;
END;
$$;
```

3. **Loops**:

```sql
-- Basic loop
DO $$
DECLARE
    v_counter integer := 1;
BEGIN
    LOOP
        RAISE NOTICE 'Counter: %', v_counter;
        v_counter := v_counter + 1;
        EXIT WHEN v_counter > 5;
    END LOOP;
END;
$$;

-- WHILE loop
DO $$
DECLARE
    v_counter integer := 1;
BEGIN
    WHILE v_counter <= 5 LOOP
        RAISE NOTICE 'Counter: %', v_counter;
        v_counter := v_counter + 1;
    END LOOP;
END;
$$;

-- FOR loop (integer range)
DO $$
BEGIN
    FOR i IN 1..5 LOOP
        RAISE NOTICE 'Counter: %', i;
    END LOOP;
END;
$$;

-- FOR loop (query results)
DO $$
DECLARE
    r record;
BEGIN
    FOR r IN (SELECT customer_id, customer_name FROM customers LIMIT 5) LOOP
        RAISE NOTICE 'Customer ID: %, Name: %', r.customer_id, r.customer_name;
    END LOOP;
END;
$$;
```

### Error Handling

```sql
DO $$
DECLARE
    v_customer_id integer := 999999;  -- Assume non-existent ID
    v_customer_name varchar(100);
BEGIN
    BEGIN
        -- Try to query non-existent customer
        SELECT customer_name INTO STRICT v_customer_name
        FROM customers
        WHERE customer_id = v_customer_id;
        
        RAISE NOTICE 'Customer found: %', v_customer_name;
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RAISE NOTICE 'Customer with ID % not found', v_customer_id;
        WHEN TOO_MANY_ROWS THEN
            RAISE NOTICE 'Multiple customers found with ID %', v_customer_id;
        WHEN OTHERS THEN
            RAISE NOTICE 'An error occurred: %', SQLERRM;
    END;
    
    RAISE NOTICE 'Script completed successfully';
END;
$$;
```

### Using Cursors

```sql
DO $$
DECLARE
    -- Declare cursor
    curs CURSOR FOR 
        SELECT customer_id, customer_name
        FROM customers
        ORDER BY customer_id
        LIMIT 10;
    
    -- Record variable
    r record;
BEGIN
    -- Open cursor
    OPEN curs;
    
    -- Loop to get data
    LOOP
        -- Get next row
        FETCH curs INTO r;
        
        -- Check if there are more rows
        EXIT WHEN NOT FOUND;
        
        -- Process current row
        RAISE NOTICE 'Customer ID: %, Name: %', r.customer_id, r.customer_name;
    END LOOP;
    
    -- Close cursor
    CLOSE curs;
END;
$$;

-- Create a basic stored procedure
CREATE OR REPLACE PROCEDURE update_product_price(
    product_id_param integer,
    new_price numeric
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE products 
    SET price = new_price
    WHERE product_id = product_id_param;
    
    COMMIT;
END;
$$;

-- Call the stored procedure
CALL update_product_price(101, 29.99); 