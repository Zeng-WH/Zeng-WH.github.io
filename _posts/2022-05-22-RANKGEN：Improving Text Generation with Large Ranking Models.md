

---

layout:     post
title:      "RANKGEN: Improving Text Generation with Large Ranking Models"
subtitle:   读读Google的研究！
date:       2022-05-22 21:00:00
author:     "Andrew Zeng"
tags:

  - 文本生成
  - 对比学习

---

论文来源：Google Research

论文链接：

https://arxiv.org/abs/2205.09726

代码链接：

https://github.com/martiansideofthemoon/rankgen

知乎链接：

https://zhuanlan.zhihu.com/p/518306383/preview?comment=0&catalog=0

来自谷歌的工作，利用大规模的Ranking模型提升文本生成的表现。

## 1. Introduction

当前的文本生成模型存在“likelihood trap”的问题，具体而言，模型在使用贪心解码或者beam search时会以较大的概率生成枯燥或重复的文本。而其他truncated采样的方法如top-k, nucleus以及typical sampling，虽然可以缓解上述问题，但是会导致inconsistencies, hallucinations, factual errors, 以及commonsense issues等问题。

作者认为上述问题的部分原因可能是“teacher forcing”导致的，在训练的时候会给ground-truth prefix，让模型预测下一个token。但是在测试的时候，prefix会包括模型生成的文本，会导致在解码的过程中错误的传递。这会使模型更关注local context，会导致模型在篇章级文本生成的过程中缺乏相关性和一致性。

针对上述问题，作者提出了RANKGEN模型，能够将prefixs和模型生成的文本映射为共享的向量空间中。能够衡量prefix和model generation之间的兼容性，从而可以灵活地作为socre function用在beam search中。

## 2. RANKGEN：a generation ranker

通过大规模的对比学习来训练RANKGEN，拉近prefix向量与gold completion之间的距离而推远prefix向量与negative的样本的距离。作者提供了两种构造负样本的方法，并实验证明了传统的语言模型无妨将gold continuations与负样本区分开来。

### 2.1 作者构造了INBOOK negatives和GENERATIVE negatives.

#### 1.  INBOOK negatives:

从与prefix相同的文档中的随机位置获得negative samples，这种样本流利通顺且与prefix主题相关。但实际上这些样本与prefix是不相关的，因为它们在文档层面不具有连续性和相关性。

可以看到绝大部分语言模型表现非常差，作者认为出现上述现象的原因可能在于：语言模型关注于local context而不是来自prefix的long-range依赖关系；语言模型偏好于出现在INBOOK中的高频词汇而不是gold continuation.

#### 2. GENERATIVE negatives

由预训练语言模型生成的样本，通常会包括重复以及有害信息。

### 2.2 训练RANKGEN

在训练的过程中，将prefix vector ![[公式]](https://www.zhihu.com/equation?tex=+p_%7Bi%7D)与gold continuation vector ![[公式]](https://www.zhihu.com/equation?tex=c_%7Bi%7D) 的距离拉近，而推远与negative vector的距离。

### 2.3 利用RANKGEN进行推理

作者提供了两种将RANKGEN运用于解码过程的方法。

1. 利用现有的方法生成文本。再利用RANKGEN分数进行排序。

2. 将nucleus或者ancestral中的采样方法中beam search的分数计算由概率替换为RANKGEN分数。如下图所示：

## 3. 实验结果

从上表可以观察到：

1. 解码过程中RANKGEN的参与能有力提升模型表现。

2. INBOOK负样本的效果比GENERATIVE的效果强大，但两者的同时使用能最大化提升模型表现。

## 4. 分析实验

### 4.1 RANKGEN究竟提升了模型哪些方面

绝大部分的提升在于prefix和生成的文本之间的关系是决定因素。（病句qwq）

### 4.2 解码速度

过多的over-generation是本模型的瓶颈

## 5. RANKGEN用于其他任务

除了文本生成任务，RANKGEN可以用于retrieval和suffix identification任务。

如在zero-shot RELic任务上，模型可以达到SOTA

## 6. Conclusion

作者提出了RANKGEN，能够根据给定的prefix，判断其后续文本的匹配分数。可以灵活地运用于文本生成系统。RANKGEN在自动和人工评估上超越了传统的采样方法。