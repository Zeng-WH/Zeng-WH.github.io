---
layout:     post
title:      "与Prompt-Tuning相关的受控生成"
subtitle:   精读论文！
date:       2022-03-17 15:30:00
author:     "Andrew Zeng"
tags:
    - 文本生成
    - Prompt Learning

---

# Controllable Natural Language Generation with Contrastive Prefixes
如果将prefix tuning用在受控生成，最朴素的想法就是针对不同的控制方向（aspect）训练独立的prefix。而作者通过对比学习任务将这些独立训练的prefix的关系纳入考虑，从而同时训练多个prefix。
比较有启发性的工作：
1. 将同一属性下的不同属性值的prefix训练引入对比学习，从而实现同时训练（比如不同主题的prefix引入对比学习），启发多种属性的组合泛化能否引入对比学习。
2. 对多属性组合进行了初步的探讨，有些实验结果值得我们参考。

## Method
在single-aspect control setting，作者介绍了supervised method以及unsupervised method

### Supervised Method

<img width="405" alt="7ca1454c66ef45a3943e2eb16a8934d2" src="https://user-images.githubusercontent.com/47687248/158756349-6c320133-7ace-422f-944b-cc75558350c7.png">
<img width="517" alt="截屏2022-03-17 下午3 16 11" src="https://user-images.githubusercontent.com/47687248/158756574-ae6ae6f0-feb3-4128-b3fd-be20200a0b0a.png">

### Unspervised Method
unspervised method指的是，对于训练样本，只有input text x而没有attribute label y，换言之input x对应的prefix是discrete latent variable z.  
<img width="518" alt="截屏2022-03-17 下午3 19 36" src="https://user-images.githubusercontent.com/47687248/158757016-f9537886-4720-47e2-bd0b-c743228e40f4.png">

## Experiment
从实验上看，加了contrastive loss的prefix tuning比独立训练的prefix表现好得多。
<img width="890" alt="b23572debea74762be9d3153816a4d8f" src="https://user-images.githubusercontent.com/47687248/158757175-e53ad29a-fb54-4245-8fbe-906b7cf0f0eb.png">
## Muti-Aspect Control
<img width="862" alt="7ae5237e60394241adec18396deb9000" src="https://user-images.githubusercontent.com/47687248/158757284-e1be393d-8f06-4f1f-bd06-256673db1723.png">
该实验表明了即使简单将单aspect的prefix拼接的效果也不错，并且不同aspect的prefix拼接顺序不会影响表现。同样将单aspect的example当作部分标注的multi-aspect的example对prefix进行semi-supervised的方法的表现也很优异。
（可以启发在多属性组合中缺少部分属性标签时，采用semi-supervised learning？）

# Controlling the Focus of Pretrained Language Generation Models （ACL 2022  Findings）
作者认为基于attention机制的模型虽然有用，但attention机制很难显示地控制模型的输出。比如attention如果关注到用户认为并不重要的内容，其实并没有很好的机制去修正。因此作者设计了用户可以直接在input中选择需要focus的内容，而模型生成相关output的机制。
（作者的目标并不是控制模型的attention模块，相反考虑的是实际输出）

## Method

<img width="633" alt="截屏2022-03-17 下午3 25 42" src="https://user-images.githubusercontent.com/47687248/158757970-3ffb4273-4f37-47dd-bc78-5e8b8f933fc6.png">

## Experiments

在具体任务上，将模型在原始的训练集（非focus vectors）上finetune；在attribute methods标注的训练集训练focus vectors，在人工重写的验证集和测试集验证和测试。
主实验结果：
<img width="830" alt="aa9ddee9d643428fa1d6eac6877accf1" src="https://user-images.githubusercontent.com/47687248/158758168-91aece18-c244-4868-badd-5146ced7c283.png">
其中attention-offset的基线指的是直接在highlight sentence的attention weight上加hyper- parameter. 
在attention上的修改的表现不如focus control，在某种程度可能佐证了attention可能不能忠实反应模型的预测结果。
其他各种消融实验比如测不同层的focus vectors的效果并不明显。

# Novelty Controlled Paraphrase Generation with Retrieval Augmented Conditional Prompt Tuning
文章从整体上看是prompt tuning方法在pharahrase generation的应用，相比于原生的prompt tuning的改动不大，更像是prompting tuning的trick	.
论文使用Retrial Example增强的Prompt Tuning来使预训练模型适用于paraphrase generation的任务。论文的控制性主要体现在paraphrase generation在词法上的novelty程度。
## Approaches
<img width="637" alt="截屏2022-03-17 下午3 32 19" src="https://user-images.githubusercontent.com/47687248/158759059-e047df5c-3f00-45a8-a292-a47c5923afa5.png">

## Experiments
很难说prompt tuning在这里起了多重要的作用。这里的检索出example，再构成prompt template，将标签暴露给样本的方法（虽然是别的数据的标签）。在与其他baseline比较时候感觉不太公平。
<img width="958" alt="a15b3df4f79246378d89ef88d994d7fd" src="https://user-images.githubusercontent.com/47687248/158759233-0bd19422-4696-4760-bc6e-a96d7ba7fdf7.png">
在消融实验中，有将KNN检索改为随机选择样本，此时模型的表现与prompt tuning，prefix-tuning差别不大。
