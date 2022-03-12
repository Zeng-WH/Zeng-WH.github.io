---

layout:     post
title:      "Knowledge-Grounded Dialogue Generation"
subtitle:   从Knowledge-Grounded Dialogue Generation 开始调研！
date:       2022-03-12 20:30:00
author:     "Andrew Zeng"
tags:
    - 对话生成

---

Knowledge-Grounded Dialogue Generation的研究比Personal/Empathetic Dialogue Generation多不少，可以关注的角度也比较多。从knowledge的来源角度可以分为text knowledge（document），infobox knowledge（table）以及commensence knowledge facts等。

## A Three-Stage Learning Framework for Low-Resource Knowledge-Grounded Dialogue Generation  （EMNLP2021）

（1）构造大规模的knowledge- grounded的对话数据集是非常困难的，而在小规模的训练样本上面对新的领域时很难表现好，因此搭建低资源场景的knowledge-grounded dialogue系统是非常重要的。

（2）作者基于weakly supervised learning提出了三阶段的学习框架，利用ungrounded dialogues和unstructured knowledge来进行weakly supervised learning.

（3）对Transformer进行改造，让其decoder能够与dialogue history和knowledge进行cross attention.

（4）数据集包括Wizard-of-Wikipedia 以及  CMU_DoG.
每个wizard turn与从Wikipedia检索得到的60句话相关作为知识，通常是noise的。两个agent会围绕主题谈论。
CMU_DoG包含两个worders之间的conversation，了解background documents，但主要关注于film reviews

## CoLV: A Collaborative Latent Variable Model forKnowledge-Grounded Dialogue Generation （EMNLP 2021）

（1）之前的Knowledge-grounded dialogue生成的方法通常包括两个子任务：knowledge selection以及knowledge-aware response generation. 通常只强调知识选择的重要性，而没有显示建模选择到的知识和生成的response之间的关系。即使knowledge selection的多样性增加了，而knowledge-aware response generation的多样性仍然会被忽视。

（2）作者希望使用collaborative latent varaiable的方法（也就是knowledge selection，knowledge-aware response generation会各有latent variable，再建立两者的关系）

（3）数据集为knowledge-grounded dialogue datasets：Wizardof Wikipedia (WoW) and Holl-E. 

## More is Better: Enhancing Open-Domain Dialogue Generation viaMulti-Source Heterogeneous Knowledge （EMNLP 2021）

（1）之前的knowledge-grounded dialogue generation主要基于单一source的homogeneous knowledge. 作者可以同时使用不同来源的知识，包括但不限于commensence knowledge facts，text knowledge（documents）以及infobox knowledge

（2）对于给定的dialogue，通常根据entity name使用matching technique来检索knowledge entries. 这样检索到的知识可能会与dialogue context或者其他源的知识不相关甚至矛盾。作者使用reference selection来选择knowledge.

（3）由于dialogue utterances和不同的知识在词语的分布不同，那么不同的知识的词语分布会影响模型的预测过程。作者提出multi-reference generation机制来解决问题。

（4）对话数据集来自three open-released Chi-nese Weibo corpora，而不同的知识分别来自
**ConceptNet（commonsense knowledge）**：
总过包括 696K triples, 27K entities, and 26 relations （**可能适合multi属性**）
**Text Knowledge：**
collect introduc-tion paragraphs of 1,663K entities from ChineseWikipedia.
**Infobox Tables：**
collect infobox tablesof 1,581K entities from Chinese Wikipedia
对于每一个dialogue，检索到200个最相关的commensence triples，1个相关的text paragraph以及1个infobox table

