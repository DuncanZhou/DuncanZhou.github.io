---
title: Personalized Search论文阅读笔记-08年SIGIR
categories: Paper
---
## 论文题目
### Exploring Folksonomy for Personalized Search

## 概念解释
**Folksonomy:**
> 该单词由folk和taxonomy组成，folk是口语中伙伴的意思，taxonomy是分类方法的意思，该词用来表示现在存在的大众分类的一种现象。目前许多应用允许用户上传资源时选择标签标注该资源。现有的应用如flickr和dogear。

## Motivation
对于这样允许大众分类的应用，如何满足用户在搜索时尽可能准确地返回用户所需要的资源是一个有意思的问题。因为如果像传统的搜索方法仅通过查询关键词去匹配搜索结果，返回的结果可能会不满足用户的初衷。而且，不同的用户在搜索不同的资源时有可能会使用同样的关键词，比如，爱好运动和爱好喝咖啡的用户在搜索杯子的时候使用的关键词都可能是“杯子”，而返回的结果对于爱好运动的用户来说应该尽可能是运动型杯子，对于爱好喝咖啡的用户来说应该尽可能是咖啡杯子。所以，这里的问题都归结于Personalized Search。

## Solution
根据之前的工作，解决这样问题的方法有两类:<font color="red">Query Refinement</font>和<font color="red">Result Processing</font> 
> 1)对于查询术语进行替换和扩展，替换成其他的术语或者用其他的术语来填充
2)对查询返回的结果再排序或者结果聚类等方法
使用较为普遍的是Result Processing，该篇论文中使用的也是第二种方法。

本文中提出的方法分为三个步骤：
1)Query中的terms先求结果排序得到Ranklist
2)通过User的Interest Vector和资源的Topic Vector求相似性得到Ranklist
3)聚合两个list得到最终的Ranklist
(具体在实验过程中，为了提高效率，第二步求Ranklist只对第一步中的前N个再去求排序)

## Details-Of-Methods
<font color= "red">**Step1：**</font>
对于Query中的terms去求资源排序，本文中采用**BM25**和**Language Model for IR(LMIR)**这两种文本检索模型。
先通过基本的文本检索去得到第一步的Ranklist。
<font color= "red">**Step2：**</font>
第二步是本文的核心。关键在于建立Topic Space,因为建立了Topic Space后，才能对用户建立兴趣向量，才能对资源建立主题向量，然后再去计算两者之间的相似性。

1.本文中的主题空间使用了Folksonomy的方法，以标注的tag作为向量的每一维，每个维的值的计算方法可以通过tfidf或者BM25来计算，从而构成用户和资源的兴趣和topic向量。
此外，本文中以ODP分好类的web pages的topic space作为baseline方法。

2.当分别构建好用户和资源的向量后，本文通过类PageRank算法来迭代形成最终的用户和资源的矩阵。R矩阵行代表用户，列代表兴趣。T矩阵行代表资源，列代表topic。
<font color= "red">**Step3：**</font>
本文中聚合两个list的方法是通过简单的WBF方法去得到最终的Ranklist

## Evaluation-Methods
本文基于一个假设：用户收藏过或标注过的资源即为用户相关资源，以此来通过检索评价标准来评价Personalized Search。

