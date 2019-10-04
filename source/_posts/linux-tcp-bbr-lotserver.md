---
title: "TCP加速bbr魔改plus锐速lotserver一键安装包"
date: 2018-10-03T00:00:00+08:00
lastmod: 2018-10-03T00:00:00+08:00
draft: false
tags: ["bbr","lotserver"]
categories: ["教程"]
comments: false
---


整出了一个集合BBR、BBR魔改、BBR plus、锐速四合一的脚本
本脚本支持KVM架构的VPS，不支持OpenVZ。


> 项目地址：(https://github.com/cx9208/Linux-NetSpeed)

# 安装

``` bash
wget https://github.com/ojbk8/Linux-NetSpeed/raw/master/tcp.sh
chmod +x tcp.sh
./tcp.sh
```

先换成对应加速
出现弹窗按`TAB键`切换选择`No`回车删除原内核

![](/images/tcp-bbrplus-lotserver.png)

重启后

``` bash
./tcp.sh
```

开启加速


 
