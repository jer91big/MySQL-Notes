
继续用我们熟悉的**魔法档案室**场景：档案管理员手里有一本 `students`（学生）账本，里面有 `id`、`name`、`score`、`class_name` 这些列。数据操作主要有三大动作：**塞入新行（`INSERT`）**、**涂改旧行（`UPDATE`）**、**撕掉整行（`DELETE`）**。

### 1. 塞入新记录：`INSERT INTO`（新同学报到，写进账本）

来了一位新同学，管理员铺开一张空白便签，按顺序填好信息，再夹进 `students` 账本里：

SQL

```sql
INSERT INTO students (id, name, score, class_name)
VALUES (101, '林小满', 88, 'CS1');
```

- **`INSERT INTO students (...)`**：管理员拿出 `students` 账本，并指着括号里的列顺序（“这几个格子我要填东西了”）。
    
- **`VALUES (...)`**：把对应的数据按顺序塞进格子。**列和值的数量、顺序、类型必须一一对上**，就像把方块塞进对应形状的模具，塞歪或类型不合就会报错。
    

如果你打算按表原本的列顺序把**所有列**都填满，也可以简写（不过实际开发里写明列名更安全，不怕以后表结构改列）：

SQL

```sql
INSERT INTO students
VALUES (102, '陈星宇', 94, 'CS1');
```

### 2. 涂改已有记录：`UPDATE`（改成绩、换班级，千万别忘 `WHERE`！）

`林小满` 补考后分数涨到了 `95` 分，管理员拿着橡皮擦和红笔走过去：

SQL

```sql
UPDATE students
SET score = 95
WHERE id = 101;
```

- **`UPDATE students`**：锁定要修改的账本。
    
- **`SET score = 95`**：把目标列改成新值（如果想同时改多列，用逗号隔开，比如 `SET score = 95, class_name = 'CS2'`）。
    
- **`WHERE id = 101`**：**最关键的瞄准镜！** 管理员只改 `id = 101` 那一行，其他同学动都不动。
    

> **高危动画警告**：如果写 `UPDATE students SET score = 95;` 却**漏掉了 `WHERE`**，管理员就会像失控的印章机一样，把**全校所有学生**的成绩全都改成 `95` 分！改数据前，先用 `SELECT ... WHERE ...` 瞄准确认一遍是个好习惯。
> 
>   

### 3. 删除记录：`DELETE FROM`（退学注销，整行撕掉）

有位同学转走了，管理员找到他的那一行记录，整行划掉或撕除：

SQL

```sql
DELETE FROM students
WHERE id = 102;
```

- **`DELETE FROM students`**：指定从哪张表删数据。
    
- **`WHERE id = 102`**：精准锁定只删 `id = 102` 这一整行。注意，`DELETE` 删的是**整行记录**，不是只清空某一格（清空某一格叫 `UPDATE ... SET 列 = NULL`）。
    

> **同样的高危动画警告**：如果执行 `DELETE FROM students;` 却**没有 `WHERE`**，管理员会把整张表里的**每一行数据全部清空**（表结构还在，账本空了）。

### 4. 三大动作的“安全检查清单”

在真实项目里，改和删比查更有分量，脑子里过一遍这三步：

1. **先 `SELECT` 看一眼**：在跑 `UPDATE` 或 `DELETE` 之前，先把后面的 `WHERE` 条件放进 `SELECT * FROM students WHERE ...` 里看一眼，确认命中的正是你想改或删的那几行。
    
2. **核对 `WHERE` 条件**：确认条件是不是写得太宽（比如漏了 `AND` 把别的行也圈进来了）。
    
3. **区分 `DELETE` 和 `TRUNCATE`**：`DELETE` 可以带 `WHERE` 挑着删；而 `TRUNCATE TABLE students;` 则是直接把整张表“推倒重来、清空所有行”，通常不带 `WHERE`，也比逐行 `DELETE` 快得多。

---

**下一篇：** [[02-MySQL约束/01-常见约束|常见约束]]
