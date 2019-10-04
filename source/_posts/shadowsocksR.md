---
title: "秋水逸冰一键shadowsocksR"
date: 2018-06-11T23:59:54-04:00
publishdate: 2018-06-11
lastmod: 2019-068-11
draft: false
tags: ["shadowsocksR","shadowsocks"]
comments: false
---

> https://shadowsocks.be/9.html

# 安装

``` bash
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```

如果链接失效可以使用下面的

``` bash
wget --no-check-certificate https://567899.xyz/bash/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```

使用命令：

``` bash
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
配置文件路径：/etc/shadowsocks.json
日志文件路径：/var/log/shadowsocks.log
代码安装目录：/usr/local/shadowsocks
卸载方法：./shadowsocksR.sh uninstall
```

安装完成后即已后台启动 ShadowsocksR ，运行：

``` bash
/etc/init.d/shadowsocks status
```


**多端口配置示范**

``` bash
{
    "server":"0.0.0.0",
    "server_ipv6":"[::]",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
    "80":"password1",
    "8770":"password2",
    "8451":"password3"
},
    "timeout":120,
    "method":"chacha20",
    "protocol":"auth_sha1_v4_compatible",
    "protocol_param":"",
    "obfs":"http_simple_compatible",
    "obfs_param":"",
    "redirect":"*:80#127.0.0.1:81",
    "dns_ipv6":false,
    "fast_open":false,
    "workers":1
}
```


``` bash
"redirect":"*:80#127.0.0.1:81"
```

上面的代码是设定shadowsocksR本地端口转发

