+++
author = "Murphy"
title = "Python中list、tuple、dict和set的对比小结"
date = "2022-05-12"

description = "Summary of list, tuple, dict and set"
tags = [
    "Python",
]

categories = [
    "CS",
   ]

+++

对Python中list、turple、dict和set这四种数据类型进行对比总结。

<!--more-->

## 概览

|            | 列表list           | 元组tuple          | 字典dict                    | 集合set            |
| ---------- | ------------------ | ------------------ | --------------------------- | ------------------ |
| 读写要求   | 读写               | 只读               | 只读                        | 读写               |
| 读取方式   | 索引               | 索引               | 键key                       | 只能遍历           |
| 值是否重复 | 是                 | 是                 | 键不能重复，值可以          | 否                 |
| 数据存储   | 连续、静态         | 连续、静态         | 不连续、动态                | 不连续、动态       |
| 是否有序   | 是                 | 是                 | 否（自动正序）              | 否                 |
| 初始化     | [lvalue1, lvalue2] | (tvalue1, tvalue2) | {key1:value1,  key2:value2} | {svalue1, svalue2} |
| 增         | list.append(value) | 只读               | dict[key1] = value1         | set.add(value1)    |
| 删         | del list[2]        | 只读               | del dict[key]               | set.remove(value1) |
| 改         | list[2]=newValue   | 只读               | dict[key1] = newValue       | set.update(value)  |
| 查         | list[2:]           | tuple[1:5]         | dict[key]                   | value in set       |
| 应用场景   | 非结构化数据存储   | 稳定的数据存储     | 结构化数据存储              | 数据剔重           |

## 列表

列表是一组有序存储的数据，它的特性包括：

- 用中括号将元素括起来，元素之间有逗号隔开。

- 列表可存储任意类型的数据，列表中各元素的类型可以不同，列表中各元素的值可以重复。

- 列表数据按顺序存储（链式存储），每个元素都有索引，支持正序、倒序两种索引，正序索引从0开始，倒序索引从-1开始。

- 列表创建后，其中的数据可以修改。

**样例代码：**

```python
# 定义列表
list_t1 = ['I', 'love', 'use', 'python', [1, 2, 'a']]
list_t2 = list(["C语言中文网", "http://c.biancheng.net"])
print('list_t1:', list_t1)
print('list_t2:', list_t2)

# 将字符串转换成列表
list_t3 = list("hello")
print('list_t3:', list_t3)
# 将元组转换成列表
tuple1 = ('Python', 'Java', 'C++', 'JavaScript')
list_t4 = list(tuple1)
print('list_t4:', list_t4)
# 将字典转换成列表
dict1 = {'a':100, 'b':42, 'c':9}
list_t5 = list(dict1)
print('list_t5:', list_t5)
#将区间转换成列表
range1 = range(1, 6)
list_t6 = list(range1)
print('list_t6:', list_t6)
#创建空列表
print(list())

# 使用列表表达式创建列表
list_t7 = [x * x for x in range(1, 11)]
print('List Comprehensions, list_t7:', list_t7)
list_t8 = [x * x for x in range(1, 11) if x % 2 == 0]
print('List Comprehensions use if, list_t8:', list_t8)
list_t9 = [m + n for m in 'ABC' for n in 'XYZ']
print('List Comprehensions use double cycle, list_t9:', list_t9)
list_t10 = [x if x % 2 == 0 else -x for x in range(1, 11)]
print('List Comprehensions use else, list_t10:', list_t10)
print('--------------------------------------')

# 获取list长度
# print('list length:%d'.format(len(list_t1)))
print('length of list_t1:', len(list_t1))
print('--------------------------------------')

# 判断列表中是否包含某元素
print('python is in list_t1:', ('python' in list_t1))
print('Python is in list_t1:', ('Python' in list_t1))
print('--------------------------------------')

# 按索引获取list中某个元素的值
print('the first element of list_t1:', list_t1[0])
print('the last element of list_t1:', list_t1[-1])
print('the last element of list_t1:', list_t1[len(list_t1)-1])

# 切片，按索引获取list中某几个元素的值,注意左闭右开
print('the first three elements of list_t1:', list_t1[0:3])
print('the first three elements of list_t1:', list_t1[:3])
print('the last two elements of list_t1:', list_t1[-2:])
print('every two elements of list_t1:', list_t1[0:5:2])
print('--------------------------------------')

# 根据元素值定位元素在列表中的index索引位置
print('index of python in list_t1：', list_t1.index('python'))
print('--------------------------------------')

# 统计某个值在列表元素中的个数
print('quantity of python in list_t1：', list_t1.count('python'))
print('quantity of Python in list_t1：', list_t1.count('Python'))
print('--------------------------------------')

# 在list最后追加元素
list_t1.append('too')
print('After append, list_t1:', list_t1)

# 在list中间插入元素
list_t1.insert(1, 'really')
print('After insert, list_t1:',list_t1)

# 把一个list添加到另一个list中
list_t5.extend(list_t6)
print('After extend, list_t5:',list_t5)

# 合并两个list
list_t7 = list_t3 + list_t4
print('After join list_t3 and list_t4:',list_t7)
print('--------------------------------------')

# 修改list中的元素
list_t1[-3] = 'java'
print('modify list_t1:', list_t1)
list_t1[4:7] = ['python', 'haha']
print('modify list_t1:', list_t1)
print('--------------------------------------')

# 删除list中的元素
del list_t1[1]
print('delete element by index， list_t1:', list_t1)
list_t1.pop()
print('delete the last element， list_t1:', list_t1)
list_t1.remove('love')
print('delete element by value， list_t1:', list_t1)
print('--------------------------------------')

# 遍历列表
print('print the elements of list_t1 one by one:',end=' ')
for i in range(len(list_t1)):
    print(list_t1[i], end=' ')
print('\n--------------------------------------')

# 同时遍历列表的索引和对应值
print('同时遍历列表的索引和对应值')
for index, value in enumerate(list_t1):
    print('index: {0} value: {1}'.format(index, value))
print('--------------------------------------')

# 排序
list_t1.sort()
print('sort List_t1:', list_t1)
list_t1.reverse()
print('reverse List_t1:', list_t1)
print('--------------------------------------')
```

**测试结果：**

```
list_t1: ['I', 'love', 'use', 'python', [1, 2, 'a']]
list_t2: ['C语言中文网', 'http://c.biancheng.net']
list_t3: ['h', 'e', 'l', 'l', 'o']
list_t4: ['Python', 'Java', 'C++', 'JavaScript']
list_t5: ['a', 'b', 'c']
list_t6: [1, 2, 3, 4, 5]
[]
List Comprehensions, list_t7: [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
List Comprehensions use if, list_t8: [4, 16, 36, 64, 100]
List Comprehensions use double cycle, list_t9: ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
List Comprehensions use else, list_t10: [-1, 2, -3, 4, -5, 6, -7, 8, -9, 10]
--------------------------------------
length of list_t1: 5
--------------------------------------
python is in list_t1: True
Python is in list_t1: False
--------------------------------------
the first element of list_t1: I
the last element of list_t1: [1, 2, 'a']
the last element of list_t1: [1, 2, 'a']
the first three elements of list_t1: ['I', 'love', 'use']
the first three elements of list_t1: ['I', 'love', 'use']
the last two elements of list_t1: ['python', [1, 2, 'a']]
every two elements of list_t1: ['I', 'use', [1, 2, 'a']]
--------------------------------------
index of python in list_t1： 3
--------------------------------------
quantity of python in list_t1： 1
quantity of Python in list_t1： 0
--------------------------------------
After append, list_t1: ['I', 'love', 'use', 'python', [1, 2, 'a'], 'too']
After insert, list_t1: ['I', 'really', 'love', 'use', 'python', [1, 2, 'a'], 'too']
After extend, list_t5: ['a', 'b', 'c', 1, 2, 3, 4, 5]
After join list_t3 and list_t4: ['h', 'e', 'l', 'l', 'o', 'Python', 'Java', 'C++', 'JavaScript']
--------------------------------------
modify list_t1: ['I', 'really', 'love', 'use', 'java', [1, 2, 'a'], 'too']
modify list_t1: ['I', 'really', 'love', 'use', 'python', 'haha']
--------------------------------------
delete element by index， list_t1: ['I', 'love', 'use', 'python', 'haha']
delete the last element， list_t1: ['I', 'love', 'use', 'python']
delete element by value， list_t1: ['I', 'use', 'python']
--------------------------------------
print the elements of list_t1 one by one: I use python 
--------------------------------------
同时遍历列表的索引和对应值
index: 0 value: I
index: 1 value: use
index: 2 value: python
--------------------------------------
sort List_t1: ['I', 'python', 'use']
reverse List_t1: ['use', 'python', 'I']
--------------------------------------
```



## 元组

元组是一组有序存储的数据，它的特性包括：

- 用小括号将元素括起来，元素之间有逗号隔开。

- 元组可存储任意类型的数据，元组中各元素的类型可以不同，元组中各元素的值可以重复。

- 元组数据按顺序存储（链式存储），每个元素都有索引，支持正序、倒序两种索引，正序索引从0开始，倒序索引从-1开始。

- 元组一经初始化，数据不能修改。

与列表相比，元组是不可变的，这使得它可以作为字典的 key，或者扔进集合里。元组放弃了对元素的增删，换取的是性能上的提升：创建元组比列表快，存储空间比列表占用更小。多线程并发的时候，元组是不需要加锁的，不用担心安全问题，编写也简单多了。

**样例代码：**

```python
# 元组和列表一样，支持获取长度函数len、使用索引获取元素、切片、for和range遍历、for和enumerate遍历，以及in、not in判断。
# 由于元组的内容不能修改，他不支持增删改方法,也不支持sort和reverse排序方法。
# 可以将元组先转化为列表，排序后再转化回元组，间接实现元组排序。
tuple_t1 = ('banana', 'color', 'apple', 'dark')
tmp_l1 = list(tuple_t1)
tmp_l1.sort()
tuple_t1 = tuple(tmp_l1)
for index, value in enumerate(tuple_t1):
    print('index:%d value:%s' % (index, value))
print('--------------------------------------')
```

**测试结果：**

```
index:0 value:apple
index:1 value:banana
index:2 value:color
index:3 value:dark
--------------------------------------
```



## 字典

字典是一组无序存储的key-value键值对，它的特性包括：

- 用大括号将元素括起来，每个元素包含一个键和一个值，键和值用冒号分隔，元素之间有逗号隔开。
- 字典中的键key可以是数字、字符串或者是元组等不可变类型；字典中的值value可以是任何数据类型。
- 字典中的键key是大小写敏感的，不能重复。可使用key，获取字典中对应的value。
- 字典根据key计算其保存位置（哈希算法），所以其内部存放顺序与key放入顺序无关。
- 与列表相比：字典的查找和插入的速度极快，不会随着key的增加而变慢；但字典需要占用大量的内存，内存浪费多。

**样例代码：**

```python
# 直接定义字典
dict1 = {'name': '小明', 'sex': 'male', 'age': 18, 'high': 180}
print('dict1:', dict1)
print('--------------------------------------')

# 使用dict和zip，将两个列表、元组或者字符串转为字典
print('use dict and zip')
list1 = ['key1','key2','key3']
list2 = ['1','2','3']
dict2 = dict(zip(list1,list2))
print('dict2:', dict2)
print('-------------')
tuple1 = ('key1','key2','key3')
tuple2 = ('1','2','3')
dict2 = dict(zip(tuple1,tuple2))
print('dict2:', dict2)
print('-------------')
str1 = 'abcde'
str2 = '12345'
dict2 = dict(zip(str1,str2))
print('dict2:', dict2)
print('--------------------------------------')

# 使用dict，将嵌套列表或者元组映射转化为字典的key和value
print('use dict')
list3 = [['key1','value1'],['key2','value2'],['key3','value3']]
dict3 = dict(list3)
print('dict3:', dict3)
print('-------------')
list4 = [('key1','value1'),('key2','value2'),('key3','value3')]
dict3 = dict(list4)
print('dict3:', dict3)
print('-------------')
tuple3 = (['key1','value1'],['key2','value2'],['key3','value3'])
dict3 = dict(tuple3)
print('dict3:', dict3)
print('-------------')
tuple4 = (('key1','value1'),('key2','value2'),('key3','value3'))
dict3 = dict(tuple4)
print('dict3:', dict3)
print('--------------------------------------')

# 使用fromkeys方法创建字典
print('use fromkeys')
list5 = ['语文', '数学', '英语']
dict4 = dict.fromkeys(list5)
print('dict4:', dict4)
dict4 = dict.fromkeys(list5, 60)
print('dict4:', dict4)
print('--------------------------------------')

# 字典的key和value互转
dict5 = {value:key for key, value in dict2.items()}
print('switch key and value of dict2:', dict5)
print('--------------------------------------')

# 获取dict的元素个数
print('length of dict1:', len(dict1))
print('--------------------------------------')

# 判断字典中是否包含某元素
print('name is in dict1:', ('name' in dict1))
print('Name is in dict1:', ('Name' in dict1))
print('--------------------------------------')

# 通过键访问元素,使用get方法避免key不存在时的异常
print('get value by key(name), dict1:', dict1['name'])
print('get value by key(name), dict1:', dict1.get('name'))
print('get value by key(school), dict1:', dict1.get('school'))
print('get value by key(school), dict1:', dict1.get('school', 'haha'))
print('--------------------------------------')

# 添加单个元素
dict1['score'] = 100
print('add key(score), dict1:', dict1)
# 添加多个元素（合并两个字典)
dict8 = {'country': 'china', 'city': 'Beijing'}
dict1.update(dict8)
print('add dict8 to dict1:', dict1)
# 修改单个元素的value
dict1['score'] = 80
print('modify value of key(score), dict1:', dict1)
#更新多个元素的值
dict1.update(name='王四', age=40)
print('modify values, dict1:', dict1)
# 设置默认
dict1.setdefault('score', 90)
print('set default value, dict1:', dict1)
dict1.setdefault('job', 'Stu')
print('set default value, dict1:', dict1)
print('--------------------------------------')

# 字典删除
dict1.pop('score')
print('delete key(score):', dict1)
del dict1['country']
print('delete key(country):', dict1)
dict1.popitem()
print('delete the last key:', dict1)
dict2.clear()
print('clear dict2:', dict2)
print('--------------------------------------')

# 字典的视图,会随着字典的变化而变化
key = dict1.keys()
value = dict1.values()
item = dict1.items()
print('keys of dict1：', key)
print('values of dict1：', value)
print('items of dict1：', item)

dict1['school']='szu'
print('keys of dict1：', key)
print('values of dict1：', value)
print('items of dict1：', item)
print('--------------------------------------')

# 使用视图，遍历字典元素
print('遍历字典元素')
for key in dict1.keys():
    print('键:', key, '\t值:', dict1[key])
print('-------------')
for key in dict1:
	print('键:', key, '\t值:', dict1[key])
print('-------------')
for key,value in dict1.items():
	print('键:', key, '\t值:', value)
print('--------------------------------------')
```

**测试结果：**

```
dict1: {'name': '小明', 'sex': 'male', 'age': 18, 'high': 180}
--------------------------------------
use dict and zip
dict2: {'key1': '1', 'key2': '2', 'key3': '3'}
-------------
dict2: {'key1': '1', 'key2': '2', 'key3': '3'}
-------------
dict2: {'a': '1', 'b': '2', 'c': '3', 'd': '4', 'e': '5'}
--------------------------------------
use dict
dict3: {'key1': 'value1', 'key2': 'value2', 'key3': 'value3'}
-------------
dict3: {'key1': 'value1', 'key2': 'value2', 'key3': 'value3'}
-------------
dict3: {'key1': 'value1', 'key2': 'value2', 'key3': 'value3'}
-------------
dict3: {'key1': 'value1', 'key2': 'value2', 'key3': 'value3'}
--------------------------------------
use fromkeys
dict4: {'语文': None, '数学': None, '英语': None}
dict4: {'语文': 60, '数学': 60, '英语': 60}
--------------------------------------
switch key and value of dict2: {'1': 'a', '2': 'b', '3': 'c', '4': 'd', '5': 'e'}
--------------------------------------
length of dict1: 4
--------------------------------------
name is in dict1: True
Name is in dict1: False
--------------------------------------
get value by key(name), dict1: 小明
get value by key(name), dict1: 小明
get value by key(school), dict1: None
get value by key(school), dict1: haha
--------------------------------------
add key(score), dict1: {'name': '小明', 'sex': 'male', 'age': 18, 'high': 180, 'score': 100}
add dict8 to dict1: {'name': '小明', 'sex': 'male', 'age': 18, 'high': 180, 'score': 100, 'country': 'china', 'city': 'Beijing'}
modify value of key(score), dict1: {'name': '小明', 'sex': 'male', 'age': 18, 'high': 180, 'score': 80, 'country': 'china', 'city': 'Beijing'}
modify values, dict1: {'name': '王四', 'sex': 'male', 'age': 40, 'high': 180, 'score': 80, 'country': 'china', 'city': 'Beijing'}
set default value, dict1: {'name': '王四', 'sex': 'male', 'age': 40, 'high': 180, 'score': 80, 'country': 'china', 'city': 'Beijing'}
set default value, dict1: {'name': '王四', 'sex': 'male', 'age': 40, 'high': 180, 'score': 80, 'country': 'china', 'city': 'Beijing', 'job': 'Stu'}
--------------------------------------
delete key(score): {'name': '王四', 'sex': 'male', 'age': 40, 'high': 180, 'country': 'china', 'city': 'Beijing', 'job': 'Stu'}
delete key(country): {'name': '王四', 'sex': 'male', 'age': 40, 'high': 180, 'city': 'Beijing', 'job': 'Stu'}
delete the last key: {'name': '王四', 'sex': 'male', 'age': 40, 'high': 180, 'city': 'Beijing'}
clear dict2: {}
--------------------------------------
keys of dict1： dict_keys(['name', 'sex', 'age', 'high', 'city'])
values of dict1： dict_values(['王四', 'male', 40, 180, 'Beijing'])
items of dict1： dict_items([('name', '王四'), ('sex', 'male'), ('age', 40), ('high', 180), ('city', 'Beijing')])
keys of dict1： dict_keys(['name', 'sex', 'age', 'high', 'city', 'school'])
values of dict1： dict_values(['王四', 'male', 40, 180, 'Beijing', 'szu'])
items of dict1： dict_items([('name', '王四'), ('sex', 'male'), ('age', 40), ('high', 180), ('city', 'Beijing'), ('school', 'szu')])
--------------------------------------
遍历字典元素
键: name 	值: 王四
键: sex 	值: male
键: age 	值: 40
键: high 	值: 180
键: city 	值: Beijing
键: school 	值: szu
-------------
键: name 	值: 王四
键: sex 	值: male
键: age 	值: 40
键: high 	值: 180
键: city 	值: Beijing
键: school 	值: szu
-------------
键: name 	值: 王四
键: sex 	值: male
键: age 	值: 40
键: high 	值: 180
键: city 	值: Beijing
键: school 	值: szu
--------------------------------------
```



## 集合

集合是一组无序的、不重复的元素，它的特性包括：

- 用大括号将元素括起来，元素之间有逗号隔开。
- 集合和字典的区别在于没有存储对应的value。
- 集合中各元素的值不可重复，所以集合可用于数据剔重。
- 集合中的元素只能是不可变的数据类型，包括整型、浮点型、字符串、元组，无法存储列表、字典、集合这些可变的数据类型。

**样例代码：**

```python
# 创建集合/不可变集合
set1 = {'Beijing', 'Shanghai', 'Shenzhen', 'HongKong'}
print('set1:', set1)
set2 = set('hello')
print('set2:', set2)
set3 = set([1,2,3,4,5,6,5,3])
print('set3:', set3)
set4 = set((1,2,3,4,5,6,6,1))
print('set4:', set4)
fzset1 = frozenset('python')
print('fset1:', fzset1)
print('--------------------------------------')

# 获取set的元素个数
print('length of set1：', len(set1))
print('--------------------------------------')

# 由于集合的无序性，访问集合只能遍历
print('访问集合只能遍历')
for elem in set1:
    print(elem, end=',')
print('\n--------------------------------------')

# 集合中添加和修改元素
set1.add('Chengdu')
set1.add((1, 2))
print('set1:', set1)
set8 = set([2, 3, 4])
set1.update(set8)
print('set1:', set1)
print('--------------------------------------')

# 删除集合元素
set1.remove(2)
print('delete set1:', set1)
set1.discard(2)
print('delete set1:', set1)
set1.discard((1, 2))
print('delete set1:', set1)
aaa = set1.pop()
print('after pop, aaa:', aaa)
print('after pop, set1:', set1)
set2.clear()
print('clear set2:', set2)
print('--------------------------------------')

# 判断集合中是否包含某元素
print('Beijing is in set1:', ('Beijing' in set1))
print('beijing is in set1:', ('beijing' in set1))
print('--------------------------------------')

# 比较两个集合的关系,判断集合关系的操作符：==、!=、<、<=、>、>=
print('set1 > set8:', (set1 > set8))
print('set3 = set4:', (set3 == set4))
print('--------------------------------------')

# 集合的并集
set5 = set1 | set8
print('union of set1 and set8:', set5)
set5 = set1.union(set8)
print('union of set1 and set8:', set5)
print('--------------------------------------')

# 集合的交集
set5 = set1 & set8
print('intersection of set1 and set8:', set5)
set5 = set1.intersection(set8)
print('intersection of set1 and set8:', set5)
print('--------------------------------------')

# 集合的差集
set5 = set1 - set8
print('difference of set1 and set8:', set5)
set5 = set1.difference(set8)
print('difference of set1 and set8:', set5)
print('--------------------------------------')

# 集合的对称差分:有所有属于集合A和集合B，并且不同时属于集合A和集合B的元素组成。
set5 = set1 ^ set8
print('symmetric difference of set1 and set8:', set5)
set5 = set1.symmetric_difference(set8)
print('symmetric difference of set1 and set8:', set5)
print('--------------------------------------')
```

**测试结果：**

```
set1: {'Shenzhen', 'Shanghai', 'Beijing', 'HongKong'}
set2: {'h', 'e', 'l', 'o'}
set3: {1, 2, 3, 4, 5, 6}
set4: {1, 2, 3, 4, 5, 6}
fset1: frozenset({'y', 'h', 'n', 'p', 'o', 't'})
--------------------------------------
length of set1： 4
--------------------------------------
访问集合只能遍历
Shenzhen,Shanghai,Beijing,HongKong,
--------------------------------------
set1: {(1, 2), 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
set1: {2, (1, 2), 3, 4, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
--------------------------------------
delete set1: {(1, 2), 3, 4, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
delete set1: {(1, 2), 3, 4, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
delete set1: {3, 4, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
after pop, aaa: 3
after pop, set1: {4, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
clear set2: set()
--------------------------------------
Beijing is in set1: True
beijing is in set1: False
--------------------------------------
set1 > set8: False
set3 = set4: True
--------------------------------------
union of set1 and set8: {2, 3, 4, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
union of set1 and set8: {2, 3, 4, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
--------------------------------------
intersection of set1 and set8: {4}
intersection of set1 and set8: {4}
--------------------------------------
difference of set1 and set8: {'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
difference of set1 and set8: {'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
--------------------------------------
symmetric difference of set1 and set8: {2, 3, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
symmetric difference of set1 and set8: {2, 3, 'Shanghai', 'Shenzhen', 'Beijing', 'HongKong', 'Chengdu'}
--------------------------------------
```



  

   
