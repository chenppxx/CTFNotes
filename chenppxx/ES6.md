# let和const

## let

**代码块内有效**

let 是在代码块内有效，var 是在全局范围内有效:

```
let a = 1;
let a = 2;
var b = 3;
var b = 4;
a  // Identifier 'a' has already been declared
b  // 4
```

**不能重复声明**

let 只能声明一次 var 可以声明多次:

```
let a = 1;
let a = 2;
var b = 3;
var b = 4;
a  // Identifier 'a' has already been declared
b  // 4
```

for 循环计数器很适合用 let

```
for (var i = 0; i < 10; i++) {
  setTimeout(function(){
    console.log(i);
  })
}
// 输出十个 10
for (let j = 0; j < 10; j++) {
  setTimeout(function(){
    console.log(j);
  })
}
// 输出 0123456789
```

**不存在变量提升**

let 不存在变量提升，var 会变量提升:

## const

const 声明一个只读变量，声明之后不允许改变。意味着，一旦声明必须初始化，否则会报错。

基本用法:

```
const PI = "3.1415926";
PI  // 3.1415926

const MY_AGE;  // SyntaxError: Missing initializer in const declaration
```

暂时性死区:

```
var PI = "a";
if(true){
  console.log(PI);  // Cannot access 'PI' before initialization
  const PI = "3.1415926";
}
```

ES6 明确规定，代码块内如果存在 let 或者 const，代码块会对这些命令声明的变量从块的开始就形成一个封闭作用域。代码块内，在声明变量 PI 之前使用它会报错。

### 注意要点

const 如何做到变量在声明初始化之后不允许改变的？其实 const 其实保证的不是变量的值不变，而是保证变量指向的内存地址所保存的数据不允许改动。此时，你可能已经想到，简单类型和复合类型保存值的方式是不同的。是的，对于简单类型（数值 number、字符串 string 、布尔值 boolean）,值就保存在变量指向的那个内存地址，因此 const 声明的简单类型变量等同于常量。而复杂类型（对象 object，数组 array，函数 function），变量指向的内存地址其实是保存了一个指向实际数据的指针，所以 const 只能保证指针是固定的，至于指针指向的数据结构变不变就无法控制了，所以使用 const 声明复杂类型对象时要慎重。



# ES6 解构赋值

## 概述

解构赋值是对赋值运算符的扩展。

他是一种针对数组或者对象进行模式匹配，然后对其中的变量进行赋值。

在代码书写上简洁且易读，语义更加清晰明了；也方便了复杂对象中数据字段获取。

## 解构模型

在解构中，有下面两部分参与：

解构的源，解构赋值表达式的右边部分。解构的目标，解构赋值表达式的左边部分。

## 数组模型的解构（Array）

**基本**

```
let [a, [[b], c]] = [1, [[2], 3]];
// a = 1
// b = 2
// c = 3
```

**可嵌套**

```
let [a, [[b], c]] = [1, [[2], 3]]; // a = 1 // b = 2 // c = 3
```

**可忽略**

```
let [a, , b] = [1, 2, 3]; // a = 1 // b = 3
```

**不完全解构**

```
let [a = 1, b] = []; // a = 1, b = undefined
```

**剩余运算符**

```
let [a, ...b] = [1, 2, 3]; //a = 1 //b = [2, 3]
```

**字符串等**

在数组的解构中，解构的目标若为可遍历对象，皆可进行解构赋值。可遍历对象即实现 Iterator 接口的数据。

let [a, b, c, d, e] = 'hello'; // a = 'h' // b = 'e' // c = 'l' // d = 'l' // e = 'o'

**解构默认值**

let [a = 2] = [undefined]; // a = 2

当解构模式有匹配结果，且匹配结果是 undefined 时，会触发默认值作为返回结果。

let [a = 3, b = a] = [];     // a = 3, b = 3 let [a = 3, b = a] = [1];    // a = 1, b = 1 let [a = 3, b = a] = [1, 2]; // a = 1, b = 2

- a 与 b 匹配结果为 undefined ，触发默认值：**a = 3; b = a =3**
- a 正常解构赋值，匹配结果：a = 1，b 匹配结果 undefined ，触发默认值：**b = a =1**
- a 与 b 正常解构赋值，匹配结果：**a = 1，b = 2**

## 对象模型的解构（Object）

**基本**

```
let { foo, bar } = { foo: 'aaa', bar: 'bbb' }; // foo = 'aaa' // bar = 'bbb'  
let { baz : foo } = { baz : 'ddd' }; // foo = 'ddd'
```

**可嵌套可忽略**

```
let obj = {p: ['hello', {y: 'world'}] }; 
let {p: [x, { y }] } = obj; // x = 'hello' // y = 'world' 
let obj = {p: ['hello', {y: 'world'}] }; 
let {p: [x, {  }] } = obj; // x = 'hello'
```

**不完全解构**

```
let obj = {p: [{y: 'world'}] }; 
let {p: [{ y }, x ] } = obj; // x = undefined // y = 'world'
```

**剩余运算符**

```
let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40}; // a = 10 // b = 20 // rest = {c: 30, d: 40}
```

**解构默认值**

```
let {a = 10, b = 5} = {a: 3}; // a = 3; b = 5; 
let {a: aa = 10, b: bb = 5} = {a: 3}; // aa = 3; bb = 5;
```



# Symbol

## 概述

ES6 引入了一种新的原始数据类型 Symbol ，表示独一无二的值，最大的用法是用来定义对象的唯一属性名。

ES6 数据类型除了 Number 、 String 、 Boolean 、 Object、 null 和 undefined ，还新增了 Symbol 。

------

## 基本用法

Symbol 函数栈不能用 new 命令，因为 Symbol 是原始数据类型，不是对象。可以接受一个字符串作为参数，为新创建的 Symbol 提供描述，用来显示在控制台或者作为字符串的时候使用，便于区分。

```
let sy = Symbol("KK");
console.log(sy);   // Symbol(KK)
typeof(sy);        // "symbol"
 
// 相同参数 Symbol() 返回的值不相等
let sy1 = Symbol("kk"); 
sy === sy1;       // false
```

## 使用场景

### 作为属性名

**用法**

由于每一个 Symbol 的值都是不相等的，所以 Symbol 作为对象的属性名，可以保证属性不重名。

```
let sy = Symbol("key1");
 
// 写法1
let syObject = {};
syObject[sy] = "kk";
console.log(syObject);    // {Symbol(key1): "kk"}
 
// 写法2
let syObject = {
  [sy]: "kk"
};
console.log(syObject);    // {Symbol(key1): "kk"}
 
// 写法3
let syObject = {};
Object.defineProperty(syObject, sy, {value: "kk"});
console.log(syObject);   // {Symbol(key1): "kk"}
```

Symbol 作为对象属性名时不能用.运算符，要用方括号。因为.运算符后面是字符串，所以取到的是字符串 sy 属性，而不是 Symbol 值 sy 属性。

```
let syObject = {};
syObject[sy] = "kk";
 
syObject[sy];  // "kk"
syObject.sy;   // undefined
```

### 注意点

Symbol 值作为属性名时，该属性是公有属性不是私有属性，可以在类的外部访问。但是不会出现在 for...in 、 for...of 的循环中，也不会被 Object.keys() 、 Object.getOwnPropertyNames() 返回。如果要读取到一个对象的 Symbol 属性，可以通过 Object.getOwnPropertySymbols() 和 Reflect.ownKeys() 取到。

```
let syObject = {};
syObject[sy] = "kk";
console.log(syObject);
 
for (let i in syObject) {
  console.log(i);
}    // 无输出
 
Object.keys(syObject);                     // []
Object.getOwnPropertySymbols(syObject);    // [Symbol(key1)]
Reflect.ownKeys(syObject);                 // [Symbol(key1)]
```



# ES6 Map 与 Set

## Map 对象

Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。

## Maps 和 Objects 的区别

- 一个 Object 的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。
- Map 中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
- Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
- Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

### Map 中的 key

**key 是字符串**

```
var myMap = new Map();
var keyString = "a string"; 
 
myMap.set(keyString, "和键'a string'关联的值");
 
myMap.get(keyString);    // "和键'a string'关联的值"
myMap.get("a string");   // "和键'a string'关联的值"
                         // 因为 keyString === 'a string'
```

**key 是对象**

```
var myMap = new Map();
var keyObj = {}, 
 
myMap.set(keyObj, "和键 keyObj 关联的值");
﻿
myMap.get(keyObj); // "和键 keyObj 关联的值"
myMap.get({}); // undefined, 因为 keyObj !== {}
```

**key 是函数**

```
var myMap = new Map();
var keyFunc = function () {}, // 函数

myMap.set(keyFunc, "和键 keyFunc 关联的值");

myMap.get(keyFunc); // "和键 keyFunc 关联的值"
myMap.get(function() {}) // undefined, 因为 keyFunc !== function () {}
```

**key 是 NaN**

```
var myMap = new Map();
myMap.set(NaN, "not a number");
 
myMap.get(NaN); // "not a number"
 
var otherNaN = Number("foo");
myMap.get(otherNaN); // "not a number"
```

虽然 NaN 和任何值甚至和自己都不相等(NaN !== NaN 返回true)，NaN作为Map的键来说是没有区别的。

