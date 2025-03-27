# PostgreSQL 服务器运行与管理指南

[← 返回 PostgreSQL 数据库教程](../PostgreSQL%20Database%20Tutorial%20with%20Navicat.md)

本指南详细介绍了如何启动、停止、配置和监控 PostgreSQL 服务器，以确保数据库系统高效稳定运行。

## 目录
- [启动与停止 PostgreSQL 服务器](#启动与停止-postgresql-服务器)
- [服务器配置](#服务器配置)
- [监控服务器状态](#监控服务器状态)
- [性能调优](#性能调优)
- [备份与恢复](#备份与恢复)
- [日志管理](#日志管理)
- [常见故障排除](#常见故障排除)

## 启动与停止 PostgreSQL 服务器

### Windows 环境

在 Windows 上，PostgreSQL 安装后通常作为系统服务运行。

**使用服务管理器：**
1. 打开服务管理器：按 Win+R，输入 `services.msc`
2. 找到 "postgresql-x64-XX"（XX 是版本号，如 15）
3. 右键点击服务，选择"启动"、"停止"或"重启"

**使用命令行：**
```
# 启动服务
net start postgresql-x64-15

# 停止服务
net stop postgresql-x64-15
```

**使用 WSL（如果通过 WSL 安装）：**
```bash
# 启动服务
wsl sudo service postgresql start

# 停止服务
wsl sudo service postgresql stop

# 重启服务
wsl sudo service postgresql restart

# 检查状态
wsl sudo service postgresql status
```

### macOS 环境

**使用 Homebrew：**
```bash
# 启动服务
brew services start postgresql

# 停止服务
brew services stop postgresql

# 重启服务
brew services restart postgresql

# 检查状态
brew services list | grep postgres
```

**使用 pg_ctl（手动控制）：**
```bash
# 启动服务
pg_ctl -D /usr/local/var/postgres start

# 停止服务
pg_ctl -D /usr/local/var/postgres stop

# 重启服务
pg_ctl -D /usr/local/var/postgres restart

# 检查状态
pg_ctl -D /usr/local/var/postgres status
```

### Linux 环境

大多数 Linux 发行版使用 systemd 来管理服务：

```bash
# 启动服务
wsl sudo systemctl start postgresql

# 停止服务
wsl sudo systemctl stop postgresql

# 重启服务
wsl sudo systemctl restart postgresql

# 检查状态
wsl sudo systemctl status postgresql

# 设置开机自启
wsl sudo systemctl enable postgresql
```

对于使用 SysV init 的旧版 Linux：

```bash
# 启动服务
wsl sudo service postgresql start

# 停止服务
wsl sudo service postgresql stop
```

## 服务器配置

PostgreSQL 的主要配置文件有：

- **postgresql.conf**：主配置文件，控制大多数服务器参数
- **pg_hba.conf**：客户端身份验证配置
- **pg_ident.conf**：用户名映射配置

### 配置文件位置

**Windows：**
```
C:\Program Files\PostgreSQL\15\data\postgresql.conf
C:\Program Files\PostgreSQL\15\data\pg_hba.conf
```

**macOS (Homebrew)：**
```
/usr/local/var/postgres/postgresql.conf
/usr/local/var/postgres/pg_hba.conf
```

**Linux：**
```
/etc/postgresql/15/main/postgresql.conf
/etc/postgresql/15/main/pg_hba.conf
```

### 重要配置参数

**postgresql.conf 重要设置：**

1. **连接设置**
   ```
   listen_addresses = '*'     # 监听地址，'*'表示所有地址
   port = 5432               # 监听端口
   max_connections = 100     # 最大连接数
   ```

2. **内存配置**
   ```
   shared_buffers = 128MB      # 共享内存缓冲区大小（建议为系统内存的1/4）
   work_mem = 4MB              # 内部排序和哈希操作的内存
   maintenance_work_mem = 64MB # 维护操作使用的内存
   ```

3. **WAL（预写式日志）设置**
   ```
   wal_level = replica      # WAL级别，可选：minimal、replica、logical
   max_wal_size = 1GB       # 触发检查点前允许的WAL最大大小
   min_wal_size = 80MB      # WAL最小保留大小
   ```

4. **性能优化**
   ```
   effective_cache_size = 4GB   # 操作系统可用于缓存磁盘数据的内存估计
   random_page_cost = 4.0       # 随机磁盘页面访问成本的估计值
   ```

**pg_hba.conf 示例配置：**
```
# TYPE  DATABASE        USER            ADDRESS                 METHOD
local   all             postgres                                peer
local   all             all                                     md5
host    all             all             127.0.0.1/32            md5
host    all             all             ::1/128                 md5
host    all             all             192.168.0.0/24          md5
```

### 应用配置更改

修改配置文件后，需要重新加载或重启服务器才能生效：

```bash
# 重新加载配置（不需要重启，但有些参数不会生效）
wsl sudo -u postgres pg_ctl reload

# 完全重启服务器
wsl sudo systemctl restart postgresql
```

## 监控服务器状态

### 使用 SQL 命令监控

连接到 PostgreSQL 数据库后，可以使用以下 SQL 命令监控状态：

```sql
-- 查看活跃连接
SELECT * FROM pg_stat_activity;

-- 查看数据库大小
SELECT pg_size_pretty(pg_database_size('database_name'));

-- 查看表大小
SELECT pg_size_pretty(pg_total_relation_size('table_name'));

-- 查看索引使用情况
SELECT * FROM pg_stat_user_indexes;

-- 查看服务器统计信息
SELECT * FROM pg_stat_database WHERE datname = 'database_name';
```

### 使用 Navicat 监控 PostgreSQL

Navicat 提供了多种监控 PostgreSQL 服务器的工具：

1. **服务器监控**：
   - 在连接上右击选择"服务器监控"
   - 监控 CPU 使用率、内存使用率、连接数等

2. **状态变量**：
   - 选择连接 → 工具 → 服务器状态
   - 查看各种性能计数器和状态变量

3. **当前连接**：
   - 选择连接 → 工具 → 进程列表
   - 查看和管理当前连接的客户端

4. **日志查看器**：
   - 查看服务器日志以排查问题

### 使用 pg_stat_statements 扩展

可以安装这个扩展来监控查询性能：

```sql
-- 创建扩展
CREATE EXTENSION pg_stat_statements;

-- 查看查询统计信息
SELECT query, calls, total_time, rows, mean_time
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;
```

## 性能调优

### 自动清理（Autovacuum）设置

自动清理是 PostgreSQL 维护数据库健康的关键：

```
# 在 postgresql.conf 中配置
autovacuum = on                        # 启用自动清理
autovacuum_vacuum_threshold = 50       # 插入、更新或删除的元组数
autovacuum_analyze_threshold = 50      # 插入、更新或删除的元组数
autovacuum_vacuum_scale_factor = 0.2   # 表大小的比例因子
autovacuum_analyze_scale_factor = 0.1  # 表大小的比例因子
```

### 内存调优

为提高性能，根据服务器可用内存调整以下参数：

```
shared_buffers = 25% of RAM           # 专用服务器建议为总内存的25%
work_mem = 32MB                       # 复杂查询可以增加这个值
maintenance_work_mem = 256MB          # 维护操作的内存
effective_cache_size = 75% of RAM     # 设置为总内存的75%减去shared_buffers
```

### 并发调优

```
max_connections = 100                 # 最大并发连接数
max_worker_processes = 8              # 最大工作进程数
max_parallel_workers_per_gather = 4   # 每个查询的并行工作者数
max_parallel_workers = 8              # 系统并行工作者总数
```

### 预写式日志（WAL）调优

这些设置对于写入密集型工作负载很重要：

```
wal_buffers = 16MB                    # WAL缓冲区大小
checkpoint_timeout = 15min            # 检查点超时
max_wal_size = 2GB                    # 检查点之间的最大WAL大小
```

## 备份与恢复

### 使用 pg_dump 进行备份

**备份单个数据库：**
```bash
wsl pg_dump -U postgres -F c -b -v -f backup.dump database_name
```

**备份所有数据库：**
```bash
wsl pg_dumpall -U postgres -f all_databases.sql
```

参数说明：
- `-F c`：自定义格式（二进制）
- `-b`：包括大对象
- `-v`：详细模式
- `-f`：输出文件

### 使用 pg_restore 恢复

```bash
wsl pg_restore -U postgres -d database_name -v backup.dump
```

### 使用 Navicat 进行备份与恢复

1. **备份数据库**:
   - 右键点击数据库 → 转储 SQL 文件
   - 选择要备份的对象和选项
   - 指定输出文件并开始备份

2. **恢复数据库**:
   - 右键点击数据库 → 执行 SQL 文件
   - 选择备份文件并执行

## 日志管理

### 配置日志

在 postgresql.conf 中配置日志选项：

```
# 日志位置
log_destination = 'stderr'       # 日志输出目标
logging_collector = on           # 启用日志收集器
log_directory = 'log'            # 日志目录
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'  # 日志文件名模式

# 日志内容
log_min_messages = warning       # 记录的最低消息级别
log_min_error_statement = error  # 导致错误的语句的最低级别
log_min_duration_statement = 250 # 记录执行时间超过此值的语句（毫秒）

# 特殊日志
log_connections = on             # 记录连接尝试
log_disconnections = on          # 记录会话结束
log_duration = off               # 记录语句执行持续时间
log_checkpoints = on             # 记录检查点信息
```

### 日志文件位置

**Windows：**
```
C:\Program Files\PostgreSQL\15\data\log\
```

**macOS (Homebrew)：**
```
/usr/local/var/log/postgres.log
```

**Linux：**
```
/var/log/postgresql/
```

### 使用 Navicat 查看日志

Navicat 提供日志查看器功能：
1. 连接到服务器
2. 右键点击连接 → 服务器日志
3. 浏览和过滤日志条目

## 常见故障排除

### 连接问题

**问题**：无法连接到数据库服务器
**解决方案**：
1. 确认服务器正在运行
2. 检查 pg_hba.conf 中的客户端认证设置
3. 确认监听地址（listen_addresses）配置
4. 检查防火墙设置
5. 确认端口没有被其他应用占用

### 性能问题

**问题**：查询执行缓慢
**解决方案**：
1. 确保表已被分析（执行 `ANALYZE`）
2. 检查查询计划（使用 `EXPLAIN ANALYZE`）
3. 为频繁查询的列添加索引
4. 增加 `work_mem` 或 `shared_buffers`
5. 调整自动清理设置

### 磁盘空间问题

**问题**：数据库占用过多磁盘空间
**解决方案**：
1. 运行 `VACUUM FULL` 回收空间（注意：这会锁定表）
2. 使用 `pg_repack` 扩展重组表而不需要长时间锁定
3. 清理旧日志文件和归档的 WAL 文件
4. 调整 WAL 文件保留策略

### 崩溃恢复

**问题**：服务器崩溃后不能启动
**解决方案**：
1. 检查数据目录权限
2. 查看日志文件以了解崩溃原因
3. 运行 `fsck` 检查文件系统一致性
4. 尝试使用 `-P` 参数启动以忽略系统索引

### 锁定问题

**问题**：长时间运行的事务阻塞其他操作
**解决方案**：
1. 使用以下查询查看锁：
   ```sql
   SELECT pid, usename, pg_blocking_pids(pid), query 
   FROM pg_stat_activity 
   WHERE cardinality(pg_blocking_pids(pid)) > 0;
   ```
2. 如果需要，终止阻塞会话：
   ```sql
   SELECT pg_terminate_backend(pid);
   ```

## 下一步

- [连接到 PostgreSQL 服务器](../Connecting_with_Navicat/README.md)
- [加载数据到 PostgreSQL](../Loading_Data/README.md)
- [浏览您的数据](../Browsing_Data/README.md)
- [学习数据库管理](../Database_Administration/README.md) 