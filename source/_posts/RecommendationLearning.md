---
title: Recommendation方向学习
categories: Paper
---
### 综述
目前推荐上研究的方向有这样几个方向:
1.Temporal Context-Aware Recommendation
2.Spatial Recommendation for Out-of-Town Users
3.Location-based and Real-time Recommendation
4.Efficiency of Online Recommendation

补充学习:
> online learning强调的是学习是实时的，流式的，每次训练不用使用全部样本，而是以之前训练好的模型为基础，每来一个样本就更新一次模型，这种方法叫做OGD（online gradient descent）。

> batch learning或者叫offline learning强调的是每次训练都需要使用全量的样本，因而可能会面临数据量过大的问题。

传统的推荐系统广泛都使用了<font color="blue">协同过滤</font>和<font color="blue">基于内容过滤技术</font>

协同过滤分为
> 基于内存的推荐和基于模型的推荐(矩阵分解)

Context-Aware Recommender Systems(CARS)包含三种范例:contextual pre-filtering,contextual post-filtering and contextual modeling.


