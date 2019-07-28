# HTML第1天课程笔记

## 网页(PC端)

##### 网页的组成

​	网页一般都是有图片,文本,链接,视频,音频,flash..........         

​    积木:  圆形,方块,长方形,半圆形,三角形......      搭建成一个城堡:      做网页 就如同是搭积木  

##### 世界5大浏览器

IE: Trident        

firefox: Gecko     

chrome: blink     

safari: webkit       

opera: presto 

##### W3C(万维网联盟)制定一套统一的标准      

###### Web标准 (web标准并不是一个标准,而是有一系列的标准组成)

​		1.结构标准:  用来控制网页的是元素的结构     -----  HTML

​		2.样式标准:  用来设置网页中的元素的外观样式,如:字体大小,颜色,粗细.....    ---------CSS

​		3.行为标准: 用来控制页面的交互行为    ---------JavaScript     js

 拓展:  

​	HTML控制结构,css控制样式,js控制行为   

​	网页布局中一个重要的原则:   三层分离原则----将HTML,css,js分开来写   

​	三层分离:     效率更高, 便于后期的维护



## HTML ----页面结构

### HTML的概念

 HTML: 超文本标签语言

​	语言:   沟通      沟通对象              人和浏览器

​	标签:  浏览器认识,我们也认识      就是通过标签去认识我们给到浏览器的东西

​	超文本:   比普通文本更牛逼的文本  点击能跳转页面

我们学习HTML其实就是学习大量的**标签**

标签的学习方法:

​	1.这个标签是干什么的.       这个标签在浏览器中的作用

​	2.知道这个标签的写法



## HTML的基本结构

​	我们做网页的时候,所有的网页上都有这样的一个结构

```html
<!doctype  html>
<html>
    <head>
         <title></title>
    </head> 

    <body>
    </body>
</html>
```

HTML中标签的写法:      <标签名字></标签名字>

head: 头部 

​	title: 标题

body: 主体

#### HTML基本结构中代码的含义

<!doctype  html>
<html>

```
<head>
     <title></title>
</head> 
```

```html
<!doctype  html>
<html>
    <head>
         <title></title>
    </head> 

    <body>
    </body>
</html>
```

<!doctype  html>:  doctype====> document type   文档类型
告诉浏览器我们下面所写的HTML代码遵循HTML的哪一个版本  (版本向下兼容)
doctype   html   表示使用的使用html5的版本语法

html: html页面的根标签    用来包裹页面上的其他内容或标签的

head: 页面的头部      包裹一些辅助的内容         如: title     link     script     base    .....

title:   给页面设置一个标题   在浏览器的页头部分

body: 页面的主体,页面中所有可以见的内容都是在body中

# HTML第2天笔记

##### a标签的几个作用:

​     1.超链接,实现页面间的跳转    <a href="要跳转的页面的路径"></a>      使用最多

​     2.实现锚点

​        1.没有发生页面间跳转

​        2.给锚点添加一个id属性,设置一个值 id="one".  a标签的href="#one"

​     3.下载

#####  表格的单元格合并:

​        跨行合并: rowspan

​        跨列合并  colspan 

​        单元格合并原则: 从上到下,从左到右.

##### 文本域: textarea 

​    cols: 一行可以显示的字符数 

​    rows: 可以显示的行数

##### 表单元素分组标签

<fieldset> 

<legend>设置分组名称</legend></fieldset>

##### 关联标签: 

​    <label></label>

​    使用方式:

​        1.直接使用label标签包裹表单元素和文本

​        2.利用label标签中的for属性

​            需要给表单元素设置id属性