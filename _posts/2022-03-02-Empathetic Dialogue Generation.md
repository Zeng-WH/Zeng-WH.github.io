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

1. EMPATHETIC-DIALOGUES dataset， speaker被要求围绕着personal emotional feelings进行谈论，模型需要从speaker的话语中推测出underlying emotion并且responds empathetically。
2. 作者解决的问题就是，由于模型缺少外部的知识，因此很难从有限的对话历史中获得隐式的emotions以及emotional interactions。（而人类通常是根据经验和外部知识来表达隐式情感）

在EMPATHETICDIALOGUES中
作者这张图比较形象，a表示dialogue history与response其实几乎没有non-stopword重叠（仅有0.5% dialogue samples），这也能解释模型在摘要数据集的bleu和rouge上的指标要高于对话数数据。而当引入外部数据后，从对话历史中的non-stop tokens可以根据knowledge path获得hints

3. 在empathetic  conversations中，聆听者通常会mirror对话者之间的emotion来建立支持。因此建立interlocutors之间的emotional depencises也是很重要的。
4. 作者引入了外部的情感知识，并基于对话历史构建了emotional context graph并将其加入到encoder以及decoder中。并且搭建了emotion-depency decoder来建模emotion depencies来生成empathetic response.





