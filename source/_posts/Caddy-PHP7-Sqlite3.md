---
title: "一键安装 Caddy+PHP7+Sqlite3 环境"
date: 2018-06-15T00:00:00+08:00
lastmod: 2018-06-15T00:00:00+08:00
draft: false
tags: ["Caddy", "PHP", "Sqlite3"]
categories: ["教程"]

---



> 项目地址： (https://github.com/dylanbai8/Onekey_Caddy_PHP7_Sqlite3)

小内存 VPS 一键搭建 Caddy+PHP7+Sqlite3 环境 (支持VPS最小内存64M)，一键翻墙 caddy+web(php+sqlite3)+v2ray+bbr。


## 一键安装 Caddy+PHP7+Sqlite3 环境

- 解析好域名,执行以下命令
- 提示：支持IPv6（AAAA记录）如果本地网络不支持IPv6可以通过cloudflareCDN转换为IP4


``` bash
wget -N --no-check-certificate https://raw.githubusercontent.com/dylanbai8/Onekey_Caddy_PHP7_Sqlite3/master/c.sh && chmod +x c.sh && bash c.sh
```

#### Caddy+PHP7+Sqlite3 环境

``` bash
启动：systemctl start caddy 
停止：systemctl stop caddy 
重启：systemctl restart caddy 
网站根目录：/www 
Caddyfile文件: /etc/dylanbai8/caddy/Caddyfile
```

#### 一键安装 typecho 博客

``` bash
bash c.sh -t
```

#### 一键安装 wordpress 博客

``` bash
bash c.sh -w
```

#### 一键安装 zblog 博客

``` bash
bash c.sh -z
```

#### 一键安装 kodexplorer 可道云

``` bash
bash c.sh -k
```

#### 一键安装 laverna 印象笔记

``` bash
bash c.sh -l
```

#### 一键整站备份（一键打包/www目录 含数据库）

``` bash
bash c.sh -a
```

#### 一键安装 v2ray 翻墙

``` bash
bash c.sh -v
```

#### 一键安装 rinetd bbr 端口加速

``` bash
bash c.sh -b
```

#### 一键卸载命令：

``` bash
卸载 caddy
bash c.sh -unc

卸载 php+sqlite
bash c.sh -unp

卸载 v2ray
bash c.sh -unv

卸载 rinetdbbr
bash c.sh -unb
```

#### 文件目录权限设置

``` bash
chmod -R 755 /www/*
chown www-data:www-data -R /www/*
```
