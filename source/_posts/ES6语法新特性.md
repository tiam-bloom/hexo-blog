---
title: ES6语法新特性
cover: 'http://qiniu.yujing.fit/typora_img/234249-15756469694661.jpg'
categories:
  - JavaScript
tags:
  - ES6
  - JavaScript
abbrlink: c2850c04
date: 2022-11-22 21:50:05
---

# ES6

参考文章:

> - https://zhuanlan.zhihu.com/p/30400262
> - http://caibaojian.com/es6/class.html
> - https://www.jianshu.com/p/d23a506cdca2

## 查看对es6语法的支持

```js
D:\desktop\Vue> npm install -g es-checker

added 13 packages in 4s
PS D:\desktop\Vue> es-checker

ECMAScript 6 Feature Detection (v1.4.2)

Variables
  √ let and const
  √ TDZ error for too-early access of let or const declarations  
  √ Redefinition of const declarations not allowed
  √ destructuring assignments/declarations for arrays and objects
  √ ... operator

Data Types
  √ For...of loop
  √ Map, Set, WeakMap, WeakSet
  √ Symbol
  √ Symbols cannot be implicitly coerced

Number
  √ Octal (e.g. 0o1 ) and binary (e.g. 0b10 ) literal forms
  √ Old octal literal invalid now (e.g. 01 )
  √ Static functions added to Math (e.g. Math.hypot(), Math.acosh(), Math.imul() )
  √ Static functions added to Number (Number.isNaN(), Number.isInteger() )

String
  √ Methods added to String.prototype (String.prototype.includes(), String.prototype.repeat() )
  √ Unicode code-point escape form in string literals (e.g. \u{20BB7} )
  √ Unicode code-point escape form in identifier names (e.g. var \u{20BB7} = 42; )
  √ Unicode code-point escape form in regular expressions (e.g. var regexp = /\u{20BB7}/u; )
  √ y flag for sticky regular expressions (e.g. /b/y )
  √ Template String Literals

Function
  √ arrow function
  √ default function parameter values
  √ destructuring for function parameters
  √ Inferences for function name property for anonymous functions
  × Tail-call optimization for function calls and recursion

Array
  √ Methods added to Array.prototype ([].fill(), [].find(), [].findIndex(), [].entries(), [].keys(), [].values() )
  √ Static functions added to Array (Array.from(), Array.of() )
  √ TypedArrays like Uint8Array, ArrayBuffer, Int8Array(), Int32Array(), Float64Array()
  √ Some Array methods (e.g. Int8Array.prototype.slice(), Int8Array.prototype.join(), Int8Array.prototype.forEach() ) added to the TypedArray prototypes    
  √ Some Array statics (e.g. Uint32Array.from(), Uint32Array.of() ) added to the TypedArray constructors

Object
  √ __proto__ in object literal definition sets [[Prototype]] link
  √ Static functions added to Object (Object.getOwnPropertySymbols(), Object.assign() )
  √ Object Literal Computed Property
  √ Object Literal Property Shorthands
  √ Proxies
  √ Reflect

Generator and Promise
  √ Generator function
  √ Promises

Class
  √ Class
  √ super allowed in object methods
  √ class ABC extends Array { .. }

Module
  × Module export command
  × Module import command


=========================================
Passes 39 feature Detections
Your runtime supports 92% of ECMAScript 6
=========================================
```

## 基础语法

### var,let,const

在能实现相同的功能情况下,  使用优先级: const > let > var

```js
const str1 = "const定义";
// const定义常量, 不可改变 Assignment to constant variable.
// str1 = "change";
console.log(str1);

// 但是常量定义的对象中的 
const obj = {
    name: "Tiam",
    age: 18
}
obj.name = "字节跳动";
obj.age = 9;
console.log(obj); // { name: '字节跳动', age: 9 }
```

### 模板字符串 | 字符串遍历

```js
// ES6 模板字符串语法
let str = "妖怪";
console.log(`${str}看打`);

// 遍历字符串
let string = "abcdefghijklmnopqrstuvwxyz";

// 打印0~25
for (const c in string) {
    console.log(c);
}
// 打印a~z
for (const c of string) {
    console.log(c);
}

// 解构赋值
const str2 = "PUA";
let [a, b, c] = str2;
console.log(a, b, c);
console.log(str2[1]);

// 字符串方法
// 判断是否包含某个字符
console.log(str2.includes("A"));
// 重复拼接自身
console.log(str2.repeat(2));
```

### set, map, object

```js
// 对象
let obj_name, obj_age;
let obj1 = {
    obj_name,
    obj_age,
    obj_func() {
        console.log(this.obj_name);
    }
}
obj1.obj_name = "对象名"
obj1.obj_func();

// Set结构, 不存储重复值
let set = new Set();
set.add("one");
set.add("two");
set.add("one");
console.log(set);  // Set(2) { 'one', 'two' }
set.forEach(i => console.log(i))

// Map结构
let map = new Map();
map.set("邓紫棋", "《多远都要在一起》").set("周杰伦", "《明明就》");
console.log(map);
```

### rest参数 | 函数参数默认值 | 箭头函数 | 扩展[运算符](https://so.csdn.net/so/search?q=运算符&spm=1001.2101.3001.7020)

```js
// 定义函数参数带默认值
function fun(name = "Tiam", age = 18) {
    console.log(`${name}, ${age}`);
}
fun();
fun("字节", 102);

// 箭头函数 | 接收不定数的参数
let fun1 = (...songs) => console.log(songs);
fun1("泡沫", "再见", "A.N.I.Y", "光年之外");

function fun2(){
    console.log(arguments);  // [Arguments] { '0': 1, '1': 2, '2': 3, '3': 4, '4': 5 }
    console.log(arguments.length);
    console.log(...arguments);  // 1 2 3 4 5
}

fun2(1,2,3,4,5);

// 扩展运算符 => 可迭代对象包括常用的集合对象（数组、Set、Map集合）和字符串都是可迭代对象. => 展开数组
let obj = {
    "壹": 1,
    "贰": 2,
    "叁": 3
}
console.log({...obj});  // { '壹': 1, '贰': 2, '叁': 3 }
```

### Class类

```js
class Person {
    // 无构造方法时, 自动创建一个无参构造方法
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    getName() {
        return this.name;
    }
    getAge() {
        return this.age;
    }
    setName(name) {
        this.name = name;
    }
    setAge(age) {
        this.age = age;
    }
    toString() {
        return this.name + '(' + this.age + ')';
    }
    run() {
        console.log(`Just run!${this.name}`);
    }
    static cry() {
        console.log("so sad, so cry!");
    }
}
// 类的本质就是 函数
console.log(typeof Person);  // function
Person.cry();
let scientist = new Person("爱因斯坦", 77);
console.log(scientist);
console.log(scientist.toString());

let mathematician = new Person();
mathematician.setName("Oula");
mathematician.setAge(98);
console.log(mathematician.getName());
console.log(mathematician.getAge());


// 类的继承
class Student extends Person {
    constructor(name, age, college) {
        super(name, age);
        this.college = college;
    }
    getCollege() { return this.college }
}

let linux_father = new Student("林纳斯", 21, "芬兰大学");
console.log(linux_father.getCollege); // [Function: getCollege]
console.log(linux_father.getCollege()); // 芬兰大学
console.log(linux_father);  // Student { name: '林纳斯', age: 21, college: '芬兰大学' }
Student.cry();


```

### let、const、var 的区别

> - var:使用 var 声明的变量，其作用域为该语句所在的函数内，且存在变量提升现象
> - let:使用 let 声明的变量，其作用域为该语句所在的代码块内，不存在变量提升。
> - const:使用 const 声明的是常量，在后面出现的代码中不能再修改该常量的值。

| var          | let            | const          |
| ------------ | -------------- | -------------- |
| 函数级作用域 | 块级作用域     | 块级作用域     |
| 变量提升     | 不存在变量提升 | 不存在变量提升 |
| 值可以改变   | 值可以改变     | 值不可以改变   |

###### 1.关于let

```js
 let arr = [];
 for (let i = 0; i < 2; i++) {
     arr[i] = function () {
         console.log(i); 
     }
 }
 arr[0]();
 arr[1]();
```

###### 2.关于const

```js
//常量赋值后，值不能修改。
const PI = 3.14;
 PI = 100; // 报错Assignment to constant variable. 
```

```cpp
const ary = [100, 200];
ary[0] = 'a';
ary[1] = 'b';
console.log(ary); // ['a', 'b']; 
ary = ['a', 'b']; // 报错Assignment to constant variable.
```

###### 3.关于var

var关键字声明的变量，无论实际声明的位置在哪，都会被视为声明在函数的顶部、如果声明不在任意函数内，则视为在全局作用域的顶部

### ES模块导入规则

***容易出错的地方\***

1. 页面不基于服务器运行，会出现跨域的错误
   *origin 'null' has been blocked by CORS policy: Cross origin requests are only*
2. 使用模块化时，页面不加type = "module" 会出现语法错误
   *app.js:1 Uncaught SyntaxError: Unexpected token {*

```javascript
<script src="./module/app.js" type="module"></script>
```

3. import导入模块时不添加 .js 的后缀名会报找不到module错误
   *GET xxx net::ERR_ABORTED 404 (Not Found)*

```javascript
import { Student } from './Student.js';
```

**导入导出方式**

1. 导出方式

> - 定义时导出
> - 批量导出
> - 导出重命名（不建议）
> - 默认导出

```javascript
    // 1.定义时导出
    export let uname = '李四';
    
    export function showStudentName(){
        console.log(uname);
    }
    
    export class SomeAnimalInfo{
        constructor(type,age){
            this.type = type;
            this.age = age;
        }
        showInfo(){
            console.log(`物种:${this.type},年龄：${this.age}`);
        }
    }
    
    // 2.批量导出
    const PI = 3.1415926;
    const DBNAME = 'Local';
    ... ...
    
    export { PI, DBNAME };
    
    // 3.默认导出 - 工具类
    export default class {
        static log(msg) {
            let now = new Date();
            console.log(`${now.toLocaleString()}    ${msg}`);
        }
        
        static setCookie(){
            
        }
        ... ...
    }
```

导入方式

> - 导入重命名
> - 导入整个模块
> - 导入默认模块

```javascript
    //1.导入重命名  as语法
    import {num, showStudentName as showName} from './all.js';
    
    // 2.导入整个模块   需要使用as重命名，接收的是一个对象
    import * as cons from './const.js';
    
    // 3.导入默认模块   需要起名，名字包含导出内容
    import Tool from './tools.js';
```

### Symbol

ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。它是JavaScript语言的第七种数据类型，前六种是：Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）

> - Symbol
> - null
> - undefined
> - boolean
> - string
> - number
> - object

```js
let str1 = "邓紫棋";
let str2 = "邓紫棋";
console.log(str1); // 邓紫棋
console.log(typeof str1); //string
console.log(str1 == str2);  // true

let str3 = Symbol("邓紫棋");
let str4 = Symbol("邓紫棋");
console.log(str3); //Symbol(邓紫棋)
console.log(typeof str3); //symbol
console.log(str3 == str4);  // false
```



### Promise对象

- Promise是异步编程的一种解决方案
- 简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。
- 从语法上说，Promise是一个对象，从它可以获取异步操作的消息。
- Promise提供统一的API，各种异步操作都可以用同样的方法进行处理。
- ES6规定，Promise对象是一个构造函数，用来生成Promise实例。
- http://caibaojian.com/es6/promise.html

### 进制表示

```js
// 二进制
console.log(0B101 == 5);  // true

// 八进制
console.log(0O777);  // 511
```



### Array.from

+ 只要是部署了Iterator接口的数据结构，`Array.from`都能将其转为数组
+ 类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map)

```js
// Array.from

let map = new Map();
map.set("邓紫棋", "《多远都要在一起》").set("周杰伦", "《明明就》");

let arr1 = Array.from(map);
console.log(arr1);

let set = new Set();
set.add("one").add("two").add("three");

let arr2 = Array.from(set);
console.log(arr2);

const arrayLike = {
    0: "Tiam",
    1: 18,
    2: "hbnu",
    length: 3
}
let arr3 = Array.from(arrayLike);
console.log(arr3);

// Array.of 方法用于将一组值，转换为数组。
// 只有当参数个数不少于2个时，Array()才会返回由参数组成的新数组。参数个数只有一个时，实际上是指定数组的长度

let arr4 = Array.of(1, "2", '3', true, set, map);
console.log(arr4);

// 函数的name属性
let fun = function () { }
console.log(fun.name);
```

### 严格模式

- ES6 => 只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式

```js
function doSomething(a, b) {
   // 声明为严格模式
  'use strict';
  // code
}
```

### 箭头函数

箭头函数有几个使用注意点。

（1）函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作Generator函数。

上面四点中，第一点尤其值得注意。*`this`对象的指向是可变的，但是在箭头函数中，它是固定的*。

```js
// 箭头函数

// 一个参数时, 不需要写括号; 函数体只有一条语句时, 且为返回语句时, 可省大括号
let pow2 = param => param * param;

console.log(pow2(9));

// 无参
let fun1 = () => {
    console.log("操作系统");
    console.log("数据结构");
}
fun1();
// 多个参数
let fun2 = (song1,song2)=>{
    console.log(`${song1}很好听`);
    return song2;
}

console.log(fun2("where are you", "kiss the rain"));
```

### Generator 函数

- Generator函数是分段执行的，`yield`语句是暂停执行的标记，而`next`方法可以恢复执行。
- Generator函数是一个状态机，封装了多个内部状态。
- 执行Generator函数会返回一个遍历器对象，也就是说，Generator函数除了状态机，还是一个遍历器对象生成函数
- `function`关键字与函数名之间有一个星号；
- 函数体内部使用`yield`语句，定义不同的内部状态

```js
// Generator 函数

function* hwg() {
    yield "Hello";
    yield "World";
    return "end";
}
let generatorFun = hwg();

for (let i = 0; i < 4; i++) {
    console.log(generatorFun.next());
}
/*
{ value: 'Hello', done: false }
{ value: 'World', done: false }
{ value: 'end', done: true }
{ value: undefined, done: true }
*/
```

## 
