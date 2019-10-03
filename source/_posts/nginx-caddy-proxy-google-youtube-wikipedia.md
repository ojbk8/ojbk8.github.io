---
title: "利用Caddy,Nginx反向代理谷歌YouTube等网站"
date: 2018-05-21T00:00:00+08:00
lastmod: 2018-05-21T00:00:00+08:00
draft: false
tags: ["Caddy"]
categories: ["教程"]

---


[Caddy](https://caddyserver.com) - The HTTP/2 Web Server with Automatic HTTPS

- go语言一个二进制单文件就是服务端
- 自动申请[Let's Encrypt](https://letsencrypt.org)免费的SSL/TLS证书,自动续期。

### Caddy一键安装脚本
``` bash
wget -N --no-check-certificate https://raw.githubusercontent.com/ojbk8/ToyoDAdoubiBackup/master/caddy_install.sh && chmod +x caddy_install.sh && bash caddy_install.sh
```

- caddy_conf_file="/usr/local/caddy/Caddyfile" 
- 日志文件：cat /tmp/caddy.log 
- 使用说明：service caddy start | stop | restart | status 
- 或者使用：/etc/init.d/caddy start | stop | restart | status 

<!--more-->

### 反代[Google](https://www.google.com)

``` bash
echo "google.yourdomains.com {
 gzip
 tls xxxx@xxx.xx
 proxy / https://www.google.com
}" >> /usr/local/caddy/Caddyfile
```

同样我们可以使用此这个方法、等其它被精准扶贫的网站

### 反代[YouTube](https://www.youtube.com)

``` bash
echo "youtube.yourdomains.com {
 gzip
 tls xxxx@xxx.xx
 proxy / https://www.youtube.com
}" >> /usr/local/caddy/Caddyfile
```

重启Caddy生效

``` bash
/etc/init.d/caddy restart
```

### 反代[维基百科](https://zh.wikipedia.org/wiki/Wiki)

``` bash
echo "wiki.yourdomains.com {
 gzip
 tls xxxx@xxx.xx
 proxy / https://zh.wikipedia.org/wiki/Wiki
}" >> /usr/local/caddy/Caddyfile
```

### Nginx反代设置

``` bash
        location / {
                proxy_pass              https://www.google.com/;
                proxy_redirect          off;
                proxy_set_header        X-Real-IP       $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                sub_filter_once off;
                }
```
