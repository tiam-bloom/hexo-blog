---
title: 'var, let 与 const'
tags:
  - js
categories:
  - JavaScript
abbrlink: '38684297'
date: 2023-02-20 16:06:13
---



## 暂时性死区

如果**区块中**存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

```js
let a = 1;
// 块级作用域
{
    a = 2;  // Cannot access 'a' before initialization
    let a = 3;
}
```

## 变量提升

变量可以在`var`**声明之前**使用，值为`undefined`。

```js
console.log(a); // undefined
var a = 1;
```

```js
console.log(a); // Cannot access 'a' before initialization
let a = 1;
```

## 重复声明

- `var`可重复声明
- `let`不可重复声明

```js
var a = 1;
var a = 2;
console.log(a); // 2
```

```js
let a = 1;
let a = 2;  // Identifier 'a' has already been declared
```

## 作用域

```js
{
    var a = 1;
}
console.log(a); // 1
```

```js
{
    let a = 1;
}
console.log(a); // ReferenceError: a is not defined
```

## 全局对象

node环境中

```js
x = "哈哈哈哈";
var y = "呵呵呵呵";
let z = "嘿嘿嘿嘿";
console.log(global.x); // "哈哈哈哈"
console.log(global.y); // undefined
console.log(global.z); // undefined
```

brower环境中

```js
      x = "哈哈哈哈";
      var y = "呵呵呵呵";
      let z = "嘿嘿嘿嘿";
      console.log(window.x); // 哈哈哈哈
      console.log(window.y); // 呵呵呵呵
      console.log(window.z); // undefined
```

总结`let`的属性特征

- 不属于**全局对象**`window` /`global`
- 不允许**重复声明** 
- 不存在**变量提升** 
- 存在**暂时性死区** 
- **块级作用域**

## const

- `const`声明的变量必须**初始化**
- 且不能修改值, **只读**(对于对象类型的数据, 可修改其属性, 但不能修改其地址, 即整体赋值)
- 作用域与`let`基本相同