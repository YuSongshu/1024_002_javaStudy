# 初识Mysql

关联文件：D:\博客\python\数据分析\MySql\01初识Mysql数据库——.md

## 数据库单独软件

java  jdk   1.8  1.9   10 -> 24  25     以后看公司要求

开发工具 eclipse  比较老  兼容jdk 1.8    

用idea 2020 兼容不了 JDK 24      装了一个 2021  JDK  24  不好使

mysql5   8    9   公司中用的多的  8   其次5    9最新   

version 版本号

port  mysql 官方端口 3306    tomcat 端口  8080  明星端口

酷狗音乐  8080    只要听歌就启动不了 tomcat

浏览器默认访问端口  80

oracle  1521 

sqlserver  1433

mysql  root   

超级管理员（拥有最高的权限）   可以自己设置密码

## 数据库

学习数据的目的 （为了我们开发的程序产生的数据能永久的保留  保留   方便我们查询呢）

数据库给我们提供了一种  数据永久存储化的机制 （持久化）  还有功能强大的查询机制

从程序的角度来讲  关心的就是四个字   `CRUD`   增删改查



sql语言  数据库的专用语言   程序员必须学会的一种语言  

业界数据库分为两种

​		1.关系型   sql型    特点结构化存储数据       代表 mysql  oracle  sqlserver  db2  基于硬盘

​			人口系统中  存储公民身份信息的表   300多列  有一列 

​			关系 （一对一   一对多   多对多）    

​			sql 一种第四代计算机编程语言  纯命令形式  写完了按回车  好使就好使  不好使就error  错误信息

​			表   存储数据的基本单位 （我们玩数据库 要想存储数据必须点自己设计和建立表（数据库的单位之一））

​					二维的  结构化（二维  有行  有列  每行的列数相等）    有行  有列

​					存储 student 信息														'多'

id(int) primary key			name（varchar(10)）unique		age not  null		teacher_id     

1							 张三							    20					1

2			 				李四							   21						1



​					存储teacher     你想存储一个实体  只能用一张表   一张表==一个实体类	 	 '一'

​					id		name

​					1		李老师

​					2		曾老师

​		

​		2.非关系型   nosql型    特点采用非结构化的方式存储数据  （可以对数据的瞬间存储能力得到很大的提升） 基于内存 （可以让数据短暂在内存中永久化保留）  redis   hbase

​				没有表  所以数据和数据之间无法建立关系

​				一般数据采用 键值对的形式存储  而不是二维的形式  

​				key-value

​				id-1

​				name-张三





## 数据库单位

### 基本单位

user     用户   root

database  仓库   

table     表 （存储数据的单位）

column   字段/列  （数据类型   大小    约束）

check 约束

​	常用的有四种 约束

| 约束        |            | 效果                                                         |
| ----------- | ---------- | ------------------------------------------------------------ |
| primary key | 主键约束   | 不能重复  不能为空   （一个表中只能有一个）                  |
| unique      | 唯一约束   | 不能重复                                                     |
| not  null   | 非空约束   | 不能为空                                                     |
| default     | 默认值约束 | 不添加数据 数据库自动添加的数据                              |
| foreign key | 外键约束   | 引用另一张表的主键，保证数据关联正确，防止乱填无效数据，保持表之间关系一致（关联的外键无法随意修改，除非两方都有） |

​		

### 高级单位

index  索引

function  函数

view   视图

produce  过程

trriger  触发器

### 单位的创建和使用顺序

​			1.创建一个用户   默认存在  root   超级管理员

​				看一看当前用户下有多少仓库

​			2.创建一个仓库  database

​			3.进入仓库

​			4.查看当前仓库中有没有表

​			5.创建表

​			6.创建表要先创建列

​			7.列有数据类型  大小 和  约束

​			8.往表中插入数据

​			9.查看表中所有的数据

​			10.修改表中的数据

​			11.删除表中的数据

​			12.对仓库  对表  对列  不满意 都可以进行编辑

​			一个用户可以管理和创建多个仓库

​			一个仓库可以管理和创建多个表

​			一个表可以管理和创建多个列

​			一个列可以管理和创建多个约束

​	

#### 拓展

SQL 执行顺序是固定的：

1. **FROM** 			找到表
2. **JOIN**                        （连接表）
3. **WHERE**  		筛选行（**还没分组**）
4. **GROUP BY** 		分组
5. **聚合函数计算**      （SUM/COUNT/AVG...）
6. **HAVING**                分组后筛选（HAVING 就是分组后的 WHERE，专门管聚合函数）
7. **SELECT**                    （查询列）
8. **DISTINCT**              （去重）
9. **ORDER BY**                （排序）
10. **LIMIT**                     （分页取数）

```sql
SELECT DISTINCT u.user_id, u.username, COUNT(o.order_id), SUM(o.order_amount) FROM users u INNER JOIN orders o ON u.user_id=o.user_id WHERE o.create_time>='2025-01-01' GROUP BY u.user_id, u.username HAVING SUM(o.order_amount)>1000 ORDER BY SUM(o.order_amount) DESC LIMIT 10;
```



#### 	

### 数据库的基本使用流程

涉及到很多的sql语言和命令

1.登录到数据库中

​	mysql    -u用户名  -p密码

2.查看当前用户下所有的仓库

​	show    databases;

3.创建仓库

​	create   database   库名;

4.进入仓库

​	use  库名;

5.查看当前仓库中所有的表

​	show   tables;

6.创建表

​	1.创建表就是创建列  不用管行  行是后插入的

​	2.创建列的语法   列名    数据类型(大小)   约束,列名    数据类型(大小)   约束

​	3.数据库的数据类型  int  整数  double 浮点数  varchar 字符串  date日期（年月日）  time 时间（时分秒）  datetime 日期时间（年月日 + 时分秒）

```sql
create table 名字 (列名 数据类型(大小) 约束,列名 数据类型(大小) 约束,列名 数据类型(大小) 约束)；
```

​	4.表==实体   存储什么实体 有什么列  （建模）

7.查看表的结构信息

​	desc  表名;

注：

Mysql 语法规范：

1. 关键字和函数名全部大写，因为小写不利于区分关键字和函数
2. 数据库名、表名、字段名全部小写
3. SQL语句必须以分号结尾



## 常用数据操作命令

#### 增（insert）

```sql
insert into ... values ...
```

```
1. 插入完整一行数据
INSERT INTO 表名 VALUES (值1, 值2, 值3, ...);			#值需要与字段对应

2. 插入指定列
INSERT INTO 表名 (字段1, 字段2, 字段3) VALUES (值1, 值2, 值3);

3. 批量插入多行
INSERT INTO 表名 (字段1,字段2) VALUES (值1,值2),(值3,值4),(值5,值6);

4. 查询结果插入
INSERT INTO 表名 (字段1,字段2) SELECT 字段1,字段2 FROM 其他表 WHERE 条件;
```

注：

字符串类型 值要加 单引号  ''

数字类型  值可以直接写



#### 删（delete）

```sql
delete from 表名 [where 条件];
```

在使用 delete 的时候和 update 一样也会与其他关键字一起使用。

例如：where

```sql
DELETE FROM 表名 WHERE 判断字段='判断的值';


DELETE FROM users WHERE id = 1;
```

否则会删除表中所有数据。



#### 改（update)

```sql
update 表名 set 字段名1 = 值1, 字段名2 = 值2, ...  [where 条件];
```

一般在使用 update 的时候都会与其他关键字一起使用。

例如：where

```sql
UPDATE 表名 SET 修改字段='修改的值' WHERE 判断字段='判断的值';
 	# 通过判断字段来找到要修改的那条数据，然后通过修改字段来修改字段中的值。

```

否则，当前表中该字段所有的值都会进行更改。



#### 查（select）

数据查询的关键字是 select，使用的语法格式如下：

```sql
SELECT 字段 FROM 表名;		#可以查询多个字段，中间用逗号隔开。
```

注：

```sql
SELECT * FROM users;
#  *：代表所有字段
```



**拓展**

```sql
select [字段]
from [表名]
[where 条件]
[group by 分组字段]
[having 筛选条件]
[order by 排序字段 [asc/desc]]
[limit 限制条数];
```









