# html

- html标题通过<h1>--<h6>标签来定义

- html段落通过<p>标签来定义

- html链接通过<a>标签来定义

- html图像通过<img>标签来定义

- 换行<br>

- <hr>创建水平线，分隔内容

	## html速查列表

	[HTML 速查列表 | 菜鸟教程 (runoob.com)](https://www.runoob.com/html/html-quicklist.html)
	
	## html注释
	
	`<!--注释内容-->`

## 语法

- HTML 元素以**开始标签**起始
- HTML 元素以**结束标签**终止
- **元素的内容**是开始标签与结束标签之间的内容
- 某些 HTML 元素具有**空内容（empty content）**
- 空元素**在开始标签中进行关闭**（以开始标签的结束而结束）
- 大多数 HTML 元素可拥有**属性**
- 大多数HTML元素可以嵌套

## 属性

- HTML 元素可以设置**属性**
- 属性可以在元素中添加**附加信息**
- 属性一般描述于**开始标签**
- 属性总是以名称/值对的形式出现，**比如：name="value"**。

实例：HTML链接由<a>标签定义。链接的地址在href属性中指定：

![image-20231127203459959](../AppData/Roaming/Typora/typora-user-images/image-20231127203459959.png)

- 属性值应该始终被包括在引号内。

## 注释

<!-- 这是一个注释 -->

## 段落

![image-20231127204700584](../AppData/Roaming/Typora/typora-user-images/image-20231127204700584.png)

- 折行

不希望产生一个新的段落的情况下进行换行，使用<br>标签

![image-20231127204817897](../AppData/Roaming/Typora/typora-user-images/image-20231127204817897.png)

- 对于 HTML，您无法通过在 HTML 代码中添加额外的空格或换行来改变输出的效果。

	当显示页面时，浏览器会移除源代码中多余的空格和空行。所有连续的空格或空行都会被算作一个空格。



## 格式化

- `<b>`加粗文本
-  `<strong>`加粗文本
- `<big>`放大字体
- `<em>`斜体
- `<i>`斜体
- `<small>`缩小文本
- `<sub>`下标
- `<sup>`上标

## 链接

HTML使用`<a>`来设置超文本链接，可以是一个字，一句话，一个图片

 <a href="https://www.runoob.com/">访问菜鸟教程</a>

![image-20231127211529201](../AppData/Roaming/Typora/typora-user-images/image-20231127211529201.png)

### 连接语法

- `href`：指定链接目标的URL，这是链接的最重要属性。可以是另一个网页的URL、文件的URL或其他资源的URL。
- `target`（可选）：指定链接如何在浏览器中打开。常见的值包括 `_blank`（在新标签或窗口中打开链接）和 `_self`（在当前标签或窗口中打开链接）。
- `title`（可选）：提供链接的额外信息，通常在鼠标悬停在链接上时显示为工具提示。
- `rel`（可选）：指定与链接目标的关系，如 nofollow、noopener 等。

### 锚点链接

除了链接到其他网页外，您还可以在同一页面内创建内部链接，这称为锚点链接。要创建锚点链接，需要在目标位置使用 <a> 元素定义一个标记，并使用#符号引用该标记。例如：

```
<a href="#section2">跳转到第二部分</a>
<!-- 在页面中的某个位置 -->
<a name="section2"></a>
```

### 下载链接

```
<a href="document.pdf" download>下载文档</a>
```

### 跳出框架

![image-20231127211955581](../AppData/Roaming/Typora/typora-user-images/image-20231127211955581.png)

## 头部

### head元素

 head元素包含了所有的头部标签元素。在 <head>元素中你可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。

### title元素

- 在html文档中是必需的
- 显示在搜索引擎结果页面的标题

### base元素

base标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接。

<head>
<base href="http://www.runoob.com/images/" target="_blank">
</head>

### link元素

`<link>` 标签定义了文档与外部资源之间的关系。

`<link>` 标签通常用于链接到样式表:

<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>

### style元素

<style> 标签定义了HTML文档的样式文件引用地址.
在<style> 元素中你也可以直接添加样式来渲染 HTML 文档:

<head>
<style type="text/css">
body {
    background-color:yellow;
}
p {
    color:blue
}
</style>
</head>
### meta元素

meta标签描述了一些基本的元数据。

<meta> 标签提供了元数据.元数据也不显示在页面上，但会被浏览器解析。

META 元素通常用于指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。

元数据可以使用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他Web服务。

<meta> 一般放置于 <head> 区域

### script元素

用于加载脚本文件

## css

CSS 可以通过以下方式添加到HTML中:

- 内联样式- 在HTML元素中使用"style" **属性**
- 内部样式表 -在HTML文档头部 `<head>` 区域使用`<style>` **元素** 来包含CSS
- 外部引用 - 使用外部 CSS **文件**

最好的方式是通过外部引用CSS文件.

## 图像

在 HTML 中，图像由`<img> `标签定义。

`<img>` 是空标签，意思是说，它只包含属性，并且没有闭合标签。

要在页面上显示图像，你需要使用源属性（src）。src 指 "source"。源属性的值是图像的 URL 地址。

**定义图像的语法是：**

<img src="url" alt="some_text">

URL 指存储图像的位置。如果名为 "pulpit.jpg" 的图像位于 www.runoob.com 的 images 目录中，那么其 URL 为 [http://www.runoob.com/images/pulpit.jpg](https://www.runoob.com/images/pulpit.jpg)。

### alt属性

alt 属性用来为图像定义一串预备的可替换的文本。

替换文本属性的值是用户定义的。

<img src="boat.gif" alt="Big Boat">

在浏览器无法载入图像时，替换文本属性告诉读者她们失去的信息。此时，浏览器将显示这个替代性的文本而不是图像。为页面上的图像都加上替换文本属性是个好习惯，这样有助于更好的显示信息，并且对于那些使用纯文本浏览器的人来说是非常有用的。

### 设置图像的高度与宽度

height（高度） 与 width（宽度）属性用于设置图像的高度与宽度。

属性值默认单位为像素:

<img src="pulpit.jpg" alt="Pulpit rock" width="304" height="228">

**提示:** 指定图像的高度和宽度是一个很好的习惯。如果图像指定了高度宽度，页面加载时就会保留指定的尺寸。如果没有指定图片的大小，加载页面时有可能会破坏HTML页面的整体布局。

## html表格

html表格有`<table>`标签来定义

- tr：表示表格的一行
- td：表示表格的单元数据格
- th：表示表格的表头单元格

`<caption>`可以为表格加上标题

## html列表

### 无序列表

<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>

### 有序列表

<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>

## html区块

### html区块元素

大多数 HTML 元素被定义为**块级元素**或**内联元素**。

块级元素在浏览器显示时，通常会以新行来开始（和结束）。

实例: `<h1>, <p>, <ul>, <table>`

### html内联元素

内联元素在显示时通常不会以新行开始。

实例: `<b>, <td>, <a>, <img>`

### `<div>`元素

HTML `<div>` 元素是块级元素，它可用于组合其他 HTML 元素的容器。

可以设置宽度、高度以及边距等样式属性。它适合用于创建页面的大块结构，例如页面的主体区域、容器、布局等。

![image-20231203210620345](../AppData/Roaming/Typora/typora-user-images/image-20231203210620345.png)

### `<span>`元素

HTML` <span>` 元素是内联元素，可用作文本的容器.

它适合用于对文本或其他行内元素进行样式化、标记或包裹。

![image-20231203211444265](../AppData/Roaming/Typora/typora-user-images/image-20231203211444265.png)

## html布局

网页布局对改善网站外观非常重要.

### `<div>`布局

![image-20231204193527743](../AppData/Roaming/Typora/typora-user-images/image-20231204193527743.png)



### `<table>`布局

![image-20231204193545238](../AppData/Roaming/Typora/typora-user-images/image-20231204193545238.png)

## html表单和输入

HTML 表单用于收集用户的输入信息。

HTML 表单表示文档中的一个区域，此区域包含交互控件，将用户收集到的信息发送到 Web 服务器。

HTML 表单通常包含各种输入字段、复选框、单选按钮、下拉列表等元素。

- `<form>` 元素用于创建表单，`action` 属性定义了表单数据提交的目标 URL，`method` 属性定义了提交数据的 HTTP 方法（这里使用的是 "post"）。
- `<label>` 元素用于为表单元素添加标签，提高可访问性。
- `<input>` 元素是最常用的表单元素之一，它可以创建文本输入框、密码框、单选按钮、复选框等。`type` 属性定义了输入框的类型，`id` 属性用于关联 `<label>` 元素，`name` 属性用于标识表单字段。
- `<select>` 元素用于创建下拉列表，而 `<option>` 元素用于定义下拉列表中的选项。

我们可以使用`<form>`标签来创建表单

### 输入元素

多数情况下被用到的表单标签是输入标签 **<input>**。

输入类型是由 **type** 属性定义。

几种常用的输入类型:

1.**文本域**:通过`<input type="text">`标签来定义,当用户在表单中键入字母,数字等内容时,就会用到文本域

在大多数浏览器中,文本域的默认宽度时20个字符

2.__密码字段__:通过标签`<input type="password>"`来定义

3.__单选按钮__:`<input type="radio">`:

![image-20231204195904349](../AppData/Roaming/Typora/typora-user-images/image-20231204195904349.png)

4.__复选框__:`<input type="checkbox"`,可以选取一个或多个选项

5.__提交按钮__:`<input type="submit">`定义了提交按钮.

当用户单击确认按钮时,表单的内容会被传送到服务器.表单的动作属性`action`定义了服务端的文件名,`action`属性会对接收到的用户输入数据进行相关的处理.

![image-20231204200230867](../AppData/Roaming/Typora/typora-user-images/image-20231204200230867.png)

上述输入数据会传送到html_form_action.php文件

以上实例中有一个 method 属性，它用于定义表单数据的提交方式，可以是以下值：

- **post**：指的是 HTTP POST 方法，表单数据会包含在表单体内然后发送给服务器，用于提交敏感数据，如用户名与密码等。
- **get**：默认值，指的是 HTTP GET 方法，表单数据会附加在 **action** 属性的 URL 中，并以 **?**作为分隔符，一般用于不敏感信息，如分页等。例如：https://www.runoob.com/?page=1，这里的 page=1 就是 get 方法提交的数据。

## html框架

通过使用框架,可以在同一个浏览器中显示不止一个页面.

## iframe-设置宽度高度

`<iframe src="demo_iframe.htm" width="200" height="200"></iframe>`

### iframe-移除边框

`<iframe src="demo_iframe.htm" width="200" height="200" frameborder="0"></iframe>`

### 使用iframe来显示目标链接页面

目标链接的属性必须使用iframe属性:

![image-20231204215446668](../AppData/Roaming/Typora/typora-user-images/image-20231204215446668.png)

## html颜色

html颜色由红色,绿色,蓝色混合而成.

HTML 颜色由一个十六进制符号来定义，这个符号由红色、绿色和蓝色的值组成（RGB）。

每种颜色的最小值是0（十六进制：#00）。最大值是255（十六进制：#FF）。

## html颜色名

## html脚本

## html字符实体

在 HTML 中，某些字符是预留的。

在 HTML 中不能使用小于号（<）和大于号（>），这是因为浏览器会误认为它们是标签。

如果希望正确地显示预留字符，我们必须在 HTML 源代码中使用字符实体（character entities）。

<:&lt       >:&gt

## URL

语法规则:

**scheme`://`host.domain`:`port`/`path`/`filename**

说明:

- - scheme - 定义因特网服务的类型。最常见的类型是 http
	- host - 定义域主机（http 的默认主机是 www）
	- domain - 定义因特网域名，比如 runoob.com
	- :port - 定义主机上的端口号（http 的默认端口号是 80）
	- path - 定义服务器上的路径（如果省略，则文档必须位于网站的根目录中）。
	- filename - 定义文档/资源的名称

### 常见的URL Scheme

![image-20231204225344565](../AppData/Roaming/Typora/typora-user-images/image-20231204225344565.png)

