---
title: Spring注册Bean对象的几种方式
categories:
  - Java
tags:
  - spring
  - java
abbrlink: 2ba898ad
date: 2023-04-27 22:24:47
---

## 一、xml配置文件形式

```java
package org.example.pojo;
// 只是一个普通的Java类
public class User {
}
```

```xml
<!-- bean.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<!-- 注册到IOC容器中, 注册为bean对象 -->
    <bean id="user" class="org.example.pojo.User" />
</beans>
```

测试

```java
    @Test
    public void testXmlConfig() {
        // 加载bean.xml配置文件
        ApplicationContext ctx = new ClassPathXmlApplicationContext("bean.xml");
        // 在ioc容器中获取一个id为user且类型为User的bean对象
        User user = ctx.getBean("user", User.class);
        // 打印其地址
        System.out.println(user);  // org.example.pojo.User@5c8eee0f
    }
```

## 二、配置类形式

```java
package org.example.pojo;

public class Admin {
}
```

```java
package org.example.config;

@Configuration 
public class BeanConfig {
    @Bean  // 将返回对象作为Bean对象注册到IOC容器中
    public Admin admin() {
        return new Admin();
    }
}

```

测试

```java
    @Test
    public void testConfigClass() {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(BeanConfig.class);
        Admin admin = ctx.getBean("admin", Admin.class);
        System.out.println(admin);  // org.example.pojo.Admin@77192705
    }
```

## 三、注解+包扫描

```java
package org.example.pojo;

@Component  // 标注Spring管理的Bean，使用@Component注解在一个类上，表示将此类标记为Spring容器中的一个Bean。
// 类似的注解还有@Controller、@Service、@Repository、@Mapper、@Configuration等， 
// @Configuration使用了cglib代理: org.example.pojo.SuperAdmin$$SpringCGLIB$$0@1e11bc55
public class SuperAdmin {
}
```

方式一: 注解形式扫描包

```java
@ComponentScan("org.example.pojo")
public class AppConfig {
}
```

测试

```java
    @Test
    public void testConfigAnnotation() {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
        SuperAdmin superAdmin = ctx.getBean("superAdmin", SuperAdmin.class);
        System.out.println(superAdmin);  // org.example.pojo.SuperAdmin@f79a760
    }
```

方式二: xml配置文件形式

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!-- xml配置文件形式 扫描包 -->
    <context:component-scan base-package="org.example.pojo"/>
</beans>
```

测试

```java
    @Test
    public void testXmlAnnotation() {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        SuperAdmin superAdmin = ctx.getBean("superAdmin", SuperAdmin.class);
        System.out.println(superAdmin);  // org.example.pojo.SuperAdmin@5cc69cfe
    }
```



---



## Spring测试环境

修改`AppConfig`

```java
@ComponentScan("org.example.pojo")
@ImportResource("classpath:bean.xml")  // 导入xml配置文件
public class AppConfig {
}
```

测试类

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = {AppConfig.class, BeanConfig.class})  // 通过注解指定配置类
// 通过注解指定xml配置文件, 但是两种不能同时使用
//@ContextConfiguration({"classpath:bean.xml","classpath:applicationContext.xml"})  
public class SpringTest {
    @Autowired  // 自动装配, 默认按照类型装配byType, spring自带的注解
    private SuperAdmin superAdmin;

    @Test
    public void test() {
        System.out.println(superAdmin);  // org.example.pojo.SuperAdmin@35beb15e
    }

    @Autowired
    @Qualifier("admin")  // 指定名称
    private Admin admin;

    @Test
    public void test1() {
        System.out.println(admin);  // org.example.pojo.Admin@263f04ca
    }

    @Resource  // 自动装配指定名称的bean byName,
    // 在jakarta.annotation.Resource包下, 或者在javax.annotation.Resource包下 (根据jdk版本选择)
    private User user;

    @Test
    public void test2() {
        System.out.println(user);  // org.example.pojo.User@5c8eee0f
    }
}
```



> 本次环境是Spring6+JDK17
>
> 测试工具用的JUniper
