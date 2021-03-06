# bootstrap  

## 1.环境搭配    
1. 基础bootstrap模板  
```html
<!DOCTYPE html>
<html>
   <head>
      <title>Bootstrap 模板</title>
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <!-- 引入 Bootstrap -->
      <link href="http://cdn.static.runoob.com/libs/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
  
      <!-- jQuery (Bootstrap 的 JavaScript 插件需要引入 jQuery) -->
      <script src="https://code.jquery.com/jquery.js"></script>
      <!-- 包括所有已编译的插件 -->
      <script src="http://cdn.static.runoob.com/libs/bootstrap/3.3.7/js/bootstrap.min.js"></script>
 
 
      <!-- HTML5 Shim 和 Respond.js 用于让 IE8 支持 HTML5元素和媒体查询 -->
      <!-- 注意： 如果通过 file://  引入 Respond.js 文件，则该文件无法起效果 -->
      <!--[if lt IE 9]>
         <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
         <script src="https://oss.maxcdn.com/libs/respond.js/1.3.0/respond.min.js"></script>
      <![endif]-->
   </head>
   <body>
      <h1>Hello, world!</h1>

   </body>
</html>
```
2. bootstrap使用了H5和CSS属性，所以需要使用H5代码段  
3. 触屏缩放meta标签  
  ``<meta name="viewport" content="width=device-width, initial-scale=1.0">``  
  禁止缩放：maximum-scale=1.0 与 user-scalable=no 一起使用。这样禁用缩放功能后，用户只能滚动屏幕  

---

## 2.全局样式配置  
1. 基本全局显示  
```html
body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 14px;
  line-height: 1.428571429;
  color: #333333;
  background-color: #ffffff;
}
```
第一条规则设置 body 的默认字体样式为 "Helvetica Neue", Helvetica, Arial, sans-serif。  
第二条规则设置文本的默认字体大小为 14 像素。  
第三条规则设置默认的行高度为 1.428571429。  
第四条规则设置默认的文本颜色为 #333333。  
最后一条规则设置默认的背景颜色为白色。  

2. 链接样式  
```html
a:hover,
a:focus {
  color: #2a6496;
  text-decoration: underline;
}

a:focus {
  outline: thin dotted #333;
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
```



---

## 3.标签元素  
### 1.响应式图像  
``<img src="..." class="img-responsive" alt="响应式图像">``  
宽度100%，高度auto,并且不超出父元素的尺寸  

### 2.内联子标题  
``<h1>我是标题1 h1. <small>我是副标题1 h1</small></h1>``  

### 3.字体变化  
``<small>（设置文本为父文本大小的 85%）、<strong>（设置文本为更粗的文本）、<em>（设置文本为斜体）``  

### 4.添加地址  
``<a href="mailto:#">mailto@somedomain.com</a>``  

### 5.引用，声明  
```xml
<blockquote>
  这是一个带有源标题的引用。
  <small>Someone famous in <cite title="Source Title">Source Title</cite></small>
</blockquote>
```

### 6.代码块  
```xml
<pre>
&lt;article&gt;
	&lt;h1&gt;Article Heading&lt;/h1&gt;
&lt;/article&gt;
</pre>
```
&lt表示<,&gt表示>  

### 7.下拉菜单  
```html
<div class="dropdown">
	<button type="button" class="btn dropdown-toggle"></button>
	<ul class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu1">
		<li role="presentation">
			<a role="menuitem" tabindex="-1" href="#">Java</a>
		</li>
	</ul>
</div>
```
btn dropdown-toggle：表示未点前提示  
dropdown-menu：表示下拉列表  

### 8.按钮组  
```html
<div class="btn-group">
    <button type="button" class="btn btn-default"></button>
</div>
```
另外还有按钮工具栏，嵌套按钮等  

---

因为 http://www.runoob.com/bootstrap 有许多案例，说得非常明白，所以就不再这里列举了。（还有轮播图，分页，导航等）  

---

## 4.网格系统  
网格系统是基于container标签里面的，当设定列数超出12个的时候，就会溢出到下一行  
1. 定义每一行：``<div class="row" >``  
2. 设立单元格的比例：``<div class="col-xs-6 col-sm-3">``  
  超小手机中占6/12,在小型设备中占3/12，另外md表示电脑，lg表示大型设备。12是分割出来的总份数  
3. 嵌套列``<div class="col-md-9"><div class="col-md-6"> ``  
  表示从9/12中再等分6份出来  
4. 偏移列：``<div class="col-xs-6 col-md-offset-3">``表示左边距增加三列  


---


bootstrap里面有制作后台管理的案例http://v3.bootcss.com/examples/dashboard/  
bootstrap&css3  
​      