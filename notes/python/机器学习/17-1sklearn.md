---
title: 17 sklearn
top: 17
tags:
  - python
categories:
  - python
---

中文：https://www.cntofu.com/book/170/index.html

<h4>评估模型指标</h4>

```python
from sklearn.datasets import load_boston
from sklearn.model_selection import cross_val_score
from sklearn.ensemble import RandomForestRegressor
import sklearn

boston = load_boston()
regressor = RandomForestRegressor(n_estimators=100, random_state=0)
cross_val_score(regressor, boston.data, boston.target, cv=10
                , scoring="neg_mean_squared_error")

# sklearn 当中的模型评估指标（打分）列表
for i in sorted(sklearn.metrics.SCORERS.keys()):
    print(i)
# accuracy
# adjusted_mutual_info_score
# adjusted_rand_score
# average_precision
# balanced_accuracy
# completeness_score
# explained_variance
# f1
# f1_macro
# f1_micro
# f1_samples
# f1_weighted
# fowlkes_mallows_score
# homogeneity_score
# jaccard
# jaccard_macro
# jaccard_micro
# jaccard_samples
# jaccard_weighted
# max_error
# mutual_info_score
# neg_brier_score
# neg_log_loss
# neg_mean_absolute_error
# neg_mean_absolute_percentage_error
# neg_mean_gamma_deviance
# neg_mean_poisson_deviance
# neg_mean_squared_error
# neg_mean_squared_log_error
# neg_median_absolute_error
# neg_root_mean_squared_error
# normalized_mutual_info_score
# precision
# precision_macro
# precision_micro
# precision_samples
# precision_weighted
# r2
# rand_score
# recall
# recall_macro
# recall_micro
# recall_samples
# recall_weighted
# roc_auc
# roc_auc_ovo
# roc_auc_ovo_weighted
# roc_auc_ovr
# roc_auc_ovr_weighted
# top_k_accuracy
# v_measure_score
```

<h4>减枝参数</h4>

树，在每次分枝时，没有使用全部特征，而是随机选取一部分的特征，从中选取相关指标（如不纯度）最优的作为分枝用的节点
    从而需要生成多个树，从中选取最优的
在不加限制的情况下，一颗决策树会生长到衡量不纯度的指标最优，或者没有更多的特征可用为止。
    这样的决策树往往会过拟合。即它在训练集上表现很好，但在测试集上表现糟糕
为了让决策树有更好的泛化性，需要对决策树进行减枝
criterion
    1.决策树需要找出最佳节点和最佳的分枝方法，对分类树来说，衡量这个“最佳”的指标叫做“不纯度”
    2.通常来说，不纯度越低，决策树对训练集的拟合越好。
    "entropy"，使用信息熵  
        信息熵对不纯度更加敏感 信息熵作为指标时，决策树的生长会更加“精细”
    "gini"，使用基尼系数 默认为"gini"
        对于高维数据或者噪音过多的数据，信息熵很容易过拟合，基尼系数在这种情况下效果往往比较好

random_state
    1.用来设置分枝中的随机模式的参数，默认为None
    2.在高维度（特征越多）时，随机性会表现更明显，低维度的数据，随机性几乎不会显现。
    3.输入任意整数，会一直长出同一棵树，让模型稳定下来

splitter

​    1.用来控制决策树中的随机选项，有两种输入值 默认为best
​    2.决策树在分枝时虽然随机，但还是会优先选择更重要的特征进行分枝（重要性可通过属性feature_importances_查看）
​    3.输入“random"，决策树在分枝时会更加随机，树会因为含有更多的不必要信息而更深更大，并因这些不必要信息而降低对训练集的拟合。这也是防止过拟合的一种方式

max_depth
    1.限制树的最大深度，超过设定深度的树枝全部剪掉。可以减少运算空间
    2.这是使用的最广泛的剪枝参数，在高维度低样本量时非常有效
    3.决策树多生长一层，对样本的需求会增加一倍，所以限制树深度能够有效地限制过拟合
    4.实际使用建议从3开始尝试，看效果再决定是否增加深度。

min_samples_split
    min_samples_split限定，一个节点必须要包含至少min_samples_split个训练样本，这个节点才允许被分枝，否则分枝就不会发生
    默认为 2

min_samples_leaf
    1.min_samples_leaf限定，一个节点在分枝后的每个子节点都必须包含至少min_samples_leaf个训练样本，否则分枝就不会发生
    2.一般搭配 max_depth，在回归树中会有神奇的效果，可以让模型更加平滑。在回归问题上可避免低方差
    3.设置过小，会造成过拟合，设置太大会阻止模型学习数据
    4.建议从5开始，如果叶节点中含有的样本变化很大的化，建议输入浮点数作为样本量的百分比来使用

max_features & min_impurity_decrease
    1.限制分枝时考虑的特征个数，超过限制个数的特征都会被舍弃。
    2.和max_depth异曲同工，max_features是用来限制高维度数据的过拟合的剪枝参数，但其方法比较暴力，是直接限制可以使用的特征数量而强行使决策树停下的参数，在不知道决策树中的各个特征的重要性的情况下，强行设定这个参数可能会导致模型学习不足
    3.如果希望通过降维的方式防止过拟合，建议使用PCA，ICA或者特征选择模块中的降维算法

class_weight & min_weight_fraction_leaf
    1.完成样本标签平衡的参数
    2.该参数默认None，此模式表示自动给与数据集中的所有标签相同的权重。即使样本不平衡，也会让其平衡。
    3.样本不平衡是指在一组数据集中，标签的一类天生占有很大的比例。比如说，在银行要判断“一个办了信用卡的人是否会违约”，就是 是vs否（1%：99%）的比例。这种分类状况下，即便模型什么也不做，全把结果预测成“否”，正确率也能有99%。因此我们要使用class_weight参数对样本标签进行一定的均衡，给少量的标签更多的权重，让模型更偏向少数类，向捕获少数类的方向建模。
    4.有了权重之后，样本量就不再是单纯地记录数目，而是受输入的权重影响了，因此这时候剪枝，就需要搭配min_weight_fraction_leaf这个基于权重的剪枝参数来使用。
    5.另请注意，基于权重的剪枝参数（例如min_weight_fraction_leaf）将比不知道样本权重的标准（比如min_samples_leaf）更少偏向主导类。
    6.如果样本是加权的，则使用基于权重的预修剪标准来更容易优化树结构，这确保叶节点至少包含样本权重的总和的一小部分<br>对于类别不多的分类问题，选择1通常是最佳选择 默认为1

<h4>交叉验证</h4>

交叉验证是用来观察模型的稳定性的一种方法，我们将数据划分为n份，依次使用其中一份作为测试集，其他n-1份作为训练集，多次计算模型的精确性来评估模型的平均准确程度。训练集和测试集的划分会干扰模型的结果，因此，用交叉验证n次的结果求出的平均值，是对模型效果的一个更好的度量

