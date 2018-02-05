---
title: 准确率和召回率及如何提高准确率
categories: Learning
---
<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>
### 准确率和召回率的计算
> 准确率是预测正确数量 / 总数量

> 精确率(precision)是针对<font color='red'>预测结果</font>而言,它表示的是预测为正的样本中有多少是真正的正样本.预测为正有两种可能,一种就是把正类预测为正类(TP),另一种就是把负类预测为正类(FP),P = TP / (TP + FP)

> 召回率(recall)是针对<font color='red'>原来的样本</font>而言的，它表示的是样本中的正例有多少被预测正确了。那也有两种可能，一种是把原来的正类预测成正类(TP)，另一种就是把原来的正类预测为负类(FN)。R = TP / (TP + FN)

精确率 = 提取出的正确信息条数 / 提取出的信息条数
召回率 = 提取出的正确信息条数 / 样本中的信息条数

> <font color='green'>举这样一个例子：某池塘有1400条鲤鱼，300只虾，300只鳖。现在以捕鲤鱼为目的。撒一大网，逮着了700条鲤鱼，200只虾，100只鳖。那么，这些指标分别如下：
正确率 = 700 / (700 + 200 + 100) = 70%
召回率 = 700 / 1400 = 50%
F值 = 70% \* 50% \* 2 / (70% + 50%) = 58.3%</font>

F值 = 精确率 \* 召回率 \* 2 / (精确率 + 召回率)

对于多分类或者n个二分类混淆矩阵上综合考察查准率(precision)和查全率(recall)
1.一种直接的做法是现在各混淆矩阵上分别计算出查准率和查全率,记为(P1,R1),...,(Pn,Rn),再计算平均值,这样就得到"宏查全率"(macro-P),"宏查全率"(macro-R),以及相应的"宏F1"(macro-F1):
\\(macro-P = \frac{1}{n}\sum_{i=1}^{n}Pi\\)

\\(macro-R=\frac{1}{n}\sum_{i=1}^{n}Ri\\)

\\(macro-F1=\frac{2\*macro-P\*macro-R}{macro-P+macro-R}\\)

2.还可先将各混淆矩阵对应元素进行平均,得到TP/FP/TN/FN的平均值,分别记为ATP,AFP,ATN,AFN,再基于这些平均值计算出"微查准率(micro-P)"/"微查全率"(micro-R)和"微F1"(micro-F1):
\\(micro-P = \frac{ATP}{ATP + AFP}\\)

\\(micro-R=\frac{ATP}{ATP + AFN}\\)

\\(micro-F1=\frac{2\*micro-P\*micro-R}{micro-P+micro-R}\\)

### 如何提高准确率
提高准确率的手段可以分为三种:1)Bagging 2)Boosting 3)随即森林

> 在一般经验中,如果把好坏不等的东西掺到一起,那么通常结果会是比最坏的要好一些,比最好的要坏一些.集成学习把多个学习器结合起来,如何能够获得比最好的单一学习器更好的性能呢?

要获得好的集成,个体学习器应"好而不同",即个体学习器要有一定的"准确性",即学习器不能太坏,并且要有"多样性",即学习器间具有差异.
目前的集成学习方法大致分为两大类:个体学习器间存在强依赖关系,必须串行生成的序列化方法,以及个体学习器间不存在强依赖关系,可同时生成的并行化方法;前者的代表是Boosting,后者的代表是Bagging和"随机森林".
Boosting最著名的代表是AdaBoost.


