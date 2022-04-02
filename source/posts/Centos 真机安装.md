---
date: 2021-10-26T17:04:21.866Z
updated: 2021-10-26T17:12:31.012Z
title: Centos 真机安装
slug: '57'
categories: 技术
type: post
permalink: posts/57
---


## 材料准备
[阿里云 centos 镜像列表](https://mirrors.aliyun.com/centos-vault/?spm=a2c6h.13651104.0.0.1e5712b2mZWPwz)

启动引导制作工具，下载 UltraISO
## 制作流程
1 打开 UltraISO
![alt](https://cdn.tandongtao.com/usr/uploads/2021/06/1625016471-image.png?imageView2/2/w/1920/q/75/format/webp)
2 打开下好的镜像

文件 -> 打开 -> 选择下好的镜像
![alt](https://cdn.tandongtao.com/usr/uploads/2021/06/1625016574-image.png?imageView2/2/w/1920/q/75/format/webp)
3 写入 U 盘

点击启动 -> 写入硬盘映像 -> 格式化
![alt](https://cdn.tandongtao.com/usr/uploads/2021/06/1625016815-image.png?imageView2/2/w/1920/q/75/format/webp)
![alt](https://cdn.tandongtao.com/usr/uploads/2021/06/1625016862-image.png?imageView2/2/w/1920/q/75/format/webp)
## 装机流程
使用 [UEFI] 方式从 U 盘启动后，会进入到这个界面。?
![alt](https://st.blackyau.net/blog/10/1.png)
按 e 编辑一下 (如果是使用 Legacy BIOS 启动，则按 tab 键)，将第一行的 **linuxefi /images/pxeboot/vmlinuz inst.stage2=hd:.......** ?
![alt](https://st.blackyau.net/blog/10/2.png)
改为下面的 **linuxefi /images/pxeboot/vmlinuz initrd=initrd.img linux dd quiet** 这里是为了确定启动盘的 设备名 便于后续的安装。?
![alt](https://st.blackyau.net/blog/10/3.png)
按下 **Ctrl-x** 后，等待片刻你就会在这里停下来。最好在这里拍个照记录一下，自己通过 **LABEL** 和 **TYPE** 记住 CentOS 启动盘的 设备名 。（下图是 sda4 ）?
![alt](https://st.blackyau.net/blog/10/4.png)
按电源键重启计算机，还是从 U 盘启动。还是按 e 编辑一下，不过这次要把第一行修改为 **linuxefi /images/pxeboot/vmlinuz inst.stage2=hd:/dev/设备名 quiet**。（下图是 sda4 ）然后按下 **Ctrl+x** 进入安装向导。
![alt](https://st.blackyau.net/blog/10/5.png)