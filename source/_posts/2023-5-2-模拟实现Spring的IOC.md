---
title: 模拟实现Spring的IOC
categories:
  - Java
tags:
  - java
  - spring
abbrlink: d807e6df
date: 2023-05-02 18:06:02
---





创建两个注解

`@Bean`实现类似`@Component`的作用

`@DI`实现类似`@Autowired`的作用

```java
// 可使用的范围, 类、接口(包括注释接口)、枚举或记录声明
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Bean {
}
```

```java
// 字段声明(包括枚举常量)
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME) // 运行时保留
public @interface DI {
}
```



```java
public interface ApplicationContext {
    // 根据类型获取Bean对象
    Object getBean(Class<?> clazz);
}
```

```java
public class AnnotationApplicationContext implements ApplicationContext {
    /**
     * 存放bean对象
     */
    private Map<Class<?>, Object> beanFactory = new HashMap<>();
    private static String rootPath;

    @Override
    public Object getBean(Class<?> clazz) {
        return beanFactory.get(clazz);
    }

    /**
     * 扫描包路径, 将带有@Bean注解的类, 注册为Bean对象
     *
     * @param basePackage 包路径
     */
    public AnnotationApplicationContext(String basePackage) {
        // 替换.为\
        String packagePath = basePackage.replaceAll("\\.", "\\\\");
        // 获取包绝对路径
        Enumeration<URL> urls;
        try {
            urls = Thread.currentThread().getContextClassLoader().getResources(packagePath);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        while (urls.hasMoreElements()) {
            URL url = urls.nextElement();
            String filePath = URLDecoder.decode(url.getFile(), StandardCharsets.UTF_8);
            rootPath = filePath.substring(0, filePath.length() - packagePath.length());
            // 包扫描@Bean注解和@DI注解
            loadBean(new File(filePath));
            loadDI();
        }
    }

    /**
     * 实例化file目录中所有带@Bean注解的类
     * @param file
     */
    private void loadBean(File file) {
        // 判断是否是文件夹
        if (!file.isDirectory()) return;
        // 获取子目录
        File[] childrenFiles = file.listFiles();
        // 判断是否存在内容
        if (childrenFiles == null) return;
        // 遍历子目录
        for (File childrenFile : childrenFiles) {
            // 递归子目录
            if (childrenFile.isDirectory()) loadBean(childrenFile);
            else {
                String pathWithClass = childrenFile.getAbsolutePath().substring(rootPath.length() - 1);
                // 判断是否为.class类型文件
                if (!pathWithClass.contains(".class")) return;
                // 获取类全路径, 删除.class, 替换\为.
                String allName = pathWithClass.replaceAll("\\\\", ".").replace(".class", "");
                try {
                    instanceClass(allName);
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }

    /**
     * 将allName全类路径下的类注册为Bean对象, 存入beanFactory中
     *
     * @param allName 类全路径
     */
    private void instanceClass(String allName) throws Exception {
        Class<?> clazz = Class.forName(allName);
        // 判断是否为接口
        if (clazz.isInterface()) return;
        Bean annotation = clazz.getAnnotation(Bean.class);
        // 判断是否存在@Bean注解
        if (annotation == null) return;
        // 实例化对象
        Object instance = clazz.getConstructor().newInstance();
        // 存入beanFactory中, 若类存在接口则将接口做key
        beanFactory.put(clazz.getInterfaces().length > 0 ? clazz.getInterfaces()[0] : clazz, instance);
    }

    /**
     * 依赖注入DI, 只有在Bean对象中才能使用@DI注解进行依赖注入
     */
    private void loadDI(){
        Set<Map.Entry<Class<?>, Object>> entries = beanFactory.entrySet();
        // 遍历对象
        for (Map.Entry<Class<?>, Object> entry : entries) {
            // 获取属性
            Object value = entry.getValue();
            Class<?> clazz = value.getClass();
            Field[] declaredFields = clazz.getDeclaredFields();
            for (Field declaredField : declaredFields) {
                DI annotation = declaredField.getAnnotation(DI.class);
                // 判断是否存在@DI注解
                if (annotation==null) return;
                // 避免私有字段无法设置
                declaredField.setAccessible(true);
                // declaredField.getType() 获取属性的类型, 根据类型获取map的value
                try {
                    // 注入属性
                    declaredField.set(value,beanFactory.get(declaredField.getType()));
                } catch (IllegalAccessException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
}
```





---

 测试

service层

```java
public interface UserService {
    void addUser();
}
```

```java
@Bean
public class UserServiceImpl implements UserService {
    @DI
    private UserDao userDao;

    @Override
    public void addUser() {
        userDao.addUser();
    }
}
```

dao层

```java
public interface UserDao {
    void addUser();
}
```

```java
@Bean
public class UserDaoImpl implements UserDao {
    @Override
    public void addUser() {
        System.out.println("add User is run!");
    }
}
```

测试类

```java
    @Test
    public void testDIAnnotation(){
        ApplicationContext ctx = new AnnotationApplicationContext("org.example");
        UserService userService = (UserService)ctx.getBean(UserService.class);
        userService.addUser();  // add User is run!
    }
```

