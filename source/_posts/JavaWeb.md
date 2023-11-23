---
title: JavaWeb
description: JavaWeb课程相关笔记
tags: JavaWeb
abbrlink: 99720b1c
date: 2022-11-19 08:04:15
---

# JavaWeb

- 2022/11/18 

## 实现Servlet的三种方式

### 1. 继承GenericServlet

```java
public class ServletDemo3 extends GenericServlet {
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("GenericServlet实现Servlet");
    }
}
```

### 2. 实现Servlet类

```java
public class ServletDemo2 implements Servlet {
    private ServletConfig servletConfig;

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("初始化。。。");
        this.servletConfig = servletConfig;
    }

    @Override
    public ServletConfig getServletConfig() {
        System.out.println("获取配置中。。。");
        return this.servletConfig;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        ServletContext servletContext = getServletConfig().getServletContext();
        String value = (String)servletContext.getAttribute("key");
        System.out.println(value);
        System.out.println("处理业务中。。。");
    }

    @Override
    public String getServletInfo() {
        System.out.println("获取servlet信息");
        return null;
    }

    @Override
    public void destroy() {
        System.out.println("我要被销毁了");
    }
}
```

### 3. 继承HttpServlet(推荐)

```java
public class ServletDemo1 extends HttpServlet {
    private ServletConfig servletConfig;
    @Override
    public void init(ServletConfig config) throws ServletException {
        this.servletConfig = config;
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 防止中文乱码
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        writer.println("Hello world 你好, 世界\n");
        String state = servletConfig.getInitParameter("state");
        writer.println("初始化参数:"+state);
        //遍历初始化参数
        Enumeration<String> initParameterNames = servletConfig.getInitParameterNames();
        while (initParameterNames.hasMoreElements()){
            String key = initParameterNames.nextElement();
            String value = servletConfig.getInitParameter(key);
            writer.println("遍历:"+key+" "+value);
        }
        // 获取ServletContext对象
        // 方式一:
        ServletContext servletContext1 = servletConfig.getServletContext();
        // 方式二:
        ServletContext servletContext2 = this.getServletContext();
        // 共享数据
        servletContext2.setAttribute("key","value");
        writer.close();
    }
}
```

## Cookie

```java
/**
 * @Author Tiam
 * @Date 2022/11/4 16:34
 * @Description: 客户端技术, 把数据以Cookie的形式返回用户的浏览器
 * cookie 使用实例: 上次访问时间
 */
@WebServlet("/cookie")
public class CookieDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 防止中文乱码
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");
        // 获取cookie
        Cookie[] cookies = req.getCookies();
        if(cookies!=null){
            for (Cookie cookie : cookies) {
                if(cookie.getName().equals("visitedTime")){
                    long time = Long.parseLong(cookie.getValue());
                    Date date = new Date(time);
                    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss E");
                    resp.getWriter().println("您上次访问本站的时间是:"+simpleDateFormat.format(date));
                }
                if (cookie.getName().equals("name")) {
                    resp.getWriter().println("您的用户名是 "+cookie.getValue());
                }
            }
        }else {
            resp.getWriter().println("这是您第一次访问本站!");
        }
        // 创建Cookie
        Cookie cookie = new Cookie("visitedTime", String.valueOf(System.currentTimeMillis()));
        // 发送Cookie到客户端
        resp.addCookie(cookie);
        //测试存储中文
        Cookie cookie1 = new Cookie("name", "陈思宇");
        // 设置一天的保存时间
        cookie1.setMaxAge(60*60*24);
        resp.addCookie(cookie1);
        // 删除cookie
        cookie1.setMaxAge(0);
        resp.addCookie(cookie1);
    }

}
```

## Session

```java
package session;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

/**
 * @Author Tiam
 * @Date 2022/11/4 17:29
 * @Description:特点: 存储一次会话的多次请求的数据
 */
public class SessionDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取Session
        HttpSession session = req.getSession();
        // 设值
        session.setAttribute("key","value");
        // 取值
        session.getAttribute("key");
        // 删除
        session.removeAttribute("key");
    }
}
```

## 域属性范围 | EL表达式

- el表达式只能从四大域中获取值
- el表达式 调用`pageContext.findAttribute`方法
- 找不到返回空字符串, 而不是null

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/17
  Time: 11:11
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    // 只使用一次的数据
    request.setAttribute("name2","request");
    // 只在当前页有效
    pageContext.setAttribute("name3","pageContext");
    // 会再次使用的数据
    session.setAttribute("name4","session");
    // 几乎所有情况都有效
    application.setAttribute("name1", "application");
%>
<h1>${name1}</h1>
<h1>${name2}</h1>
<h1>${name3}</h1>
<h1>${name4}</h1>
</body>
</html>
```

## JSP语法

```jsp
<%@ page import="java.util.Date" %><%--
  Created by IntelliJ IDEA.
  User: admin
  Date: 2022/10/28
  Time: 16:49
  To change this template use File | Settings | File Templates.
--%>

<%--java指令--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>JSP语法学习 2022/11/11</title>
</head>
<body>
<a href="applicationDemo.jsp">applicationDemo.jsp</a>
<h1>1. Jsp表达式</h1>
<%= new Date() %>
<h1>2. JSP脚本片段</h1>
<%
    int num = 1;
    for (int i = 0; i < 100; i++) {
        out.write("\n" + (num += i));
    }
%>
<h1>3. 组合的脚本片段</h1>
<%
    num = 0;
    if (1 + 1 == 2) {
%>
<p> html标签 </p>
<%
        out.write("1+1=2");
    }
%>
<h1>4. jsp声明</h1>
<%!
    private int sum(int k) {
        int sum = 0;
        for (int i = 0; i < k; i++) sum += i;    
        return sum;
    }
    int he = sum(100);
    private void print(Object o) {
        System.out.println(o);
    }
%>

<%
    print("云边有个小卖部");
%>

<h1>5. jsp的注释</h1>
<!--html的注释-->

<%
    // java单行注释
    /* java 多行注释 */
    /**
     * javadoc风格注释
     */
%>

<%--jsp注释风格--%>

<h1>6. jsp指令</h1>
<%--导入包, 每个包以逗号隔开--%>
<%@page import="java.util.List" %>

<%--将其他文件内容包含进来--%>
<%@include file="login.jsp" %>
<jsp:include page="login.jsp"/>

<%--指定错误处理页面--%>
<%@page errorPage="loginWithCode.jsp" %>
</body>
</html>
```

## JSP标签

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/17
  Time: 11:20
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>JSP标签</title>
</head>
<body>
<%-- 引入页面 三种方式--%>
<jsp:include page="applicationDemo.jsp" flush="true"/>
<%@include file="applicationDemo.jsp" %>
<%
    pageContext.include("./applicationDemo.jsp");
%>
<%--页面跳转--%>
<jsp:forward page="applicationDemo.jsp"/>

<%--引入参数--%>
<jsp:include page="applicationDemo.jsp">
    <jsp:param name="param1" value="Hello"/>
</jsp:include>

<jsp:forward page="applicationDemo.jsp">
    <jsp:param name="param2" value="Good"/>
</jsp:forward>
</body>
</html>
```

## PageContext

```jsp
<%@ page import="java.text.SimpleDateFormat" %>
<%@ page import="java.util.Date" %>
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/17
  Time: 10:21
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--跳转--%>
<%--<jsp:forward page="index.jsp"/>--%>
<html>
<head>
    <title>pageContextDemo</title>
</head>
<body>
<h1>out对象输出</h1>
<% pageContext.getOut().write("pageContext.getOut().write(\"\");"); %>

<h1>pageContext作为域对象</h1>
<%
    pageContext.setAttribute("name", "陈思宇");
    pageContext.setAttribute("age", 18);

    String name = (String) pageContext.getAttribute("name");
    // pageContext.getAttribute("age");
%>
<p>姓名: <%=name%>
</p>
<p>年龄: ${age}
</p>
<p>年龄: <%=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss E").format(new Date())%>
</p>
<%
    // 引入页面
    pageContext.include("./login.jsp");
    // 跳转失效
    pageContext.forward("./index.jsp");
%>
</body>
</html>
```

## Servlet验证码

### loginWithCode.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/4
  Time: 17:42
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>验证码登录</title>
    <script>
        function changeImg() {
            document.getElementById("validateCodeImg").src = "generateCode?"+Math.random();
        }
    </script>
</head>
<body>
<form action="/checkCode" method="post">
    验证码:<input type="text" name="validateCode">
    <img src="/generateCode" alt="看不清,换一张" id="validateCodeImg" onclick="changeImg()">
    <a href="javascript:void(0)" onclick="changeImg()">看不清,换一张</a>
    <input type="submit" value="提交">
</form>
</body>
</html>
```

### GenerateCode.java

```java
package loginCode;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.HashMap;
import java.util.Random;

/**
 * @Author Tiam
 * @Date 2022/11/4 17:48
 * @Description: 生成验证码图片
 */
@WebServlet("/generateCode")
public class GenerateCode extends HttpServlet {
    private static final int WIDTH = 120;
    private static final int HEIGHT = 30;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1. 生成图片
        BufferedImage bufferedImage = new BufferedImage(WIDTH, HEIGHT, 1);
        // 2. 获取图片
        Graphics graphics = bufferedImage.getGraphics();
        // 3. 设置图片背景
        setBackground(graphics);
        // 4. 设置边框
        setBorder(graphics);
        // 5. 设置干扰线
        setRandomLine(graphics);
        // 6. 生成验证码字符串
        String randomString = getRandomString((Graphics2D) graphics, "mixed");
        System.out.println("本次生成的验证码是: " + randomString);
        // 7. 将验证码保存到Session
        req.getSession().setAttribute("checkCode", randomString);
        // 8. 设置浏览器以图片的格式获取验证码
        resp.setContentType("image/jpg");
        // 9. 将验证码发送给浏览器
        ImageIO.write(bufferedImage, "jpg", resp.getOutputStream());
    }

    /**
     * @param graphics
     * @param type     mixed => 混合验证码, number => 数字验证码, letter => 字母验证码
     * @return
     */
    private String getRandomString(Graphics2D graphics, String type) {
        graphics.setColor(Color.RED);
        graphics.setFont(new Font("宋体", Font.BOLD, 20));
        // 字符串类型
        HashMap<String, String> map = new HashMap<>(3);
        map.put("number", "0123456789");
        map.put("letter", "QWERTYUIOPASDFGHJKLZXCVBNM");
        map.put("mixed", "1234567890QWERTYUIOPASDFGHJKLZXCVBNM");

        String randomString = setRandomString(graphics, map.get(type));
        return randomString;
    }

    private String setRandomString(Graphics2D graphics, String s) {
        StringBuilder sb = new StringBuilder();
        // 字符倾斜角度
        int x = 5;
        String chars = "";
        // 字符串长度
        int num = 4;
        for (int i = 0; i < num; i++) {
            // 定义角度
            int degree = new Random().nextInt() % 30;
            chars = s.charAt(new Random().nextInt(s.length() - 1)) + "";
            sb.append(chars);

            graphics.rotate(degree * Math.PI / 180, x, 20);
            graphics.drawString(chars, x, 20);
            x += 30;
        }
        return sb.toString();
    }

    /**
     * 设置图片干扰线
     *
     * @param graphics
     */
    private void setRandomLine(Graphics graphics) {
        graphics.setColor(Color.GREEN);

        // 干扰线数量
        int num = 5;
        for (int i = 0; i < num; i++) {
            // 随机生成两个点(x1,y1), (x2,y2), 将之连成线
            int x1 = new Random().nextInt(WIDTH);
            int y1 = new Random().nextInt(HEIGHT);

            int x2 = new Random().nextInt(WIDTH);
            int y2 = new Random().nextInt(HEIGHT);

            graphics.drawLine(x1, y1, x2, y2);
        }

    }

    /**
     * 设置图片背景
     *
     * @param graphics
     */
    private void setBackground(Graphics graphics) {
        graphics.setColor(Color.white);
        graphics.fillRect(0, 0, WIDTH, HEIGHT);
    }

    /**
     * s设置图片边框
     *
     * @param graphics
     */
    private void setBorder(Graphics graphics) {
        graphics.setColor(Color.BLUE);
        graphics.drawRect(1, 1, WIDTH - 2, HEIGHT - 2);
    }
}
```

### CheckCode.java

```java
/**
 * @Author Tiam
 * @Date 2022/11/10 11:11
 * @Description:
 */
@WebServlet("/checkCode")
public class CheckCode extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");
        // 获取用户输入, 根据input的name属性, 获得用户输入的验证码
        String validateCode = req.getParameter("validateCode");
        System.out.println("获取用户输入 =>" + validateCode);
        // 获取Session中的字符串
        String checkCode = (String) req.getSession().getAttribute("checkCode");
        if (validateCode.equalsIgnoreCase(checkCode)) {
            resp.getWriter().println("验证码正确");
        } else {
            resp.getWriter().println("验证输入有误, 请重新输入");
        }
    }
}
```

## Servlet模拟登录

### login.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/3
  Time: 11:23
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录页面</title>
</head>
<body>
<form action="/LoginServlet" method="post">
    账号:<input name="name" type="text">
    密码:<input name="pwd" type="password">
    <input type="submit" value="登录">
</form>
</body>
</html>
```

### LoginServlet.java

```java
/**
 * @Author Tiam
 * @Date 2022/11/3 11:22
 * @Description: 处理登录请求
 */
@WebServlet("/LoginServlet")
public class LoginServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 防止中文乱码
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");
        // 获取用户输入的账号密码
        String name = req.getParameter("name");
        String pwd = req.getParameter("pwd");
        // 模拟正确的账号密码
        Map<String,String> real  = new HashMap<>(2);
        real.put("name","admin");
        real.put("pwd","123");
        if(real.get("name").equals(name) && real.get("pwd").equals(pwd)){
            // 转发 -> 浏览器中地址不改变
            req.getRequestDispatcher("/index.jsp").forward(req,resp);
            // 重定向 -> 浏览器中地址改变
            // resp.sendRedirect("/index.jsp");
            System.out.println("登录成功");
        }else {
            PrintWriter writer = resp.getWriter();
            writer.println("账号或密码错误!");
        }
    }
}
```

## 简易计算器

### calculator.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/18
  Time: 16:28
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<jsp:useBean id="calculatorBean" class="el.Calculator"/>
<%--发送参数, 通配符* 表示全部参数 --%>
<jsp:setProperty name="calculatorBean" property="*"/>
<%
    // 调用计算方法
    calculatorBean.calculator();
%>
<html>
<head>
    <title>简易计算器 JSP+JavaBean开发模式样例</title>
</head>
<body>
<form action="calculator.jsp" method="post">
    <table border="1px">
        <tr>
            <td colspan="2">简易计算器</td>
        </tr>
        <tr>
            <td>first parameter</td>
            <td><input type="text" name="firstParam"/></td>
        </tr>
        <tr>
            <td>operator</td>
            <td>
                <select name="operator">
                    <option name="+">+</option>
                    <option name="-">-</option>
                    <option name="*">*</option>
                    <option name="/">/</option>
                </select>
            </td>
        </tr>
        <tr>
            <td>second parameter</td>
            <td><input type="text" name="secondParam"/></td>
        </tr>
        <tr>
            <td colspan="2"><input type="submit" value="计算"/></td>
        </tr>
    </table>
    <hr/>
    <p>计算结果是:
        <jsp:getProperty name="calculatorBean" property="firstParam"/>
        <jsp:getProperty name="calculatorBean" property="operator"/>
        <jsp:getProperty name="calculatorBean" property="secondParam"/>
        =
        <jsp:getProperty name="calculatorBean" property="result"/>
    </p>
</form>
</body>
</html>
```

### Caculator.java

```java
package el;

import lombok.Data;

/**
 * @Author Tiam
 * @Date 2022/11/18 16:37
 * @Description: 简易计算结果集
 */
@Data
public class Calculator {
    private double firstParam;
    private double secondParam;
    private String operator = "+";
    private double result;

    public void calculator() {
        switch (this.operator) {
            case "+":
                this.result = this.firstParam + this.secondParam;
                break;
            case "-":
                this.result = this.firstParam - this.secondParam;
                break;
            case "*":
                this.result = this.firstParam * this.secondParam;
                break;
            case "/":
                if (this.firstParam == 0) throw new RuntimeException("被除数不能为0");
                this.result = this.firstParam / this.secondParam;
                break;
            default:
                throw new RuntimeException("运算符异常");
        }
    }
}
```

## Tomcat 乱码问题

- 打开在安装目录下的`conf`文件夹中的`logging.properties`文件
- 更改这几个编码格式
- server日志正常，打印正常，（全部更改为GBK将会导致打印乱码）
- TomcatLocalhostLog 正常
- TomcatcatalinaLog 正常

```properties
1catalina.org.apache.juli.FileHandler.encoding = GBK
2localhost.org.apache.juli.AsyncFileHandler.encoding = GBK
3manager.org.apache.juli.AsyncFileHandler.encoding = UTF-8
4host-manager.org.apache.juli.AsyncFileHandler.encoding = UTF-8
java.util.logging.ConsoleHandler.encoding = GBK
```

## JSTL标签

### catch

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/25
  Time: 16:47
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<jsp:useBean id="person" class="hbnu.week_1.Person" />
<html>
<head>
    <title>catch 标签</title>
</head>
<body>
<c:catch var="errorInfo">
    <%--    未导入person => 异常--%>
    <c:set target="${person}" property="name" value="张三"/>
    <c:out value="person.name"/>
</c:catch>
<h3>异常信息:</h3>
<li><c:out value="${errorInfo}"/></li>
<li><c:out value="${errorInfo.printStackTrace()}"/></li>
<li><c:out value="${errorInfo.toString()}"/></li>
</body>
</html>
```

### choose

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/25
  Time: 17:14
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>choose 多条件选择标签</title>
</head>
<body>
    <c:set var="score" value="88"/>
    <h1>只会执行一个, 遇到第一个符合条件的不再执行执行后面的</h1>
    <c:choose>
        <c:when test="${score>=90}">
            <c:out value="优秀"/>
        </c:when>
        <c:when test="${score>=80}">
            <c:out value="良好"/>
        </c:when>
        <c:when test="${score>=70}">
            <c:out value="一般"/>
        </c:when>
        <c:when test="${score>=60}">
            <c:out value="还行"/>
        </c:when>
        <c:otherwise>
            <c:out value="成绩不合格, 请回考试中心重新打印成绩"/>
        </c:otherwise>
    </c:choose>
</body>
</html>
```

### forEach ***

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/25
  Time: 17:25
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>forEach 遍历标签</title>
</head>
<body>
<h1>forEach 遍历标签</h1>
- name 从集合中取出的元素 (必须)
- items 要遍历的集合 (必须)
- state 元素在集合中的信息.
<c:forEach var="name" items="Collections" varStatus="state" begin="begin" end="end" step="step" >
    <c:out value="name" />
</c:forEach>

</body>
</html>
```

### remove

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/25
  Time: 16:42
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>JSTL remove</title>
</head>
<body>
<h3>remove 标签</h3>
<%-- 默认page域--%>
<c:set var="name" value="陈思宇"/>

<li><c:out value="${name}"/> </li>

<c:remove var="name" />

<li><c:out value="${name}" default="已被移除显示 默认值"/> </li>
</body>
</html>
```

### set

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/11/25
  Time: 16:25
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<jsp:useBean id="person" class="hbnu.week_1.Person" />
<html>
<head>
    <title>JSTL set标签</title>
</head>
<body>
<h3>将数据存入指定域中</h3>
<c:set var="username" value="陈思宇" scope="request"/>
<li><c:out value="${username}"/></li>

<h3>将数据存入bean中</h3>
<c:set target="${person}" property="name">张三</c:set>
<c:set target="${person}" property="age" value="18" />
<li><c:out value="${person.name}"/></li>
<li><c:out value="${person.age}"/></li>
</body>
</html>
```

## 遍历数据到jsp

`SelectAllUserServlet.java`

```java
@WebServlet("/findAllUser")
public class SelectAllUserServlet extends HttpServlet {
    private UserService userService = new UserServiceImpl();
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取所有用户信息
        List<User> users = userService.findAllUser();
        // 存入request域, forward跳转可访问, a标签跳转不可访问
        req.setAttribute("users",users);
        req.getRequestDispatcher("/hbnu/week_2/userInfo.jsp").forward(req,resp);
    }
}
```

`userInfo.jsp`

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 30362
  Date: 2022/12/1
  Time: 10:13
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>用户信息</title>
</head>
<body>
<table align="center" border="1px solid black">
    <tr>
        <th>序号</th>
        <th>账户</th>
        <th>密码</th>
        <th>邮箱</th>
    </tr>
    <%--从Request域中取出数据, 并遍历--%>
    <jsp:useBean id="users" scope="request" type="java.util.List"/>
    <c:forEach var="user" items="${users}" varStatus="status" >
        <tr align="center">
            <td>${status.count}</td>
            <td>${user.name}</td>
            <td>${user.pwd}</td>
            <td>${user.email}</td>
        </tr>
    </c:forEach>
</table>
</body>
</html>
```



## 数据库连接工具类 | 数据池

### db.properties

```properties
driverClassName = com.mysql.cj.jdbc.Driver
url = jdbc:mysql://localhost:3306/jdbcdemo?serverTimezone=GMT&useSSL=false&characterEncoding=utf-8
username = root
password = 123456
```

### JDBCUtils

- `mysql-connector-j-8.0.31.jar`

```java
import java.sql.*;
import java.util.ResourceBundle;

public class JDBCUtils {
    // 获取配置文件
    static ResourceBundle bundle;
    static String driverClass;
    static String url;
    static String username;
    static String password;

    static {
        bundle = ResourceBundle.getBundle("配置文件路径 \\ jdbc"); // 无文件后缀
        driverClass = bundle.getString("driverClassName");
        url = bundle.getString("url");
        username = bundle.getString("username");
        password = bundle.getString("password");
        try {
            Class.forName(driverClass);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, username, password);
    }

    public static void close(ResultSet rs, Statement st,Connection co){
        try {
            if(rs!=null) rs.close();
            if(st!=null) st.close();
            if(co!=null) co.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### C3P0Utils

- `c3p0-0.9.1.2.jar`

```java
package hbnu.week_2;

import java.beans.PropertyVetoException;
import java.sql.*;

import com.mchange.v2.c3p0.ComboPooledDataSource;

import javax.sql.DataSource;

/**
 * @Author Tiam
 * @Date 2022/12/1 10:10
 * @Description:
 */
public class C3P0Util {
    public static final String NAME = "root";
    public static final String PWD = "123456";
    public static final String URL = "jdbc:mysql://localhost:3306/java_design_20221102";
    public static final String DRIVER = "com.mysql.cj.jdbc.Driver";
    public static ComboPooledDataSource c3p0;

    static {
        c3p0 = new ComboPooledDataSource();
    }


    public static Connection getConnection() throws SQLException, PropertyVetoException {
        c3p0.setDriverClass(DRIVER);
        c3p0.setJdbcUrl(URL);
        c3p0.setUser(NAME);
        c3p0.setPassword(PWD);
        return c3p0.getConnection();
    }

    public static void cloes(ResultSet rs, Statement st, Connection co) {
        try {
            if (rs != null) rs.close();
            if (st != null) st.close();
            if (co != null) co.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### DruidUtils

- `druid-1.1.10.jar`

```java
package hbnu.week_2;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.sql.*;
import java.util.HashMap;
import java.util.Map;

/**
 * @Author Tiam
 * @Date 2022/12/1 10:10
 * @Description:
 */
public class DruidUtil {
    private static final String NAME = "root";
    private static final String PWD = "123456";
    private static final String URL = "jdbc:mysql://localhost:3306/java_design_20221102";
    private static final String DRIVER = "com.mysql.cj.jdbc.Driver";

    private static DataSource dataSource;

    static {
        Map<String, String> map = new HashMap<>(4);
        // 键 固定
        map.put("username", NAME);
        map.put("password", PWD);
        map.put("url", URL);
        map.put("driverClassName", DRIVER);
        try {
            dataSource = DruidDataSourceFactory.createDataSource(map);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() throws Exception {
        return dataSource.getConnection();
    }

    public static void cloes(ResultSet rs, Statement st, Connection co) {
        try {
            if (rs != null) rs.close();
            if (st != null) st.close();
            if (co != null) co.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### DAO

```JAVA
    @Override
    public List<User> findAllUser() {
        Connection con = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            con = DruidUtil.getConnection();
            String sql = "select * from User";
            ps = con.prepareStatement(sql);
            rs = ps.executeQuery();
            // 封装为集合返回
            List<User> res = new ArrayList<>();
            while (rs.next()) {
                // 根据数据库表中的字段名获取其对应的数据
                res.add(new User(rs.getString("username"), rs.getString("password"), rs.getString("email")));
            }
            return res;
        } catch (SQLException | PropertyVetoException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 释放资源
            DruidUtil.cloes(rs,ps,con);
        }
        return null;
    }
```



## Filter

- web.xml中注册

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <filter>
        <filter-name>FilterUser</filter-name>
        <filter-class>hbnu.week_2.filter.FilterUser</filter-class>
    </filter>
    <!--    配置过滤器拦截规则-->
    <filter-mapping>
        <filter-name>FilterUser</filter-name>
<!-- 直接访问该路径将会被拦截, 转发跳转则不会被拦截-->
        <url-pattern>/hbnu/week_2/userInfo.jsp</url-pattern>
    </filter-mapping>

<!--    过滤器链-->

</web-app>
```

- 或者使用注解的方式注册

```java
package hbnu.week_2.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

/**
 * @Author Tiam
 * @Date 2022/12/2 16:57
 * @Description: 过滤器 javax.servlet下的包
 */

/**
 * 拦截目标路径, 不再需要在web.xml中注册
 * 通过注解 注册的过滤器, 根据类名的字符串大小 比较进行 先后顺序执行
 */
@WebFilter("/hbnu/week_2/userInfo.jsp")
public class FilterUser1 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("过滤器初始化...");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        // 放行
        // filterChain.doFilter(servletRequest, servletResponse);
        System.out.println(servletRequest);
        System.out.println(servletResponse);
        System.out.println("请求被拦截.");
    }

    @Override
    public void destroy() {
        System.out.println("过滤器被销毁...");

    }
}

```

### 登录拦截

```java
import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

/**
 * @Author Tiam
 * @Date 2022/12/8 10:17
 * @Description:
 */
@WebFilter("/*")
public class FilterLogin implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest)servletRequest;
        HttpServletResponse resp = (HttpServletResponse)servletResponse;
        // todo 判断去向, 如果不是访问登录页面, 拦截验证是否登录, 
        HttpSession session = req.getSession();
        // 如果未登录
        if (session.getAttribute("user")== null) {
            // 重定向到登录页面
            resp.sendRedirect("/login.jsp");
        }else{
            // 放行
            filterChain.doFilter(servletRequest,servletResponse);
        }

    }

    @Override
    public void destroy() {

    }
}
```

## 监听器

- 事件源
- 监听器
- 注册监听器
- 响应行为

- 为每个web应用创建一个ServletContext对象

- web.xml中注册监听器

```xml
	<listener>
        <listener-class>hbnu.week_2.listener.ListenerDemo1</listener-class>
    </listener>
```

```java
package hbnu.week_2.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

/**
 * @Author Tiam
 * @Date 2022/12/8 10:31
 * @Description:
 */
@WebListener  // 直接使用注解注册
public class ListenerDemo1 implements ServletContextListener {
    @Override
    // 当ServletContext对象被 创建 时触发
    public void contextInitialized(ServletContextEvent servletContextEvent) {

    }

    @Override
    // 当ServletContext对象被 销毁 时触发
    public void contextDestroyed(ServletContextEvent servletContextEvent) {

    }
}
```

```java
package hbnu.week_2.listener;

import javax.servlet.ServletContextAttributeEvent;
import javax.servlet.ServletContextAttributeListener;
import javax.servlet.annotation.WebListener;

/**
 * @Author Tiam
 * @Date 2022/12/8 10:55
 * @Description: 监听Servlet域属性
 */
@WebListener
public class ListenerDemo2 implements ServletContextAttributeListener {
    @Override
    public void attributeAdded(ServletContextAttributeEvent servletContextAttributeEvent) {

    }

    @Override
    public void attributeRemoved(ServletContextAttributeEvent servletContextAttributeEvent) {

    }

    @Override
    public void attributeReplaced(ServletContextAttributeEvent servletContextAttributeEvent) {

    }
}
```

