# 加载数据到 PostgreSQL 数据库

[← 返回 PostgreSQL 数据库教程](../PostgreSQL%20Database%20Tutorial%20with%20Navicat.md)

本指南将详细介绍如何使用 Navicat 将各种格式的数据导入 PostgreSQL 数据库，包括导入表格数据、执行批量导入、数据迁移以及处理各种导入挑战。

## 目录
- [导入方法概述](#导入方法概述)
- [从文件导入数据](#从文件导入数据)
- [从其他数据库导入](#从其他数据库导入)
- [使用导入向导](#使用导入向导)
- [执行批量插入](#执行批量插入)
- [使用 COPY 命令](#使用-copy-命令)
- [导入 JSON 和 XML 数据](#导入-json-和-xml-数据)
- [导入地理空间数据](#导入地理空间数据)
- [数据导入策略](#数据导入策略)
- [排除导入错误](#排除导入错误)
- [性能优化技巧](#性能优化技巧)

## 导入方法概述

PostgreSQL 提供了多种将数据导入数据库的方法，Navicat 使这一过程更加直观和高效。以下是主要导入方法的比较：

| 导入方法 | 适用场景 | 优点 | 限制 |
|----------|----------|------|------|
| 导入向导 | 结构化文件导入 | 用户友好，支持多种格式 | 大数据集可能较慢 |
| SQL 脚本 | 预定义数据导入 | 高度可定制，可重复使用 | 需要 SQL 知识 |
| COPY 命令 | 高性能批量导入 | 速度最快，服务器端处理 | 文件需在服务器上可访问 |
| 批量插入 | 中等大小数据集 | 客户端友好，事务控制 | 不如 COPY 命令快 |
| 外部数据封装器 (FDW) | 连接外部数据源 | 实时数据访问，不需要复制 | 性能开销，配置复杂 |

## 从文件导入数据

Navicat 支持从各种文件格式导入数据到 PostgreSQL 表中。

### 从 CSV/TSV 文件导入

1. **准备导入**：
   - 在 Navicat 中连接到您的 PostgreSQL 数据库
   - 右键点击目标表，选择**导入向导**
   
   ![导入向导](images/import_wizard.png)

2. **选择文件格式和位置**：
   - 选择**文件**作为源
   - 选择格式为 **CSV** 或 **TXT**（用于 TSV）
   - 浏览并选择您的数据文件

3. **配置导入选项**：
   - **字段分隔符**：CSV 使用逗号(,)，TSV 使用制表符(\t)
   - **文本分隔符**：通常为双引号(")
   - **首行为字段名**：如果 CSV 文件第一行包含列名，勾选此项
   - **编码**：选择文件的字符编码（如 UTF-8）

4. **字段映射**：
   - 将 CSV 列映射到表中的相应列
   - 可以跳过不需要导入的列
   - 设置数据类型转换规则

5. **导入设置**：
   - **插入记录**：将数据作为新记录添加
   - **如果表存在则清空**：在导入前清空表
   - **忽略导入错误**：继续导入即使有错误
   - **启用事务**：将整个导入作为一个事务

6. **执行导入**：
   - 点击**开始**按钮执行导入
   - 查看导入进度和结果摘要

### 从 Excel 文件导入

1. 右键点击目标表，选择**导入向导**
2. 选择**文件**作为源，格式为 **Excel**
3. 浏览并选择您的 Excel 文件
4. 选择要导入的工作表
5. 配置映射和导入选项（类似于 CSV 导入）
6. 执行导入

### 从 JSON 文件导入

1. 右键点击目标表，选择**导入向导**
2. 选择**文件**作为源，格式为 **JSON**
3. 浏览并选择您的 JSON 文件
4. 设置 JSON 路径和数组路径（如果导入数组数据）
5. 将 JSON 字段映射到表列
6. 配置特殊字段处理（如嵌套对象）
7. 执行导入

## 从其他数据库导入

Navicat 允许您直接从其他数据库导入数据到 PostgreSQL。

### 从其他 PostgreSQL 数据库导入

1. **准备导入**：
   - 确保在 Navicat 中同时连接了源和目标 PostgreSQL 数据库
   - 右键点击目标表，选择**导入向导**

2. **选择源数据库**：
   - 选择**数据库**作为源
   - 从下拉列表中选择源连接和数据库

3. **选择源表和字段**：
   - 选择要导入数据的源表
   - 选择要导入的特定字段

4. **配置导入选项**：
   - 设置过滤条件以仅导入特定记录
   - 配置高级选项，如批处理大小和超时设置

5. **执行导入**：
   - 点击**开始**按钮执行导入
   - 查看导入进度和结果摘要

### 从 MySQL/SQL Server/Oracle 导入

1. 在 Navicat 中创建相应数据库类型的连接
2. 按照与上述类似的步骤进行操作
3. 注意处理数据类型差异和特殊字符

## 使用导入向导

Navicat 的导入向导提供了高级功能，可以增强数据导入过程。

### 自定义数据转换

在导入过程中，您可以应用以下转换：

1. **设置目标值**：
   - **NULL 值处理**：为空值设置默认值
   - **日期时间格式**：指定日期和时间的输入格式
   - **布尔值表示**：定义哪些值应转换为 true/false

2. **使用表达式**：
   - 在导入映射中使用 SQL 表达式
   - 例如：`UPPER([FieldName])` 将文本转换为大写

### 使用预览功能

导入向导的预览功能帮助您在执行导入前验证导入设置：

1. 点击**预览**按钮查看如何解析数据
2. 检查字段映射和数据类型转换
3. 验证特殊字符和编码是否正确处理

### 保存和重用导入配置

频繁执行类似的导入？保存导入配置以节省时间：

1. 配置完导入设置后，点击**保存配置**
2. 为配置提供名称和描述
3. 下次导入时，可以通过**加载配置**按钮重用设置

## 执行批量插入

对于大型数据集，批量插入可以显著提高性能。

### 使用 Navicat 批量插入

1. 在导入向导中配置设置时：
   - 选中**批量插入**选项
   - 设置**每批包含的记录数**（通常建议 1000-5000）

2. 批量插入的优势：
   - 减少与服务器的通信次数
   - 加快大型数据集的导入
   - 降低服务器负载

### 通过 SQL 脚本执行批量插入

对于更高级的控制，可以手动创建批量插入脚本：

```sql
BEGIN;

INSERT INTO target_table (column1, column2, column3)
VALUES
  ('value1_1', 'value1_2', 'value1_3'),
  ('value2_1', 'value2_2', 'value2_3'),
  ('value3_1', 'value3_2', 'value3_3'),
  /* 可以添加更多行 */;

COMMIT;
```

执行此脚本：
1. 在 Navicat 中打开查询编辑器
2. 粘贴脚本并执行
3. 对于非常大的数据集，将其分成多个事务

## 使用 COPY 命令

PostgreSQL 的 COPY 命令是导入和导出数据的最快方法。

### 通过 Navicat 使用 COPY 命令

1. 在 Navicat 的查询编辑器中，执行 COPY 命令：

```sql
COPY target_table FROM '/path/to/your/file.csv' 
WITH (FORMAT csv, HEADER true, DELIMITER ',', QUOTE '"');
```

2. 注意事项：
   - 文件路径必须是**服务器上**的路径，不是本地路径
   - 如果使用 Windows WSL，确保路径格式正确

### 客户端 COPY 命令 (\\copy)

如果文件在本地计算机上，可以使用 psql 的 \copy 命令：

```bash
wsl psql -U postgres -d your_database -c "\copy target_table FROM '/local/path/to/file.csv' WITH (FORMAT csv, HEADER true)"
```

在 Navicat 中，您可以：
1. 打开终端/命令行工具
2. 执行上述命令（调整连接参数和文件路径）
3. 返回 Navicat 刷新表格查看导入的数据

## 导入 JSON 和 XML 数据

PostgreSQL 对 JSON 和 XML 数据类型有出色的支持。

### 导入 JSON 数据

1. **创建包含 JSON 列的表**：

```sql
CREATE TABLE json_data (
    id SERIAL PRIMARY KEY,
    data JSONB
);
```

2. **使用 Navicat 导入 JSON 文件**：
   - 按照前面的 JSON 导入步骤操作
   - 确保将 JSON 数据映射到 JSONB 类型列

3. **导入嵌套的 JSON 数据**：
   - 使用 JSON 路径表达式解析复杂结构
   - 或者保留完整 JSON 对象，稍后使用 PostgreSQL 的 JSON 函数查询

### 导入 XML 数据

1. **创建包含 XML 列的表**：

```sql
CREATE TABLE xml_data (
    id SERIAL PRIMARY KEY,
    data XML
);
```

2. **导入 XML 文件**：
   - 使用导入向导，选择合适的源格式
   - 或者使用 SQL 直接导入 XML 文件内容：

```sql
INSERT INTO xml_data (data)
SELECT XMLPARSE(DOCUMENT convert_from(pg_read_binary_file('/path/to/file.xml'), 'UTF8'));
```

## 导入地理空间数据

PostgreSQL 通过 PostGIS 扩展提供了强大的地理空间数据支持。

### 准备 PostGIS

1. **安装 PostGIS 扩展**：

```sql
CREATE EXTENSION postgis;
```

2. **创建地理空间表**：

```sql
CREATE TABLE spatial_data (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    geom GEOMETRY(Point, 4326)
);
```

### 导入 Shapefile 数据

1. **使用 shp2pgsql 工具**（命令行）：

```bash
wsl shp2pgsql -s 4326 -I input_file.shp spatial_data > spatial_data.sql
wsl psql -U postgres -d your_database -f spatial_data.sql
```

2. **通过 Navicat 执行生成的 SQL**：
   - 打开 Navicat 查询编辑器
   - 加载并执行上述生成的 SQL 文件

### 导入 GeoJSON 数据

使用 PostgreSQL 的 JSON 功能和 PostGIS 导入 GeoJSON：

```sql
-- 假设您已有一个存储 GeoJSON 的表
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

## 数据导入策略

### 大型数据集导入策略

对于几百万行的数据集：

1. **禁用约束和触发器**：
   - 在导入前禁用触发器、外键和索引
   - 导入完成后重新启用

```sql
-- 禁用表的触发器
ALTER TABLE your_table DISABLE TRIGGER ALL;

-- 导入数据...

-- 重新启用触发器
ALTER TABLE your_table ENABLE TRIGGER ALL;
```

2. **分批导入**：
   - 将大型数据集分割成较小的批次
   - 每批导入后提交事务

3. **优化服务器设置**：
   - 临时增加维护工作内存：
   ```sql
   SET maintenance_work_mem = '1GB';
   ```
   - 调整自动清理设置：
   ```sql
   SET autovacuum_vacuum_threshold = 5000;
   SET autovacuum_analyze_threshold = 5000;
   ```

### 数据质量策略

确保导入数据的质量：

1. **预处理数据**：
   - 清理源数据中的不一致和错误
   - 标准化格式（日期、数字等）

2. **使用临时表**：
   - 首先导入到临时表
   - 验证和清理数据
   - 将清理后的数据移动到目标表

```sql
-- 创建临时表
CREATE TEMP TABLE temp_import AS
SELECT * FROM target_table WITH NO DATA;

-- 导入数据到临时表...

-- 验证和清理
DELETE FROM temp_import WHERE column1 IS NULL;
UPDATE temp_import SET column2 = UPPER(column2);

-- 将数据移动到目标表
INSERT INTO target_table SELECT * FROM temp_import;

-- 清理
DROP TABLE temp_import;
```

## 排除导入错误

### 常见导入错误及解决方法

| 错误类型 | 可能原因 | 解决方法 |
|----------|----------|----------|
| 字段不匹配 | 源数据列数与目标表不同 | 检查字段映射，调整导入设置或修改表结构 |
| 数据类型冲突 | 源数据值与目标列类型不兼容 | 使用数据类型转换，或在导入前清理数据 |
| 唯一约束冲突 | 导入数据违反唯一键/主键约束 | 检查源数据重复值，决定是否更新现有记录 |
| 外键约束失败 | 导入数据引用不存在的外键值 | 先导入参考表，或临时禁用外键约束 |
| 编码问题 | 字符编码不匹配 | 确保正确设置文件编码，考虑预处理转换 |

### 诊断导入问题

1. **启用详细日志**：
   - 在导入向导中选择详细日志选项
   - 或在 postgresql.conf 中设置更详细的日志记录

2. **检查 PostgreSQL 服务器日志**：
   - 在 Navicat 中右键点击连接并选择"服务器日志"
   - 或者查看服务器日志文件位置：
     - Windows: `C:\Program Files\PostgreSQL\15\data\log\`
     - Linux: `/var/log/postgresql/`

3. **尝试导入较小的数据样本**：
   - 从源数据中提取少量记录（例如 10-100 行）
   - 尝试导入以识别具体问题

## 性能优化技巧

### 提高导入速度的技巧

1. **优化索引**：
   - 在导入前删除索引，导入后重新创建：
   ```sql
   -- 记录现有索引（需要先创建）
   SELECT 'CREATE INDEX ' || indexname || ' ON ' || tablename || ' USING ' || indexdef || ';'
   FROM pg_indexes
   WHERE tablename = 'your_table';
   
   -- 删除索引
   DROP INDEX index_name;
   
   -- 导入数据...
   
   -- 重新创建索引
   CREATE INDEX index_name ON your_table(column);
   ```

2. **使用 UNLOGGED 表**：
   - 对于临时导入，使用 UNLOGGED 表减少 WAL 生成：
   ```sql
   CREATE UNLOGGED TABLE staging_table (LIKE target_table);
   
   -- 导入到 staging_table...
   
   -- 完成后移动数据
   INSERT INTO target_table SELECT * FROM staging_table;
   ```

3. **调整服务器参数**：
   - 增加工作内存：
   ```sql
   SET work_mem = '256MB';
   ```
   - 禁用 WAL 归档（仅测试环境）：
   ```sql
   SET wal_level = 'minimal';
   ```

### 并行处理

利用 PostgreSQL 的并行功能：

1. **并行数据加载**：
   - 将数据集分区
   - 使用多个连接并行导入不同分区
   - 使用 COPY 命令的 WORKERS 选项（PostgreSQL 14+）：
   ```sql
   COPY large_table FROM 'large_file.csv' WITH (FORMAT csv, WORKERS 4);
   ```

2. **使用 Foreign Data Wrappers 进行高级导入**：
   - 设置 file_fdw：
   ```sql
   CREATE EXTENSION file_fdw;
   CREATE SERVER file_server FOREIGN DATA WRAPPER file_fdw;
   ```
   - 创建外部表：
   ```sql
   CREATE FOREIGN TABLE ext_table (
     /* 列定义 */
   ) SERVER file_server
   OPTIONS (filename '/path/to/file.csv', format 'csv', header 'true');
   ```
   - 从外部表导入：
   ```sql
   INSERT INTO target_table SELECT * FROM ext_table;
   ```

## 下一步

成功导入数据后，建议：

1. **验证导入的数据**：
   - 检查记录数是否符合预期
   - 验证数据完整性和一致性

2. **优化数据库**：
   - 重建索引
   - 运行 ANALYZE 以更新统计信息
   - 根据需要执行 VACUUM 操作

3. **继续学习其他功能**：
   - [浏览导入的数据](../Browsing_Data/README.md)
   - [运行 SQL 查询](../Running_SQL_Queries/README.md)
   - [导出数据](../Exporting_Data/README.md)
   - [数据库管理](../Database_Administration/README.md) 