---
title: "搭建配置frps内网穿透服务端开机自启"
date: 2018-05-15T00:00:00+08:00
lastmod: 2018-05-15T00:00:00+08:00
draft: false
tags: ["frps"]
categories: ["教程"]
comments: false
---

 
在debian ubuntu x64上测试通过 [其它系统frp下载](https://github.com/fatedier/frp/releases)

假设你的IPV4地址为11.22.33.44 假设是以ROOT用户登陆并在root用户默认目录下


### 下载安装frp

``` bash
wget -N -O frp.tar.gz https://github.com/fatedier/frp/releases/download/v0.23.1/frp_0.23.1_linux_amd64.tar.gz
tar -zxvf frp.tar.gz
mv frp_0.23.1_linux_amd64 frp
```
### 配置frps服务端
修改文件frp/frps.ini服务端配置文件,按需要添加删减,示范如下

<!--more-->


``` bash
echo "privilege_token = 12345
vhost_http_port = 800

[ssh]
listen_port = 6000

[ftp]
listen_port = 221

[aria2]
listen_port = 6844

[websocket]
listen_port = 4567" >> frp/frps.ini
```

### 配置frpc客户端
``` bash
server_port = 7000
privilege_token = 12345
vhost_http_port = 800

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000

[web]
type = http
local_ip = 127.0.0.1
local_port = 80
custom_domains = www.yourdomain.com

[ftp]
type = tcp
local_ip = 127.0.0.1
local_port = 21
remote_port = 221

[aria2]
type = tcp
local_ip = 127.0.0.1
local_port = 6800
remote_port = 6844
```

### 设置开机自动启动frp服务

添加自启文件
``` bash
echo "[Unit]
Description=frps daemon
After=syslog.target network.target
Wants=network.target

[Service]
Type=simple
ExecStart=/root/frp/frps -c /root/frp/frps.ini
Restart= always
RestartSec=1min

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/frps.service
```
#### 启动并设为开机自启
``` bash
systemctl enable frps
```

#### 启动frps

``` bash
systemctl start frps
```

#### 状态查询

``` bash
systemctl status frps
```

### 其它

如果使用的是自定义配置文件，经测试在koolshare_LEDE_X86路由器固件中不能开机启动frpc客户端
需要在**[common]**添加以下代码即可

``` bash
login_fail_exit = false
tcp_mux = true
log_file = /dev/null
log_level = error
log_max_days = 1
```


