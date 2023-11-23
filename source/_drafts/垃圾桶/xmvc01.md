---
title: springboot模拟登录
cover: >-
  http://qiniu.yujing.fit/image/nice/moon-1527501.jpg
tags: []
id: '384'
categories:
  - - spring boot
abbrlink: e35ee2ef
date: 2021-09-26 22:16:22
---

一个简单的登录判断功能，成功跳转到主页面，显示用户表，否则跳回登录页。如直接访问主页，想跳过登录页，会判断session是否存在用户，有则进入主页面,没有则不显示页面,显示空白。

## 功能大致展示:

## xmvc01数据库user表

```sql
/*
SQLyog Ultimate v12.08 (64 bit)
MySQL - 5.5.12 
*********************************************************************
*/
/*!40101 SET NAMES utf8 */;

create table `user` (
`id` varchar (96),
`userName` varchar (300),
`password` varchar (300)
); 
insert into `user` (`id`, `userName`, `password`) values('1','admin','123');
insert into `user` (`id`, `userName`, `password`) values('2','root','321');
```

## 目录结构

![](http://yujing.fit/wp-content/uploads/2021/09/图片-69.png)

## 登录表单

```html
<form action="${pageContext.request.contextPath}/login" method="post"  >
<div class="form-group">
<label for="userName">用户名</label> <input type="text"
class="form-control" name="userName" placeholder="请输入用户名">
</div>
<div class="form-group">
<label for="password">密码</label> <input type="password"
class="form-control" name="password" placeholder="请输入密码"
value="" >
</div>
<div class="form-inline text-right">
<button type="submit" class="btn btn-primary">登录</button>
<button type="reset" class="btn btn-default" focus>重置</button>
</div>
</form>
```

## 用户列表

```html
<div class="panel-heading">用户列表</div>
                <table class="table table-bordered table-hover text-center">
                    <thead>
                    <tr class="info">
                        <td>用户名称</td>
                        <td>用户密码</td>
                    </tr>
                    </thead>
                    <tbody>
                    <c:forEach var="u" items="${userList}">
                        <tr>
                            <td>${u.userName }</td>
                            <td>${u.password }</td>
                        </tr>
                    </c:forEach>
                    </tbody>
                </table>
            </div>
```

请看注释

## LoginController.java

![](http://yujing.fit/wp-content/uploads/2021/09/图片-70.png)

## userService.java

![](http://yujing.fit/wp-content/uploads/2021/09/图片-72.png)

## userServiceImpl.java

![](http://yujing.fit/wp-content/uploads/2021/09/图片-73-1024x787.png)

## userMapper.java

![](http://yujing.fit/wp-content/uploads/2021/09/图片-74.png)

## userMapper.xml

![](http://yujing.fit/wp-content/uploads/2021/09/图片-75-1024x287.png)

## userController.java

![](http://yujing.fit/wp-content/uploads/2021/09/图片-77-1024x548.png)

## 点击查看源[码](https://wws.lanzoui.com/iu4KDujm55g)