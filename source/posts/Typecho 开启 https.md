---
date: 2021-10-26T16:17:35.172Z
updated: 2021-10-26T16:19:03.376Z
title: Typecho 开启 https
slug: '11'
categories: 技术
type: post
permalink: posts/11
---


# 前景
今天给我的日志站点加了 ssl 后，发现站点无法提交信息。
 然后我就想了一下也没啥问题出现啊，然而我遗忘了一步。

# 解决方法

1. 先部署好 ssl 证书，开启强制跳 https
2. 在 ty 根目录的找到 config.inc.php
3. 在 config.inc.php 里加入一下代码，保存即可

需要添加的代码：

```
/** 开启HTTPS **/
define('__TYPECHO_SECURE__',true);
```

# 其他
附上一个免费申请 ssl 证书的站点，支持泛解析
[button color="info" icon="" url="https://www.sslforfree.com/" type="round"]猛戳直达[/button]