---
layout:     post
title:      "overleaf to arxiv的流程"
subtitle:   记录技术！
date:       2022-04-09 11:00:00
author:     "Andrew Zeng"
tags:
    - 技术分享
---

## 1. 前言

将overleaf上写的论文上传到arxiv，是非常方便的，但也存在一些让人折腾的细节，记录一下踩到的坑，给以后的自己和其他人借鉴。

## 2. 流程

### 2.1  从Overleaf上准备好预提交的论文文件

点击overleaf左上角的submit按钮，选择**Submit your paper to the arXiv.**

![截屏2022-04-09 上午10 44 56](https://user-images.githubusercontent.com/47687248/162554296-d828a50f-951d-44b3-8e9c-5e12d96f8166.png)

选择**Download project ZIP with submission files（e.g.bbl）**

![截屏2022-04-09 上午10 46 06](https://user-images.githubusercontent.com/47687248/162554306-8d3ad315-a887-481f-864d-82d1ed12cacc.png)

原因是使用overleaf的过程中，参考文献通常会写在`.bib`文件里。再在正文中用如下命令调用

```latex
\bibliographystyle{IEEEtran}
\bibliography{reference.bib}
```

然而将overleaf上的文件直接打包下载再上传arxiv的时候会报出如下错误

![](https://hua-ys.github.io/img/post-oa-bbl-error.jpg)

解决方案很简单，这里需要利用到overleaf提供的**submit**功能，替你编译并生成`.bbl`文件。submit界面中选择arxiv并`Download project ZIP with submission files (e.g. .bbl)`。

### 2.2 arXiv提交论文

#### 2.2.1 Start

登陆进arxiv，就可以START NEW SUBMISSION，第一页如下图所示

![](https://pic2.zhimg.com/80/v2-889ae02c3a51c7b5fac8411a146d745d_720w.jpg)

重点关注的是 License，对于要发表的文章，一般选择是

倒数第二个 **[http://arxiv.org](https://link.zhihu.com/?target=http%3A//arxiv.org) perpetual, non-exclusive license**

其次用得多的是 第一个 CC 署名 license

#### 2.2.2 Add Files

下一步我们就到了提交文件

![img](https://pic2.zhimg.com/80/v2-2b04320bf82ce0cac0ca6f6226f9348d_720w.jpg)

上传我们在第一步准备好的文章。这里记一下自己遇到的坑：

1. 需要删除其他PDFLATEX文件（比如这里的`acl.text`,`Response_Letter.tex`），只保留论文对应的PDFLATEX文件。
2. arxiv网站对文件命名的支持范围比较低，会将一些文件名替换成他们支持的文件名，这很可能会导致PDFLATEX无法找到对应的文件，进而无法编译。比如这里：


![截屏2022-04-09 上午11 00 40](https://user-images.githubusercontent.com/47687248/162554324-613615e3-d9bc-4894-a759-1bb3179a1024.png)

​		`ACL2022_new(3).pdf`的文件名就被更改为`ACL2022_new_3_.pdf`

​		在编译的时候就会有如下报错：


![截屏2022-04-09 上午11 02 38](https://user-images.githubusercontent.com/47687248/162554338-77139b93-6908-44ab-884f-0b385bcbd663.png)


​		需要及时修改PDFLATEX文件中使用的文件名

#### 2.2.3 Process

编译成功：

![截屏2022-04-09 上午11 08 09](https://user-images.githubusercontent.com/47687248/162554350-63c6db5b-3d76-4220-b792-26b3fb7d0836.png)

#### 2.2.4 填写文章基本信息

![](https://pic1.zhimg.com/80/v2-0df61b8d8c4ba337eaf2235e3d811db8_720w.jpg)

作者用逗号隔开

摘要不支持 latex 语法，不要换行

Comments可以写中了什么会议

## 3. 后续

大约两三天后，会收到成功的邮件，会给你一个文章的密码，这是用来让你的共同作者也登记这篇文章到他的账号下，可以共同管理。

从arXiv到被 谷歌学术 收录大概需要一周的时间。
