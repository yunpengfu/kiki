# 第3章 决策树
* 决策树（Decision Tree）算法是一种基本的分类与回归方法，是最经常使用的数据挖掘算法之一。我们这章节只讨论用于分类的决策树。  
* 决策树模型呈树形结构，在分类问题中，表示基于特征对实例进行分类的过程。它可以认为是 if-then 规则的集合，也可以认为是定义在特征空间与类空间上的条件概率分布。  
> 决策树学习通常包括 3 个步骤：特征选择、决策树的生成和决策树的修剪。 

决策树的定义：    
* 分类决策树模型是一种描述对实例进行分类的树形结构。决策树由结点（node）和有向边（directed edge）组成。结点有两种类型：内部结点（internal node）和叶结点（leaf node）。内部结点表示一个特征或属性(features)，叶结点表示一个类(labels)。  

## 决策树 原理
用决策树对需要测试的实例进行分类：从根节点开始，对实例的某一特征进行测试，根据测试结果，将实例分配到其子结点；这时，每一个子结点对应着该特征的一个取值。如此递归地对实例进行测试并分配，直至达到叶结点。最后将实例分配到叶结点的类中。  
图3-1构造了一个假想的邮件分类系统，他首先检测发送邮件域名地址。如果地址为myEmployer.com，则将其放在分类“无聊时需要阅读的邮件”中。如果邮件不是来自这个域名，则检查邮件内容是否包含单词“曲棍球”，如果包含则将邮件归类到“需要及时处理的朋友邮件”，如果不包含则将邮件归类到“无需阅读的垃圾邮件”。
![邮件分类系统](https://raw.githubusercontent.com/apachecn/AiLearning/master/img/ml/3.DecisionTree/%E5%86%B3%E7%AD%96%E6%A0%91-%E6%B5%81%E7%A8%8B%E5%9B%BE.jpg "邮件分类系统")
### 决策树的构造
在构造决策树时，我们需要解决的第一个问题就是，当前数据集上哪个特征在划分数据分类时起决定性作用。为了找到决定性的特征，划分出最好的结果，我们必须评估每个特征。完成测试之后，原始数据被划分为几个数据子集。这些数据子集会分布在第一个决策点的所有分支上。如果某个数据下的数据属于同一类型，则当前无需阅读的垃圾邮件已经正确地划分数据分类，无需进一步对数据集进行分类。如果数据子集内的数据不属于同一类型，则需要重复划分数据子集的过程。划分数据自己的算法和划分原始数据集的方法相同，直到所有具有相同类型的数据均在一个数据子集内。  
创建分支的伪代码函数createBranch() 如下所示：
```
  def createBranch():
  '''
  此处运用了迭代的思想。 感兴趣可以搜索 迭代 recursion， 甚至是 dynamic programing。
  '''
    检测数据集中的所有数据的分类标签是否相同:
        If so return 类标签
        Else:
            寻找划分数据集的最好特征（划分之后信息熵最小，也就是信息增益最大的特征）
            划分数据集
            创建分支节点
                for 每个划分的子集
                    调用函数 createBranch （创建分支的函数）并增加返回结果到分支节点中
            return 分支节点
```
### 信息熵 & 信息增益
熵（entropy）： 熵指的是体系的混乱的程度，在不同的学科中也有引申出的更为具体的定义，是各领域十分重要的参量。  
信息论（information theory）中的熵（香农熵）： 是一种信息的度量方式，表示信息的混乱程度，也就是说：信息越有序，信息熵越低。例如：火柴有序放在火柴盒里，熵值很低，相反，熵值很高。  
信息增益（information gain）： 在划分数据集前后信息发生的变化称为信息增益。  
/补充
### 决策树 算法特点
优点：计算复杂度不高，输出结果易于理解，数据有缺失也能跑，可以处理不相关特征。  
缺点：容易过拟合。  
适用数据类型：数值型和标称型。  
### 决策树 开发流程
收集数据：可以使用任何方法。  
准备数据：树构造算法 (这里使用的是ID3算法，只适用于标称型数据，这就是为什么数值型数据必须离散化。 还有其他的树构造算法，比如CART)  
分析数据：可以使用任何方法，构造树完成之后，我们应该检查图形是否符合预期。  
训练算法：构造树的数据结构。  
测试算法：使用训练好的树计算错误率。  
使用算法：此步骤可以适用于任何监督学习任务，而使用决策树可以更好地理解数据的内在含义。  


## 决策树 代码
表3-1 海洋生物数据  
<table>
  <tr>
    <th></th>
    <th>不浮出水面是否可以生存</th>
    <th>是否有脚蹼</th>
    <th>是否有脚蹼</th>
  </tr>
  <tr>
    <td>1</td>
    <td>是</td>
    <td>是</td>
    <td>是</td>
  </tr>
  <tr>
    <td>2</td>
    <td>是</td>
    <td>是</td>
    <td>是</td>
  </tr>
  <tr>
    <td>3</td>
    <td>是</td>
    <td>否</td>
    <td>否</td>
  </tr>
  <tr>
    <td>4</td>
    <td>否</td>
    <td>是</td>
    <td>否</td>
  </tr>
  <tr>
    <td>5</td>
    <td>否</td>
    <td>是</td>
    <td>否</td>
  </tr>
</table>  

1. 构造表3-1所示简单鱼鉴定数据集createDataSet()函数：
```
  def createDataSet():
    dataSet = [[1, 1, 'yes'],
               [1, 1, 'yes'],
               [1, 0, 'no'],
               [0, 1, 'no'],
               [0, 1, 'no']]
    labels = ['no surfacing', 'flippers']
    return dataSet, labels
```  
2. 函数calcShannonEnt(dataSet)计算给定数据集的香农熵：  
> 计算香农熵的公式：H = -求和（i=1，2，3……n）p（Xi）log2p（Xi）
```
  def calcShannonEnt(dataSet):
    # 计算香农熵的第一种实现方式start
    numEntries = len(dataSet) # 求list的长度，表示计算参与训练的数据量
    # 下面输出我们测试的数据集的一些信息
    # 例如：<type 'list'> numEntries:  5 是下面的代码的输出
    # print type(dataSet), 'numEntries: ', numEntries

    # 计算分类标签label出现的次数
    labelCounts = {}
    for featVec in dataSet:
        currentLabel = featVec[-1]  # 将当前实例的标签存储，即每一行数据的最后一个数据代表的是标签
        # 为所有可能的分类创建字典，如果当前的键值不存在，则扩展字典并将当前键值加入字典。每个键值都记录了当前类别出现的次数。
        if currentLabel not in labelCounts.keys():
            labelCounts[currentLabel] = 0
        labelCounts[currentLabel] += 1

    # 对于label标签的占比，求出label标签的香农熵
    shannonEnt = 0.0
    for key in labelCounts:
        # 使用所有类标签的发生频率计算类别出现的概率。
        prob = float(labelCounts[key])/numEntries
        shannonEnt -= prob * log(prob, 2) # 计算香农熵，以 2 为底求对数

    # 计算香农熵的第二种实现方式
    # # 统计标签出现的次数
    # label_count = Counter(data[-1] for data in dataSet)
    # # 计算概率
    # probs = [p[1] / len(dataSet) for p in label_count.items()]
    # # 计算香农熵
    # shannonEnt = sum([-p * log(p, 2) for p in probs])
    return shannonEnt
```
3. 按照给定特征划分数据集
```
  def splitDataSet(dataSet, index, value):  # 待划分的数据集dataSet, 划分数据集的特征index,需要返回的特征的值value
    # 切分数据集的第一种方式
    retDataSet = []
    for featVec in dataSet: 
        # index列为value的数据集【该数据集需要排除index列】
        # 判断index列的值是否为value
        if featVec[index] == value:
            # [:index]表示前index行，即若 index 为2，就是取 featVec 的前 index 行
            reducedFeatVec = featVec[:index]
            reducedFeatVec.extend(featVec[index+1:])
            # [index+1:]表示从跳过 index 的 index+1行，取接下来的数据
            # 收集结果值 index列为value的行【该行需要排除index列】
            retDataSet.append(reducedFeatVec)

    # # 切分数据集的第二种方式 start
    # retDataSet = [data for data in dataSet for i, v in enumerate(data) if i == axis and v == value]
    return retDataSet
```
4. 函数chooseBestFeatureToSplit()选择最好的数据集划分方式：
```
  def chooseBestFeatureToSplit(dataSet):
    # 选择最优特征的第一种方式
    numFeatures = len(dataSet[0]) - 1    # 求第一行有多少列的 Feature, 最后一列是label列嘛
    baseEntropy = calcShannonEnt(dataSet)   # label的信息熵
    # 最优的信息增益值, 和最优的Featurn编号
    bestInfoGain, bestFeature = 0.0, -1
    for i in range(numFeatures):
        # 获取每一个实例的第i+1个feature，组成list集合
        featList = [example[i] for example in dataSet]
        # 获取剔重后的集合，使用set对list数据进行去重
        uniqueVals = set(featList)
        # 创建一个临时的信息熵
        newEntropy = 0.0
        # 遍历某一列的value集合，计算该列的信息熵 
        # 遍历当前特征中的所有唯一属性值，对每个唯一属性值划分一次数据集，计算数据集的新熵值，并对所有唯一特征值得到的熵求和。
        for value in uniqueVals:
            subDataSet = splitDataSet(dataSet, i, value)
            prob = len(subDataSet)/float(len(dataSet))
            newEntropy += prob * calcShannonEnt(subDataSet)
        # gain[信息增益]: 划分数据集前后的信息变化， 获取信息熵最大的值
        # 信息增益是熵的减少或者是数据无序度的减少。最后，比较所有特征中的信息增益，返回最好特征划分的索引值。
        infoGain = baseEntropy - newEntropy
        print('infoGain=', infoGain, 'bestFeature=', i, baseEntropy, newEntropy)
        if (infoGain > bestInfoGain):
            bestInfoGain = infoGain
            bestFeature = i
    return bestFeature

    # # 选择最优特征的第二种方式
    # # 计算初始香农熵
    # base_entropy = calcShannonEnt(dataSet)
    # best_info_gain = 0
    # best_feature = -1
    # # 遍历每一个特征
    # for i in range(len(dataSet[0]) - 1):
    #     # 对当前特征进行统计
    #     feature_count = Counter([data[i] for data in dataSet])
    #     # 计算分割后的香农熵
    #     new_entropy = sum(feature[1] / float(len(dataSet)) * calcShannonEnt(splitDataSet(dataSet, i, feature[0])) \
    #                    for feature in feature_count.items())
    #     # 更新值
    #     info_gain = base_entropy - new_entropy
    #     print('No. {0} feature info gain is {1:.3f}'.format(i, info_gain))
    #     if info_gain > best_info_gain:
    #         best_info_gain = info_gain
    #         best_feature = i
    # return best_feature
```
5. 函数majorityCnt()选择出现次数最多的一个结果：
```
  def majorityCnt(classList):
    # majorityCnt的第一种方式
    classCount = {}
    for vote in classList:
        if vote not in classCount.keys():
            classCount[vote] = 0
        classCount[vote] += 1
    # 倒叙排列classCount得到一个字典集合，然后取出第一个就是结果（yes/no），即出现次数最多的结果
    sortedClassCount = sorted(classCount.iteritems(), key=operator.itemgetter(1), reverse=True)
    # print 'sortedClassCount:', sortedClassCount
    return sortedClassCount[0][0]

    # # majorityCnt的第二种方式
    # major_label = Counter(classList).most_common(1)[0]
    # return major_label
```
6. 函数createTree(dataSet, labels)创建树：
```
  def createTree(dataSet, labels):
    classList = [example[-1] for example in dataSet]
    # 如果数据集的最后一列的第一个值出现的次数=整个集合的数量，也就说只有一个类别，就只直接返回结果就行
    # 第一个停止条件：所有的类标签完全相同，则直接返回该类标签。
    # count() 函数是统计括号中的值在list中出现的次数
    if classList.count(classList[0]) == len(classList):
        return classList[0]
    # 如果数据集只有1列，那么最初出现label次数最多的一类，作为结果
    # 第二个停止条件：使用完了所有特征，仍然不能将数据集划分成仅包含唯一类别的分组。
    if len(dataSet[0]) == 1:
        return majorityCnt(classList)

    # 选择最优的列，得到最优列对应的label含义
    bestFeat = chooseBestFeatureToSplit(dataSet)
    # 获取label的名称
    bestFeatLabel = labels[bestFeat]
    # 初始化myTree
    myTree = {bestFeatLabel: {}}
    # 注：labels列表是可变对象，在PYTHON函数中作为参数时传址引用，能够被全局修改
    # 所以这行代码导致函数外的同名变量被删除了元素，造成例句无法执行，提示'no surfacing' is not in list
    del(labels[bestFeat])
    # 取出最优列，然后它的branch做分类
    featValues = [example[bestFeat] for example in dataSet]
    uniqueVals = set(featValues)
    for value in uniqueVals:
        # 求出剩余的标签label
        subLabels = labels[:]
        # 遍历当前选择特征包含的所有属性值，在每个数据集划分上递归调用函数createTree()
        myTree[bestFeatLabel][value] = createTree(splitDataSet(dataSet, bestFeat, value), subLabels)
        # print 'myTree', value, myTree
    return myTree
```
