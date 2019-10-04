---
title: "hexo同步布署在github-coding并实现国内外自动分流"
date: 2018-03-29T00:00:00+08:00
lastmod: 2018-03-29T00:00:00+08:00
draft: false
tags: ["hexo"]
categories: ["教程"]
comments: false
---


建议单独托管在github上，因为国内大环境背景下，你懂的啦！

![](/images/coding-domain-Filing.png)

### 域名解析

登陆[DNSPOD控制台](https://www.dnspod.cn)  在域名添加2条记录值

|记录类型|线路类型|记录值|
|----|----|----|
|CNAME|国外|username.github.io|
|CNAME|国内|username.coding.me|

<!--more-->

### 配置文件
在`_config.yml`站点配置文件中添加配置：

``` bash
# Deployment
# Docs: https://hexo.io/docs/deployment.html
deploy:
- type: git
  repo: git@github.com:username/username.github.io.git
  branch: master
- type: git
  repo: git@git.coding.net:username/username.coding.me.git
  branch: master
```

然后执行`hexo deploy`完成。
