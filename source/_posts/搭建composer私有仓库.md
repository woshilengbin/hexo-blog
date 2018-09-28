---
title: 搭建composer私有仓库
date: 2018-09-28 11:44:55
tags:
---

现在网上到处充斥这各种搭建私有仓库文档，但对于新手来说，总是有那么多的坑。这里我把我的摸索历程写出来给大家参考，希望大家能少踩一些坑。
一，  安装 Composer
Composer 的安装已经有很完善的教程，这里我就不重复造轮子了。安装 Composer

安装好之后就能直接在命令行使用 composer命令，如果不是的话，请检查安装过程，后续步骤会直接使用 composer 来做演示。

二， Composer 配置国内镜像
这是使用全局方式：

composer config -g repo.packagist composer https://packagist.phpcomposer.com

为啥要配置国内镜像。原因大家都懂得。。。。

三，使用Git搭建代码仓库
自建Git仓库，github仓库, SVN 仓库 都可以用来做为我们的私有仓库。

这里我们选择 自建Git仓库。因为相对于 自建SVN仓库，自建Git仓库配合起来会更方便一些。

Github 与自建Git仓库功能大概是一样的，但既然需要私有化，就不希望代码让别人看到，所以我们这里选择自建Git仓库。当然GitHub私有项目也是可以的，但是需要收费，我们这里就不考虑了，有需要使用的可以尝试一下。

第一步： 安装Git
$ sudo apt-get install git

第二步：创建一个git用户，用来运行git服务：
$ sudo adduser git

第三步：客户端机器生成公钥
$ ssh-keygen

上面的命令可以直接在Linux上执行，但是在Windows上可能找不到命令。需要安装 Git客户端才能使用。点击下载 . 

查看公钥路径：

[Linux]: /Users/you/.ssh/id_rsa.pub 

[Windows]: C:\Users\[username]\.ssh\id_rsa.pub

第四步：创建证书登录
收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。如果文件或路径不存在，自行创建即可。

第五步：初始化Git仓库
先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

$ sudo git init --bare sample.git

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

$ sudo chown -R git:git sample.git

第六步：禁用shell登录
出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

$ git:x:1001:1001:,,,:/home/git:/bin/bash

改为：

$ git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

第七步 : 克隆远程仓库
现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：

$ git clone git@server:/srv/sample.git

第八步：配置Git裸仓库HTTP克隆
$ cd /srv/sample.git

$ mv hooks/post-update.sample hooks/post-update

$ chmod a+x hooks/post-update

之后就可以使用 HTTP 方式克隆：

$ git clone http://example.com/sample.git

注意：上面步骤执行完毕之后，可能会不成功。需要在仓库根目录下执行：

$ git update-server-info

第九步：创建包代码并提交到Git仓库
$ composer init

按照提示一步一步输入，类型选择：library

配置完成后如下所示：


$ git add .
$ git commit -m "init" 

$ git push

$ git tag -a v0.1.2 -m 'add 0.1.2 version....'

$ git push origin v0.1.2

提交代码，添加 tag 并推送到服务器。tag 就是包的版本号

四：使用 Satis 搭建私有仓库
1. 建立项目
使用 Composer 自带的建项目功能，这个相当于git clone+composer install+ 运行 post-install 脚本。

$ composer create-project composer/satis my-satis --stability=dev --keep-vcs

2. 建立配置文件
在/path/to/my-satis目录下建立satis.json文件


name: 项目名称

homepage : 私有包主页，后续会用到。

repositories : 资源包来源，里面配置私有仓库url，就是上前面创建的私有Git仓库地址。

require : 配置 git仓库中存在的包。

3. 生成仓库列表
执行：

php bin/satis build satis.json ./web

就可以在path/to/my-satis/web/里生成仓库列表了。

可能会报协议错误，默认是禁止 http 方式获取代码。需要单独配置开启。




执行完毕后。会在项目根目录生成 web 目录。

4. 配置 webServer
将 web 目录配置 webServer 访问。虚拟域名就是之前我们配置的 homepage : packagist.example.com


5. 在其它项目中使用私有源
只需要在项目的 composer.json 文件的根上添加

这样便可以正常安装新添加好的包
还要记得配置：


否则项目安装的时候还是提示项目不是HTTPS。

五、安装测试
$ compser init

初始化项目，类型选择：project 

composer.json 示例：


安装私有包

compser require luopingping/test:*

如果不出意外，操作完成之后就能将私有Git仓库里的代码都下载下来了。

至此安装完成。