---

layout:     post
title:      "pyplot的一些tips"
subtitle:   分享技术！
date:       2022-06-12 13:30:00
author:     "Andrew Zeng"
tags:

  - 技术分享
---

不断更新pyplot使用过程中的一些tips，供参考！

## 1. plt.scatter

相关文档：https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html

### 1.1 简言之

```python
import matplotlib.pyplot as plt
plt.scatter(X[:, 0], X[;,1]) # 二维散点图
plt.scatter(X[:, 0], X[;,1], X[:, 2]) # 三维散点图
```

### 1.2 添加散点颜色

```python
color = [0,1,2,....,len(X)] # color 可以为列表
plt.scatter(X[:, 0], X[;,1], c=color) # 二维散点图
```

### 1.3 改变散点大小

```python
plt.scatter(X[:, 0], X[;,1], s=30)
```

### 1.4 增加散点名称

如果需要在图中标出每个散点的名称。采用`plt.annotate`, 假设`scatter_name`为散点名称列表

```python
plt_name = []
for i, tlt in enumerate(scatter_name):
  plt_name.append(plt.annotate(tlt, X[i], fontsize=6)) # fontsize可以改变字号
```

在散点较为密集的情况下注释会重合，可以使用`adjustText`解决`matplotlib plt.text()`文字重叠问题

参考：https://zhuanlan.zhihu.com/p/360342023

安装`adjustText`：

```python
pip install adjustText
or 
conda install -c conda-forge adjusttext
```

使用：

上述代码后增加一行

```python
adjust_text(plt_name)
```





