---
layout:     post
title:      "Kformer: Knowledge Injection in Transformer Feed-Forward Layers"
subtitle:   精读论文！
date:       2022-03-01 16:00:00
author:     "Andrew Zeng"
tags:
    - Knowledge Injection
    - Prompt Learning

---

论文提出了一种简单且有效的将知识融入预训练语言模型的方法，控制信号的融入可以借鉴。同时，它的实验结果比较有意思，让人思考，直接将knowledge->prefix再融入模型的思路是否有效。

## 1. 问题

之前将知识嵌入语言模型的方法，主要在input/representation level将额外的知识加入。存在的问题：
1. 依赖于在knowledge corpus上pretrain，这往往time-consuming且需要大量的资源。
2. 之前的方法，只是聚焦于加入一种knowledge source比如triplets或者texts，因此需要构造可以处理不同knowledge scources的模型。

## 2. 方法

作者的思路基于之前研究发现的现象：
预训练语言模型中的FFN可以被看成存储factual knowledge的记忆。

KFormer的表示：

思路很简单，在得到knowledge representation以后，分别投影到FFN第一个线性层和第二个线性层的维度。相当于扩展了线形层的参数：

## 3. 实验

1. 实验中的基线包括了直接在输入层直接concat knowledge（**Concat**）以及将knowledge融入self-attention（**Attention**）
这里的Attention方法，就是将knowledge分别映射成key和value，然后再与attention中的key和value concat。这种方法，与prefix在模型中的操作完全一致，也就是说如果将knowledge得到prefix在作用在模型上，和这里的原理几乎一致。但看实验结果：

Kformer这种方法应该是效果要好的多，但Attention这种方法的优势相比较Concat都不太明显，可能Prefix在融入知识，表示知识方面不太具有优势？

2. Kformer其实只作用在预训练模型的最后三层，作者做了对比实验

可以看到除了4-6层加入知识损害了模型表现以外，其他层加知识均能提升模型表现。在高层加入知识的提升最大，也应证了早先研究中，高层layer能捕捉更多的semantic knowledge. 当在所有层都加入知识后，模型的表现得到提升，但是不如最高层的结果，作者认为可能是中间层的不利影响。



