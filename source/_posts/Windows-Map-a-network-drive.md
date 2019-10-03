---
title: "Windows中将OneDrive映射为本地磁盘"
date: 2017-04-20T00:00:00+08:00
lastmod: 2017-04-20T00:00:00+08:00
draft: false
tags: ["OneDrive", "Windows"]
categories: ["教程"]

---

将 OneDrive 映射为本地网络驱动器
登录到 [OneDrive](https://onedrive.live.com/) 在浏览器地址栏即可获取账户 ID。类似：

``` bash
https://onedrive.live.com/?id=root&cid=xxxxxxxxxxx
```

其中xxxxxxxxxxx为Onedrive账户ID

![](/images/onedriveid.png)

打开`此电脑` `计算机` `映射网络驱动器`

![](/images/windows-map-a-network-drive.png)

`映射网络驱动器`在`文件夹`输入

``` bash
https://d.docs.live.net/xxxxxxxxxxx
```

按提示输入账号密码即能完成映射网络驱动器

![](/images/windows-map-a-network-drive1.png)




