---
title: application.yaml配置
tags: []
id: '427'
categories:
  - - spring boot
abbrlink: d96f0873
date: 2021-10-13 20:52:32
---

```java
public class Pet {
    private String type;
    private int age;
	//setter方法
}
```

```java
@Component  //注入spring的bean中
@ConfigurationProperties(prefix = "user")
//在配置文件中,将以user开头的属性注入
public class User {
    private int id;  //基本数据类型 int
    private String username;  //基本数据类型 String
    private String password;  
    private List hobby;  //集合
    private String\[\] song;  //数组
    private Map map;  //Map对象
    private Pet pet;  //POJO对象

//setter方法
}
```

application.yeal **注意!冒号后有个空格!!!!** application.properties 比 application.yeal优先级高

```yml
user:
  id: 2
  username: linda
  password: 123
  hobby: \[footboll,basketboll\]
  song: \[she,where\]
  map: {k1: v1,k2: v2}
  pet: {type: cat,age: 12}
```

```java

测试类

@RunWith(SpringRunner.class)
@SpringBootTest
public class Tests01 {
    @Autowired
    private User user;
    @Test
    public void test01(){
        System.out.println(user);
    }
}
```

