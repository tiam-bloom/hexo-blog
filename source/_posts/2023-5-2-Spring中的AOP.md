---
title: Spring中的AOP
categories:
  - Java
tags:
  - spring
  - aop
abbrlink: 27397cfd
date: 2023-05-02 17:15:24
---





现在有一个接口`Calculator`

```java
public interface Calculator {
    int add(int a, int b);
    int sub(int a, int b);
    int mul(int a, int b);
    int div(int a, int b);
}
```

其实现类`CalculatorImpl`实现了基本的算数

```java
public class CalculatorImpl implements Calculator {
    @Override
    public int add(int a, int b) {
        return a + b;
    }

    @Override
    public int sub(int a, int b) {
        return a - b;
    }

    @Override
    public int mul(int a, int b) {
        return a * b;
    }

    @Override
    public int div(int a, int b) {
        return a / b;
    }
}
```

那么现在有一个需求, 就是我需要在每次计算前后打印日志, 显示参数`a`和`b`是多少, 以及计算结果是多少

## 重写实现类

使用`CalculatorLogImpl`重新去实现`Calculator`接口, 便可达到目的

```java
public class CalculatorLogImpl implements Calculator {
    @Override
    public int add(int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = a + b;
        System.out.println("[日志]结果为: " + result);
        return result;
    }

    @Override
    public int sub(int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = a - b;
        System.out.println("[日志]结果为: " + result);
        return result;
    }

    @Override
    public int mul(int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = a * b;
        System.out.println("[日志]结果为: " + result);
        return result;
    }

    @Override
    public int div(int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = a / b;
        System.out.println("[日志]结果为: " + result);
        return result;
    }
}
```

测试一下

```java
    @Test
    public void testCalculatorLog() {
        Calculator calculator = new CalculatorLogImpl();
        calculator.add(1, 2);
        calculator.sub(1, 2);
        calculator.mul(1, 2);
        calculator.div(1, 2);
    }
```

没有问题

```
[日志]参数为: 1, 2
[日志]结果为: 3
[日志]参数为: 1, 2
[日志]结果为: -1
[日志]参数为: 1, 2
[日志]结果为: 2
[日志]参数为: 1, 2
[日志]结果为: 0

进程已结束,退出代码0
```

但是很明显, 方法中新增了与业务逻辑无关的代码, 且这样冗余代码过多, 每个方法中都有相同的代码

---

## 静态代理

使用静态代理类`CalculatorStaticProxy`优化(封装), 业务代码一层, 日志再封装一层

```java
public class CalculatorStaticProxy implements Calculator{
    private Calculator calculator;

    public CalculatorStaticProxy(Calculator calculator) {
        this.calculator = calculator;
    }

    @Override
    public int add(int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = calculator.add(a, b);
        System.out.println("[日志]结果为: " + result);
        return result;
    }

    @Override
    public int sub(int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = calculator.sub(a, b);
        System.out.println("[日志]结果为: " + result);
        return result;
    }

    @Override
    public int mul(int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = calculator.mul(a, b);
        System.out.println("[日志]结果为: " + result);
        return result;
    }

    @Override
    public int div(int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = calculator.div(a, b);
        System.out.println("[日志]结果为: " + result);
        return result;
    }
}
```

测试没问题, 虽然这样分离了业务逻辑代码和日志, 但是代码依然冗余

(Attention: 这里使用的是`CalculatorImpl`接口实现类, 之后也都使用的这这个, 而不是`CalculatorLogImpl`)

```java
    @Test
    public void testStaticProxy() {
        CalculatorStaticProxy calculatorStaticProxy = new CalculatorStaticProxy(new CalculatorImpl());
        calculatorStaticProxy.add(1, 2);
        calculatorStaticProxy.sub(1, 2);
        calculatorStaticProxy.mul(1, 2);
        calculatorStaticProxy.div(1, 2);
    }
```

---

## 动态代理

使用动态代理

```java
public class DynamicProxy {
    /**
     *
     * @param target 被代理对象
     * @return 返回代理对象
     */
    public static Object getProxy(Object target) {
        ClassLoader classLoader = target.getClass().getClassLoader();
        Class<?>[] interfaces = target.getClass().getInterfaces();
        InvocationHandler invocationHandler = (proxy, method, args) -> {
            int a = (int) args[0], b = (int) args[1];
            System.out.println("[日志]参数为: " + a + ", " + b);
            Object result = method.invoke(target, args);
            System.out.println("[日志]结果为: " + result);
            return result;
        };
        return Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);
    }
}
```

代码一下就简洁了很多, 优雅!

```java
    @Test
    public void testCalculatorDynamicProxy() {
        Calculator proxy = (Calculator) DynamicProxy.getProxy(new CalculatorImpl());
        proxy.add(1, 2);
        proxy.sub(1, 2);
        proxy.mul(1, 2);
        proxy.div(1, 2);
    }
```



---

## 函数式编程

还有种方式, 函数式编程也可实现

```java
import java.util.function.IntBinaryOperator;

public class FunctionalCode {
    /**
     *
     * @param calculator 接受两个参数同为类型int,返回值类型也为int 。
     * @param a 参数1
     * @param b 参数2
     * @return 返回计算结果
     */
    public static int calculateLog(IntBinaryOperator calculator, int a, int b) {
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = calculator.applyAsInt(a, b);
        System.out.println("[日志]结果为: " + result);
        return result;
    }
}
```

测试, 需传入具体使用的方法, 将方法参数化

```java
    @Test
    public void testFunctional() {
        Calculator calculator = new CalculatorImpl();
        FunctionalCode.calculateLog(calculator::add, 1, 2);
        FunctionalCode.calculateLog(calculator::sub, 1, 2);
        FunctionalCode.calculateLog(calculator::mul, 1, 2);
        FunctionalCode.calculateLog(calculator::div, 1, 2);
    }
```



---

## Spring的Aspects

Spring的AOP其实也是使用的动态代理的方式实现

需导入依赖`aspects`, `aop`在`context`包中带有

```xml
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.version}</version>
        </dependency>
```

日志切面类

```java
package org.example.aop;

@Aspect  // 切面类
@Component
public class LogAspect {
    /**
     * 切入点表达式: org.example.Calculator下的所有方法
     * 声明切入点表达式: 通过@Pointcut注解声明切入点表达式
     * 便于复用切面表达式
     */
    @Pointcut("execution(public int org.example.Calculator.*(..))")
    public void pointcut() {
    }
	// 环绕通知
    @Around("pointcut()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        Object[] args = pjp.getArgs();
        int a = (int) args[0], b = (int) args[1];
        System.out.println("[日志]参数为: " + a + ", " + b);
        int result = (int) pjp.proceed();
        System.out.println("[日志]结果为: " + result);
        return result;
    }
}
```

配置类

```java
@ComponentScan("org.example.aop")  // 扫描LogAspect类所在包
@Configuration
@EnableAspectJAutoProxy  // 开启代理
public class AppConfig {
    // 将CalculatorImpl注册为Bean对象
    @Bean
    public Calculator calculator(){
        return new CalculatorImpl();
    }
}
```

测试

```java
    @Test
    public void testSpringAspect(){
        ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
        Calculator calculator = ctx.getBean("calculator", Calculator.class);
        calculator.add(1, 2);
        calculator.sub(1, 2);
        calculator.mul(1, 2);
        calculator.div(1, 2);
    }
```

或者

```java
@SpringJUnitConfig(classes = AppConfig.class)  // Spring整合Junit5
// 等同于下面两个注解
// @ExtendWith(SpringExtension.class)
// @ContextConfiguration(classes = {AppConfig.class})
public class SpringTest {
    @Autowired
    private Calculator calculator;

    @Test
    public void testSpringAspect() {
        calculator.add(1, 2);
        calculator.sub(1, 2);
        calculator.mul(1, 2);
        calculator.div(1, 2);
    }
}
```





---

可以看出, 一种需求的实现方式是有很多种的, 根据实际情况选择更适合的