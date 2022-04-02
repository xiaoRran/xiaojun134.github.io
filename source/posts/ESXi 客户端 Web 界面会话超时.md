---
date: 2021-10-26T13:42:03.261Z
updated: null
title: ESXi 客户端 Web 界面会话超时
slug: '2'
categories: 技术
type: post
permalink: posts/2
---


主机客户端 Web 界面会话**每 15 分钟自动超时一次**，然后您必须再次重新登录 ESXi 主机客户端 Web 界面。
要避免这种繁琐的情况，可以 增加会话超时，从而更改 ESXi 主机客户端 Web 界面中的高级配置参数。

导航到主机 > 管理 > 系统 > 高级设置，然后向右下滚搜索 `UserVars.HostClientSessionTimeout`

将 “新值” 字段设置为 0，然后单击 “ 保存” 以使网页无限期打开。

![截图](https://pic.imgdb.cn/item/6128cf8744eaada73999b0f2.png)