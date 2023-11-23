---
title: 第一个Spring Boot入门程序
tags: []
id: '263'
categories:
  - - spring boot
abbrlink: 7dca1831
date: 2021-09-12 15:31:10
---

1、基于spring框架。

2、使用“约定优先配置”的思想，优化繁琐的手动配置方式。

3、依靠大量注解，实现自动化配置。

4、Spring Boot 2.1.3版本必须要求JDK版本 8 及以上

5、第三方项目构建工具 Maven（3.3+）和Gradle（4.4+）

6、①以Maven方式构建

②以Spring Initializr 方式构建

7、@SpringBootApplication 标记为主程序启动类，是SpringBoot的核心注解。

①以Maven方式构建

**主程序启动类 ：**

```java
package com;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication //标记为主程序启动类
public class ManualChapter01Application {
    //启动方法
    public static void main(String[] args){
        SpringApplication.run(ManualChapter01Application.class,args);
    }
}
```

**请求处理控制类：**

```java
package com.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @GetMapping("/hello")
    public String Hello(){
        return "Hello Spring Boot";
    }
}
```

**依赖文件pom.xml:**

提示框选择"Enable Auto-Import" 选项,自动导入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>manual_chapter01</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```

②以Spring Initializr 方式构建

**目录结构:**

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-27.png)

注**意:** controller类的包应与主程序启动类 _同级_

运行 主程序启动类:

输入 地址: Http://localhost:8080/hello

运行结果:

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-28.png)