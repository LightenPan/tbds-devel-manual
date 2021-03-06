## 分类算法

* [Logistic Regression](#1-logistic-regression)
* [Naive Bayes](#2-naive-bayes)
* [SVM](#3-svm)
* [SparseLogisticRegression](#4-sparse-logistic-regression)
* [RandomForest](#5-randomforest)
* [DecisionTree](#6-decisiontree)
### 1. Logistic Regression

* **算法说明**

  LR算法是一种最常见的分类算法，因其模型简单、可解释性强等特点在工程领域得到广泛应用。目前实现的LR算法仅支持二分类。

* **训练节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| label \| 参与计算的features \| 不参与计算的features \|
    * label：Double类型，且仅存在0或1。通过**算法参数**中的**数据标签列**指定
    * 参与计算的features：可通过**算法参数**的**选择特征列**指定
    * 不参与计算的features：可包括不参与计算的特征
  * 输出
    * PMML格式的LR model
    * 模型格式：\| 类别数\| 特征个数 \| 偏置项 \| 特征权重 \|
  * 算法参数
    * 数据标签列：数据标签\(label\)所在列，从0开始计数，默认0
    * 选择特征列：从0开始计数，以类似“1-12,15”的方式选择特征，表示取特征在表中的1到12列，15列
    * 并行数：训练数据的分区数、spark的并行数
    * 抽样率：输入数据的采样率
    * 正则化系数：默认为0，表示不添加正则项
    * elasticNet：范围：0~1.0，0表示L2正则，1表示L1正则，0~1.0表示L1和L2正则的结合，默认为1.0
    * 最大迭代次数：默认为100

* **预测节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| 参与计算的features \| 不参与计算的features \|
    * 参与计算的features：同训练节点
    * 不参与计算的features：如果存在则保留在输出中,对于Libsvm格式的数据，也包括了label列，该label仅用于标识样本ID
  * 输出
    * 数据形式：Dense
    * 格式：\| 不参与计算的features \| probabilities \|
    * probabilities：两列，分别对应该样本属于0和1的概率

### 2. Naive Bayes

* **算法说明**

  朴素贝叶斯是一种常用的多类分类算法，该算法假设各个特征之间是相互独立的，通过贝叶斯公式计算出某个样本属于某个类别的概率。朴素贝叶斯算法有multinomial naive Bayes和Bernoulli naive Bayes两种。该算法常用于文本分类，每个特征表示词在一篇文档的出现的次数（multinomial naive Bayes）或者是否出现（Bernoulli naive Bayes，用0，1表示的特征）。

* **训练节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| label \| 参与计算的features \| 不参与计算的features \|
    * label：Double类型，通过**算法参数**中的**数据标签列**指定
    * 参与计算的features：可通过**算法参数**的**选择特征列**指定
    * 不参与计算的features：可包括不参与计算的特征
  * 输出
    * PMML格式的naive bayes model
    * 模型格式：\| 类别列表 \| 各类别的先验概率 \| 各类别的条件概率 \| 模型类型 \|
  * 算法参数
    * 数据标签列：数据标签\(label\)所在列，从0开始计数
    * 选择特征列：从0开始计数，以类似“1-12,15”的方式选择特征，表示取特征在表中的1到12列，15列
    * 并行数：训练数据的分区数、spark的并行数
    * 抽样率：输入数据的采样率
    * 模型类型：multinomial or bernoulli

* **预测节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| 参与计算的features \| 不参与计算的features \|
    * 参与计算的features：同训练节点
    * 不参与计算的features：如果存在则保留在输出中,对于Libsvm格式的数据，也包括了label列，该label仅用于标识样本ID
  * 输出
    * 数据形式：Dense
    * 格式：\| 不参与计算的features \| probabilities \|
    * probabilities：分别对应该样本属于每个类别的概率，类别按数值大小升序排列

### 3. SVM

* **算法说明**

  当前平台提供了线性SVM，该算法通过SGD优化带有正则项的目标函数，不支持核函数的选择，label必须是0或者1。

* **训练节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| label \| 参与计算的features \| 不参与计算的features \|
    * label：Double类型，且仅存在0或1。通过**算法参数**中的**数据标签列**指定
    * 参与计算的features：可通过**算法参数**的**选择特征列**指定
    * 不参与计算的features：可包括不参与计算的特征
  * 输出
    * PMML格式的SVM model
    * 模型格式：\| 特征个数\| 类别个数\| 特征权重 \| 偏置项 \| 阈值 \|
  * 算法参数：
    * 数据标签列：数据标签\(label\)所在列，从0开始计数
    * 选择特征列：从0开始计数，以类似“1-12,15”的方式选择特征，表示取特征在表中的1到12列，15列
    * 并行数：训练数据的分区数、spark的并行数
    * 抽样率：输入数据的采样率
    * SGD步长：SGD参数迭代的步长，默认1.0
    * SGD优化数据量：使用多大百分比的数据做SGD优化。默认值1.0
    * 正则化参数：默认值0.01
    * 最大迭代次数：默认100

* **预测节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| 参与计算的features \| 不参与计算的features \|
    * 参与计算的features：同训练节点
    * 不参与计算的features：如果存在则保留在输出中,对于Libsvm格式的数据，也包括了label列，该label仅用于标识样本ID
  * 输出
    * 数据形式：Dense
    * 格式：\| 不参与计算的features数据 \| predict\_value \|
    * predict\_value：0-1之间的预测值，double类型

### 4. Sparse Logistic Regression

* **算法说明**

  基于Dummy模块生成的instances目录下的One-Hot训练集，高维稀疏的Logistic Regression模型。该LR算法是在模型上优化了ADMM算法。ADMM是Boyd提出的优化算法，ADMM算法因为其收敛快，易于分布式计算，被广泛应用于大规模优化的场景中。  
  Sparse Logistic Regression需要将“Dummy”特征转换模块作为前置节点生成one-hot特征。“Dummy”模块的说明使用，[请戳]()

* **训练节点**

  * 输入
    * 格式：\|features\|
    * 说明：one-hot格式的特征，以","连接    
  * 输出
    * text格式的模型
    * \| 特征索引 \| 特征权重 \|
  * 算法参数：
    * 并行数：训练数据的分区数、spark的并行数    
    * 抽样率：输入数据的采样率
    * 子模型数：并行训练的ADMM子模型数，不宜设置太大，一般小于100，参考值：10 ~ 100
    * L1-Norm：L1正则项参数，正整数，参考值：0.01
    * rho：对偶变量更新迭代步长，同时也是拉格朗日惩罚项系数，整数，根据L1-norm、子模型数和kappa值调整
    * ADMM迭代次数：循环迭代次数，默认20，整数，参考值：10 ~ 20
  * 其他：
    * 并行数是子模型数的N倍，N可以是5~10
    * 配置的excutor数最好等于子模型数，或者是子模型数的1/n
    * kappa = L1-Norm /（rho \* 子模型数\)。ADMM通过kappa值控制LR模型的稀疏度，特征的权重小于kappa将会被赋值为0。因此，可以通过调整L1-Norm、rho和子模型数控制生成模型的稀疏度。 一般kappa要小于0.001

* **预测节点**

  * 输入
    * 格式：\|features\|
    * 说明：one-hot格式的特征，以","连接，且与训练集的特征映射保持一致
  * 输出
    * 格式：\| target列 \| 预测的概率 \|

### 5. RandomForest

* **算法说明**

  随机森林是决策树的一种ensemble算法，可用于分类和回归。平台上的RandomForest分类算法支持连续、非连续特征的多分类任务。

* **训练节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| label \| 参与计算的features \| 不参与计算的features \|
    * label：Double类型，通过**算法参数**中的**数据标签列**指定
    * 参与计算的features：可通过**算法参数**的**选择特征列**指定
    * 不参与计算的features：可包括不参与计算的特征
  * 输出
    * PMML格式的RandomForest model
    * 模型格式：\| treeId \| node \|，所有组成森林的随机树
    * treeId：树的标号
    * node：树中的节点信息
  * 算法参数：
    * 数据标签列：数据标签\(label\)所在列，从0开始计数
    * 选择特征列：从0开始计数，以类似“1-12,15”的方式选择特征，表示取特征在表中的1到12列，15列
    * 并行数：训练数据的分区数、spark的并行数
    * 标签类别个数：待训练数据label类别数\(_必填_, 数据需处理成从0开始连续编号的形式\)
    * 决策树棵树：模型使用的决策树的个数，默认为10
    * 决策树最大深度： 训练决策树所允许的高度上限, 默认为10
    * 决策树最大分支数： 训练决策树所允许的最大分叉树, 默认为32
    * 抽样率： 训练每课数使用的数据百分比，默认为1.0, 减少可以加快训练过程
    * 标签抽样策略：训练决策树选择标签的策略
      * auto：自动选择，当决策树棵树为1时选择all，否则选择sqrt
      * all：选择全部标签
      * sqrt：选择开根号个数的标签
      * log：选择取对数个数的标签
    * 决策树特征选择算法：
      * Gini：基尼指数
      * Entropy：熵

* **预测节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| 参与计算的features \| 不参与计算的features \|
    * 参与计算的features：同训练节点
    * 不参与计算的features：如果存在则保留在输出中,对于Libsvm格式的数据，也包括了label列，该label仅用于标识样本ID
  * 输出
    * 数据形式：Dense
    * 格式：\| 不参与计算的features \| predict\_value \|
    * predict\_value：预测的类别，根据投票决定

### 6. DecisionTree

* **算法说明**

  决策树算法是机器学习中非常常用的一类分类/回归算法。决策树算法有很多优点，如，解释性好，可以处理类别特征，支持多分类，不需要做特征scaling，可以表示非线性模型。平台上的决策树分类算法支持连续、非连续特征的多分类任务，最高可以支持百万级别的样本。

* **训练节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| label \| 参与计算的features \| 不参与计算的features \|
    * label：Double类型，通过**算法参数**中的**数据标签列**指定
    * 参与计算的features：可通过**算法参数**的**选择特征列**指定
    * 不参与计算的features：可包括不参与计算的特征
  * 输出
    * PMML格式的DecisionTree model
    * 模型格式：\| treeId \| node \|，决策树中的所有节点信息
      * treeId：树标号，都为0
      * node：树中的节点信息
    * 算法参数
      * 数据标签列：数据标签\(label\)所在列，从0开始计数
      * 选择特征列：从0开始计数，以类似“1-12,15”的方式选择特征，表示取特征在表中的1到12列，15列
      * 并行数：训练数据的分区数、spark的并行数
      * 标签类别个数：待训练数据label类别数\(_必填_, 数据需处理成从0开始连续编号的形式\)
      * 决策树最大深度：训练决策树所允许的高度上限, 默认为10
      * 决策树最大分支数：训练决策树所允许的最大分叉树, 默认为32
      * 决策树特征选择方法：Gini：基尼指数；Entropy：熵

* **预测节点**

  * 输入
    * 数据形式：[Dense或Libsvm](./tdw_ml_jarvis_dataformat.md#2-数据格式要求)
    * 格式：\| 参与计算的features \| 不参与计算的features \|
    * 参与计算的features：同训练节点
    * 不参与计算的features：如果存在则保留在输出中,对于Libsvm格式的数据，也包括了label列，该label仅用于标识样本ID
  * 输出
    * 数据形式：Dense
    * 格式：\| 不参与计算的features数据 \| predict\_value \|
    * predict\_value：预测的类别
