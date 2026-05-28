# JDBC

## 初识

JDBC（Java Database Connectivity）是 Java 操作关系型数据库的标准 API，位于 `java.sql` 与 `javax.sql` 包，通过厂商驱动实现跨数据库统一访问，核心价值是 “一次编写、到处运行”

两块代码  1.jdbc   2.多线程  Thread

java.sql.三个接口  （Connection conn    Statement  stmt    ResultSet  rs） 

| 组件 / 接口           | 核心作用             | 常用方法 / 场景                                              |
| --------------------- | -------------------- | ------------------------------------------------------------ |
| **DriverManager**     | 驱动管理与连接获取   | `getConnection(url, user, password)`：建立数据库连接         |
| **Connection**        | 数据库物理连接通道   | `createStatement()`/`prepareStatement()`：创建执行对象；`commit()`/`rollback()`：事务控制 |
| **Statement**         | 执行静态 SQL         | `executeQuery()`（查询，返回 ResultSet）；`executeUpdate()`（增删改，返回影响行数） |
| **PreparedStatement** | 预编译 SQL（防注入） | 支持参数占位符 `?`，性能与安全性优于 Statement               |
| **ResultSet**         | 存储查询结果集       | `next()`：遍历结果；`getXxx(columnIndex/columnLabel)`：获取字段值 |
| **SQLException**      | 数据库操作异常       | 捕获连接失败、SQL 语法错误等，含错误码与状态信息             |





## 详细的使用和开发步骤

1.得到数据库的驱动类   mysql官方提供     ->C C++  java  python (mysql 手机  -> 人 java      使用说明书 驱动)

​	mysql-connector-java-5.0.3-bin.jar

​	mysql-connector-java-8.0.22.jar   mysql8  自己装的mysql



2.java工程 需要得到驱动类 并且把驱动类变成工程能使用的一部分（外部的jar 第三方类库 如何构建到工程中）

​	eclipse 或 idea  两个工具对于jar构建 方式不一样的

​	对于eclipse  只需要把驱动包复制到工程的根路径中

​	点击根路径中的驱动包 鼠标右键 -> build path -> add  to build path 构建成功



3.知道驱动类的地址 因为java需要加载驱动类才能建立和mysql的连接

​	驱动类一定在驱动包中

​	mysql5  com.mysql.jdbc.Driver

​    mysql8  com.mysql.cj.jdbc.Driver



4.声明出三个核心的jdbc对象

​	连接   语句   结果集

```java
	
		//声明  三个对象
		Connection conn;
		Statement stmt;
		ResultSet rs;
```

注：

从 JDBC 4.0 开始，**不需要手动写 `Class.forName(驱动类名)` 了**，只要导入了 MySQL 驱动包，会自动注册驱动。



​		参数   连接数据库的 IP地址   协议    端口    仓库名   用户名   密码  

​		IP地址   协议    端口    仓库名 合并成了一个参数  URL  地址

​		url    user   password  driver 驱动类的地址

```java
	
		//创建连接参数
		String url = "jdbc:mysql://localhost:3308/ccht20";
		String user = "root";
		String password = "mysql";
		String driver = "com.mysql.jdbc.Driver";
```



注：

private static final

```
private static final String URL = "jdbc:mysql://localhost:3306/寒假作业001（人员信息管理系统）?serverTimezone=GMT";
```

private（访问控制）

static（静态 / 类级别）

final（不可变）：与c的const类似

组合效果

​	访问：仅本类可见，外部无法直接调用（安全）

​	存储：类加载时初始化、全类共享一份（省内存）

​	赋值：一次赋值终身只读，防止意外篡改（稳定）





6.写出捕获异常的代码 try和catch 在try中尝试运行  写出加载驱动类和建立连接的代码

​	建立连接就是给 conn 赋值

```java
	try {
			Class.forName(driver);//加载驱动类
			conn = DriverManager.getConnection(url, user, password);//建立连接   到这数据库就连上了
			System.out.println("连接成功!");
		} catch (Exception e) {
			e.printStackTrace();
		}
```

7.本质我们要永久的保存数据  需要找到表   对表进行数据的  增删改查 == 交互

​	创建出语句对象  发送sql语句

```java
//创建语句对象
stmt = conn.createStatement();
```

## 交互

### executeUpdate

​				增删改   java  发送sql 给mysql       送  insert   update  delete  不会有结果 但是会有影响的行数

​				行数应该是1  只要不是0  就表示成功了

​				int rows = stmt.executeUpdate(sql);

```java
			//交互  演示  insert  update  delete
			//准备sql  
			//String  sql = "insert into dog(name,age) values ('大黄',3)";
			//String sql = "update dog set name = '小白' where name = '大黄'";
			String sql = "delete from dog where id = 6";
			//发送到mysql中 得到影响的行数
			int rows = stmt.executeUpdate(sql);
			System.out.println(rows);
```

​				



#### 注入

统一格式		' "++" '				如果是like		'%"++"%'

```
String sql = "SELECT * FROM user WHERE name = '" + keyword + "'";
String sql = "SELECT * FROM user WHERE name LIKE '%" + keyword + "%'";
```

##### 注

高危！

如果用户输入`' OR '1'='1`，拼接后 SQL 会变成：

```
SELECT * FROM user WHERE name LIKE '%' OR '1'='1%'
```

直接绕过权限，泄露全表数据，属于典型的 SQL 注入漏洞。

**本质问题**：将用户输入直接作为 SQL 语法的一部分，而非纯数据参数。



若为数字则无需单引号，避免字符串比较





### executeQuery

​	查询      java  发送sql 给mysql       送  select  mysql会给我结果   

​				rs = stmt.executeQuery(sql);

```java
//交互  演示 select 语法最多
			String sql = "select *  from dog";
			//千万去海豚中看一眼语句查询的结果长什么样
			rs = stmt.executeQuery(sql);
			//rs怎么用啊？   mysql已经把结果放到rs中了  
			//rs游标  == 指针  
//		        id  name       age       <-rs默认指向结果列名的这行的一个指针
//		    ------  ------  --------
//		         4  旺财             3			
//		         5  来福             3			<-rs.next().next()
//		         6  小白             3			<-rs.next().next().next()
			
			//滚动rs指针  一次能滚动一行
			/*rs.next();
			rs.next();
			rs.next();
			rs.next();*/
			//rs.next();//返回值是布尔  有下一行是返回 true 没有下一行返回false
			//取值 rs.get数据类型(取值的列名或列的索引  索引从1开始)
			/*System.out.println(rs.getInt(1));
			System.out.println(rs.getString(2));
			System.out.println(rs.getInt("age"));*/
			
			//如何取出所有的数据
			while(rs.next()) {
				System.out.print(rs.getInt(1)+" ");
				System.out.print(rs.getString(2));
				System.out.print(rs.getInt("age"));
			}
```



### execute

万能执行（所有 SQL）	

rs = stmt.executeQuery(sql);

返回 `true` → 是查询：用 `stmt.getResultSet()` 取结果集

返回 `false` → 是更新 / DDL：用 `stmt.getUpdateCount()` 取受影响行数

循环取多结果：`getMoreResults()` 切换下一个结果，直到 `getUpdateCount() == -1` 结束

```
// Oracle 存储过程（返回多个游标）
CREATE OR REPLACE PROCEDURE get_multi_data(
    p_deptno IN NUMBER,
    p_emp_cursor OUT SYS_REFCURSOR,
    p_dept_cursor OUT SYS_REFCURSOR
) AS
BEGIN
    OPEN p_emp_cursor FOR SELECT * FROM emp WHERE deptno = p_deptno;
    OPEN p_dept_cursor FOR SELECT * FROM dept WHERE deptno = p_deptno;
END;
/

// JDBC 调用（CallableStatement + execute）
String callSql = "{call get_multi_data(?, ?, ?)}";
try (Connection conn = DriverManager.getConnection(
        "jdbc:oracle:thin:@localhost:1521:ORCL", "user", "pwd");
     CallableStatement cstmt = conn.prepareCall(callSql)) {

    cstmt.setInt(1, 20); // 输入参数
    cstmt.registerOutParameter(2, oracle.jdbc.OracleTypes.CURSOR);
    cstmt.registerOutParameter(3, oracle.jdbc.OracleTypes.CURSOR);

    // 执行存储过程
    boolean hasResult = cstmt.execute();

    // 循环处理所有结果集（Oracle 多游标）
    while (true) {
        if (hasResult) {
            ResultSet rs = cstmt.getResultSet();
            while (rs.next()) {
                // 处理数据
            }
            rs.close();
        } else {
            int count = cstmt.getUpdateCount();
            if (count == -1) break; // 无更多结果，退出
        }
        hasResult = cstmt.getMoreResults(); // 切换下一个结果
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```



### 总结

| 方法          | 用途                 | 返回值                      |
| ------------- | -------------------- | --------------------------- |
| executeQuery  | 执行查询（SELECT）   | `ResultSet` 结果集          |
| executeUpdate | 执行增删改 / DDL     | int（受影响行数或 0）Oracle |
| execute       | 任意 SQL（复杂场景） | boolean：是否有结果集       |



## 关闭

先关rs再关stmt再关conn（先开conn再开stmt再开rs）

```
rs.close();
stmt.close();
conn.close();
```





### 拓展

调用封装好的工具方法

比手动写 `rs.close(); stmt.close(); conn.close();` 更简洁、更安全、更规范，效果完全一样

```
 public static void closeResources(Connection conn, Statement stmt, ResultSet rs) {		
        try {
            if (rs != null) rs.close();
            if (stmt != null) stmt.close();
            if (conn != null) conn.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

用时

```
closeResources(conn, stmt, rs);
```

注：

参数可以随意但关闭顺序（函数体）永远是rs => pstmt  =>  conn
