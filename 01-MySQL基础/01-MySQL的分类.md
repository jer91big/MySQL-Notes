SQL（Structured Query Language，结构化查询语言）的命令按照其功能和用途，通常可以清晰地划分为 **5 大类**。掌握这些分类能够帮助我们更好地理解数据库的操作逻辑。

  

### 1. DQL (Data Query Language - 数据查询语言)

- **主要作用**：用于从数据库中**查询**数据。这是日常开发中使用频率最高的 SQL 类别。
    
- **核心关键字**：`SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`
    
- **典型示例**：
- 
    SQL
    ```sql
    SELECT name, age FROM users WHERE age > 18;
    ```

    `SELECT` 的详细用法（`FROM`、`WHERE`、`ORDER BY`、`LIMIT` 等）见 [[01-SELECT查询]]。
    
### 2. DML (Data Manipulation Language - 数据操作语言)

- **主要作用**：用于对表中的**数据（行）进行增、删、改**操作。
    
- **核心关键字**：`INSERT`（插入）, `UPDATE`（更新）, `DELETE`（删除）
    
 
- **典型示例**：
    
    SQL
    ```sql
    INSERT INTO users (name, age) VALUES ('张三', 25);
    UPDATE users SET age = 26 WHERE name = '张三';
    DELETE FROM users WHERE name = '张三';
    ```
    

### 3. DDL (Data Definition Language - 数据定义语言)

- **主要作用**：用于定义或修改数据库的**结构**（包括数据库本身、表、视图、索引等），通常由数据库管理员使用。
    
   
- **核心关键字**：`CREATE`（创建）, `DROP`（删除）, `ALTER`（修改）, `TRUNCATE`（截断/清空表）
    
 
- **典型示例**：
    
    SQL
    ```sql
    CREATE TABLE users (id INT, name VARCHAR(50));
    ALTER TABLE users ADD COLUMN email VARCHAR(100);
    DROP TABLE users;
    ```

    表结构的具体创建、修改和删除，见 [[03-MySQL 表的创建、删除与修改]]。

### 4. DCL (Data Control Language - 数据控制语言)

- **主要作用**：用于定义数据库的**安全策略、访问权限和用户管理**。  
    
- **核心关键字**：`GRANT`（授予权限）, `REVOKE`（撤销权限） 
    
- **典型示例**：  
    
    SQL
    
    ```sql
    GRANT SELECT, INSERT ON database.* TO 'username'@'localhost';
    ```
    

### 5. TCL (Transaction Control Language - 事务控制语言)

- **主要作用**：用于管理数据库的**事务**，确保多条数据操作符合 ACID 特性（原子性、一致性、隔离性、持久性）。

- **核心关键字**：`COMMIT`（提交）, `ROLLBACK`（回滚）, `SAVEPOINT`（保存点）
   
    
- **典型示例**：
    SQL
    ```sql
    START TRANSACTION;
    -- 执行一些 DML 操作
    COMMIT; -- 确认提交
    ```

### 💡 核心分类速查表

|**分类**|**全称**|**主要功能**|**核心关键字**|
|---|---|---|---|
|**DQL**|Data Query Language|查询数据|`SELECT`|
|**DML**|Data Manipulation Language|增、删、改表中的数据|`INSERT`, `UPDATE`, `DELETE`|
|**DDL**|Data Definition Language|定义和修改数据库结构|`CREATE`, `DROP`, `ALTER`|
|**DCL**|Data Control Language|管理用户权限与安全|`GRANT`, `REVOKE`|
|**TCL**|Transaction Control Language|管理数据库事务|`COMMIT`, `ROLLBACK`|

---

**下一篇：** [[01-MySQL基础/02-MySQL中的数据类型|MySQL中的数据类型]]
