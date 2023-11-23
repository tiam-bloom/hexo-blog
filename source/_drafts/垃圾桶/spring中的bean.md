---
title: Spring中的Bean
tags: []
id: '271'
categories:
  - - spring boot
abbrlink: 5a117f65
date: 2021-09-15 18:36:58
---

## 以Maven方式创建文件

#### 1 文件目录结构:

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-29.png)

#### 2 在pom.xml中添加配置文件,将自动下载jar包

```xml
<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

        <!-- Spring核心模块 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>
    </dependencies>
```

#### 3 新增配置文件

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-30-1024x732.png)

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-31-1024x252.png)

#### 4

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-32-1024x366.png)

#### 5 UserController类中添加save方法

```java
public void save(){
        System.out.println("success print controller");
    }
```

#### 6 添加测试文件,运行测试

查看控制台,成功调用UserController类中的save方法,打印出 " success print controller "

```java
@Test
    public void test01(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        UserController userController = (UserController)applicationContext.getBean("userController");
        userController.save();
    }
```

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-33-1024x497.png)

## 基于xml的Spring DI实现

通过Controller类调用Service中的方法,直接调用Controller,而测试类就不用既调用COntroller类又调用Service类,就可实现成功调用两个类中的方法.

#### 1 创建UserService类

```java
public class UserService {
    public void save(){
        System.out.println("success print service");
    }
}
```

#### 2 在Controller中注入Service

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-34-1024x378.png)

#### 3 在Controller类中调用Service

```java
private UserService userService;  //引入UserService

    public void setUserService(UserService userService) {
        this.userService = userService;    //添加setter方法
    }

    public void save(){
        System.out.println("success print controller");
        userService.save();    //调用save方法
    }
```

#### 4 运行显示成功调用

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-36-1024x723.png)

## 基于注解的Spring DI实现

#### 1

```java
@Controller("userController")
public class UserController {
    @Resource(name = "userService")

    private UserService userService;

//    public void setUserService(UserService userService) {
//        this.userService = userService;
//    }

    public void save(){
        System.out.println("success print controller");
        userService.save();
    }
}
```

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-41.png)

UserService:

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-43.png)

#### 2

添加扫描器扫描包

bean被注解代替

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-45.png)

#### 3 运行项目

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-47.png)

源文件下载:[https://wwr.lanzoui.com/ijP6hu2f4sj](https://wwr.lanzoui.com/ijP6hu2f4sj)