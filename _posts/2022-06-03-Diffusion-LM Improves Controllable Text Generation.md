---

layout:     post
title:      "Diffusion-LM Improves Controllable Text Generation"
subtitle:   读读Stanford的研究！
date:       2022-06-03 18:00:00
author:     "Andrew Zeng"
tags:

  - 文本生成
  - 可控相关
---

看到Prefix-Tuning的作者最近也在研究可控文本生成，主要思路是将CV生成中DDPM迁移到NLP中，并且用类似PPLM的方法实现控制。

这里DDPM可以见
https://zhuanlan.zhihu.com/p/523960047

## 1. 介绍

之前的工作主要关注简单的属性控制，然而在更复杂，更细粒度上的控制（如语法结构）进展甚微。
作者指的更复杂，更细粒度的控制：

![4c365e26bce64ad885d999e16d6de25b](https://user-images.githubusercontent.com/47687248/171829783-85ffe292-3dbc-4d50-be5a-096cee8b4517.png)

为了解决上述困境，作者基于连续的扩散模型提出了非自回归的语言模型（Diffusion-LM）
至于选择扩散模型的原因，根据文章总结为以下几点：

1. 中间变量的连续且分层的特性使得简单的梯度算法就可以实现复杂且可控的生成任务。
2. 由于文本通常是离散的，因此运用离散的扩散模型具有极大的困难，因此作者对标准的扩散模型进行了改造: 在传统的步骤上添加了embedding step和rounding step，设计了学习embedding的训练目标，并且提出了提升rounding表现的方法。
3. 之前的控制生成方法，通常基于自回归语言模型，只能从左至右生成。这导致PPLM无法修复在之前步中的错误。

## 2. Diffusion Models for Continuous Domains

如下图所示：

![c2859cc4729d40efb319ae101bb1c4e5](https://user-images.githubusercontent.com/47687248/171830017-4581ac73-a096-4d5c-a161-e7cf9a60622b.png)

经典的训练目标：

![d2b41e441df942a8b9522dc8b53683c5](https://user-images.githubusercontent.com/47687248/171830047-234aa95d-13f5-431a-ad8b-f0d087fa2987.png)

简化的训练目标：

![443340e26591490882b655b2ddfec896](https://user-images.githubusercontent.com/47687248/171830087-124cf7dc-92f3-4b51-95fb-abd6d41ce231.png)

详细介绍见论文

## 3. Diffusion-LM: Continuous Diffusion Language Modeling

作者对标准的扩散模型进行了部分修改。

### 3.1 End-to-end Training

为了将连续的扩散模型运用到离散的文本，定义embedding函数$EMB(w_{i})$将每一个词语映射为向量。

![c2859cc4729d40efb319ae101bb1c4e5](https://user-images.githubusercontent.com/47687248/171830401-c7c307aa-ee4c-4a46-a804-6a88820919d6.png)

在上图中，在forward process中，添加马尔可夫变换使得将离散的词语$w$映射为$x_{0}$, $q_{\phi}(x_{0}|w)=N(EMB(w),\sigma_{0}I)$; 在reverse process中，添加了可训练的rouding step，$p_{\theta}(w|x_{0})=\Pi_{i=1}^{n}p_{\theta}(w_{i}|x_{i})$, 其中$p_{\theta}(w_{i}|x_{i})$是softmax分布，训练目标如下所示：

![19295bddcc3c4fb7b2e6bcf88a504297](https://user-images.githubusercontent.com/47687248/171830479-f82a4e78-a366-4184-b50d-cb27269b586c.png)

### 3.2  Reducing Rounding Errors

作者在这里设计了方法来减少Rounding Errors，详见论文。

## 4. Decoding and Controllable Generation with Diffusion-LM

### 4.1 Controllable Text Generation

![bee8e0276e7444f8844360104bcd68e5](https://user-images.githubusercontent.com/47687248/171830744-7f92c6bf-c27f-4657-a0ef-a6cb9c37939a.png)

在训练好Diffusion-LM后，作者设计了plug-and-play的机制来控制Diffusion-LM。相比于直接控制的离散的text，作者在由Diffusion-LM生成的连续的隐变量$x_{0:T}$上进行控制。

![3e3fad5b41d3404a8af86780d1dc910d](https://user-images.githubusercontent.com/47687248/171830911-7d7c320c-97c7-42cd-8e6d-542532b7553a.png)

同时为了生成流利的文本，设计了其他的训练目标。

### 4.2 同时为了生成流利的文本，设计了其他的训练目标。

作者在解码的过程中使用了 Minimum Bayes Risk Decoding

## 5. 实验结果

可以看到相比PPLM，FUDGE模型，Diffusion-LM表现优异。详细实验分析见论文。

![d2bb23b04cd44a8eb458c163125dc2fb](https://user-images.githubusercontent.com/47687248/171831225-d6b36950-24c5-47de-a747-ec12fd388ec7.png)

如下表Diffusion-LM在属性组合控制上表现优异

![aac543d7b07049b9b4eceb381cefe576](https://user-images.githubusercontent.com/47687248/171831260-6d5597cf-0c96-4713-b000-0b93f48c0a28.png)

## 6. 总结

论文提出Diffusion-LM，在复杂且细粒度的控制上达到了优异的效果。
但Diffusion-LMs仍然存在缺点：
1. 较高的perplexity
2. 解码的速度相对较慢
3. 训练较难收敛
