---

layout:     post
title:      "Task-Oriented Dailogue Pre-Trained Model"
subtitle:   TOD调研！
date:       2022-07-17 21:00:00
author:     "Andrew Zeng"
tags:

  - 对话生成
  - Knowledge-Grounded Dialogue
---

调研了常见的Task-Oriented对话预训练模型，包括TOD-BERT，UBAR，PLATO系列，PPTOD，GALAXY以及SPACE.

## TOD-BERT

https://www.semanticscholar.org/paper/TOD-BERT%3A-Pre-trained-Natural-Language-for-Dialogue-Wu-Hoi/5b015296730273921889e54a0a31e3b173017026

### 1. Year

2020.10

### 2. BackBone

BERT (由BERT-base-uncased模型初始化)

TOD-BERT代码：https://github.com/jasonwu0731/ToD-BERT

同时开源了权重

### 3. Training Data

英文数据

9 nine different task-oriented datasets which are English, human-human and multi-turn.

In total, there are 100,707 dialogues, which contain 1,388,152 utterances over 60 domains.

<img width="675" alt="截屏2022-07-17 下午1 59 30" src="https://user-images.githubusercontent.com/47687248/179397434-39069ea4-7701-4e00-b694-490c56266f97.png">

### 4. Training

将对话$D=\{S_{1},U_{1},\cdots,S_{n},U_{n}\}$, 拼接为$[SYS]S_{1}[USR]U_{1}\cdots$, 仍然使用标准的position embeddings以及segmentation embeddings.

<img width="565" alt="截屏2022-07-17 下午2 13 58" src="https://user-images.githubusercontent.com/47687248/179397555-544bc4c7-18a8-470d-a7c8-e250e83522ff.png">

训练策略采用了BERT的Masked Language modeling（MLM）以及Response contrastive loss（RCL）。

1. MLM
2. RCL则对于golden response以及同一个batch中的其他response的[CLS]的respresentation使用对比学习

注：由于在intent分类的过程中需要[CLS]的表示，使用无监督的方法训练[CLS]的表示是个可以努力的方向

### 5. Finetune Task

Intent Recognition; Dialogue State Tracking; Dialogue Act Prediction; Response Selection

## UBAR

https://www.semanticscholar.org/paper/UBAR%3A-Towards-Fully-End-to-End-Task-Oriented-Dialog-Yang-Li/63169665bd592fb818678c47644b29302877d50e

### 1. Year

2021.03

### 2. BackBone

GPT-2

UBAR代码：https://github.com/TonyNemo/UBAR-MultiWOZ

同时开源了权重

### 3. Training Data

只有finetune阶段MultiWOZ 2.0&2.1

#### 4. Training

<img width="743" alt="截屏2022-07-17 下午7 25 37" src="https://user-images.githubusercontent.com/47687248/179397915-53227a11-d7d1-4864-a8fd-b9711b03fc31.png">

#### 5. Finetune Task

End-to-end Modeling; Dialog State Tracking

## PPTOD

https://www.semanticscholar.org/paper/Multi-Task-Pre-Training-for-Plug-and-Play-Dialogue-Su-Shu/841ca3d87a422451596b8a4b8350e92106971791

### 1. Year

2022.03

### 2. BackBone

T5 (包括small,base,large)

### 3. Training Data

In total, there are over 2.3M utterances across 80 domains.
 
PPTOD代码：https://github.com/awslabs/pptod

权重已开源

### 4. Training

将多个TOD任务统一为Text-to-Text的形式

<img width="831" alt="截屏2022-07-17 下午6 36 49" src="https://user-images.githubusercontent.com/47687248/179398248-1a64c928-0f73-4a06-b8d7-d3b3bf72138f.png">

### 5. Finetune Task

Intent Recognition; End-to-end Modeling; Dialog State Tracking

## PLATO

PLATO其实开放域的预训练模型。

https://aclanthology.org/2020.acl-main.9.pdf

### 1. Year

2020.04

### 2. BackBone

UniLM （supports both bi-directional encoding anduni-directional decoding） 权重由$BERT_{Base}$初始化得到 132M

PLATO代码：https://github.com/PaddlePaddle/Research/tree/master/NLP/Dialogue-PLATO

权重已开源

### 3. Training Data

Large-scale conversation datasets –Twitter (Cho et al., 2014) and Reddit (Zhou et al.,2018; Galley et al., 2019) are employed for pre-training, which results in 8.3 million training samples in total. (训了两周)

### 4. Training

输入内容包括latent variable（z); dialogue context以及response.  其中z的token embedding $E_{z}$直接由latent embedding sapce得到。

训练策略包括negative log-likelihood(NLL)loss,bag-of-words(BOW)loss以及response selec-tion(RS)loss.

1. NNL loss 
2. BOW loss （使用latent variable以及context去预测response中的word）
3. RS loss (helps distinguish whether theresponse is relevant with the dialogue context andconsistent with the background knowledge)

<img width="824" alt="截屏2022-07-17 下午3 36 09" src="https://user-images.githubusercontent.com/47687248/179398534-634cb7d4-29ec-4a6c-8df7-708daca4b7e7.png">

### 5. Finetune Task

End-to-end Modeling

## PLATO-2

### 1. Year

2021.05

### 2. BackBone

UniLM架构 1.6B

PLATO-2代码：https://github.com/PaddlePaddle/Knover/tree/develop/projects/PLATO-2

英文权重已开源

### 3. Training Data

The training set contains 811M (context, response) samples, ranging from December 2005 to December 2019. For the validation set, 0.2M samples are selected from the rest data after December2019. The English vocabulary contains 8K BPE tokens (Sennrich et al., 2016), constructed with theSentencePiece library. The Chinese pre-training data is collected from public domain social medias.After filtering, there are 1.2B (context, response) samples in the training set.  As for the Chinese vocabulary, it contains 30K BPE tokens.

### 4. Training

<img width="802" alt="截屏2022-07-17 下午5 45 52" src="https://user-images.githubusercontent.com/47687248/179398769-3a2e8ba9-7380-4fe9-bdf7-9d6fb62da641.png">

### 5. Fintune Task

End-to-end Modeling

## PLATO-XL

### 1. Year

2021.09

### 2. BackBone

UniLM架构 11B

<img width="720" alt="截屏2022-07-17 下午4 49 44" src="https://user-images.githubusercontent.com/47687248/179398816-c1c37e89-7225-48de-b735-ea7198449fe1.png">

PLATO-XL代码：https://github.com/PaddlePaddle/Knover/blob/develop/projects/PLATO-XL/README.md

英文权重已开源

### 3. Training Data

同上

### 4. Training

规模上来了以后就NLL Loss即可

<img width="723" alt="截屏2022-07-17 下午5 05 34" src="https://user-images.githubusercontent.com/47687248/179398911-06900238-18db-4a37-954c-7626760e44f8.png">

### 5. Finetune Task

End-to-end Modeling; Dialog State Tracking

## GALAXY

经过与PLATO的比较，GALAXY更像是拥有标签数据的版本。具体而言，作者构造了有标签的数据，如dialogue act有了标签。

https://www.semanticscholar.org/paper/GALAXY%3A-A-Generative-Pre-trained-Model-for-Dialog-He-Dai/127ffc8697630a76b1b4149c24d1350f69205f41

### 1. Year

2022.03

### 2. BackBone

Unilm

GALAXY代码：https://github.com/siat-nlp/GALAXY

### 3. Training Data

UniDA（Labeled Dataset）：使用统一的DA taxonnmy做TOD任务。 UniDA（Unlabled Dataset）

<img width="309" alt="截屏2022-07-17 下午7 37 44" src="https://user-images.githubusercontent.com/47687248/179399043-5351652e-28ae-441e-9cf3-555b790980ca.png">

### 4. Training

与第一代PLATO非常相似

<img width="709" alt="截屏2022-07-17 下午7 38 23" src="https://user-images.githubusercontent.com/47687248/179399060-53176be6-03f4-4c71-9a10-b87460ee7063.png">

### 5. Finetune Task

End-to-end Modeling

## SPACE

相比于GALAXY，加入了Prompt，同时分为多个module，可以承担不同的任务

https://www.semanticscholar.org/paper/Unified-Dialog-Model-Pre-training-for-Task-Oriented-He-Dai/553703db1b3e54e957ed91bad952ff3ba4f59bd5

### 1. Year

2022.07

### 2. BackBone

Unilm

### 3. Training Data

<img width="346" alt="截屏2022-07-17 下午7 43 31" src="https://user-images.githubusercontent.com/47687248/179399185-d0ffc023-3e84-4247-bbeb-4da3939eb905.png">

### 4. Training

基于有标签数据和无标签数据设计了多任务训练目标。

<img width="733" alt="截屏2022-07-17 下午7 45 24" src="https://user-images.githubusercontent.com/47687248/179399272-d67d0dbd-8cb3-44ea-830e-7de082c11c78.png">

### 5. Finetune Task

Intent Recognition; End-to-end Modeling; Dialog State Tracking

具体而言：

For dialog understanding tasks, we only leveragethe dialog encoding module and the dialog understanding moduleto fine-tune our pre-trained model, discarding the dialog policy andgeneration modules. Thus only the understanding prompt sequence𝑝𝑢is employed to extract sentence representations, which are usedfor classification tasks such as intent recognition or dialog statetracking. For policy optimization and dialog generation tasks, weretain the integrated model architecture to produce appropriateresponses, where two kinds of prompts $p^{u}$and $p^{o}$ are adopted tocharacterize the dialog understanding and dialog policy for betterdialog generation.








