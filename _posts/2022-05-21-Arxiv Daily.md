---
layout:     post
title:      "Arxiv Daily"
subtitle:   日常读论文！（2022y05m21d-2022y06m13d）
date:       2022-06-13 20:00:00
author:     "Andrew Zeng"
tags:
    - 文本生成
    - 对话生成
    - Prompt Learning
    - Meta Learning
---

##  Paper1: Long-term Control for Dialogue Generation: Methods and Evaluation

(1). 作者提出了新的问题：constrained long-term dialogue genreation. 所谓long-tem的控制，希望控制词汇能够自然地出现在后续对话中。如下图所示

![aa537f577b0a4b1bb1f7e7e46f4fc6a0](https://user-images.githubusercontent.com/47687248/169651511-f4d126d9-e9df-473b-b729-1ad9e21dded3.png)

(2). 作者提出了衡量long-term控制的指标。既衡量控制词的占比，也衡量控制词是否出现在对话中正确的位置。
(3). 作者提出了解决上述问题的基线模型。


## Paper2: Prompt Tuning for Discriminative Pre-trained Language Models （ACL 2022 Finding）

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

## Paper3: Learning a Better Initialization for Soft Prompts via Meta-Learning


### 1. Introduction

传统的Prompt-Tuning方法在没有比较好的初始化时很难在小样本设置上表现良好。本文在PPT的基础上利用Meta Learning通过考虑预训练数据的latent structure来提升PPT的初始化。

PPT使用自监督预训练任务预训练soft prompts，将这些预训练好的prompts运用到小样本的下游任务上。作者认为，PPT的限制是将所有的预训练数据混合在一起，而将所有的样本平等对待。因此，PPT保持了仅与预训练任务相关的冗余信息，会影响模型在下游任务的表现。

作者提出的MetaPT，希望能够将预训练数据和下游任务共享的特征编码进prompt的初始化中，从而使得模型可以更快迁移到下游任务中。

### 2. Meta-learned Prompt Tuning

![a1e4d8e84b6e4a1586a4a310400d4c27](https://user-images.githubusercontent.com/47687248/170505082-30de48cf-add7-4313-9c65-0efac818f7f3.png)

#### 2.1 Pretraining Tasks Data

构造完成预训练数据后，通过聚类算将样本分为K类

![a539ce284d9c495bbaa01847a11adc94](https://user-images.githubusercontent.com/47687248/170505208-6694e4ef-3bab-4e56-a217-2222e678ba6d.png)

#### 2.2 Prompt-MAML Algorithm

使用MAML学习meta任务中的通用特征。随机初始化soft prompts的参数$P$, 对于meta task $T_{i}$, 采样m个样本，计算平均损失$L_{T_{i}}(f_{P})$,暂时梯度更新soft prompts：

![b3a7358ea21b45959df3b330391f6005](https://user-images.githubusercontent.com/47687248/170505366-1a101d7b-b367-479b-a2ec-7b0e0403ce8f.png)

更新完prompts后，再采样m个样本，计算损失$L_{T_{i}}(f_{P_{i}^{'}})$, 将上述过程运用于其他meta tasks，更新prompts：

![76171c81922a4189ab6d24df20bb38f1](https://user-images.githubusercontent.com/47687248/170505424-4c1fcdcf-2660-4c11-b530-4cacc04c1ce5.png)

上式为prompts一次完整更新过程。

![7704fd897f054f9ca211e67fb4acba09](https://user-images.githubusercontent.com/47687248/170505485-ff884387-7ce9-4514-9eaa-c2619de7e7a0.png)

### 3. Experiments

可以看到MetaPT在7个任务上相比较PPT都有了较大的提升。

![f6074c3d4a09441a823878093488b805](https://user-images.githubusercontent.com/47687248/170505608-537ac889-d172-4a01-8e18-e46e98d266b5.png)

同时在小样本上，MetaPT也比之前的方法稳定

![d0e39eaf7b824f33a19de355f9d072f9](https://user-images.githubusercontent.com/47687248/170505661-1570ff80-96d1-49f6-8ff1-41a10f6e83a3.png)

## Paper4: CoNT：Contrastive Neural Text Generation

### 1. Intro

作者对于将对比学习运用于文本生成方法存在的瓶颈把握的很准，与我在实验中观察到的现象一致：

1. 对比样本的构造

通过在离散空间或者连续空间中给ground truth增加扰动来构造对比样本的方法直接用在文本生成中表现不佳。

2. 对比损失的选择

按照simCLR的设置简单实用from-batch 正负样本，并且采用InfoNCE损失而忽视负样本之间的区别，这种方法收效甚微。

3. 解码策略的选择

作者针对上述瓶颈进行了优化，提出了CoNT.

### 2. Method

CoNT在三个方法领先于Naive CL：

1. CoNT使用自身的预测结果来作为负样本从而构造对比学习样本集合。

2. CoNT使用N-pairs对比损失替换InfoNCE损失。N-pairs对比损失能够对所有的样本对有在句子级别更细粒度的监督学习。

3. CoNT将学习得到的相似度函数加入到推理过程。

![810f933ea7914f8383a10d0d4b4bb7a6](https://user-images.githubusercontent.com/47687248/173346766-be896ef9-9035-4050-80a7-542c0efd6e80.png)


### 3. Experiments

从实验结果，本文提出的框架相比于传统的对比学习方法有了较大的方法。

![c4f303d34ea64a8daff91118cbca5a9b](https://user-images.githubusercontent.com/47687248/173346820-d29649bc-3999-478c-a12a-99585cf94bc4.png)

作者同时展示了对Hard Negative Samples的区分。

可以看到navie CL能够将batch tokens以及其他tokens清晰地区分开，但是没有办法将低质量的预测结果与ground truth区分开，这与其对比学习训练目标有关。而CoNT能够将低质量的生成结果与ground truth推开，说明CoNT能够保证获得sequence generation的更好的表示。

![9d416c34a9e44bc7bac5148c266baea5](https://user-images.githubusercontent.com/47687248/173346886-f430c0cd-4914-4a00-8b17-3cf72a861535.png)


