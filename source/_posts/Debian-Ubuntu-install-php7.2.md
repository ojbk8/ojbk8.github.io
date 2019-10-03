---
title: "Debian和Ubuntu安装php7.2"
date: 2018-07-21T21:35:31-04:00
publishdate: 2018-07-21
lastmod: 2018-07-21
draft: false
tags: ["php","Debian","Ubuntu"]

---

适用系统： 

* Ubuntu 16.04 LTS 
* Ubuntu 14.04 LTS 
* Debian 9 stretch 
* Debian 8 jessie

# 安装 PHP

Ond?ej Sury 的 PHP PPA 为 Ubuntu 16.04/14.04 提供了 PHP7.2 版本，同时也有通过个人网站为 Debian 9/8 提供 PHP7.2 版本，因此 Ubuntu 是源于 Debian 所以基本可以通用，同时维护难度较低，软件源安装的 PHP 默认以 Unix Socket 的状态运行在 /run/php/php7.1-fpm.sock，比使用 TCP 以 localhost:9000 的方式性能更好。


# 添加软件源

> Ubuntu安装软件源拓展工具：

``` bash
apt -y install software-properties-common apt-transport-https lsb-release ca-certificates
```

添加 Ond?ej Sury 的 PHP PPA 源，需要按一次回车：

``` bash
add-apt-repository ppa:ondrej/php
```

更新软件源缓存：

``` bash
apt update
```

> Debian安装软件源拓展工具：

``` bash
apt -y install software-properties-common apt-transport-https lsb-release ca-certificates
```

添加 GPG

``` bash
wget -O /etc/apt/trusted.gpg.d/php.gpg https://mirror.xtom.com.hk/sury/php/apt.gpg
```

添加 sury 软件源

``` bash
sh -c 'echo "deb https://mirror.xtom.com.hk/sury/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'  
```

更新软件源缓存：

``` bash
apt-get update
```

# 安装 PHP7.2：

``` bash
apt install php7.2-fpm php7.2-mysql php7.2-curl php7.2-gd php7.2-mbstring php7.2-xml php7.2-xmlrpc php7.2-zip php7.2-opcache -y
```


> 设置 PHP

安装完成后，编辑 /etc/php/7.2/fpm/php.ini 替换换 ;cgi.fix_pathinfo=1 为 cgi.fix_pathinfo=0 快捷命令：

``` bash
sed -i 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/' /etc/php/7.2/fpm/php.ini
```

> 管理 PHP

安装好了先重启一下！

``` bash
systemctl restart php7.2-fpm
```

> 更多操作：


``` bash
php -v #查看PHP版本号
systemctl restart php7.2-fpm #重启
systemctl start php7.2-fpm #启动
systemctl stop php7.2-fpm #关闭
systemctl status php7.2-fpm #检查状态
```

> 更新 PHP

运行下面的命令系统就会更新所有可以更新的软件包括 PHP

``` bash
apt update
apt upgrade -y
```


安装更多组件


``` bash
apt-cache search php7.2
```

上面的一条命令安装 PHP 只是安装了部分 PHP 拓展，更多的软件可见：

``` bash
php-radius - radius client library for PHP
php-http - PECL HTTP module for PHP Extended HTTP Support
php-uploadprogress - file upload progress tracking extension for PHP
php-yaml - YAML-1.1 parser and emitter for PHP
php-mongodb - MongoDB driver for PHP
php-apcu - APC User Cache for PHP
php-imagick - Provides a wrapper to the ImageMagick library
php-ssh2 - Bindings for the libssh2 library
php-redis - PHP extension for interfacing with Redis
php-memcached - memcached extension module for PHP, uses libmemcached
php-apcu-bc - APCu Backwards Compatibility Module
php-rrd - PHP bindings to rrd tool system
php-uuid - PHP UUID extension
php-memcache - memcache extension module for PHP
php-zmq - ZeroMQ messaging bindings for PHP
php-igbinary - igbinary PHP serializer
php-msgpack - PHP extension for interfacing with MessagePack
php-geoip - GeoIP module for PHP
php-tideways - Tideways PHP Profiler Extension
php-yac - YAC (Yet Another Cache) for PHP
php-mailparse - Email message manipulation for PHP
php-oauth - OAuth 1.0 consumer and provider extension
php-gnupg - PHP wrapper around the gpgme library
php-propro - propro module for PHP
php-raphf - raphf module for PHP
php-solr - PHP extension for communicating with Apache Solr server
php-stomp - Streaming Text Oriented Messaging Protocol (STOMP) client module for PHP
php-gearman - PHP wrapper to libgearman
php-phalcon - full-stack PHP framework delivered as a C-extension
php-ds - PHP extension providing efficient data structures for PHP 7
php-sass - PHP bindings to libsass - fast, native Sass parsing in PHP
php-lua - PHP Embedded lua interpreter
libapache2-mod-php7.2 - server-side, HTML-embedded scripting language (Apache 2 module)
libphp7.2-embed - HTML-embedded scripting language (Embedded SAPI library)
php7.2-bcmath - Bcmath module for PHP
php7.2-bz2 - bzip2 module for PHP
php7.2-cgi - server-side, HTML-embedded scripting language (CGI binary)
php7.2-cli - command-line interpreter for the PHP scripting language
php7.2-common - documentation, examples and common module for PHP
php7.2-curl - CURL module for PHP
php7.2-dba - DBA module for PHP
php7.2-dev - Files for PHP7.2 module development
php7.2-enchant - Enchant module for PHP
php7.2-fpm - server-side, HTML-embedded scripting language (FPM-CGI binary)
php7.2-gd - GD module for PHP
php7.2-gmp - GMP module for PHP
php7.2-imap - IMAP module for PHP
php7.2-interbase - Interbase module for PHP
php7.2-intl - Internationalisation module for PHP
php7.2-json - JSON module for PHP
php7.2-ldap - LDAP module for PHP
php7.2-mbstring - MBSTRING module for PHP
php7.2-mysql - MySQL module for PHP
php7.2-odbc - ODBC module for PHP
php7.2-opcache - Zend OpCache module for PHP
php7.2-pgsql - PostgreSQL module for PHP
php7.2-phpdbg - server-side, HTML-embedded scripting language (PHPDBG binary)
php7.2-pspell - pspell module for PHP
php7.2-readline - readline module for PHP
php7.2-recode - recode module for PHP
php7.2-snmp - SNMP module for PHP
php7.2-soap - SOAP module for PHP
php7.2-sqlite3 - SQLite3 module for PHP
php7.2-sybase - Sybase module for PHP
php7.2-tidy - tidy module for PHP
php7.2-xml - DOM, SimpleXML, WDDX, XML, and XSL module for PHP
php7.2-xmlrpc - XMLRPC-EPI module for PHP
php7.2-zip - Zip module for PHP
php7.2-xsl - XSL module for PHP (dummy)
php7.2 - server-side, HTML-embedded scripting language (metapackage)
php7.2-sodium - libsodium module for PHP
```

以后内容转载自 https://www.mf8.biz/debian-install-php7-2/

# Caddy调用PHP7.2

在`/etc/caddy/Caddyfile`中添加以下代码

``` bash
fastcgi / /run/php/php7.2-fpm.sock php
```
