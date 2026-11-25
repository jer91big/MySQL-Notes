# AS 和 DISTINCT

今天我们来学习 SQL 中两个非常常用的关键字：`AS` 和 `DISTINCT`。

它们都不难，但在实际查询中出现频率非常高，尤其是以后学习多表查询、数据统计时。

单表查询的整体框架见 [[01-SELECT查询]]，多表查询中的表别名用法见 [[03-多表查询]]。

## 一、`AS`：给字段或表起别名

`AS` 的作用是：**为字段或表设置一个临时名称，也叫别名（Alias）。**

你可以把它理解为给数据起一个更容易理解的昵称。

### 1. 给字段起别名

假设我们有一张学生表 `students`：

| id | name | age |
| --- | --- | --- |
| 1 | 张三 | 20 |
| 2 | 李四 | 21 |
| 3 | 王五 | 19 |

我们执行：

```sql
SELECT
    name AS 学生姓名,
    age AS 学生年龄
FROM students;
```

查询结果：

| 学生姓名 | 学生年龄 |
| --- | --- |
| 张三 | 20 |
| 李四 | 21 |
| 王五 | 19 |

这里：

- `name` 是原来的字段名。
- `学生姓名` 是起的别名。
- `AS` 负责连接原名称和别名。

**注意：** `AS` 不会修改数据库中的真实字段名称，只会修改查询结果中显示的名称。

### 2. 给计算结果起别名

这个在实际开发中非常常见。

```sql
SELECT
    name,
    age + 1 AS next_age
FROM students;
```

意思是：

1. 查询学生姓名。
2. 将学生年龄加 1。
3. 把计算结果命名为 `next_age`。

查询结果：

| name | next_age |
| --- | --- |
| 张三 | 21 |
| 李四 | 22 |
| 王五 | 20 |

你之前学过的 SQL 字符串函数也可以使用 `AS`（见 [[02-字符串函数]]）：

```sql
SELECT
    name,
    LENGTH(name) AS name_length
FROM students;
```

这里 `name_length` 就是计算出来的姓名长度。

### 3. 给表起别名

`AS` 不仅能给字段起别名，也能给表起别名。

```sql
SELECT
    s.name,
    s.age
FROM students AS s;
```

这里：

- `students` 是原表名。
- `s` 是表的别名。
- `s.name` 就相当于 `students.name`。

以后学习多表查询时，表别名会非常重要（见 [[03-多表查询]]）。

**补充：** MySQL 中 `AS` 可以省略：

```sql
SELECT name AS 学生姓名 FROM students;
```

等价于：

```sql
SELECT name 学生姓名 FROM students;
```

不过建议初学阶段保留 `AS`，可读性更好。

## 二、`DISTINCT`：去除重复数据

`DISTINCT` 的作用是：**去除查询结果中的重复记录，只保留不同的值。**

假设有一张学生表：

| id | name | major |
| --- | --- | --- |
| 1 | 张三 | 软件工程 |
| 2 | 李四 | 计算机科学 |
| 3 | 王五 | 软件工程 |
| 4 | 赵六 | 软件工程 |
| 5 | 孙七 | 计算机科学 |

现在我们想知道学校有哪些专业。

如果执行：

```sql
SELECT major
FROM students;
```

结果：

| major |
| --- |
| 软件工程 |
| 计算机科学 |
| 软件工程 |
| 软件工程 |
| 计算机科学 |

发现专业重复了。

这时候就可以使用 `DISTINCT`：

```sql
SELECT DISTINCT major
FROM students;
```

结果：

| major |
| --- |
| 软件工程 |
| 计算机科学 |

重复的专业被去掉了。

### 1. `DISTINCT` 可以查询多个字段

例如：

```sql
SELECT DISTINCT major, age
FROM students;
```

这里要特别注意：

**`DISTINCT` 是对多个字段组合起来的结果进行去重，而不是分别对每个字段去重。**

例如：

| major | age |
| --- | --- |
| 软件工程 | 20 |
| 软件工程 | 20 |
| 软件工程 | 21 |
| 计算机科学 | 20 |

去重后：

| major | age |
| --- | --- |
| 软件工程 | 20 |
| 软件工程 | 21 |
| 计算机科学 | 20 |

只有两个字段都相同，才会被认为是重复记录。

### 2. `DISTINCT` 和 `COUNT` 配合使用

这是非常实用的组合。

假设我们想知道学生表中一共有多少种不同的专业。

```sql
SELECT COUNT(DISTINCT major) AS major_count
FROM students;
```

结果：

| major_count |
| --- |
| 2 |

拆开理解：

- `DISTINCT major`：找出不重复的专业。
- `COUNT()`：统计数量（见 [[03-聚合函数与分组查询]]）。
- `AS major_count`：给统计结果起别名。

## 三、`AS` 和 `DISTINCT` 对比记忆

| 关键字 | 作用 | 记忆方法 |
| --- | --- | --- |
| `AS` | 起别名 | 给字段或表改个显示名称 |
| `DISTINCT` | 去重 | 相同的数据只保留一份 |

记忆口诀：

- `AS`：改名字，不改数据。
- `DISTINCT`：去重复，不改原表。

---

**下一篇：** [[03-MySQL查询/03-多表查询|多表查询]]
