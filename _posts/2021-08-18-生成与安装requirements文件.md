# 2021-08-18-生成与安装requirements文件

## 1. 使用pip freeze生成

一般情况，我们可以直接使用Python下的 `pip` 包管理工具，来生成 requirements.txt 文件，命令如下：



```python
pip freeze > D:\pycharm\requirements.txt
```

其中，`D:\pycharm\requirements.txt` 为生成的 requirements.txt 文件的具体路径。通过这个  `pip freeze`  方式生成时，会把整个Python环境下的所有包都列出生成，比较适用于Python项目为虚拟环境的情况。

## 2. 安装requirements.txt中的内容

如果要安装requirements.txt中的类库内容，那么你可以执行

```python
pip install -r requirements.txt
```

