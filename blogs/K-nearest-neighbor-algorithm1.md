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

## 1.  k近邻算法概述

k近邻算法

优点：精度高、对异常值不敏感、无数据输入假定。

缺点：计算复杂度高、空间复杂度高。

适用数据范围：数值型和标称型。

工作原理：存在一个样本数据集合，也称作训练样本集，并且样本集中每个数据都存在标签，即我们知道样本集中每一数据与所属分类的对应关系。输入没有标签的新数据后，将新数据的每个特征与样本集中数据对应的特征进行比较，然后算法提取样本集中特征最相似数据（最近邻）的分类标签。一般来说，我们只选择样本数据集中前k个最相似的数据，这就是k近邻算法中k的出处，通常k是不大于20的整数。最后，选择k个最相似数据中出现次数最多的分类，作为新数据的分类。



我们举一个电影分类的例子，使用k近邻算法分类爱情片和动作片。有人曾经统计过很多电影的打斗镜头和接吻镜头，图1显示了6部电影的打斗和接吻镜头数。假如有一部未看过的电影，如何确定它是爱情片还是动作片呢？

![K-nearest-neighbor-algorithm1.1](https://MurphyHanxu.github.io/blogs-images/images/K-nearest-neighbor-algorithm1.1.png)

![K-nearest-neighbor-algorithm1.2](https://MurphyHanxu.github.io/blogs-images/images/K-nearest-neighbor-algorithm1.2.png)

即使不知道未知电影属于哪种类型，我们也可以通过某种方法计算出来。首先计算未知电影与样本集中其他电影的距离。此处暂时不关心如何计算得到这些距离值，使用Python实现电影分类应用时，会提供具体的计算方法。

![K-nearest-neighbor-algorithm1.3](https://MurphyHanxu.github.io/blogs-images/images/K-nearest-neighbor-algorithm1.3.png)

现在我们得到了样本集中所有电影与未知电影的距离，按照距离递增排序，可以找到k个距离最近的电影。假定k=3，则三个最靠近的电影依次是He's Not Really into Dudes、Beautiful Woman和California Man。k近邻算法按照距离最近的三部电影的类型，决定未知电影的类型，而这三部电影全是爱情片，因此我们判定未知电影是爱情片。



k近邻算法一般流程

1. 收集数据：可以使用任何方法
2. 距离计算所需要的数值，最好是结构化的数据格式
3. 分析数据：可以使用任何方法
4. 训练数据：此步骤不适用于k近邻算法
5. 测试算法：计算错误率
6. 使用算法：首先需要输入样本数据和结构化的输出结果，然后运行k近邻算法判定输入数据分别属于哪个分类，最后应用对计算出的分类执行后续的处理。



### 1.1  准备：使用Python导入数据

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

![K-nearest-neighbor-algorithm1.4](https://MurphyHanxu.github.io/blogs-images/images/K-nearest-neighbor-algorithm1.4.png)

现在我们已经知道Python如何解析数据，如何加载数据，以及kNN算法的工作原理，接下来我们将使用这些方法完成分类任务。



### 1.2  从文本文件中解析数据

本节使用程序清单1.2.1的函数运行kNN算法，为每组数据分类。这里首先给出k近邻算法的伪代码和实际的Python代码，然后详细地解释每行代码的含义。该函数的功能是使用k近邻算法将每组数据划分到某个类中，其伪代码如下：

```python
对未知类别属性的数据集中的每个点依次执行以下操作：
(1)计算已知类别数据集中的点与当前点之间的距离
(2)按照距离递增次序排序
(3)选取与当前点距离最小的k个点
(4)确定前k个点所在类别的出现频率
(5)返回前k个点出现频率最高的类别作为当前点的预测分类
```

```python
#  程序清单1.2.1 k近邻算法
def classify0(inX, dataSet, labels, k):
    # 函数说明:打开并解析文件，对数据进行分类：1代表不喜欢,2代表魅力一般,3代表极具魅力
    # numpy函数shape[0]返回dataSet的行数
    dataSetSize = dataSet.shape[0]
    # 在列向量方向上重复inX共1次(横向),行向量方向上重复inX共dataSetSize次(纵向)
    diffMat = np.tile(inX, (dataSetSize, 1)) - dataSet
    # 二维特征相减后平方
    sqDiffMat = diffMat ** 2
    # sum()所有元素相加,sum(0)列相加,sum(1)行相加
    sqDistances = sqDiffMat.sum(axis=1)
    # 开方,计算出距离
    distances = sqDistances ** 0.5
    # 返回distances中元素从小到大排序后的索引值
    sortedDistIndices = distances.argsort()
    # 定一个记录类别次数的字典
    classCount = {}
    for i in range(k):
        # 取出前k个元素的类别
        voteIlabel = labels[sortedDistIndices[i]]
        # dict.get(key,default=None),字典的get()方法,返回指定键的值,如果值不在字典中返回默认值。
        # 计算类别次数
        classCount[voteIlabel] = classCount.get(voteIlabel, 0) + 1
    # python3中用items()替换python2中的iteritems()
    # key=operator.itemgetter(1)根据字典的值进行排序
    # key=operator.itemgetter(0)根据字典的键进行排序
    # reverse降序排序字典
    sortedClassCount = sorted(classCount.items(), key=operator.itemgetter(1), reverse=True)
    # 返回次数最多的类别,即所要分类的类别
    return sortedClassCount[0][0]
```

classify0()函数有4个输入参数：用于分类的输入向量是inX，输入的训练样本集为dataSet，标签向量为labels，最后的参数k表示用于选择最近邻居的数目，其中标签向量的元素数目和矩阵dataSet的行数相同。程序清单1.2.1使用欧氏距离公式，计算两个向量点xA和xB之间的距离①：
$$
d=\sqrt {(xA_0-xB_0)^2+(xA_1-xB_1)^2}
$$
计算完所有点之间的距离后，可以对数据按照从小到大的次序排序。然后，确定前k个距离最小元素所在的主要分类②，输入k总是正整数。最后，将classCount字典分解为元组列表，然后使用程序第二行导入运算法模块的itemgetter方法，按照第二个元素的次序对元组进行排序②。此处的排序为逆序，即按照从最大到最小次序排序，最后返回发生频率最高的元素标签。

为了预测数据所在分类，在Python提示符下输入下列命令：

```python
>>>kNN.classify0([0, 0], group, labels, 3)
```

输出结果应该是B，大家也可以改变输入[0, 0]为其他值，测试程序的运行结果。

到现在为止，我们已经构造了第一个分类器，使用这个分类器可以完成很多分类任务。从这个实例出发，构造使用分类算法将会更加容易。



### 1.3  如何测试分类器

上文我们已经使用k近邻算法构造了第一个分类器，也可以检验分类器给出的答案是否符合我们的预期。但我们不免有疑问：“分类器何种情况下会出错？”或“答案是否总是正确的？”答案是否定的，分类器并不会得到百分百正确的结果，我们可以使用多种方法检测分类器的正确率。此外分类器的性能也会收到多种因素的影响，如分类器设置和数据集等。不同的算法在不同数据集上的表现可能完全不同，这也是本部分的6章都在讨论分类算法的原因所在。

为了测试分类器的效果，我们可以使用一致答案的数据，当然答案不能告诉分类器，检验分类器给出的结果是否符合预期结果。通过大量的测试数据，我们可以得到分类器的错误率——分类器给出错误结果的次数除以测试执行的总数。错误率是常用的评估方法，主要用于评估分类器在某个数据集上的执行效果。完美分类器的错误率为0，最差分类器的错误率为1，在这种情况下，分类器根本无法找到一个正确答案。

上一节介绍的例子已经可以正常运转了，但是并没有太大的实际用处，本章的后两节将在现实世界中使用k近邻算法。
