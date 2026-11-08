# 27_Servlet

前端：展示数据

后端：处理数据 ， 业务逻辑 （如：登录的业务） 

 Java类，处理业务逻辑的地方，



## Servlet

Servlet：普通的java类但是，继承或者实现接口的方式，拥有一个可以被访问的路径 地址 

作用：控制器，负责接收请求、调用工具、跳转页面



Servlet创建方式两种：

- 实现接口，拥有访问地址 
- 继承HttpServlet抽象类，拥有访问地址 ，更加灵活



Servlet配置 (两种任选其一)：

- web.xml文件配置
- @WebServlet注解配置（@单词 ，类似属性 ，比较直接，不用复杂的操作）





```java
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end
#parse("File Header.java")
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.io.IOException;

@WebServlet(name = "${Class_Name}", value = "/${Class_Name}")
public class ${Class_Name} extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request,response);
    }
}
```



## 导入MySQL

idea2024 导入有问题，有时会导入没反应

解决方法

在 web\WEB-INF 下创建名为 lib 的文件夹并将jar包放入

