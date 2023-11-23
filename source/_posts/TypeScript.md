---
title: typescript
tags:
  - typescript
  - ts
categories:
  - TypeScript
abbrlink: 98b5ec51
date: 2023-02-15 22:22:11
---



# TypeScript

## 介绍

> [官方](https://www.typescriptlang.org/)介绍:  TypeScript is **JavaScript with syntax for types.**
>
> 翻译过来就是: "TypeScrip是带有**类型**语法的JavaScript。"

## 历史

> 2012年10月，微软发布了首个公开版本的TypeScript。2013年6月19日，在经历了一个[预览版](https://baike.baidu.com/item/预览版/8601080?fromModule=lemma_inlink)之后微软正式发布了[正式版](https://baike.baidu.com/item/正式版/5077488?fromModule=lemma_inlink)TypeScript。**TypeScript**的作者是[安德斯·海尔斯伯格](https://baike.baidu.com/item/安德斯·海尔斯伯格/2152925?fromModule=lemma_inlink)，C#的[首席架构师](https://baike.baidu.com/item/首席架构师/10884085?fromModule=lemma_inlink)。

## 安装

```shell
> npm install -g typescript
```



## 数据类型

- 与js中数据类型一致，增加了枚举类型
- number
- string
- boolean
- undefined
- BigInt
- Symbol
- Object

函数返回类型

- void
- never

```ts
    let a: any; // any类型, 可以赋值任意类型的值
    let hello = 'hello';  // 隐式any类型, 赋值时推断出string类型
    let helloworld: string = `${hello} world`;
    console.log(helloworld);
```

```ts
    // 数组
    let list1: number[] = [1, 2, 3];
    console.log(list1);

    let list2: Array<number> = [1, 2, 3];
```

- 枚举

```ts
    // 枚举, 默认从0开始(没有初始值时)
    enum Color { Red = '#fe5f55', Green = '#fff1c1', Blue = '#293462' };
	// 限制变量c的值只能为枚举中的值
    let c: Color = Color.Green;
    console.log(c);  // #fff1c1
```

```ts
    // void, 表示没有任何类型, 一般用于函数返回值, 其值只能为undefined或null
    function warnUser(): void {
        console.log('This is my warning message');
    }
    console.log(warnUser());  // undefined
```

```ts
    // never, 表示永远不会有返回值的类型, 一般用于函数返回值
    // 返回never的函数必须存在无法达到的终点
    function error(message: string): never {
        throw new Error(message);
    }
    // 自动推断返回类型为never
    function error_(message: string) {
        return error("Something failed");
    }
```

## 解构赋值

```ts
    let obj = {
        name: 'zhangsan',
        age: 18,
    }
    let { name, age }: { name: string, age: number } = obj;
    type C = { name: string, age: number };
    let { name: name1, age: age1 }: C = obj;
```

## 接口`interface`

```ts
// 预定义对象中属性的类型
interface Person {
    // 只读属性
    readonly name: string;
    age: number;
    // 可选属性
    isAdmin?: boolean;
}
type P = Person;
let zhangsan: P = {
    name: 'zhangsan',
    age: 18,
};

```

## 只读属性

- 做为变量使用的话用 const`，

- 若做为属性则使用`readonly`。

```ts
// 只读数组
let arr: ReadonlyArray<number> = [1, 2, 3];
// let newArr:number[] = arr;  // "readonly number[]" 为 "readonly"，不能分配给可变类型 "number[]"
let newArr: number[] = arr as number[];  // 类型断言, it's ok
```

## 接口与type

- `type`可实现的`interfere`都可以实现
- 区别: type不可新增属性

```ts
interface User {
    name: string,
    age: number,
}
interface VIPUser extends User {
    money: number,
}
let jam: VIPUser;
// 可向已定义的接口添加新属性height
interface User {
    height: number,
}
let rose: User = {
    name: 'rose',
    age: 18,
    height: 165
}
```



```ts
    type User = {
        name: string,
        age: number,
    }
    type VIPUser = User & {
        money: number,
    }
    // 不能再向User中添加新属性
    let jack: VIPUser;
```



## 泛型

```ts
/**
 * T 代表泛型，具体什么类型是调用这个方法的时候决定的
 * @param arg 
 * @returns 
 */
function fun<T>(arg: T): T {
    return arg;
}

/**
 * 泛型类, data的类型由实例化的时候决定
 */
class Data<T> {
    data: T;
}
```



## 枚举作类型

```ts
    // 枚举, 默认从0开始(没有初始值时)
    enum Color { Red = '#fe5f55', Green = '#fff1c1', Blue = '#293462' };
	// 限制变量c的值只能为枚举中的值
    let c: Color = Color.Green;
    console.log(c);  // #fff1c1
```



## 函数参数

```ts
/**
 * 
 * @param id 多类型参数
 */
function moreType(id: number | string) {

}
/**
 * 
 * @param id 可选参数
 */
function optional(id?: number) {

}
/**
 * 
 * @param fun 当参数为函数时
 */
function doFun(fun: (str: string) => void) {
    fun('hello typrScript');
}
```

## 相关链接

- 中文手册: https://typescript.bootcss.com/
- 官方文档: https://www.typescriptlang.org/zh/docs/
- 中文手册2: https://www.tslang.cn/docs/handbook/basic-types.html
- 菜鸟TS教程: https://www.runoob.com/typescript/ts-tutorial.html
