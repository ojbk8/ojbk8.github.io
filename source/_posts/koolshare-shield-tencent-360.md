---
title: "Koolshare-openwrt-lede屏蔽360腾讯的方法"
date: 2018-10-12T00:00:00+08:00
lastmod: 2018-10-12T00:00:00+08:00
draft: false
tags: ["Koolshare"]
categories: ["教程"]
comments: false
---

登陆后依次进入`系统`,`进阶设置`,`配置dnsmasq`
或者在`/etc/dnsmasq.conf`添加以下内容

### 屏蔽腾讯的方法
``` bash
address=/qq.com/127.0.0.1
address=/tencent.com/127.0.0.1
address=/qcloud.com/127.0.0.1
address=/gtimg.com/127.0.0.1
address=/gtimg.cn/127.0.0.1
address=/qpic.cn/127.0.0.1
address=/qlogo.cn/127.0.0.1
address=/url.cn/127.0.0.1
```

### 屏蔽360腾讯的方法
``` bash
address=/360.cn/127.0.0.1
address=/360.com/127.0.0.1
address=/so.com/127.0.0.1
address=/i360mall.com/127.0.0.1
address=/360shouji.com/127.0.0.1
address=/360.net/127.0.0.1
address=/360totalsecurity.com/127.0.0.1
```


PS **抓包网址还不够全面，且对UDP流量不起作用**  欢迎留言补充
