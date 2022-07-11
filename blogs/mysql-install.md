+++
author = "Murphy"
title = "Win 10环境下安装MySQL 5.7"
date = "2022-05-23"

description = "Installation of MySQL 5.7 in Win 10"
tags = [
    "SQL",
]

categories = [
    "CS",
   ]

+++

记录在Win 10下安装、配置MySQL 5.7的过程。

<!--more-->

## 安装步骤

1. 官网下载安装包（https://dev.mysql.com/downloads/mysql/）。
   打开网站，默认显示的是最新的8.0版本；如果要下载5.7版本，需要打开Archives页签，将查询条件设置为5.7.36（目前5.7版本系列中最新的一个版本）。下载不包含测试套件的包即可。

2. 将zip包解压到本机某个目录下。
   例如： D:\mysql\mysql-5.7.36-winx64

3. 在该目录下手工创建my.ini文件，拷贝如下内容，并根据实际安装目录修改两个basedir和datadir。

   basedir配置为mysql安装目录，datadir在安装目录后加data。
   
   **注意：**此时data文件夹并不存在，在执行后续安装步骤时会由系统自动创建。
   
   ```ini
   [mysqld]
   #设置3306端口
   port=3306
   #设置mysql的安装目录
   basedir=D:\mysql\mysql-5.7.36-winx64
   #设置mysql数据库的数据的存放目录
   datadir=D:\mysql\mysql-5.7.36-winx64\data
   #允许最大连接数
   max_connections=200
   #允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
   max_connect_errors=10
   #服务端使用的字符集默认为UTF8
   character-set-server=utf8
   #创建新表时将使用的默认存储引擎
   default-storage-engine=INNODB
   #默认使用“mysql_native_password”插件认证
   default_authentication_plugin=mysql_native_password
   
   [mysql]
   #设置mysql客户端默认字符集
   default-character-set=utf8
   [client]
   #设置mysql客户端连接服务端时默认使用的端口
   port=3306
   default-character-set=utf8
   ```
   
   

4. 配置环境变量，为系统环境变量path添加一个值：MySQL安装目录下的bin目录。

   例如 D:\mysql\mysql-5.7.36-winx64\bin

5. 以管理员身份打开cmd窗口，执行如下命令初始化数据库：

   **注意：**一定要用管理员身份打开cmd窗口，否则后续的安装步骤可能因为权限问题异常。

   打开windows菜单，“开始 > Windows管理工具 > Windows系统 > 命令提示符”，单击鼠标右键，选择“更多 > 以管理员身份运行”。

   ```shell
   C:\Windows\system32> mysqld --initialize-insecure --user=mysql
   ```

   初始化完成后，检查MySQL根目录，会发现步骤3指定的data文件夹已创建成功。

   同时，系统会创建root用户（密码为空）。

6. 为windows安装mysql服务。

   ```shell
   C:\Windows\system32> mysqld -install
   ```

   系统提示“service successfully installed.”则代表配置成功。系统创建的默认服务名为mysql。

7. 启动数据库服务

   ```shell
   C:\Windows\system32> net start mysql
   ```

   

8. 以root用户登录mysql。

   ```
   C:\Windows\system32> mysql -u root -p
   ```

   因为初始化数据库时我们没有设置密码，所以这里系统提示输入密码时，直接回车即可。
   出现 mysql> 提示符，代表登录成功。

9. 为root设置密码。

   下面命令中引号内的部分即为要设置的密码，请自行修改。

   ```mysql
   mysql> alter user user() identified by "xxxpasswd";
   ```

   修改完成，退出登录。

   ```mysql
   mysql> quit
   ```

   

10. 使用新设置的密码再次登录mysql，查看系统默认的数据库。

    安装完毕，日常访问数据库时，可以用普通模式打开cmd窗口。

    ```mysql
    C:\Windows\system32> mysql -u root -p
    
    mysql> show databases;
    ```

    

11. 创建自己的数据库

    这里假设要创建的数据库名为“mypython”，请根据自己的需要自行修改。

    ```mysql
    mysql> create database mypython;
    ```

    创建完毕，仍然可以用show databases命令检查。

    至此，MySQL的安装和配置完成，后续可以在这个环境基础上创建数据库表，并向表中插入、修改、删除、查询数据。

    

## 其他

安装过程中如果有异常需要回退，可以使用以下命令和前面的对应。

- 删除数据库：

  ```mysql
  mysql> drop database mypython;
  ```

  

- 停止数据库服务

  ```shell
  C:\Windows\system32> net stop mysql
  ```

  

- 卸载服务

  ```shell
  C:\Windows\system32> mysqld -remove
  ```

  





   
