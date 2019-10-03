---
title: "SSH远程会话管理工具screen使用教程"
date: 2016-06-22T02:48:18-04:00
publishdate: 2016-06-22
lastmod: 2016-06-22
draft: false
tags: []

---


在刚接触Linux时最怕的就是SSH远程登录Linux VPS编译安装程序时（比如安装lnmp）网络突然断开，或者其他情况导致不得不与远程SSH服务器链接断开，远程执行的命令也被迫停止，只能重新连接，重新运行。


一、screen命令是什么？

Screen是一个可以在多个进程之间多路复用一个物理终端的全屏窗口管理器。Screen中有会话的概念，用户可以在一个screen会话中创建多个screen窗口，在每一个screen窗口中就像操作一个真实的telnet/SSH连接窗口那样。

二、如何安装screen命令？

除部分精简的系统或者定制的系统大部分都安装了screen命令，如果没有安装，
CentOS系统可以执行：

``` bash
yum install screen
```

Debian/Ubuntu系统执行：

``` bash
apt-get install screen
```

三、screen命令使用方法？

1、常用的使用方法

用来解决文章开始我们遇到的问题，比如在安装lnmp时。

1.1 创建screen会话

可以先执行：screen -S lnmp ，screen就会创建一个名字为lnmp的会话。 VPS侦探 https://www.vpser.net/

1.2 暂时离开，保留screen会话中的任务或程序

当需要临时离开时（会话中的程序不会关闭，仍在运行）可以用快捷键Ctrl+a d(即按住Ctrl，依次再按a,d)


1.3 恢复screen会话

当回来时可以再执行执行：`screen -r lnmp` 即可恢复到离开前创建的lnmp会话的工作界面。如果忘记了，或者当时没有指定会话名，可以执行：screen -ls screen会列出当前存在的会话列表，如下图：


11791.lnmp即为刚才的screen创建的lnmp会话，目前已经暂时退出了lnmp会话，所以状态为Detached，当使用`screen -r lnmp`后状态就会变为Attached，11791是这个screen的会话的进程ID，恢复会话时也可以使用：screen -r 11791

1.4 关闭screen的会话

执行：`exit` 会提示：[screen is terminating]，表示已经成功退出screen会话。VPS侦探 https://www.vpser.net/

2、远程演示
首先演示者先在服务器上执行 screen -S test 创建一个screen会话，观众可以链接到远程服务器上执行screen -x test 观众屏幕上就会出现和演示者同步。

3、常用快捷键


- `Ctrl+a c` ：在当前screen会话中创建窗口
- `Ctrl+a w` ：窗口列表
- `Ctrl+a n ：下一个窗口
- `Ctrl+a p` ：上一个窗口
- `Ctrl+a 0-9` ：在第0个窗口和第9个窗口之间切换


执行：`screen -S lnmp` 创建screen会话。

* 如果网络掉线，可以重新连接SSH，再执行 `screen -r lnmp` 就会看到你的lnmp安装进程。
* 有时候screen异常退出可能会提示状态为Attached，可以执行：`screen -D -r lnmp` 进行恢复。
* 有时候可能会创建了多个同名的screen会话，可以执行：`screen -ls` 查看对应会话的session id，然后使用`screen -D -r sessionid` 进行恢复。


转载自： https://www.vpser.net/manage/screen.html
