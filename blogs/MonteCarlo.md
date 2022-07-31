+++

author = "Murphy"
title = "蒙特卡洛算法基于python的简单实现"
date = "2022-07-31"

description = "Installation of MySQL 5.7 in Win 10"
tags = [
    "Mathematical Modeling",
]

categories = [
    "CS",
   ]

+++

基于python用蒙特卡洛方法解决几道简单的数学建模问题。

<!--more-->

#### 前言

##### 什么是蒙特卡洛算法？

蒙特·卡罗（ Monte Carlo method），又称统计模拟方法，是二十世纪四十年代中叶由于科学技术的发展和电子计算机的发明，而被提出的一种以概率统计理论为指导的一类非常重要的数值计算方法。是指使用随机数（或更常见的伪随机数）来解决很多计算问题的方法。

##### 起源

蒙特卡洛方法于20世纪40年代美国在第二次世界大战中研制原子弹的“曼哈顿计划”计划成员S.M.乌拉姆和J.冯·诺依曼首先提出。数学家冯·诺依曼用驰名世界的赌城——摩纳哥的Monte Carlo——来命名这种方法，为它蒙上一层神秘色彩。公认的蒙特卡洛方法的起源是1777年法国数学家布丰提出用投针实验的方法求圆周率π。

##### 基本思想

当所求解问题是某种随机事件出现的概率，或者是某个随机变量的期望值时，通过某种“实验”的方法，以这种事件出现的频率估计这一随机事件的概率，或者得到这个随机变量的某些数字特征，并将其作为问题的解。

##### 工作步骤

构造或描述概率过程
实现从已知概率分布抽样
建立各种估计量

#### 例1

Buffon's needle problem

18世纪，蒲丰提出以下问题：设我们有一个以平行且等距木纹铺成的地板法国数学家布丰（1707-1788）最早设计了投针试验。
这一方法的步骤是：
1） 取一张白纸，在上面画上许多条间距为a的平行线。
2） 取一根长度为l（l≤a） 的针，随机地向画有平行直线的纸上掷n次，观察针与直线相交的次数，记为m。
3）计算针与直线相交的概率．
18世纪，法国数学家布丰提出的“投针问题”，记载于布丰1777年出版的著作中：“在平面上画有一组间距为a的平行线，将一根长度为l（l≤a）的针任意掷在这个平面上，求此针与平行线中任一条相交的概率。”
布丰本人证明了，这个概率是：
(其中π为圆周率）

布丰本人证明了，这个概率是：

![Monte Carlo1](https://bkimg.cdn.bcebos.com/formula/3a0c6c5228d39686af63569aa4728d13.svg)

```python
#  布丰投针实验用于估计pi的精确值
import random
import math
import numpy as np
import matplotlib.pyplot as plt

l = 1.5  # 任意给出针的长度l=1.5
a = 2  # 平行线的宽度，要求a>l即可
n = 1000 # 做n次投针实验，n越大pi越精确
m = 0  # 用于记录针与平行线相交的次数
x = []
phi = []

#  画图
fig = plt.figure()
ax = fig.add_subplot(111)
ax.set(xlim=[0, math.pi], ylim=[0, a/2], title='An Example Axes',
       ylabel='Y-Axis', xlabel='X-Axis')

for i in range(0, n):
    temp = np.random.random() * a / 2
    x.append(temp)
for i in range(0, n):
    temp = np.random.random() * math.pi
    phi.append(temp)
for i in range(0, n):
    if x[i] <= l/2*math.sin(phi[i]):
        m = m+1
        plt.scatter(phi[i], x[i], color='red')
p = m/n  # 针和平行线相交出现的频率
mypi = (2*l)/(a*p)
print(mypi)
plt.show()

#  注意在画图时，n不能取的太大，否则导致占计算机内存较多，画图需要花费较长时间呈现。
#  而如果想得到越精确的mypi则n越大越好
```

#### 例2

1.计算函数y = x^2在[0,1]区间的定积分

2.计算函数y = ln(1+x)/1+x^2 在[0,1]区间的定积分

通过生成大量的随机数来估算定积分的值。同例1一样也是几何概型问题。

```python
import math
import matplotlib.pylab as plt
import numpy as np
import random
#  1.计算函数y = x^2在[0,1]区间的定积分
# m = 1000000
# n = 0
# for i in range(m):
#     x = random.random()
#     y = random.random()
#     if y >= x**2:
#         n += 1
# r = n / m
# print("r = {}".format(r))

# r = 0.666817

#  2.计算函数y = ln(1+x)/1+x^2 在[0,1]区间的定积分

#  绘制函数图像
x = np.linspace(0, 1, num=50)
y = np.log(1 + x) / (1 + x**2)
plt.plot(x, y, '-')
plt.show()
m = 100000
n = 0
for i in range(m):
    x = random.random()
    y = random.random()
    if np.log(1 + x) / (1 + x**2) > y:
        n += 1
ans = n / m
print(ans)
```

#### 例3

三门问题

三门问题（Monty Hall probelm）亦称为蒙提霍尔问题，出自美国电视游戏节目Let’s Make a Deal。
参赛者会看见三扇关闭了的门，其中一扇的后面有一辆汽车，选中后面有车的那扇门可赢得该汽车，另外两扇门则各藏有一只山羊。
当参赛者选定了一扇门，但未去开启它的时候，节目主持人开启剩下两扇门的其中一扇，露出其中一只山羊。主持人其后问参赛者要不要换另一扇仍然关上的门。
问题是：换另一扇门是否会增加参赛者赢得汽车的几率？

如果严格按照上述条件，即主持人清楚地知道，自己打开的那扇门后面是羊，那么答案是会。不换门的话，赢得汽车的几率是1/3,，换门的话，赢得汽车的几率是2/3。



蒙特卡洛思想的应用

应用蒙特卡洛重点在使用随机数来模拟类似于赌博问题的赢率问题，并且通过多次模拟得到所要计算值的模拟值。
在三门问题中，用0、1、2分代表三扇门的编号，在[0,2]之间随机生成一个整数代表奖品所在门的编号prize，再次在[0,2]之间随机生成一个整数代表参赛者所选择的门的编号guess。用变量change代表游戏中的换门（true）与不换门（false）。

![Monte Carlo2](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/Monte%20Carlo2.png)

```python
import random


def play(change):
    prize = random.randint(0, 2)
    guess = random.randint(0, 2)
    if guess == prize:
        if change:
            return False
        else:
            return True
    else:
        if change:
            return True
        else:
            return False


def winRate(change, N):
    win = 0
    for i in range(N):
        if (play(change)):
            win += 1
    print("中奖率为{}".format(win / N))


N = 1000000

if __name__ == "__main__":
    print("每次换门的中奖概率：")
    winRate(True, N)
    print("每次都不换门的中奖概率：")
    winRate(False, N)

# 理论换门2/3 不换门1/3
# 每次换门的中奖概率：
# 中奖率为0.665769
# 每次都不换门的中奖概率：
# 中奖率为0.33292
```

#### 例4

M&M豆贝叶斯统计问题

M&M豆是有各种颜色的糖果巧克力豆。制造M&M豆的Mars公司会不时变更不同颜色巧克力豆之间的混合比例。
1995年，他们推出了蓝色的M&M豆。在此前一袋普通的M&M豆中，颜色的搭配为：30%褐色，20%黄色，20%红色，10%绿色，10%橙色，10%黄褐色。这之后变成了：24%蓝色，20%绿色，16%橙色，14%黄色，13%红色，13%褐色。

假设我的一个朋友有两袋M&M豆，他告诉我一袋是1994年，一带是1996年。但他没告诉我具体那个袋子是哪一年的，他从每个袋子里各取了一个M&M豆给我。一个是黄色，一个是绿色的。那么黄色豆来自1994年的袋子的概率是多少？

```python
import time
import random
for i in range(10):
    print(time.strftime("%Y-%m-%d %X",time.localtime()))
    dou = {1994:{'褐色':30,'黄色':20,'红色':20,'绿色':10,'橙色':10,'黄褐':30},
           1996:{'蓝色':24,'绿色':20,'橙色':16,'黄色':14,'红色':13,'褐色':13}}
    num = 10000
    list_1994 = ['褐色']*30*num+['黄色']*20*num+['红色']*20*num+['绿色']*10*num+['橙色']*10*num+['黄褐']*10*num
    list_1996 = ['蓝色']*24*num+['绿色']*20*num+['橙色']*16*num+['黄色']*14*num+['红色']*13*num+['褐色']*13*num
    random.shuffle(list_1994)
    random.shuffle(list_1996)
    count_all = 0
    count_key = 0
    for key in range(100 * num):
        if list_1994[key] == '黄色' and list_1996[key] == '绿色':
            count_all += 1
            count_key += 1
        if list_1994[key] == '绿色' and list_1996[key] == '黄色':
            count_all += 1
    print(count_key / count_all,20/27)
    print(time.strftime("%Y-%m-%d %X",time.localtime()))
    
    # ...
    # 0.7407064573459715 0.7407407407407407
    # 20/27是理论答案

```

