# 17_idea开发工具

idea开发工具

<img src="D:\博客\java\web开发\辅助图片_视音频等资料\17\01.png" alt="01" style="zoom: 50%;" />





破解工具：IDEAActivationFile

打开idea至登录页面

单击 Win.bat 文件

等待跳 Success ，之后打开`IDEAActivationFile\Activation_Code\idea.txt` 复制其内容

粘贴到



## 初识idea

### 创建第一个Project

<img src="D:\博客\java\web开发\辅助图片_视音频等资料\17\02.png" alt="02" style="zoom: 40%;" />



Name：Project名称

Location: Project 存放地址

| 选项                               |                          | 适合场景                       |
| ---------------------------------- | ------------------------ | ------------------------------ |
| Add sample code                    | 提供可运行的基础代码模板 | 快速搭建项目骨架，直接跑通流程 |
| Generate code with onboarding tips | 提供带注释 / 说明的代码  | 学习代码逻辑，理解每一步的作用 |

老手不勾选



### 创建第一个 Module

右键创好的Project文件，new一个 Module

![03](D:\博客\java\web开发\辅助图片_视音频等资料\17\03.png)



### 创建第一个 Package

右键 src 文件夹 new 一个 Package

<img src="D:\博客\java\web开发\辅助图片_视音频等资料\17\04.png" alt="04" style="zoom:65%;" />



### 创建第一个 class

同理



#### 快捷创建方式

创建 Package 时

命名为xxx.xxx.xxx

会识别最后一个为class名

示例

```
com.la.demo.array.ArrayListDemo
```

<img src="D:\博客\java\web开发\辅助图片_视音频等资料\17\05.png" alt="05" style="zoom:250%;" />





## 注

### 包名有说法

#### 反域名

把域名**倒过来**当包名前缀，保证全球唯一、避免类名冲突Oracle。

域名：`www.baidu.com`

反域名：`com.baidu.www`（实际用`com.baidu`即可）

#### 标准包名结构

**com. 公司名.项目名.模块名**

以百度（域名为`baidu.com`）举例：

- 公司名：`baidu`

- 项目名：`search`（搜索项目）

- 模块名：

  ```
  user
  ```

  （用户模块）

  

   完整包名：

  ```
  com.baidu.search.user
  ```

#### 命名规则

1. **全小写**，无大写、无中文。
2. 只能用：**小写字母、数字、下划线**（尽量别用下划线）。
3. 用`.`分层，**不能以`.`开头 / 结尾**。
4. 不能是 Java 关键字（如`int`、`class`）。

