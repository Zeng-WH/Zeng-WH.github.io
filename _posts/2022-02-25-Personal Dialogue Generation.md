---
layout:     post
title:      "Personal Dialogue Generation"
subtitle:   从Personal Dialogue Generation 开始调研！
date:       2022-02-25 22:00:00
author:     "Andrew Zeng"
tags:
    对话生成
---


**VALUENET: A New Dataset for Human Value Driven Dialogue System （AAAI 2022）**


（1）论文提出了用于评价文本属性的10种value，以及对应的数据集和模型。具体而言，对于给定的文本，模型会产生该文本在10个属性上的得分，进而得出该文本的属性。

（2）作者在Personal-Chat和EmpatheticDialogues上做了实验，比如在Personal-Chat上，
利用（1）中的模型可以得到每个utterance和personal的属性。利用这些属性，作者尝试提升对话生成的表现，具体就不细说了。

（3）利用论文里的方法，我们应该可以实现不同属性组合的方法。

**One Chatbot Per Person: Creating Personalized Chatbots based on Implicit User Profiles （SIGIR 2021）**


（1） 以前的Personalized chatbots需要的是显示（explict）的personal数据（比如personal chat或者convai2），该文章从large-scale的对话历史中提取隐式（implicit）的personal profiles. 基于学到的user profile来生成personalized responses.

（2）为了强调historical responess与input post的关联，为historical post-response pairs建立了key-value memory网络。在历史记忆的基础上，基于highting historical responses来构建动态的post-aware user profile

（3）在Reddit dataset和Weibo dataset上训练，数据集的形式是对于每一个用户有特定的标记，每一个用户下有很多dialogue utterances对。

**Learning Implicit User Profiles for Personalized Retrieval-BasedChatbot （CIKM 2021）**


（1） 跟上文一个组的工作，还是获取隐式的user profile. 通过建模personalized language style（using the user’s historical responses）以及personalized preferences（each post-response pair ）获得。

（2）基于检索的方式，匹配符合 personalized languagestyle and personalized preference的response.上一篇则是生成的方式。

（3）数据集一模一样。


**Generating Personalized Dialogue via Multi-Task Meta-Learning** （PotsDial 2021）

（1）解决的问题不算新，将模型更好地adapt到新的persona上。之前已经有工作，如PAML（Persona Agnostic Meta-Learning，19年的工作）使用meta-learning来解决问题。作者指出PAML在meta-learning和inference的过程中没法使用persona statements。

（2）作者提出引入persona construction task，是得模型在meta-learning的过程中使用persona statement. 元学习的任务根据不同的persona来划分。

（3）数据集是PersonChat，评价还是那些指标。

**DLVGen: A Dual Latent Variable Approach to Personalized Dialogue Generation** （ICAART 2022）

（1）与上文同一个组的工作，还是解决在推理的时候没有personal statement的情况（ generate  personalized  dialogue  givenonly the dialogue context）

（2）但在训练的时候还是会使用使用personal statement，同时为了区别于CVAE，多加了一种latent variabel，因此还加了正则化来规范化两种latent varibale.

（3）采用了Convai2数据集。

**Learning to Predict Persona Information for Dialogue Personalization without Explicit Persona Description** （2021.11.30）

（1） 还是考虑在测试的时候没有显示的personal information的问题，相比较之前的工作，在提取出的personal information表示的可解释性上做了工作。

（2）两种方法分别是persona approximator和persona generator，为了增强可解释性，如persona approximator会将persona encoder的输出与对persona information编码的信息比较相似度。persona generator生成显式的persona信息。

（3）当condition在self persona上时，persona decription的加入会使得response中保持流畅性和一致性。当condition在their persona时，能够增加用户的参与感。

（4）数据集是persona chat和dailydialog chat，为了测试模型的可迁移性，在persona上pretrain后，在dailydialog上finetune

**Federated Natural Language Generation for Personalized Dialogue System** （2021.10.13 投了AAAI 2022，但应该没中）

（1）提出persona information的data privacy的问题，并将联邦学习框架结合对话生成来解决safe personalized services的问题。

（2）使用的数据集包括**Cornell  Movie-Dialogs**（contains a large metadata-rich collection of fictional conversations extracted from raw movie scripts）

数据集描述：

<img width="784" alt="5cbea999f2ab4615bc4a61f974efb4bb" src="https://user-images.githubusercontent.com/47687248/155882426-d99b027a-794a-4bfa-a0de-dea495404373.png">

以及**The Big Bang Theory** dataset 和 **Friends** dataset

**Call for Customized Conversation : Customized Conversation Grounding Persona and Knowledge** 

（1）作者希望解决的问题，希望基于knowledge的模型在给出knowledge-groubded的回答时能够根据用户的personal information. 具体说来，在给素食者提供餐馆的时候，需要考虑对方的素食的persona. 但之前的数据集显然并不能既有persona information又具有knowledge.

（2）因此，作者提出了新的数据集FoCus，支持knowledge-grounded answers that reflect user’s persona. 在数据集上还提了baseline.

（3）FoCus数据集，Persona由5个sentence定义（experience,preference, possession, hobby or interest）以及knowledge构成
<img width="354" alt="2fec57d31b91453c964ae196fdd44b6f" src="https://user-images.githubusercontent.com/47687248/155882529-705c5820-7322-4ad1-8ae4-d602dd107642.png">

**Dual Task Framework for Improving Persona-grounded Dialogue Dataset** （AAAI 2022）

（1）之前的personal dialogue generation模型主要是从模型的角度出发，作者从persona dataset数据集本身出发，之前数据集中的persona information缺少detailed experiance以及knowledge. 之前的person information存在不少linguistic biases，比如persona discription的reuse等。

（2）作者提出了对偶任务框架，用来生成相关的persona来提升数据集。

**Transferable Persona-Grounded Dialogues via Grounded Minimal Edits** （EMNLP 2021）

(1) 之前的受控生成在transferability存在两个困难：1. 由于标注者通常将persona或者 knowledge嵌入response，会导致grounded dialogue dataset和natural conversations之间存在distributional gap，因此训练出的模型会产生不自然的responses，并且会受dialogue history的distributional shift的影响。2. 没办法对unseen类型的concept进行泛化，比如在PERSONACHAT上训练的模型无法泛化到world knowledge上。

(2) 作者提出grounding minimal editor，在existing responses上进行操作。最小化编辑existing responses，使其ground在concept同时，和dialogue history coherent. 但当提供其他类型的concept，不会遭遇gap

(3) 作者在PERSONCHAT数据集基础上提出了PERSONAMINEDIT，用来衡量方法的有效性。

