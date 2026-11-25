

## **一、创建表（CREATE TABLE）**

**作用：** 在数据库中创建一张新表。

基本语法：

```
CREATE TABLE 表名 (
    列名1 数据类型,
    列名2 数据类型,
    列名3 数据类型
);
```

其中「数据类型」有哪些可选值，见 [[MySQL中的数据类型]]。

示例：

```
CREATE TABLE student (
    id INT,
    name VARCHAR(20),
    age INT,
    score DOUBLE
);
```

创建一张名为 `student` 的表，包含学号、姓名、年龄和成绩。

## **二、删除表（DROP TABLE）**

**作用：** 删除数据库中的整张表，包括表结构和表中的所有数据。

基本语法：

```
DROP TABLE 表名;
```

示例：

```
DROP TABLE student;
```

如果表存在才删除，不存在也不报错：

```
DROP TABLE IF EXISTS student;
```

注意

删除表会同时删除表中的所有数据，操作不可轻易恢复。

## **三、修改表名（RENAME TABLE）**

**作用：** 修改已有数据表的名称。

基本语法：

```
ALTER TABLE 旧表名 RENAME TO 新表名;
```

示例：

```
ALTER TABLE student RENAME TO students;
```

将 `student` 表修改为 `students`。

也可以使用：

```
RENAME TABLE student TO students;
```

## **四、修改列名（RENAME COLUMN）**

**作用：** 修改表中某个字段（列）的名称。

基本语法：

```
ALTER TABLE 表名
RENAME COLUMN 旧列名 TO 新列名;
```

示例：

```
ALTER TABLE student
RENAME COLUMN name TO username;
```

将 `student` 表中的 `name` 列修改为 `username`。

## **五、修改列类型（MODIFY COLUMN）**

**作用：** 修改表中某一列的数据类型或其他列属性。

基本语法：

```
ALTER TABLE 表名
MODIFY COLUMN 列名 新数据类型;
```

示例：

```
ALTER TABLE student
MODIFY COLUMN age VARCHAR(10);
```

将 `age` 列的数据类型从 `INT` 修改为 `VARCHAR(10)`。

注意：修改列类型时，如果原来的列有 `NOT NULL`、`DEFAULT` 等属性（这些属于约束，见 [[常见约束]]），需要根据需要一并写上，避免丢失原有属性。

## **六、添加新列（ADD COLUMN）**

**作用：** 在已有的数据表中添加一个新的字段。

基本语法：

```
ALTER TABLE 表名
ADD COLUMN 列名 数据类型;
```

示例：

```
ALTER TABLE student
ADD COLUMN address VARCHAR(100);
```

在 `student` 表中添加一个 `address` 列，用来存储学生地址。

还可以指定添加位置：

```
-- 在最前面添加
ALTER TABLE student
ADD COLUMN address VARCHAR(100) FIRST;

-- 在指定列后面添加
ALTER TABLE student
ADD COLUMN address VARCHAR(100) AFTER name;
```

## **七、删除指定列（DROP COLUMN）**

**作用：** 删除数据表中的某一个字段。

基本语法：

```
ALTER TABLE 表名
DROP COLUMN 列名;
```

示例：

```
ALTER TABLE student
DROP COLUMN address;
```

删除 `student` 表中的 `address` 列。

注意

删除列会同时删除该列中的所有数据。

# **八、知识点汇总表**

建议把下面这张表作为复习重点：

|操作|SQL 语句|作用|
|---|---|---|
|创建表|`CREATE TABLE`|创建新表|
|删除表|`DROP TABLE`|删除整张表|
|修改表名|`RENAME TO`|修改表名称|
|修改列名|`RENAME COLUMN`|修改字段名称|
|修改列类型|`MODIFY COLUMN`|修改字段类型|
|添加新列|`ADD COLUMN`|添加字段|
|删除指定列|`DROP COLUMN`|删除字段|

# **九、一个完整的记忆案例**

假设现在有一张学生表 `student`，我们依次执行下面的操作：

```
-- 1. 创建表
CREATE TABLE student (
    id INT,
    name VARCHAR(20),
    age INT
);

-- 2. 修改表名
ALTER TABLE student RENAME TO students;

-- 3. 修改列名
ALTER TABLE students
RENAME COLUMN name TO username;

-- 4. 修改列类型
ALTER TABLE students
MODIFY COLUMN age VARCHAR(10);

-- 5. 添加新列
ALTER TABLE students
ADD COLUMN address VARCHAR(100);

-- 6. 删除指定列
ALTER TABLE students
DROP COLUMN address;

-- 7. 删除整张表
DROP TABLE students;
```

## **十、几个容易混淆的地方**

|语句|区别|
|---|---|
|`DROP TABLE`|删除整张表|
|`DROP COLUMN`|只删除某一列|
|`MODIFY COLUMN`|修改列的数据类型或属性|
|`RENAME COLUMN`|只修改列名|
|`ADD COLUMN`|添加新列|

**特别记住：**

- `CREATE`：创建
    
- `DROP`：删除
    
- `ALTER`：修改表结构
    
- `RENAME`：重命名
    
- `MODIFY`：修改列属性
    

这几条语句是后面学习 Java Web、MySQL 数据库设计以及后端开发时非常基础的内容。

建议你先把这份笔记保存下来，接下来学习 `INSERT`、`UPDATE`、`DELETE`、`SELECT` 时，就能把 MySQL 的基本操作串起来了。其中 `SELECT` 的用法见 [[SELECT查询]]。