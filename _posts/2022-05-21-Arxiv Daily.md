---
layout:     post
title:      "Arxiv Daily"
subtitle:   日常读论文！（2022y05m21d）
date:       2022-05-21 20:00:00
author:     "Andrew Zeng"
tags:
    - 文本生成
    - 对话生成
    - Prompt Learning
---

##  Long-term Control for Dialogue Generation: Methods and Evaluation

(1). 作者提出了新的问题：constrained long-term dialogue genreation. 所谓long-tem的控制，希望控制词汇能够自然地出现在后续对话中。如下图所示

![aa537f577b0a4b1bb1f7e7e46f4fc6a0](https://user-images.githubusercontent.com/47687248/169651511-f4d126d9-e9df-473b-b729-1ad9e21dded3.png)

(2). 作者提出了衡量long-term控制的指标。既衡量控制词的占比，也衡量控制词是否出现在对话中正确的位置。
(3). 作者提出了解决上述问题的基线模型。


## Prompt Tuning for Discriminative Pre-trained Language Models （ACL 2022 Finding）

![bd11d1b583504723816f8c06e84a1d27](https://user-images.githubusercontent.com/47687248/170032301-811ee7fe-5793-4357-a121-ec4477580aee.png)

当前prompt-tuning的工作主要关注generative PLMs，如BERT，GPT等，却忽视了discriminative PLMs，如ELECTRA. 作者提出了DPT，第一个用于discriminative PLMs的prompt tuning框架。作者在文本分类和QA问题上测试了方法的效果，DPT在全样本和小样本上的表现甚至超越了finetuning的结果。

![e72ecb90942741478febef4ba36947d7](https://user-images.githubusercontent.com/47687248/170032414-f33f09ce-3885-42c6-b7b7-6de604cebe57.png)

### Preliminary
所谓的discriminative PLMs，如上图(a)所示，在预训练的过程中，先使用generator来对文本进行token级别的替换，再使用discriminator来检测被替换的token。训练完成后丢弃generator，而针对下游任务finetune discriminator.

在finetune的过程中，如进行文本分类的任务，会引入新的classification head，重新学习参数，这会与预训练过程产生gap. 如图(b)所示。

### Methodology
作者通过DPT，将文本分类与QA规范为dscriminative任务。如文本分类任务中，对于给定的文本$x$, 将其规范化为模版。

![b0fc3fa81a3f4c7eb276f4bdfb96544c](https://user-images.githubusercontent.com/47687248/170032422-e1210af5-11ee-4cab-aeab-07ce24fbccd7.png)

通过discriminative PLMs决定哪些class候选token是合适的。其中模型判断为original的token即为正确，判断为replaced即为错误。如上图(c)所示。

### Experiment
可以看到，方法虽然简单。但是效果显著
DPT方法相比较finetune在全样本和少样本上都有较大提升。

![32da7dd43f724978ba6cb50f1b6f0b12](https://user-images.githubusercontent.com/47687248/170032472-26242645-a8ca-42b1-b566-fb4b15948320.png)

从下图可以看出，DPT的方法相比较finetune有更好的稳定性。

![a88dd089056746229e3e01c97a723af3](https://user-images.githubusercontent.com/47687248/170032518-03447f1c-3c63-4000-8fd4-7cf3faf2e544.png)
