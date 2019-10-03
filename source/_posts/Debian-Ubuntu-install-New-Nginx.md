---
title: "Debian9安装最新版Nginx"
date: 2018-11-21T21:35:31-04:00
publishdate: 2018-11-21
lastmod: 2018-11-21
draft: false
tags: ["Debian","Nginx"]

---

apt-get默认安装的nginx为1.10，如果嫌太老了可以修改安装源的方式来获得更新的版本；

一、添加Nginx源
新建一个nginx.list文件

``` bash
vi /etc/apt/sources.list.d/nginx.list
```

加入以下内容：

``` bash
deb http://nginx.org/packages/mainline/debian/ stretch nginx
deb-src http://nginx.org/packages/mainline/debian/ stretch nginx
```

下面这个我也不晓得干什么的，反正老外是这样写的

``` bash
wget -qO - http://nginx.org/keys/nginx_signing.key | apt-key add -
```

更新一下让源生效

``` bash
apt update
```

 

二、安装更新版本nginx

如果之前用apt命令安装了旧版本nginx,需要卸载掉才行

``` bash
apt remove nginx-common
```

正式开始安装

``` bash
apt install nginx
```

 

安装完成查看版本:


``` bash
nginx -v
```

会列出`Nginx` `OpenSSL`相关配置文件路径...

查看nginx进程

``` bash
ps aux | grep nginx
pgrep nginx
```


安装好的文件位置：

``` bash
/usr/sbin/nginx  #主程序
/etc/nginx  #存放配置文件
/usr/share/nginx  #存放静态文件
/var/log/nginx  #存放日志
/etc/nginx/nginx.conf  #默认nginx配置文件
```

Nginx控制命令：

``` bash
###启动
systemctl enable nginx
###重启
systemctl restart nginx
###停止
systemctl stop nginx
###也可以使用
/etc/init.d/nginx start # 启动Nginx服务
/etc/init.d/nginx stop # 停止Nginx服务
/etc/init.d/nginx  restart # 重启Nginx服务
```

# 增加 Nginx 虚拟主机

检查`/etc/nginx/nginx.conf`配置文件，确保文件中有：`include /etc/nginx/conf.d/*.conf;` 默认已经有了的。

然后在目录`/etc/nginx/conf.d/`下面新建文件`site1.conf`，`site2.conf`...文件名任意写，自己看明白就OK  

Nginx配置支持PHP

``` bash
	location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
	}
```    

或者

``` bash
	location ~ \.php$ {
        fastcgi_pass unix:/run/php/php7.2-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
	}
```

又或者

``` bash
	location ~ \.php$ {
	include snippets/fastcgi-php.conf;
	fastcgi_pass unix:/run/php/php7.2-fpm.sock;
	}
```


# 解决Nginx php-fpm配置有误引起的502错误

针对该问题的解决方案是，修改`/etc/nginx/conf/vhosts`下的conf文件，

将`fastcgi_pass 127.0.0.1:9000;`修改为`fastcgi_pass unix:/run/php/php7.0-fpm.sock;`

重启Nginx服务后，WEB访问依然报错502，继续定位分析。

在nginx error_log日志`/var/log/nginx/error.log`中出现了以下新的报错内容：

``` bash
2017/07/29 11:24:47 [crit] 6114#0: *1 connect() to unix:/run/php/php7.2-fpm.sock failed (13: Permission denied) while connecting to upstream, client: 183.14.134.xx, server: 112.74.89.xx, request: “GET / HTTP/1.1”, upstream: “fastcgi://unix:/run/php/php7.2-fpm.sock:”, host: “112.74.89.xx”
```


编辑文件`/etc/php/7.2/fpm/pool.d/www.conf`找到

``` bash
listen.owner = www-data
listen.group = www-data
;listen.mode = 0660
```

修改为

``` bash
;listen.owner = www-data
;listen.group = www-data
listen.mode = 0666
```

重启下Nginx和php

``` bash
systemctl restart nginx
systemctl restart php7.2-fpm
```
