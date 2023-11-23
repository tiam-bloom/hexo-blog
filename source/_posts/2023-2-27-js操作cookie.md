---
title: js操作cookie
tags:
  - cookie
  - js
categories:
  - JavaScript
abbrlink: 45aa9bcf
date: 2023-02-27 15:48:11
---

## cookie介绍(转自wiki)

**HTTP cookie**，简称**cookie**，是[用户](https://zh.wikipedia.org/wiki/用户_(计算机))浏览[网站](https://zh.wikipedia.org/wiki/网站)时由[网络服务器](https://zh.wikipedia.org/wiki/网络服务器)创建并由用户的[网页浏览器](https://zh.wikipedia.org/wiki/网页浏览器)存放在用户电脑或其他设备上的小文本文件。

Cookie使Web服务器能够在用户的设备上存储状态信息（如添加到在线商店购物车中的商品）或跟踪用户的浏览活动（如点击特定按钮、登录或记录历史）

### 属性

一个cookie可能有多个属性，如Domain、Path、Expires、Max-Age、Secure、HttpOnly，例如：

```
HTTP/1.0 200 OK
Set-Cookie: LSID=DQAAAK…Eaem_vYg; Path=/accounts; Expires=Wed, 13 Jan 2021 22:23:01 GMT; Secure; HttpOnly
Set-Cookie: HSID=AYQEVn…DKrdst; Domain=.foo.com; Path=/; Expires=Wed, 13 Jan 2021 22:23:01 GMT; HttpOnly
Set-Cookie: SSID=Ap4P…GTEq; Domain=foo.com; Path=/; Expires=Wed, 13 Jan 2021 22:23:01 GMT; Secure; HttpOnly
…
```

#### Domain和Path

`Domain`和`Path`属性定义了cookie的范围。它们告诉浏览器cookie属于哪个网站。Cookies只能设置在当前资源的顶级域及其子域上。

如果服务器未指定cookie的`Domain`和`Path`，则它们默认为所请求资源的域和路径[[20\]](https://zh.wikipedia.org/zh-sg/Cookie#cite_note-uMnRY-20)。

#### Expires和Max-Age

`Expires`属性定义了浏览器应该删除cookie的时间，格式为`Wdy, DD Mon YYYY HH:MM:SS GMT`，或者`Wdy, DD Mon YY HH:MM:SS GMT`（其中YY大于或等于0并且小于等于69）。

此外，也可使用`Max-Age`将cookie的过期时间设置为某一段时间之后（相对于浏览器接收cookie的时间而言）。但一些浏览器可能不支持`Max-Age`，如Internet Explorer

#### Secure和HttpOnly

`Secure`属性旨在将cookie加密，使浏览器仅通过[安全/加密](https://zh.wikipedia.org/wiki/超文本传输安全协议)连接使用cookie。

`HttpOnly`要求浏览器不要通过HTTP（和HTTPS）以外的渠道使用cookie。这意味着无法通过客户端脚本语言（尤其是[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)）访问cookie，因此无法通过[跨站点脚本攻击](https://zh.wikipedia.org/wiki/跨網站指令碼)轻易窃取。

## 原生js操作

```js
        // 读取cookie(会读取全部, 需自行解析)
        console.log(document.cookie);
        // 设置cookie, 默认关闭浏览器失效, 存在name的key覆盖, 不存在添加
        document.cookie = "name=zs";
        // 设置过期时间在2023年3月28日失效
        document.cookie = `age=18;expires=${new Date(2023, 2, 28)}`;
        // 设置有效时间为3600秒
        document.cookie="gender=男;max-age=3600";
        // 删除Cookie 1. 设置过期时间为过去的时间 2. 设置max-age为0
        document.cookie = "name=;expires=Thu, 01 Jan 1970 00:00:00 GMT";
        document.cookie = "age=;max-age=0";
```

## 第三方库

https://www.npmjs.com/package/js-cookie

```js
        // 设置cookie, 已存在则覆盖(修改)
        Cookies.set('name', 'value');
        // 设置cookie, 有效期为7天
        Cookies.set('name', 'value', { expires: 7 });
        // 读取cookie
        console.log(Cookies.get('name'));
        // 删除cookie
        Cookies.remove('name');
```

## 更多

- https://zh.javascript.info/cookie