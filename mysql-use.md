+++
author = "Jasmine"
title = "最常用的MySQL语句"
date = "2022-3-30"

description = "SQL"
tags = [
    "MySQL",
]

categories = [
    "CS",
   ]

+++

整理MySQL最常用的SQL语句（建表、增删改查数据）。更多的命令可参考：https://www.runoob.com/mysql/mysql-select-database.html

<!--more-->

## 常用命令

执行下列SQL语句前，请参照上节内容登录MySQL环境。

1. 选择数据库（上节已创建数据库mypython）。
   
   ```mysql
   mysql> use mypython;
   ```
   
   系统提示“Database changed”代表操作成功。
   
2. 查看系统中默认的表。

   在我们创建数据库时，系统会默认创建一些表。使用如下命令可查看。

   ```mysql
   mysql> show tables;
   ```

3. 查看其他库的所有表。

   当已经在一个库中，想查看另外库的表，可用如下命令。

   ```mysql
   mysql>show tables from 库名； 
   ```

4. 使用create table语句，创建自己的表。

   下例的表名为students_info，他有id，name，gender，class四个字段，其中id为主键。

   ```mysql
   mysql> create table students_info(
   id varchar(10) not null, #列名 列类型
   name varchar(10) not null,
   gender varchar(10),
   class int,
   primary key (id)
    );
   ```

   下例的表名为test_scores，他有id，student_id，subject，score，submit_date四个字段，id为主键，且为自增长字段，即每次插入数据时自动加1。

   ```mysql
   mysql> create table test_scores(
   id int not null  auto_increment,
   student_id varchar(10) not null ,
   subject varchar(10) not null,
   score int not null,
   submit_date Date,
   primary key  (id)
    );
   ```

   创建完毕，可以重复步骤2的命令（show tables），检查是否创建成功。

5. 查看服务器版本

   ```mysql
   mysql>select version()
   ```



## MYSQL的语法规范

1. 不区分大小写，但建议关键字大写，表明、列名小写

2. 每条命令最好用分号结尾

3. 每条命令根据需要，可以进行缩进或换行

4. 注释

   ```mysql
   单行注释：#注释内容
   单行注释：--注释内容
   多行注释：/*注释内容*/
   ```



## DQL语言的学习

### 基础查询

1. 查询常量、表达式、函数
   
   ```mysql
   mysql>
   select 内容1,内容2... from 表名；
   ```
   
   查询的结果是一个虚拟的表格
   
2. 起别名

   ```mysql
   mysql>
   select 内容1 as 别名1,内容2 as 别名2 from 表名;
   select 内容1 别名1,内容2 别名2 from 表名;
   ```

3. 去重

   ```mysql
   mysql>
   select distinct 内容 from 表名;
   ```

4. +号的作用

   只有一个功能：运算符。

   select 100+90;	两个操作数都为数值型，则做加法运算。

   select '123'+321; 	其中一方为字符型，试图将字符型数值转换成数值型。如果转换成功，则继续做加法运算。如果转换失败，则将字符型数值转换成0。

   select null+1;	只要其中一方为null，则结果一定为null。

   ```mysql
   mysql>
   select 内容1+内容2 as 别名 from 表名;
   ```

5. 使用concat进行连接

   ```mysql
   mysql>
   select concat(内容1，内容2) as 别名 from 表名;
   ```




### 条件查询

1. where语法

   ```mysql
   mysql>
   select 内容 from 表名 where 筛选条件;
   ```

   根据筛选条件可分类：

   1.按条件表达式筛选。条件运算符：＞,＜,＝,<>, >=, <=

   2.按逻辑表达式筛选。用于连接逻辑表达式。逻辑运算符：and, or, not

   3.模糊查询。

   ​	like, 一般和通配符搭配使用，通配符：%：任意多（0）个字符，_：任意单个字符。

   ​	between and,  data between 10 and 20等价于data >=10 and data <=20

   ​	in, 判断某字段的值是否属于in列表中的某一项。使用in提高语句简洁度，in列表的值类型必须一致或兼容，不支持使用通配符。

   ​	is (not) null,=或<>不能用于判断null值，is (not) null 可以用来判断null值。

2. 安全等于<=>

   安全等于既可以判断null值又可以判断普通数值，可读性低。

   is (not) null只能判断null值，可读性高。

### 排序查询

1. 语法

   ```mysql
   mysql>
   select 内容 from 表名 where 筛选条件 order by 排序列表 desc(asc)
   ```

   特点：

   1.desc 降序，asc   升序。如果不写默认是升序。

   2.order by子句中可以支持单个字段、多个字段、表达式、函数、别名。

   3.order by子句一般是放在查询语句的最后面，limit子句除外。

   

### 常见函数

功能：类似于java的方法，将一组逻辑语句封装在方法体中，对外暴露方法名。

好处：1.隐藏了实现细节。2.提高代码的重用性。

调用：

```mysql
mysql>
select 函数名() from 表名;
```

分类：1.单行函数：如concat(), length(), ifnull() 等

​			2.分组函数：做统计使用，又称为统计函数、聚合函数、组函数

1. 字符函数

   length()：获取参数值的字节个数

   ```mysql
   mysql>
   select length('john');
   --4
   select length('我爱你')
   --9
   ```

   

   concat()：拼接字符串

   ```mysql
   mysql>
   select concat(lastname,'_',firstname) 姓名 from employees;
   ```

   

   upper(), lower()：大写小写

   ```mysql
   mysql>
   select upper('john');
   select lower('JOHN');
   ```

   

   substr(), substring()：返回子字符串 

   索引从1开始

   ```mysql
   mysql>
   select substr('柯西的柯西收敛原理',4);
   --柯西收敛原理
   select substr('柯西的柯西收敛原理',4,4);
   --柯西收敛
   ```

   

   instr()：返回子串第一次出现的索引，如果找不到返回0

   ```mysql
   mysql>
   select instr('有理数是实数集的稠子集','实数集');
   --5
   ```

   

   trim()：掐头去尾

   ```mysql
   mysql>
   select trim('   你是人间肆月甜   ');
   --你是人间肆月甜
   select trim('a'from'aaaaa你是人间肆月甜');
   --你是人间肆月甜
   ```

   

   lpad()：用指定字符实现左填充指定长度

   ```mysql
   mysql>
   select lpad('mathmatical analysis',5,'*');
   --*****mathmatical analysis
   ```

   

   rpad()：用指定字符实现右填充指定长度

   ```mysql
   mysql>
   select rpad('mathmatical analysis',5,'*');
   --mathmatical analysis*****
   ```


   replace()：替换

   ```mysql
   mysql>
   select replace('张无忌爱上了周芷若','周芷若','赵敏');
   --张无忌爱上了赵敏
   ```

   

2. 数学函数

   round()：四舍五入

   ```mysql
   mysql>
   select round(5.567,2);
   --5.57
   select round(5.2);
   --5
   ```

   

   ceil()：向上取整，返回>=改参数的最小整数

   ```mysql
   mysql>
   select ceil();
   ```

   

   floor()：向下取整，返回<=改参数的最大整数

   ```mysql
   mysql>
   select floor(-9.99);
   -- -10
   ```

   

   truncate()：截断

   ```mysql
   mysql>
   select truncate(1.65,1);
   --1.6
   ```

   

   mod()：取余

   ```mysql
   mysql>
   select mod(10,3);
   --1
   ```

   mod(a,b)=a-a/b*b

   

3. 日期函数

   now()：返回当前系统日期+时间

   ```mysql
   mysql>
   select now();
   ```

   

   curdate()：返回当前日期，不包含时间

   ```mysql
   mysql>
   select curdate();
   ```

   

   curtime()：返回当前时间，不包含日期

   ```mysql
   mysql>
   select curtime();
   ```

   

   year(), month(), monthname(), day(), hour(), minute(), second()：可以获取数据的对应值

   ```mysql
   mysql>
   select year(now());
   ```

   

   str_to_date：将字符通过指定的格式转换成日期

   ```mysql
   mysql>
   select str_to_date('2002-2-19','%Y-%c-%d');
   --2002-2-19
   ```

   

   date_format：将日期转换成字符

   ```mysql
   mysql>
   select date_format(now(),'%Y年%m月%d日');
   ```

   

