欢迎来到 SQL 的世界！你可以把**数据库（Database）**想象成一个巨大的**魔法档案室**，里面摆满了整齐的**表格（Table）**，就像一本本分类账本。每一行是一条**记录（Row）**，每一列是一个**属性字段（Column）**。

而 **SQL（特别是 `SELECT` 查询）**，就是你递给档案管理员的**寻宝咒语**——只要念对顺序，管理员就会嗖地一下把你要的东西翻出来交到你手上！

`SELECT` 属于 SQL 五大分类中的 **DQL（数据查询语言）**，五大分类的完整体系可以看 [[MySQL的分类]]。

### 1. 最基础的双人舞：`SELECT` + `FROM`

假设档案室里有一张叫 `students`（学生）的表格（建表过程见 [[MySQL 表的创建、删除与修改]]，列类型见 [[MySQL中的数据类型]]），里面记着名字、班级和分数。你想把所有学生的名字和分数看一眼：

```sql
SELECT name, score
FROM students;
```

- **`FROM students`**：管理员先转身走向标着 `students` 的书架，把整本账本抱到桌面上（**锁定目标**）。
- **`SELECT name, score`**：接着，你用手指点出账本上只要看 `name` 和 `score` 这两列，别的先别管（**挑出字段**）。
- **分号 `;`**：相当于一句咒语念完，轻轻打个响指。

如果你想把整张表的所有列都看个遍，可以用星号 `*`（代表“全都要”）：

```sql
SELECT *
FROM students;
```

### 2. 加上滤镜：`WHERE`（按条件筛人）

整张表人太多了，你只想找**分数大于等于 90 分**的学霸：

```sql
SELECT name, score
FROM students
WHERE score >= 90;
```

你可以把 `WHERE` 想象成一道**安检门**。每一行数据走过来时，安检门都会检查一下：`score >= 90` 吗？通过的放行进入结果，没通过的留在原地。

常见安检暗号：

- `=` 等于，`!=` 或 `<>` 不等于，`>`、`>=`、`<`、`<=`
- `AND`（并且）、`OR`（或者）——比如 `WHERE class = 'A' AND score >= 90`

除了大小于号和 `AND` / `OR`，安检门还认一整套暗号，下面逐个拆开。

#### `IN`：点名白名单

想知道学生是不是来自 `'CS1'`、`'CS2'` 或 `'CS3'`，不用连写三个 `OR`，直接递上一张**白名单**，命中名单里任意一个就放行：

```sql
SELECT name, class_name
FROM students
WHERE class_name IN ('CS1', 'CS2', 'CS3');
```

#### `BETWEEN ... AND`：闭区间围栏

在成绩单上划出 `80` 到 `90` 的围栏，**包含两端边界**（等价于 `score >= 80 AND score <= 90`）：

```sql
SELECT name, score
FROM students
WHERE score BETWEEN 80 AND 90;
```

#### `LIKE` + `%` / `_`：模糊寻人镜

- **`%`** 代表**任意长度（0 个、1 个或多个）字符**，像伸缩网：`LIKE '张%'` 抓出所有姓“张”的（“张”、“张三”、“张无忌”）；`LIKE '%三%'` 抓出名字里带“三”的。
- **`_`** 代表**严格 1 个字符**，像单人格子：`LIKE '李_'` 只抓两个字的名字（如“李明”），抓不到“李思思”。

```sql
SELECT name
FROM students
WHERE name LIKE '陈__'; -- 姓“陈”且总共 3 个字
```

#### `IS NULL` / `IS NOT NULL`：空缺探测仪

**为什么 `= NULL` 查不到任何数据？** 在 SQL 里，`NULL` 表示“未知 / 还没填”，不是具体的值。拿“未知”去跟“未知”做 `=` 比较，结果既不是真也不是假，而是“未知”，安检门不会放行！所以必须用 `IS NULL` 测出“这一格是不是空缺”，用 `IS NOT NULL` 测出“是不是已经填了内容”。

```sql
SELECT name FROM students WHERE score IS NULL;     -- 还没录入成绩的
SELECT name FROM students WHERE score IS NOT NULL; -- 已经有成绩的
```

#### `NOT`：反转开关

把安检结果调个头——原本放行的拦下，原本拦下的放行。常和前面几个合体：`NOT IN`、`NOT BETWEEN`、`NOT LIKE`。

```sql
SELECT name, class_name
FROM students
WHERE class_name NOT IN ('CS1', 'CS2');
```

### 3. 起别名与去重：`AS` 和 `DISTINCT`

- **`AS`**：给字段或表起一个临时名称，只改查询结果的显示名称，不改真实表结构和数据。
- **`DISTINCT`**：对查询结果去重；如果后面跟多列，则按整行组合去重。

详细用法、示例和对比记忆见 [[AS 和 DISTINCT]]。

### 4. 排排队：`ORDER BY`（按规矩排序）

挑出来的学霸名单乱糟糟的，你想让**分数高的排在最前面**（降序 `DESC`），如果分数一样，就按名字字母顺着排（升序 `ASC`，默认就是它）：

```sql
SELECT name, score
FROM students
WHERE score >= 90
ORDER BY score DESC, name ASC;
```

想象一下，筛选完的数据像一群小朋友站成一队，管理员吹响哨子：“按分数从高到低重新报数排队！”

### 5. 掐头去尾：`LIMIT`（只看前几名）

榜单太长，你只想看**前三名**的冠军：

```sql
SELECT name, score
FROM students
WHERE score >= 90
ORDER BY score DESC
LIMIT 3;
```

队伍排好后，管理员拿出一把剪刀，咔嚓剪掉第 4 名及以后的所有人，只留下最前面的 3 个交给你。

#### 分页：翻书页的书签

做网页列表或报表时，一页通常只显示几条，翻到第 2 页、第 3 页就要用到**偏移量（跳过几条）**和**条数（拿几条）**。假设每页显示 `5` 条（`page_size = 5`）：

- **第 1 页**：跳过 `0` 条，拿 `5` 条 → `LIMIT 5`（或 `LIMIT 0, 5` / `LIMIT 5 OFFSET 0`）
- **第 2 页**：跳过前 `5` 条，拿 `5` 条 → `LIMIT 5, 5`（MySQL 语法：`LIMIT 偏移量, 行数`）或 `LIMIT 5 OFFSET 5`（标准 / PostgreSQL 等常见写法：`LIMIT 行数 OFFSET 偏移量`）
- **第 `k` 页**：跳过 `(k - 1) * page_size` 条，拿 `page_size` 条。

```sql
-- 查第 3 页（每页 5 条，跳过前 10 条，取第 11~15 条）
SELECT id, name, score
FROM students
ORDER BY id ASC
LIMIT 10, 5;
```

> **分页小叮咛**：分页几乎一定要搭配 `ORDER BY`！没有排序时，数据库每次翻出来的顺序可能不稳定，翻着翻着就会出现漏数据或重复数据的“鬼打墙”。

### 6. 咒语的“幕后执行顺序”（超重要！）

初学者最容易踩的坑是：**你写的顺序，不是数据库干活的顺序！**

- **你写出来的顺序**：`SELECT` → `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `ORDER BY` → `LIMIT`
- **管理员实际干活的顺序**：
    1. **`FROM`**：先抱出哪张表；
    2. **`WHERE`**：再用安检门过滤掉不符合条件的行；
    3. **`GROUP BY`**：把剩下的行按指定列分成小组；
    4. **`HAVING`**：再筛掉不符合条件的聚合小组；
    5. **`SELECT`**：接着从剩下的行里挑出需要的列（甚至做计算）；
    6. **`ORDER BY`**：然后把挑出来的结果排好队；
    7. **`LIMIT`**：最后截断出指定的几条。

其中 `GROUP BY` / `HAVING` 属于分组统计，详细用法见 [[聚合函数与分组查询]]。

记住这个幕后动画，以后写复杂查询时，你就知道哪一步发生了什么，不容易迷路啦！

### 7. 延伸阅读

- 多表查询（连接、子查询、`UNION`）：[[多表查询]]
- 聚合与分组统计：[[聚合函数与分组查询]]
- 条件判断（`CASE WHEN`）：[[流程函数]]
