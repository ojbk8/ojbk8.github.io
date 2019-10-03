---
title: "Debian/Ubuntu系统手动更新内核并启用TCP BBR拥塞控制算法"
date: 2017-12-21T22:03:07-04:00
publishdate: 2017-12-21
lastmod: 2017-12-21
draft: false
tags: ["Debian","Ubuntu","bbr"]

---

查看内核版本

``` bash
uname -r
```

BBR要求内核为kernel4.9以上版本。

如果大于或等于4.9的可以直接开启BBR,开启步骤如下

配置文件

``` bash
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
```

加载配置到内核参数中

``` bash
sysctl -p
sysctl net.ipv4.tcp_available_congestion_control
```

顺利的话下面的命令就能看到bbr模块了

``` bash
lsmod | grep bbr
```

``` bash
sysctl net.ipv4.tcp_available_congestion_control
```

返回值一般为：

``` bash
net.ipv4.tcp_available_congestion_control = bbr cubic reno
```

``` bash
sysctl net.ipv4.tcp_congestion_control
```

返回值一般为：

``` bash
net.ipv4.tcp_congestion_control = bbr
```

``` bash
sysctl net.core.default_qdisc
```

返回值一般为：

``` bash
net.core.default_qdisc = fq
```

如果内核版本低于4.9的需要手动升级到4.9或以上才能开启

去这里[下载最新版的内核 deb 安装包](https://kernel.ubuntu.com/~kernel-ppa/mainline/)

1. 如果系统是 64 位，则下载 amd64 的 linux-image 中含有 generic 这个 deb 包；
2. 如果系统是 32 位，则下载 i386 的 linux-image 中含有 generic 这个 deb 包；

安装的命令如下（以最新版的 64 位 4.12.4 举例而已，请替换为下载好的 deb 包）：

``` bash
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v4.12.4/linux-image-4.12.4-041204-generic_4.12.4-041204.201707271932_amd64.deb
dpkg -i linux-image-4.12.4-041204-generic_4.12.4-041204.201707271932_amd64.deb
```

安装完成后，再执行命令：

``` bash
/usr/sbin/update-grub
```

最后，重启 VPS 即可。

``` bash
reboot
```
