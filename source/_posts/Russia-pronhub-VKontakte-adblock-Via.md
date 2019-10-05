---
title: 俄罗斯IP上P站添加规则跳过VK免登陆验证安卓Via浏览器AdBlock
categories: 未分类
comments: false
date: 2019-10-05 10:26:17
tags:
---

毛子机器上PH会弹出一个18+的提示，让登陆VK之类的毛子社交平台验证。

只需要在浏览器的去广告插件中加入两个自定义规则就可以了。不需要登陆VKontakte验证就可以玩PH。

PC端浏览器比如说chrome的扩展程序[AdBlock](https://chrome.google.com/webstore/search/abp?hl=zh-CN)等等 好像规则都是通用的。

``` bash
cn.pornhub.com###age-verification-wrapper
cn.pornhub.com###age-verification-container
```

![](/images/AdBlock-VK-pornhub.png)

安卓手机端可以用[Via浏览器](http://viayoo.com/zh-cn/)自带去广告功能，支持自定义规则。

依次打开`via浏览器`中的`设置` `通用`  `广告标记`  `+` 添加

`地址`加入网址

``` bash
cn.pornhub.com
```

`标签`加入规则

``` bash
#age-verification-wrapper,#age-verification-container
```

转载自https://www.hostloc.com/thread-581829-1-1.html

