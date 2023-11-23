---
title: Promise、async和await
categories:
  - JavaScript
tags:
  - 异步
  - js
abbrlink: '85116811'
date: 2023-06-13 22:34:12
---

# Promise、async和await



## async和await

```js
function getName() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("John Doe");
    }, 2000);
  });
}
// 被async修饰的函数, 总是返回一个 promise, 其值被包装在一个Promise对象中返回
async function getAge() {
  return 25;
}

function getPwd() {
  return Promise.resolve(123456);   // 封装一个值为Promise对象
}

// await: 让 JavaScript 引擎等待直到 promise 完成（settle）并返回结果
// 比如获取异步函数(axios、ajax)中得到的值
async function test() {
  const name = getName();
  const name1 = await getName();  //  仅允许在异步函数和模块顶级使用 "await" 表达式。
  console.log(name); // Promise { 'John Doe' }
  console.log(name1); // John Doe

  const age = getAge();
  const age1 = await getAge();
  console.log(age);  // Promise { 25 }
  console.log(age1);  // 25
    
  const pwd = getPwd();
  const pwd1 = await getPwd();
  console.log(pwd); // Promise { 123456 }
  console.log(pwd1); // 123456
}

test();
```

## Promise

一个promise有三种状态, 且状态只能改变一次

- `pending` 表示未决定的状态
- `resolved` / `fulfilled` 表示成功
- `rejected`  表示失败

同步状态下, 先改变状态, 再执行回调; 异步下, 先执行回调, 再改变状态

```js
/******************使用回调函数 操作异步函数中获取的值********************************************************** */

function getName(callback) {
  setTimeout(() => {
    callback("John Doe");
  }, 2000);
}
getName(name => {
  console.log(name);
});

/*******************使用 promise 操作异步函数中获取的值********************************************************* */

function getAge() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(18);
    }, 2000);
  });
}

getAge()
  .then(age => {
    // 使用 promise 操作异步函数中获取的值
    console.log(age, 1);
    return age; // 传递给下一个 then, promise链
  })
  .then(age => {
    console.log(age, 2);
    return age;
  });

/******************** 一个Promise对象可以做什么? ******************************************************** */

const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("John Doe");
    reject("Error");
  }, 2000);
});

promise
  // 捕获 resolve 中的内容
  .then(name => {
    console.log(name);
  })
  // 捕获 reject 中的内容
  .catch(error => {
    console.log(error);
  })
  .finally(() => {
    // 无论 resolve 或 reject 都会执行
  });
```

## 模拟axios

使用[XMLHttpRequest](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)和Promise封装一个简单的[Axios](https://www.axios-http.cn/)

```js
	  // 模拟封装一个axios
      const axios = ({ method = "get", url }) => {
        return new Promise((resolve, reject) => {
          const xhr = new XMLHttpRequest();
          xhr.open(method, url);
          xhr.responseType = "json";  // 返回类型
          xhr.send();
          xhr.onreadystatechange = () => {
            if (xhr.readyState == 4) {
              // 当请求返回状态码在[200, 300) 范围内时, 表示成功
              if (xhr.status / 100 == 2) resolve(xhr.response);
              else reject(xhr.status);
            }
          };
        });
      };
      axios.get = url => axios({ url });
      axios.post = url => axios({ method: "POST", url });

      // 测试
      axios({ url: "https://api.uomg.com/api/rand.qinghua" })
        .then(res => {
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });

      axios.get("https://api.uomg.com/api/rand.qinghua").then(res => {
        console.log(res);
      });
```

## 更多

- [Promise，async/await (javascript.info)](https://zh.javascript.info/async)
- 
