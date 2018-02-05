---
title: 后缀树
tags: algorithm
categories: Learning
---
###后缀树学习
> 概念: 后缀树是一种PAT树(检索树),它描述了给定字符串的所要后缀,许多重要的字符串操作都能够在后缀树上快速地实现.

定义: 一个长度为n的字符串s,它的后缀树定义为一棵满足如下条件的树:
* 1.从根到树叶的路径与s的后缀一一对应.即每条路径唯一代表了s的一个后缀；
* 每条边都代表一个非空的字符串；
* 所有内部节点(除根节点)都至少两个子节点.由于并非所有的字符串都存在这样的树,因此s通常使用一个终止符进行填充(通常使用$).

### 计算最长回文字串
Manacher算法: 用一个辅助数组Len,Len[i表示以字符T[i]为中心的最长回文串最友字符到T[i]的长度.
```
//Manacher算法计算过程  
int MANACHER(String str)  
{  
     int mx=0,ans=0,po=0;//mx即为当前计算回文串最右边字符的最大值  
     for(int i=1;i<=str.len;i++)  
     {  
         if(mx>i)  
         Len[i]=min(mx-i,Len[2*po-i]);//在Len[j]和mx-i中取个小  
         else  
         Len[i]=1;//如果i>=mx，要从头开始匹配  
         while(st[i-Len[i]]==st[i+Len[i]])  
         Len[i]++;  
         if(Len[i]+i>mx)//若新计算的回文串右端点位置大于mx，要更新po和mx的值  
         {  
             mx=Len[i]+i;  
             po=i;  
         }  
         ans=max(ans,Len[i]);  
     }  
     return ans-1;//返回Len[i]中的最大值-1即为原串的最长回文子串额长度   
  }  
```
