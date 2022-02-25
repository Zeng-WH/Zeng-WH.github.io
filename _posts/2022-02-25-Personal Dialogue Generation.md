# 2022-02-25-Personal Dialogue Generation

- [ ] VALUENET: A New Dataset for Human Value Driven Dialogue System （AAAI 2022）
（1）论文提出了用于评价文本属性的10种value，以及对应的数据集和模型。具体而言，对于给定的文本，模型会产生该文本在10个属性上的得分，进而得出该文本的属性。
（2）作者在Personal-Chat和EmpatheticDialogues上做了实验，比如在Personal-Chat上，
利用（1）中的模型可以得到每个utterance和personal的属性。利用这些属性，作者尝试提升对话生成的表现，具体就不细说了。
（3）利用论文里的方法，我们应该可以实现不同属性组合的方法。

- [ ] One Chatbot Per Person: Creating Personalized Chatbots based on Implicit User Profiles （SIGIR 2021）
（1） 以前的Personalized chatbots需要的是显示（explict）的personal数据（比如personal chat或者convai2），该文章从large-scale的对话历史中提取隐式（implicit）的personal profiles. 基于学到的user profile来生成personalized responses.
（2）为了强调historical responess与input post的关联，为historical post-response pairs建立了key-value memory网络。在历史记忆的基础上，基于highting historical responses来构建动态的post-aware user profile
（3）在Reddit dataset和Weibo dataset上训练，数据集的形式是对于每一个用户有特定的标记，每一个用户下有很多dialogue utterances对。

- [ ] Learning Implicit User Profiles for Personalized Retrieval-BasedChatbot （CIKM 2021）
（1） 跟上文一个组的工作，还是获取隐式的user profile. 通过建模personalized language style（using the user’s historical responses）以及personalized preferences（each post-response pair ）获得。
（2）基于检索的方式，匹配符合 personalized languagestyle and personalized preference的response.上一篇则是生成的方式。
（3）数据集一模一样。

