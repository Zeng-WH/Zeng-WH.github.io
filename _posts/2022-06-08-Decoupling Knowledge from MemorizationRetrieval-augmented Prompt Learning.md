---

layout:     post
title:      "Decoupling Knowledge from Memorization:Retrieval-augmented Prompt Learning"
subtitle:   从记忆中解藕出知识！
date:       2022-06-08 00:30:00
author:     "Andrew Zeng"
tags:

  - Prompt Learning
  - Knowledge Injection
---

## 1. Introduction

使用prompt learning的预训练模型在低资源场景或者新的领域通常无法保持稳定的泛化性。作者认为的可能的原因在于传统的prompt learning方法在训练过程中强行记忆非典型的样本或者在小样本场景下过拟合于浅层的模式。近期的研究提出了长尾理论，表明服从长尾分布的数据会有小规模的非典型样本的“sub-populations”。PLMs会在预测时会倾向于强行记忆这些非典型的样例而不是学习到普遍的模式。

为解决上述问题，作者提出了基于prompt learning的retrieval-augmented框架（RETROPROMPT）。希望通过检索的机制来从单纯的记忆中解藕出知识从而提升prompt learning的泛化能力。如下图

<img width="617" alt="2f793ca574a84257b3d65beef2415dfe" src="https://user-images.githubusercontent.com/47687248/172434763-1285db1c-d7b8-4851-be0c-afc60b4fcc2a.png">

**作者设计了记忆分数用来判断出哪些样本是非典型的样本**

## 2. RETROPROMPT: etrieval-augmented Prompt Learning

模型的结构如下图所示

![f5cb500b00384f9f88563207ba8f276c](https://user-images.githubusercontent.com/47687248/172434822-f9a215c7-666e-405b-b5c6-96cdff1e81af.png)

RETROPROMPT包括了三个组件：用于增强输入内容的neural demonstration的检索；KNN引导的训练；基于KNN概率的cloze-style预测。

模型各组件的介绍详见论文，本文不做过多解释。

## 3. 关于Memorization

（其余实验结果详见论文）

既然作者提出希望解决模型过于强行记忆非典型样本，自然需要有衡量memorization的指标。

### 3.1 Memorization Measurement的定义

将memorization measures定义为当训练样本z从训练集移除后模型分类效果的变化。

<img width="934" alt="c1932b00ca054c6f9140ab03e9eff300" src="https://user-images.githubusercontent.com/47687248/172434883-09f68651-ea3a-4d16-829f-fb2ab6678533.png">

### 3.2 Top-memorized Instances：Typical or Atypical

具有较高memorization measures的样本自然是模型着重记忆的样本，作者希望能够判断这些样本就是Typical还是Atypical的。

作者使用SST-2数据集来分析样本是否为atypical数据，主要通过检查postive phrases的占比进行判断。通过统计可以发现typical positive instance通常有着相对较高的positive phrases占比，typical negative instance通常有着较低positive phrases占比。

因此作者选择了训练集中Top-10 %以及Bottom-10 %的memorization score的样本，统计其中positive phrases占比。

<img width="468" alt="7b11841eba2a43e5bb4b69bc9cd42825" src="https://user-images.githubusercontent.com/47687248/172434926-60ab1f2b-a174-4181-87c9-44381fa47067.png">

由上表可以看出得出结论：

1. PLMs倾向于给atypical samples给予更多的记忆。具体而言，无论是LM-BFF还是本文的模型，top-10 % memorized的负样本比整体的负样本的positive phrases比例高。

2. LM-BFF模型在hard samples上的memorization scores要低于fine-tune。作者认为prompt learning可以帮助模型回忆在预训练阶段学到的之后，而不是在下游数据上增强记忆。

3. 本文提出的方法具有更低的memorization scores。说明本文从记忆中解藕出知识能够减轻PLMs盲目地记忆问题。

## 4. 思考

本文提出的问题值得关注，关于如何缓建预训练语言模型对非典型样本的盲目记忆。同时衡量记忆程度的指标也值得借鉴。
