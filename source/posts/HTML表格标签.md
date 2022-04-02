---
date: 2021-11-10T16:01:17.580Z
updated: 2021-11-10T16:05:58.415Z
title: HTML表格标签
slug: '009'
categories: 前端之旅
type: post
permalink: posts/009
---


####  表格标签结构

```
<table></table>  表格标签  （表格样式在“table”标签内设置）
<tr></tr>  行标签
<th></th>  表头单元格标签
<td></td> 单元格标签
（单元格里面可以放任何元素，文字链接图片等都可以）
```

#### 表格结构标签

`<thead> `标签 表格的头部区域

`<tbody>`标签 表格的主体区域

**实际用法**

```
 <table align="center" width="500" height="249" border="1" cellspacing="0"> 
       <thead> <!--表格头部标签-->
        <tr>
            <th>排名</th> 
            <th>关键词</th>
            <th>趋势</th>
            <th>今日搜索</th>
            <th>最近七日</th> 
            <th> 相关链接</th> 
        </tr>
    </thead>
    <tbody> <!--表格主体标签-->
        <tr> 
            <td>1</td> 
            <td>鬼吹灯</td> 
            <td></td>
            <td>345</td> 
            <td>123</td>
            <td><a href="https://mxwl520.cn">贴吧</a> <a href="https://mxwl520.cn">图片</a> <a href="https://mxwl520.cn">百科</a></td>
        </tr>

        <tr> 
            <td>2</td> 
            <td>盗墓笔记</td> 
            <td></td>
            <td>124</td> 
            <td>675432</td>
            <td><a href="https://mxwl520.cn">贴吧</a> <a href="https://mxwl520.cn">图片</a> <a href="https://mxwl520.cn">百科</a></td>
        </tr>
    </tbody>
```

#### 表格属性标签

 border  边框标签

cellspacing 单元格与单元格之间的距离

roespan="合并单元格的个数" 跨行合并

colspan="合并单元格的个数" 跨列合并

写法`<td colspan="2"></td>`

| 属性名      | 属性值              | 描述                                             |
| ----------- | ------------------- | ------------------------------------------------ |
| align       | left、center、right | 规定表格相对周围元素的对齐方式borde              |
| border      | 1或者“”             | 规定表格单元是否拥有边框，默认为“”，表示没有边框 |
| cellpadding | 像素值              | 规定单元边沿与其内容之间的空白，默认为1像素      |
| cellspacing | 像素值              | 规定单元格之间的空白，默认2像素                  |
| width       | 像素值或百分比      | 规定表格的宽度                                   |