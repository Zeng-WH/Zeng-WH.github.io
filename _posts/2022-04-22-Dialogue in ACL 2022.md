---
layout:     post
title:      "Dialogue in ACL 2022"
subtitle:   ACL冲冲冲！
date:       2022-04-22 21:00:00
author:     "Andrew Zeng"
tags:
      - 对话生成

---

将陆续阅读ACL 2022中与对话相关的论文。

# A Model-agnostic Data Manipulation Method for Persona-based Dialogue Generation

有限的persona-based dialogue数据很难训练好对话生成模型。从数据的角度看，该任务的困难在于：

 (1) 很难大规模扩展当前的persona-based对话数据集。
 
 (2) 相比于传统的对话数据，当前任务的对话样本很难被学习。具体而言，因为对话历史和persona的共同参与。同时PersonaChat数据集中不是所有的repsponse都能与提供的persona保持一致。
 
![f35550b97381447bb981827b186b8ee1](https://user-images.githubusercontent.com/47687248/164726499-b2a262ca-b72f-4419-99de-60c5a75d49dc.png)

作者其实提出了一种增强persona-based dialogue data的方法，可以用于不同的模型。称为$D^{3}$, 包括三个步骤：

![066c9505c31e466786693d695635d27b](https://user-images.githubusercontent.com/47687248/164726565-de86a56b-e8d4-45c3-86b6-5d782bd6058d.png)

（1）Data distillation

传统的数据集被简化为只包括useful和less redundant的persona sentences和dialogue utterances，被简化后的数据集能够更容易地被fit.

（2）Data diversification

在有了更简单的distilled样本后，在该数据集上进行数据增强。如edit new personas, and110then align them with new and consistent responses to improve data diversity

（3）Data curriculum

在拥有augmented distilled data以及original data的情况下，使用curriculumn进行训练。具体而言，让base模型先在easier augmented distilled data 上训练，再在harder original data上训练。

