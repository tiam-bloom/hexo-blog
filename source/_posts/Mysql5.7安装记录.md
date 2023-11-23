---
title: Mysql5.7安装记录
tags:
  - mysql
categories:
  - 教程
abbrlink: b7c158be
date: 2023-03-20 13:03:04
---



## Mysql5.7安装记录

【2023-3-5 19:26:15 星期日】

下载地址: [MySQL :: Download MySQL Community Server](https://dev.mysql.com/downloads/mysql/5.7.html)

下载后应是一个`zip`的压缩文件, 然后解压在某个路径下，我是放在``F:\Program\mysql-5.7.41-winx64`路径下

> 注意路径中不可有**中文**和**空格**!

<img src="https://qiniu.yujing.fit/typora_img/image-20230305183451849.png" alt="image-20230305183451849" style="zoom: 33%;" />

一、解压后，在根目录下新建一个名为`my.ini`配置文件（注意文件后缀是`ini`)

二、填充以下内容，注意根据自己所解压的目录，更改内容。

```ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录   ----------是你的文件路径-------------
basedir=D:\MySQL_Navicat\mysql-8.0.27-winx64
# 设置mysql数据库的数据的存放目录  ---------是你的文件路径data文件夹自行创建
# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
datadir=D:\Mysql\mysql\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
# 服务端使用的字符集默认为utf8mb4
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
#mysql_native_password
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```



## 操作实例

三、按照以下操作

> 说明: 因还未配置环境变量，所以必须在`bin`目录下执行命令，还需在前面加上`.\`表示当前路径下的命令（有的可能直接在当前路径下就行）

```shell
# 1. 切换路径到bin目录下(必须以管理员方式运行终端, 否则后面会因无权限导致失败)
PS C:\Windows\system32> F:
PS F:\> cd .\Program\
PS F:\Program> cd .\mysql-5.7.41-winx64\
PS F:\Program\mysql-5.7.41-winx64> cd bin
# 2. 初始化, 获取默认密码
PS F:\Program\mysql-5.7.41-winx64\bin> .\mysqld --initialize --console
# 此处应有一些内容（忘记复制了）, 其中包含默认密码，在最后一行，注意查看！！！

# 3. 安装mysql服务并启动
PS F:\Program\mysql-5.7.41-winx64\bin> .\mysqld --install mysql
Service successfully installed.
PS F:\Program\mysql-5.7.41-winx64\bin> net start mysql
mysql 服务正在启动 .
mysql 服务已经启动成功。

# 4. 连接MySQL
PS F:\Program\mysql-5.7.41-winx64\bin> .\mysql -uroot -p
Enter password: ************  # 输入刚刚获取的默认密码
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.41

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
# 5. 修改密码为123456
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.00 sec)
# 6. 退出
mysql> exit
Bye
PS F:\Program\mysql-5.7.41-winx64\bin>
```

## 配置系统环境变量

> 配置环境变量的目的是为了可以在任意目录下执行命令，原本只能在`bin`目录下。（可能需要重启才会生效，未生效的话，试试重启电脑）

四、在**系统**环境变量中，新建系统变量，变量名：`MYSQL_HOME`，变量值：`F:\Program\mysql-5.7.41-winx64`，（这里以自己所在的路径为准）

五、在**系统**环境变量中，找到变量名为`Path`的一项，点击编辑，新建值：`%MYSQL_HOME%\bin`



六、（*此项不必须*）可在服务中，更改mysql启动类型为手动启动，避免拖慢电脑。（可能会导致后面有些时候连接不上，注意查看mysql服务是否已启动）

<img src="http://qiniu.yujing.fit/typora_img/image-20230305190523724.png" alt="image-20230305190523724" style="zoom: 50%;" />

## 配合数据库管理工具navicat

相关文章：[MySQL - navicat16 安装与破解 - 简书 (jianshu.com)](https://www.jianshu.com/p/9c4c499429da)

<img src="http://qiniu.yujing.fit/typora_img/image-20230305191006297.png" alt="image-20230305191006297" style="zoom: 50%;" />