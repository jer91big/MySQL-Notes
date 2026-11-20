MySQL 中的**约束（Constraint）**是数据库非常重要的知识点。它主要用来限制表中数据的存储规则，保证数据的正确性和完整性。

你可以先记住一句话：

**约束就是给数据库中的数据设置规矩，不符合规矩的数据不能随便存进去。**

# **一、MySQL 常见的六种约束**

MySQL 常见约束有以下六种：

|约束|关键字|作用|
|---|---|---|
|非空约束|`NOT NULL`|不允许字段为 NULL|
|唯一约束|`UNIQUE`|字段值不能重复|
|主键约束|`PRIMARY KEY`|唯一标识表中的每一条记录|
|默认约束|`DEFAULT`|没有指定值时使用默认值|
|检查约束|`CHECK`|检查数据是否符合指定条件|
|外键约束|`FOREIGN KEY`|建立两个表之间的关联关系|

我们一个一个来看。

# **二、非空约束（NOT NULL）**

**作用：** 限制某个字段不能为空（NULL）。

例如：学生表中的姓名必须填写，不能留空。

```
CREATE TABLE student (
    id INT,
    name VARCHAR(20) NOT NULL,
    age INT
);
```

这里：

```
name VARCHAR(20) NOT NULL
```

表示 `name` 字段不允许为 NULL。

例如：

```
INSERT INTO student (id, name, age)
VALUES (1, '张三', 20);
```

允许

```
INSERT INTO student (id, name, age)
VALUES (2, NULL, 20);
```

不允许

因为姓名设置了非空约束。

**记忆：NOT NULL = 不能为空。**

# **三、唯一约束（UNIQUE）**

**作用：** 保证某个字段中的数据不能重复。

例如：学生的手机号不能重复。

```
CREATE TABLE student (
    id INT,
    name VARCHAR(20),
    phone VARCHAR(20) UNIQUE
);
```

这样：

|id|name|phone|
|---|---|---|
|1|张三|13800000001|
|2|李四|13800000002|
|3|王五|13800000001|

第三条数据会插入失败，因为手机号重复了。

注意：

- `UNIQUE` 允许存在 NULL。
    
- MySQL 的唯一索引允许存在多个 NULL 值。
    
- 如果既不能重复，又不能为 NULL，可以同时使用 `UNIQUE NOT NULL`。
    

**记忆：UNIQUE = 不能重复。**

# **四、主键约束（PRIMARY KEY）**

重点掌握

**作用：** 唯一标识表中的每一条记录。

例如：学生表中的学号，每个学生都应该有一个独一无二的学号。

```
CREATE TABLE student (
    id INT PRIMARY KEY,
    name VARCHAR(20),
    age INT
);
```

这里：

```
id INT PRIMARY KEY
```

表示 `id` 是主键。

主键有两个重要特点：

1. 不能为 NULL。
    
2. 不能重复。
    

例如：

|id|name|age|
|---|---|---|
|1|张三|20|
|2|李四|21|
|3|王五|19|

如果再插入一条 `id = 1` 的记录，就会报错。

**主键和唯一约束有什么区别？**

|对比|PRIMARY KEY|UNIQUE|
|---|---|---|
|是否允许重复|不允许|不允许|
|是否允许 NULL|不允许|允许|
|每张表数量|只能有一个主键约束|可以有多个唯一约束|
|主要作用|唯一标识记录|保证字段值唯一|

补充：一个主键可以由多个字段共同组成，叫作**联合主键**。

例如：

```
CREATE TABLE score (
    student_id INT,
    course_id INT,
    score DOUBLE,
    PRIMARY KEY (student_id, course_id)
);
```

表示学生 ID 和课程 ID 组合起来作为主键。

# **五、默认约束（DEFAULT）**

**作用：** 当插入数据时没有给某个字段指定值，就使用默认值。

例如：学生表中的状态默认是正常状态。

```
CREATE TABLE student (
    id INT PRIMARY KEY,
    name VARCHAR(20),
    status VARCHAR(20) DEFAULT '正常'
);
```

插入数据：

```
INSERT INTO student (id, name)
VALUES (1, '张三');
```

因为没有指定 `status`，所以数据库会自动使用默认值。

结果：

|id|name|status|
|---|---|---|
|1|张三|正常|

注意：如果显式插入 NULL，默认值不会自动替代 NULL。

**记忆：DEFAULT = 没有指定值时，使用默认值。**

# **六、检查约束（CHECK）**

**作用：** 限制字段中的数据必须满足指定条件。

例如：学生年龄必须大于等于 18 岁。

```
CREATE TABLE student (
    id INT PRIMARY KEY,
    name VARCHAR(20),
    age INT CHECK (age >= 18)
);
```

这样：

```
INSERT INTO student VALUES (1, '张三', 20);
```

允许。

```
INSERT INTO student VALUES (2, '李四', 16);
```

不允许，因为年龄不符合条件。

还可以设置：

```
score DOUBLE CHECK (score >= 0 AND score <= 100)
```

表示成绩必须在 0～100 之间。

注意：MySQL 从 **8.0.16** 开始才真正执行 CHECK 约束，旧版本可能只解析而不执行。

**记忆：CHECK = 必须满足指定条件。**

# **七、外键约束（FOREIGN KEY）**

重点掌握

外键是初学者比较容易混淆的一个知识点。

先看一个实际场景：

一个学校有两个表：

**1. 班级表 class**

|id|class_name|
|---|---|
|1|软件1班|
|2|软件2班|

**2. 学生表 student**

|id|name|class_id|
|---|---|---|
|1|张三|1|
|2|李四|1|
|3|王五|2|

你会发现：

学生表中的 `class_id` 对应班级表中的 `id`。

也就是说，学生属于哪个班级，由 `class_id` 决定。

这时候就可以使用外键约束。

**1. 创建班级表**

```
CREATE TABLE class (
    id INT PRIMARY KEY,
    class_name VARCHAR(20)
);
```

**2. 创建学生表**

```
CREATE TABLE student (
    id INT PRIMARY KEY,
    name VARCHAR(20),
    class_id INT,
    FOREIGN KEY (class_id) REFERENCES class(id)
);
```

重点理解这句话：

```
FOREIGN KEY (class_id) REFERENCES class(id)
```

拆开来看：

- `FOREIGN KEY (class_id)`：指定学生表中的 class_id 是外键。
    
- `REFERENCES class(id)`：这个外键关联班级表中的 id。
    

这样就建立了两个表之间的关联。

例如：

```
INSERT INTO class VALUES (1, '软件1班');

INSERT INTO student VALUES (1, '张三', 1);
```

允许

因为班级表中存在 id = 1。

但是：

```
INSERT INTO student VALUES (2, '李四', 3);
```

不允许

因为班级表中不存在 id = 3。

外键可以防止出现不存在的班级，却有学生属于这个班级的情况。

**记忆：FOREIGN KEY = 建立表与表之间的关联，保证引用的数据存在。**

补充：外键引用的字段必须是被引用表中具有索引的字段，通常使用主键。实际项目中还需要考虑删除、更新时的处理规则，例如：

```
FOREIGN KEY (class_id)
REFERENCES class(id)
ON DELETE CASCADE
```

`ON DELETE CASCADE` 表示删除被引用的班级记录时，关联的学生记录也会被删除。使用时要谨慎。

# **八、六种约束总结**

这张表建议直接保存到笔记中：

|约束|核心作用|记忆关键词|
|---|---|---|
|NOT NULL|不允许为空|必填|
|UNIQUE|不允许重复|唯一|
|PRIMARY KEY|唯一标识记录|身份证|
|DEFAULT|自动使用默认值|默认|
|CHECK|满足指定条件|检查|
|FOREIGN KEY|关联其他表|表关系|

## **九、综合案例：创建一张学生表**

把前面六种约束结合起来：

```
CREATE TABLE student (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    phone VARCHAR(20) UNIQUE,
    age INT CHECK (age >= 18),
    status VARCHAR(20) DEFAULT '正常',
    class_id INT,
    FOREIGN KEY (class_id) REFERENCES class(id)
);
```

这里额外出现了一个知识点：

`AUTO_INCREMENT`：自动增长。

例如插入学生时：

```
INSERT INTO student (name, phone, age, class_id)
VALUES ('张三', '13800000001', 20, 1);
```

不需要手动填写 id，MySQL 会自动生成一个递增的 ID。

注意：`AUTO_INCREMENT` 是列属性，不属于上面六种约束。

# **十、课后练习**

按照我们之前学习 HTML/CSS 和操作系统的方式，我们一题一题巩固。

**约束专项练习**

1 / 4

**第** **1** **题**

一个学生表中，要求手机号不能重复，但允许为空，应该使用什么约束？

A. NOT NULL

B. UNIQUE

C. PRIMARY KEY

D. DEFAULT

提交答案

这六种约束中，我建议你优先掌握 **PRIMARY KEY、UNIQUE、FOREIGN KEY**。尤其是外键，它是理解后面数据库多表关系、Java 后端开发中表结构设计的基础。