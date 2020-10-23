# 模型的训练与使用

> 简单的说， 机器学习是把原始的大数据，通过算法的训练，生成有用的模型，确定模型的准确度后，再用这些模型去预测未知的数据。TDInsight在已有丰富的算法库和计算框架，以及数据的的基础，支持了模型特性。通过训练， 保存， 配置，使用等完成整个过程，并最大程度地与原有的工作流体验保持一致。

## 1. 模型训练

适合进行模型训练的算法可配置生成模型训练节点（哪些算法可进行模型训练由管理员进行配置，配置过程“乐高化”，设置后发布即可使用）。

用户使用时，拖拽一个这样的节点到画布，这个节点的前面会自动带一个模型生成节点。如下图

模型生成节点通过图标上烧杯的空和满标识生成的过程状态。

![](../../.gitbook/assets/model1.png)

**模型观察台：**

可以查看模型的属性，深度学习算法还支持动态信息展示

![](../../.gitbook/assets/model2.png)

## 2. 模型保存，收藏和更新

### 1\)    保存规则：

TDInsight使用默认的静态路径和动态路径保存模型。每个“模型生成节点”的实例都会在静态路径下保存一份模型。只有当用户收藏模型或更新模型的时候才会存储到动态路径下。

### 2\)    收藏：

右键模型生成节点，点击收藏就可以把生成的模型存入个人模型。为了增加模型的辨识度， 支持重命名

### 3\)    取消收藏：

与普通节点不同的是，用户左侧的模型工具栏中模型库是可以操作的，如果不再需要某个收藏过的模型， 可以取消收藏。

![](../../.gitbook/assets/model3.png)

### 4\)    TDInsight的模型更新机制：

模型生成之后，为了得到更好的模型结果，可能是需要再次训练的。为此，TDInsight配备了模型双向更新机制。这种双向的模型更新机制，提供了最灵活解耦方式。训练者和使用者，都有充分的自由，去判断和决定是否要更新模型，以及是否要使用最新的模型，达到最佳的模型效果。

**模型训练者，可以选择是否更新模型，以及哪种运行方式更新模型。**

![](../../.gitbook/assets/model4.jpg)

还可以手动的，在任务实例页面，进行手工模型更新。如果同一个模型（同一个流的同一个节点生成的）已经被用户收藏过，那么右键菜单的“收藏”会变成更新。

![](../../.gitbook/assets/model5.jpg)

**而模型的使用者，也可以选择是否跟随母体更新**，如下。

不跟随更新的时候，模型节点就会冻结，固定使用目前的模型参数，不再变化。当然了，一旦重新开启，又可以随着母体进行动态更新了。

![](../../.gitbook/assets/model6.jpg)

一般情况下， 可以使用多参数实例进行批量的参数调优， 并使用可视化节点对模型的效果进行评估。再选择效果较好的一个模型进行更新。

## 3. 模型节点的使用

训练好的模型可以用来进行预测， 模型节点的使用和其他算法节点一致， 只需要选择后拖拽到画布，配置好参数后即可执行。一般在模型节点之后会跟随一个可视化的评估节点。

拖拽后， 同样需要为模型节点设置一下参数（有哪些参数也是管理员在配置模型生成节点的时候就指定的）。一般包含算法参数，输入参数和输出参数，资源参数， 以及一个“模型更新”的开关参数。如下图。

模型信息和状态： 模型节点鼠标hover上会展示3个部分的信息：

--**静态信息**：模型ID，名称，大小，类型等。由生成模型的节点类型决定。

**节点信息**：模型节点在本工作流中的信息

**执行信息：**上次或当前运行时间，结果等信息。

模型的状态和节点一样， 有就绪，运行，成功，终止，失败等。 模型右键菜单也和生成它的节点类型一致。 比如LR算生成的LR模型，其右键菜单就有LR算法节点一致。

又可以随着母体进行动态更新了。

![](../../.gitbook/assets/model7.png)

> 注： 当参数“跟随母体更新”开启时，表示模型执行时会从模保存的动态路径上获取最近一次训练的模型覆盖现有的模型， 见上一节的更新规则。

## 4.模型自动运行

除去上面模型的收藏后再预测，TDInsight目前又提供了一种新的模型使用方式：用户在配置算法时可以自主选择模型是否自动运行，如果自动运行，那么在训练过程完成之后就会自动使用生成的模型来进行预测过程.

具体设置如下： 点击代表模型的小尾巴，就会在右侧出现如下的配置：

 ![](../../.gitbook/assets/model8.png) 如果将此开关打开，那么在训练完成之后会自动进入运行状态，如下： ![](../../.gitbook/assets/model9.png)

如果将此开关关闭，那么在训练完成之后，不会进行预测过程，会保持为如下状态：

 ![](../../.gitbook/assets/model10.png) 不管自动运行与否，用户均可以继续进行模型收藏等操作。
