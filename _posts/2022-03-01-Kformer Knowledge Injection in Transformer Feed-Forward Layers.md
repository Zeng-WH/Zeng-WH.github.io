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
<img width="506" alt="b869142b52b346cea1e6219c4b693808" src="https://user-images.githubusercontent.com/47687248/156134354-cc6d0268-a0ac-486c-8971-603f72821236.png">

思路很简单，在得到knowledge representation以后，分别投影到FFN第一个线性层和第二个线性层的维度。相当于扩展了线形层的参数：
<img width="396" alt="f58464c94c4343dc838a1c7b64709304" src="https://user-images.githubusercontent.com/47687248/156134413-694b45cd-4efd-4243-bee3-4d6f4114542b.png">

## 3. 实验

（1）. 实验中的基线包括了直接在输入层直接concat knowledge（**Concat**）以及将knowledge融入self-attention（**Attention**）
这里的Attention方法，就是将knowledge分别映射成key和value，然后再与attention中的key和value concat。这种方法，与prefix在模型中的操作完全一致，也就是说如果将knowledge得到prefix在作用在模型上，和这里的原理几乎一致。但看实验结果：
<img width="692" alt="e255bf135cd04c14894ce3b99d285eea" src="https://user-images.githubusercontent.com/47687248/156134500-3ca1a50f-ddb3-43a3-922d-7ec775cdd459.png">
<img width="456" alt="604ec470aec44bfaa6abee6e197bf2a7" src="https://user-images.githubusercontent.com/47687248/156134547-1e664c34-392e-4fb4-83ca-98f60dc79dc5.png">

Kformer这种方法应该是效果要好的多，但Attention这种方法的优势相比较Concat都不太明显，可能Prefix在融入知识，表示知识方面不太具有优势？

（2）. Kformer其实只作用在预训练模型的最后三层，作者做了对比实验
<img width="439" alt="d314222d2803472db4cd9527d1a5ac50" src="https://user-images.githubusercontent.com/47687248/156134835-626d040a-6f56-413f-84c9-f8855654d733.png">

可以看到除了4-6层加入知识损害了模型表现以外，其他层加知识均能提升模型表现。在高层加入知识的提升最大，也应证了早先研究中，高层layer能捕捉更多的semantic knowledge. 当在所有层都加入知识后，模型的表现得到提升，但是不如最高层的结果，作者认为可能是中间层的不利影响。



