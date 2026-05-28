# java最重要的概念 （框架）

分层

已有的代码进行功能划分  分成不同的层次   层次之间可以重复应用

1.连接数据库  和数据库交互       						持久层   dao

2.本身 程序的业务逻辑和算法  （提取出来）   业务层   serivce

3.前端  菜单  （提取出来） 								视图层  View  springMvc



View<-> serivce<->dao





## 持久层

```
public class DbUtils {
	private  static Connection conn = null;
	private  static Statement stmt = null;
	private  static ResultSet rs = null;
	
	private  static String url = "jdbc:mysql://localhost:3308/ccht20";
	private  static String user = "root";
	private  static String password = "mysql";
	private  static String driver = "com.mysql.jdbc.Driver";
	
	//如何去连接数据库  不用反复链接
	//类的初始化     构造方法    静态代码块 （最先加载  只加载一次）
	static {
		try {
			Class.forName(driver);
			conn = DriverManager.getConnection(url, user, password);
			System.out.println("连接成功!");
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	public int update(String sql) {
		
		int rows = 0;
		try {
			stmt = conn.createStatement();
			rows = stmt.executeUpdate(sql);
		}catch(Exception e) {
			e.printStackTrace();
		}
		
		
		return rows;
	}
	
	
	//有问题的  技术所限 仍然返回rs  不但没简化 更复杂了  java->mysql  没有利用内存
 	public ResultSet query(String sql) {
		try {
			stmt = conn.createStatement();
			rs = stmt.executeQuery(sql);
		}catch(Exception e) {
			e.printStackTrace();
		}
		return rs;
	}
	
	/*//查询返回的数据结构类 == 内存
	public ArrayList<HashMap<String,Object>> queryFotList(String sql){
		//咋写啊
	}*/
	
	//关闭
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
		if(conn!=null) {
			try {
				conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
	

}

```



## 业务层

```
public class Utils {
	
	//写两个方法   录入  查看  spring框架的作者 java体系中最伟大的框架   提示词
	//依赖dao层对象  注入dao
	
	private DbUtils db = new DbUtils();
	private Scanner scan = new Scanner(System.in);
	
	public void addSp() {
		
		//实现录入商品的方法
		System.out.println("请您输入商品名称");
		String sname = scan.next();
		System.out.println("请您输入商品价格");
		double sprice = scan.nextDouble();
		System.out.println("请您输入商品类型");
		String stype = scan.next();
		

		int rows = db.update(sql);
		if(rows != 0) {
			System.out.println("操作成功!");
		}else {
			System.out.println("操作失败!");
		}
	}
	
	
	public void getAllSp() 
	{
		String sql = "select *  from sp order by sprice desc";
		ResultSet rs = db.query(sql);
		System.out.println("编号=名称=价格=类型");
		try {
			while(rs.next()) {
				int id = rs.getInt(1);
				String name = rs.getString(2);
				double price = rs.getDouble("sprice");
				String type = rs.getString(4);
				
				System.out.println(id+"="+name+"="+price+"="+type);
			}
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	//关闭jdbc释放数据库资源
	public void close() {
		db.close();
	}

}

```



## 视图层 

```
//前端  菜单
public class Main {
	
	public static void main(String[] args) {
		
		
		Scanner scan = new Scanner(System.in);
		
		//对象注入过来  springIOC 依赖注入
		Utils u = new Utils();
		
		boolean bol = true;
		while(bol) {
			System.out.println("欢迎进入商品管理系统");
			System.out.println("1.录入商品信息");
			System.out.println("2.查看所有的商品信息");
			System.out.println("3.退出系统");
			System.out.println("请您输入功能序号选择功能");
			int i = scan.nextInt();
			if(i == 1) {
				u.addSp();
			}else if(i == 2) {
				u.getAllSp();
			}else if(i == 3) {
				u.close();
				bol = false;
				System.out.println("谢谢使用!");
			}else {
				System.out.println("对不起只有3个功能!");
			}
		}
		
	}

}

```

