# 常用 HTML 标签

html:标记语言

## 初识

```
<!DOCTYPE html><!--文档类型 html-->
<html lang="en"><!--html网页 开始标签-->
<head><!--语义标签-->
  <meta charset="UTF-8"><!--亚洲编码格式，中文不会乱码-->
  <title>第一个网页</title><!--标题 网页 -->
  <!--link，引入资源，修改网页logo  href:图片 地址 在哪-->
  <link rel="icon" href="./png/19.png">
</head>
<body><!--浏览器可视区域-->
三大类： <br>
  块级标签：高度由内部撑开，宽度100%，自动换行
  <hr>
  行级标签：高度由内部撑开，宽度由内容敞开，不会自动换行
  <br>
  <hr>
  行级块标签：不会自动换行，高度可以设置，宽度可以设置
</body>
</html>
```

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <!--语义标签（元数据类标签，全部属于head内部语义元素）-->
    <!--1. 字符编码：必须写在最顶部，防止中文乱码-->
    <meta charset="UTF-8">

    <!--2. 移动端适配视口，手机页面必备语义配置-->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

    <!--3. 页面标题：强制必填，浏览器标签展示、SEO核心语义-->
    <title>HTML head语义标签学习页面</title>

    <!--4. 网页描述：搜索引擎抓取展示简介，SEO语义-->
    <meta name="description" content="讲解HTML head内部所有语义元标签，前端入门基础">

    <!--5. 网页关键词：用于检索匹配，SEO语义-->
    <meta name="keywords" content="HTML,head,语义标签,meta,link,前端">

    <!--6. 网页作者信息元标签-->
    <meta name="author" content="前端学习者">

    <!--7. 基准路径base：统一页面内所有相对路径前缀-->
    <base href="https://example.com/">

    <!--8. link：关联外部资源，语义为“文档与外部文件关系”-->
    <!--引入外部CSS样式表-->
    <link rel="stylesheet" href="style.css">
    <!--网页小图标favicon-->
    <link rel="icon" href="favicon.ico" type="image/x-icon">

    <!--9. style：内嵌CSS样式，定义当前页面样式语义-->
    <style>
        body { background: #f5f5f5; }
    </style>

    <!--10. script：脚本标签，引入/编写JS逻辑-->
    <script src="index.js"></script>

    <!--11. noscript：浏览器禁用JS时展示提示，兼容语义标签-->
    <noscript>您的浏览器已关闭JavaScript，页面功能无法正常使用</noscript>
</head>
<body>
    <!--页面可见内容放这里，布局语义标签header/main/section/footer等写在body-->
</body>
</html>
```









## 1. 标题

```
<h1>大标题</h1>
<h2>副标题</h2>
<h3>小标题</h3>
```

## 2. 段落标签

```
<p>这是一段普通文字</p>
```

## 3. 链接（可点击）

```
<a href="http://127.0.0.1:8000/api/user">点我跳转到用户接口</a>
```

注：

```
<p><a href="http://127.0.0.1:8000/api/user">点我跳转到用户接口</a></p>
<a href="http://127.0.0.1:8000/api/user">点我跳转到用户接口</a>
```

上者两种写法都能正常运行

区别

< p >作用：自动换行 + 上下留一点空隙

若不用< p >两个链接会挨在一起，不换行



## 4. 换行、分割线

```
<br>   	 	#<!-- 换行 -->
<hr>    	#<!-- 横线 -->
```

注：

两个< p >的行间距要大于< br > 的行间距



## 5. 输入框（表单必备）

```
<input type="text" name="username" placeholder="请输入姓名">
<input type="password" name="pwd" placeholder="请输入密码">
```

## 6. 按钮

```
<button>点我</button>
```

### 核心属性

#### type

（默认为submit提交表单）

- `button`：普通按钮，无默认行为，需 JS 绑定事件
- `submit`：提交表单数据（默认值）
- `reset`：重置表单为初始值



#### disabled：禁用按钮，不可点击

#### name/value：表单提交时的键值对

#### onclick：点击事件（内联写法，推荐用 addEventListener）

```
普通按钮
<button type="button" onclick="alert('点击成功')">普通按钮</button>

<!-- 提交按钮（需在 <form> 内） -->
<form action="/submit">
  <input type="text" name="username">
  <button type="submit">提交表单</button>
  <button type="reset">重置表单</button>
</form>

<!-- 禁用按钮 -->
<button type="button" disabled>禁用按钮</button>
```



## 7. 表单（前后端交互核心）

```
<form action="/submit" method="post">
    <input name="username">
    <button type="submit">提交</button>
</form>
```



## 8.link（小图标）

```
<link rel="icon" href="./png/19.png">
```



## 9. div（盒子，用来布局）

```
<div>
    这里放内容、图片、表单都可以
</div>
```



## 10.列表

  ul（无序列表） ol（有序）  li标签使用

**`<ul>` unordered list 无序列表**

内容**无先后顺序**，默认前面是实心圆点，多用于导航、菜单、清单、分类列表。

**`<ol>` ordered list 有序列表**

内容**有先后步骤 / 排序**，默认阿拉伯数字 1、2、3，用于步骤、排名、操作流程。

**`<li>` list item 列表项**

唯一列表内容容器，**ul/ol 内部只能直接放 li**，文字、图片、div、p 都要写在 li 里面。

```
<ol type="a">
  <li>列表1</li>
  <li>列表2</li>
  <li>列表3</li>
  <li>列表4</li>
  <li>列表5</li>
</ol>

<ul>
  <li>列表1</li>
  <li>列表2</li>
  <li>列表3</li>
  <li>列表4</li>
  <li>列表5</li>
</ul>
```



## 11.表格

CSS设置 样式 宽度 等 内容

```
<table style="width: 1000px;border: 2px solid blue">
  <tr style="background-color: yellow"><!--row 行-->
    <td>周一</td>
    <td>周二</td>
    <td>周三</td>
    <td>周四</td>
    <td>周五</td>
  </tr>
  <tr><!--row 行-->
    <td>周一</td>
    <td>周二</td>
    <td>周三</td>
    <td>周四</td>
    <td>周五</td>
  </tr>
</table>
```





