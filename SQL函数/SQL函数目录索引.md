# SQL 函数

SQL 函数可以理解为查询时的“微型加工机”：输入数据，输出加工后的结果。按处理方式，大致可以分成两类：

- **逐行加工**：字符串函数、数值函数、日期函数、流程函数——每一行输入对应一行输出。
- **多行汇总**：聚合函数——多行输入汇总成一个结果。

### 笔记目录

- [[SQL函数/字符串函数]]：`UPPER`、`LOWER`、`TRIM`、`SUBSTRING`、`CONCAT`、`REPLACE` 等文本处理函数。
- [[SQL函数/聚合函数与分组查询]]：`COUNT`、`SUM`、`AVG`、`MAX`、`MIN`，以及 `GROUP BY` / `HAVING` 分组统计。
- [[SQL函数/流程函数]]：`CASE WHEN` 条件判断。

### 后续待补充

- 数值函数：`ROUND`、`CEIL`、`FLOOR`、`ABS`、`MOD`、`RAND`
- 日期时间函数：`NOW()`、`CURDATE()`、`DATE_FORMAT`、`DATEDIFF`、`DATE_ADD`
- NULL 处理函数：`IFNULL()`、`COALESCE()`、`NULLIF()`

查询基础可以看 [[SELECT查询]]。
