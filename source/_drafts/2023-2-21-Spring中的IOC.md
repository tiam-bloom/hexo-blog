---
title: Spring中的IOC
date: 2023-2-21 10:24:15 
categories:
- Java
tags:
- java
- spring
- ioc
---



## Bean

介绍: [Core Technologies (spring.io)](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction)

> IoC也称为依赖注入 (dependency injection，DI)。这是一个过程，对象仅通过构造函数参数，工厂方法的参数或在构建或从工厂返回对象实例后在对象实例上设置的属性来定义它们的依赖关系 (即它们使用的其他对象) 方法。然后，容器在创建bean时注入这些依赖关系。这个过程基本上是bean本身的逆 (因此称为控制反转)，它通过使用类的直接构造或诸如服务定位器模式之类的机制来控制其依赖项的实例化或位置。

一个Spring IOC容器管理一个或多个[Bean](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-definition)。这些Bean是使用您提供给容器的配置元数据(例如，以XML定义的形式)创建的。

| Property属性             | Explained in…解释                                            |
| :----------------------- | :----------------------------------------------------------- |
| Class                    | [Instantiating Beans](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-class) |
| Name                     | [Naming Beans](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-beanname) |
| Scope                    | [Bean Scopes](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes) |
| Constructor arguments    | [Dependency Injection](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-collaborators) |
| Properties               | [Dependency Injection](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-collaborators) |
| Autowiring mode          | [Autowiring Collaborators](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-autowire) |
| Lazy initialization mode | [Lazy-initialized Beans](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lazy-init) |
| Initialization method    | [Initialization Callbacks](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lifecycle-initializingbean) |
| Destruction method       | [Destruction Callbacks](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lifecycle-disposablebean) |

![image-20230224082547892](http://qiniu.yujing.fit/typora_img/image-20230224082547892.png)

## 控制反转IOC

```java
public class UserService {
    public void addUser(){
        System.out.println("addUser() go!");
    }
}
```

方式一: `xml`配置

```xml
<!-- 配置文件applicationContext.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="userService" class="com.example.service.UserService"/>
</beans>
```

```java
// 测试 
    @Test
    public void iocTest(){
        // 加载配置文件
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 根据bean ID获取实例
        UserService userService = applicationContext.getBean("userService", UserService.class);
        // UserService userService = (UserService)applicationContext.getBean("userService");  // 或者使用强转
        // 调用实例方法
        userService.addUser();
    }
```

方式二: 配置类(需导入`aop`包)

```java
// 配置类(不再需要xml配置文件)
@Configuration
public class AppConfig {
    @Bean
    public UserService userService(){
        return new UserService();
    }
}
```

```java
// 测试
	@Test
    public void iocTest01(){
        ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
        UserService userService = ctx.getBean(UserService.class);
        userService.addUser();
    }
```

方式三: 混合使用

```xml
<!-- 配置文件, 注意!添加了xmlns:context属性, 和xsi:schemaLocation属性中添加了新值 -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd 
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd ">
	<!-- 配置类所在包路径 -->
    <context:component-scan base-package="com.example.config"/>
    <!-- 其他 bean 配置 ... -->
</beans>
```

```java
// 配置类
package com.example.config;
import ...

@Configuration
public class AppConfig {
    @Bean
    public UserService userService(){
        return new UserService();
    }
}
```

```java
// 测试
    @Test
    public void iocTest() {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = applicationContext.getBean("userService", UserService.class);
        userService.addUser();
    }
```

## 依赖注入DI

```xml
    <bean id="adminService" class="com.example.service.AdminService">
        <!--向name属性(必须提供一个setter方法)注入一个变量值, 
		调用adminService类中的getName方法,
		应用程序上下文中的所有 bean 都可以作为预定义变量使用-->
        <property name="name" value="#{ adminService.getName() }"/>
    </bean>
```

上下两种形式等同

```xml
<!-- beans中引入新属性 xmlns:p="http://www.springframework.org/schema/p" -->
<bean id="adminService" class="com.example.service.AdminService" p:name="#{ adminService.getName() }" />
```

```xml
<!-- 以properties文件的形式注入值, 但是这个id=mappings怎么用? -->
	<bean id="mappings" class="org.springframework.context.support.PropertySourcesPlaceholderConfigurer">
        <property name="properties" >
            <value>
                adminService.name = 张三
            </value>
        </property>
    </bean>
```

## 作用域

[Bean的作用域 (spring.io)](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes)

## 管理Bean的三种模式

### 静态工厂模式创建Bean

```java
public class AppConfig {
    public static UserService getUserService() {
        return new UserService();
    }
}
```

```xml
<bean id="userFactory" class="com.example.config.AppConfig" factory-method="getUserService" />
```

```java
    @Test
	public void iocTest02(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = applicationContext.getBean("userFactory", UserService.class);
        userService.addUser();
    }
```

### 实例工厂模式

```java
public class AppConfig {
    public UserService userService(){
        return new UserService();
    }
}
```

```xml
    <bean id="factory" class="com.example.config.AppConfig" />
    <bean id="userService" factory-bean="factory" factory-method="userService" />
```

```java
    @Test
    public void iocTest03(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = applicationContext.getBean("userService", UserService.class);
        userService.addUser();
    }
```

### 无参构造方式👌***

- 无法重写构造器

```xml
<bean id="userService" class="com.example.service.UserService"/>
```

```java
    @Test
    public void iocTest03(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = applicationContext.getBean("userService", UserService.class);
        userService.addUser();
    }
```

## Bean中属性值

- id 指定唯一名称
- name 指定多个名称, 用逗号或分号相隔
- class 具体实现类, 类的全限定名
- scope 作用域singleton(单例), prototype(多例), request, session, global Session
- constructor-arg
- property
- ref
- value
- list
- set
- map
- entry



## 属性注入DI

- 有参构造
- setter
- 接口注入

### 有参构造

```java
public class User {
    private String name;
    // 有参构造函数
    public User(String name) {
        this.name = name;
    }
}
```

```xml
		<bean id="user" class="com.example01.pojo.User">
            <constructor-arg name="name" value="张三(基本类型)" />  
        </bean>
```

### setter注入**

`property` `value`

```java
public class User {
    private String name;
    // ...
    private Properties properties;
    // setter方法...
    public void setName(String name) {
        this.name =  name;
    }
}
```

```xml
    <bean id="user" class="com.example01.pojo.User">
        <property name="name" value="张三"/>
        <!-- ...集合,map,set注入-->
        <property name="hobbies" >
            <list>
                <value>唱</value>
                <value>跳</value>
                <value>rap</value>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="1" value="张三"/>
                <entry key="2" value="李四"/>
            </map>
        </property>
        <property name="set">
            <set>
                <value>张三</value>
                <value>李四</value>
            </set>
        </property>
        <property name="properties">
            <props>
                <prop key="1">张三</prop>
                <prop key="2">李四</prop>
            </props>
        </property>
    </bean>
```

注入引用类型*

`property` `ref`

```java
public class UserDao {
    public void findUserById(){
        System.out.println("findUserById() go!");
    }
}
```

```java
public class UserService {
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void findUserById(){
        userDao.findUserById();
    }
}
```

```xml
    <bean id="userDao" class="com.example01.dao.UserDao"/>
    <bean id="userService" class="com.example01.service.UserService">
        <property name="userDao" ref="userDao"/>
    </bean>
```

```java
    public void test03(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = applicationContext.getBean("userService", UserService.class);
        userService.findUserById();
    }
```

## 注解方式





