---
title: 安装hexo，搭建博客
---



准备
你需要准备好以下软件：

Node.js环境
Git

配置Hexo
安装Hexo
在自己认为合适的地方创建一个文件夹，然后在文件夹空白处按住Shift+鼠标右键，然后点击在此处打开命令行窗口。（同样要记住啦，下文中会使用在当前目录打开命令行来代指上述的操作）

在命令行中输入：

npm install hexo-cli -g

初始化Hexo
接着上面的操作，输入：

hexo init


首次体验Hexo
继续操作，同样是在命令行中，输入：

hexo g

然后输入：

hexo s
然后会提示：

INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.



下一步将我们的Hexo与GitHub关联起来，打开站点的配置文件_config.yml，翻到最后修改为：

deploy: 
type: git
repo: 这里填入你之前在GitHub上创建仓库的完整路径，记得加上 .git
branch: master参考如下：




保存站点配置文件。

其实就是给hexo d 这个命令做相应的配置，让hexo知道你要把blog部署在哪个位置，很显然，我们部署在我们GitHub的仓库里。最后安装Git部署插件，输入命令：

npm install hexo-deployer-git --save


这时，我们分别输入三条命令：



hexo clean 
hexo g 
hexo d
其实第三条的 hexo d 就是部署网站命令，d是deploy的缩写。完成后，打开浏览器，在地址栏输入你的放置个人网站的仓库路径，即 http://xxxx.github.io (知乎排版可能会出现"http://"字样，参考下图) 比如我的xxxx就是我的GitHub用户名：




你就会发现你的博客已经上线了，可以在网络上被访问了。