---
title: githubpage+hexo搭建个人主页
date: 2017-02-03 17:10:16
tags: [github,hexo,个人主页,个人博客]
---
之前一直想着建立一个属于自己的个人技术博客，由于接触的不多，也不知道什么hexo这些快速建博客的工具，只能选择放弃。

机缘巧合之下看到了一篇有关hexo建blog的文章，恰巧假期闲来无事也就捣鼓了这个博客，过程也不是很复杂，只要有些许的编程水平就可以搭建了。趁着刚刚建起博客时兴奋的余热，顺手写起了这篇教程，也给有建站兴趣的小伙伴们来一个参考吧。本文也尽量用简单的方式来演示建站的详细过程。当然，本博客也会随缘看心情放放一些其他的技术文档，见闻录，照片墙之类，有兴趣的小伙伴也可以关注下本博客。

PS：本人是使用**Windows 10**环境下建立的博客，有空的话我也会尝试用Mac建立博客，届时我也会再发布一篇针对Mac建站的教程（有生之年系列）

# 适合的人群

- 爱折腾的人
- 想写Blog
- 会用Markdown
- 了解Git
- 有一定编程基础

<!--more-->

# 建站需要的准备

- 注册Github及安装Git
- 安装Node.js
- 安装hexo

# 注册Github

首先再点[这里](https://github.com/)注册Github账号，注册流程很简单网上教程也很多这里就不多讲了。

## 创建仓库

![建站](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203174815.jpg)

登录账号后，点击这里建立你的仓库。

![仓库](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203175238.jpg)

这里仓库的名字要与你的github名字一致，`你的名字.github.io`

然后点Create repository即可。

## 下载Git

点[这里](https://git-scm.com/downloads)下载并安装Git Bash。

## 生成SSH密钥

打开刚刚安装的Git Bash，键入

```bash
ssh-keygen -t rsa -C "注册github时的邮箱"
```

然后一路回车就可以了。待密钥生成完毕会在你的用户目录下的`.ssh`里面多出**id_rsa**，**id_rsa.pub**两个文件

其中我的目录是：

```text
C:\Users\Darren（你自己的电脑用户名）\.ssh\
```

用记事本打开**id_rsa.pub**，全选然后复制到[这里](https://github.com/settings/ssh)

![ssh](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203182507.jpg)

在这里点`New SSH key`，Title里面的随便填，把**id_rsa.pub**里面的东西全部复制到key里面，然后点`Add SSH key`即可。

# 安装Node.js

点[这里](https://nodejs.org/en/)下载并安装Node.js，他有通用版和最新版，一般选通用版就可以了。

![Node](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203183222.jpg)

# 安装Hexo

有能力的小伙伴可以通过[官网](https://hexo.io/)和[Hexo官方文档](https://hexo.io/docs/)来查阅安装。

接下来的操作都在git bash里面进行。在里面粘贴

```bash
npm install -g hexo
```

安装完后键入`hexo`出现下面这样就是成功了。

![hexo](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203184801.jpg)

## 创建博客文件夹

找到一个你想存放博客的地方，右键选Git Bash打开输入

```bash
hexo init 你博客的名字
```

![init](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203185340.jpg)

```bash
cd 你博客的名字
```

## 安装依赖包

```bash
npm install
```

![install](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203185535.jpg)

## 本地查看

生成静态页面

```bash
hexo g
```

先安装server

```bash
npm install hexo -server --save
```

启动本地预览

```bash
hexo s
```

此时在地址栏中输入`http://localhost:4000/.`就能看到你自己的博客了，但此时还是本地的。

# Github创建博客

先在github里面打开你刚建的那个仓库，点击setting。

![set](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203200404.jpg)

在下面github page里面点choose a theme

![theme](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203200431.jpg)

记事本或sublime打开博客目录下的`_config.yml`编辑。

只用改下列部分

```bash
title: 博客标题
subtitle: 副标题
description: 描述
author: 作者
language: zh-CN
timezone: Asia/Hong_Kong
```

还有

```bash
url: http://你的用户名.com
```

在最后

```bash
  deploy:
  type: git
  repo: 仓库地址
  branch: master
```

仓库地址可以在这里找到，复制粘贴过去。

![地址](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203200256.jpg)

# Hexo部署

```bash
hexo g
hexo d
```

这样就部署完成了，网页里打开`你的用户名.github.io`就是你的个人主页了。

# 常用Hexo命令

常用命令

```bash
hexo help #查看帮助
hexo init #初始化一个目录
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成网页，可以在 public 目录查看整个网站的文件
hexo server #本地预览，'Ctrl+C'关闭
hexo deploy #部署.deploy目录
hexo clean #清除缓存，**强烈建议每次执行命令前先清理缓存，每次部署前先删除 .deploy 文件夹**
```

简写

```bash
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

# 新建文章

```bash
hexo new 标题
```

这时候在你的博客目录下的`source`里面的`_post`文件夹内就会有你的`标题.md`博客了。

![位置](http://okqqc0v2n.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170203205303.jpg)

打开用markdown的语法进行编辑即可。编辑完成后

```bash
hexo g        //生成静态页面
hexo s        //本地预览
hexo d        //上传部署
```

至此，你的博客就算是建立完成了。赶紧开始着手折腾属于你自己的小窝吧。