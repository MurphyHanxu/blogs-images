+++

author = "Murphy"

title = "k近邻算法--2.示例：使用k近邻算法改进约会网站的配对效果"

date = "2022-08-10"

description = ""

tags = ["Machine Learning",]

categories = [ "CS",]

math=true

toc=true

+++

<!--more-->

## 2.2  示例：使用k近邻算法改进约会网站的配对效果

我的朋友海伦一直使用在线约会网站寻找适合自己的约会对象。尽管约会网站会推荐不同的人选，但她没有从中找到喜欢的人。经过一番总结，她发现曾交往过三种类型的人：(1)不喜欢的人、(2)魅力一般的人、(3)极具魅力的人。

尽管发现了上述规律，但海伦依然无法将约会网站推荐的匹配对象归入恰当的分类。她觉得可以在周一到周五约会那些魅力一般的人，而周末则更喜欢与那些极具魅力的人为伴。海伦希望我们的分类软件可以更好地帮助她将匹配对象划分到确切的分类中。此外海伦还收集了一些约会网站未曾记录的数据信息，她认为这些数据更有助于匹配对象的归类。

示例：在约会网站上使用k近邻算法

1. 收集数据：提供文本文件

2. 准备数据：使用Python解析文本文件

3. 分析数据：使用Matplotlib画二维扩散图

4. 训练算法：此步骤不适用于k近邻算法

5. 测试算法：使用海伦提供的部分数据作为测试样本。

   测试样本和非测试样本的区别在于：测试样本是已经完成分类的数据，如果预测分类与实际类别不同，则标记为一个错误。

6. 使用算法：产生简单的命令行程序，然后海伦可以输入一些特征数据以判断对方是否为自己喜欢的类型。



### 2.1  准备数据：从文本文件中解析数据

海伦收集约会数据已经有了一段时间，她把这些数据存放在文本文件datingTextSet.txt中，每个样本数据占据一行，总共有1k行。海伦的样本主要包含一下三种特征：

- 每年获得的飞行常客里程数
- 玩视频游戏所耗时间百分比
- 每周消费的冰淇淋公升数

在将上述特征数据输入到分类器之前，必须将待处理数据的格式改变为分类器可以接收的格式。将结果类型(1)不喜欢的人赋值为1，(2)魅力一般的人赋值为2，(3)极具魅力的人赋值为3。存放在文本文件datingTextSet2.txt中。

在kNN.py中创建名为file2matrix的函数，以此来处理输入格式问题。该函数的输入为文件名字符串，输出为训练样本矩阵和类标签向量。将下面的代码增加到kNN.py中：

```python 
#  程序清单2.1.1 将文本记录到转换NumPy的解析程序
def file2matrix(filename):
    fr = open(filename)
    #  ①得到文件行数
    numberOfLines = len(fr.readlines())
    #  ②创建返回的NumPy矩阵
    returnMat = zeros((numberOfLines,3))
    #  ③解析文件数据到列表
    classLabelVector = []
    fr = open(filename)
    index = 0
    for line in fr.readlines():
        line = line.strip()
        listFromLine = line.split('\t')
        returnMat[index,:] = listFromLine[0:3]
        classLabelVector.append(int(listFromLine[-1]))
        index += 1
    return returnMat, classLabelVector
```

从上面的代码可以看到，Python处理文本文件非常容易。首先我们需要知道文本文件包含多少行。打开文件，得到文件的行数①。然后创建以零填充的矩阵NumPy②。为了简化处理，我们将该矩阵的另一维度设置为固定值3，你可以按照自己的实际需求增加相应的代码以适应变化的输入值。循环处理文件中的每行数据③，首先使用函数line.strip()截取掉所有的回车字符，然后使用tab字符\t将上一步得到的整行数据分割成一个元素列表。接着，我们选取前3个元素，将它们存储到特征矩阵中。Python语言可以使用索引值-1表示列表中的最后一个元素。需要注意的是，我们必须明确地通知解释器，告诉它列表中存储的元素值为整型，否则Python语言会将这些元素当做字符串处理。以前我们必须自己处理这些变量值类型问题，现在这些细节问题完全可以交给NumPy函数库来处理。

```python
>>>datingDataMat, datingLabels = kNN.file2matrix('./datingTestSet.txt')
```

使用函数file2matrix读取文件数据，必须确保文件datingTestSet.txt存储在我们的工作目录中。现在已经从文本文件中导入了数据，并将其格式化为想要的格式，接着我们需要了解数据的真实含义。当然我们可以直接浏览文本文件，但一般来说，我们会采用图形化的方式直观地展示数据。

### 2.2  分析数据：使用Matplotlib创建散点图

首先我们使用Matplotlib制作原始数据的散点图，在Python命令行环境中，输入下列命令:

```python 
import matplotlib.pyplot as plt
from matplotlib import font_manager  # 导入字体管理模块
datingDataMat, datingLabels = file2matrix(r'./datingTestSet.txt')
# print(datingDataMat)
# print(datingLabels)
fig = plt.figure()
ax = fig.add_subplot(111)
ax.scatter(datingDataMat[:, 1], datingDataMat[:, 2])
my_font = font_manager.FontProperties(fname=r"C:/WINDOWS/Fonts/simfang.ttf")
plt.xlabel("玩视频游戏所耗时间百分比", fontproperties = my_font)
plt.ylabel("每周消费的冰淇淋公升数", fontproperties = my_font)
plt.show()
```

输出效果如下图所示，散点图使用datingDataMat矩阵的第二、第三列数据，分别表示特征值“玩视频游戏所耗时间百分比”和“每周所消费的冰淇淋公升数”。

![K-nearest-neighbor-algorithm2.1](https://MurphyHanxu.github.io/blogs-images/images/K-nearest-neighbor-algorithm2.1.png)



由于没有使用样本分类的特征值，我们很难从上图中看到任何有用的数据模式信息。一般来说，我们会采用色彩或其他的记号来标记不用样本分类，以便更好地理解数据信息。重新输入上面的代码，更改scatter函数：

```python 
def showdatas(datingDataMat, datingLabels):
    from matplotlib.font_manager import FontProperties
    import matplotlib.lines as mlines
    import matplotlib.pyplot as plt
    # 设置汉字格式
    font = FontProperties(fname=r"c:\windows\fonts\simsun.ttc", size=14)
    # 将fig画布分隔成1行1列,不共享x轴和y轴,fig画布的大小为(13,8)
    # 当nrow=2,nclos=2时,代表fig画布被分为四个区域,axs[0][0]表示第一行第一个区域
    fig, axs = plt.subplots(nrows=2, ncols=2, sharex=False, sharey=False, figsize=(13, 8))

    numberOfLabels = len(datingLabels)
    LabelsColors = []
    for i in datingLabels:
        if i == 1:
            LabelsColors.append('black')
        if i == 2:
            LabelsColors.append('orange')
        if i == 3:
            LabelsColors.append('red')
    # 画出散点图,以datingDataMat矩阵的第一(飞行常客例程)、第二列(玩游戏)数据画散点数据,散点大小为15,透明度为0.5
    axs[0][0].scatter(x=datingDataMat[:, 0], y=datingDataMat[:, 1], color=LabelsColors, s=15, alpha=.5)
    # 设置标题,x轴label,y轴label
    axs0_title_text = axs[0][0].set_title(u'每年获得的飞行常客里程数与玩视频游戏所消耗时间占比', fontproperties=font)
    axs0_xlabel_text = axs[0][0].set_xlabel(u'每年获得的飞行常客里程数', fontproperties=font)
    axs0_ylabel_text = axs[0][0].set_ylabel(u'玩视频游戏所消耗时间占', fontproperties=font)
    plt.setp(axs0_title_text, size=9, weight='bold', color='red')
    plt.setp(axs0_xlabel_text, size=7, weight='bold', color='black')
    plt.setp(axs0_ylabel_text, size=7, weight='bold', color='black')

    # 画出散点图,以datingDataMat矩阵的第一(飞行常客例程)、第三列(冰激凌)数据画散点数据,散点大小为15,透明度为0.5
    axs[0][1].scatter(x=datingDataMat[:, 0], y=datingDataMat[:, 2], color=LabelsColors, s=15, alpha=.5)
    # 设置标题,x轴label,y轴label
    axs1_title_text = axs[0][1].set_title(u'每年获得的飞行常客里程数与每周消费的冰激淋公升数', fontproperties=font)
    axs1_xlabel_text = axs[0][1].set_xlabel(u'每年获得的飞行常客里程数', fontproperties=font)
    axs1_ylabel_text = axs[0][1].set_ylabel(u'每周消费的冰激淋公升数', fontproperties=font)
    plt.setp(axs1_title_text, size=9, weight='bold', color='red')
    plt.setp(axs1_xlabel_text, size=7, weight='bold', color='black')
    plt.setp(axs1_ylabel_text, size=7, weight='bold', color='black')

    # 画出散点图,以datingDataMat矩阵的第二(玩游戏)、第三列(冰激凌)数据画散点数据,散点大小为15,透明度为0.5
    axs[1][0].scatter(x=datingDataMat[:, 1], y=datingDataMat[:, 2], color=LabelsColors, s=15, alpha=.5)
    # 设置标题,x轴label,y轴label
    axs2_title_text = axs[1][0].set_title(u'玩视频游戏所消耗时间占比与每周消费的冰激淋公升数', fontproperties=font)
    axs2_xlabel_text = axs[1][0].set_xlabel(u'玩视频游戏所消耗时间占比', fontproperties=font)
    axs2_ylabel_text = axs[1][0].set_ylabel(u'每周消费的冰激淋公升数', fontproperties=font)
    plt.setp(axs2_title_text, size=9, weight='bold', color='red')
    plt.setp(axs2_xlabel_text, size=7, weight='bold', color='black')
    plt.setp(axs2_ylabel_text, size=7, weight='bold', color='black')
    # 设置图例
    didntLike = mlines.Line2D([], [], color='black', marker='.',
                              markersize=6, label='didntLike')
    smallDoses = mlines.Line2D([], [], color='orange', marker='.',
                               markersize=6, label='smallDoses')
    largeDoses = mlines.Line2D([], [], color='red', marker='.',
                               markersize=6, label='largeDoses')
    # 添加图例
    axs[0][0].legend(handles=[didntLike, smallDoses, largeDoses])
    axs[0][1].legend(handles=[didntLike, smallDoses, largeDoses])
    axs[1][0].legend(handles=[didntLike, smallDoses, largeDoses])
    # 显示图片
    plt.show()
```

![K-nearest-neighbor-algorithm2.2](https://MurphyHanxu.github.io/blogs-images/images/K-nearest-neighbor-algorithm2.2.png)



由于利用颜色和尺寸标识了数据点的属性类别，因而我们基本上可以从上图看到数据点所属三个样本分类的区域轮廓。



### 2.3  准备数据：归一化数值

![K-nearest-neighbor-algorithm2.3](https://MurphyHanxu.github.io/blogs-images/images/K-nearest-neighbor-algorithm2.3.png)

我们很容易发现，数值插值大的属性对计算结果影响很大，也就是说，每年获取的飞行常客里程数对于计算结果的影响将远远大于其他两个特征的影响。而产生的这种现象的原因，仅仅是因为飞行常客里程数远大于其他特征值。但海伦认为这三种特征是同等重要的，因此作为三个等权重的特征之一，飞行常客里程数并不应该如此严重地影响到计算结果。

在处理不同取值范围的特征值时，我们通常采用的方法是将数值归一化，如将取值范围处理为0到1或-1到1之间。

我们需要在文件kNN.py中增加一个新函数autoNorm()，该函数可以自动将数字特征值转化为0到1的区间。

```python 
#  程序清单2.3.1 归一化特征值
def autoNorm(dataSet):
    minVals = dataSet.min(0)
    maxVals = dataSet.max(0)
    ranges = maxVals - minVals
    normDataSet = zeros(shape(dataSet))
    m = dataSet.shape[0]
    normDataSet = dataSet - tile(minVals, (m,1))
    normDataSet = normDataSet/tile(ranges, (m,1))   #element wise divide
    return normDataSet, ranges, minVals
```

在函数autoNorm()中，我们将每列的最小值放在变量minVals中，将最大值放在变量maxVals中，其中dataSet.min(0)中的参数0可以使得函数可以从列中选取最小值，而不是选取当前行的最小值。然后，函数计算可能的取值范围，并创建新的返回矩阵。需要注意的是，特征值矩阵有1000x3个值，而minVals和range的值都为1x3。为了解决这个问题，我们使用NumPy库中tile()函数将变量内容复制成输入矩阵同样大小的矩阵，注意这是具体特征值相除，而对于某些数值处理软件包，/可能意味着矩阵除法，但在NumPy库中，矩阵除法需要使用函数linalg.solve(matA, matB)。



### 2.4  测试算法：作为完整程序验证分类器

前面已经将数据按照需求进行了处理，本节我们将测试分类器的效果。如果分类器的正确率满足要求，海伦就可以使用这个软件来处理约会网站提供的约会名单了。机器学习算法一个很重要的工作就是评估算法的正确率，通常我们只提供已有数据的90%作为训练样本来训练分类器，而使用其余的10%数据去测试分类器，检测分类器的正确率。需要注意的是，10%测试数据应该是随机选择的，由于海伦提供的数据并没有按照特定目的来排序，所以我们可以随意选择10%数据而不影响其随机性。

前面我们已经提到可以使用错误率来检测分类器的性能。对于分类器来说，错误率就是分类器给出错误结果的次数除以测试数据的总数，完美分类器的错误率为0，而错误率为1的分类器不会给出任何正确的分类结果。代码里我们定义一个计数器变量，每次分类器错误地分类数据，计数器就加1，程序执行完成之后计数器的结果除以数据点总数即是错误率。

```python
#  程序清单2.4.1 分类器针对约会网站的测试代码
def datingClassTest():
    # 打开的文件名
    filename = "./datingTestSet.txt"
    # 将返回的特征矩阵和分类向量分别存储到datingDataMat和datingLabels中
    datingDataMat, datingLabels = file2matrix(filename)
    # 取所有数据的百分之十
    hoRatio = 0.10
    # 数据归一化,返回归一化后的矩阵,数据范围,数据最小值
    normMat, ranges, minVals = autoNorm(datingDataMat)
    # 获得normMat的行数
    m = normMat.shape[0]
    # 百分之十的测试数据的个数
    numTestVecs = int(m * hoRatio)
    # 分类错误计数
    errorCount = 0.0

    for i in range(numTestVecs):
        # 前numTestVecs个数据作为测试集,后m-numTestVecs个数据作为训练集
        classifierResult = classify0(normMat[i,:], normMat[numTestVecs:m,:],
            datingLabels[numTestVecs:m], 4)
        print("分类结果:%d\t真实类别:%d" % (classifierResult, datingLabels[i]))
        if classifierResult != datingLabels[i]:
            errorCount += 1.0
    print("错误率:%f%%" %(errorCount/float(numTestVecs)*100))
```

datingClassTest()首先使用了file2matrix()和autoNorm()函数从文件中读取数据并将其转换为归一化特征值。接着计算测试向量的数量，此步决定了normMat向量中哪些数据用于测试，哪些数据用于分类器的训练样本；然后将这两部分数据输入到原始kNN分类器函数classify0。最后，函数计算错误率并输出结果。我们可以改变函数datingClassTest()内变量hoRatio和变量k的值，检测错误率是否随着变量值的变化而增加。依赖于分类算法、数据集和程序设置，分类器的输出结果可能有很大的不同。



### 2.5  使用算法：构建完整可用系统

前面我们已经在数据上对分类器进行了测试，现在终于可以使用这个分类器为海伦来对人们分类。我们会给海伦一小段程序，通过该程序海伦会在约会网站上找到某个人并输入他的信息，程序会给出她对对方喜欢程度的预测值。

```python 
#  程序清单2.5.1  约会网站预测函数
def classifyPerson():
    # 输出结果
    resultList = ['讨厌','有些喜欢','非常喜欢']
    # 三维特征用户输入
    precentTats = float(input("玩视频游戏所耗时间百分比:"))
    ffMiles = float(input("每年获得的飞行常客里程数:"))
    iceCream = float(input("每周消费的冰激淋公升数:"))
    # 打开的文件名
    filename = "datingTestSet.txt"
    # 打开并处理数据
    datingDataMat, datingLabels = file2matrix(filename)
    # 训练集归一化
    normMat, ranges, minVals = autoNorm(datingDataMat)
    # 生成NumPy数组,测试集
    inArr = np.array([precentTats, ffMiles, iceCream])
    # 测试集归一化
    norminArr = (inArr - minVals) / ranges
    # 返回分类结果
    classifierResult = classify0(norminArr, normMat, datingLabels, 3)
    # 打印结果
    print("你可能%s这个人" % (resultList[classifierResult-1]))
```

接着我们只需输入数据，海伦即可得到预测结果。

![K-nearest-neighbor-algorithm2.4](https://MurphyHanxu.github.io/blogs-images/images/K-nearest-neighbor-algorithm2.4.png)
