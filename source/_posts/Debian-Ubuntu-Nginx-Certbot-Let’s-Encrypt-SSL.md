---
title: "Nginx使用官方工具 Certbot 配置申请 Let’s Encrypt SSL 安全证书详细教程"
date: 2018-11-21T21:35:31-04:00
publishdate: 2018-11-21
lastmod: 2018-11-21
draft: false
tags: ["Debian","Nginx"]
comments: false
---


### 获取 Certbot 客户端

``` bash
wget https://dl.eff.org/certbot-auto
chmod a+x ./certbot-auto
./certbot-auto --help
```


### 配置 nginx 、验证域名所有权

在虚拟主机配置文件（ /etc/nginx/conf.d/xxx.com.conf ）中添加如下内容，这一步是为了通过 Let’s Encrypt 的验证，验证 linuxstory.org 这个域名是属于我的管理之下。（具体解释可见下一章“一些补充说明”的“ certbot 的两种工作方式”）

``` bash
location ^~ /.well-known/acme-challenge/ {
   default_type "text/plain";
   root     /home/wwwroot/xxx.com/;
}

location = /.well-known/acme-challenge/ {
   return 404;
}
```

`/home/wwwroot/xxx.com/`是站点根目录路径
`xxx.com`替换成自己的域名


3、重载 nginx


``` bash
systemctl restart nginx
```

4、生成证书

``` bash
./certbot-auto certonly --webroot -w /home/wwwroot/xxx.com/ -d  xxx.com
```

证书生成成功后，会有 Congratulations 的提示，并告诉我们证书放在 /etc/letsencrypt/live 这个位置

``` bash
/etc/letsencrypt/live/xxx.com/fullchain.pem
/etc/letsencrypt/live/xxx.com/privkey.pem
```

配置 Nginx修改`/etc/nginx/conf.d/xxx.com.conf`使用 SSL 证书

``` bash
ssl_certificate      /etc/letsencrypt/live/xxx.com/fullchain.pem;
ssl_certificate_key  /etc/letsencrypt/live/xxx.com/privkey.pem;
```

配置所有http自动跳转到https

``` bash
server {
    listen 80;
    server_name xxx.com;
    return 301 https://$server_name$request_uri;
}
```

重载 nginx


``` bash
systemctl restart nginx
```

### 后续工作

出于安全策略， Let’s Encrypt 签发的证书有效期只有 90 天，所以需要每隔三个月就要更新一次安全证书，虽然有点麻烦，但是为了网络安全，这是值得的也是应该的。好在 Certbot 也提供了很方便的更新方法。

测试`Certbot`进行更新

``` bash
./certbot-auto renew --dry-run
```

如果出现类似的结果，就说明测试成功了（总之有 Congratulations 的字眼）

``` bash
Congratulations, all renewals succeeded. The following certs have been renewed:  
   /etc/letsencrypt/live/linuxstory.org/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
** (The test certificates above have not been saved.)
```

设置定时任务

出于安全策略， Let’s Encrypt 签发的证书有效期只有 90 天。通过 ./certbot-auto renew 命令可以续签。 编辑 crontab 文件：

``` bash
crontab -e
```

添加 certbot 命令： 

在每天凌晨3点运行。该命令将检查服务器上的证书是否将在未来30天内过期，如果是，则进行更新。--quiet 指令告诉 certbot 不要生成输出。

``` bash
0 3 * * * /root/letsencrypt/certbot-auto renew --quiet
```

手动更新的方法

``` bash
./certbot-auto renew -v
```

自动更新的方法

``` bash
./certbot-auto renew --quiet --no-self-upgrade
```

完整的配置示范

``` bash
server {
    listen       80;
    server_name  xxx.com;
    return 301 https://$server_name$request_uri;
}
server {
    listen       443;
    server_name  xxx.com;
    root         /usr/share/nginx/test;
    location / {
	index  index.php index.html index.htm;
    }
	
	# php
	location ~ \.php$ {
	fastcgi_pass unix:/run/php/php7.2-fpm.sock;
	fastcgi_index index.php;
	fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	include fastcgi_params;
	}
	
	# Certbot
	location ^~ /.well-known/acme-challenge/ {
	default_type "text/plain";
	root   /usr/share/nginx/test/;
	}
	location = /.well-known/acme-challenge/ {
	return 404;
	}

	# ssl
    ssl on;
    ssl_certificate    /etc/letsencrypt/live/xxx.com/fullchain.pem;
    ssl_certificate_key    /etc/letsencrypt/live/xxx.com/privkey.pem;
    #include /etc/letsencrypt/options-ssl-nginx.conf;
    #ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    #ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;	

}
```


# certbot泛域名证书

申请命令：

``` bash
certbot certonly -d *.xxx.com --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```

`--manual`交互式获取，`--preferred-challenges dns`使用DNS验证的方式（泛域名只能使用DNS验证），`--server`指明支持acme-v02的Server地址，默认是acme-v01的地址。


按照要求输入邮箱，同意协议，当看到下面信息：

``` bash 
------------------------------------------------------------
Please deploy a DNS TXT record under the name
_acme-challenge.xxx.com with the following value:

JHkwGFgXq3OgedI-4RU1X0EcFUz7cxIPN7r1Qyw5JTw

Before continuing, verify the record is deployed.
------------------------------------------------------------
Press Enter to Continue
```

暂停操作，配置域名解析，添加一条TXT类型的解析：

纪录：`_acme-challenge.xxx.com` 结果：`JHkwGFgXq3OgedI-4RU1X0EcFUz7cxIPN7r1Qyw5JTw`

等待3-5分钟，使用以下命令检查解析是否设置成功：

``` bash
dig -t txt _acme-challenge.xxx.com @8.8.8.8
```

域名解析结果正确，再回到certbot申请的流程中敲回车，成功后，certbot会提示证书和私钥的路径，接着在nginx中配置并重启即可


问题一： `certbot`升级完成后，运行certbot进行泛域名申请时，提示缺少库文件：

No module named 'requests.packages.urllib3'

通过以下命令安装依赖：

``` bash
pip install requests urllib3 pyOpenSSL --force --upgrade
```

问题二： 再次运行时，继续提示错误：

ImportError: 'pyOpenSSL' module missing required functionality. Try upgrading to v0.14 or newer.


通过以下命令修复：

``` bash
pip install --upgrade --force-reinstall 'requests==2.6.0'
```

注意

申请的泛域名证书*.xxx.com并不能用在xxx.com，所以服务器还是需要两个证书。

