---
title: 阿里云使用宝塔面板部署 Hexo 博客
cover: 'https://pic.imgdb.cn/item/6103cf7d5132923bf84c99a3.jpg'
abbrlink: 14325c2f
date: 2021-11-05 08:22:28
---


It has been 541 days since the last update, the content of the article may be outdated.

宝塔系统依托于 `Centos` 开发，所以我们服务器系统选择 Centos7+

## 安装宝塔

```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```

安装完成后，登录宝塔，首次登录会提示我们安装环境，我们勾选 `Nginx` 即可。

之后，我们在 `home` 文件夹下新建 `git`，`hexo` 这两个文件夹

[![img](https://bu.dusays.com/2021/09/28/edc98ab86ee44.png)](https://bu.dusays.com/2021/09/28/edc98ab86ee44.png)



随后，我们登录我们的服务器，一次执行

```
cd ..
cd home
cd git
git init --bare blog.git
```

接着，在目录 `/home/git/blog.git/hooks` 下新建文件 `post-receive`，写入一下代码

```
git --work-tree=/home/hexo --git-dir=/home/git/blog.git checkout -f
```

给 `post-receive` 权限

```
chmod +x /home/git/blog.git/hooks/post-receive
```

## 配置 Nginx

宝塔面板默认的 `nginx` 配置文件在根目录 ->www->serve->nginx->conf 下，找到 `nginx.conf`，编辑它

重启 `nginx` 服务

```
service nginx restart
```

## 本地 Hexo 配置

修改`_config.yml` 中的配置

```
deploy:
  type: git
  repo: user@ip:/home/git/blog
```

这个时候配置差不多就好了，`hexo generate --deplay` 部署后就可以访问博客了

接下来在宝塔面板里添加网站

[![img](https://bu.dusays.com/2021/09/28/f0e14ae0e58ea.png)](https://bu.dusays.com/2021/09/28/f0e14ae0e58ea.png)



之后在阿里云将域名解析到服务器 ip 即可



## SSL 证书

在阿里云中申请个证书，将内容填入其中即可

[![img](https://bu.dusays.com/2021/09/28/d24d1d90c359b.png)](https://bu.dusays.com/2021/09/28/d24d1d90c359b.png)