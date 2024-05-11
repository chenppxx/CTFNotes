# 目录结构

```
├── build                      // 构建相关  
├── config                     // 配置相关
├── src                        // 源代码
│   ├── api                    // 所有请求
│   ├── assets                 // 主题 字体等静态资源
│   ├── components             // 全局公用组件
│   ├── directive              // 全局指令
│   ├── filtres                // 全局 filter
│   ├── icons                  // 项目所有 svg icons
│   ├── lang                   // 国际化 language
│   ├── mock                   // 项目mock 模拟数据
│   ├── router                 // 路由
│   ├── store                  // 全局 store管理
│   ├── styles                 // 全局样式
│   ├── utils                  // 全局公用方法
│   ├── vendor                 // 公用vendor
│   ├── views                   // view
│   ├── App.vue                // 入口页面
│   ├── main.js                // 入口 加载组件 初始化等
│   └── permission.js          // 权限管理
├── static                     // 第三方不打包资源
│   └── Tinymce                // 富文本
├── .babelrc                   // babel-loader 配置
├── eslintrc.js                // eslint 配置项
├── .gitignore                 // git 忽略项
├── favicon.ico                // favicon图标
├── index.html                 // html模板
└── package.json               // package.json

```







# build文件夹下的index.js文件

1. **引入依赖**：脚本首先引入了`runjs`、`chalk`和`vue.config.js`三个模块。
	- `runjs`：一个Node.js工具，用于运行Shell命令。
	- `chalk`：一个用于在终端中添加颜色的库。
	- `vue.config.js`：Vue应用的配置文件。

JavaScript代码中的 `const { run } = require('runjs')` 这行代码执行了如下操作：

1. **调用`require`函数**：`require`是Node.js中用来加载模块的函数。这里它被用来加载`runjs`模块。
2. **解构赋值**：使用JavaScript的解构赋值语法，从`runjs`模块中提取并赋值`run`方法到一个同名的局部变量中。

具体来说：

- `require('runjs')`：这会加载`runjs`模块，该模块是一个Node.js包，用于在Node.js环境中执行Shell命令。
- `const { run }`：这是一个ES6（ECMAScript 2015）引入的解构赋值语法。它允许你从一个对象中提取多个属性并分别赋值给新的变量。在这个例子中，它提取了`runjs`模块导出的`run`属性，并创建了一个同名的局部变量`run`。

最终，这行代码的结果是，你得到了一个可以直接调用的`run`函数，它允许你在Node.js脚本中执行Shell命令。例如，`run('echo "Hello World"')`将会在终端中输出"Hello World"。



在JavaScript代码中，`const chalk = require('chalk')` 这行代码执行了以下操作：

1. **调用 `require` 函数**：`require` 是Node.js环境中用于加载模块的函数。当你调用 `require('chalk')` 时，它会在Node.js的模块查找路径中查找名为 `chalk` 的模块并加载它。
2. **引入 `chalk` 模块**：`chalk` 是一个流行的Node.js库，它允许你在终端中输出彩色文本。这个库提供了多种方法来改变文本的颜色和样式，例如加粗、斜体、下划线等。
3. **创建 `chalk` 变量**：通过 `const chalk` 的声明，你创建了一个名为 `chalk` 的常量，它指向被加载的 `chalk` 模块。这意味着在你的代码中，你可以通过 `chalk` 来访问该模块提供的所有功能。

使用 `chalk` 的一个简单示例是改变文本颜色：

```
console.log(chalk.blue('这段文本是蓝色的'));
console.log(chalk.bold.underline.red('这段文本是红色的，加粗并带有下划线'));
```



# 解构路由属性

**解构路由属性**：`const { redirect, path } = item` 这行代码从传入的 `item` 对象中解构出 `redirect` 和 `path` 属性。`item` 代表面包屑导航中的一个项，通常是一个路由对象。



# 相关代码解释

- `this.$router.push(redirect)`：调用 Vue Router 实例的 `push` 方法进行路由跳转。`redirect` 可以是一个字符串，表示目标路由的路径；也可以是一个对象，表示一个路由配置，包含 `name` 或 `path` 等属性。

- `$router`：是Vue Router在Vue组件实例上挂载的一个属性，它是一个Router实例的引用。Vue Router是一个官方的路由管理器，用于构建单页面应用程序（SPA）。通过 `$router`，你可以访问路由状态、进行路由跳转等操作。

- `<el-breadcrumb class="app-breadcrumb" separator="/">`:

	`class="app-breadcrumb"`为面包屑导航添加自定义类名。

	`separator="/"`定义了面包屑导航中各个部分之间的分隔符。

- `const { params } = this.$route`

	是一个使用了ES6对象字面量属性值缩写和解构赋值的语法结构。这个表达式用于从`this.$route`对象中提取`params`属性，并将这个属性的值赋给新的常量`params`。

- ```
	<style lang="scss" scoped>是一个用于定义组件样式的标签，它有以下几个含义：
	<style>：这是HTML中定义CSS样式的标准标签。
	
	lang="scss"：这个属性指定了样式表使用的是SCSS语法。SCSS是Sass的语法的一个变体，它使用花括号来包裹CSS规则，并且支持变量、混合（mixins）、函数等CSS预处理器特性。lang是language的缩写，告诉Vue这个<style>块中使用的是哪种语言的语法。
	
	scoped：这个属性表示样式的作用域被限定在当前组件内部。带有scoped属性的CSS样式只会应用到定义它们的组件及其子组件，而不会渗透到其他组件中。这有助于避免不同组件间的样式冲突。
	
	结合在一起，<style lang="scss" scoped>意味着：
	
	这是一个使用SCSS语法的CSS样式块。
	这些样式仅应用于定义它们的Vue组件，不会影响全局样式。
	```

- ```
	methods: {
	
	  toggleClick() {
	
	   this.$emit('toggleClick')
	
	  }
	
	 }
	 methods：定义了组件的方法：
	toggleClick：当用户点击汉堡菜单图标时触发，通过this.$emit('toggleClick')向父组件发送一个自定义事件toggleClick，通知父组件切换菜单的状态。
	```

- ```
	computed: {
	    isExternal() {
	      return isExternal(this.iconClass)
	    },
	    iconName() {
	      return `#icon-${this.iconClass}`
	    },
	    svgClass() {
	      if (this.className) {
	        return 'svg-icon ' + this.className
	      } else {
	        return 'svg-icon'
	      }
	    },
	    styleExternalIcon() {
	      return {
	        mask: `url(${this.iconClass}) no-repeat 50% 50%`,
	        '-webkit-mask': `url(${this.iconClass}) no-repeat 50% 50%`
	      }
	    }
	  }
	在Vue组件中，computed属性用于创建“计算属性”，这些属性会根据它们的响应式依赖自动重新计算。在SvgIcon组件中，computed属性被用来根据组件的props动态生成样式和类名。下面是对这些计算属性的详细分析：
	
	isExternal
	isExternal() {
	  return isExternal(this.iconClass);
	}
	这个计算属性调用了一个名为isExternal的函数（这个函数是从外部导入的），传入当前组件的iconClass prop。如果iconClass是一个外部链接，这个函数返回true，否则返回false。这个属性用于决定是使用内嵌的SVG图标还是使用外部链接的SVG图标。
	
	iconName
	iconName() {
	  return `#icon-${this.iconClass}`;
	}
	这个计算属性简单地返回一个字符串，它由一个固定前缀#icon-和iconClass prop的值拼接而成。这个字符串将用作SVG use元素的xlink:href属性值，以便引用内嵌SVG的特定部分。
	
	svgClass
	svgClass() {
	  if (this.className) {
	    return 'svg-icon ' + this.className;
	  } else {
	    return 'svg-icon';
	  }
	}
	这个计算属性根据组件的className prop来返回一个类名字符串。如果className prop被提供了，那么返回的类名将包括'svg-icon'和用户定义的类名；如果没有提供className，则只返回'svg-icon'。这个类名将被应用到SVG元素上，以便根据类名应用相应的CSS样式。
	
	styleExternalIcon
	styleExternalIcon() {
	  return {
	    mask: `url(${this.iconClass}) no-repeat 50% 50%`,
	    '-webkit-mask': `url(${this.iconClass}) no-repeat 50% 50%`
	  };
	}
	这个计算属性返回一个对象，包含CSS样式规则。如果iconClass是一个外部链接，这些样式将被应用到一个div元素上，以显示外部SVG图标。这里使用了CSS的mask属性来将SVG图标设置为元素的掩膜，并通过url函数引用外部SVG图标的URL。no-repeat 50% 50%指定了图标不重复，并且位于掩膜区域的中心。此外，还提供了一个针对Webkit浏览器的前缀样式，以确保更好的浏览器兼容性。
	
	这些计算属性结合起来，提供了一种灵活的方式来根据组件的props动态决定如何显示SVG图标，无论是内嵌的还是外部链接的。
	```

	









## export dafault关键字

在JavaScript和Vue.js中，`export default`是一个关键字，用于从模块中导出一个默认的值。当你使用`export default`导出一个值时，你可以在另一个模块中通过任意名称来导入这个值。

以下是`export default`的一些关键点：

1. **默认导出**：一个模块只能有一个默认导出。默认导出可以是任何类型的值，包括函数、对象、类或原始值。
2. **导入方式**：默认导出的值在使用`import`语句导入时，可以被赋予任意名称。
3. **与命名导出的区别**：除了默认导出，模块还可以使用`export`（非`export default`）进行命名导出。命名导出可以有多个，并且导入时必须使用原名称。

### 示例

假设有一个模块`math.js`，它导出了一个名为`add`的函数：

```
// math.js
function add(a, b) {
  return a + b;
}

export default add;
```

在另一个模块中，你可以这样导入`add`函数：

```
// app.js
import myAddFunction from './math.js';

console.log(myAddFunction(1, 2)); // 输出 3
```

在这个例子中，尽管`add`函数在`math.js`中被导出为默认值，但在导入时，我们给它赋予了一个新的名称`myAddFunction`。

在Vue.js中，`export default`通常用于导出Vue组件：

```
// MyComponent.vue
<template>
  <div>{{ message }}</div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello, World!'
    };
  }
}
</script>
```

这样，你就可以在其他Vue组件或应用中导入并使用`MyComponent`了。

使用`export default`是模块化编程的一种常见做法，它有助于代码的重用和组织。