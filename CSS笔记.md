# CSS课程笔记

## 背景相关

#### 设置背景图片位置：

 background-position:的值有两种表示方式

​                1.使用表示方位的单词     上：top  中:center  下:bottom     左:left   中: center  右:right

​                    background-positon:

​                        一个值的时候： 设置的这个值表示的就是该方向，另一个方向默认居中

​                        两个值的时候：设置的值就是表示的方向，而且与设置的值的顺序无关

​                2.设置具体的数值

​                     background-positon:

​                      一个值的时候：这个值表示水平方向的移动距离 ，垂直方向默认居中

​                      两个值的时候：第一个一定是表示水平方向移动的距离，第二个表示垂直方向移动的距离

#### 设置背景图片附着      

scroll: 滚动    fixed: 固定，附着

 /* background-attachment: scroll; */



## 三种模式总结区别

| 元素模式  | 元素排列        | 设置样式        | 默认宽度     | 包含           |
| ----- | ----------- | ----------- | -------- | ------------ |
| 块级元素  | 一行只能放一个块级元素 | 可以设置宽度高度    | 容器的100%  | 容器级可以包含任何标签  |
| 行内元素  | 一行可以放多个行内元素 | 不可以直接设置宽度高度 | 它本身内容的宽度 | 容纳文本或则其他行内元素 |
| 行内块元素 | 一行放多个行内块元素  | 可以设置宽度和高度   | 它本身内容的宽度 |              |



## 盒子模型

####  设置边框

 /* 设置边框宽度 */

​            /* border-width: 10px; */

​            /* 设置边框颜色 */

​            /* border-color: pink; */

​            /* 设置边框样式 */

​             /* border-style: solid;  实线边框 */

​            /* border-style: dotted;  点线边框 */

​            /* border-style: dashed;  虚线边框 */

​            /* border-style: double; 双实线边框 */

​            /* 边框的综合写法：

​                border: border-width  border-style  boreer-color;

​             */

​            /* border:   10px pink solid; */

   border-collapse: collapse;   /*将表格的边框合并成一条 */





##  文字阴影

 text-shadow    text-shadow: h-shadow v-shadow blur color;

 text-shadow: 100px  100px rgba(255,255,0,.5);



## 盒子阴影(CSS3)

- 语法:

```css
box-shadow:水平阴影 垂直阴影 模糊距离（虚实）  阴影尺寸（影子大小）  阴影颜色  内/外阴影；
```

- 前两个属性是必须写的。其余的可以省略。
- 外阴影 (outset) 是默认的 但是不能写           想要内阴影可以写  inset 

```css
div {
			width: 200px;
			height: 200px;
			border: 10px solid red;
			/* box-shadow: 5px 5px 3px 4px rgba(0, 0, 0, .4);  */
			/* box-shadow:水平位置 垂直位置 模糊距离 阴影尺寸（影子大小） 阴影颜色  内/外阴影； */
			box-shadow: 0 15px 30px  rgba(0, 0, 0, .4);
			
}
```

# 

## 浮动的特点：

​            1.脱标（脱离标准流）,不占标准流中的位置 

​            2.浮动改变元素的显示方式（针对行内元素    块级元素的显示方式不变，但是浮动之后鞠咏行内块级元素的特点）

​            3.如果浮动的元素前面是标准流的块级元素，那么浮动的元素只会在他元素的位置上左右浮动，不会和标准流的元素在一行显示；如果浮动的元素前面也是浮动的元素，那么这两个浮动的元素会在一行显示.



## 清除浮动

1.    .clearfix::after {

​            /* content: '.';       这个点的作用是为了解决浏览器的兼容问题 */

​            /* clear: both;     清除浮动的代码 */

​            /* display: block;   因为清除浮动需要使用块级元素，伪元素是一个行内元素，所以转成块级元素 */

​            /* visibility: hidden; 隐藏content中的内容 */

​            /* height: 0;   消除content中的内容撑开的高度 */

​            /* line-height: 0;  解决不同浏览器之间的兼容 */

​        }

​        .clearfix {

​            /* *zoom: 1;     为了兼容IE6-7 */

​        }



2.    .clearfix::before,

​        .clearfix::after {

​            content: '';

​            display: table;

​        }

​        .clearfix::after {

​            clear: both;

​        }



## 元素的显示和隐藏

​        .father:hover .son {

​          visibility: hidden;   隐藏元素，元素占位置 

​            visibility: visible;    显示元素 



​       display: none;     隐藏元素，不占位置 

​            display: block;     除了会将隐藏的元素显示，还会将元素转换成块级元素

​        }



##         鼠标样式 

​        a {

​            /* cursor: text;    文本光标 */

​            /* cursor: auto; */

​            /* cursor: default;  跟随操作系统鼠标样式 */

​        }

​        div {

​            /* cursor: pointer;   鼠标小手，   最常用的 */

​            /* cursor: move;   移动光标 */

​            /* cursor: auto;    跟随浏览器鼠标样式 */

​            cursor:default;

​        }



##             溢出处理

​             overflow: hidden;    /*溢出隐藏*/

​            /* overflow:auto;   有溢出加滚动条，没有溢出不加 */

​            /* overflow: visible;  默认的，不做任何处理 */

​            /* overflow: scroll;   始终都有滚动条 */



##            用省略号替代溢出部分 



​            /* 第一：不能让文本换行显示，让文本必须在一行现象 */

​            /* 文本不换行显示 */

​            white-space: nowrap;

​            /* 溢出隐藏 */

​            overflow: hidden;

​            /* 设置省略号替代 */

​            text-overflow: ellipsis;

##            设置字间距 

​            /* letter-spacing: 10px; */



##            设置内减模式 

​            /* box-sizing: content-box;   默认   border和padding会改变盒子大小 */

​            /* box-sizing: border-box;    内容区域减少，给border和padding，保证整个盒子大小不变 */





​     