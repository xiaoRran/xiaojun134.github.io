---
title: 使用Hexo和Github搭建个人博客
tags: Hexo
categories: 技术
cover: >-
  https://api.r10086.com/%E5%9B%BE%E5%8C%85/%E5%8A%A8%E6%BC%AB%E7%BB%BC%E5%90%882/70493472_p0.jpg
abbrlink: 2e566f5
date: 2022-01-25 10:00:30
---

# 使用Hexo和Github搭建个人博客

> 仅为个人经验总结，应该有更好的搭建方式。建议：最好至少要具备一定的前端知识。
>
> 本文结合[青云工作室](https://qystudio.ltd/)以及个人的总结所编写

## 常用指令

`Hexo init`初始化博客
`Hexo n 标题` 创建一篇文章
`Hexo clean` 清理文件
`Hexo g` 生成静态文件
`Hexo d` 部署博客
`hexo server`启动本地服务
`hexo render`渲染文件

## 准备工具

在开始配置之前，需要下载以下几个程序：

```
1、Git

2、node

3、Typora （推荐，用来编辑博客，可用其他编辑器，也可搭建完成后再安装）建议直接到官网下载。

4、一个代码编辑器，推荐VScode，用于修改配置文件时使用。
```

## 开始配置

- 安装Git

windows：到git官网上下载,Download git,下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。

linux：对linux来说实在是太简单了，因为最早的git就是在linux上编写的，只需要一行代码

```
sudo apt-get install git
```

安装好后，用git –version 来查看一下版本

- 安装nodejs

windows：nodejs选择LTS版本就行了。

linux：

```
sudo apt-get install nodejs
sudo apt-get install npm
```

安装完后，打开命令行

```
node -v
npm -v
```

检查一下有没有安装成功

windows在git安装完后，就可以直接使用git bash来敲命令行，不需要使用windows自带的CMD，CMD有点难用。

- 安装hexo

前面git和nodejs安装好后，就可以安装hexo了，先桌面上创建一个文件夹blog然后在这个文件夹下直接右键git bash打开。

输入命令

```
npm install -g hexo-cli
```

用hexo -v查看一下版本

至此Hexo就安装完成。

接下来初始化一下hexo

```
hexo init myblog
```

这个myblog可以自己取什么名字都行，然后

```
cd myblog //进入这个myblog文件夹
npm install
```

新建完成后，指定文件夹目录下有：

node_modules: 依赖包
public：存放生成的页面
scaffolds：生成文章的一些模板
source：用来存放你的文章
themes：主题
_config.yml: 博客的配置文件

```
hexo g
hexo server
```

打开hexo的服务，待出现提示”Hexo is running at http://localhost:4000. Press Ctrl+C to stop”后即代表成功，其他提示则错。

打开浏览器地址栏输入localhost:4000，出现hexo的页面则代表你的blog创建成功，但这一步是本地预览，还未托管成功，别人在网上看不到。如下：

大概长这样：

![](https://cdn.jsdelivr.net/gh/caicheng918/CDN@4.0/102/20200313180932972.png)

注：如第（1）步成功，第（2）步失败，代表4000端口被占用，返回Git CMD先按Ctrl+C退出服务，输入：

```
hexo s -p 3600 #以3600端口启动服务
```

再在浏览器输入localhost:3600，就能进入页面。

使用ctrl+c可以把服务关掉

## 上传到Github Pages

- 首先安装插件：

```
npm install hexo-deployer-git –save
```

- 在自己的Github主页创建一个新的repository。创建的repository的名字必须为 yourname.github.io。注意替换yourname。
- 设置Git的SSH。

回到博客根目录的git bash中，输入

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```

然后创建SSH,一路回车

```
ssh-keygen -t rsa -C "youremail"
```

成功生成后一般会在C盘user文件夹里找到.ssh这个文件夹，里面的id_rsa.pub文件就是SSH密钥。(注意需要打开显示隐藏文件才能看到.ssh)

将这个密钥复制下来，在Github的SSH设置里面填入这个密钥，保存后才能部署成功

- 在本地hexo目录下的config_yml里定位到deploy编辑成如下格式，注意冒号有一个空格，必须严格按照格式填写。

```
deploy:
   type:  git 
   repository:  http://github.com/yourname/yourname.github.io.git
   branch:  master
```

- 需要部署时在博客根目录Git bush使用以下三件套命令：

```
hexo clean #清除缓存

hexo g #生成静态文件

hexo d #推送到远端仓库
```

本地测试时在博客根目录Git bush使用命令：

```
hexo s
```

出现Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.时则可以在浏览器打开http://localhost:4000进行测试。

本地测试时可以改动博客的代码或文件，仅需在浏览器刷新就能看到改动，因此更新博客时最好在本地测试无误后再上传。

## 编写博客并上传

- 如果没安装，可以安装Typora了，这是一个常用的编辑器，具体功能自行探索。

- 安装好后，在blog根目录下打开Git Bush Here，（下文均默认此操作），输入新建文章命令：

```
hexo n "文章名" #生成一篇文章
```



此时，会在source文件夹里的_posts文件夹里生成 “第一篇文章.md”的文件，双击它可调用Typora进行编写。

其中，title是文章标题，time是生成时间，tags是标签（没有tags暂时不管），然后是正文。编写完后即保存。

注：当你编辑一个新文件完成后，必须将此文件保存到/blog/source/_posts/路径下，不然不能上传博客。

在Git Bush Here里面输入：

```
hexo d
```

即可更新到github。

## 使用Vercel部署并绑定域名

用Vercel部署的好处一是快，二是每当push到github时，Vercel会自动同步，省去了手动部署的麻烦，部署完成还会发送邮件通知。

- 前往https://vercel.com/注册，注册时选择绑定github。

- 创建新项目，选择导入github的博客仓库。

- 在域名提供商处将域名解析到Vercel的DNS地址。

- 在setting里选择domain，填入自己的域名，确定。

- 过一会便能通过此域名访问博客。更新时只需要照常hexo d到github即可，Vercel会自动同步部署。

## 更改主题

由于建立博客后的初始主题很难看，所以应当换个主题。不同的主题配置方式都不太相同，需要自己修改主题配置文件，所以需要自己去动手了解。官方主题网站：https://hexo.io/themes/ ， 选择好后跳转到github下载。好看的有butterfly，next等。以Jerry开发的主题butterfly为例：

![](https://cdn.jsdelivr.net/gh/caicheng918/CDN@4.0/102/image-20201129220151788.png)

在GitHub下载ZIP文件后（或者本地clone仓库），解压。

将文件夹复制到/blog/themes下，然后回到blog\根目录下，用记事本打开_config.yml文件，在最后找到这三行代码：

```
Extensions

Plugins: https://hexo.io/plugins/

Themes: https://hexo.io/themes/

theme: butterfly
```

<center><img src="https://cdn.jsdelivr.net/gh/caicheng918/CDN@4.0/102/image-20201129220401269.png"></center>

将上方theme后的代码改为下载主题的 文件夹的文件名，其他不改，注意空格，如改为Next主题：

```
Extensions

Plugins: https://hexo.io/plugins/

Themes: https://hexo.io/themes/

theme: Next
```

完成后点击保存。回到Git Bush Here输入：

```
hexo g	#生成

hexo d

hexo clean	#清理hexo缓存

hexo g --d  #一键部署
```

