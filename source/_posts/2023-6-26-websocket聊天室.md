---
title: websocket聊天室
categories:
  - JavaScript
tags:
  - websocket
  - socket.io
  - node.js
abbrlink: 5ba3d
date: 2023-06-26 11:20:12
---





# websocket聊天

## 代码

```js
// server.js
const http = require("http");
const fs = require("fs");
const ws = require("socket.io"); // 引入socket.io

const server = http.createServer((req, res) => {
  const html = fs.readFileSync("./client.html"); // client.html是发送给客户端的文件(客户端界面)
  res.end(html);
});

server.listen(8000, () => {
  console.log("服务运行在8000端口");
});

// http服务与ws服务相关联, 返回io服务实例
const io = ws(server);

// 监听用户的连接事件
io.on("connection", socket => {
  // 发生在用户连接io服务器时
  console.log("有新用户进入房间");
  // 获取有关新用户的个人信息
  const id = socket.id;
  io.emit("message", id, `欢迎【${id}】进入房间`, true);

  // 消息发送事件
  socket.on("message", mes => {
    console.log(id, mes);
    io.emit("message", id, mes); // 发送消息给所有客户端（广播）
  });

  // 离开时触发
  socket.on("disconnect", () => {
    console.log("有用户离开房间");
    io.emit("message", id, `用户【${id}】离开房间`, true);
  });
});
```

```html
<!-- client.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Node.js+WebSocket聊天室</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/socket.io/3.0.0-rc4/socket.io.js"></script>
    <style>
      .container {
        width: 800px;
        margin: 0 auto;
        text-align: center;
      }
      .message-form {
        display: flex;
        justify-content: center;
        align-items: center;
      }
      .me {
        color: blue;
        text-align: end;
      }
      .other {
        text-align: start;
      }
      .public {
        color: gray;
        font-size: small;
      }
      .public::before {
        content: "公告：";
      }
    </style>
  </head>
  <body>
    <div class="container">
      <h1>SocketIO聊天室</h1>
      <form class="message-form" action="#" onsubmit="sendMessage(event)">
        <textarea
          class="message"
          name="message"
          placeholder="按Ctrl+enter发送消息"
          rows="1"
          cols="30"
          required
          oninvalid="setCustomValidity('请先输入消息内容')"
          oninput="setCustomValidity('')"
        ></textarea>
        <button class="send">发送</button>
      </form>
      <div class="messages">
        <p class="public">欢迎来到聊天室</p>
      </div>
    </div>
    <script>
      const socket = io.connect("/"); // 连接聊天室的io服务器（"/"为io服务器的根地址）
      const mesForm = document.querySelector(".message-form");

      // 绑定键盘事件
      document.addEventListener("keydown", e => {
        if (e.keyCode === 13 && e.ctrlKey) {
          sendMessage(e);
        }
      });

      // 发送消息
      function sendMessage(e) {
        e.preventDefault();
        const mes = mesForm.message.value;
        socket.send(mes); // 发送消息到服务器
        mesForm.message.value = ""; // 清空文本框
      }

      // 当服务器广播消息时，触发message事件，消息内容在回调函数中
      socket.on("message", (id, mes, is = false) => {
        // 发送的消息，发送者的id
        const p = document.createElement("p");
        p.innerText = is ? mes : `【${id}】：${mes}`;
        // 当消息是自己发送的时候，字体颜色为红色
        p.classList.add(is ? "public" : id === socket.id ? "me" : "other");
        // 将消息添加到消息列表中
        const messages = document.querySelector(".messages");
        messages.append(p);
      });
    </script>
  </body>
</html>
```

package.json

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "nodejs-websocket": "^1.7.2",
    "socket.io": "^4.5.4"
  }
}
```

## 运行

启动方式: 
```bash
$ npm i

$ node server.js
```

地址: http://localhost:8000

## 相关资料
- socket.io: https://socket.io/zh-CN/docs/v4/
- MDN(websocket): https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket
- javascript.info: https://zh.javascript.info/websocket



## 运行效果

![websocket聊天功能演示](http://qiniu.yujing.fit/typora_img/websocket聊天功能演示.gif)