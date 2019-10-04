---
title: "VMwareWorkstation单臂路由配置安装openwrt(LEDE)单网卡国际互联网"
date: 2018-10-28T00:00:00+08:00
lastmod: 2018-10-28T00:00:00+08:00
draft: false
tags: ["openwrt","VMwareWorkstation"]
categories: ["教程"]
comments: false
---


为什么要配置虚拟机环境
出差时，比如udp2raw等个别软件只能在LINUX环境才能使用，而在WIN环境下就没法例用它。


### 所需要的软件

* [VMware Workstation 15 Pro](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html) 注册码SN:`ZY1MK-4UD80-M81VY-RZYNX-Z22X2`
* [StarWind V2V Converter](https://www.starwindsoftware.com/starwind-v2v-converter) 
* [KoolShare 固件](http://firmware.koolshare.cn/LEDE_X64_fw867)  *选择普通的就行，不一定非要虚拟机转盘或PE下写盘专用*

### 使用StarWind V2V-Image Converter转换格式

`Image format`选择`Vmware growable image`

![](/images/StarWind V2V-Image Converter-1.png)

![](/images/StarWind V2V-Image Converter-2.png)


### 配置VMware Workstation
开始新建虚拟机，选择`稍后安装操作系统`

![](/images/openwrt-lede-VMware-Workstation-1.png)

客户机操作系统选择`Linx`版本`其它Linx 4.X或更高版本内核64位` 

![](/images/openwrt-lede-VMware-Workstation-2.png)

![](/images/openwrt-lede-VMware-Workstation-3.png)

一路默认完成创建,`编辑虚拟机设置`
删除打印机(可选)，删除原有的硬盘，然后添加硬盘`使用现有虚拟磁盘` 找到刚才转换好的后缀为.vmdk格式的并使用它。


![](/images/openwrt-lede-VMware-Workstation-4.png)

![](/images/openwrt-lede-VMware-Workstation-5.png)

`网络适配器`中的`网络连接`选择`桥接模式(B):直接连接物理网络`

![](/images/openwrt-lede-VMware-Workstation-6.png)

### 配置LEDE软路由

配置网络 
``` bash
vi /etc/config/network
```

这里有2种联网方式

#### 通过上级路由器拨号上网

上级路由器已经设置好了拨号上网。
假设宿主机路由器的网关为10.10.10.1
![](/images/openwrt-lede-VMware-Workstation-7.png)
此时可以删除WAN口(可选)，也可以保留。
![](/images/openwrt-lede-VMware-Workstation-8.png)
上图中上级拨号路由器的IP地址为`10.10.10.1`虚拟机软路由器的IP地址为`10.10.10.11` 按实际IP修改

1. `IPV4 网关`务必一定要设置成上级路由器的IP地址
2. `IPV4 地址`必须要跟上级路由器在同一个网段内
3. `DNS`可以设置成公共DNS，也可以使用运营商的。

联网后去`酪软中心`安装`科学上网`插件并配置好以后
其它设置，包括本机电脑的只要网关设置成`10.10.10.11`就能享受国际互联网了。

#### 在虚拟机路由器里拨号上网

待完善中... ...
