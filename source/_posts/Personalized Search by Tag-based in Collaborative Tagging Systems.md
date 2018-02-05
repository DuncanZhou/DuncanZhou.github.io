---
title: Personalized Search论文阅读笔记
categories: Paper
---
## 论文题目
### Personalized Search by Tag-based User Profiling and Resource Profile in Collaborative Tagging Systems

## Concept
> **Collaboative Tagging Systems**:协标签系统，类似于YouTube、Flickr这样一类系统，允许用户使用标签对资源进行标注，而得出用户的标签；所以，用户具有标签，资源也有标签，这样称为协同标签。

> **folksonomy**:大众分类的。如上所说的一些系统中允许用户去标注资源，即，利用大众的力量去分类资源。

> **personalized search**:个性化搜索，在返回和查询相关的结果时不仅考虑和搜索的关键词相关，并且考虑用户的profile(偏好)。

## About this paper
论文中首先总结了之前的工作中对用户和资源的profile构建的方法，用户和资源的profile的tag的权重计算方法有TF、TF-IDF、BM25，以及用户兴趣和资源相似性的计算方法，但这些方法都存在一定的局限性。
**TF方法**：对于标注比较频繁或者比较活跃的用户，经常使用某些tag标注。如果使用TF计算tag的权重，那么，对于不经常标注资源的用户，其偏好的标签权重必定比活跃的用户tag小很多。
<font color = "green">举个例子:</font>
> Tom = {(tag1:300;tag2:200;tag3:280)}; Jerry = {(tag1:50;tag2:30;tag3:10)} 对于，Tom和Jerry来说，tag1都是其偏好的，但是如果以TF来计算，tag1对于Tom的偏好程度是大于Jerry的，而其实，tag1对Jerry是更为感兴趣的。同理，对于资源标签权重计算方法也存在相似问题。

**TF-IDF方法**：这里为TF-IDF的演变版，TF-IUF和TF-IRF。TF-IDF用来表示一个tag能否表示该实体的程度，同理，TF-IUF和TF-IRF表示一个标签能否表示该用户和资源。但是，这里存在的问题是，标签的权值并不能表示用户对于该标签的偏好程度。
<font color = "green">举个例子:</font>
> Tom的标签使用频率如下:[tag1:500;tag2:400;...;tagn:1]。如果使用TF-IDF方法来计算标签权重，那么，tagn的TF-IDF权重是最大的，但是，tagn并不是用户最感兴趣的。同理，对于资源标签权重计算方法也存在相似问题。

**BM25**:BM25方法是以TF和TF-IDF的值作为变量，所以BM25方法也存在以上的局限性。

**用户兴趣与资源相似性计算方法**:一般计算该相似度采用的方法是余弦相似性等相似性计算法方法，这存在一些问题。因为现在用户的标签的权值代表的是用户对该tag的感兴趣程度，而不是与目标资源的相关程度，所以用一般相似性计算方法是不合理的。
<font color = "green">举个例子:</font>
> Resource1 = {(tag1:0.9;tag2:0.95;tag3:0.85)}; Resource2 = {(tag1:0.9;tag2:0.1;tag3:0.01)}; User = {(tag1:0.8;tag2:0.5;...;tag3:0.25)}。如果用一般方法计算相似度，那么R1相似性小于R2，而与用户偏好相似的更多是R1。是因为用户对tag1，tag2，tag3都是偏好的，而R2只是对tag1更符合一些。

所以，基于以上方法的不合理之处，该论文提出了计算user和resource标签权值及计算用户兴趣和资源相关性的方法。
## 论文提出的方法
### User Profiles Modeling
> 某用户中标签的权值 = 该标签被用来标注的次数 / 该用户标签总标注次数

### Resources Profiles Modeling
> 某资源中标签的权值 = 使用该标签标注该资源的用户数 / 标注该资源的总用户数

### Personalized Search
分为查询相似性计算和用户兴趣与查询相似性计算。相关公式见论文。
最后，将两个相似性分数融合得到相似性分数。

## 实验结果准确性度量标准
数据集：MovieLens数据集和该论文自己的一个demo系统。
<font color = "red">1.标准1</font>:imp = 1 / rp - 1 / rb;rp为新方法对目标资源的等级，rb为baseline方法对目标资源的等级。
<font color = "red">2.标准2</font>:HR：命中率计算方法
<font color = "red">3.标准3</font>:MRR：平均倒数评级 
