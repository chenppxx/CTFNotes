# css

## 什么是 CSS?

- CSS 指层叠样式表 (**C**ascading **S**tyle **S**heets)
- 样式定义**如何显示** HTML 元素
- 样式通常存储在**样式表**中
- 把样式添加到 HTML 4.0 中，是为了**解决内容与表现分离的问题**
- **外部样式表**可以极大提高工作效率
- 外部样式表通常存储在 **CSS 文件**中
- 多个样式定义可**层叠**为一个

## 注释

/* 注释内容 */

## id和class选择器

### id选择器

id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。

id选择器以'#'来定义

![image-20231205152638943](../AppData/Roaming/Typora/typora-user-images/image-20231205152638943.png)

![image-20231205152650202](../AppData/Roaming/Typora/typora-user-images/image-20231205152650202.png)

**注意**:id属性不要以数字开头,在部分浏览器中会失效

### class选择器

实例:

`.center{text-align:center;}`

表示所有拥有center类的html元素均为居中:

`<h1 class="center">  内容  </h1>`

- 也可以指定特定的html元素使用class

`p.center {text-align:center;}`

所有的p元素使用`class="center"`会让该元素文本居中

- 多个class选择器可以使用空格分开

`<p class="center color>  内容  </p>"`

## css创建

当读到一个样式表时，浏览器会根据它来格式化 HTML 文档。

### 插入样式表三种方法

#### 外部样式表

当样式需要应用于很多页面时，外部样式表将是理想的选择。

每个页面使用 <link> 标签链接到样式表。 <link> 标签在（文档的）头部：

![image-20231205153929257](../AppData/Roaming/Typora/typora-user-images/image-20231205153929257.png)

浏览器会从文件mystyle.css中读到样式声明,并根据它来格式化文档

#### 内部样式表

当单个文档需要特殊的样式时，就应该使用内部样式表。

你可以使用 <style> 标签在文档头部定义内部样式表，就像这样:

![image-20231205154645603](../AppData/Roaming/Typora/typora-user-images/image-20231205154645603.png)

#### 内联样式

要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style 属性可以包含任何 CSS 属性.

![image-20231205154953349](../AppData/Roaming/Typora/typora-user-images/image-20231205154953349.png)

**多重样式优先级**:

内联样式>内部样式表>外部样式表>浏览器默认样式

## css background(背景)

CSS 属性定义背景效果:

- background-color
- background-image
- background-repeat
- background-attachment
- background-position

### 背景颜色

`body {background-color:"red";}`

### 背景图像

`body {background-image:url('paper.gif');}`

### 背景图像-水平或垂直平铺

**水平平铺**:

![image-20231205162713129](../AppData/Roaming/Typora/typora-user-images/image-20231205162713129.png)

### 背景图像- 设置定位与不平铺

不想让图像平铺且显示在右上角以免遮挡文字时:

![image-20231205162851628](../AppData/Roaming/Typora/typora-user-images/image-20231205162851628.png)

### 背景-简写属性

可以将属性合并在同一个属性中:

![image-20231205163048436](../AppData/Roaming/Typora/typora-user-images/image-20231205163048436.png)

当使用简写属性时，属性值的顺序为：:

- background-color
- background-image
- background-repeat
- background-attachment
- background-position

### 如何设定固定的背景图像

`body {background-attachment:fixed;}`

## css文本格式

### 字体大小

`{font-size:150%;}`

### 文本颜色

### 文本的对其方式

通过`text-align`实现

文本排列属性是用来设置文本的水平对齐方式。

文本可居中或对齐到左或右,两端对齐.

当text-align设置为"justify"，每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）。

### 文本修饰

`text-decoration` 属性用来设置或删除文本的装饰。

`a {text-decoration:none;}`删除链接的下划线

也可以装饰文字:

`h1 {text-decoration:overline;}`

`h1 {text-decoration:line-through;}`

`h1 {text-decoration:underline;}`

### 文本缩进

用来指定文本的第一行的缩进

`p {text-indent:50px;}`

### 指定字符

之间的空间

`letter-spacing:2px;`

### 指定行与行之间的空间

`line-height:70%;`

### 增加单词之间的空白空间

`word-spacing:30px;`

### 禁用元素内文字不换行

`white-space:nowrap;`

### 添加文本阴影

`h1 {text-shadow:2px 2px #FF0000;}`

## css字体

CSS字体属性定义字体，加粗，大小，文字样式。

![image-20231205213626440](../AppData/Roaming/Typora/typora-user-images/image-20231205213626440.png)

### css字型

在CSS中，有两种类型的字体系列名称：

- **通用字体系列** - 拥有相似外观的字体系统组合（如 "Serif" 或 "Monospace"）
- **特定字体系列** - 一个特定的字体系列（如 "Times" 或 "Courier"）

### css字体系列

font-family 属性设置文本的字体系列。

font-family 属性应该设置几个字体名称作为一种"后备"机制，如果浏览器不支持第一种字体，他将尝试下一种字体。

**注意**: 如果字体系列的名称超过一个字，它必须用引号，如Font Family："宋体"。

多个字体系列是用一个逗号分隔指明：

`p {font-family:"Times New Roman",Times,serif;}`

### css字体样式

![image-20231205214311941](../AppData/Roaming/Typora/typora-user-images/image-20231205214311941.png)

分别是正常的文字,斜体,倾斜的文字(和斜体非常类似,不太支持)

## css链接

链接的样式,可以用任何css属性.

特别的链接,可以有不同样式,这取决于他们的状态.

- a:link-正常,未访问的链接
- a:visited-已经访问过的链接
- a:hover-当用户鼠标放在链接上时
- a:active-点击链接时

实例:

`a:link{color:#000000;}`

当设置为若干链路状态的样式，也有一些顺序规则：

- a:hover 必须跟在 a:link 和 a:visited后面
- a:active 必须跟在 a:hover后面

## css列表

css列表属性作用:

- 设置列表项标记为有序列表
- 设置列表项标记为无序列表
- 设置列表项标记为图像

**无序列表**:`ul`

**有序列表**:`ol`

### 普通列表项标记

**list-style-type属性**

`ul.a {list-style-type:circle;}`

`ul.b {list-style-type:square;}`

`ol.c {list-style-type:upper-roman;}` 罗马数字的有序列表

`ol.d {list-style-type:lower-alpha;}` 小写字母的有序列表

### 图像作为列表项标记

**list-style-image属性**

`ul {list-style-image:url('地址')}`

## css表格

- 指定表格边框,使用border属性,下面的例子指定了一个表格的Th和TD元素的黑色边框：

table, th, td 

{ 

   border: 1px solid black;

 }

- **折叠边框**:border-collapse 属性设置表格的边框是否被折叠成一个单一的边框或隔开：

table {    border-collapse:collapse; }

 table,th, td {    border: 1px solid black; }

- width和height定义表格的宽度和高度

- **表格文字对其**:text-align属性设置文本水平对其方式,向左,右或中心.

vertical_align属性设置垂直对其,顶部,底部或中间.

- **表格填充**:控制边框和表格内容之间的间距,使用td和th元素的填充属性padding.
- **表格颜色**:background-color指定th和td中的背景颜色,color指定文本颜色.边框的颜色由table {  border:1px solid green }

## css盒子模型

不同部分的说明：

- **Margin(外边距)** - 清除边框外的区域，外边距是透明的。
- **Border(边框)** - 围绕在内边距和内容外的边框。
- **Padding(内边距)** - 清除内容周围的区域，内边距是透明的。
- **Content(内容)** - 盒子的内容，显示文本和图像。

div {    width: 300px;    border: 25px solid green;    padding: 25px;    margin: 25px; }

## css边框

**border-style**:属性用来定义边框的样式

- none:默认无边框

- dotted: 定义一个点线边框

- dashed: 定义一个虚线边框
- solid: 定义实线边框

- double: 定义两个边框。 两个边框的宽度和 border-width 的值相同

**边框宽度**:`border-width`属性可以指定边框宽度

可以指定长度值如2px     或者使用三个关键字:**hick,medium(默认),thin**

**边框颜色**:**border-color**

**边框-单独设置各边**:

p

{

​	border-top-style:dotted; 

​    border-right-style:solid; 

​    border-bottom-style:dotted; 

​    border-left-style:solid;

}

## css轮廓

- 使用`outline`属性在元素周围画线

p 
{
	border:1px solid red;
	outline:green dotted thick;
}

- 使用`outline-style`属性设置轮廓样式
- `outline-color`设置轮廓的颜色
- `outline-width`设置轮廓宽度

**注意**:

1.outline是不占空间的，既不会增加额外的width或者height（这样不会导致浏览器渲染时出现reflow或是repaint）

2.outline有可能是非矩形的（火狐浏览器下）

## css外边距

margin属性定义元素周围的空间

**单边外边距属性**:

`margin-top`	`margin-bottom`	`margin-left`	`margin-right`

**简写属性**:

`margin:25px 50px 75px 100px;`

分别表示上右下左(顺时针方向)

`margin:25px 50px 75px;`

分别表示上,左右,下

`margin:25px 50px;`

上下,左右

`margin:25px;`

都是25px

## css填充

`padding`属性定义填充属性

用法和`margin`完全形同

## css分组和嵌套选择器

**css分组选择器**:

h1,h2,p
{
    color:green;
}

**css嵌套选择器**:

- **p{ }**: 为所有 **p** 元素指定一个样式。
- **.marked{ }**: 为所有 **class="marked"** 的元素指定一个样式。
- **.marked p{ }**: 为所有 **class="marked"** 元素内的 **p** 元素指定一个样式。
- **p.marked{ }**: 为所有 **class="marked"** 的 **p** 元素指定一个样式。

## css尺寸

- 设置元素高度宽度

`p.ex
{
	height:100px;
	width:100px;
}`

## CSS Display(显示) 与 Visibility（可见性）

**隐藏元素`display:none`或`visibility:hidden`:**

**区别**:`visibolity`隐藏的元素仍会占用空间,影响布局,`display`不会占用任何空间,

### display-块和内联元素

- 块元素是一个元素,占用了全部宽度,在前后都是换行符.如`<h1>``<p>``<div>`
- 内联元素只需要必要的宽度,不强制换行.如`<span>``<a>`

**用display改变一个元素显示**:

`display:inline;`     显示为内联元素

`display:block;`       显示为块元素

## CSS Position(定位)

- static

	html元素的默认值,遵循正常的文档流

	静态定位的元素不会受到 top, bottom, left, right影响。

- fixed 固定

	元素位置相当与浏览器窗口固定

	`p.pos_fixed {    position:fixed;    top:30px;    right:5px; }`

- relative 相对定位

	以正常位置为标准进行移动,其原本所占空间不会改变

- absolute 绝对定位

	绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于<html>

	absolute 定位使元素的位置与文档流无关，因此不占据空间。

	absolute 定位的元素和其他元素重叠。

- sticky 粘性定位

	当页面滚动出目标区块,会像fixed一样固定在目标位置

	`div.sticky {    position: -webkit-sticky; /* Safari */    position: sticky;    top: 0;    background-color: green;    border: 2px solid #4CAF50; }`

- z-index 重叠元素

	指定一个元素的堆叠顺序

	`img {    position:absolute;    left:0px;    top:0px;    z-index:-1; }`

## CSS 布局 - Overflow(添加滚动条)

CSS overflow 属性可以控制内容溢出元素框时在对应的元素区间内添加滚动条。

`overflow:scroll`      会添加滚动条	

`overflow:auto`         如果内容被修剪,则会添加滚动条

`overflow:visible`    内容会被显示在显示框之外

## Float(浮动)

`img
{
    float:right;
}`

图片有浮动,如有文本流,会环绕在它左边

### float属性

对图片廊使用`float`属性

### clear属性

清除浮动

## css布局

### 元素居中对齐

`margin:auto;`可以使元素居中对齐

### 文本居中对齐

`text-align:center;`使文本居中对齐

### 图片居中对齐

`img {  display: block;  margin: 0 auto; }`

- `display: block;` 将图片转换成块级元素，使其能够设置宽度和高度。

### 使用定位方式来左右对齐

`position:absolute;`属性来对齐元素

`.right {	position:absolute;	right:0px;	}`

### 左右对齐 - 使用 float 方式

`.right {
    float: right;
    width: 300px;
    border: 3px solid #73AD21;
    padding: 10px;
}`

### 垂直居中对齐 - 使用 padding

`.center {
    padding: 70px 0;
    border: 3px solid green;
}`

## css组合选择符

- 后代选择器(以空格'   '分隔)
- 子元素选择器(以大于 **>** 号分隔）
- 相邻兄弟选择器（以加号 **+** 分隔）
- 普通兄弟选择器（以波浪号 **～** 分隔）

### 后代选择器

`div p
{
  background-color:yellow;
}`

### 子元素选择器

与后代选择器相比,只选择一级子元素

`div>p{}`

### 相邻兄弟选择器

如果需要选择紧接在另一个元素后的元素，而且二者有相同的父元素，可以使用相邻兄弟选择器

`div+p
{
  background-color:yellow;
}`

### 后续兄弟选择器

后续兄弟选择器选取所有指定元素之后的相邻兄弟元素。

`div~p
{
  background-color:yellow;
}`

## CSS 伪类(Pseudo-classes)

CSS伪类是用来添加一些选择器的特殊效果。

`a:link {color:#FF0000;} /* 未访问的链接 */
a:visited {color:#00FF00;} /* 已访问的链接 */
a:hover {color:#FF00FF;} /* 鼠标划过链接 */
a:active {color:#0000FF;} /* 已选中的链接 */`

### CSS :first-child 伪类

可以使用 :first-child 伪类来选择父元素的第一个子元素。

`p:first-child
{
    color:blue;
}`

**伪类结合css类**

`a.red:visited {color:#FF0000;}`

## css伪元素

- `:first-line`

	用于向文本的首行设置特殊样式

- `:first-letter`

	用于向文本的首字母设置特殊样式

- `:before`

	在元素的内容前面插入新内容

- `:after`

	可以在元素的内容后面插入新内容

**伪元素可以结合css类**:

`p.article:first-letter {color:#ff0000;}`

## css导航栏

**导航栏=链接列表**:使用<ul>和<li>元素

### 垂直导航栏

使用**内联(inline)**

`ul {    list-style-type: none;    margin: 0;    padding: 0; }`删除边距和填充

**注意**:可以在 **border** <ul> 上添加 **border** 属性来让导航栏有边框。如果要在每个选项上添加边框，可以在每个 <li> 元素上添加**border-bottom**

### 水平导航栏

使用**浮动(float)**

内联:`li {    display:inline; }`

浮动:`li {    float:left; }`

### 使用active来标记哪个选项被选中

`li a:hover:not(.active)`

`.active {background-color:green}`

## css下拉菜单

**HTML 部分：**

我们可以使用任何的 HTML 元素来打开下拉菜单，如：<span>, 或 a <button> 元素。

使用容器元素 (如： <div>) 来创建下拉菜单的内容，并放在任何你想放的位置上。

使用 <div> 元素来包裹这些元素，并使用 CSS 来设置下拉内容的样式。

**CSS 部分：**

`.dropdown` 类使用 `position:relative`, 这将设置下拉菜单的内容放置在下拉按钮 (使用 `position:absolute`) 的右下角位置。

`.dropdown-content` 类中是实际的下拉菜单。默认是隐藏的，在鼠标移动到指定元素后会显示。 注意 `min-width` 的值设置为 160px。你可以随意修改它。 **注意:** 如果你想设置下拉内容与下拉按钮的宽度一致，可设置 `width` 为 100% ( `overflow:auto` 设置可以在小尺寸屏幕上滚动)。

我们使用 `box-shadow` 属性让下拉菜单看起来像一个"卡片"。

`:hover` 选择器用于在用户将鼠标移动到下拉按钮上时显示下拉菜单。

<style>
.dropdown {
  position: relative;
  display: inline-block;
}
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
  padding: 12px 16px;
}
.dropdown:hover .dropdown-content {
  display: block;
}
</style>

<div class="dropdown">
  <span>鼠标移动到我这！</span>
  <div class="dropdown-content">
    <p>菜鸟教程</p>
    <p>www.runoob.com</p>
  </div>
</div>

## css属性选择器

`[title] {    color:blue; }`

把包含`title`的多有元素变为蓝色

`[title=runoob] {    border:5px solid green; }`

把包含`title=runoob`的元素设定边框样式

