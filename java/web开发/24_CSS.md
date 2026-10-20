# CSS 简述

## 全称与定位

**CSS**：Cascading Style Sheets，层叠样式表。

作用：专门用来给 HTML 页面**美化布局、设置样式**，分离网页的「结构」与「表现」。

- HTML：搭建页面骨架（文字、图片、按钮等内容结构）
- CSS：控制外观（颜色、大小、字体、间距、排版、动画等）：没有办法全部学习完的

## 核心三大特性

1. 层叠性：多个样式作用于同一元素时，按规则叠加、冲突样式取优先级高的。
2. 继承性：父元素的部分样式（字体、颜色等）会自动传给子元素。
3. 优先级：选择器权重决定谁生效：行内样式 > id 选择器 > 类 / 属性选择器 > 标签选择器。



### 使用方式

- 每个HTML标签 内部 属性，style（CSS样式属性）：内部
- style，写在head标签中的，通过选择器 选中 
- 一样的，区别 引入的 单独css文件 封装 

```
<!--px:长度单位 vh rem等 -->
<button style="width: 200px;height: 30px">按钮</button>
<!--2.边框变为蓝色 , 文本颜色白色，背景蓝色-->
<button style="width: 200px;height: 30px;border: 1px solid dodgerblue;color: white;background-color: dodgerblue">按钮</button>
<!--3.边框 边圆-->
<button style="width: 200px;height: 30px;border: 1px dashed red;color: white;background-color: dodgerblue;border-radius: 5px">按钮</button>
```



#### 选择器

行内样式 > id 选择器 > 类 / 属性选择器>父子选择器 > 标签选择器（元素选择器）\>通配符选择器

##### 行内样式（内联样式）

直接写在 HTML 标签的 `style` 属性里，只对当前这一个标签生效。

```
<p style="color: red; font-size: 18px;">红色文字</p>
```



##### id 选择器 

一个页面中同一个 id 只能出现一次，不能重复

以 `#` 开头，后面跟 id 名称

```
<head>
	<style>
    	#btn{
      		background-color: aqua;
    	}
	</style>
</head>

<body>
<button id="btn">按钮</button>
</body>
```



#####  类 / 属性选择器

| 选择器     | 标识         | 适用场景                         |
| ---------- | ------------ | -------------------------------- |
| 类选择器   | `.className` | 自定义批量样式，项目最常用       |
| 属性选择器 | `[attr]`     | 利用原有标签属性筛选，表单、链接 |

```
<head>
	<style>
    	.btn2{
      		color: dodgerblue;
    	}
	</style>
</head>

<body>
<p class="btn2">段落</p>
</body>
```



##### 父子选择器

| 写法         | 符号 | 匹配范围                 | 语法                           |
| ------------ | ---- | ------------------------ | ------------------------------ |
| 后代选择器   | 空格 | 所有后代（子、孙、曾孙） | 父选择器 子选择器 { 样式 }     |
| 子元素选择器 | `>`  | 仅直接一级子元素         | 父选择器 > 直接子元素 { 样式 } |

```
.father p {color: red;}
```

```
.father > p {  color: red; }
```

```
<div class="father">
  <p>儿子p</p>
  <div>
    <p>孙子p</p>
  </div>
</div>
```



##### 标签选择器（元素选择器）

作用范围全局：匹配页面所有对应标签，无法单独控制某一个

```
<head>
	<style>
    	 p{
      		color: green;
      		font-size: 50px;
    	}
	</style>
</head>

<body>
<p class="btn2">段落</p>
</body>
```





##### 通配符选择器

语法

```
* {
  属性: 值;
}
```

```
*{
  background-color: palegoldenrod;
}
```







### 定位

边距：

- 外边距
- 内边距

postion定位属性 ：

- 固定的：定死 
- 绝对的：脱离文档流格式，本质 相较于最近的 postion 的标签来进行定位 
- 相对的：相对原来标签位置 移动，发生变化，其实元素还在原来的地方
- 静态的：默认的 



## 标签 

### link标签

专门用来引入外部资源，单标签，写在 `<head>` 里

```
<link rel="stylesheet" href="css/index02.css">
```



### a标签

#### 超链接

```
<a href="index.html">去index</a>
```

```
<a href="https://www.baidu.com/">去百度</a>
```

href：指定跳转地址

target 两个常用值

**target="_self"**

默认值，不写 target 时等价于这个；

效果：在当前页面 / 当前标签页覆盖原有内容跳转。



**target="_blank"**

效果：打开全新浏览器标签页，原页面保留。

```
<a href="https://www.baidu.com/" target="_self">去百度_self</a>
<a href="https://www.baidu.com/" target="_blank">去百度_blank</a>
```



#### 锚点

a 标签 href + id

```
<div id="box" style="margin-top: 1000px;">我是锚点位置</div>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<p>内容</p>
<a href="#box">跳到盒子</a>

```



### 外边距(边框)

```
.box{
	/*background-color: dodgerblue;*/			//盒子内容区设置背景颜色
	 border: 2px solid dodgerblue;				//2px 实线、道奇蓝色；边框会额外撑大盒子尺寸
	width: 500px;
	height: 300px;								//盒子内容区域宽 500px，高 300px

//margin 外边距
	/*margin: 150px auto;*/						//上下外边距 150px，左右自动居中		
	/*margin-right: 500px;*/					//盒子上方距离右方元素距离			
	margin-left: 500px;							//盒子上方距离左方元素距离
	margin-top: 50px;							//盒子上方距离上方元素距离

//padding 内边距（内容与盒子边框之间的距离）
	padding-top: 100px;							//内部文字 / 内容距离上边框 100px
	padding-left: 100px;						//内部文字 / 内容距离左边框 100px
}
```



### position标签

`position: relative`：相对定位，**相对于自身原来的标准位置**偏移，不脱离文档流，原位置保留空白。

`position: absolute`：绝对定位，**相对于最近一层已定位（relative/absolute/fixed/sticky）的父元素**偏移；若无，则相对于浏览器视口；完全脱离文档流，不占原有位置。

```
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <style>
    div{
      position: absolute;
      top: 10px;
    }
    img{
      position: relative;
      top: 90px;
    }
  </style>
</head>
<body>
<span>编辑</span>

<div style="width: 500px;height: 500px;border: 1px solid dodgerblue;margin-top: 200px">
    <img src="./img/bj.png" width="30px" >
     <div style="width: 50px;height: 50px;background-color: dodgerblue"></div>
</div>
</body>
```



## AI美化

...给我美化一下,要求高端大气上档次,网页运行的时候从天上掉下来

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI美化后的注册页面</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft YaHei", system-ui, sans-serif;
        }
        body{
            background-image: url("img/reg_bg_min.jpg");
            background-size: cover;
        }
        /* 页面外层弹性容器 */
        .wrap {
            display: flex;
            width: 100%;
            height: 100vh;
            align-items: center;
        }
        /* 左侧品牌文字区域 */
        #brand-box {
            width: 50%;
            padding: 0 80px;
            color: #ffffff;
            /* 文字淡入动画 */
            animation: textFade 1.2s ease-out forwards;
        }
        .logo {
            width: 140px;
            height: 45px;
            margin-bottom: 120px;
            filter: drop-shadow(0 0 10px rgba(255,255,255,0.3));
        }
        .brand-title h3 {
            font-size: 72px;
            font-weight: 600;
            letter-spacing: 4px;
            margin-bottom: 16px;
            text-shadow: 0 4px 20px rgba(0,0,0,0.2);
        }
        .brand-title p {
            font-size: 42px;
            font-weight: 300;
            opacity: 0.9;
        }
        .help-link {
            margin-top: 120px;
        }
        .help-link a {
            color: #fff;
            font-size: 18px;
            text-decoration: none;
            padding: 10px 20px;
            border: 1px solid rgba(255,255,255,0.4);
            border-radius: 999px;
            transition: all 0.4s ease;
        }
        .help-link a:hover {
            background: rgba(255,255,255,0.15);
            border-color: #fff;
        }

        /* 右侧注册卡片容器 + 从天掉落动画核心 */
        #form-box {
            width: 50%;
            display: flex;
            justify-content: flex-start;
        }
        .register-card {
            width: 520px;
            padding: 50px 40px;
            background: rgba(255, 255, 255, 0.92);
            border-radius: 20px;
            box-shadow: 0 15px 60px rgba(0, 0, 0, 0.25);
            /* 从天掉落回弹动画 */
            animation: cardDrop 1s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
            transform: translateY(-120vh);
        }

        /* 卡片头部 */
        .card-header h1 {
            font-size: 34px;
            color: #1a237e;
            margin-bottom: 12px;
            font-weight: 700;
        }
        .card-header span {
            font-size: 16px;
            color: #666;
        }
        .card-header a {
            color: #4834d4;
            text-decoration: none;
            font-weight: 500;
        }
        .card-header a:hover {
            text-decoration: underline;
        }

        /* 表单输入区域 */
        .form-item {
            margin-top: 26px;
        }
        .form-item span {
            display: block;
            font-size: 16px;
            color: #333;
            margin-bottom: 8px;
            font-weight: 500;
        }
        .form-item input {
            width: 100%;
            height: 48px;
            padding: 0 16px;
            border: 1px solid #d1d5db;
            border-radius: 12px;
            font-size: 16px;
            transition: all 0.4s ease;
            outline: none;
            background: #f8f9ff;
        }
        .form-item input:hover {
            border-color: #b4bfff;
            box-shadow: 0 0 0 4px rgba(72, 52, 212, 0.08);
        }
        .form-item input:focus {
            border-color: #4834d4;
            box-shadow: 0 0 0 4px rgba(72, 52, 212, 0.15);
            background: #fff;
        }

        /* 注册按钮高端渐变发光 */
        .submit-btn {
            width: 100%;
            height: 52px;
            margin-top: 36px;
            border: none;
            border-radius: 12px;
            font-size: 19px;
            font-weight: 600;
            color: #fff;
            background: linear-gradient(90deg, #4834d4, #6c5ce7);
            cursor: pointer;
            transition: all 0.4s ease;
            letter-spacing: 2px;
        }
        .submit-btn:hover {
            background: linear-gradient(90deg, #3725b3, #5b4bdb);
            box-shadow: 0 8px 24px rgba(72, 52, 212, 0.4);
            transform: translateY(-3px);
        }
        .submit-btn:active {
            transform: translateY(0);
        }

        /* 核心动画：卡片从天落下 */
        @keyframes cardDrop {
            0% {
                transform: translateY(-120vh);
                opacity: 0;
            }
            70% {
                transform: translateY(15px);
                opacity: 1;
            }
            100% {
                transform: translateY(0);
                opacity: 1;
            }
        }
        /* 文字淡入动画 */
        @keyframes textFade {
            from {
                opacity: 0;
                transform: translateX(-40px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
    </style>
</head>
<body>
<div class="wrap">

    <!-- 左侧品牌介绍 -->
    <div id="brand-box">
        <img class="logo" src="img/baidu.png" alt="logo">
        <div class="brand-title">
            <h3>用科技</h3>
            <p>让复杂的世界更简单</p>
        </div>
        <div class="help-link">
            <a href="https://passport.baidu.com/v2/?ucenterfeedback#reg">帮助中心</a>
        </div>
    </div>

    <!-- 右侧注册卡片（带下落动画） -->
    <div id="form-box">
        <div class="register-card">
            <div class="card-header">
                <h1>欢迎注册</h1>
                <span>已有账号？ <a href="#">立即登录</a></span>
            </div>
            <div class="form-item">
                <span>用户名</span>
                <input type="text" placeholder="请设置用户名">
            </div>
            <div class="form-item">
                <span>手机号码</span>
                <input type="tel" placeholder="请输入11位手机号">
            </div>
            <div class="form-item">
                <span>密&nbsp;&nbsp;&nbsp;&nbsp;码</span>
                <input type="password" placeholder="请设置6-16位密码">
            </div>
            <div class="form-item">
                <span>短信验证码</span>
                <input type="text" placeholder="请输入手机收到的验证码">
            </div>
            <button class="submit-btn">立即注册</button>
        </div>
    </div>
</div>
</body>
</html>
```

