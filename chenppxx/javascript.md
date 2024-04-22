# javascript

- `type` 属性用于指定 `<script>` 标签中脚本的类型，但在现代的 HTML5 中，通常可以省略 `type` 属性，因为默认值为 `text/javascript`。

- 使用 `document.write()` 方法在页面加载完成后会覆盖整个文档，这可能导致意外行为。建议使用 `innerHTML` 属性来更新元素的内容，而不是直接使用 `document.write()`。

- **indexof()**方法从头开始搜索指定字符串,返回第一次出现的位置

- **lastIndexOf()**方法从末尾开始搜索,返回最后一次出现的位置

	两者都可以指定第二个参数,从指定位置搜索.如:

	`str.indexof("a",5)`

## js输出

### JavaScript 显示数据

JavaScript 可以通过不同的方式来输出数据：

- 使用 **window.alert()** 弹出警告框。
- 使用 **document.write()** 方法将内容写到 HTML 文档中。
- 使用 **innerHTML** 写入到 HTML 元素。
- 使用 **console.log()** 写入到浏览器的控制台。

### 操作HTML元素

如需从 JavaScript 访问某个 HTML 元素，您可以使用 document.getElementById(*id*) 方法。

请使用 "id" 属性来标识 HTML 元素，并 innerHTML 来获取或插入元素内容：

`<p> id="demo>我的第一个段落.</p>`

`<script>`

`document.getElementById("demo").innerHTML="段落已修改.";`

`</script>`



## js语法

### js字面量

在编程语言中,一般固定值称为字面量.如3.14

**数字(Number)字面量**可以是整数或者小数,或者科学计数法(e)

**字符串（String）字面量** 可以使用单引号或双引号

**表达式字面量** 用于计算

**数组（Array）字面量** 定义一个数组：

[40, 100, 1, 5, 25, 10]

**对象（Object）字面量** 定义一个对象：

{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}

**函数（Function）字面量** 定义一个函数：

function myFunction(a, b) { return a * b;}

### JavaScript 变量

在编程语言中，变量用于存储数据值。

JavaScript 使用关键字 **var** 来定义变量， 使用等号来为变量赋值：

`var x,length;`

`x = 5`;

`length = 6;`

### js操作运算符

使用**算术运算符**给变量赋值

使用**赋值运算符**给变量赋值

### js注释

`//`是单行注释

`/*``*/`是多行注释

### js数据类型

js有多种数据类型:数字,字符串,数组,对象等等;

`var length = 16;                                  // Number 通过数字字面量赋值 
var points = x * 10;                              // Number 通过表达式字面量赋值
var lastName = "Johnson";                         // String 通过字符串字面量赋值
var cars = ["Saab", "Volvo", "BMW"];              // Array  通过数组字面量赋值
var person = {firstName:"John", lastName:"Doe"};  // Object 通过对象字面量赋值`

### 数据类型概念

16 + "volve"    ---    输出"16volve"

### js函数

js语句可以写在函数内,函数可以重复引用:

**引用一个函数** = 调用函数(执行函数内的语句).

`function myFunction(a, b) {
   	return a * b;         
                     
// 返回 a 乘以 b 的结果
}`

### JS字母大小写

js对字母大小写是敏感的

**getElementByid** 和 **getElementbiID** 是不同的

### js字符集

js使用Unicode字符集



## js变量

- 变量必须以字母开头
- 变量也能以 $ 和 _ 符号开头（不过我们不推荐这么做）
- 变量名称对大小写敏感（y 和 Y 是不同的变量）

**重新声明js变量,变量的值不会丢失**

## js数据类型

### js拥有动态类型

**相同的变量可用作不同的类型**

变量的数据类型可以使用`typeof`操作符来看

**typeof** "John"         *// 返回 string*
**typeof** 3.14          *// 返回 number*
**typeof** **false**         *// 返回 boolean*
**typeof** [1,2,3,4]       *// 返回 object*
**typeof** {name:'John', age:34} *// 返回 object*

### js数组

`var cars=new Array("Saab","Volvo","BMW");`

创建了一个名位 **cars** 的数组

### Undefined和Null

Undefined 这个值表示变量不含有值。

可以通过将变量的值设置为 null 来清空变量。

### 声明变量类型

声明新变量是,可以使用关键词`new`来声明其类型,如:

`var carname = new String;`

  ## js对象

对象也是一个变量,但对象可以包含多个值(多个变量),每个值以`name:value`的形式赋值

`var car = {name:"jack",model:500,color:"white"};`

### 对象属性

可以说 "JavaScript 对象是变量的容器"。

但是，我们通常认为 "JavaScript 对象是键值对的容器"。

键值对通常写法为 **name : value** (键与值以冒号分割)。

键值对在 JavaScript 对象通常称为 **对象属性**。

### 访问对象属性

`person.lastname`

`person["lastname"]`

### 对象方法

对象的方法定义了一个函数,并作为对象的属性存储.

该方法通过添加()调用(作为一个函数).

`name = person.fullName();`

`name = person.fullName;`

前者调用函数执行

后者作为一个定义函数的字符串返回

## JS函数

函数就是包裹在花括号中的代码块，前面使用了关键词 function：

function *functionname*()
{
  *// 执行代码*
}

**调用带参数的函数**:

`<script>`

`function myFunction(name,job){`

`     	alert("Welcome " + name + ", the " + job); `     

`} `

`</script>`

**带有返回值的函数**:通过**return**实现

如果仅需要退出函数,也可以使用**return**语句

### 局部js变量

在 JavaScript 函数内部声明的变量（使用 var）是*局部*变量，所以只能在函数内部访问它。（该变量的作用域是局部的）。

您可以在不同的函数中使用名称相同的局部变量，因为只有声明过该变量的函数才能识别出该变量。

只要函数运行完毕，本地变量就会被删除。

### 全局js变量

在函数外声明的变量是*全局*变量，网页上的所有脚本和函数都能访问它。

**局部变量会在函数运行后删除**

**全局变量会在页面关闭后删除**

如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量。

## js字符串

字符串的索引从0开始

**可以在字符串中使用引号,字符串中的引号不要与字符串的引号相同**

**可以在字符串中添加转义字符来使用引号**

### 字符串长度

使用内置属性**length**来计算字符串的长度

`var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";`

`var sln = txt.length;`

### 模板字符串

JavaScript 中的模板字符串是一种方便的字符串语法，允许你在字符串中嵌入表达式和变量。

模板字符串使用反引号 **``** 作为字符串的定界符分隔的字面量。

模板字面量是用反引号（**`**）分隔的字面量，允许多行字符串、带嵌入表达式的字符串插值和一种叫带标签的模板的特殊结构。

### 占位符${expression}

除了普通字符串外，模板字面量还可以包含占位符——一种由美元符号和大括号分隔的嵌入式表达式：**${expression}**。

字符串和占位符被传递给一个函数（要么是默认函数，要么是自定义函数）。默认函数（当未提供自定义函数时）只执行字符串插值来替换占位符，然后将这些部分拼接到一个字符串中。

**模板字符串当作HTML模板使用**:

let header = "";
let tags = ["RUNOOB", "GOOGLE", "TAOBAO"];

let html = `<h2>${header}</h2><ul>`;
for (const x of tags) {
  html += `<li>${x}</li>`;
}

html += `</ul>`;

## js运算符

与c语言类似

**数字和字符串相加,返回字符串**

- `===`:绝对等于(值和类型均相等)
- `!==`:不绝对等于(值和类型有一个不相等,或两个都不相等)

### 条件运算符

voteable=(age<18)?"年龄太小":"年龄已达到";

## js条件语句

- **if 语句** - 只有当指定条件为 true 时，使用该语句来执行代码
- **if...else 语句** - 当条件为 true 时执行代码，当条件为 false 时执行其他代码
- **if...else if....else 语句**- 使用该语句来选择多个代码块之一来执行
- **switch 语句** - 使用该语句来选择多个代码块之一来执行

### for/in循环

var person={fname:"Bill",lname:"Gates",age:56};   

for (x in person)  // x 为属性名 

{    txt=txt + person[x];   }

### for循环 while循环 do/while循环

c语言类似

## break 和 continue 语句

和c语言类似

### js标签

可以对js语句进行标记

`label:`

`statement`

须在语句之前加上冒号

list:

{

​	语句;

​	语句;

​	break list;

​	语句;

​	语句;

}

## typeof,null,undefined

### typeof

检测变量的数据类型

**注意**:数组是一种特殊的对象类型,返回object

### null

**null**是一个只有一个值的特殊类型,表示一个空对象引用.

可以设置为**null**来清空对象

**注意**:typeof null的返回值是object

### undefined

**undefined**是一个没有设置值的变量

可以设置为**undefined**来清空对象

**注意**:typeof返回值是undefined

**区别**:**null**和**undefined**的值相等,但类型不同

null === undefined      //False

null == undefined       //True

## js类型转换

js有6种不同的数据类型:

- string
- number
- boolean
- object
- function
- symbol

3种对象类型:

- Object
- Date
- Array

2个不包含任何值的数据类型:

- null
- undefined

### constructor属性

**constructor** 属性返回所有 JavaScript 变量的构造函数。

"John".constructor         // 返回函数 String() { [native code] }
(3.14).constructor         // 返回函数 Number() { [native code] }
false.constructor         // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor       // 返回函数 Array()  { [native code] }
{name:'John', age:34}.constructor // 返回函数 Object() { [native code] }
new Date().constructor       // 返回函数 Date()  { [native code] }
function () {}.constructor     // 返回函数 Function(){ [native code] }

### string(),tostring()将数字转换成字符串

String(x)`

`String(123)`

`String(100+23)`

`x.toString()`
`(123).toString()`
`(100 + 23).toString()`

**String()**和**toString**也可以将布尔值,日期转换为字符串

### 字符串转换为数字

全局方法 **Number()** 可以将字符串转换为数字。

字符串包含数字(如 "3.14") 转换为数字 (如 3.14).

空字符串转换为 0。

其他的字符串会转换为 NaN (不是个数字)。

### 一元运算符+

**Operator +** 可用于将变量转换为数字：

`var y = "5";   // y 是一个字符串`
`var x = + y;   // x 是一个数字`

如果变量不能转换，它仍然会是一个数字，但值为 NaN (不是一个数字):

`var y = "John";  // y 是一个字符串`
`var x = + y;   // x 是一个数字 (NaN)`

### 布尔值转换为数字

`Number(false)   // 返回 0`
`Number(true)   // 返回 1`

### 日期转换为数字

**Number()**

`d = new Date();
Number(d)     // 返回 1404568027739`

**getTime()**

`d = new Date();
d.getTime()    // 返回 1404568027739`

### 自动转换类型

当 JavaScript 尝试操作一个 "错误" 的数据类型时，会自动转换为 "正确" 的数据类型。

5 + null  	// 返回 5     	null 转换为 0
"5" + null 	// 返回"5null" 	 null 转换为 "null"
"5" + 1   	// 返回 "51"   	1 转换为 "1" 
"5" - 1   	// 返回 4     	"5" 转换为 5

### 自动转换为字符串

当你尝试输出一个对象或一个变量时 JavaScript 会自动调用变量的 toString() 方法

## js正则表达式

正则表达式是由一个字符序列形成的搜索模式。

当你在文本中搜索数据时，你可以用搜索模式来描述你要查询的内容。

正则表达式可以是一个简单的字符，或一个更复杂的模式。

正则表达式可用于所有文本搜索和文本替换的操作。

### 语法

```
/正则表达式主体/修饰符(可选)
```

`var patt = /runoob/i`

**/runoob/i**是一个正则表达式

**runoob**是正则表达式主体(用于检索)

**i**是一个修饰符(搜索不区分大小写)

### search()

- 使用正则表达式

	var str = "Visit Runoob!";  

	var n = str.search(/Runoob/i);

返回字符串起始位置,输出6

- 使用字符串

	var str = "Visit Runoob!";  

	var n = str.search("Runoob");

### replace()

- 使用正则表达式

	var str = document.getElementById("demo").innerHTML;  var txt = str.replace(/microsoft/i,"Runoob");

- 使用字符串

	var str = document.getElementById("demo").innerHTML;  var txt = str.replace("Microsoft","Runoob");

### 正则表达式修饰符

**i**:执行对大小写不敏感的匹配

**g**:执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。

**m**: 执行多行匹配。

### 正则表达式模式

*方括号用于查找某个范围内的字符*

**[abc]**:查找方括号之间的任何字符

**[0-9]**:查找0-9的数字

**(x|y)**:查找任何以|分隔的选项

**\d**:查找数字

**\s**:查找空格

**\b**:匹配单词边界

**n+**: 匹配任何包含至少一个 *n* 的字符串。

**n***: 匹配任何包含零个或多个 *n* 的字符串。

**n?**:匹配任何包含零个或一个 *n* 的字符串。

### 正则表达式方法

#### test()

test() 方法用于检测一个字符串是否匹配某个模式，如果字符串中含有匹配的文本，则返回 true，否则返回 false。

以下实例用于搜索字符串中的字符 "e"：

`var patt = /e/;
patt.test("The best things in life are free!");`

可以不用设置正则表达式变量

`/e/.test("The best things in life are free!")`

#### exec()

exec() 方法用于检索字符串中的正则表达式的匹配。

该函数返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。

以下实例用于搜索字符串中的字母 "e":

`/e/.exec("The best things in life are free!");`

## JavaScript 错误 - throw、try 和 catch

**try** 语句测试代码块的错误。

**catch** 语句处理错误。

**throw** 语句创建自定义错误。

**finally** 语句在 try 和 catch 语句之后，无论是否有触发异常，该语句都会执行。

**try**和**catch**语句成对出现



function myFunction() {
  var message, x;
  message = document.getElementById("p01");
  message.innerHTML = "";
  x = document.getElementById("demo").value;
  try { 
    if(x == "") throw "值是空的";
    if(isNaN(x)) throw "值不是一个数字";
    x = Number(x);
    if(x > 10) throw "太大";
    if(x < 5) throw "太小";
  }
  catch(err) {
    message.innerHTML = "错误: " + err + ".";
  }
  finally {
    document.getElementById("demo").value = "";
  }
}



**finally**语句一定会执行

**throw**语句允许我们创建自定义错误

正确的技术术语是:创建或**抛出异常**.

如果把 throw 与 try 和 catch 一起使用，那么您能够控制程序流，并生成自定义的错误消息。

## js调试

### console.log()

如果浏览器支持调试,可以使用**console.log()**在调试窗口上打印js值

### 设置断点

在调试窗口中,可以设置js代码的断点

在每个断点上，都会停止执行 JavaScript 代码，以便于我们检查 JavaScript 变量的值。

在检查完毕后，可以重新执行代码（如播放按钮）。

### debugger

**debugger** 关键字用于停止执行 JavaScript，并调用调试函数。

这个关键字与在调试工具中设置断点的效果是一样的。

如果没有调试可用，debugger 语句将无法工作。

## js变量提升

### js声明提升

变量可以在使用后声明,也就是说变量可以先使用再说明

**声明提升：函数声明和变量声明总是会被解释器悄悄地被"提升"到方法体的最顶部。**

### js初始化不会提升

var x = 5; // 初始化 x

elem = document.getElementById("demo"); // 查找元素
elem.innerHTML = x + " " + y;      // 显示 x 和 y

var y = 7; // 初始化 y

上述,var y提升了.但是y=7不会提升

## js严格模式(use strict)

### 严格模式声明

严格模式通过在脚本或函数的头部添加 **use strict**; 表达式来声明。

实例中我们可以在浏览器按下 **F12 (或点击"工具>更多工具>开发者工具")** 开启调试模式，查看报错信息。

`"use strict";
x = 3.14;    // 报错 (x 未定义)`



`"use strict";
myFunction();`

`function myFunction() {
  y = 3.14;  // 报错 (y 未定义)
}`

**在函数内部声明是局部作用域 (只在函数内使用严格模式):**

`x = 3.14;    // 不报错
myFunction();`

`function myFunction() {
  "use strict";`
`  y = 3.14;  // 报错 (y 未定义)
}`

### 严格模式的限制

- 不允许使用未声明的变量

- 不允许删除变量或对象

- 不允许删除函数

- 不允许变量重名

- 不允许使用八进制

- 不允许使用转义字符

- 不允许对只读属性赋值

- 不允许对一个使用getter()方法获取的属性进行赋值

- 不允许删除一个不允许删除的属性

- 变量名不能使用"eval"字符串

- 不能使用"arguments"字符串

- 不允许使用以下语句

	`"use strict";
	with (Math){x = cos(2)}; // 报错`

- 由于一些安全原因,在作用域eval()创建的变量不能被调用
- 禁止**this**关键字指向全局对象

### eval()

`eval()` 是 JavaScript 中的一个内置函数，用于将字符串作为代码进行解析和执行。它接受一个字符串作为参数，并将该字符串解析为可执行的 JavaScript 代码。

使用 `eval()` 函数时，传递给它的字符串会被当作 JavaScript 代码进行解析和执行。这可以用于动态地执行字符串形式的代码，例如：

`const code = "console.log('Hello, world!');";`

`eval(code); *// 输出 "Hello, world!"*`

### 保留关键字

为了向将来Javascript的新版本过渡，严格模式新增了一些保留关键字：

- implements
- interface
- let
- package
- private
- protected
- public
- static
- yield

## js使用误区

### 比较运算符常见错误

- 在常规的比较中，数据类型是被忽略的，以下 if 条件语句返回 true：

`var x = 10;
var y = "10";`
`if (x == y)`

- `switch`语句会使用恒等运算符(===)进行比较运算

### 浮点型数据使用注意事项

JavaScript 中的所有数据都是以 64 位**浮点型数据(float)** 来存储。

所有的编程语言，包括 JavaScript，对浮点型数据的精确度都很难确定：

`var x = 0.1;
var y = 0.2;`
`var z = x + y      // z 的结果为 0.30000000000000004`
`if (z == 0.3)      // 返回 false`

为解决以上问题，可以用整数的乘除法来解决：

`var z = (x * 10 + y * 10) / 10;    // z 的结果为 0.3`

### 字符串分行

字符串断行需使用反斜杠(\),如:

`var x = "hello \`

`world!;"`

## html表单

### 获取表单的输入值

`var x=document.form["表单名字"]["input名字"].value`

### html表单验证

HTML 表单验证可以通过 JavaScript 来完成。

`function validateForm() {    `

`var x = document.forms["myForm"]["fname"].value;   `

` if (x == null || x == "") {        `

`alert("需要输入名字。");        return false; //阻止2表单提交  }`

` }`

### html表单自动验证

HTML 表单验证也可以通过浏览器来自动完成。

如果表单字段 (fname) 的值为空, **required** 属性会阻止表单提交：

`<form action="" method="post>"`

`<input type="text" name="firstname" required="required">`

`<input type="submit" value="提交">`

`</form>`

### 数据验证

**服务端数据验证**是在数据提交到服务器上后再验证。

**客户端数据验证**是在数据发送到服务器前，在浏览器上完成验证。

### html约束验证

[JavaScript 表单 | 菜鸟教程 (runoob.com)](https://www.runoob.com/js/js-validation.html)

## js验证API

### input   max,min属性

`<input id="demo" type="number" min="100" max="300">`

### 约束验证DOM方法

- checkValidity()       如果input元素合法,返回true

- setCustomValidity()        

	设置 input 元素的 validationMessage 属性，用于自定义错误提示信息的方法。

	使用 setCustomValidity 设置了自定义提示后，validity.customError 就会变成 true，checkValidity 总是会返回 false。如果要重新判断需要取消自定义提示，方式如下：

	```
	setCustomValidity('') 
	setCustomValidity(null) 
	setCustomValidity(undefined)
	```

### 约束验证DOM属性

- validity		                        布尔属性值，返回 input 输入值是否合法
- validationMessage			浏览器错误提示信息
- willValidate			             指定 input 是否需要验证

### Validity属性

| customError     | 设置为 true, 如果设置了自定义的 validity 信息。            |
| --------------- | ---------------------------------------------------------- |
| patternMismatch | 设置为 true, 如果元素的值不匹配它的模式属性。              |
| rangeOverflow   | 设置为 true, 如果元素的值大于设置的最大值。                |
| rangeUnderflow  | 设置为 true, 如果元素的值小于它的最小值。                  |
| stepMismatch    | 设置为 true, 如果元素的值不是按照规定的 step 属性设置。    |
| tooLong         | 设置为 true, 如果元素的值超过了 maxLength 属性设置的长度。 |
| typeMismatch    | 设置为 true, 如果元素的值不是预期相匹配的类型。            |
| valueMissing    | 设置为 true，如果元素 (required 属性) 没有值。             |
| valid           | 设置为 true，如果元素的值是合法的。                        |

## js保留关键字

### js保留关键字

|          |           |            |           |              |
| -------- | --------- | ---------- | --------- | ------------ |
| abstract | arguments | boolean    | break     | byte         |
| case     | catch     | char       | class*    | const        |
| continue | debugger  | default    | delete    | do           |
| double   | else      | enum*      | eval      | export*      |
| extends* | false     | final      | finally   | float        |
| for      | function  | goto       | if        | implements   |
| import*  | in        | instanceof | int       | interface    |
| let      | long      | native     | new       | null         |
| package  | private   | protected  | public    | return       |
| short    | static    | super*     | switch    | synchronized |
| this     | throw     | throws     | transient | true         |
| try      | typeof    | var        | void      | volatile     |
| while    | with      | yield      |           |              |

### js对象,属性和方法

| Array     | Date     | eval     | function      | hasOwnProperty |
| --------- | -------- | -------- | ------------- | -------------- |
| Infinity  | isFinite | isNaN    | isPrototypeOf | length         |
| Math      | NaN      | name     | Number        | Object         |
| prototype | String   | toString | undefined     | valueOf        |

### Windows保留关键字

| alert          | all                | anchor      | anchors            | area               |
| -------------- | ------------------ | ----------- | ------------------ | ------------------ |
| assign         | blur               | button      | checkbox           | clearInterval      |
| clearTimeout   | clientInformation  | close       | closed             | confirm            |
| constructor    | crypto             | decodeURI   | decodeURIComponent | defaultStatus      |
| document       | element            | elements    | embed              | embeds             |
| encodeURI      | encodeURIComponent | escape      | event              | fileUpload         |
| focus          | form               | forms       | frame              | innerHeight        |
| innerWidth     | layer              | layers      | link               | location           |
| mimeTypes      | navigate           | navigator   | frames             | frameRate          |
| hidden         | history            | image       | images             | offscreenBuffering |
| open           | opener             | option      | outerHeight        | outerWidth         |
| packages       | pageXOffset        | pageYOffset | parent             | parseFloat         |
| parseInt       | password           | pkcs11      | plugin             | prompt             |
| propertyIsEnum | radio              | reset       | screenX            | screenY            |
| scroll         | secure             | select      | self               | setInterval        |
| setTimeout     | status             | submit      | taint              | text               |
| textarea       | top                | unescape    | untaint            | window             |

### HTML事件句柄

| onblur    | onclick    | onerror     | onfocus     |
| --------- | ---------- | ----------- | ----------- |
| onkeydown | onkeypress | onkeyup     | onmouseover |
| onload    | onmouseup  | onmousedown | onsubmit    |

## js this关键字

在js中this不是固定不变的,会随着执行环境而改变

- 在方法中，this 表示该方法所属的对象。
- 如果单独使用，this 表示全局对象。
- 在函数中，this 表示全局对象。
- 在函数中，在严格模式下，this 是未定义的(undefined)。
- 在事件中，this 表示接收事件的元素。
- 类似 call() 和 apply() 方法可以将 this 引用到任何对象。

### 事件中的 this

在 HTML 事件句柄中，this 指向了接收事件的 HTML 元素：

`<button onclick="this.style.display='none'">点我后我就消失了 </button>`

上述**this**指向`<button>`元素

## js let 和 const

let 声明的变量只在 let 命令所在的代码块内有效。

const 声明一个只读的常量，一旦声明，常量的值就不能改变。

### 全局变量

在函数外声明的变量作用域是全局的

### 局部变量

在函数内声明的变量作用域是局部的(函数内)

**注意**:函数内使用 var 声明的变量只能在函数内访问，如果不使用 var 则是全局变量。

### 重新定义变量

`var x = 10; // 这里输出 x 为 10`

`{    `

`         var x = 2;    // 这里输出 x 为 2 `

`} `

`// 这里输出 x 为 2`



`var x = 10; // 这里输出 x 为 10 `

`{     `

`let x = 2;    // 这里输出 x 为 2 `

`}`

` // 这里输出 x 为 10`

### 循环作用域

`var i = 5; `

`for (var i = 0; i < 10; i++)`

`{   `

` // 一些代码....`

`}`

`// 这里输出 i 为 10`



`var i = 5;`

`for (let i = 0; i < 10; i++)`

`{    `

`// 一些代码... `

`} `

`// 这里输出 i 为 5`

### 局部变量

在函数体内使用 **var** 和 **let** 关键字声明的变量有点类似。

它们的作用域都是 **局部的**:

### 全局变量

在函数体外或代码块外使用 **var** 和 **let** 关键字声明的变量也有点类似。

它们的作用域都是 **全局的**:

### const关键字

const 用于声明一个或多个常量，声明时必须进行初始化，且初始化后值不可再修改(**只读**)

**注意**:**const**并非定义真正的常量,可以修改常量数组,但不能重新赋值

**const** 关键字在不同作用域，或不同块级作用域中是可以重新声明赋值的:

## js JSON

### 什么是 JSON?

- JSON 英文全称 **J**ava**S**cript **O**bject **N**otation
- JSON 是一种轻量级的数据交换格式。
- JSON是独立的语言 *****
- JSON 易于理解。

### JSON实例

{"sites":[    {"name":"Runoob", "url":"www.runoob.com"},     {"name":"Google", "url":"www.google.com"},    {"name":"Taobao", "url":"www.taobao.com"} ]}

### JSON 字符串转换为 JavaScript 对象

首先，创建 JavaScript 字符串，字符串为 JSON 格式的数据：

`var text = '{ "sites" : [' + `

`'{ "name":"Runoob" , "url":"www.runoob.com" },' + `

`'{ "name":"Google" , "url":"www.google.com" },' + `

`'{ "name":"Taobao" , "url":"www.taobao.com" } ]}';`

然后，使用 JavaScript 内置函数 JSON.parse() 将字符串转换为 JavaScript 对象:

`var obj = JSON.parse(text);`

最后，在你的页面中使用新的 JavaScript 对象：

`document.getElementById("demo").innerHTML = obj.sites[1].name + " " + obj.sites[1].url;`

## javascript:void(0) 含义

下面的代码创建了一个超级链接，当用户点击以后不会发生任何事。

`<a href="javascript:void(0)">单击此处什么也不会发生</a>`

当用户链接时，void(0) 计算为 0，但 Javascript 上没有任何效果。

以下实例中，在用户点击链接后显示警告信息：

`<a href="javascript:void(alert('Warning!!!'))">点我!</a>`

以下实例中参数 a 将返回 undefined :

`a = void ( b = 5, c = 7 );`

### href="#"与href="javascript:void(0)"的区别

**#** 包含了一个位置信息，默认的锚是**#top** 也就是网页的上端。

而javascript:void(0), 仅仅表示一个死链接。

在页面很长的时候会使用 **#** 来定位页面的具体位置，格式为：**# + id**。

## js异步编程

### 异步的概念

异步（Asynchronous, async）是与同步（Synchronous, sync）相对的概念。

在我们学习的传统单线程编程中，程序的运行是同步的（同步不意味着所有步骤同时运行，而是指步骤在一个控制流序列中按顺序执行）。而异步的概念则是不保证同步的概念，也就是说，一个异步过程的执行将不再与原有的序列有顺序关系。

简单来理解就是：同步按你的代码顺序执行，异步不按照代码顺序执行，异步的执行效率更高。

以上是关于异步的概念的解释，接下来我们通俗地解释一下异步：异步就是从主线程发射一个子线程来完成任务。

### 什么时候用异步编程

在前端编程中（甚至后端有时也是这样），我们在处理一些简短、快速的操作时，例如计算 1 + 1 的结果，往往在主线程中就可以完成。主线程作为一个线程，不能够同时接受多方面的请求。所以，当一个事件没有结束时，界面将无法处理其他请求。

现在有一个按钮，如果我们设置它的 onclick 事件为一个死循环，那么当这个按钮按下，整个网页将失去响应。

为了避免这种情况的发生，我们常常用子线程来完成一些可能消耗时间足够长以至于被用户察觉的事情，比如读取一个大文件或者发出一个网络请求。因为子线程独立于主线程，所以即使出现阻塞也不会影响主线程的运行。但是子线程有一个局限：一旦发射了以后就会与主线程失去同步，我们无法确定它的结束，如果结束之后需要处理一些事情，比如处理来自服务器的信息，我们是无法将它合并到主线程中去的。

为了解决这个问题，JavaScript 中的异步操作函数往往通过回调函数来实现异步任务的结果处理。

### 回调函数

回调函数就是一个函数，它是在我们启动一个异步任务的时候就告诉它：等你完成了这个任务之后要干什么。这样一来主线程几乎不用关心异步任务的状态了，他自己会善始善终。

`function print() {    document.getElementById("demo").innerHTML="RUNOOB!"; } `

`setTimeout(print, 3000);`

这段程序中的 setTimeout 就是一个消耗时间较长（3 秒）的过程，它的第一个参数是个回调函数，第二个参数是毫秒数，这个函数执行之后会产生一个子线程，子线程会等待 3 秒，然后执行回调函数 "print"，在命令行输出 "RUNOOB!"。

当然，JavaScript 语法十分友好，我们不必单独定义一个函数 print ，我们常常将上面的程序写成：

`setTimeout(function () {    document.getElementById("demo").innerHTML="RUNOOB!"; }, 3000);`

### 异步 AJAX





## js Promise

**函数瀑布**:

setTimeout(function () {    

​	 console.log("First");   

 	setTimeout(function () {        

​		    console.log("Second");        

​		    setTimeout(function () {            

​		 	     console.log("Third");        

​		  }, 3000);   

​     }, 4000); 

}, 1000);

**Promise**:

new Promise(function (resolve, reject) {    setTimeout(function () {        console.log("First");        resolve();    }, 1000); }).then(function () {    return new Promise(function (resolve, reject) {        setTimeout(function () {            console.log("Second");            resolve();        }, 4000);    }); }).then(function () {    setTimeout(function () {        console.log("Third");    }, 3000); });

### Promise的构造函数

Promise 构造函数接受一个函数作为参数，该函数是同步的并且会被立即执行，所以我们称之为起始函数。起始函数包含两个参数 resolve 和 reject，分别表示 Promise 成功和失败的状态。

起始函数执行成功时，它应该调用 resolve 函数并传递成功的结果。当起始函数执行失败时，它应该调用 reject 函数并传递失败的原因。

Promise 构造函数返回一个 Promise 对象，该对象具有以下几个方法：

- then：用于处理 Promise 成功状态的回调函数。
- catch：用于处理 Promise 失败状态的回调函数。
- finally：无论 Promise 是成功还是失败，都会执行的回调函数。

下面是一个使用 Promise 构造函数创建 Promise 对象的例子：

当 Promise 被构造时，起始函数会被同步执行：

const promise = new Promise((resolve, reject) => {  // 异步操作  setTimeout(() => {    if (Math.random() < 0.5) {      resolve('success');    } else {      reject('error');    }  }, 1000); });  promise.then(result => {  console.log(result); }).catch(error => {  console.log(error); });



Promise 类有 .then() .catch() 和 .finally() 三个方法，这三个方法的参数都是一个函数，.then() 可以将参数中的函数添加到当前 Promise 的正常执行序列，.catch() 则是设定 Promise 的异常处理序列，.finally() 是在 Promise 执行的最后一定会执行的序列。 .then() 传入的函数会按顺序依次执行，有任何异常都会直接跳到 catch 序列：

new Promise(function (resolve, reject) {    console.log(1111);    resolve(2222); }).then(function (value) {    console.log(value);    return 3333; }).then(function (value) {    console.log(value);    throw "An error"; }).catch(function (err) {    console.log(err); });

**执行结果**:

1111

2222

3333

An error

详情见:[JavaScript Promise | 菜鸟教程 (runoob.com)](https://www.runoob.com/js/js-promise.html)

## js 规范

### 命名规则

一般很多代码语言的命名规则都是类似的，例如:

- 变量和函数为小驼峰法标识, 即除第一个单词之外，其他单词首字母大写（ **lowerCamelCase**）
- 全局变量为大写 (**UPPERCASE** )
- 常量 (如 PI) 为大写 (**UPPERCASE** )