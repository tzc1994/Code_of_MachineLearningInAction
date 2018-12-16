作业1：概括决策树分类算法
    step1:划分数据，划分依据是信息增益的度量，需要找出一个信息增益最大的特征来分类
```python
    def chooseBestFeatureToSplit(dataSet):
    numFeatures = len(dataSet[0]) - 1
    baseEntropy = calShannonEnt(dataSet)
    bestInfoGain = 0.0
    bestFeature = -1
    for i in range(numFeatures):
        featList = [example[i] for example in dataSet]
        uniqueVals = set(featList)
        newEntropy = 0.0
        for value in uniqueVals:
            subDataSet = splitDataSet(dataSet, i, value)
            prob = len(subDataSet) / float(len(dataSet))
            newEntropy += prob*calShannonEnt(subDataSet)
        infoGain = baseEntropy - newEntropy
        if infoGain > bestInfoGain:
            bestInfoGain = infoGain
            bestFeature = i
    return bestFeature
```
    step2:构建树-
          数据集的最后一列为真实的分类，如果最后一列标签全一样即是一类，不需再分
          如果只有一列特征，则按特征的数量从高到低排序，返回这样一个最高类的label
          选择最优的分类特征，选择该特征对应的node-Label,产生subDataSet,集删除掉该
          最优特征这一列，然后利用subDataSet和下一个node-Label重复这样的操作，即递归，
          将结果保存再嵌套的dictionary里。
```python
    def createTree(dataSet, labels):
    classList = [example[-1] for example in dataSet]
    if classList.count(classList[0]) == len(classList):
        return classList[0]
    if len(dataSet[0]) == 1:
        return majorityCnt(classList)
    bestFeat = chooseBestFeatureToSplit(dataSet)
    bestFeatLabel = labels[bestFeat]
    myTree = {bestFeatLabel:{}}
    del(labels[bestFeat])
    featValues = [exampl[bestFeat] for example in dataSet]
    uniqueVals = set(featValues)
    for value in uniqueVals:
        subLabels = labels[:]
        myTree[bestFeatLabel][value] = createTree(splitDataSet(dataSet, bestFeat, value), subLabels)
    return myTree
```
### Tips:
    决策树的学习算法包含特征选择，决策树的生成与决策树的剪枝过程.
    