---
title: "hexo文章编辑模版示范"
date: 2018-07-25T00:00:00+08:00
lastmod: 2018-07-25T00:00:00+08:00
draft: false
tags: ["hexo"]
categories: ["教程"]
comments: false
---



``` bash
eefef
```

`这是灰底字`

### 修改字体颜色/大小/背景色
如果想自定义字体大小以及颜色，可以直接在 Markdown 文档中使用 html 语法

``` bash
<font size=4 > 这里输入文字，自定义字体大小 </font>
<font color="#FF0000"> 这里输入文字，自定义字体颜色</font> 
<span style="background-color: #ff6600;">这里输入文字，自定义字体背景色</span>
<font color="#000000" size=4><span style="background-color: #ADFF2F;">这是综合起来的效果 </span></font>
<font color="#FFFFFF" size=4><span style="background-color: #68228B;">这是综合起来的效果2 </span></font>
```


<!--more-->


其中#FF0000为RGB颜色代码，读者可去[RGB颜色查询对照表网站](/rgb.html)查找自己喜欢的颜色。

若想在RGB颜色值与十六进制颜色码之间相互转化，可查看[该网站](http://link.zhihu.com/?target=http%3A//www.sioe.cn/yingyong/yanse-rgb-16/)。

<font size=4 > 这里输入文字，自定义字体大小 </font>
<font color="#FF0000"> 这里输入文字，自定义字体颜色</font> 
<span style="background-color: #ff6600;">这里输入文字，自定义字体背景色</span>
<font color="#000000" size=4><span style="background-color: #ADFF2F;">这是综合起来的效果 </span></font>
<font color="#FFFFFF" size=4><span style="background-color: #68228B;">这是综合起来的效果2 </span></font>


### 文本居中对齐

``` bash
{% centerquote %}blah blah blah{% endcenterquote %}
```

展示如下

{% centerquote %}blah blah blah{% endcenterquote %}


### 插入代码

``` bash 
``` bash
```

演示效果

``` bash 
bash xxx.sh
```

### 插入表格
``` bash
|函数名|功能|其它|
|----|----|----|
|max|求最大值|其它1|
|min|求最小值|其它2|
```
演示效果

|函数名|功能|其它|
|----|----|----|
|max|求最大值|其它1|
|min|求最小值|其它2|
