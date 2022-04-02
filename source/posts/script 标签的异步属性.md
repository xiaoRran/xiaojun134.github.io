---
date: 2021-10-26T16:23:03.574Z
updated: null
title: script 标签的异步属性
slug: '65'
categories: 技术
type: post
permalink: posts/65
---


通常我为了防止 JS 阻塞页面加载，会把 script 标签放在 body 标签末尾。
## 同步
同步模式：又称阻塞模式，会阻止浏览器的后续执行，停止后续解析，只有当前加载完成才能进行下一步操作。

## 异步
异步模式：async 属性、defer 属性。

    <script async></script>
    <script defer></script>
    <!-- OR -->
    <script async="true"></script>
    <script defer="true"></script>
通过 createElement 创建的 script 标签其属性 async 默认为 true

给创建的 script 标签添加 defer 属性：

    var elm = document.createElement("script");
    elm.defer = true;

defer 与 async 的区别：defer 要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行；async 一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。
defer 是 “渲染完再执行”，async 是 “下载完就执行”。
另外，如果有多个 defer 脚本，会按照它们在页面出现的顺序加载，并且多个 async 脚本是不能保证加载顺序的。