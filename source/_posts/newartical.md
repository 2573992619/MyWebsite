---
title: apache2部署网站
date: 2023-11-17 20:34:23
tags: 网站搭建
---
# <center>使用Apache2搭建网站</center>
由于本站的建设采用了Apache2，所以本文主要用于记录在本站开发时的大致流程和学习内容。

对于遇到的文章之外的问题，可以参阅[官方文档](https://httpd.apache.org/docs/2.4/zh-cn/)
## 一、Apache2简介
Apache HTTP服务器（通常称为Apache）是一个开源的、跨平台的Web服务器软件，它是目前世界上最流行的Web服务器之一。以下是Apache的一些关键特点和简要介绍：

**开源性质**： Apache是一个开源项目，这意味着其源代码是公开可用的，任何人都可以查看、修改和分发。

**跨平台性**： Apache可以在多种操作系统上运行，包括Unix、Linux、Windows、Mac OS等。这使得它成为一个灵活的解决方案，可以适应不同的服务器环境。

**模块化架构**： Apache采用模块化的架构，允许用户根据需要选择性地加载不同的模块。这使得它非常灵活，并能够支持各种功能和扩展。

**虚拟主机支持**： Apache支持虚拟主机，这使得在同一台物理服务器上托管多个域名成为可能。每个虚拟主机可以有自己独立的配置和网站内容。

**安全性**： Apache提供了许多安全性功能，包括SSL/TLS支持、访问控制、身份验证等，使得用户能够保护其Web服务器和应用程序免受恶意攻击。

**高度可定制性**： 由于其模块化的特性，Apache可以根据用户的具体需求进行定制。用户可以选择加载所需的模块，以便满足其特定的要求。

**性能优化**： Apache通过使用多进程（或多线程）和其他性能优化技术，以提供快速、高效的Web服务器性能。

**活跃的社区支持**： 由于其开源性质，Apache拥有一个庞大的用户和开发者社区。这个社区不断地为Apache引入新的功能、修复bug，并提供支持和文档。

总体而言，Apache是一个强大而灵活的Web服务器，适用于各种规模的项目和需求。
## 二、Apache2安装

### 1.安装

我使用的是Ubuntu，在安装Apache2的时候只需要在命令行中输入：
```bash
    sudo apt update
    sudo apt install apache2
```
上述命令分别用于更新软件包索引和安装apache2。

### 2.验证Apache的状态
在安装完成后，可以通过以下命令验证apache2的状态：
```bash
    systemctl status appache2
```
出现以下结果，表示服务已经启动。

![运行效果](http://123.56.141.254/img/apache_status.png)


### 3.可能遇到的问题
如果显示服务没能启动，可能遇到的原因是apache默认监听的80端口被别的进程占用，可以通过以下指令检查端口的占用情况：
```shell
    #sudo lsof -i : 端口号
    sudo lsof -i : 80
```

如果希望结束某进程，可以使用下面两种方式其中之一：
```shell
    kill PID
    pkill 进程名
```
## 三、Apache2配置简要说明
安装成功之后，我们来看看怎么对apache2进行配置来上线我们的网站.
### 1.默认配置
如果不对配置进行修改，直接访问我们的域名或者IP，会得到以下内容：
![运行效果](http://123.56.141.254/img/apache_default_html.png)
这是Apache2的默认页面，同时也表示服务已经正常启动。

Apache2的配置内容存放在地址 /etc/apache2 之下，上面显示的页面存放在地址/var/www/html中，这个地址也是Apache2的默认的文档根地址，我们对服务器网站的寻址会被默认映射到这个地址下。

例如，我们对 http://www.example.com/fish/guppies.html 这个页面发请求，实际得到的是服务器中 /var/www/html/fish/guppies.html 的内容。 前面的http://www.example.com 就被映射为了/var/www/html。

如果不想要更改默认配置，也可以直接在/var/www/html下直接建立自己的网站。
### 2.配置Apache2
我们回到apache2的配置文件所在的位置/etc/apache2,里面包含以下内容：
![运行效果](http://123.56.141.254/img/etc_apache2.png)

如果你想在其他位置建站的话，需要修改其中的配置，我以我建站为例来说明：

我是使用用户名为git的用户进行网站建设，我的网站存放在/www/hexo中，我的远程仓库存放在/www/repo中，这里也简要说明一下建仓过程：
#### 2.1 创建相关文件夹
```shell
    mkdir /www/repo #仓库文件夹
    mkdir /www/hexo #网站
```

#### 2.2 权限更改

```shell
    #仓库权限
    chown -R git:git /www/repo
    chmod -R 755 /www/repo
    #网站权限
    chown -R git:git /www/hexo
    chmod -R 755 /www/hexo
```
这里的权限更改是将这两个目录及其子目录的所有者更改为用户git以及其所属的用户组
- -R：表示递归更改目录及子目录
- git:git 前一个表示用户，后一个表示与之相关联的用户组 
- 755：权限的数字码。 1. 所有者（owner）具有读（r）、写（w）、执行（x）权限。 2.所属组（group）具有读（r）和执行（x）权限。 3.其他用户具有读（r）和执行（x）权限。只有用户git拥有更改权

#### 2.3仓库初始化
```shell
    cd /www/repo
    git init --bare hexo.git #确保之前已经安装了git
```
#### 2.4 建立Git钩子来自动部署
我希望我的仓库在收到对网站的更新后，能够自动的部署到/www/hexo中，这需要建立一个钩子脚本文件。
```shell
    vim /www/repo/hexo.git/hooks/post-receive #打开post-recieve脚本文件
```

在里面编辑如下内容：
```shell
    #!/bin/bash
    git --work-tree=/www/hexo --git-dir=/www/repo/hexo.git checkout -f #这行命令的目的是在每次推送后将最新版本的代码检出到 /www/hexo 目录。
```
- #!/bin/bash : 指定解释器为bash
- --work-tree=/www/hexo : 指定 Git 工作树的目录为 /www/hexo。
- --git-dir=/www/repo/hexo.git: 指定 Git 仓库的目录为 /www/repo/hexo.git
- checkout -f: 从仓库中检出最新的版本到工作树，并使用 -f 参数强制覆盖工作树中的现有文件。

最后再次修改权限：
```shell
    chown -R git:git /www/repo/hexo.git/hooks/post-receive #确保 post-receive 脚本的所有者是 git 用户和 git 用户组。
    chmod +x /www/repo/hexo.git/hooks/post-receive #添加执行权限 (+x) 给 post-receive 脚本，使其可以被执行。
```
仓库建设到此结束。


#### 2.5修改Apache2中默认的文档根目录
言归正传，我希望在/www/hexo位置建站，需要更改Apache2的配置文件，将我的域名映射到我的网站处。

在Apache2中，这个配置在/etc/apache2/sites-available下的000-default.conf文件中，我们打开这个文件，修改其中的DocumentRoot如下所示(换成我的网站地址)：
![运行效果](http://123.56.141.254/img/000.png)

#### 2.6修改Apache2对我们的网站目录的访问权限
在/etc/apache2目录下的apache2.conf配置文件中会看到以下内容：
![运行效果](http://123.56.141.254/img/document.png)
这里用于控制对特定目录的访问权限。
我们可以向其中添加如下内容：
```shell
<Directory /www/hexo>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```
这样Apache2就可以访问我们的网站目录了。
#### 2.7 重新加载Apache2配置
以上配置修改完毕之后，运行以下命令来重载Apache2
```shell
    sudo systemctl reload apache2
```
到此，基本配置更改完毕，可以尝试在浏览器中访问自己的网站来验证。
## 四、Apache2的基本用法
在搭建网站的过程中，用到了一些apache2的常用指令，在这里进行一下汇总：
```shell
sudo service apache2 start    #启动 Apache2 服务
sudo service apache2 stop     #停止 Apache2 服务
sudo service apache2 restart  #重启 Apache2 服务
sudo service apache2 reload   #重新加载配置文件（不中断正在进行的连接）
sudo service apache2 status   #查看 Apache2 服务状态
```