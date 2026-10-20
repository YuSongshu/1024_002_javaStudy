# jdbc的封装

## 安装依赖

右键文件夹 =>Open Module Settings		或		左键文件夹 =>F4

左键Dependencies(依赖项)

左键+号添加依赖包mysql-connector-j-9.5.0





## 封装

数据库工具类 封装方法 进行sql发送、执行



### 三大对象

```
private static Connection connection = null;
private static Statement statement = null;
private static ResultSet resultSet = null;
```



### 四个连接参数

```
private static String url;
private static String username;
private static String password;
private static String driver;
```



### 静态代码块连接数据库

```
static {
    //1.读取 properties文件  现在 stream 之中
    InputStream stream  = DbUtils.class.getClassLoader().getResourceAsStream("db.properties");
    //2.创建Properties对象
    Properties properties = new Properties();
    try {
        //3.加载配置文件
        properties.load(stream);
        //4.获取数据
        driver = properties.getProperty("db.driver");
        url = properties.getProperty("db.url");
        username = properties.getProperty("db.username");
        password = properties.getProperty("db.password");
        //5.加载驱动
        Class.forName(driver);
        //6.获取连接
        connection = DriverManager.getConnection(url,username,password);
        System.out.println("ok了");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

#### 配置文件

以 `.properties` 为后缀的文本文件，采用 `key=value` 键值对格式存储数据

##### 创建

右键src，new => Resource Bundle

```
#key=value?
db.url=jdbc:mysql://localhost:3306/实战02?serverTimezone=GMT
db.username=root
db.password=20061126hys 
db.driver=com.mysql.cj.jdbc.Driver
```

注

等号两边无空格

没有多余分号 

若不识别中文可改为URL 编码



### 查询

#### 普通查询

传入查询sql

```
public void query(String sql){
    try {
        statement = connection.createStatement();
        resultSet = statement.executeQuery(sql);
        }
    }catch (Exception e){
        e.printStackTrace();
    }
```



但java程序中 查询 要封装查询结果，因为简单易用 提高程序数据性能

于是用ArrayList<HashMap<String,Object>>

```
public ArrayList<HashMap<String,Object>> query(String sql){

    //java程序中 查询 ArrayList<HashMap<String,Object>>、
    ArrayList<HashMap<String,Object>> list = new ArrayList<>();

    try {
        statement = connection.createStatement();
        resultSet = statement.executeQuery(sql);

        //获取全面的信息,几列，每一列列名
        ResultSetMetaData metaData = resultSet.getMetaData();

        while (resultSet.next()){
            //第一条数据 创建一个HashMap做缓存
            HashMap<String,Object> map = new HashMap<>();

            //有几列
            for (int i = 1; i <= metaData.getColumnCount();i++){
                //列名 map key值
                String key = metaData.getColumnName(i);
                Object value = resultSet.getObject(metaData.getColumnName(i));
                //存储到 map
                map.put(key,value);
            }
            list.add(map);
        }
    }catch (Exception e){
        e.printStackTrace();
    }
    return list;
}
```



注

HashMap不允许重名，故其封装方法一般用于有主键的表



#### 预处理查询



```
public ArrayList<HashMap<String,Object>>  preparedQuery(String sql,Object... param){
    ArrayList<HashMap<String,Object>> list = new ArrayList<>();

    //声明
    PreparedStatement preparedStatement = null;
    try {

        //1.赋值
        preparedStatement = connection.prepareStatement(sql);

        //2.?赋值
        if (param != null && param.length > 0){
            for (int i = 0; i < param.length;i++){
                preparedStatement.setObject(i + 1,param[i]);
            }
        }

        resultSet = preparedStatement.executeQuery();

        ResultSetMetaData metaData = resultSet.getMetaData();

        while (resultSet.next()){  //每一条数据
            HashMap<String,Object> map = new HashMap<>();

            for (int i = 1; i < metaData.getColumnCount();i++){ //有几列数据
                String key = metaData.getColumnName(i);
                Object value = resultSet.getObject(key);
                map.put(key,value);
            }
            list.add(map);
        }

    }catch (Exception e){
        e.printStackTrace();
    }finally {
        try {
            //释放预处理语句 对象 资源
            if (preparedStatement != null){
                preparedStatement.close();
            }
        }catch (SQLException e){
            e.printStackTrace();
        }
    }
    return  list;
}
```



自定义参数param默认为数组

