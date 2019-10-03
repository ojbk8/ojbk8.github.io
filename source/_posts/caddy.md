---
title: "caddy官方脚本一键手动安装与使用"
date: 2019-02-14T22:16:58-04:00
publishdate: 2019-02-14
lastmod: 2019-02-14
draft: false
tags: ["caddy"]

---

caddy官网 ：https://caddyserver.com/

手动下载： https://caddyserver.com/download

Github：https://github.com/mholt/caddy

caddy会自动签发证书,有效期3个月，自动续期。

``` bash
CN = Let's Encrypt Authority X3
O = Let's Encrypt
C = US
```

**在启动caddy服务前，请确保域名A记录解析到VPS的IP上并生效。**

# 官方脚本安装

``` bash
curl https://getcaddy.com | bash -s personal
```

或者

``` bash
wget -qO- https://getcaddy.com | bash -s personal
```

若需安装插件

``` bash
curl https://getcaddy.com | bash -s personal http.git,dns
```

# 配置caddy

创建配置文件放到 /etc/caddy 目录

``` bash
sudo mkdir /etc/caddy
sudo touch /etc/caddy/Caddyfile
sudo chown -R root:www-data /etc/caddy
```

配置ssl证书目录

``` bash
sudo mkdir /etc/ssl/caddy
sudo chown -R www-data:root /etc/ssl/caddy
sudo chmod 0770 /etc/ssl/caddy
```

配置网站目录

``` bash
sudo mkdir /var/www
sudo chown www-data:www-data /var/www
```

配置 systemd

``` bash
sudo curl -s  https://raw.githubusercontent.com/mholt/caddy/master/dist/init/linux-systemd/caddy.service  -o /etc/systemd/system/caddy.service
sudo systemctl daemon-reload
sudo systemctl enable caddy.service
sudo systemctl status caddy.service
```

配置Caddfile配置文件
修改Caddfile文件

``` bash
vi /etc/caddy/Caddyfile
```

一个简单的websocket加静态网站配置

``` bash
www.google.com
{
  log /var/log/caddy/access.log
  tls google@gmail.com
  proxy /caressr 127.0.0.1:10000 {
    websocket
    header_upstream -Origin
  }
  root /var/www/
}
```

给log路径赋权

``` bash
sudo chown www-data:www-data /var/log/caddy
```

上例是一个简单的websocket加静态网站配置。第一行为自己的域名，tls后面加上邮箱会自动申请let’sencrypt ssl证书。Caddfile更多配置详见官网。

通过systemd管理caddy


``` bash
sudo systemctl start caddy.service
sudo systemctl stop caddy.service
sudo systemctl restart caddy.service
sudo systemctl reload caddy.service
```

# Caddy无法启动报错

``` bash
sudo systemctl status caddy.service
● caddy.service - Caddy HTTP/2 web server
   Loaded: loaded (/etc/systemd/system/caddy.service; enabled; vendor preset: en
   Active: failed (Result: exit-code) since Sat 2019-06-15 01:41:29 UTC; 1min 10
     Docs: https://caddyserver.com/docs
 Main PID: 712 (code=exited, status=1/FAILURE)

Jun 15 01:41:27 sg systemd[1]: Started Caddy HTTP/2 web server.
Jun 15 01:41:28 sg caddy[712]: Activating privacy features... 2019/06/15 01:41:2
Jun 15 01:41:28 sg caddy[712]: 2019/06/15 01:41:28 [INFO] [sg.567899.xyz] acme:
Jun 15 01:41:29 sg caddy[712]: 2019/06/15 01:41:29 [INFO] [sg.567899.xyz] AuthUR
Jun 15 01:41:29 sg caddy[712]: 2019/06/15 01:41:29 [INFO] [sg.567899.xyz] acme:
Jun 15 01:41:29 sg caddy[712]: 2019/06/15 01:41:29 [INFO] [sg.567899.xyz] acme:
Jun 15 01:41:29 sg systemd[1]: caddy.service: Main process exited, code=exited,
Jun 15 01:41:29 sg systemd[1]: caddy.service: Unit entered failed state.
Jun 15 01:41:29 sg systemd[1]: caddy.service: Failed with result 'exit-code'.
```

[解决方案](https://caddy.community/t/caddy-wont-start-could-not-start-http-server-for-challenge-listen-tcp-80-bind-permission-denied/2543)


修改服务文件取消以下注释：

``` bash
vi /etc/systemd/system/caddy.service
```

找到

``` bash
;CapabilityBoundingSet=CAP_NET_BIND_SERVICE
;AmbientCapabilities=CAP_NET_BIND_SERVICE
;NoNewPrivileges=true
```

修改为

``` bash
CapabilityBoundingSet=CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_BIND_SERVICE
NoNewPrivileges=true
```


# Caddy自动申请SSL证书位置：

``` bash
/etc/ssl/caddy/acme/acme-v02.api.letsencrypt.org/sites/xxx.xxx(域名)/
```
