---
title: "Ubuntu18.04关闭防火墙开放所有端口"
date: 2019-06-02T03:50:18-04:00
draft: false
tags: [""]
Keywords: [""]
categories: ["未分类"]
---

``` bash
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -F
iptables-save > /etc/iptables/rules.v4
```
