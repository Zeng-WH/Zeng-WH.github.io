---
layout:     post
title:      "grep查找进程"
subtitle:   记录技术！
date:       2022-05-19 21:00:00
author:     "Andrew Zeng"
tags:
    - 技术分享
    - Linux服务器
---

当无法通过`top-i`获取进程的PID时，可以通过grep查找执行的命令从而获得对应的PID.

## 1. 查找指定进程

`train_gpt_prompt`是需要查找的进程的关键字

```shell
ps -ef | grep train_gpt_prompt //train_gpt_prompt是进程关键字
```

```shell
(***/conda-env/seq2seq) [*** ~]$ ps -ef | grep train_gpt_prompt

hadoop-+ 1313922 390945 0 11:40 pts/248 00:00:00 bash train_gpt_prompt10.sh

hadoop-+ 1393966 517060 0 14:49 pts/249 00:00:00 bash train_gpt_prompt_ctrl1.sh

hadoop-+ 1500439 1499837 0 20:49 pts/256 00:00:00 grep --color=auto train_gpt_prompt
```

第一条记录是查找出的进程；第二条结果是[grep](https://so.csdn.net/so/search?q=grep&spm=1001.2101.3001.7020)进程本身，并非真正要找的进程。

## 2. 查找指定进程个数

```shell
ps -ef | grep mmp-front -c  或者
ps -ef | grep -c mmp-front12
```

```shell
(***/conda-env/seq2seq) [*** ~]$ ps -ef | grep train_gpt_prompt -c
3
```

## 3. 杀死进程

```shell
kill -KILL PID
```

