# 2021-04-04-Centos7安装Brat工具及使用

BRAT是一个基于web的文本标注工具，主要用于对文本的结构化标注，用BRAT生成的标注结果能够把无结构化的原始文本结构化，供计算机处理。利用该工具可以标注实体，事件、关系、属性等，只支持在linux下安装，其使用需要webserver，官方给出的教程使用的是Apache2。

参考：https://blog.csdn.net/u011440696/article/details/109364035

## 1 配置Apache

### 1.1 安装Apache

Centos7下Apache的安装包叫httpd，执行以下命令进行安装：

```bash
yum install httpd -y
```

### 1.2 修改主配置文件httpd.conf

```bash
vim /etc/httpd/conf/httpd.conf
```

添加如下内容：

```bash
<Directory /var/www/html/brat>
    AllowOverride Options Indexes FileInfo Limit
    Require all granted
    AddType application/xhtml+xml .xhtml
    AddType font/ttf .ttf
For CGI support
    AddHandler cgi-script .cgi
    # Comment out the line above and uncomment the line below for FastCGI
    AddHandler fastcgi-script fcgi
</Directory>
```

/var/www/html/brat路径是与后面要安装brat的路径对应！

### 1.3 配置userdir模块

使用以下命令：

```bash
vim /etc/httpd/conf.d/userdir.conf
```

找到文件中代码：

```bash
<IfModule mod_userdir.c>
    #
    # UserDir is disabled by default since it can confirm the presence
    # of a username on the system (depending on home directory
    # permissions).
    #
    UserDir disabled

    #
    # To enable requests to /~user/ to serve the user's public_html
    # directory, remove the "UserDir disabled" line above, and uncomment
    # the following line instead:
    #
    #UserDir public_html
</IfModule>
```

修改为

```python
<IfModule mod_userdir.c>
    #
    # UserDir is disabled by default since it can confirm the presence
    # of a username on the system (depending on home directory
    # permissions).
    #
    #UserDir disabled

    #
    # To enable requests to /~user/ to serve the user's public_html
    # directory, remove the "UserDir disabled" line above, and uncomment
    # the following line instead:
    #
    #UserDir public_html
    UserDir brat
</IfModule>
```

### 1.4 开启Apache服务

```
systemctl start httpd
```

#### 1.4.1 设置开机自启Apache服务

```
systemctl enable httpd
```

#### 1.4.2 Apache命令大全

后面每次更改配置文件后，都需要重启apache服务

```
#重新启动Apache
systemctl restart httpd
#停止Apache
systemctl stop httpd
#查看Apache状态
systemctl status httpd
#设置开机不启动Apache
systemctl disable httpd
```

### 1.5 测试Apachhe

Apache的存放路径：/var/www/html
进入apache网页的存放路径，进行网页测试:

```bash
cd var/www/html
```

新建一个测试网页：

```bash
vim index.html
```

在文件中添加以下内容：

```html
<html>
<body>
<p>Hello World</p>
</body>
</html>
```

可以在`远程主机:80`查看到页面

## 2 安装Brat

下载并解压改名为brat放在Apache文件夹中：/var/www/html/

### 2.1 对brat目录授权

```
sudo chmod 777 -R /var/www/html/brat
```

### 2.2 安装Brat

参考官网安装流程

### 2.3 安装完成后，更改data和work目录的用户组和权限

```
chgrp -R apache data work
chmod -R g+rwx data work
```

可在`远程主机:80\brat`登陆页面

## 3. Brat使用

参见

https://blog.csdn.net/u011440696/article/details/109364035

https://blog.csdn.net/guofei_fly/article/details/104113217

