# DQL

## SQL指令

DQL（数据查询语言）：查询数据 （SELECT）

DDL （数据定义语言）：创建表   （INSERT、UPDATE、DELETE）

DML（数据操作语言）：添加 修改 删除 （CREATE、ALTER、DROP） 

DCL（数据控制语言）：权限管理（GRANT、REVOKE）



查询 Select

### DQL语法结构

SELECT  字段列表 

> 指的是查询出来的列

FROM 查询表

> 指的是查询的表

WHERE 条件

> 指的是通过条件筛选数据 

GRUOP BY 分组字段

> 一个列，纵向数据 

ORDER BY 排序字段

> 排序

LIMIT 分页条件 

> 指的是 一页几条数据



## 分类讲解

- 基础查询
- 条件查询
- 聚合函数（计算数据 count max min）
- 分组查询
- 排序查询
- 分页查询 



### 基础查询

#### 指定字段查询数据

1.指定字段查询数据

```sql
select 列,.... from 表名;
```

```sql
select bname,bprice from books;
```



#### 查询所有列

2.查询所有列

```sql
select * from books;
```



#### 起别名 

3.查询书籍的种类，起别名 

```sql
select 列 as 自己起别名 from 表名;
select 列 自己起别名 from 表名;
SELECT btype AS type FROM books;
```



#### DISTINCT 去重

4.查询种类，但是我不要重复的

```sql
SELECT DISTINCT btype  FROM books;
```



### 条件查询

#### 基本语法

```sql
select 列,.... from 表名 where 条件 ....
```



##### 条件

> 比较： > >= < <= = !=  like(模糊) between 区间 
>
> 逻辑： && and   || or  ！ not



1.查询大于30的书籍

```sql
SELECT * FROM books WHERE bprice > 30;
```



2.查询书籍名称叫盗墓笔记

```sql
SELECT * FROM books WHERE bname = '盗墓笔记';
```

###### like:模糊查询

```sql

where bname like '%张'   张结尾的数据

where bname like '张%'   张开头的数据

where bname like '%张%'  数据只要带张查询

SELECT * FROM books WHERE bname LIKE '%盗墓笔记%';
```



3.价格20-30之间的书籍 

```sql
SELECT * FROM books WHERE bprice BETWEEN 20 AND 30;
```

```sql
SELECT * FROM books WHERE bprice > 20 AND bprice < 30; 
```



4.查询年份03年1月份 到 03年12月份 中间 出版的书籍 

```sql
SELECT * FROM books WHERE bdate BETWEEN '2003-01' AND '2003-12';
```

```sql
SELECT *FROM booksWHERE publish_date >= '2003-01' AND publish_date <= '2003-12';
```

注：

数据库会把 '2003-01' 自动补全为 '2003-01-01 00:00:00'
把 '2003-12' 自动补全为 '2003-12-01 00:00:00'
结果：只查到 2003-01-01 到 2003-12-01 0 点之间的数据，漏掉 2003-12-02 至 2003-12-31 的所有记录

```sql
SELECT * FROM books WHERE bdate BETWEEN '2003-01' AND '2004-01-01'; 
#但会把 2004-01-01 00:00:00 也算进去
SELECT *FROM booksWHERE publish_date >= '2003-01' AND publish_date < '2004-01-01';
#不漏数据 不含2004-01-01
```

5.查询计算机类和动漫类的书籍

```sql
SELECT * FROM books WHERE btype = '计算机类' OR btype = '动漫类'
```

```sql
SELECT * FROM books WHERE btype IN('计算机类','动漫类');
```



6.查询种类为空的数据 

```sql
SELECT * FROM books WHERE btype IS NOT NULL;
```



### 聚合函数

> 方法，mysql写的，

```sql
count统计数量  max最大值 min最小值 sum和  svg平均值
```

> 应该搭配分组查询一起使用的。

```sql
select 聚合函数(列) from 表名 
```



1.统计一共有多少本书

```sql
SELECT COUNT(id) FROM books
```

注：

COUNT3 种用法：

COUNT (*)：统计**所有行（含 NULL）**

COUNT (列)：统计列**非 NULL 值**行数

COUNT (DISTINCT 列)：统计列**去重后非 NULL 值**



2.查询一下表中最贵的书是多少钱 

```sql
SELECT max(bprice) FROM books
```

min()同理

3.统计 求和 一共多少钱

```sql
SELECT SUM(bprice) FROM books
```



4.平均值

AVG()

```
SELECT AVG(cj) FROM score;
```



### 分组查询

```sql
select 列 from 表名 [where 条件] gruop by 分组列 [having 分组之后的过滤]
```

> where:查询的到数据之前 筛选
>
> having：得到数据之后，在进行条件过滤



1.基础查询，根据书籍分类分组

```sql
SELECT btype FROM books GROUP BY btype 
```



2.标准的分组查询，搭配聚合函数，统计数量

```sql
SELECT btype,count(id) FROM books GROUP BY btype
```



3.只想要计算机类的书籍

```sql
SELECT btype,count(id) FROM books WHERE btype  = '计算机类' GROUP BY btype    
```

```sql
SELECT btype,count(id) FROM books GROUP BY btype   HAVING  btype  = '计算机类'
```

注：

SELECT 后面只能出现两种内容:`分组字段` `聚合函数`

```
原因：
GROUP BY 会把多行数据合并成一行（分组）
一旦合并，原来的普通字段就不存在了，数据库不知道该显示哪一行的值，所以不允许出现
分组字段
合并后依然唯一，数据库知道显示什么
eg:
name	class	score
小明		1 班		80
小红		1 班		90
小刚		2 班		70
SELECT class FROM student GROUP BY class;
输出：
1班
2班

1 班合并成一行
但 1 班有 小明、小红 两个 name
数据库不知道显示谁，所以直接报错
```

WHERE 不能用聚合函数

```
WHERE 是在分组之前执行的，聚合函数是在分组之后才能计算的(详见06MySQL初识)
```

HAVING 必须跟在 GROUP BY 后面

```
HAVING 的作用：对分组后的结果进行筛选
GROUP BY 的作用：先把数据分组
没有分组，就没有 HAVING
```





### 排序查询

> 排序数据

```sql
select 列 from 表名 [where] order by 排序列
```

> 多列排序的，默认 升序 asc  降序 desc 

1.按照价格 升序排序 

```sql
SELECT * FROM books ORDER BY bprice
```

```sql
SELECT * FROM books ORDER BY bprice DESC
```



2.价格一样，再次按照id排序

```sql
SELECT * FROM books ORDER BY bprice,id DESC
```





### 分页查询

```sql
select 列 from 表名 [where] limit 起始索引,查询记录数;
```

```
select 列 from 表名 [where] limit 查询记录数;
```

1.查询五条数据

```sql
SELECT * FROM books LIMIT 5;
```



2.第二页

```sql
SELECT * FROM books LIMIT 10,5;

（页码 - 1） * 每页条数
```



3.查询最畅销类型的书籍，只要一个

```sql
SELECT btype,COUNT(id) FROM books GROUP BY btype

SELECT btype,COUNT(id) x FROM books GROUP BY btype ORDER BY  x desc LIMIT 1
```

