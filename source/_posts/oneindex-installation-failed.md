---
title: "Oneindex程序安装失败解决办法"
date: 2017-10-04 15:03:05
draft: false
tags: ["oneindex"]
categories: ["教程"]
comments: false
---

项目地址： https://github.com/donwa/oneindex

功能：

* 不占用服务器空间，不走服务器流量；
* 直接列出 OneDrive 目录，文件直链下载。

源码安装运行需求：

1. PHP空间，PHP 5.6+ 需打开curl支持
2. OneDrive 账号 (个人、企业版或教育版/工作或学校帐户)
3. OneIndex 程序

# Oneindex程序安装失败解决办法

打开这个网址https://apps.dev.microsoft.com/#/appList

里面会列出oneindex的应用，点击进去后，会看到你的应用ID（client_ID）。

找到【应用程序机密】，点击【生成新密码】，就会生成一个新的client_secret，然后重新安装时候填这个新的client_secret就可以了
