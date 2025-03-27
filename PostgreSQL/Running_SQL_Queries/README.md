# 在 PostgreSQL 中运行 SQL 查询

[← 返回 PostgreSQL 数据库教程](../PostgreSQL%20Database%20Tutorial%20with%20Navicat.md)

本指南详细介绍如何使用 Navicat 在 PostgreSQL 数据库中编写和执行 SQL 查询，从基本查询到高级技术和性能优化。

## 目录
- [Navicat 查询界面](#navicat-查询界面)
- [基本 SQL 查询](#基本-sql-查询)
- [高级查询技术](#高级查询技术)
- [查询性能优化](#查询性能优化)
- [查询管理和组织](#查询管理和组织)
- [存储过程和函数](#存储过程和函数)
- [PL/pgSQL 脚本编写](#plpgsql-脚本编写)
- [查询故障排除](#查询故障排除)

## Navicat 查询界面

### 创建和打开查询

1. **创建新查询**：
   - 点击工具栏上的"查询"按钮
   - 或选择菜单：查询 → 新建查询
   - 或使用快捷键 Ctrl+Q
   
   ![新建查询](images/new_query.png)

2. **查询编辑器界面**：
   - **编辑区域**：编写 SQL 代码的主要区域
   - **结果网格**：显示查询结果的区域
   - **信息面板**：显示查询执行信息和统计数据
   - **工具栏**：常用功能按钮（执行、停止、格式化等）

3. **连接选择**：
   - 确保从下拉菜单中选择了正确的 PostgreSQL 连接
   - 可以切换使用的连接和数据库

### 编辑器功能

1. **代码编辑功能**：
   - **语法高亮**：SQL 关键字、表名、函数等使用不同颜色显示
   - **自动补全**：输入时显示可能的表名、列名和关键字建议
   - **代码折叠**：折叠 SQL 语句块，使长查询更易管理
   - **括号匹配**：高亮显示匹配的括号
   - **行号**：显示代码行号以便引用

2. **代码辅助**：
   - **表设计器集成**：右击表名，选择"设计表"查看表结构
   - **SQL 代码片段**：保存和插入常用代码片段
   - **SQL 格式化**：点击格式化按钮自动美化 SQL 代码
   - **SQL 构建器**：使用可视化界面构建查询

## 基本 SQL 查询

### SELECT 查询

1. **简单 SELECT 查询**：

```sql
-- 从单个表中检索所有列
SELECT * FROM customers;

-- 检索特定列
SELECT customer_id, first_name, last_name, email 
FROM customers;

-- 添加条件（WHERE 子句）
SELECT * FROM customers 
WHERE country = 'USA';
```

2. **结果排序**：

```sql
-- 单列排序（升序）
SELECT * FROM products 
ORDER BY price;

-- 单列排序（降序）
SELECT * FROM products 
ORDER BY price DESC;

-- 多列排序
SELECT * FROM customers 
ORDER BY country, city, last_name;
```

3. **筛选结果**：

```sql
-- 使用 DISTINCT 查询唯一值
SELECT DISTINCT country FROM customers;

-- 使用 LIMIT 限制结果数量
SELECT * FROM products 
ORDER BY price DESC 
LIMIT 10;

-- 使用 OFFSET 进行分页
SELECT * FROM products 
ORDER BY product_id 
LIMIT 20 OFFSET 40;  -- 显示第3页（每页20条）
```

### 使用条件表达式

1. **比较运算符**：

```sql
-- 等于
SELECT * FROM products WHERE price = 19.99;

-- 不等于
SELECT * FROM products WHERE category_id != 3;

-- 大于、小于
SELECT * FROM products WHERE price > 100;
SELECT * FROM products WHERE price <= 50;

-- BETWEEN 范围查询
SELECT * FROM products 
WHERE price BETWEEN 20 AND 50;

-- IN 列表匹配
SELECT * FROM customers 
WHERE country IN ('USA', 'Canada', 'Mexico');
```

2. **逻辑运算符**：

```sql
-- AND 组合条件
SELECT * FROM customers 
WHERE country = 'USA' AND state = 'CA';

-- OR 条件
SELECT * FROM products 
WHERE category_id = 1 OR category_id = 2;

-- AND 和 OR 组合
SELECT * FROM products 
WHERE (category_id = 1 OR category_id = 2) 
AND price < 100;

-- NOT 反转条件
SELECT * FROM customers 
WHERE NOT country = 'USA';
```

3. **NULL 值处理**：

```sql
-- 查找 NULL 值
SELECT * FROM customers 
WHERE phone_number IS NULL;

-- 查找非 NULL 值
SELECT * FROM customers 
WHERE phone_number IS NOT NULL;
```

### 使用模式匹配

1. **LIKE 运算符**：

```sql
-- 以特定文本开头
SELECT * FROM products 
WHERE product_name LIKE 'Apple%';

-- 以特定文本结尾
SELECT * FROM products 
WHERE product_name LIKE '%Phone';

-- 包含特定文本
SELECT * FROM products 
WHERE product_name LIKE '%Pro%';

-- 匹配特定位置的字符
SELECT * FROM customers 
WHERE phone_number LIKE '555-___-____';
```

2. **正则表达式**：

```sql
-- 使用 ~ 进行正则表达式匹配
SELECT * FROM customers 
WHERE email ~ '[a-z]+@gmail\.com';

-- 不区分大小写的正则匹配
SELECT * FROM products 
WHERE product_name ~* '^mac';
```

## 高级查询技术

### 联接查询

1. **内联接（INNER JOIN）**：

```sql
-- 联接两个表
SELECT o.order_id, o.order_date, c.customer_name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id;

-- 联接多个表
SELECT o.order_id, c.customer_name, p.product_name, oi.quantity
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN order_items oi ON o.order_id = oi.order_id
INNER JOIN products p ON oi.product_id = p.product_id;
```

2. **外联接**：

```sql
-- 左外联接
SELECT c.customer_name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;

-- 右外联接
SELECT p.product_name, oi.order_id
FROM order_items oi
RIGHT JOIN products p ON oi.product_id = p.product_id;

-- 全外联接
SELECT c.customer_name, o.order_id
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id;
```

3. **自联接**：

```sql
-- 员工和经理关系
SELECT e1.employee_name AS employee, e2.employee_name AS manager
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.employee_id;
```

### 聚合和分组

1. **聚合函数**：

```sql
-- 计算总数
SELECT COUNT(*) FROM customers;

-- 计算某列总数（忽略 NULL）
SELECT COUNT(phone_number) FROM customers;

-- 求和
SELECT SUM(amount) FROM orders;

-- 平均值
SELECT AVG(price) FROM products;

-- 最大值和最小值
SELECT MAX(price), MIN(price) FROM products;
```

2. **分组查询**：

```sql
-- 按单列分组
SELECT country, COUNT(*) AS customer_count
FROM customers
GROUP BY country;

-- 按多列分组
SELECT country, city, COUNT(*) AS customer_count
FROM customers
GROUP BY country, city;

-- 使用 HAVING 过滤分组
SELECT category_id, AVG(price) AS avg_price
FROM products
GROUP BY category_id
HAVING AVG(price) > 100;
```

3. **窗口函数**：

```sql
-- 使用 ROW_NUMBER()
SELECT 
    product_name,
    price,
    category_id,
    ROW_NUMBER() OVER(PARTITION BY category_id ORDER BY price DESC) AS price_rank
FROM 
    products;

-- 使用 RANK() 和 DENSE_RANK()
SELECT
    product_name,
    price,
    category_id,
    RANK() OVER(PARTITION BY category_id ORDER BY price DESC) AS price_rank,
    DENSE_RANK() OVER(PARTITION BY category_id ORDER BY price DESC) AS dense_price_rank
FROM 
    products;

-- 使用 SUM() 作为窗口函数
SELECT
    order_id,
    product_id,
    amount,
    SUM(amount) OVER(PARTITION BY order_id) AS order_total
FROM 
    order_items;
```

### 子查询

1. **基本子查询**：

```sql
-- 在 WHERE 子句中使用子查询
SELECT product_name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products);

-- 使用 IN 操作符
SELECT customer_name
FROM customers
WHERE customer_id IN (
    SELECT DISTINCT customer_id
    FROM orders
    WHERE order_date >= '2023-01-01'
);
```

2. **相关子查询**：

```sql
-- 查找每个类别中最贵的产品
SELECT p1.product_id, p1.product_name, p1.category_id, p1.price
FROM products p1
WHERE p1.price = (
    SELECT MAX(p2.price)
    FROM products p2
    WHERE p2.category_id = p1.category_id
);
```

3. **EXISTS 子查询**：

```sql
-- 查找有订单的客户
SELECT customer_name
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

4. **在 FROM 子句中使用子查询**：

```sql
-- 使用子查询作为派生表
SELECT category_name, avg_price
FROM (
    SELECT 
        c.category_name,
        AVG(p.price) AS avg_price
    FROM 
        products p
    JOIN 
        categories c ON p.category_id = c.category_id
    GROUP BY 
        c.category_name
) AS category_stats
WHERE avg_price > 50;
```

### 公用表表达式（CTE）

1. **基本 CTE**：

```sql
-- 使用 WITH 子句定义 CTE
WITH customer_orders AS (
    SELECT 
        c.customer_id,
        c.customer_name,
        COUNT(o.order_id) AS order_count,
        SUM(o.amount) AS total_spent
    FROM 
        customers c
    LEFT JOIN 
        orders o ON c.customer_id = o.customer_id
    GROUP BY 
        c.customer_id, c.customer_name
)
SELECT * FROM customer_orders
WHERE order_count > 0
ORDER BY total_spent DESC;
```

2. **递归 CTE**：

```sql
-- 使用递归 CTE 生成数字序列
WITH RECURSIVE numbers AS (
    SELECT 1 AS n   -- 基本情况
    UNION
    SELECT n + 1    -- 递归步骤
    FROM numbers
    WHERE n < 10    -- 终止条件
)
SELECT * FROM numbers;

-- 使用递归 CTE 查询层次结构
WITH RECURSIVE subordinates AS (
    -- 基本情况：查找 CEO
    SELECT employee_id, employee_name, manager_id
    FROM employees
    WHERE employee_id = 1  -- 假设 CEO 的 ID 是 1
    
    UNION ALL
    
    -- 递归步骤：查找所有直接报告给已识别员工的员工
    SELECT e.employee_id, e.employee_name, e.manager_id
    FROM employees e
    INNER JOIN subordinates s ON e.manager_id = s.employee_id
)
SELECT * FROM subordinates;
```

### 集合操作

1. **UNION 和 UNION ALL**：

```sql
-- 合并两个查询结果（去除重复）
SELECT product_id, product_name FROM products
WHERE category_id = 1
UNION
SELECT product_id, product_name FROM products
WHERE price < 20;

-- 合并两个查询结果（保留重复）
SELECT product_id, product_name FROM products
WHERE category_id = 1
UNION ALL
SELECT product_id, product_name FROM products
WHERE price < 20;
```

2. **INTERSECT**：

```sql
-- 查找同时满足两个条件的记录
SELECT product_id FROM products
WHERE category_id = 1
INTERSECT
SELECT product_id FROM products
WHERE price < 20;
```

3. **EXCEPT**：

```sql
-- 查找满足第一个条件但不满足第二个条件的记录
SELECT product_id FROM products
WHERE category_id = 1
EXCEPT
SELECT product_id FROM products
WHERE price < 20;
```

## 查询性能优化

### 使用 EXPLAIN 分析查询

1. **基本 EXPLAIN**：

```sql
-- 查看查询计划
EXPLAIN
SELECT * FROM customers
WHERE country = 'USA';

-- 查看实际执行统计（包括执行时间）
EXPLAIN ANALYZE
SELECT c.customer_name, COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE c.country = 'USA'
GROUP BY c.customer_name;
```

2. **解读 EXPLAIN 输出**：
   - **扫描类型**：了解 Seq Scan（全表扫描）vs Index Scan（索引扫描）
   - **成本估算**：查看每个操作的成本
   - **行数估计**：了解每步处理的行数
   - **执行时间**：查看实际执行时间（使用 EXPLAIN ANALYZE）

### 索引优化

1. **查看表的索引**：

```sql
-- 列出表的索引
SELECT
    i.relname AS index_name,
    a.attname AS column_name
FROM
    pg_class t,
    pg_class i,
    pg_index ix,
    pg_attribute a
WHERE
    t.oid = ix.indrelid
    AND i.oid = ix.indexrelid
    AND a.attrelid = t.oid
    AND a.attnum = ANY(ix.indkey)
    AND t.relkind = 'r'
    AND t.relname = 'your_table_name';
```

2. **创建合适的索引**：

```sql
-- 创建 B-tree 索引（默认索引类型）
CREATE INDEX idx_customers_country ON customers(country);

-- 创建复合索引
CREATE INDEX idx_customers_country_city ON customers(country, city);

-- 创建唯一索引
CREATE UNIQUE INDEX idx_customers_email ON customers(email);

-- 创建部分索引
CREATE INDEX idx_orders_recent ON orders(order_date)
WHERE order_date > '2023-01-01';
```

3. **索引使用技巧**：
   - 为经常出现在 WHERE 子句、JOIN 条件和 ORDER BY 子句中的列创建索引
   - 避免对频繁更新的列创建不必要的索引
   - 定期使用 VACUUM ANALYZE 更新统计信息

### 查询优化技巧

1. **优化连接查询**：
   - 确保连接条件上有索引
   - 小表放在连接左侧（对于嵌套循环连接）
   - 使用 JOIN 语法而不是在 WHERE 子句中使用连接条件

2. **使用适当的数据过滤**：
   - 尽早过滤数据（WHERE 条件）以减少处理的行数
   - 在子查询中使用适当的过滤条件

3. **避免常见性能陷阱**：
   - 避免在 WHERE 子句中对列使用函数（会阻止索引使用）
   - 避免使用 SELECT *（只选择需要的列）
   - 使用 EXISTS 代替 IN 进行相关子查询
   - 避免使用外部连接，除非必要

## 查询管理和组织

### 保存和组织查询

1. **保存查询**：
   - 在查询编辑器中点击"保存"
   - 为查询提供名称和描述
   - 选择保存位置（连接或项目）

2. **查询分类和组织**：
   - 创建查询文件夹进行组织
   - 使用命名约定标识不同类型的查询
   - 为复杂查询添加注释和文档

3. **查询模板**：
   - 创建通用查询模板
   - 使用参数化查询提高灵活性
   - 共享常用查询供团队使用

### 导出查询结果

1. **导出到文件**：
   - 执行查询后，右键点击结果网格
   - 选择"导出结果"
   - 选择格式（CSV、Excel、HTML、JSON、XML、PDF）
   - 配置导出选项并保存

2. **复制结果**：
   - 选择要复制的行和列
   - 右键点击，选择"复制"或按 Ctrl+C
   - 粘贴到其他应用程序（如电子表格）

3. **生成数据报表**：
   - 将查询结果转换为格式化报表
   - 使用 Navicat 的报表生成功能
   - 导出为 PDF 进行共享

## 存储过程和函数

### 创建存储函数

PostgreSQL 使用自定义函数而不是存储过程（直到 PostgreSQL 11 引入 PROCEDURE 关键字）：

1. **创建基本函数**：

```sql
-- 创建返回标量值的函数
CREATE OR REPLACE FUNCTION get_customer_count()
RETURNS integer AS $$
BEGIN
    RETURN (SELECT COUNT(*) FROM customers);
END;
$$ LANGUAGE plpgsql;

-- 使用函数
SELECT get_customer_count();
```

2. **带参数的函数**：

```sql
-- 创建带参数的函数
CREATE OR REPLACE FUNCTION get_orders_by_customer(cust_id integer)
RETURNS TABLE(order_id integer, order_date date, amount numeric) AS $$
BEGIN
    RETURN QUERY
    SELECT o.order_id, o.order_date, o.amount
    FROM orders o
    WHERE o.customer_id = cust_id
    ORDER BY o.order_date DESC;
END;
$$ LANGUAGE plpgsql;

-- 使用函数
SELECT * FROM get_orders_by_customer(123);
```

3. **返回表的函数**：

```sql
-- 创建返回多行的函数
CREATE OR REPLACE FUNCTION get_top_products(n integer)
RETURNS TABLE(product_id integer, product_name varchar, sales_count bigint) AS $$
BEGIN
    RETURN QUERY
    SELECT 
        p.product_id, 
        p.product_name, 
        COUNT(oi.order_id) AS sales_count
    FROM 
        products p
    JOIN 
        order_items oi ON p.product_id = oi.product_id
    GROUP BY 
        p.product_id, p.product_name
    ORDER BY 
        sales_count DESC
    LIMIT n;
END;
$$ LANGUAGE plpgsql;

-- 使用函数
SELECT * FROM get_top_products(10);
```

### 创建存储过程（PostgreSQL 11+）

```sql
-- 创建基本存储过程
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

-- 调用存储过程
CALL update_product_price(101, 29.99);
```

### 在 Navicat 中使用函数和存储过程

1. **查看现有函数和存储过程**：
   - 在对象浏览器中展开"函数"节点
   - 双击函数查看其定义

2. **创建新函数**：
   - 右键点击"函数"节点
   - 选择"新建函数"
   - 使用函数设计器填写函数详情

3. **测试函数**：
   - 右键点击函数
   - 选择"运行"
   - 提供任何所需的参数值

## PL/pgSQL 脚本编写

### 基本 PL/pgSQL 语法

1. **变量声明和赋值**：

```sql
DO $$
DECLARE
    v_customer_id integer := 100;
    v_customer_name varchar(100);
    v_order_count integer;
BEGIN
    -- 从查询中赋值
    SELECT customer_name INTO v_customer_name
    FROM customers
    WHERE customer_id = v_customer_id;
    
    -- 计算值
    SELECT COUNT(*) INTO v_order_count
    FROM orders
    WHERE customer_id = v_customer_id;
    
    -- 输出信息
    RAISE NOTICE 'Customer: %, Order Count: %', v_customer_name, v_order_count;
END;
$$;
```

2. **条件语句**：

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
    
    -- CASE 语句
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

3. **循环**：

```sql
-- 基本循环
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

-- WHILE 循环
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

-- FOR 循环（整数范围）
DO $$
BEGIN
    FOR i IN 1..5 LOOP
        RAISE NOTICE 'Counter: %', i;
    END LOOP;
END;
$$;

-- FOR 循环（查询结果）
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

### 错误处理

```sql
DO $$
DECLARE
    v_customer_id integer := 999999;  -- 假设不存在的 ID
    v_customer_name varchar(100);
BEGIN
    BEGIN
        -- 尝试查询不存在的客户
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

### 使用游标

```sql
DO $$
DECLARE
    -- 声明游标
    curs CURSOR FOR 
        SELECT customer_id, customer_name
        FROM customers
        ORDER BY customer_id
        LIMIT 10;
    
    -- 记录变量
    r record;
BEGIN
    -- 打开游标
    OPEN curs;
    
    -- 循环获取数据
    LOOP
        -- 获取下一行
        FETCH curs INTO r;
        
        -- 检查是否还有行
        EXIT WHEN NOT FOUND;
        
        -- 处理当前行
        RAISE NOTICE 'Customer ID: %, Name: %', r.customer_id, r.customer_name;
    END LOOP;
    
    -- 关闭游标
    CLOSE curs;
END;
$$;
```

## 查询故障排除

### 常见错误及解决方法

1. **语法错误**：
   - 检查 SQL 语法（关键字拼写、缺少括号等）
   - 使用 Navicat 的 SQL 格式化功能帮助识别问题
   - 查看错误消息中的行号和列号指示

2. **表或列不存在**：
   - 确认表名和列名拼写正确
   - 检查是否在正确的模式中查找表
   - 使用 Navicat 的对象浏览器查看可用表和列

3. **权限问题**：
   - 确认当前用户对相关表有必要的权限
   - 查看 PostgreSQL 角色和权限设置
   - 联系数据库管理员获取必要权限

### 调试查询

1. **拆分复杂查询**：
   - 将复杂查询分解为更小的部分单独测试
   - 使用临时表存储中间结果
   - 从最简单的部分开始，逐步添加复杂性

2. **使用 EXPLAIN 了解查询执行**：
   - 分析查询执行计划识别性能问题
   - 检查表扫描方法（全表扫描 vs 索引扫描）
   - 确认索引是否按预期使用

3. **记录和注释**：
   - 为查询添加详细注释
   - 记录任何假设或依赖
   - 保留查询的不同版本进行比较

## 高级 PostgreSQL 特性

### JSON 查询

1. **基本 JSON 操作**：

```sql
-- 创建包含 JSON 数据的表
CREATE TABLE product_details (
    product_id integer PRIMARY KEY,
    details jsonb
);

-- 插入 JSON 数据
INSERT INTO product_details (product_id, details)
VALUES (
    1, 
    '{"name": "Laptop", "specs": {"ram": "16GB", "cpu": "i7", "storage": "512GB SSD"}, "tags": ["electronics", "computer"]}'
);

-- 访问 JSON 对象字段（使用 -> 操作符获取 JSON 值）
SELECT 
    product_id,
    details->'name' AS product_name,
    details->'specs'->'ram' AS ram
FROM 
    product_details;

-- 访问 JSON 对象字段（使用 ->> 操作符获取文本值）
SELECT 
    product_id,
    details->>'name' AS product_name,
    details->'specs'->>'ram' AS ram
FROM 
    product_details;
```

2. **复杂 JSON 查询**：

```sql
-- 根据 JSON 属性过滤
SELECT * FROM product_details
WHERE details->>'name' = 'Laptop';

-- 数组元素查询
SELECT * FROM product_details 
WHERE details->'tags' ? 'electronics';

-- 使用 jsonb_array_elements 展开 JSON 数组
SELECT 
    product_id,
    details->>'name' AS product_name,
    jsonb_array_elements_text(details->'tags') AS tag
FROM 
    product_details;

-- 使用 jsonb_each 遍历键值对
SELECT 
    product_id,
    key AS spec_name,
    value AS spec_value
FROM 
    product_details,
    jsonb_each(details->'specs') AS specs;
```

### 全文搜索

```sql
-- 创建包含全文搜索向量的表
CREATE TABLE articles (
    article_id serial PRIMARY KEY,
    title text,
    content text,
    search_vector tsvector
);

-- 插入示例数据
INSERT INTO articles (title, content) VALUES
('PostgreSQL Tutorial', 'Learn how to use PostgreSQL with practical examples'),
('SQL Basics', 'Introduction to SQL syntax and database concepts');

-- 更新全文搜索向量
UPDATE articles SET
search_vector = to_tsvector('english', title || ' ' || content);

-- 创建 GIN 索引以加速全文搜索
CREATE INDEX article_search_idx ON articles USING gin(search_vector);

-- 执行全文搜索查询
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

## 下一步

在掌握了如何使用 Navicat 运行 SQL 查询后，您可能希望了解：

- [导出 PostgreSQL 数据](../Exporting_Data/README.md)
- [PostgreSQL 数据库管理](../Database_Administration/README.md) 