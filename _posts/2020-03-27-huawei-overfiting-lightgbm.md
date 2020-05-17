---
layout: post
title: '机器学习中的过拟合和玄学调参'
date: 2020-03-27
author: Only
tags: 过拟合 LightGBM 机器学习
---

> 参加"华为网络人工智能黑客松大赛-硬盘异常检测"遇到的问题和一些尝试。

### 过拟合（Over-Fitting）

#### 什么是过拟合？

* 过拟合是模型在训练集和测试集上的结果有差异性，具体表现为在训练集上的表现很好，而在测试集上的表现很差。

<img src="http://onlywyj.gitee.io/image_bed/blog/2020-03-27-01.png" alt="01"  />

* 如上图，在我们黑客松的比赛中，算法运行后`validation set fdr score:`的得分会比`test set fdr score:`的高。但是两者如果相差不大的话，拟合就是适度的，不必过度担心。
* 值得注意的是：**过拟合是几乎无法被消除的，只可以对其抑制。**

#### 过拟合产生常见的原因

* 数据有噪声
* 训练数据不足，有限的训练数据
* 训练模型过度导致模型非常复杂

#### 过拟合的解决方法

* 清洗无效数据
* 增加训练集的数据量
* 降低模型复杂度，缩小宽度和减小深度
* Dropout
* 正则化
* 贝叶斯方法

还有很多，就不一一列举了，下面说一下通过调参的方式抑制过拟合（因为上面的那些方法没怎么用过~~）

#### LightGBM玄学调参(一时调参一时爽，天天调参天天爽)

<img src="http://onlywyj.gitee.io/image_bed/blog/2020-03-27-02.png" alt="02" style="zoom: 67%;" />

* 使用较小的 `max_bin`
* 使用较小的 `num_leaves`
* 使用 `min_data_in_leaf 和 min_sum_hessian_in_leaf`
* 通过设置 `bagging_fraction` 和 `bagging_freq` 来使用 bagging
* 通过设置 `feature_fraction `
* 使用`lambda_l1`, `lambda_l2` 和 `min_gain_to_split` 来使用正则
* 尝试 `max_depth` 来避免生成过深的树
* 尝试 `extra_trees`

经过测试发现，使用`feature_fraction`时效果达到了最好，虽然不多

![03](http://onlywyj.gitee.io/image_bed/blog/2020-03-27-03.png)

#### 预期之外的结果

<img src="http://onlywyj.gitee.io/image_bed/blog/2020-03-27-04.png" alt="04" style="zoom:80%;" />

* 测试集的分数居然比训练集高，这种结果既不是过拟合，也不是欠拟合，我感觉我发现了一个新大陆。所以本着讲(hou)究(yan)科(wu)学(chi)的态度,我将此种现象命名为**“超拟合”**，英文名为**“Chao-fitting”**，即为超过预期的意思。

<img src="http://onlywyj.gitee.io/image_bed/blog/2020-03-27-05.png" alt="05" style="zoom:80%;" />

好吧，其实是使用`boosting_type='dart'`后出现的这种情况，Dropout参数设置的有点高，就一点点~

<img src="http://onlywyj.gitee.io/image_bed/blog/2020-03-27-06.png" alt="06" style="zoom: 50%;" />

### 各种深度学习架构，模型和技巧的集合

* 在查资料的时候无意中发现的：https://github.com/rasbt/deeplearning-models

### 神经网络游乐场

* 网页端的神经网络，可以拿来玩一玩： [Tensorflow — Neural Network Playground](http://playground.tensorflow.org/#activation=tanh&batchSize=10&dataset=circle&regDataset=reg-plane&learningRate=0.03&regularizationRate=0&noise=0&networkShape=4,2&seed=0.90370&showTestData=false&discretize=false&percTrainData=50&x=true&y=true&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false)

### 参考资料

* [LightGBM官方文档](https://lightgbm.readthedocs.io/en/latest/)