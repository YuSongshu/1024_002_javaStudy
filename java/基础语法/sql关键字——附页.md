# sql关键字

1.DQL（数据查询语言）

**SELECT**：查询数据（核心） **FROM**：指定表/数据源 **WHERE**：行级条件过滤  **GROUP BY**：分组（配合聚合函数）  **HAVING**：分组后过滤（对聚合结果）  **ORDER BY**：排序（ASC/DESC）  **DISTINCT**：去重  **LIMIT/OFFSET**：分页（MySQL）  **TOP**：取前N行（SQL Server）



2.DML（数据操作语言）—— 增删改

**INSERT**：插入数据 **DELETE：**删除数据  **MERGE**：合并数据（部分数据库） **UPDATE**：更新数据  



3.DDL（数据定义语言）—— 建库/建表/改结构  

**CREATE**：创建（库、表、视图、索引）  **ALTER**：修改结构（表、列）  **DROP**：删除（表、库、索引）  **TRUNCATE**：清空表（比 DELETE 快）  **RENAME**：重命名 



4.DCL（数据控制语言）—— 权限

  **GRANT**：授权  **REVOKE**：撤销权限  **DENY**：拒绝权限 



5.TCL（事务控制语言）  

**COMMIT**：提交事务  **ROLLBACK**：回滚  **SAVEPOINT**：保存点  ### 二、常用辅助关键字  **AS**：别名（可省略）  **AND/OR/NOT**：逻辑运算  **BETWEEN ... AND ...**：范围判断  **IN**：多值匹配  **LIKE**：模糊匹配（%、_）  **IS NULL/IS NOT NULL**：空值判断  **JOIN/INNER/LEFT/RIGHT/FULL**：多表连接  **UNION/UNION ALL**：结果集合并  **ANY/SOME/ALL**：子查询比较





执行顺序（面试高频） 1. **FROM** → 2. **WHERE** → 3. **GROUP BY** → 4. **HAVING** → 5. **SELECT** → 6. **ORDER BY** → 7. **LIMIT** 