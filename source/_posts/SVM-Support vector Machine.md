---
title: 支持向量机(Support Vector Machine)学习
categories: Seminar
---
# 支持向量机(SVM-Support Vector Machine)：
## 定义
1.SVM是一种分类算法，是一种*二类分类模型*,用于解决分类和回归问题。通过寻求结构化风险最小来提高学习机泛化能力，实现经验风险和置信范围最小化，从而达到在统计样本量较少的情况下，亦能获得良好统计规律的目的。
> *i.e.*<font color=red>给定一个包含正例和反例的样本集合，svm的目的是寻找一个超平面来对样本进行分割，把样本中的正例面和反例面分开，但不是简单的分开，原则是使正例和反例之间的间隔最大，鲁棒性最好。</font>

2.*基本公式*：在样本空间中，划分超平面的线性方程：![线性方程](https://github.com/DuncanZhou/images/raw/master/1.PNG)
样本空间中任意点x到超平面（w，b）距离为![距离](https://github.com/DuncanZhou/images/raw/master/2.PNG)
假设正确分类，![](https://github.com/DuncanZhou/images/raw/master/3.PNG)“间隔”为![](https://github.com/DuncanZhou/images/raw/master/4.PNG)
所以，现在的目标是求得*“最大间隔”*![](https://github.com/DuncanZhou/images/raw/master/5.PNG)
这就是SVM的基本型。

3.求“最大间隔”过程中的问题转化（转换成对偶问题）
> 最大 -> 最小 -> 凸二次规划|<font color=red>拉格朗日乘子法</font>

## 线性划分 -> 非线性划分
### 1.问题
![](https://github.com/DuncanZhou/images/raw/master/6.PNG)
之前的讨论是假设样本是线性可分的，然而现实生活任务中，原始样本空间也许并不存在一个能正确划分两类样本的超平面。(如“异或问题”)，<font color=red>对于这样的问题，可以将原始样本空间映射到一个更高维的特征空间。</font>（Fortunately,如果原始空间是有限集，那么一定存在一个高维特征空间是样本可分。）
### 2.解决方案
映射后求解“最大间隔”的解
![](https://github.com/DuncanZhou/images/raw/master/7.PNG)
### 3.涉及到的问题
在求解过程中涉及计算样本Xi与Xi映射到特征空间之后的内积。由于特征空间维数可能很高，甚至可能是无穷维，因此直接计算内积通常是困难的，为了避开这个问题，设想这样一个函数-核函数![](https://github.com/DuncanZhou/images/raw/master/8.PNG)
求解后得到![](https://github.com/DuncanZhou/images/raw/master/9.PNG)
### 4.常用的核函数
![](https://github.com/DuncanZhou/images/raw/master/10.PNG)

## 软间隔与正则化
> *软间隔*：现实任务中往往很难确定合适的核函数使训练集在特征空间中线性可分，即使恰好找到了某个核函数使训练集在特征空间中线性可分，也很难判定这个貌似线性可分的结果不是由于过拟合所造成的。

*解决该问题的一个方法是允许svm在一些样本上出错*。如![](https://github.com/DuncanZhou/images/raw/master/11.PNG)

也就是在求解最大化间隔时，同时使不满足约束的样本尽可能少。![](https://github.com/DuncanZhou/images/raw/master/15.PNG)

三种常用的替代损失函数：![](https://github.com/DuncanZhou/images/raw/master/12.PNG)

共性：![](https://github.com/DuncanZhou/images/raw/master/13.PNG)

## 支持向量回归（Support Vector Regression）
给定样本D={(x1,y1),(x2,y2),...},希望学得一个回归模型，使得f(x)与y尽可能接近，w和b是待确定参数。
传统回归模型通常直接基于模型输出f(x)与真实输出y之间的差别来计算损失，当且仅当f(x)与y完全相同时，损失才为0.与次不同，SVR假设我们能容忍f(x)与y之间最多有e的偏差，小于等于e的都算0误差。SVR问题形式化为![](https://github.com/DuncanZhou/images/raw/master/14.PNG)
