+++
author = "Murphy"
title = "Mysql-use notes"
date = "2022-06-30"

description = "SQL"
tags = [
    "SQL",
]

categories = [
    "CS",
   ]

+++

学习Mysql[视频](https://www.bilibili.com/video/BV12b411K7Zu?spm_id_from=333.337.search-card.all.click&vd_source=b3b5aca8a2675b381887886fba026f67)记录的笔记
整理MySQL最常用的SQL语句（增删改查CRUD）。
更多的命令可参考：https://www.runoob.com/mysql/mysql-select-database.html

<!--more-->
#Mysql-use
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

   

4. 其它函数

   ```mysql
   mysql>
   select version();
   select database();
   select user();
   ```

   
   
5. 流程控制函数

   1.if()：

   ```mysql
   mysql>
   select if(表达式,ans1,ans2);
   ```

   如果表达式为真，返回ans1。如果表达式为假，返回ans2。

   2.case()：

   ```mysql
   mysql>
   select salary, department_id
   case department_id
   when 30 then salary*1.1
   when 40 then salary*1.2
   when 50 then salary*1.3
   else salary
   end as new salary
   from employees;
   ```
   
   ```mysql
   mysql>
   select salary,
   case
   when salary>20000 then 'A'
   when salary>15000 then 'B'
   when salary>10000 then 'C'
   else 'D'
   end as salary level
   from employees
   ```

### 分组函数

功能：用作统计使用，又称为聚合函数或统计函数或组函数。

sum 求和，avg 平均值，max 最大值，min 最小值，count 计算个数

1. 简单的使用

   ```mysql
   mysql>
   select sum(salary) from employees;
   select avg(salary) from employees;
   select min(salary) from employees;
   select max(salary) from employees;
   select count(salary) from employees;
   ```

   ```mysql
   mysql>
   select sum(salary) 和, avg(salary) 平均 from employees;
   ```

2. 参数支持哪些类型

   sum(), avg()，一般用于数值型。

   max(), min()，可以处理任何类型。

   count()，计算不为null的个数。

3. 是否忽略null值

   以上分组函数都忽略null值

4. 可以和distinct搭配使用

   ```mysql
   mysql>
   select sum(distinct salary), sum(salary) from employees;
   select count(distinct salary), count(salary) from employees;
   ```

5. count()函数的详细介绍

   ```mysql
   mysql>
   select count(salary) from employees;
   select count(*) from employees;
   #在表里写一列常量值并统计个数
   select count(1) from employees;
   ```

   效率：

   myisam存储引擎下，count(*)的效率高

   innodb存储引擎下，count(*)和count(1)效率差不多，比count(字段)要高一些

6. 和分组函数一同查询的字段有限制

   和分组函数一同查询的字段要求是group by后的字段

### 分组查询

```mysql
mysql>
select group_function(column), column
from table
where condition
group by group_by_expression
order by column;
```

注意，查询列表必须特殊，要求是分组函数和group by后出现的字段

例：

```mysql
mysql>
select max(salary), job_id
from employees
group by job_id;
```

```mysql
mysql>
select count(*), location_id
from departments
group by location_id;
```

```mysql
mysql>
select avg(salary), department_id
from employees
where email like '%a%'
group by department_id;
```

```mysql
mysql>
select max(salary), manager_id
from employees
where commision_pct is not null
group by manager_id;
```

```mysql
mysql>
#添加分组后的筛选
select count(*), department_id
from employees
group by department_id
having count(*)>2;
```

```mysql
mysql>
#添加分组后的筛选
select max(salary), job_id
from employees
where commission_pet is not null
group by job_id
having max(salary)>12000;
```

```mysql
mysql>
#添加分组后的筛选
select min(salary), manager_id
from employees
where manager_id>12
group by manager_id
having min(salary)>5000;
```



按表达式或函数分组，例：

```mysql
mysql>
select count(*) c, length(lase_name) len_name
from employees
group by len_name
having c>5;
```



按多个字段分组，例：

```mysql
mysql>
select avg(salary), department_id , job_id
from employees
group by department_id, job_id
```



添加排序，例：

```mysql
mysql>
select avg(salary), department_id , job_id
from employees
where department_id is not null
group by department_id, job_id
having avg(salary)>10000
order by avg(salary) desc;
```



总结：

1.分组查询中的筛选条件分为两类。分组前筛选，分组后筛选。数据源不一样。前者为原始表，后者为分组后的结果集。位置不一样。前者为group by子句前，关键字为where，后者为group by子句后，关键字为having。

分组函数做条件肯定是放在having子句中。

2.group by子句支持单个字段分组，多个字段分组，表达式或函数

3.也可以添加排序（排序放在整个分组查询的最后）



### 连接查询

含义：又称多表连接，当查询的字段来自于多个表时，就会用到连接查询。

笛卡尔乘积现象：表1有m行，表2有n行，结果=m*n行

发生原因：没有有效的连接条件

分类：

​			按年代分类：sq192标准：仅支持内连接；

​									sq199标准：支持内连接，外连接（左外连接+右外连接），交叉连接

​			按功能分类：内连接(inner)：等值连接；非等值连接，自连接。

​								   外连接：左外连接(left)；右外连接(right)；全外连接(full)。

​								   交叉连接(cross)

#### sq92:

1. 等值连接

   例：

```mysql
mysql>
select last_name, department_name
from employees, departments
where employees.'department_id' = departments.'department_id';
```

```mysql
mysql>
select last_name, e.'job_id', job_title
from employees as e, jobs as j
where e.'job_id' = j.'job_id';
```

注意：如果为表起了别名，则查询的字段（select）就不能使用原来的表名去限定

```mysql
mysql>
select last_name, depatment_name
from employees as e, departments as d
where e.'department_id' = d.'department_id' and e.'commission_pct' is not null;
```

```mysql
mysql>
select department_name, city
from departments as d, location as l
where d.'location_id' = l.'location_id' and city like '_o%'
```

```mysql
mysql>
#可以添加分组和排序
select count(*) as 个数, city
from departments as d, locations as l
where d.'location_id' = l.'location_id'
group by city;
```

```mysql
mysql>
select job_title, count(*)
from employees as e, jobs as j
where e.'job_id' = j.'job_id'
group by job_title
order by count(*) desc;
```

总结

1.多表等值连接的结果为多表的交集部分

2.n表连接，至少需要n-1个连接条件

3.多表的顺序没有要求

4.一般需要为表起别名

5.可以搭配前面介绍的所有子句使用，比如排序、分组、筛选



​	2.非等值连接

例：

```mysql
mysql>
select salary, grade_level
from emoloyees as e, job_grades as g
where salary between g.'lowest_sal' and g.'hightest_sal'
and g.'grade_level' = 'A';
```



​    3.自连接

例：

```mysql
mysql>
select e.'employee_id', e.'last_name', m.'employee_id', m.'last_name'
from employees as e, employees as m
where e.'manager_id' = m.'employee_id';
```



#### sq99:

内连接

特点：

添加排序、分组、筛选

inner可以省略

筛选条件放在where后面，连接条件放在on后面，提高分离性，便于阅读

inner join连接和sql92语法中的等值连接效果是一样的

​	1.等值连接

```mysql
mysql>
#语法
select 查询列表
from 表1
inner join 表2
on 连接条件;
```



例：

```mysql
mysql>
select last_name, department_name
from employees as e
inner join departments as d
on e.'department_id' = d.'department_id';
```

```mysql
mysql>
select last_name, job_title
from employees as e
inner join jobs as 
on e.'job_id' = j.'job_id'
where e.'last_name' like '%e%';
```

```mysql
mysql>
select city, count(*) 部门个数
from departments as d
inner join lacations as l
on d.'location_id' = l.'location_id'
group by city
having count(*)>2;
```

```mysql
mysql>
select last name, department_name, job_title
from employees as e
inner join departments as d on e.'department_id' = d.'department_id'
inner join jobs as j on e.'job_id' = j.'job_id'
order by department_name desc;
```



​	2.非等值连接

例：

```mysql
mysql>
select salary, grade_level
from employees as e
join job_grades as g
on e.'salary' between g.'lowest_sal' and g.'highest_sal';
```



​	3.自连接

例：

```mysql
mysql>
select e.last_name, m.last_name
from employees as e
join employees as m
on e.'manager_id' = m.'employee_id'
where e.'last_name' like '%k%';
```



外连接

应用场景：用于查询一个表中有，而另一个表中没有的记录

特点：

1.外连接的查询结果为主表中的所有记录

​	如果从表中有和它匹配的，则显示匹配的值

​	如果从表中没有和它匹配的，则显示null

​	外连接查询结果=内连接查询结果+主表中有而从表中没有的记录

2.左外连接，left join左边的是主表

   右外连接，right join右边的是主表

3.左外和右外交换两个表的顺序，可以实现同样的效果



例：

```mysql
mysql>
#左外连接
select d.*, e.'employee_id'
from departments as d
left join employees as e
on d.'department_id' = e.'department_id'
where e.'employee_id' is null;
```

```mysql
mysql>
#右外连接
select d.*, e.'employee_id'
from employees as e
right join departments as d
on d.'department_id' = e.'department_id'
where e.'employee_id' is null;
```





交叉连接：

```mysql
mysql>
#本质为实现笛卡尔乘积
select girls.*, boys.*
from girls 
cross join boys;
```



### 子查询

含义：

出现在其他语句中的select语句，称为子查询。内部嵌套其他select语句的查询，称为主查询。

分类：

按子查询出现的位置。

按结果集的行列数不同。标量子查询（一行一列）；列子查询（一列多行）；行子查询（一行多列）；表子查询（多行多列）



​	1.标量子查询

例：

```mysql
mysql>
select *
from employees
where salary>(
				select salary
				from employees
				where last_name = 'Abel'
				)
;
```



```mysql
mysql>
select last_name, job_id, salary
from employees
where job_id = (
    			select job_id
				from employees
				where employee_id = 141
               )
and salary > (
				select salary
				from employees
				where employee_id = 143
);
```





​	2.列子查询

例：

```mysql
mysql>
select last_name
from employees
where department_id in(
						select distinct department_id
						from departments
						where location_id in(1400,1700)
);
```



```mysql
mysql>
select last_name
from employees
where department_id =(
						select distinct department_id
						from departments
						where location_id in(1400,1700)
);
```



​	3.行子查询

例：

```mysql
mysql>
select*
from employees
where (employee_id, salary) = (
								select min(employee_id, max(salary)
                                from employees)
```



​	4.select 后面的子查询

例：

```mysql
mysql>
select d.*,(
			select count(*)
			from employees
    		where e.'department_id' = d.'department_id'
)
from departments as d;
```



```mysql
mysql>
select (
		select department_name
    	from departments as d
    	inner join employees as e
    	on d.'d.department_id' = e.'department+id'
    	where e.'employee_id' = 102
) 部门名;
```



​	5.from后面的子查询

例：

```mysql
mysql>
select ag_dep.*, g.'grade_level'
from(
		select avg(salary), department_id
		from employees
		group by department_id
) as ag_dep
inner join job_grades as g
on ag_dep.ag between lowest_sal and highest_sal;
```



​	6.exists后面（相关子查询）

例：

```mysql
mysql>
select exists(select employee_id from employees where salary = 30000);
```





### 分页查询

应用场景：当要显示的数据，一页显示不全，需要分页提交sql请求

语法：

```mysql
mysql>
select 查询列表
from 表
(join type) join 表2
where 筛选条件
group by 分组字段
having 分组后的筛选
order by 排序
limit offset, size;
#offset要显示条目的起始索引（起始索引从0开始）(page-1)*s
#size 要显示的条目个数
```



例：

```mysql
mysql>
#查询前5条员工信息
select*
from employees
limit 0,5
```

```mysql
mysql>
#查询第11条到第25条员工信息
select*
from employees 
limit 10,15
```

```mysql
mysql>
#有奖金的员工信息，并且工资前10名显示
select*
from employees
where commision_pct is not null 
order by salary desc
limit 10;
```



特点：

1.limit语句放在查询语句的最后，执行逻辑也是在最后



### 联合查询

语法：

```mysql
mysql>
查询语句1
union
查询语句2
```



应用场景：

用于将多个表中相似信息连接在一起



特点：

1.要求多条查询语句的查询列表是一致的！

2.要求多条查询语句查询的每一列的类型和顺序是一致的

3.union关键字默认去重，如果使用union all可以包含重复项



例：

```mysql
mysql>
select *
from employees
where email like '%a%'
union
select *
from employees
where department_id>90;
```



## DML语言的学习

数据操作语言：数据的插入 insert，修改 update，删除 delete

### 插入语句

语法一：

```mysql
mysql>
insert into 表名(列名,...)
values(值1,...);
```



例：

```mysql
mysql>
#不可以为null的值必须插入值，可以为null的列如何插入值？
insert into beauty(id, name, sex, phone)
values(15,'王心凌', '女', '13888888888')
```



语法二：

```mysql
mysql>
insert into 表名
set 列名=值,列名=值...;
```



注：

1.方式一可以省略列名，但添加值的顺序必须和表中的顺序一致

2.方式一可以支持插入多行，方式二不支持

3.方式一支持子查询，省略了values()，方式二不支持



### 修改语句

语法：

```mysql
mysql>
update 表名
set 列=新值, 列=新值...
where 筛选条件;
```

```mysql
mysql>
update 表1 别名
inner (right,left) join 表2 别名
on 连接条件
set 列=值...
where 筛选条件;
```



### 删除语句

语法：

```
mysql>
delete from 表名
where 筛选条件;

mysql>
truncate table 表名;
```    
1.delete可以加where条件，truncate不能加

2.truncate删除，效率比delete高

3.假如要删除的表中有自增长列，如果用delete删除后，再插入数据，自增长列的值从断点开始，而如果truncate删除后，再插入数据，自增长列的值从1开始。

4.truncate删除后没有返回值，而delete删除后有返回值。

5.truncate删除不能回滚，而delete删除可以回滚。

##DDL 数据定义语言

库和表的管理

一、库的管理：创建、修改、删除

二、表的管理：创建、修改、删除、复制

创建：create，修改alter，删除drop

###一、库的管理

1.库的创建
```
mysql>
create database if not exists 库名;
```
2.库的修改

更改库的字符集
```
mysql>
alter database 库名 character set gbk;
```
3.库的删除
```
mysql>
drop database if exists 库名;
```
###二、表的创建

1.表的创建
```
mysql>
create table if not exists 表名(
列名, 列的类型, [(长度) 约束],
列名, 列的类型, [(长度) 约束],
...
列名, 列的类型, [(长度) 约束]
);
```

2.表的修改

修改列名
```
mysql>
alter table 表名 change column 旧列名 新列名 类型;
```
列的类型或约束
```
mysql>
alter table 表名 modify column 列名 新类型;
```
添加列
```
mysql>
alter table 表名 add column 新列名 新类型;
```
删除列
```
mysql>
alter table 表名 drop column 列名;
```
修改表名
```
mysql>
alter table 表名 rename to 新表名;
```
3.表的删除
```
mysql>
drop table if exists 表名;
```
4.表的复制
```
mysql>
只复制表的结构
create table 新表名 like 旧表名;

mysql>
复制表的结构和数据
create table 新表名
select * from 旧表名

mysql>
只复制部分数据
create table 新表名
select 内容1，内容2
from 旧表名
where 条件;
    
mysql>
只复制某些字段
create table 新表名
select 内容
from 旧表名
where 0;
```


##常见的数据类型
数值型：整型，小数（定点数、浮点数）
字符型：较短的文本char,varchar，较长的文本text,blob（较长的二进制数据）
日期型：

###一、整型
整数类型|字节|范围
--|--|--
tinyint|1|有符号：-128~127，无符号：0~255
smallint|2|有符号：-32768~32767，无符号：0~65535
mediumint|3|有符号：-8388608~8388607，无符号：0~1677215
int/integer|4|有符号：-2147483648~2147483647，无符号：0~4294967295
bigint|8|有符号：-2^64-1~2^64，无符号2^65+1
特点：
1.如果不设置无符号还是有符号，默认是有符号，如果想设置无符号，需要添加unsigned关键字。
2.如果插入的数值超出了整型的范围，会报out of range异常，并且插入临界值。
3.如果不设置长度，会有默认的长度
4.若要设置显示宽度，则在数据类型后面加上（要显示的长度），如果不够会用0在左边填充，但必须搭配关键字zerofill写在数据类型后使用。

1.如何设置有符号和无符号
```
mysql>
drop table if exists tab_int
create table tab_int(
    t1 int,
    t2 int unsigned
)
insert into tab_int values(-123)
insert into tab_int values(-123);
```

###二、小数
浮点数类型|字节
--|--
float|4
double|8

定点数类型|字节
--|--
decimal(M,D)|M+2
特点：
1.M表示整数位数+小数位数，D表示小数点后数字的位数，如果小数点后位数超出会自动四舍五入，如果整数部分超出整数位数则会插入临界值。
2.M和D都可以省略，会生成默认长度

###三、字符型
较短的文本：char,varchar，
其他：binary和varbinary用于保存较短的二进制，enum用于保存枚举，set用于保存集合
较长的文本：text,blob（较长的二进制数据）
字符串类型|最多字符数|描述及存储需求|特点|空间耗费|效率
--|--|--|--|--|
char(M)|M，可以省略，默认为1|M为0~255之间的整数|固定长度的字符|比较耗费|高
varchar(M)|M，不能省略|M为0~65535之间的整数|可变长度的字符|比较节省|低

###四、日期型

字符串类型|字节|最小值|最大值
--|--|--|--
date|4|1000-01-01|9999-12-31
datetime|8|1000-01-01 00:00:00|9999-12-31 23:59:59
timestamp|4|1970年|2038年的某个时刻
time|3|-838:59:59|838:59:59
year|1|1901|2155