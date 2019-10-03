---
title: "V2ray官方安装配置shadowsocks+WebSocket+TLS+Web+Cloudflare_CDN"
date: 2019-01-15T03:03:25-04:00
publishdate: 2019-01-15
lastmod: 2019-01-15
draft: false
tags: ["V2ray","shadowsocks","WebSocket","Caddy","Cloudflare"]

---

1. https://v2ray.com/
2. https://github.com/v2ray/v2ray-core
3. https://toutyrater.github.io/
4. https://guide.v2fly.org/
5. https://github.com/veekxt/v2ray-template

本篇主要针对websocket+tls+web三件套的使用方法

## 准备工作

1. 拥有公网IP且80 443端口可用的墙外VPS，NAT鸡一般不会给这2个端口使用权。
2. 注册并拥有域名
3. 域名绑定使用[Cloudflare ](https://www.cloudflare.com/zh-cn/)家的NS解析服务。


Cloudflare Nameservers分配给我的2个免费NS服务器。

``` bash
lynn.ns.cloudflare.com
tara.ns.cloudflare.com
```

把域名原有的NS删除并填上Cloudflare Nameservers分配给我的2个免费NS服务器。

假设我想

- a.yourdomain.com 用作直连
- b.yourdomain.com 套Cloudflare_CDN

在Cloudflare里添加A记录值Name分别为a和b到VPS的公网IP的2条解析，确保解析生效后才能使用。

## V2Ray安装

日常更新及安装curl

Debian & Ubuntu

``` bash
apt-get update
apt-get install wget curl -y
```

CentOS
``` bash
yum update
yum install wget curl -y
```

安装V2Ray

``` bash
bash <(curl -L -s https://install.direct/go.sh)
```

如果上面的官方链接失效了可以使用下面的

``` bash
bash <(curl -L -s https://raw.githubusercontent.com/ojbk8/v2ray-core/master/go.sh)
```


## V2ray配置文件

备份重命名默认生成的原始V2ray配置文件

``` bash
mv /etc/v2ray/config.json /etc/v2ray/config_Backup.json
```

按需要改动下面`/etc/v2ray/config.json`里面的`port` 和 `id`和 `password`等值，建议改成`/etc/v2ray/config_Backup.json`默认随机生成的值，当然也可以照抄不改直接复制使用。

编辑文件`/etc/v2ray/config.json`

``` bash
vi /etc/v2ray/config.json
```

加入下面的代码

``` json
{
  "inbounds": [{
    "port": 22782,
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "4e0f9136-5ea8-462c-bce9-7dd75376d378",
          "alterId": 64
        }
      ]
    },
	"streamSettings": {
	"network": "ws",
	"security": "none",
	"wsSettings": {
		"path": "/ray"
		}
      },
	"listen": "127.0.0.1"
  }],
    "inboundDetour": [
    {
      "protocol": "shadowsocks",
      "port": 1008,
      "settings": {
        "method": "chacha20-ietf-poly1305",
        "password": "yourpassword",
        "udp": true
      }
    },
    {
      "protocol": "shadowsocks",
      "port": 1009,
      "settings": {
        "method": "aes-128-gcm",
        "password": "youpassword",
        "udp": true
      }
    }
  ],
  "outbounds": [
  {
    "protocol": "freedom",
    "settings": {}
  }
  ]
}
```


**此文件配置可同时兼顾使用**

1. shadowsocks
2. WebSocket+TLS+Web
3. WebSocket+TLS+Web+Cloudflare_CDN


## 管理V2Ray进程


``` bash
service v2ray start
```

ubuntu 16.10+ 使用

``` bash
systemctl start v2ray.service
systemctl status v2ray.service
systemctl stop v2ray.service
systemctl restart v2ray.service
```

此脚本会自动安装以下文件：

- /usr/bin/v2ray/v2ray：V2Ray 程序；
- /usr/bin/v2ray/v2ctl：V2Ray 工具；
- /etc/v2ray/config.json：配置文件；
- /usr/bin/v2ray/geoip.dat：IP 数据文件
- /usr/bin/v2ray/geosite.dat：域名数据文件

此脚本会配置自动运行脚本。自动运行脚本会在系统重启之后，自动运行 V2Ray。目前自动运行脚本只支持带有 Systemd 的系统，以及 Debian / Ubuntu 全系列。

运行脚本位于系统的以下位置：

- /etc/systemd/system/v2ray.service: Systemd
- /etc/init.d/v2ray: SysV

脚本运行完成后，你需要：

1. 编辑 /etc/v2ray/config.json 文件来配置你需要的代理方式；
2. 运行 service v2ray start 来启动 V2Ray 进程；
3. 之后可以使用 service v2ray start|stop|status|reload|restart|force-reload 控制 V2Ray 的运行。

## 更新

停止v2ray覆盖重新安装一次再启动它

``` bash
service v2ray stop
bash <(curl -L -s https://install.direct/go.sh)
service v2ray start
```

## 配置WEB服务器WebSocket的规则

在配置文件中添加v2ray的WebSocket的规则，然后重启在WEB服务器生效。

### Caddy

``` bash
	proxy /ray localhost:22782 {
		websocket
		header_upstream -Origin
	}
```

点击查看[caddy安装教程](/post/caddy/)


### Nginx

``` bash
location /ray {
	proxy_redirect off;
	proxy_pass http://127.0.0.1:22782;
	proxy_http_version 1.1;
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection "upgrade";
	proxy_set_header Host $http_host;
	proxy_read_timeout 300s;
    }
```

### Apache

把以下配置加到VirtualHost值

``` bash
  <Location "/ray/">
    ProxyPass ws://127.0.0.1:22782/ray/ upgrade=WebSocket
    ProxyAddHeaders Off
    ProxyPreserveHost On
    RequestHeader append X-Forwarded-For %{REMOTE_ADDR}s
  </Location>
```

## Cloudflare设置CDN

注意：开启Cloudflare的CDN前，确保灰色云朵(用作直连)是没有点亮的，websocket+tls+web祼连可用。

- a.yourdomain.com 灰色云朵(用作直连)
- b.yourdomain.com 彩色云朵(套Cloudflare_CDN)

只要把a设置成灰色云朵，b设置成彩色云朵。如下图

![](/images/cloudflare-cdn.png)

使用Cloudflare的SSL证书

进入Cloudflare的`Crypto`把`SSL`选项设置成`Full`证书生效后是

```
Universal SSL Status - Active Certificate
```

![](/images/cloudflare-Crypto-SSL.png)

**注意**


Cloudflare的`Crypto`里面的`Minimum TLS Version`选择**强制TLS1.3需要V2ray服务端和客户端同时使用4.19.1+版本才能用**

此时

ping a.yourdomain.com 是VPS的IP

ping b.yourdomain.com 是Cloudflare的IP

## 客户端连接使用

``` json
{
  "inbounds": [
    {
      "port": 1080,
      "listen": "127.0.0.1",
      "protocol": "socks",
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      },
      "settings": {
        "auth": "noauth",
        "udp": false
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "a.yourdomain.com",
            "port": 443,
            "users": [
              {
                "id": "4e0f9136-5ea8-462c-bce9-7dd75376d378",
                "alterId": 64
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",
		"security": "none",
        "wsSettings": {
          "path": "/ray"
        }
      }
    }
  ]
}
```



- 地址(address):a.yourdomain.com
- 端口(port):443
- 用户ID:4e0f9136-5ea8-462c-bce9-7dd75376d378
- 额外ID(alterId):0  #只要≤服务端的值即可，值越少越不占内存
- 加密方式(security):none
- 传输协议(network):ws
- 伪装类型(type):none
- 伪装域名(host):留空不填
- 路径(path): /ray
- 底层传输安全: tls

把域名换成`b.yourdomain.com`即套用Cloudflare的CDN



