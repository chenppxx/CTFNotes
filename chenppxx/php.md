# php



用xampp搭建的php环境时,访问一个php文件可以在浏览器地址栏输入

http://localhost:(端口号)/test.php

**注意**:请记住您的站点根目录为 xampp 目录下的 htdocs 文件夹。



**PHP_EOL** 为换行符。



## php伪协议

### 适用条件

文件包含！！！的时候，可能遇到的文件包含函数：
1、include
2、require
3、include_once
4、require_once
5、highlight_file
6、show_source
7、flie
8、readfile
9、file_get_contents
10、file_put_contents
11、fopen (比较常见)

php支持的伪协议:

1 file:// — 访问本地文件系统
2 http:// — 访问 HTTP(s) 网址
3 ftp:// — 访问 FTP(s) URLs
4 php:// — 访问各个输入/输出流（I/O streams）
5 zlib:// — 压缩流
6 data:// — 数据（RFC 2397）
7 glob:// — 查找匹配的文件路径模式
8 phar:// — PHP 归档
9 ssh2:// — Secure Shell 2
10 rar:// — RAR
11 ogg:// — 音频流
12 expect:// — 处理交互式的流

### php://filter

**php://filter**可以获取指定文件源码。当它与包含函数结合时，php://filter流会被当作php文件执行。所以我们一般对其进行编码，让其不执行。从而导致 任意文件读取。

**协议参数**:

名称	   描述
resource=<要过滤的数据流>	这个参数是必须的。它指定了你要筛选过滤的数据流。
read=<读链的筛选列表>	该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。
write=<写链的筛选列表>	该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。
<；两个链的筛选列表>	任何没有以 read= 或 write= 作前缀 的筛选器列表会视情况应用于读或写链。
**常用**:

php://filter/read=convert.base64-encode/resource=index.php

php://filter/resource=index.php

利用filter协议读文件±，将index.php通过base64编码后进行输出。这样做的好处就是如果不进行编码，文件包含后就不会有输出结果，而是当做php文件执行了，而通过编码后则可以读取文件源码。

而使用的convert.base64-encode，就是一种过滤器。

#### 过滤器

##### 字符串过滤器

该类通常以`string`开头，对每个字符都进行同样方式的处理。

##### string.rot13

一种字符处理方式，字符右移十三位。

##### string.toupper

将所有字符转换为大写。

##### string.tolower

将所有字符转换为小写。

##### string.strip_tags
这个过滤器就比较有意思，用来处理掉读入的所有标签，例如XML的等等。在绕过死亡exit大有用处。

##### 转换过滤器
对数据流进行编码，通常用来读取文件源码。

convert.base64-encode & convert.base64-decode

base64加密解密

convert.quoted-printable-encode & convert.quoted-printable-decode

可以翻译为可打印字符引用编码，使用可以打印的ASCII编码的字符表示各种编码形式下的字符。

##### 压缩过滤器
注意，这里的压缩过滤器指的并不是在数据流传入的时候对整个数据进行写入文件后压缩文件，也不代表可以压缩或者解压数据流。压缩过滤器不产生命令行工具如 gzip的头和尾信息。只是压缩和解压数据流中的有效载荷部分。

用到的两个相关过滤器：zlib.deflate（压缩）和 zlib.inflate（解压）。zilb是比较主流的用法，至于bzip2.compress和 bzip2.decompress工作的方式与 zlib 过滤器大致相同。

##### 加密过滤器
mcrypt.*和 mdecrypt.*使用 libmcrypt 提供了对称的加密和解密。

### data://

数据流封装器，以传递相应格式的数据。可以让用户来控制输入流，当它与包含函数结合时，用户输入的data://流会被当作php文件执行。

`data:[<mediatype>][;base64],<data>`

其中，`<mediatype>` 表示媒体类型（例如，text/html、image/jpeg等），可选参数；`;base64` 表示数据是否使用 Base64 编码，也是可选参数；`<data>` 是实际的数据内容。

### file://

用于访问本地文件系统，并且不受allow_url_fopen，allow_url_include影响
file://协议主要用于访问文件(绝对路径、相对路径以及网络路径)
比如：`http://www.xx.com?file=file:///etc/passsword`

### php://

在allow_url_fopen，allow_url_include都关闭的情况下可以正常使用
php://作用为访问输入输出流

### php://input

php://input可以访问请求的原始数据的只读流，将post请求的数据当作php代码执行。当传入的参数作为文件名打开时，可以将参数设为php://input,同时post想设置的文件内容，php执行时会将post内容当作文件内容。从而导致任意代码执行。

例如：
`http://127.0.0.1/cmd.php?cmd=php://input`
POST数据：<?php phpinfo()?>
注意：
当enctype="multipart/form-data"的时候 php://input` 是无效的

遇到file_get_contents()要想到用php://input绕过。

### zip://

zip:// 可以访问压缩包里面的文件。当它与包含函数结合时，zip://流会被当作php文件执行。从而实现任意代码执行。





1.empty() 函数用于检查一个变量是否为空。
empty() 判断一个变量是否被认为是空的。当一个变量并不存在，或者它 的值等同于 FALSE，那么它会被认为不存在。如果变量不存在的话，empty()并不会产生警告。
empty() 5.5 版本之后支持表达式了，而不仅仅是变量。

2.mb_substr() 函数返回字符串的一部分，之前我们学过 substr() 函数，它只针对英文字符，如果要分割的中文文字则需要使用 mb_substr()。
注释：如果 start 参数是负数且 length 小于或等于 start，则 length 为 0。

mb_strpos()：返回要查找的字符串在别一个字符串中首次出现的位置(即数字)

3.

.表示当前目录
. .表示当前目录的上一级目录。
. ./表示当前目录下的某个文件或文件夹，视后面跟着的名字而定
./表示当前目录上一级目录的文件或文件夹，视后面跟着的名字而定。
例如：
文件夹 a
下面有 文件夹b c 和文件 d。
文件夹b下面有e.php 和文件f。
则e中的 . 表示 文件夹b
./f 表示b下面的文件f。
. .表示a文件夹。
. ./d 表示a文件夹下的d文件。



## php正则表达式的逆向模式和子模式

正则表达式一个最重要的特性就是将匹配成功的模式的某部分进行存储供以后使用这一能力。 
对一个正则表达式模式或部分模式两边添加圆括号()可以把这部分表达式存储到一个临时缓冲区中。 

所捕获的每个子匹配都按照在正则表达式模式中从左至右所遇到的内容按顺序存储。 
存储子匹配的缓冲区编号从1开始，连续编号至最大99个子表达式。 
每个缓冲区都可以使用'\n'(或用'$n')访问，其中n为1至99的阿拉伯数字，用来按顺序标识特定缓冲区(子表达式)。 

最简单最有用的例子是确定文字中连续出现两个相同单词的位置 (如下):

`<?php 
$string = "Is is the cost of of gasoline going up up"; 
$pattern = "/\b([a-z]+) \\1\b/i"; //这里的\\1不能使用\$1或$1 
$str = preg_replace($pattern, "\\1", $string); //这里的\\1可以使用\$1或$1，引用第一个子匹配 
echo $str; //效果是Is the cost of gasoline going up 
?> 
`

例中的子表达式就是圆括号内的项。\b匹配单词的开始或结束。+匹配重复一次或更多次。 
该子表达式匹配的是一个或多个字母字符的单词，即由'[a-z]+'匹配的。 

该正则表达式的第二部分是对前面所捕获的子匹配的引用，也就是由附加表达式所匹配的第二次出现的单词，用'\\1'来引用第一个子匹配，第一个\是转义符。 



`正则表达式的逆向引用($0-99或\\0-99)和子模式以(/()/)开始。 
这里$0是全部匹配模式的匹配项。$1是第1个子匹配，$2至$99依次是第2个至第99个子匹配。 
用$1-99后向引用子匹配时，如果后面的字符是数字，要用花括号区别开。例：${1}1 。 `

**函数 
**mixed preg_replace ( mixed pattern, mixed replacement, mixed subject [, int limit]) 

功能 
在 subject 中搜索 pattern 模式的匹配项并替换为 replacement。如果指定了 limit，则仅替换 limit 个匹配，如果省略 limit 或者其值为 -1，则所有的匹配项都会被替换。 
replacement可以包含\\n形式或$n形式的逆向引用，n可以为0到99，\\n表示匹配pattern第n个子模式的文本，\\0表示匹配整个pattern的文本。 

**子模式** 
\$pattern参数中被圆括号括起来的正则表达式，子模式的数目即从左到右圆括号的数目。（pattern即模式）

`<?php 
$time=date("Y-m-d H:i:s"); 
$pattern = "/(\d{4})-(\d{2})-(\d{2}) (\d{2}):(\d{2}):(\d{2})/i"; 
$replacement = "\$time格式为：$0<BR>替换后的格式为：$1年$2月$3日 $4时$5分$6秒"; 
print preg_replace($pattern, $replacement, $time); 
?> 
`

`输出： 
$time格式为：2011-07-25 17:59:26 
替换后的格式为：2011年07月25日 17时59分26秒 `





## php简介

### PHP 是什么？

- PHP（全称：PHP：Hypertext Preprocessor，即"PHP：超文本预处理器"）是一种通用开源脚本语言。
- PHP 脚本在服务器上执行。
- PHP 可免费下载使用。

### PHP 文件是什么？

- PHP 文件可包含文本、HTML、JavaScript代码和 PHP 代码
- PHP 代码在服务器上执行，结果以纯 HTML 形式返回给浏览器
- PHP 文件的默认文件扩展名是 **.php**。

### PHP 文件是什么？

- PHP 文件可包含文本、HTML、JavaScript代码和 PHP 代码
- PHP 代码在服务器上执行，结果以纯 HTML 形式返回给浏览器
- PHP 文件的默认文件扩展名是 **.php**。

## 为什么使用 PHP？

- PHP 可在不同的平台上运行（Windows、Linux、Unix、Mac OS X 等）
- PHP 与目前几乎所有的正在被使用的服务器相兼容（Apache、IIS 等）
- PHP 提供了广泛的数据库支持
- PHP 是免费的，可从官方的 PHP 资源下载它：[ www.php.net](http://www.php.net/)
- PHP 易于学习，并可高效地运行在服务器端

## php语法

### 基本的 PHP 语法

PHP 脚本可以放在文档中的任何位置。

PHP 脚本以 **<?php** 开始，以 **?>** 结束：

PHP 文件的默认文件扩展名是 **.php**。

PHP 文件通常包含 HTML 标签和一些 PHP 脚本代码。

下面，我们提供了一个简单的 PHP 文件实例，它可以向浏览器输出文本 "Hello World!"：

`<!DOCTYPE html>`
`<html>`
`<body>`

`<h1>My first PHP page</h1>`

`<?php`
`echo "Hello World!";`
`?>`

`</body>`
`</html>`

PHP 中的每个代码行都必须以分号结束。分号是一种分隔符，用于把指令集区分开来。

通过 PHP，有两种在浏览器输出文本的基础指令：**echo** 和 **print**。

**php注释**:`//`:多行注释        `/*...*/`:多行注释

### php变量

- **变量以 $ 符号开始，后面跟着变量的名称**
- 变量名必须以字母或者下划线字符开始
- 变量名只能包含字母、数字以及下划线（A-z、0-9 和 _ ）
- 变量名不能包含空格
- 变量名是区分大小写的（$y 和 $Y 是两个不同的变量）

### 声明php变量

php 没有声明变量的命令.

变量在第一次赋值给它的时候被创建.

### php变量作用域

- local
- global
- static
- parameter

在 PHP 函数内部声明的变量是局部变量，仅能在函数内部访问.

#### php global 关键字

global 关键字用于函数内访问全局变量。

PHP 将所有全局变量存储在一个名为 $GLOBALS[*index*] 的数组中。 *index* 保存变量的名称。这个数组可以在函数内部访问，也可以直接用来更新全局变量。

<?php $x=5; $y=10;  function myTest() {    $GLOBALS['y']=$GLOBALS['x']+$GLOBALS['y']; }   myTest(); echo $y; ?>

#### static作用域

当一个函数完成时，它的所有变量通常都会被删除。然而，有时候您希望某个局部变量不要被删除。

要做到这一点，请在您第一次声明变量时使用 **static** 关键字：

然后，每次调用该函数时，该变量将会保留着函数前一次被调用时的值。

**注释：**该变量仍然是函数的局部变量。

## echo和print

在 PHP 中有两个基本的输出方式： echo 和 print。

echo 和 print 区别:

- echo - 可以输出一个或多个字符串
- print - 只允许输出一个字符串，返回值总为 1

**提示：**echo 输出的速度比 print 快， echo 没有返回值，print有返回值1。

### echo

echo 是一个语言结构，使用的时候可以不用加括号，也可以加上括号： echo 或 echo()。

使用echo命令可以包含HTML标签:

`echo "<h2>PHP 很有趣!</h2>";`

`echo "这是一个", "字符串，", "使用了", "多个", "参数。";`

`$cars=array("Volvo","BMW","Toyota");`

`echo "我车的品牌是 {$cars[0]}";`

### print

print 同样是一个语言结构，可以使用括号，也可以不使用括号： print 或 print()。

**用法和echo差不多**

## PHP EOF(heredoc) 使用说明

PHP EOF(heredoc)是一种在命令行shell（如sh、csh、ksh、bash、PowerShell和zsh）和程序语言（像Perl、PHP、Python和Ruby）里定义一个字符串的方法。

使用概述：

- \1. 必须后接分号，否则编译通不过。
- \2. **EOF** 可以用任意其它字符代替，只需保证结束标识与开始标识一致。
- **3. 结束标识必须顶格独自占一行(即必须从行首开始，前后不能衔接任何空白和字符)。**
- \4. 开始标识可以不带引号或带单双引号，不带引号与带双引号效果一致，解释内嵌的变量和转义符号，带单引号则不解释内嵌的变量和转义符号。
- \5. 当内容需要内嵌引号（单引号或双引号）时，不需要加转义符，本身对单双引号转义，此处相当与q和qq的用法。

## php数据类型

PHP 变量存储不同的类型的数据，不同的数据类型可以做不一样的事情。

PHP 支持以下几种数据类型:

- String（字符串）
- Integer（整型）
- Float（浮点型）
- Boolean（布尔型）
- Array（数组）
- Object（对象）
- NULL（空值）
- Resource（资源类型）

### php字符串

一个字符串是一串字符的序列，就像 "Hello world!"。

你可以将任何文本放在单引号和双引号中：

### php整型

整数是一个没有小数的数字。

整数规则:

- 整数必须至少有一个数字 (0-9)
- 整数不能包含逗号或空格
- 整数是没有小数点的
- 整数可以是正数或负数
- 整型可以用三种格式来指定：十进制， 十六进制（ 以 0x 为前缀）或八进制（前缀为 0）。

### php浮点型

浮点数是带小数部分的数字，或是指数形式。

### php对象

对象数据类型也可以用于存储数据。

在 PHP 中，对象必须声明。

首先，你必须使用class关键字声明类对象。类是可以包含属性和方法的结构。

然后我们在类中定义数据类型，然后在实例化的类中使用数据类型：

<?php
class Car
{
 ` var $color;`
  function __construct($color="green") {
    $this->color = $color;
  }
  function what_color() {
    return $this->color;
  }
}
?>

function print_vars($obj) {
  foreach (get_object_vars($obj) as $prop => $val) {
   echo "\t$prop = $val\n";
  }
}

`get_object_vars` 函数来获取该对象的属性和对应的值

### php null值

NULL 值表示变量没有值。NULL 是数据类型为 NULL 的值。

NULL 值指明一个变量是否为空值。 同样可用于数据空值和NULL值的区别。

可以通过设置变量值为 NULL 来清空变量数据：

### php 资源类型

PHP 资源 resource 是一种特殊变量，保存了到外部资源的一个引用。

常见资源数据类型有打开文件、数据库连接、图形画布区域等。

由于资源类型变量保存有为打开文件、数据库连接、图形画布区域等的特殊句柄，因此将其它类型的值转换为资源没有意义。

使用 **get_resource_type()** 函数可以返回资源（resource）类型：

## php 比较

虽然 PHP 是弱类型语言，但也需要明白变量类型及它们的意义，因为我们经常需要对 PHP 变量进行比较，包含松散和严格比较。

- 松散比较：使用两个等号 **==** 比较，只比较值，不比较类型。
- 严格比较：用三个等号 **===** 比较，除了比较值，也比较类型。

## php 常量

常量是一个简单值的标识符。该值在脚本中不能改变。

一个常量由英文字母、下划线、和数字组成,但数字不能作为首字母出现。 (常量名不需要加 $ 修饰符)。

**注意：** 常量在整个脚本中都可以使用。

### 设置php常量

使用define()函数

`define("GREETING", "欢迎访问 Runoob.com");`          大小写敏感

`define("GREETING", "欢迎访问 Runoob.com", true);`   大小写不敏感

**常量是全局的**

## php字符串

### 并指运算符(.)

在 PHP 中，只有一个字符串运算符。

并置运算符 (.) 用于把两个字符串值连接起来。

下面的实例演示了如何将两个字符串变量连接在一起：

`<?php`
`txt1="Hello world!";`
`txt2="What a nice day!";`
`echo $txt1 . " " . $txt2;`
`?>`

### strllen()返回字符串的长度

### strpos()函数(查找)

strpos() 函数用于在字符串内查找一个字符或一段指定的文本。

如果在字符串中找到匹配，该函数会返回第一个匹配的字符位置。如果未找到匹配，则返回 FALSE。

下面的实例在字符串 "Hello world!" 中查找文本 "world"：

`<?php`
`echo strpos("Hello world!","world");`
`?>`

## php运算符

`/` 是除

**intdiv()**是整除

`<?php var_dump(intdiv(10, 3)); ?>`       输出:int(3)

### php 数组运算符

| 运算符  | 名称   | 描述                                                         |
| :------ | :----- | :----------------------------------------------------------- |
| x + y   | 集合   | x 和 y 的集合                                                |
| x == y  | 相等   | 如果 x 和 y 具有相同的键/值对，则返回 true                   |
| x === y | 恒等   | 如果 x 和 y 具有相同的键/值对，且顺序相同类型相同，则返回 true |
| x != y  | 不相等 | 如果 x 不等于 y，则返回 true                                 |
| x <> y  | 不相等 | 如果 x 不等于 y，则返回 true                                 |
| x !== y | 不恒等 | 如果 x 不等于 y，则返回 true                                 |

`$x = array("a" => "red", "b" => "green"); `
`$y = array("c" => "blue", "d" => "yellow"); `
`$z = $x + $y; // union of $x and $y
var_dump($z);`

输出:array(4) { ["a"]=> string(3) "red" ["b"]=> string(5) "green" ["c"]=> string(4) "blue" ["d"]=> string(6) "yellow" }

### 三元运算符

?:

### 组合比较符

```
$c = $a <=> $b;
```

解析如下：

- 如果 **$a > $b**, 则 **$c** 的值为 **1**。
- 如果 **$a == $b**, 则 **$c** 的值为 **0**。
- 如果 **$a < $b**, 则 **$c** 的值为 **-1**。

### 运算符优先级

| 结合方向 | 运算符                                                   | 附加信息                 |
| :------- | :------------------------------------------------------- | :----------------------- |
| 无       | clone new                                                | clone 和 new             |
| 左       | [                                                        | array()                  |
| 右       | ++ -- ~ (int) (float) (string) (array) (object) (bool) @ | 类型和递增／递减         |
| 无       | instanceof                                               | 类型                     |
| 右       | !                                                        | 逻辑运算符               |
| 左       | * / %                                                    | 算术运算符               |
| 左       | + – .                                                    | 算术运算符和字符串运算符 |
| 左       | << >>                                                    | 位运算符                 |
| 无       | == != === !== <>                                         | 比较运算符               |
| 左       | &                                                        | 位运算符和引用           |
| 左       | ^                                                        | 位运算符                 |
| 左       | \|                                                       | 位运算符                 |
| 左       | &&                                                       | 逻辑运算符               |
| 左       | \|\|                                                     | 逻辑运算符               |
| 左       | ? :                                                      | 三元运算符               |
| 右       | = += -= *= /= .= %= &= \|= ^= <<= >>= =>                 | 赋值运算符               |
| 左       | and                                                      | 逻辑运算符               |
| 左       | xor                                                      | 逻辑运算符               |
| 左       | or                                                       | 逻辑运算符               |
| 左       | ,                                                        | 多处用到                 |

## php数组

### php数值数据

**count()**函数用于返回数组的长度,搭配for循环可以遍历数组元素

### php关联数组

关联数组是使用您分配给数组的指定的键的数组。

这里有两种创建关联数组的方法：

`$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");`

or:

`$age['Peter']="35";`
`$age['Ben']="37";`
`$age['Joe']="43";`

可以使用**foreach()**函数遍历关联数组:

`foreach($age as $x=>$x_value) {`

 `  ` `echo "Key=" . $x . ", Value=" . $x_value;    echo "<br>"; }`

## php 数组排序

数组中的元素可以按字母或数字顺序进行降序或升序排列。

- sort() - 对数组进行升序排列
- rsort() - 对数组进行降序排列
- asort() - 根据关联数组的值，对数组进行升序排列
- ksort() - 根据关联数组的键，对数组进行升序排列
- arsort() - 根据关联数组的值，对数组进行降序排列
- krsort() - 根据关联数组的键，对数组进行降序排列

 

## php 超级全局变量

PHP中预定义了几个超级全局变量（superglobals） ，这意味着它们在一个脚本的全部作用域中都可用。 你不需要特别说明，就可以在函数及类中使用。

PHP 超级全局变量列表:

- $GLOBALS
- $_SERVER
- $_REQUEST
- $_POST
- $_GET
- $_FILES
- $_ENV
- $_COOKIE
- $_SESSION

### PHP $GLOBALS

$GLOBALS 是PHP的一个超级全局变量组，在一个PHP脚本的全部作用域中都可以访问。

$GLOBALS 是一个包含了全部变量的全局组合数组。变量的名字就是数组的键。

以下实例介绍了如何使用超级全局变量 $GLOBALS:

`<?php  $x = 75;  $y = 25;  `

`function addition()  {     $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];  }  addition();  echo $z;  ?>`

### PHP $_SERVER

$_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建。不能保证每个服务器都提供全部项目；服务器可能会忽略一些，或者提供一些没有在这里列举出来的项目。



## php循环

- **while** - 只要指定的条件成立，则循环执行代码块
- **do...while** - 首先执行一次代码块，然后在指定的条件成立时重复这个循环
- **for** - 循环执行代码块指定的次数
- **foreach** - 根据数组中每个元素来循环代码块

### foreach

```
foreach ($array as $key => $value)
{
    要执行代码;
}
```

每一次循环，当前数组元素的键与值就都会被赋值给 $key 和 $value 变量（数字指针会逐一地移动），在进行下一次循环时，你将看到数组中的下一个键与值。

## php函数

大致和c语言相同

### php 变量函数

变量函数是指在 PHP 中，将一个变量作为函数名来调用的函数。

变量函数可以让我们在运行时动态地决定调用哪个函数。

`$func = 'foo';`
`$func();     *// 调用 foo()*`



## php魔术常量

PHP 向它运行的任何脚本提供了大量的预定义常量。

不过很多常量都是由不同的扩展库定义的，只有在加载了这些扩展库时才会出现，或者动态加载后，或者在编译时已经包括进去了。

有八个魔术常量它们的值随着它们在代码中的位置改变而改变。

### `__LINE__`

文件中的当前行号

### `__FILE__`

文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。

### `__DIR_`

文件所在的目录

### `__FUNCTION_`

函数名称

`<?php `

`function test() {`

`echo  '函数名为：' . __FUNCTION__ ; `

`} `

`test(); `

`?>`

### `__CLASS`

类的名称

### `__TRAIT_`

### `__METHOD_`

### `__NAMESPACE_`



## PHP 类定义

```
<?php
class phpClass {
  var $var1;
  var $var2 = "constant string";
  
  function myfunc ($arg1, $arg2) {
     [..]
  }
  [..]
}
?>
```

解析如下：

- 类使用 **class** 关键字后加上类名定义。
- 类名后的一对大括号({})内可以定义变量和方法。
- 类的变量使用 **var** 来声明, 变量也可以初始化值。
- 函数定义类似 PHP 函数的定义，但函数只能通过该类及其实例化的对象访问。

**在实例化对象后,可以使用`->`使用该对象调用成员方法**

### php 构造函数

构造函数是一种特殊的方法。主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，在创建对象的语句中与 **new** 运算符一起使用。

```
function __construct( $par1, $par2 ) {
   $this->url = $par1;
   $this->title = $par2;
}
```

`$runoob = new Site('www.runoob.com', '菜鸟教程');`

可以舒适化$runoob类

### php 析构函数

析构函数(destructor) 与构造函数相反，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。PHP 5 引入了析构函数的概念，这类似于其它面向对象的语言，其语法格式如下：

```
void __destruct ( void )
```

```
<?php
class MyDestructableClass {
   function __construct() {
       print "构造函数\n";
       $this->name = "MyDestructableClass";
   }

   function __destruct() {
       print "销毁 " . $this->name . "\n";
   }
}

$obj = new MyDestructableClass();
?>
```

### extend 继承

PHP 不支持多继承，格式如下：

```
class Child(继承) extends Parent(被继承) {
   // 代码部分
}
```

### 方法重写

如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。

### 访问控制

PHP 对属性或方法的访问控制，是通过在前面添加关键字 public（公有），protected（受保护）或 private（私有）来实现的。

- **public（公有）：**公有的类成员可以在任何地方被访问。
- **protected（受保护）：**受保护的类成员则可以被其自身以及其子类和父类访问。
- **private（私有）：**私有的类成员则只能被其定义所在的类访问。

## PHP 表单

### 获取下拉菜单的数据

`$q = isset($_GET['q'])? htmlspecialchars($_GET['q']) : '';`

`<form action="" method="get">`

`<select name="q">`

`</select>`

表单使用 GET 方式获取数据，action 属性值为空表示提交到当前脚本，我们可以通过 select 的 name 属性获取下拉菜单的值

**当下拉菜单多选时:**可以将name设置为**q[]**

### 单选按钮表单

`<input type="radio" name="q" value="RUNOOB" />Runoob`

### checkbox 复选框

`<input type="checkbox" name="q[]" value="RUNOOB"> Runoob<br>`

## PHP $_REQUEST 变量

预定义的 $_REQUEST 变量包含了 $_GET、$_POST 和 $_COOKIE 的内容。

$_REQUEST 变量可用来收集通过 GET 和 POST 方法发送的表单数据。

## php class中public,private,protected的区别，以及实例

**一，public,private,protected的区别**

public:权限是最大的，可以内部调用，实例调用等。

protected: 受保护类型，用于本类和继承类调用。

private: 私有类型，只有在本类中使用。