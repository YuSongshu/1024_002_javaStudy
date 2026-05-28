# MySQL进阶

SELECT *  FROM dog

只要执行select 语句一定会从表数据中的到结果

java或python永远只能获取结果不能获取表数据

java或python只能获取select语句查询的结果无法直接获取表中的数据

数据导入导出 ->数据迁移  ETL工程师

```sql
CREATE TABLE `books` (

  `id` INT(10) NOT NULL AUTO_INCREMENT,

  `bname` VARCHAR(10) DEFAULT NULL,

  `bauthor` VARCHAR(10) DEFAULT NULL,

  `bdate` DATE DEFAULT NULL,

  `bprice` DOUBLE DEFAULT NULL,

  `btype` VARCHAR(10) DEFAULT NULL,

  PRIMARY KEY  (`id`)

) ENGINE=INNODB DEFAULT CHARSET=gb2312;
```



```sql
INSERT  INTO `books`(`id`,`bname`,`bauthor`,`bdate`,`bprice`,`btype`) VALUES (1,'盗墓笔记-七星鲁王宫','南派三叔','2001-03-20',23.5,'小说类'),(2,'盗墓笔记-怒海潜沙','南派三叔','2002-03-21',24.5,'小说类'),(3,'盗墓笔记-云顶天宫','南派三叔','2003-03-22',24.6,'小说类'),(4,'鬼吹灯-精绝古城','天下霸唱','2001-04-20',26.5,'小说类'),(5,'鬼吹灯-龙岭迷窟','天下霸唱','2002-04-21',27.5,'小说类'),(6,'鬼吹灯-云南虫谷','天下霸唱','2003-04-20',28.6,'小说类'),(7,'茅山后裔-传国玉玺','大力金刚掌','2003-06-20',29.6,'小说类'),(8,'茅山后裔-兰亭集序','大力金刚掌','2004-06-20',30.6,'小说类'),(9,'茅山后裔-将门虎子','大力金刚掌','2005-06-20',31.6,'小说类'),(10,'海贼王','尾田荣一郎','2002-06-21',19.6,'动漫类'),(11,'火影忍者','岸本齐史','2003-06-20',20.6,'动漫类'),(12,'七龙珠','鸟山鸣','2000-06-20',21.6,'动漫类'),(13,'不良人','关心','2017-06-21',23.7,'动漫类'),(14,'斗罗大陆','张三','2017-07-20',24.6,'动漫类'),(15,'一念永恒','耳根','2021-08-20',26.6,'动漫类'),(16,'C语言基础','谭浩强','2000-07-20',36.6,'计算机类'),(17,'数据结构','严蔚敏','2001-08-20',37.6,'计算机类'),(18,'操作系统','严蔚敏','2002-09-20',38.6,'计算机类'),(19,'故事会','团体','2000-07-20',16.6,'文学类'),(20,'意林','团体','2001-08-20',17.6,'文学类'),(21,'计算机报','团体','2002-09-20',18.6,'文学类'),(22,'幻城','郭敬明','2002-09-20',18.6,'小说类'),(23,'幻城','郭敬明','2002-09-20',38.6,NULL),(24,'不良人','关心','2017-06-21',23.7,'动漫类'),(25,'11','11','2017-09-22',12.8,NULL);
```

软件 图形化界面 （你点击图片人家帮你生成sql）

生成sql 可能跟我们平时用的不太一样

正常sql语法就很多种  

生成 标准语法  文言文

我们用的 应用语法  现代汉语



对这个表要有一个基本的认知 

做项目->得到需求->根据需求设计数据库 （需要几张表 每张表中都有什么列 建立表和表什么关系） 数据建模

设计一个优秀的表  

## 三范式

符合数据库的3INF   三范式

1.关系型数据要符合原子性 第一范式

2.第一范式成立才会有第二范式  关系型数据每行数据必须有唯一标识  主键

3.第二范式成立才会有第三范式  不应该在一个表中的列应该拿出去单独建表    配置关系 否则产生数据冗余

查询图书表中的图书都有哪些类型

```sql
SELECT  btype FROM books
```



![ScreenShot_2026-03-22_153455_839](D:\博客\java\辅助图片_视音频等资料\08\ScreenShot_2026-03-22_153455_839.png)

数据冗余解决方案

### DISTINCT

`DISTINCT` 是 SQL 中**用于去除查询结果中重复数据**的关键字，核心作用是**只返回唯一不重复的值**，常和 `SELECT` 配合使用。

```sql
SELECT DISTINCT 字段名 FROM 表名;
```



```sql
SELECT DISTINCT btype FROM books
```

![ScreenShot_2026-03-22_154050_022](D:\博客\java\辅助图片_视音频等资料\08\ScreenShot_2026-03-22_154050_022.png)

数据冗余根源是 类型其实不应该在图书表中 

应该单独建立一个图书类型表 和 图书表建立关系

影片表

id      yname           yzy......

1       大话西游-月光宝盒   周星驰,朱茵,莫文蔚,蓝洁瑛

2       大话西游-大圣娶亲       周星驰,朱茵,莫文蔚,蓝洁瑛

## 前条件

SELECT *  FROM 表名   *     所有列

SELECT 列名,列名 FROM 表名  两列

SELECT 聚合函数 FROM 表名

建表 考虑性能问题   列数   数据类型大小

## 分组

1.分组 通过某个特定条件  类型   把一个大的数据集合 分成若干个小的数据集合

2.目的 为了统计个数 为了方便管理

正式分组要求前条件必须带聚合函数  参与分组的条件 必须和聚合函数一起成为前条件 

使用count(id) 命中了id列带的索引  索引增加查询的效率 降低修改和删除的效率

SELECT btype,COUNT(id)  FROM books GROUP BY btype   无效数据

前条件>后条件>分组>排序>LIMIT  书写顺序  执行顺序

查询计算机类的图书有多少本

有两种写法

```sql
SELECT btype,COUNT(id)  FROM books WHERE btype = '计算机类' GROUP BY btype
```

想分组完成在这个分组的结果中在继续找 HAVING  不能用where

```sql
SELECT btype,COUNT(id)  FROM books GROUP BY btype  HAVING btype = '计算机类'
```



查询价格最贵的图书是谁？

SELECT MAX(bprice) FROM books



## 嵌套查询

当遇见难题的时候 一个sql怎么的都解决不了问题了 

用嵌套查询

```sql
SELECT * FROM books WHERE bprice = (SELECT MAX(bprice) FROM books)
```



## 关系

一对一  不学

一对多  学习

多对多

A表和B表建立关系有三种 以上三种

A表中数据一行对应B表中的一行

A表中数据一行对应B表中的多行

A表中数据多行对应B表中的多行



### 1.如何建立一对多关系

​    想建立关系选先挑选和是的实体模型 表

​    例如 班级和学生 一对多  或  老师和学生 多对多

​    确定谁是一 谁是多

​    班级一方

​    学生多方

​    

​    想建立一对多关系只需要在多方表中额外创建一列用来存储一方的主键

​    建立一对多关系只需要在多方表中建立外键

​    class

​    id    cname

​    1     一年一班

​    2     一年二班

​    3     一年三班

​    

​    student

​    id    sname   class_id (建立关系额外创建的列 == 外键)

​    1     小红  1

​    2     小强  1

​    3     小刚  2

​    4     小明  2

​    

```sql
CREATE TABLE class (id INT(2) PRIMARY KEY AUTO_INCREMENT,cname VARCHAR(10));

CREATE TABLE student (id INT(2) PRIMARY KEY AUTO_INCREMENT,sname VARCHAR(10),c_id INT(2));
```

### 2.如何使用关系  基于关系列进行联合查询

​    关系列  class.id == student.class_id   进行联合查询

​    sql允许多表查询   

​    查询出的数据叫笛卡尔积

​            class中有M行数据 student中有N行数据  笛卡尔积 M*N

​    SELECT *  FROM class,student 

####     基于关系

​    SELECT *  FROM class,student WHERE class.id = student.c_id

​    

​    SELECT c.cname,s.sname  FROM class c,student s WHERE c.id = s.c_id

​    

​    数据库有5大约束

​    PRIMARY KEY 主键

​    NOT NULL 非空

​    UNIQUE 唯一

​    DEFAULT 默认值

​    FOREIGN KEY 外键约束

​    

#### 添加外键约束

建表时添加

```
 CONSTRAINT 外键名 FOREIGN KEY (本表字段) REFERENCES 另一张表(另一张表的主键)
```

  表已存在时添加

```
ALTER TABLE 子表名
ADD CONSTRAINT 外键名 FOREIGN KEY (本表字段) REFERENCES 主表名(主表主键);
```

  1.通过给多方设置外键c_id 建立一对多关系 但是没有给外键c_id添加约束  == 逻辑关联

​    2.通过给多方设置外键c_id 建立一对多关系 给外键c_id添加约束  == 物理关联

​    设置外键约束

​    1.通过图形化界面

​    2.写语句

​    ALTER TABLE `student`  

​    ADD CONSTRAINT `t1` FOREIGN KEY (`c_id`) REFERENCES `class`(`id`);

​    

​    外键约束只能操作多方表

3.如何解除关系