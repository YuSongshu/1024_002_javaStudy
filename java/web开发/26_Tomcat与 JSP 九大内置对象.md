# Tomcat与 JSP 九大内置对象

Tomcat + JSP 九大内置对象

服务器的作用：

- Tomcat 是一个「Java Web 应用服务器软件」。（框架）
- 「运行容器 + 请求管家」—— 你的代码最终就是跑在 Tomcat 里面的。
  - 登录需求 --> 网页上输入账号 密码 登录按钮 -- 发起一个请求 ---> 项目中 专门写 登录方法的java类 完成操作 

1. **接请求**：监听端口（默认 8080），把浏览器发来的 HTTP 网络请求，解析成 Java 代码能读懂的请求对象。
2. **跑代码**：加载并运行你写的 JavaWeb 代码，把请求交给对应的 Java 方法去处理。
3. **返结果**：把 Java 代码处理完的结果，再包装成 HTTP 响应，发回给用户浏览器。
4. **管并发**：同时来几十个、上百个用户请求，Tomcat 会自动分配线程、调度排队，保证程序不崩溃。



tomcat输出打印控制台乱码？

点击Edit Configurations... 编辑配置

VM处添加: `-Dfile.encoding=UTF-8`  

(VM options 是给 JVM（Java 虚拟机） 的启动参数，用来控制内存、GC、系统属性等，不是程序 main 的入参。)







每次学的东西，为了业务痛点

| 内置对象    | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| request     | 请求对象，代表客户端 HTTP 请求，包含了客户端浏览器的请求信息。请求范围有效 |
| responses   | 响应对象，代表服务端 HTTP 响应，用于向客户发送响应信息。响应范围有效 |
| out         | 输出对象，用于向客户端法发送输出内容，可以通过 out 对象向浏览器输出 HTML 代码 |
| page        | 页面对象，代表了当前 JSP 本身，可以通过 page 对象来访问当前 JSP 页面的脚本和定义方法 |
| pageContext | 页面上下文对象，在 JSP 页面中主要作用是管理页面属性，实现页面内不同部分的数据共享 |
| config      | 配置对象，当前 JSp 页面的配置信息，可以获取当前页面的配置参数 |
| exception   | 异常对象，对象主要用于处理页面中的异常情况。声明当前页面是一个处理异常的页面。 |
| session     | 会话对象，代表服务器端和客户端创建的会话，用于多个请求之间共享数据 |
| application | 应用对象，代表整个 web 程序，用于在整个应用范围内共享数据    |



记得：

- request：请求对象
  - 设置接受参数的编码格式
  - 接收传递参数
  - 传递数据：使用java代码传递，任何类型的数据
  - 跳转页面：一起使用的，方式 post
- responses：响应对象
  - 设置响应参数编码格式 （现在用不到）
  - 传递数据：简单的 数值  字符串 
  - 跳转页面，跳转方式 get
- session：会话对象
  - 共享数据，默认时间30分钟，当前浏览器有效 
  - 存入，共享





## request

tomcat 引包

单击项目 + F4  => Dependencies,添加tomcat



### 登录需求

form表单，HTML标签

#### 提交数据

form表单：提交数据，包裹输入框，提交属性按钮

可以通过它将数据传到别的jsp文件里

```
<form action="login_service.jsp" >
  <p>
    <input type="text" placeholder="账号" name="acc">
  </p>
  <p>
    <input type="password" placeholder="密码" name="pass">
  </p>
  <p>
    <button>登录</button>
  </p>
</form>
```

注：

每一个输入框中必须有一个name属性，提交数据，接收数据，10个数据，10个name



action: 提交数据地址 与 href一样，不过href不能拼，只能写死

```
<a href="login_service.jsp?acc=admin&pass=123456">去传递数据</a>
```

method：跳转方式，有 get post

​		区别：get 会把数据拼在url上         post 数据不会显示在外面 



传递数据的方式

```
<form action="for_value.jsp">
    <p>
        <input type="text" placeholder="账号" name="acc" >
    </p>
    <p>
        <%--单选--%>
        <input type="radio" name="sex" checked value="男">男
        <input type="radio" name="sex" value="女">女
    </p>
    
    <p>
    	<%--多选--%>
        <input type="checkbox" value="周一" name="week">周一
        <input type="checkbox" value="周二" name="week">周二
        <input type="checkbox" value="周三" name="week">周三
        <input type="checkbox" value="周四" name="week">周四
        <input type="checkbox" value="周五" name="week">周五
    </p>
    <p>
        <%--下拉--%>
        <select name="add">
            <option>长春</option>
            <option>吉林</option>
            <option>北京</option>
        </select>
    </p>

    <p>
        <textarea name="info"></textarea>
    </p>
    <p>
        <button>提交</button>
        <input type="submit" value="登录">
    </p>
</form>
```









#### 接收数据

`request.getParameter();`: 【接收表单传递数据】

```
String acc = request.getParameter("acc");
String pass = request.getParameter("pass");
```

模拟校验

```
String acc = request.getParameter("acc");
String pass = request.getParameter("pass");
String acc1 = "admin";
String pass1 = "123456";
if (acc.equals(acc1) && pass.equals(pass1)){

request.setAttribute("acc",map);  // --------  request传递数据
request.getRequestDispatcher("success.jsp").forward(request,response);
}else {
//登录失败 
//重定向  get方式
response.sendRedirect("index.jsp?acc="+acc+"&pass="+pass);

}
```



### type

![input的type取值分类](.\辅助图片_视音频等资料\26\input的type取值分类.png)





## response

response不能用复杂数据（map...）

只能拼简单的

```
String acc = "admin";
String pass = "123456";
response.sendRedirect("index.jsp?acc="+acc+"&pass="+pass);
```

## session

session是干什么的？

session 是服务器帮"某个特定浏览器"保存的一份数据，跟请求多少次无关

HTTP 有个天生的缺陷：**它没有记忆**。每个请求都是独立的，服务器处理完就忘。

这带来一个麻烦：用户登录成功了，下一个页面服务器怎么知道"你就是刚才登录的那个人"？

工作流程

```
① 浏览器第一次访问
   服务器一看：这人有 session 吗？没有 → 创建一个，编号 JSESSIONID=ABC123
   ↓
② 服务器在响应里告诉浏览器：给你个编号 ABC123，收好
   浏览器存下这个 Cookie
   ↓
③ 浏览器第 2 次、第 3 次访问
   自动带上 Cookie: JSESSIONID=ABC123
   ↓
④ 服务器一看编号 ABC123 → 哦是老熟人，把他那份数据拿出来

```

![HttpSession常用API全景分组](./辅助图片_视音频等资料\26\HttpSession常用API全景分组.png)







## Servlet

Servlet 就是 JavaWeb 中用来接收、处理 HTTP 请求的 Java 类

