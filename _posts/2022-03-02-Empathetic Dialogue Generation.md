---
layout:     post
title:      "Empathetic Dialogue Generation"
subtitle:   从Empathetic Dialogue Generation 开始调研！
date:       2022-03-02 23:30:00
author:     "Andrew Zeng"
tags:
    对话生成
---

## **Knowledge Bridging for Empathetic Dialogue Generation**  （AAAI 2022）

（1）. EMPATHETIC-DIALOGUES dataset， speaker被要求围绕着personal emotional feelings进行谈论，模型需要从speaker的话语中推测出underlying emotion并且responds empathetically。

（2）. 作者解决的问题就是，由于模型缺少外部的知识，因此很难从有限的对话历史中获得隐式的emotions以及emotional interactions。（而人类通常是根据经验和外部知识来表达隐式情感）

<img width="715" alt="40813048c7494561ae0fb1abc25e9902" src="https://user-images.githubusercontent.com/47687248/156395617-7e9c6d24-f763-488b-84d6-35257b478547.png">

在EMPATHETICDIALOGUES中
作者这张图比较形象，a表示dialogue history与response其实几乎没有non-stopword重叠（仅有0.5% dialogue samples），这也能解释模型在摘要数据集的bleu和rouge上的指标要高于对话数数据。而当引入外部数据后，从对话历史中的non-stop tokens可以根据knowledge path获得hints


（3）. 在empathetic  conversations中，聆听者通常会mirror对话者之间的emotion来建立支持。因此建立interlocutors之间的emotional depencises也是很重要的。

（4）. 作者引入了外部的情感知识，并基于对话历史构建了emotional context graph并将其加入到encoder以及decoder中。并且搭建了emotion-depency decoder来建模emotion depencies来生成empathetic response.

##  CEM: Commonsense-aware Empathetic Response Generation （AAAI 2022）
（1）. 之前Empathetix Response Generation的研究主要在监测用户和使用用户的emotion来生成empathetic responses

（2）. 作者认为让chatbot表现出empathy，需要affection和cognition两个方面，affection指的是识别用户的emotion，cognition指的是考虑用户的situation并能做出response.

（3）. 作者的方法就是使用commonsense knowledge来获得关于用户的处境的信息，进而使用额外的信息来生成empathetic dialogue generation。

（4）. 同样是在EMPATHETICDIALOGUES数据集上操作。

## Towards an Online Empathetic Chatbot with Emotion Causes （SIGIR 2021 短文）
（1）. 作者认为在Empathetic Resonding中，学习到引起用户的emotion的causes是非常重要的（emotion causes）。之前的研究还是在根据具体的emotion class来控制回复。

（2）. 在实际的网络上的数据（社交平台）中，人们通常不愿意分享负面的情感。因此在网路上的数据，emotion causes的含量是很低的（EMPATHETICDIALOGUES的emotion causes的含量高，但与网络的数据差异比较大），作者因此提出了数据集X-EMAC，会包含emotion label和emotion cause.

（3）. 利用这些数据在CDialoGPT上测试了数据。

## 关于数据集EMPATHETIC-DIALOGUES dataset
The EmpatheticDialogues dataset is a large-scale multi-turn empathetic dialogue dataset collected on the Amazon Mechanical Turk, containing 24,850 one-to-one open-domain conversations. Each conversation was obtained by pairing two crowd-workers: a speaker and a listener. The speaker is asked to talk about the personal emotional feelings. The listener infers the underlying emotion through what the speaker says and responds empathetically. The dataset provides 32 evenly distributed emotion labels.

## Emotional/Empathetic dialogue generation区别
Emotional dialogue generation通过手动设置specified label来控制emotional responses；Empathetic dialogue generation模型需要从speaker的话语中推测出emotion，并且responds empathetically.





