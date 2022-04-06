---
layout:     post
title:      "与对比学习相关的Prompt-Learning"
subtitle:   精读论文！
date:       2022-04-06 10:30:00
author:     "Andrew Zeng"
tags:
    - 对比学习
    - Prompt Learning

---

# Prototypical Verbalizer for Prompt-based Few-shot Tuning （ACL 2022）

## 1. 解决的问题

Prompt-based的方法在预测时，需要将output words映射为verbalizer中的words. 而manual verbalizer取决于domain-specific prior knowledge以及human efforts；而search-based verbalizer以及soft verbalizer的方法在low-data regime的情形下很难在large vocabulary或者embedding space中寻找或获得verbalizer.
作者尝试通过估计出每一类别的prototype vectors来作为verbalizer. 作者使用带有InfoNCE estimator的对比学习来训练prototype vectors.
(prototypes作为同一类别中的instances的embeddings的代表，能够encapsulate一些class-level semantic features)

## 2. Prototypical Verbalizer

![982dfa8511954e239f3e2ff152647646](https://user-images.githubusercontent.com/47687248/161882820-9025c7ff-93fb-4fa9-a70e-f9d11c4b698d.png)
<img width="632" alt="截屏2022-04-06 上午10 09 23" src="https://user-images.githubusercontent.com/47687248/161882866-2db4f11b-91f6-498a-a1dd-31311b0e9f04.png">

## 3. Experiments

### 3.1 Single Verbalizer Results
ProtoVerb在小样本的情况下，尤其是在每类只有1 or 2 instances 的情况下表现远远甚于SearchVerb和SoftVerb. 当训练样本sufficient时，ProtoVerb能够达到超越ManualVerb的表现。
![2b8646a847ad4c119f5c16abb20de007](https://user-images.githubusercontent.com/47687248/161883401-72d10161-6b83-4b0d-af2e-50c3a80242ca.png)

### 3.2 Multiple Verbalizer Results
![a900d8803b7b47c9a3df0009110845c0](https://user-images.githubusercontent.com/47687248/161883475-f888e002-2768-45ad-826a-bf365def3d1c.png)
1. 在小样本的情况下，prompt-based tuning远远超过fine-tuning方法。当有足够训练数据时，fine-tuning模型能够产生相当的结果。
2. ProtoVerb + ManualVerb方法结合时，能够产生更好的结果。
3. ProtoVerb+在untuned PLMs上仍然非常有效，证明了这种no-tuning method的有效性。

## 4. Analysis

### 4.1 Fixed Model Experiments
定量分析当PLMs固定时verbalizers的影响
![bc08089fc72c49bead8c37e5ef9c55a0](https://user-images.githubusercontent.com/47687248/161883521-a3e93fc6-974b-49cf-b7c6-33f73a904797.png)

### 4.2 Ablation Study
![fa0a433d0e7343e3984e30adf9005494](https://user-images.githubusercontent.com/47687248/161883642-4150a673-8c0f-48b6-aa52-9bba6644dca4.png)

### 4.3 Robustness on Noisy Samples
![999eb1a3bce74c7c8b8c6524b7f1c39d](https://user-images.githubusercontent.com/47687248/161883684-242cbf4e-e88b-4fac-80c8-dd8fca78c3d0.png)

### 4.4 Prototype Discretization
作者收集了每一类中最similar的词语，可以这样理解，prompt机制使得PLMs能够选择最conclusive的信息，并且在[MASK]位填上。可以看到随着training instances的增加，更"conceptual"的词语会出现表格中。
![777232c15f284455883673c1d52dbb38](https://user-images.githubusercontent.com/47687248/161883826-c95708ed-ca1a-4bc8-ab6d-c07b769db70b.png)

# Making Pre-trained Language Models End-to-end Few-shotLearners with Contrastive Prompt Tuning

## 1. 解决的问题：
prompt-based learning的表现极大依赖于handcrafted prompts以及verbalizers，限制了方法在实际世界的使用。作者提出的end-to-end Constrastive Prompt Tuning框架来在不需要手动设计task- specific prompts以及verbalizers来fine-tuning PLMs.
## 2. CP-TUNING: PROPOSED APPROACH

### 2.1 Task-invariant Prompt Encoding
![567ec3cd5b1841e09ba21e3ae366ee08](https://user-images.githubusercontent.com/47687248/161883911-ab3b32cd-1911-4e92-824c-835605e92414.png)
```
[PRO] (Prompt) 连续的prompt embeddings
[TMSK] (Token Mask) 用来算MLM loss的token mask
[OMSK] (Output Mask) 用来生成output result placeholder
```
### 2.2 Verbalizer-free Class Mapping
当前的prompt-based方法需要设计handcrafted verbalizer来建立tokens和class labels. Verbalizer-based的方法在entire vocabulary产生distribuions，仅关注其中few words的概率（比如例子中的”good“和”terrible“）而词语之间的semantic association会被很大程度忽视。比如”nice“，”fantastic“，”bad“以及”horrible“也会与class labels相关。如果将high-dimensinal, sparse distribution 替换为lower-dimensional，dense representations, 模型的generalization能力和灵活性会大大降低。
<img width="628" alt="截屏2022-04-06 上午10 31 17" src="https://user-images.githubusercontent.com/47687248/161884068-d89053ec-c217-498d-b1a7-728b395e87e5.png">
<img width="641" alt="截屏2022-04-06 上午10 31 49" src="https://user-images.githubusercontent.com/47687248/161884205-49cfdd5a-0f09-4c83-917f-38451c643b8c.png">

### 2.3 Model Inference
<img width="649" alt="截屏2022-04-06 上午10 32 50" src="https://user-images.githubusercontent.com/47687248/161884260-747b730c-ac46-4013-8b78-44527f5384ea.png">

## Experiments

主实验表明：
1. Prompt-based methods在few-shot上的表现远远超过standard fine-tuning
2. CP-Tuning相比较所有state-of-the-art的方法都有一致的提升。
其余的分析实验如消融实验，Parameter analysis，batch size analysis以及visualization，不平衡数据分析详见论文。

