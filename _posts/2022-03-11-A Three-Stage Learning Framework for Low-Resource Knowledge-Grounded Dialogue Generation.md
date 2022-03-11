---
layout:     post
title:      "A Three-Stage Learning Framework for Low-Resource Knowledge-Grounded Dialogue Generation"
subtitle:   精读论文！
date:       2022-03-11 10:50:00
author:     "Andrew Zeng"
tags:
    - Knowledge-Grounded Dialogue
    - 对话生成

---

论文提出了一种针对低资源场景下的knowledge-grounded dialogue generation任务的多阶段的学习框架。其中，对使用ungrounded dialogues以及unstructured knowledge参与预训练以及将它们结合的方法值得关注。

## 一. 问题

构造大规模的knowledge- grounded的对话数据集是非常困难的，而在小规模的训练样本上训练的模型在面对新的领域时很难表现好，因此搭建低资源场景的knowledge-grounded dialogue系统是非常重要的。

## 二. 方法

（1）作者基于weakly supervised learning提出了三阶段的学习框架，利用ungrounded dialogues和unstructured knowledge来进行weakly supervised learning.
<img width="856" alt="d6e5d0f90f4b4e4aaa4525e36772f2ee" src="https://user-images.githubusercontent.com/47687248/157893215-024b52d9-966d-47b3-8f50-63aead433079.png">

三阶段包括：对knowledge部分进行pretrain；对unground dialogues部分进行pretrain；利用ungrounded dialogues和unstructured knowledge构造伪样本训练；利用knowledge-grounded dialogue训练。

（2）对Transformer进行改造，让其decoder能够与dialogue history和knowledge进行cross attention.
<img width="540" alt="65fa87230d554c7eb40bd99e9214baa0" src="https://user-images.githubusercontent.com/47687248/157893265-c77ad7ba-79dc-48af-a3b3-0699e68f863c.png">

（3）Wizard-of-Wikipedia 以及  CMU_DoG.
每个wizard turn与从Wikipedia检索得到的60句话相关作为知识，通常是noise的。两个agent会围绕主题谈论。
CMU_DoG包含两个worders之间的conversation，了解background documents，但主要关注于film reviews

## 三. 实验结果

实验结果不错，其中消融实验比较值得研究：
![44897dfbe08d4ea88807abde8fdf2b26](https://user-images.githubusercontent.com/47687248/157893300-8a9ec038-28da-4181-9313-972a40616b3a.png)

可以清楚分析出有用的模块。



