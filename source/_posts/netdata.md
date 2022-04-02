---
title: 树莓派安装帅气的netdata监控工具
tags: 树莓派
categories: 技术
cover: 'https://ae01.alicdn.com/kf/He59e55d9ee7549c09b1e2e87a2151e13r.jpg'
abbrlink: 2acfd067
date: 2021-11-29 15:00:30
---

## 树莓派安装netdata

NetData 是一个用于系统和应用的分布式实时性能和健康监控工具。 界面ui也比较炫酷。

我这里就安装一下`netdata.`

#### 安装依赖

`
sudo apt-get install zlib1g-dev uuid-dev libuv1-dev liblz4-dev libjudy-dev libssl-dev libmnl-dev gcc make git autoconf autoconf-archive autogen automake pkg-config curl
`

#### 安装汉化版本

* 切换到`home cd ~`

* 使用cdns的git服务器，没有科学上网的手段的小伙伴大可放心。

* `git clone https://codechina.csdn.net/mirrors/Fhaohaizi/netdata.git`

* `cd netdata`

* 给权限 `chmod +x netdata-installer.sh chmod +x netdata-installer-zh.sh`

* 安装 `sudo ./netdata-installer.sh`

**启动和关闭**
```
# 停止
systemctl stop netdata
# 启动
systemctl start netdata
# 重启
 systemctl restart netdata
# 开机启动
 systemctl enable netdata
```

**开放19999 端口。**

#### 配置内网穿透，设置账号密码授权

内网穿透 继续使用自己搭建的`frp` ，不了解的小伙伴可以翻我前面关于`frp`的文章。这里重点说明如何配置密码访问。

在服务器上配置nginx

内网穿透 继续使用自己搭建的`frp` ，不了解的小伙伴可以翻我前面关于`frp`的文章。这里重点说明如何配置密码访问。

在服务器上配置nginx

* 创建存密码的目录 `sudo mkdir /etc/nginx/passwd`

* 创建存密码的文件 `sudo touch /etc/nginx/passwd/htpasswd`

* 创建 用户叫 `netdata` 的密码文件 `printf "netdata:$(openssl passwd -apr1)" > /etc/nginx/passwd/htpasswd` 输入此命令后 输入两边密码。

* 新建nginx配置文件 , `sudo vi /etc/nginx/conf.d/netdata.conf`

```
server {
    listen 80;
    # 域名
    server_name netdata.yingwiki.top;
 
   auth_basic "netdata Login";
   #  上一步生成的密码文件路径
   auth_basic_user_file /etc/nginx/passwd/htpasswd;
 
    location / {
        # 你的frp定义http端口和域名
	proxy_pass  http://netdata.yingwiki.top:8080;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
        proxy_pass_request_headers on;
        proxy_set_header Connection "keep-alive";
        proxy_store off;
    }
}
```

* `sudo nginx -t` 检查nginx配置

* `sudo nginx -s reload `没有问题重载配置

## 完成

现在就可以输入` http://`域名 访问啦 ，我在这里就不配置 https的，需要的小伙伴自己改一下就好了。

![alt](https://halo-yxq-1302651434.cos.ap-beijing.myqcloud.com/halo%E5%8D%9A%E5%AE%A2/netdata/image-20210208021126747.png)