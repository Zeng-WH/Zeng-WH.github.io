---

layout:     post
title:      "Markdown的一些Tips"
subtitle:   分享技术！
date:       2022-06-14 12:11:00
author:     "Andrew Zeng"
tags:

  - 技术分享
---

不断更新Markdown使用过程中的一些Tips，供参考！

# 1. Markdown中的注释

参考：https://stackoverflow.com/questions/4823468/comments-in-markdown

That is:

```
[comment]: <> (This is a comment, it will not be included)
[comment]: <> (in  the output file unless you use it in)
[comment]: <> (a reference style link.)
```

Or you could go further:

```
[//]: <> (This is also a comment.)
```

To improve platform compatibility (and to save one keystroke) it is also possible to use `#` (which is a legitimate hyperlink target) instead of `<>`:

```
[//]: # (This may be the most platform independent comment)
```

I use standard HTML tags, like

```
<!---
your comment goes here
and here
-->
```