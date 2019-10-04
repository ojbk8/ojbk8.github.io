---
title: test
categories: 未分类
date: 2017-10-03 15:03:05
tags:
comments: false
---


这是{% label default@Default%}

这是{% label primary@Primary%} 8899 

这是{% label success@Success%} test 

这是{% label info@Info%} 

这是{% label warning@Warning%} 

这是{% label danger@Danger%} 

<div>{% button /,首页,home fa-fw,首页%}</div>

这是有删除线的~~{% label danger@Danger%}~~


{% note %}默认 用来替代引用标签还行{%endnote%} {% note default %} 
默认 有图标的引用标签吧{%endnote%} {% note info 
%}可以补充一些信息{%endnote%} {% note 
primary%}Primary，不知道怎么用{%endnote%} {% note 
warning%}Warning，写一些警告信息{%endnote%} {% note danger 
%}Danger,就是这样很危险吧{% endnote%} {% note 
success%}Success，成功是什么鬼啊{%endnote%}
{% note success no-icon%}没有图标的样子{%endnote%}



{% note default %} default 提示块标签 {% endnote %} {% note primary 
%} primary 提示块标签 {% endnote %} {% note success %} success 
提示块标签 {% endnote %} {% note info %} info 提示块标签 {% endnote 
%} {% note warning %} warning 提示块标签 {% endnote %} {% note danger 
%} danger 提示块标签
{% endnote %}



{% cq %}世间所有的相遇，都是久别重逢{% endcq %}



{% tabs tab,1 %} 名字为tab，默认在第1个选项卡，如果是-1则隐藏 <!-- 
tab --> **选项卡 1** <!-- endtab --> <!-- tab --> **选项卡 2** <!-- 
endtab --> <!-- tab A --> **选项卡 3** 名字为A <!-- endtab -->
{% endtabs %}
