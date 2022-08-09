+++

author = "Murphy"
title = "k近邻算法--1.k近邻算法概述"
date = "2022-08-09"

description = ""
tags = [
    "Machine Learning",
]

categories = [
    "CS",
   ]

math=true

toc=true

+++

简单地说，k-近邻算法采用测量不同特征值之间的距离方法进行分类。

<!--more-->

## 2.1k近邻算法概述

k近邻算法

优点：精度高、对异常值不敏感、无数据输入假定。

缺点：计算复杂度高、空间复杂度高。

适用数据范围：数值型和标称型。

工作原理：存在一个样本数据集合，也称作训练样本集，并且样本集中每个数据都存在标签，即我们知道样本集中每一数据与所属分类的对应关系。输入没有标签的新数据后，将新数据的每个特征与样本集中数据对应的特征进行比较，然后算法提取样本集中特征最相似数据（最近邻）的分类标签。一般来说，我们只选择样本数据集中前k个最相似的数据，这就是k近邻算法中k的出处，通常k是不大于20的整数。最后，选择k个最相似数据中出现次数最多的分类，作为新数据的分类。



我们举一个电影分类的例子，使用k近邻算法分类爱情片和动作片。有人曾经统计过很多电影的打斗镜头和接吻镜头，图1显示了6部电影的打斗和接吻镜头数。假如有一部未看过的电影，如何确定它是爱情片还是动作片呢？

![K-nearest-neighbor-algorithm1.1](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/K-nearest-neighbor-algorithm1.1.png)

![K-nearest-neighbor-algorithm1.2](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/K-nearest-neighbor-algorithm1.2.png)

即使不知道未知电影属于哪种类型，我们也可以通过某种方法计算出来。首先计算未知电影与样本集中其他电影的距离。此处暂时不关心如何计算得到这些距离值，使用Python实现电影分类应用时，会提供具体的计算方法。

![K-nearest-neighbor-algorithm1.3](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/K-nearest-neighbor-algorithm1.3.png)

现在我们得到了样本集中所有电影与未知电影的距离，按照距离递增排序，可以找到k个距离最近的电影。假定k=3，则三个最靠近的电影依次是He's Not Really into Dudes、Beautiful Woman和California Man。k近邻算法按照距离最近的三部电影的类型，决定未知电影的类型，而这三部电影全是爱情片，因此我们判定未知电影是爱情片。





k近邻算法的一般流程

1.收集数据：可以使用任何方法。

2.准备数据：距离计算所需要的数值，最好是结构化的数据格式。

3.分析数据：可以使用任何方法。

4.训练算法：此步骤不适用于k近邻算法。

5.测试算法：计算错误率。

6.使用算法：首先需要输入样本数据和结构化的输出结果，然后运行k近邻算法判定输入数据分别属于哪个分类，最后应用对计算出的分类执行后续的处理。



### 2.1.1准备：使用Python导入数据

首先，创建名为kNN.py的Python模块，我们使用的所有代码都在这个文件中。在kNN.py文件中增加下面的代码：

```python
import numpy
import operator
def createDataSet():
    group = array([[1.0, 1.1], [1.0, 1.0], [0, 0], [0, 0.1]])
    labels = ['A', 'A', 'B', 'B']
    return group, labels
```

这里有4组数据，每组数据有两个我们已知的属性或者特征值。上面的group矩阵每行包含一个不同的数据，我们可以把它想象为某个日志文件中不同的测量点或者入口。由于人类大脑的限制，我们通常只能可视化处理三维以下的事务。因此为了简单地实现数据可视化，对于每个数据点我们通常只使用两个特征。

向量label包含了每个数据点的标签信息，label包含的元素个数等于group矩阵行数。这里我们将数据点(1, 1.1)定义为类A，数据点(0, 0.1)定义为类B。为了说明方便，例子中的数值是任意选择的，并没有给出轴标签。下图是带有类标签信息的四个数据点。

![K-nearest-neighbor-algorithm1.4](https://raw.githubusercontent.com/MurphyHanxu/blogs-images/master/images/K-nearest-neighbor-algorithm1.4.png)
