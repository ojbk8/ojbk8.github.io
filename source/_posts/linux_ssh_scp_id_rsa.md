---
title: "Linux下scp使用RSA秘钥传输数据Scp命令免密码远程下载上传"
date: 2018-02-11T01:41:23-04:00
draft: false
tags: [""]
Keywords: [""]
categories: ["教程"]
---

### 生成密钥对

``` bash
ssh-keygen
```

* `id_rsa`为私钥文件(保存在client)
* `id_rsa.pub`为公钥(用于追加到server的用户目录`/.ssh/authorized_keys`文件中)

### 下载数据

``` bash
scp -i ~/.ssh/id_rsa -r root@server_ip:/var/www/ /var/www/
```

### 上传数据

``` bash
scp -i ~/.ssh/id_rsa -r /var/www/ root@server_ip:/var/www/
```

### ssh登录

``` bash
ssh user@host
```

### 其它



ssh使用RSA密钥对登录也是同理，service名称为sshd,配置文件为`/etc/ssh/sshd_config`


``` bash
chmod -R 700 ~/.ssh/ 
chmod 600 ~/.ssh/authorized_keys
``` 





### 指定端口

`-P`: 大写的P, 指定端口号 

``` bash 
scp -P 788 -i ~/.ssh/id_rsa -r /var/www/ root@server_ip:/var/www/
```
