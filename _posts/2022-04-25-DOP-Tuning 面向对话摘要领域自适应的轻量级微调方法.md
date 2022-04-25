---
layout:     post
title:      "NAACL 2022 | DOP-Tuning: 面向对话摘要领域自适应的轻量级微调方法"
subtitle:   ACL冲冲冲！
date:       2022-04-25 17:30:00
author:     "Andrew Zeng"
tags:
      - Prompt Learning
      - 文本生成


---

本文介绍一下我们组在面向领域迁移的对话摘要任务上的工作。

![图片](https://mmbiz.qpic.cn/mmbiz_png/Psho9dm7oDGVibSM8H7iaqjkXI5FUsyfRdAEx3yR9uhfhWvBa8gSAEBj89O1DW3lyy35AoJcXX12pTL98YZjUSFA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**论文标题：**

Domain-Oriented Prefix-Tuning: Towards Efficient and Generalizable Fine-tuning for Zero-Shot Dialogue Summarization

**论文来源：**

NAACL 2022

**论文链接：**

https://arxiv.org/abs/2204.04362

**代码链接：**

https://github.com/Zeng-WH/DOP-Tuning

## 1. Motivations

1. 当前的对话摘要模型往往缺乏在新领域上的泛化性，因为大规模的生成式预训练模型往往需要大量的人工标注的黄金摘要，在 few/no labeled 的场景下无法扩展到新的领域。
2. 当前研究摘要领域迁移的方法需要耗时的预训练和大规模额外的语料库。他们仅关注沉重的预训练步骤而不是轻量化的微调过程。

## 2. Contributions

1.  我们第一个探索面向领域迁移的对话摘要任务的fine-tuning方法，并且在TODSum(TODSum是我们提出的对话摘要数据集[TODSum](https://arxiv.org/abs/2110.12680))和QMSum两个数据集上建立了实用且全面的benchmarks.
2.  提出了轻量且有效的面向领域的PrezZfix-tuning的模型，该模型使用领域词初始化的prefix模块以及离散的prompt来从大规模预训练模型中交互式地提取知识。
3.  进行了充分的实验和定量分析来证明了我们提出的方法的有效性，并且讨论了面向领域迁移的对话摘要所存在的挑战。

## 3. Methodology

模型结构包括Domain-Oriented Prefix，Prompt Encoder以及Decoder三个部分。

![89601b192ff24bbcb37aaf8f1b2338c6](https://user-images.githubusercontent.com/47687248/165057557-fda144c5-1bd3-4929-81c7-5671ab32ae79.png)


### 3.1 Domain-Oriented Prefix

为了缓解领域耦合的问题，我们提出了domain-oriented的prefix模块来从源域和目标域中获取共享的知识。
采用two-step构建Domain-Oriented Prefix，包括初始化和参数化。

![image](https://user-images.githubusercontent.com/47687248/165057725-487201b1-46da-45f1-88a0-b529f04d6bb5.png)


#### 3.1.1 Initialization

1.  利用LDA主题模型从对话文本中提取每个领域的关键词，并且将他们拼接起来构成domain word（prefix）序列$x_{dw}$
2.  随机初始化domain word序列组成可学习的矩阵$M_{\theta}\in R^{|x_{dw}|d_{m}}$

#### 3.1.2 Parametrization

1.  利用MLP和预训练的BART模型分别得到domain word序列的表示，重新训练MLP使用MSE loss使得MLP的输出与预训练的BART的decoder hidden states相同，以此从预训练模型中解藕出领域知识。
2.  在更新MLP参数的过程中保持预训练的BART的参数固定。
3.  得到MLP的参数初始化，并使用预训练好的MLP来映射prefix表示的初始化embeddings.

### 3.2 Prompt Encoder

#### 3.2.1 Discrete Prompts

1.  将TODSum数据集中的对话状态和QMSum中的queries作为离散的Prompts
2.  对于对话状态这种结构化的信息，将结构化的信息转化为文本序列。

#### 3.2.2 Transformer Layer

1.  将离散的prompt序列$x_{dp}$以及对话文本序列$x_{d}$作为encoder的输入序列。
2.  通过修改添加domain-oriented prefix序列的键值对来修改self- attention机制。

### 3.3 Decoder

将Prefix模块也加到decoder上，以类似的方式修改cross-attention和self- attention机制。

### 3.4 Training Strategy

采用如下的训练目标更新梯度：

<img width="805" alt="e57e9ff9765e44f4b4e0a87ac6af44b3" src="https://user-images.githubusercontent.com/47687248/165057976-c37eca89-bb9f-4c11-8b84-3fb21ba68e84.png">

在训练过程中中固定BART的参数，而更新prefix的参数。在训练时使用来自源域的领域词作为prefix序列。当训练完成以后，保存domain-oriented的prefix模块的参数，而丢弃掉预训练好的BART模块的参数。
在测试的过程中，目标域的领域词则被MLP模块映射为prefix表示。

## 4. Experimental Setup

### 4.1  Datasets

在两个multi-domain对话摘要数据集上评估了模型的效果。

![image](https://user-images.githubusercontent.com/47687248/165058127-321b66bc-711d-4f98-ac63-a45438f05462.png)


#### 4.1.1 TODSum

TODSum是基于经典对话数据集MultiWOZ提出的task-oriented对话摘要数据集。根据领域信息，数据集可以被划为5个领域：restaurant，hotel，attraction，taxi以及train. 在实验时，选择5个域中的4个域作为源域，剩下的域作为目标域，从源域中抽取200个样本作为验证集，源域的剩余数据作为训练集，目标域的数据作为测试集。

#### 4.1.2 QMSum

QMSum数据集包括上千条会议录音数据，包括三个领域：academic，committee以及product. 采用类似于TODSum数据集的处理方式。

### 4.2 Main Results

#### 4.2.1 Results on TODSum

![4dbdb43c2e3142fd92523ca9aadec458](https://user-images.githubusercontent.com/47687248/165058282-6f033a85-410c-410b-b48d-214a72a22313.png)

可以看到，Prefix-Tuning相比较BART，BART w. DS.,表现要差，是因为对话文本很长且复杂，仅使用fine-tuning参数的20%很难理解领域知识，以及识别对话中的关键内容。在与Prefix-tuning具有相同量级的参数下，DOP- tuning在5个领域都有了较大的提升。这表明由领域词初始化的prefix模块以及有对话状态组成的离散的prompts发挥了重要的作用。除此之外，模型比全参数fine-tuning的模型BART要好，说明模型可以有效地从源域和目标域中解藕出知识。上述结果表示，在有限的数据情况下，模型仍然可以达到SOTA的结果。

#### 4.2.2 Results on QMSum

![b22e45cf03554374808330cbc773fe30](https://user-images.githubusercontent.com/47687248/165058369-d1f8c780-8583-4396-b467-64d05c6b731d.png)


整体表现的趋势与在TODSum数据集上的表现一致，但是可以看出在Rouges的分数相对而言较低，是因为领域并没有明显的领域词，导致了严重的领域耦合的问题。除此之外，由于会议文本过长，很难从对话中捕捉核心内容。总的来说，这些结果表明多领域设置对于会议摘要是非常必要且有意义的。

## 5. Qualitative Analysis

### 5.1 Number of Domain Words

探究领域词数量的影响，可以看到领域词数量在140时使rouge达到了峰值，当低于140时，效果降低，说明参数量不足影响了模型的表现。当领域词数量超过阈值时，模型的表现下降，说明了太长的prefix序列给BART增加了负担，并且引入了额外的噪声。但是，领域词数量的变化并没有对模型的表现的有太大的影响（只有2～3%的起伏），说明了domain-oriented prefix模块和有效性和模型的鲁棒性。

![f31ac144b97549aa90d84813653b4ac8](https://user-images.githubusercontent.com/47687248/165058462-5b0df7ca-b9cd-4e50-a36d-15d195fbc629.png)

### 5.2 Quality of Domain Words

探究领域词的质量的影响，将领域词的中的一定比例的词语以与领域无关的词汇替代。可以看到，随着更多的噪声被引入，模型受到更大的影响且表现下降。当噪声的比例超过100%时，模型的表现甚至比Prefix- Tuning糟糕。这是因为，我们使用完全无关的词汇去初始化Prefix模块，相比较随机初始化引入了更多的噪声影响了DOP的表现。从这一点看，引入高质量的领域词有利于领域解藕，高质量的领域词对摘要生成是重要的。

![3303ad68395040e68b08203f4ac28d90](https://user-images.githubusercontent.com/47687248/165058527-b933ad77-3137-4b9d-ba45-ed3ec737cd1d.png)

### 5.3 Ablation Study

研究了domain-oriented initialization和discrete prompts的影响。同时去掉两个模块与原始prefix-tuning相同。可以看到去除prefix- tuning中的domain- oriented初始化会使模型表现严重下降，说明domain word信息在面对新领域时引入相关知识的重要性。同时，移除离散的prompts也会使模型表现更糟糕（但仍然会好于Prefix-Tuning），说明离散的prompts能让模型更关注对话中核心的内容进而提升模型的表现。

![faf6a973a06d4f64bd74f3c99becce12](https://user-images.githubusercontent.com/47687248/165058585-f8e3dd24-9f68-4cc5-b0cb-670ddb698351.png)

### 5.4 Effect of Prefix Module in Encoder and Decoder

由于DOP-method在encoder和decoder中均引入了prefix模块，研究两个部分的prefix模块对模型表现的影响。可以看到，当两个部分的prefix被移除后，模型的表现均下降，说明了两个模块的prefix都是必要且高效的。

一个有趣的现象是移除encoder的prefix的影响要小于移除decoder的prefix的影响。一个比较合理的解释是在encoder和decoder端的prefix的作用是不一样的。在encoder端的prefix主要帮助模型理解对话，而decoder端的prefix主要帮助模型生成。因此，对于摘要生成，decoder端的prefix模块对模型更有用。

![4e9bda3719bb4c9bb210b920ab907f58](https://user-images.githubusercontent.com/47687248/165058661-66e0bf75-3766-41ed-9cf9-448b5e443e3a.png)


### 5.5 Human Evaluation

对模型进行了人工评估。

![f757f1261bda4bceb881baa790173595](https://user-images.githubusercontent.com/47687248/165058716-dcb18574-ec70-40d7-ac51-725654fd3cd4.png)

表中显示，所有模型的流畅程度都较高，说明在较强的backbone上微调的抽象摘要模型能够生成更流畅的句子。在事实一致性上，DOP以及BART ws DS好于Prefix-tuning的表现，说明对话状态信息能引导模型更关注与核心的信息，例如槽值和意图。初次之外，DOP-tuning在领域相关性上的表现超过了其他基线模型。说明了domain-oriented模块在提升模型识别领域相关特征以及从源域和目标域解藕出知识的能力。

### 5.6 Effect of Training Data

![20f9b8d72e8844abb49c580b1e47bdea](https://user-images.githubusercontent.com/47687248/165058800-9f6323d4-ac48-406a-b256-017a18156ae4.png)

#### 5.6.1 Performance in Few-shot Settings

对于TODSum数据集，固定源域的数据规模，将目标域的数据加入训练数据。可以看到随着目标域数据的增加，BART w. DS和DOP的表现提升，但DOP-tuning始终好于BART w. DS. 说明目标域的知识增加可以让模型学到目标域的信息。

#### 5.6.2 Effect of Source Domain Data Size

保持zero-shot的设置不变，调整源域数据的规模

![fca012c5f1e441718b2181ace0c80153](https://user-images.githubusercontent.com/47687248/165058855-56a9767a-da09-4f77-aee4-6c83e43c58b0.png)

可以看到随着数据规模的减小，BART w. DS的表现变差，而DOP-tuning能够相对优秀地保持稳定。说明DOP-tuning对数据规模不太敏感，并且具有一定程度的鲁棒性。这与主实验的结果一致，模型在有限和unseen data上表现优异。

### 5.7 Prefix Length vs. Input Length

研究Prefix Length和input Length的关系，具体而言source input length，target input length以及对应的optimal prefix length的关系。可以得出更长的inputs可能更青睐于更短的prefix.

![1d1a29e6f80b4d3b90e22b10c45c5215](https://user-images.githubusercontent.com/47687248/165058896-b39fa528-04f3-4dd6-85da-a32c49d54a81.png)

## 6. Challenges

总结了抽象对话摘要的低资源领域迁移的挑战：

1.  Confusion between domains with high similarity
    对于词汇表高度重合的领域，如restaurant和hotel，train和taxi，模型会产生domain-confusing句子。以hotel- restaurant对作为例子，当restaurant作为目标域，“book a restaurant room that can accommodate 3 people”会被生成，这样的句子其实更应该存在hotel领域中。但需要注意的是，这种challenge并不会影响关键因素的准确率，但language style则是不合适的。
2.  Information dispersion
    由于对话数据通常是长序列，因此模型很难对长对话中的所有方面都能pay attention，因此会产生对话中关键元素的注意力的偏差，尤其是在轻量和小参数训练的场景下。

## 7. Conclusion

在本文中，我们提出了基于高效且可泛化的微调方法面向领域的domain-oriented prefix-tuning模型解决对话摘要中的领域迁移的方法。使用领域词初始化的prefix模块能够从源域解藕出目标域的知识，而离散的prompts能够提升模型的泛化性。在zero-shot和few-shot下的实验说明我们的方法在两个数据集下取得了巨大的进步。
