# SQL注入安全

## 核心原理与成因

原理:先确定sql语句，再执行，所以注入的变量可以改变原本sql的语法



代码直接用字符串拼接构造 SQL（如`"SELECT * FROM users WHERE username='"+input+"'"`）

无有效防护：未做参数化、转义或过滤，特殊字符（`'`、`"`、`;`、`--`、`UNION`等）被数据库解析为 SQL 语法



示例

```
System.out.println("请您输入用户名");
String username = scan.nextLine();
System.out.println("请您输入密码");
String psd = scan.next();			
String sql = "select *  from userinfo where username = '"+username+"' and password = '"+psd+"'";
rs = stmt.executeQuery(sql);
			
if(rs.next()) {
	System.out.println("登录成功!");
}else {
	System.out.println("登录失败!");
}
```

如果输入`' or 1 = 1 --`

 最终拼接成的 SQL 语句会变成:

```
select * from userinfo where username = '' or 1 = 1 -- ' and password = ''
```

1. `'` 闭合了前面的单引号，让 SQL 语法 “裂开”
2. `or 1=1` 永远为真
3. `--` 注释掉后面所有内容（包括密码判断）

**结果：条件永远成立 → 直接登录成功！**





## 核心防御：参数化查询（预编译）

原理：先编译和确定sql语句的语法再执行

```
String sql = "SELECT * FROM users WHERE username=? AND password=?";
//创建预处理
PreparedStatement pstmt = conn.prepareStatement(sql);
//给?号赋值
pstmt.setString(1, username); 
pstmt.setString(2, password);
ResultSet rs = pstmt.executeQuery();
```

? :参数占位符

pstmt.set数据类型（占位符位置,变量名）;

| 变量类型 | 方法           | 例子         |
| -------- | -------------- | ------------ |
| 字符串   | `setString()`  | 用户名、密码 |
| 整数     | `setInt()`     | 年龄、ID     |
| 小数     | `setDouble()`  | 金额         |
| 布尔     | `setBoolean()` | 状态         |
| 日期     | `setDate()`    | 注册时间     |





## 预处理应用

### dao封装

连接数据库  不用反复连接

```
static {
	try {
		Class.forName(driver);
			conn = DriverManager.getConnection(url, user, password);
			System.out.println("连接成功!");
	}catch(Exception e) {
		e.printStackTrace();
	}
}
```



### 预处理的更新

与普通更新不同，预处理的更新  sql 带?号，要给?赋值的参数，但?数不确定故预处理的更新很麻烦

于是我们可以对它封装

示例

```
public int preUpate(String sql,Object... obj) {
		//obj 用它来给sql中的?号赋值 通过obj的长度来判断sql中的?有几个
	int rows = 0;
	try {
		pstmt = conn.prepareStatement(sql);
		//通过obj 遍历来确定?号的个数和给?号赋值
		for(int i = 0; i<obj.length; i++) {
			pstmt.setObject(i+1, obj[i]);
		}
		rows = pstmt.executeUpdate();
	}catch(Exception e) {
		e.printStackTrace();
	}
	return rows;
}

```

```
String sql = "insert into sp (sname,sprice,stype) values (?,?,?)";

System.out.println("请您输入商品名称");
String sname = scan.next();
System.out.println("请您输入商品价格");
double sprice = scan.nextDouble();
System.out.println("请您输入商品类型");
String stype = scan.next();

int rows = db.preUpate(sql, sname,sprice,stype);
System.out.println(rows);
```



注：

Object 根类 所有的类的父类  表示所有的数据类型





### 预处理的查询

同理

```
public ResultSet preQuery(String sql,Object... obj) {
	try {
		pstmt = conn.prepareStatement(sql);
		for(int i = 0; i<obj.length; i++) {
			pstmt.setObject(i+1, obj[i]);
		}
		rs = pstmt.executeQuery();
	}catch(Exception e) {
		e.printStackTrace();
	}
	return rs;
 }
```

只是要返回rs





### 关闭

```
public void close() {
	if(rs!=null) {
		try {
			rs.close();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	if(stmt!=null) {
		try {
			stmt.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	if(pstmt!=null) {
		try {
			pstmt.close();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	if(conn!=null) {
		try {
			conn.close();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

可以直接用



## 动态参数

语法：

方法名(参数类型... 变量名)

示例

```
public static void showName(String... names) {
	//name就是数组了
	for(String name:names) 
		System.out.println(name);	
}
public static void main(String[] args) {
	showName("张三","李四","王五");
}
```

动态参数本质就是数组



注：

对于for，java有自己的特别语法

```
for(String name:names) 
System.out.println(name);

for(String name:names) 
	System.out.println(name);
	
for(String name:names) {
	System.out.println(name);
}
	
//都对，但不加{}只会识别一行在for循环中

```

：相当于python的in

与普通写法的对比

```
for(String name:names) {
	System.out.println(name);
}
```

```
for (int i = 0; i < names.length; i++) {
    String name = names[i];
    System.out.println(name);
}
```

