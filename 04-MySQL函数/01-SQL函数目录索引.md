# SQL 函数

SQL 函数可以理解为查询时的“微型加工机”：输入数据，输出加工后的结果。按处理方式，大致可以分成两类：

- **逐行加工**：字符串函数、数值函数、日期函数、流程函数——每一行输入对应一行输出。
- **多行汇总**：聚合函数——多行输入汇总成一个结果。

### 笔记目录

- [[02-字符串函数]]：`UPPER`、`LOWER`、`TRIM`、`SUBSTRING`、`CONCAT`、`REPLACE` 等文本处理函数。
- [[03-聚合函数与分组查询]]：`COUNT`、`SUM`、`AVG`、`MAX`、`MIN`，以及 `GROUP BY` / `HAVING` 分组统计。
- [[04-流程函数]]：`CASE WHEN`、`IF` 条件判断，以及 `IFNULL`、`COALESCE`、`NULLIF` 空值处理。
- [[05-数值函数]]：`ROUND`、`TRUNCATE`、`CEIL`、`FLOOR`、`ABS`、`MOD`、`POWER`、`SQRT`、`RAND`。
- [[06-日期时间函数]]：`NOW()`、`CURDATE()`、`DATE_FORMAT`、`DATEDIFF`、`DATE_ADD` / `DATE_SUB`。

### 下一步

核心函数到这里就齐了（`CAST()` / `CONVERT()` 类型转换可按需再补）。接下来进入 **索引**（笔记待写）。

查询基础可以看 [[01-SELECT查询]]。

---

**下一篇：** [[04-MySQL函数/02-字符串函数|字符串函数]]
