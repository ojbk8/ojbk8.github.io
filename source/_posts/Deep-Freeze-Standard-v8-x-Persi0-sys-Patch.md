---
title: 冰点还原Deep Freeze Standard8.x安装激活
categories: 未分类
comments: false
date: 2018-07-05 11:51:07
tags:
---

冰点还原Deep Freeze Standard8.x是一款WINDOWS平台保护免疫电脑病毒的保护软件。

### 下载

[下载冰点还原Deep-Freeze-Standard.v8.5.3](/uploads/Deep.Freeze.Standard.v8.5.3.rar) 并解压


### 安装

#### 获取安装序列号

![](Deep-Freeze-Standard.v8.x_SN.png)

#### 安装冰点还原

打开Faronics_DFS\Faronics_DFS\DFStd.exe安装

### 激活

安装完成后先按住键盘Shift键双击冰点还原的图标，选择启动后解冻，重启电脑，退出守护状态。

![](Deep-Freeze-Standard-exit.png)

使用PCHunter解锁C盘下的Persi0.sys文件。或者使用360之类的解锁这个文件锁定。

![](Deep-Freeze-Standard1.png)

![](Deep-Freeze-Standard2.png)

运行`dfs.v8.53.020.5458-patch.exe`对C盘下的`Persi0.sys`文件进行补丁。

![](Deep-Freeze-Standard-Patch.png)

重启电脑，此时冰点还原Deep-Freeze-Standard应该已经激活成功了

按住键盘Shift键双击冰点还原的图标，查看激活状态并开启它

![](Deep-Freeze-Standard-status.png)

温情提醒：如果用PCHunter无法解锁进程可以在WINPE来补丁Persi0.sys文件！
