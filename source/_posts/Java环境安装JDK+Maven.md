---
title: Java环境安装JDK+Maven大致流程
tags:
  - java
  - maven
abbrlink: aae4df1d
date: 2023-03-17 16:25:54
---



# Java环境安装JDK+Maven大致流程

## 下载

下载java集成开发环境IDE: https://www.jetbrains.com/zh-cn/idea/

> 其实下载IDEA后, 根据里面提示可直接下载jdk, 便可运行Java文件了, 新版IDEA也集成了maven

下载JDK1.8: 

- https://www.oracle.com/cn/java/technologies/javase/javase8-archive-downloads.html

- https://www.oracle.com/java/technologies/downloads/#java8-windows

> 即使Java目前已经更新到Java19了, 但是使用最多的依旧还是Java8(Jdk1.8), 官网强制登陆才能下载旧版本





## 环境配置

系统变量中新增， 值为**安装路径**

<img src="http://qiniu.yujing.fit/typora_img/image-20230317160056394.png" alt="image-20230317160056394" style="zoom: 67%;" />

<img src="http://qiniu.yujing.fit/typora_img/image-20230317160245388.png" alt="image-20230317160245388" style="zoom:67%;" />

测试（可能需要重启电脑）

<img src="http://qiniu.yujing.fit/typora_img/image-20230317160444277.png" alt="image-20230317160444277" style="zoom:50%;" />

---

## maven下载

下载地址： [Maven – Download Apache Maven](https://maven.apache.org/download.cgi)

![image-20230317160907584](http://qiniu.yujing.fit/typora_img/image-20230317160907584.png)



## 设置配置文件

路径： `"~\apache-maven-3.8.8\conf\settings.xml"`

settings.xml中添加（配置阿里镜像源）

```xml
	<mirror>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <mirrorOf>central</mirrorOf>
    </mirror>
```

> 注意：要在`<mirrors>`标签中添加

添加本地库地址(自己创建一个文件夹作为 本地库)

```xml
<localRepository>D:/Program Files/maven-3.8.8/repository</localRepository>
```

## maven环境变量

<img src="http://qiniu.yujing.fit/typora_img/image-20230317161149599.png" alt="image-20230317161149599" style="zoom:67%;" />

在Path中新增

<img src="http://qiniu.yujing.fit/typora_img/image-20230317161247594.png" alt="image-20230317161247594" style="zoom:67%;" />

测试

<img src="http://qiniu.yujing.fit/typora_img/image-20230317161352227.png" alt="image-20230317161352227" style="zoom:67%;" />

## IDEA配置

<img src="http://qiniu.yujing.fit/typora_img/image-20230317162031070.png" alt="image-20230317162031070" style="zoom: 50%;" />