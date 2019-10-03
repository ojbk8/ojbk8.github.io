---
title: "ubuntu-18.04设置systemctl开机启动脚本rc.local"
date: 2019-02-23T23:17:32-04:00
draft: false
tags: [""]
Keywords: [""]
categories: ["教程"]
---

使用`update-rc.d`以及`rc.local`等方法就是不生效。后来在ubuntu的官方论坛ubuntu-16.10开始不再使用initd管理系统改用systemd

ubuntu-18.04 LTS版本用的是systemctl命令来替换了service和chkconfig的功能。

> systemd is now used for user sessions. System sessions had already been provided by systemd in previous Ubuntu releases.


比如以前启动 mysql 服务用:

``` bash
sudo service mysql start
```

现在用：

``` bash
sudo systemctl start mysqld.service
```

其实这个改动到不是算大，主要是开机启动比以前复杂多了。systemd 默认读取 /etc/systemd/system 下的配置文件，该目录下的文件会链接/lib/systemd/system/下的文件。

执行 ls /lib/systemd/system 你可以看到有很多启动脚本，其中就有我们需要的 rc.local.service

打开脚本内容：

``` bash
cat /lib/systemd/system/rc.local.service
```

可以看出`/lib/systemd/system/rc.local.service`的启动顺序是没有`Install`段

``` bash
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

# This unit gets pulled automatically into multi-user.target by
# systemd-rc-local-generator if /etc/rc.local is executable.
[Unit]
Description=/etc/rc.local Compatibility
ConditionFileIsExecutable=/etc/rc.local
After=network.target

[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
RemainAfterExit=yes
```

一般正常的启动文件主要分成三部分

1. `Unit`段: 启动顺序与依赖关系 
2. `Service`段: 启动行为,如何启动，启动类型 
3. `Install`段: 定义如何安装这个配置文件，即怎样做到开机启动

下面为`/lib/systemd/system/rc.local.service`添加`Install`段

``` bash
echo "
[Install]  
WantedBy=multi-user.target  
Alias=rc-local.service" >> /lib/systemd/system/rc.local.service
```

这里需要注意一下ubuntu-18.04默认是没有`/etc/rc.local`这个文件的，需要自己创建

``` bash
sudo touch /etc/rc.local
```

添加`rc.local`自定义启动

``` bash
sudo echo "#!/bin/bash" > /etc/rc.local
```

添加测试,编辑文件`/etc/rc.local`中添加


``` bash
echo "test" > /root/test.txt
```

给予可执行权限

``` bash
chmod 755 /etc/rc.local
```

`systemd`默认读取`/etc/systemd/system`下的配置文件, 所以还需要在`/etc/systemd/system`目录下创建软链接

``` bash
ln -s /lib/systemd/system/rc.local.service /etc/systemd/system/
```

重启系统,然后看看/root/test.txt文件的内容为test 是否存在就知道开机脚本是否生效了。
