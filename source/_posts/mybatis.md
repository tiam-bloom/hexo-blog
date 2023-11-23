---
title: MyBatis
cover: >-
  http://qiniu.yujing.fit/image/nice/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20201216212306.jpg
tags: SpringBoot
id: '313'
categories:
  - spring boot
abbrlink: 1bff3c65
date: 2021-09-17 22:03:02
---

以Maven方式创建文件

## 1 搭建目录结构,如下:

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-50.png)

## 2 引入数据库Customer表

```sql
CREATE DATABASE mybatis; 
USE mybatis; 
create table `t_customer` (
`id` int (32),
`username` varchar (150),
`jobs` varchar (150),
`phone` varchar (48)
); 
insert into `t_customer` (`id`, `username`, `jobs`, `phone`) values('1','joy','doctor','13745874578');
insert into `t_customer` (`id`, `username`, `jobs`, `phone`) values('2','jack','doctor','17623194363');
insert into `t_customer` (`id`, `username`, `jobs`, `phone`) values('3','qqq','programmer','13311111111');
```

## 3 在pom.xml文件中导入所需的MyBatis依赖包

配置后会自动下载导入

```xml
<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!--mybatis所需的jar包-->
        <dependency>
            <groupId>asm</groupId>
            <artifactId>asm</artifactId>
            <version>3.3.1</version>
        </dependency>

        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
            <version>2.2.2</version>
        </dependency>

        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.1.1</version>
        </dependency>
        <dependency>
            <groupId>org.javassist</groupId>
            <artifactId>javassist</artifactId>
            <version>3.17.1-GA</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.7</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.5</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.3.0</version>
        </dependency>
    </dependencies>
```

## 4 MyBatis-config.xml

连接数据库,配置Mapper的位置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis" />
                <property name="username" value="root" />
                <property name="password" value="123456" />
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="mapper/CustomerMapper.xml" />
    </mappers>
</configuration>
```

## 5 映射文件 CustomerMapper.xml

创建所需的SQL语句

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mapper.CustomerMapper">
    <select id="findCustomerById" parameterType="Integer"
            resultType="com.example.po.Customer">
select * from t_customer where id = #{id}
</select>
    <select id="findCustomerByName" parameterType="String"
            resultType="com.example.po.Customer">
        select * from t_customer where username like '%${value}%'
    </select>
    <insert id="addCustomer"  parameterType="com.example.po.Customer">
        insert into t_customer(username,jobs,phone)
        values(#{username},#{jobs},#{phone})
    </insert>
    <update id="updateCustomer" parameterType="com.example.po.Customer">
       update t_customer set
       username=#{username},jobs=#{jobs},phone=#{phone}
       where id=#{id}
    </update>
    <delete id="deleteCustomer" parameterType="Integer">
       delete from t_customer where id=#{id}
    </delete>

</mapper>
```

## 6 创建Customer表的持久化类

快捷生成getter and setter方法 和 to String()方法,略.

```java
    private Integer id;
    private String username;
    private String jobs;
    private String phone;
```

## 7 导入日志文件log4j.properties

输出日志信息,在控制台查看SQL语句

```properties
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
log4j.logger.mapper=DEBUG  
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

![](http://47.101.172.219/wp-content/uploads/2021/09/图片-52-1024x410.png)

## 8 创建测试文件

除查询以外,其他需要提交事务 sqlSession.commit();

```java
import com.example.po.Customer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class MybatisTest {
    @Test
    public void CustomerTest() throws Exception {
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory =
                new SqlSessionFactoryBuilder().build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //添加数据
//        Customer customerIn = new Customer();
//        customerIn.setUsername("rose");
//        customerIn.setJobs("student");
//        customerIn.setPhone("13333533092");
//        int rows = sqlSession.insert("mapper.CustomerMapper.addCustomer", customerIn);
//        if (rows>0){
//            System.out.println("插入了"+rows+"条数据");
//        }else{
//            System.out.println("插入失败!");
//        }
//        sqlSession.commit();//提交事务
        //查询
        //Customer customerId = sqlSession.selectOne("mapper.CustomerMapper.findCustomerById", 3);//查询Id
        //List<Customer> customerName = sqlSession.selectList("mapper.CustomerMapper.findCustomerByName", "j");//模糊查询姓名
        //System.out.println(customerId.toString());
        //System.out.println(customerName.toString());

        //更新
//        Customer customerUp = new Customer();
//        customerUp.setId(3); //根据Id修改数据
//        customerUp.setUsername("qqq");
//        customerUp.setJobs("programmer");
//        customerUp.setPhone("13311111111");
//        int row = sqlSession.update("mapper.CustomerMapper.updateCustomer", customerUp);
//        if (row>0){
//            System.out.println("更新了"+row+"条数据");
//        }else{
//            System.out.println("更新失败!");
//        }
//        sqlSession.commit();

        //删除
        int del = sqlSession.delete("mapper.CustomerMapper.deleteCustomer", 26);
        if (del > 0) {
            System.out.println("删除了" + del + "条数据");
        } else {
            System.out.println("删除失败!");
        }
        sqlSession.commit();



        sqlSession.close();
    }
}
```

## 9 运行测试

因没有Id为26的,所以删除失败.
