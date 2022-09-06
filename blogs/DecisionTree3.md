+++

author = "Murphy"
title = "决策树--3.测试和分类储存器"
date = "2022-08-08"

description = ""
tags = [
    "Machine Learning",
]

categories = [
    "CS",
   ]

math= true

toc=true

+++

本节我们将把重点转移到如何利用决策树执行数据分类上，我们将使用决策树构建分类器，以及实际应用中如何存储分类器。

<!--more-->

## 3.  测试和储存分类器

### 3.1  测试算法：使用决策树执行分类

依靠训练数据构造了决策树之后，我们可以将它用于实际数据的分类。在执行数据分类时，需要决策树以及用于构造树的标签向量。然后，程序比较测试数据与决策树上的数值，递归执行该过程直到进入叶子节点。最后将测试数据定义为叶子节点所属的类型。

将程序清单3.1.1的代码添加到文件tree.py中

```python
#  程序清单3.1.1 使用决策树的分类函数
def classify(inputTree, featLabels, testVec):
    firstStr = list(inputTree.keys())[0]
    secondDict = inputTree[firstStr]
    #  ①将标签字符串转换为索引
    featIndex = featLabels.index(firstStr)
    key = testVec[featIndex]
    valueOfFeat = secondDict[key]
    if isinstance(valueOfFeat, dict): 
        classLabel = classify(valueOfFeat, featLabels, testVec)
    else: classLabel = valueOfFeat
    return classLabel
```

程序清单3.1.1定义的函数也是一个递归函数，在存储带有特征的数据会面临一个问题：程序无法确定特征在数据集中的位置，例如前面例子的第一个用于划分数据集的特征是no surfacing属性，但是在实际数据集中该属性存储在哪个位置？是第一个属性还是第二个属性？特征标签列表将帮助程序处理这个问题。使用index方法查找当前列表中第一个匹配firstStr变量的元素①。然后代码递归遍历整棵树，比较testVec变量中的值与树节点的值，如果达到叶子节点，则返回当前节点的分类标签。

将程序清单3.1.1的代码添加到文件trees.py之后，输入下列命令：

```python
>>>myDat, labels = trees.createDataSet()
>>>labels
["no surfacing", "flippers"]
>>>myTree = treePlotter.retrieveTree(0)
>>>trees.classify(myTree, labels, [1,0])
"no"
>>>trees.classify(myTree, labels, [1,1])
"yes"
```

第一节点名为no surfing，它有两个子节点：一个是名字为0的叶子节点，类标签为no；另一个是名为flippers的判断节点，此处进入递归调用，flippers节点有两个子节点。以前绘制的树形图和此处代表树的数据结构完全相同。

### 3.2  使用算法：决策树的存储

构造决策树时很耗时的任务，即使处理很小的数据集，如前面的样本数据，也要花费几秒的数据，如果数据集很大，则会耗费很多计算时间。然而用创建好的决策树解决分类问题，则可以很快完成。因此，为了节省计算时间，最好能够在每次执行分类时调用已经构造好的决策树。为了解决这个问题，需要使用Python模块pickle序列化对象。序列化对象可以在磁盘上保存对象，并在需要的时候读取出来。任何对象都可以执行序列化操作，字典对象也不例外。

```python
#  程序清单3.2.1 使用pickle模块存储决策树
def storeTree(inputTree,filename):
    import pickle
    fw = open(filename, "wb")
    pickle.dump(inputTree, fw)
    fw.close()
    
def grabTree(filename):
    import pickle
    fr = open(filename, "rb")
    return pickle.load(fr)
```

在Python命令提示符下输入下列命令验证上述代码：

```python
>>>trees.storeTree(myTree, "classifierStorage.txt")
>>>trees.grabTree("classifierStorage.txt")
{'no surfacing': {0: 'no', 1: {'flippers': {0: 'no', 1: 'yes'}}}}
```

通过上面的代码，我们可以将分类器存储在硬盘上，而不用每次对数据分类时重新学习一遍，这也是决策树的优点之一。我们可以预先提炼并存储数据集中包含的知识信息，在需要对事物进行分类时再使用这些知识。

我们之后将学习另一个决策树算法CART，本章使用的算法称为ID3，它是一个好的算法但并不完美。ID3算法无法直接处理数值型数据，尽管我们可以通过量化的方法将数值型数据转化为标称型数据，但是如果存在太多的划分，仍会面临其他问题。	
