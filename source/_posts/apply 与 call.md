---
title: JS 中的 apply 与 call 
tags:
  - js
  - apply 与 call
categories:
  - JavaScript
abbrlink: 5bea93f3
date: 2023-02-20 17:15:25
---



# apply 与 call

共同点: 使用定值`this`调用函数

不同点: **参数的形式不同**，apply是将多个参数组成一个数组，call是直接传入多个参数

- [apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
- [call](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

> MDN:
>
> > **备注：** 该方法的语法和作用与 [`apply()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 方法类似，只有一个区别，就是 `call()` 方法接受的是**一个参数列表**，而 `apply()` 方法接受的是**一个包含多个参数的数组**。

```js
/**
 * 
 * @param {number} begin 
 * @param {number} end 
 */
function loop1(begin = 0, end) {
  console.log("loop1");
  for (let i = begin; i < end; i++) {
    let k = i / 1000;  // 执行一些操作
  }
}

/**
 * 执行函数fun, 并返回其运行时间
 * @param {Function} fun
 * @param  {...any} args  rest参数, 可传入任意数量和类型的值, 传入fun函数的参数
 * @returns
 */
function getRunningTime(fun, ...args) {
  let begin = Date.now();
  fun.apply(this, args); // args是一个数组, 与  fun.call(this, ...args); 功能相同
  const end = Date.now();
  return end - begin;
}

const time = getRunningTime(loop1, 1, 10000000);
console.log(time);
```

