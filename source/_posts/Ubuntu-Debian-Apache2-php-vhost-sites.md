---
title: "Ubuntu_Debian配置Apache2_PHP配置站点"
date: 2019-01-11T03:45:26-04:00
draft: false
tags: ["Ubuntu","Debian","Apache2","PHP"]
Keywords: [""]
categories: ["教程"]
comments: false
---

日常更新

``` bash
apt-get update -y
apt-get upgrade -y
```

查找程序名

``` bash
sudo apt-cache search apache | grep 'apache2'
```

# 安装Apache2

``` bash
apt-get install apache2
```

安装好后开服务

``` bash
service apache2 start
systemctl enable apache2
```

检查Apache2运行状态

``` bash
systemctl status apache2
```

安装Apache的php支持模块

``` bash
sudo apt-get install libapache2-mod-php
```

重启Apache2

``` bash
/etc/init.d/apache2 restart
```

停止

``` bash
/etc/init.d/apache2 stop
```

状态查询

``` bash
/etc/init.d/apache2 status
```

在浏览器中输入IP地址，看到以下页面说明apache成功安装

Debian系统上Apache2 Web服务器安装的配置布局如下：

``` bash
/etc/apache2/
|-- apache2.conf
|       `--  ports.conf
|-- mods-enabled
|       |-- *.load
|       `-- *.conf
|-- conf-enabled
|       `-- *.conf
|-- sites-enabled
|       `-- *.conf
```


1. apache2.conf is the main configuration file. It puts the pieces together by including all remaining configuration files when starting up the web server.
2. ports.conf is always included from the main configuration file. It is used to determine the listening ports for incoming connections, and this file can be customized anytime.
3. Configuration files in the mods-enabled/, conf-enabled/ and sites-enabled/ directories contain particular configuration snippets which manage modules, global configuration fragments, or virtual host configurations, respectively.
4. They are activated by symlinking available configuration files from their respective *-available/ counterparts. These should be managed by using our helpers a2enmod, a2dismod, a2ensite, a2dissite, and a2enconf, a2disconf . See their respective man pages for detailed information.
5. The binary is called apache2. Due to the use of environment variables, in the default configuration, apache2 needs to be started/stopped with /etc/init.d/apache2 or apache2ctl. Calling /usr/bin/apache2 directly will not work with the default configuration.

Document Roots

By default, Debian does not allow access through the web browser to any file apart of those located in /var/www, public_html directories (when enabled) and /usr/share (for web applications). If your site is using a web document root located elsewhere (such as in /srv) you may need to whitelist your document root directory in /etc/apache2/apache2.conf.

The default Debian document root is /var/www/html. You can make your own virtual hosts under /var/www. This is different to previous releases which provides better security out of the box.

Reporting Problems

Please use the reportbug tool to report bugs in the Apache2 package with Debian. However, check existing bug reports before reporting a new bug.

Please report bugs specific to modules (such as PHP and others) to respective packages, not to the web server itself.

查看apache2版本号

``` bash
apache2 -v
```

显示

``` bash
Server version: Apache/2.4.25 (Debian)
Server built:   2019-04-02T19:05:13
```

列出 Debian 仓库中与 Apache 相关的软件列表。

``` bash
apt-cache search apache | less
```

带有 “apache” 字眼的软件包通常都是与相应软件包有关的，如 -doc, -dev, -common, -perl …


https://wiki.debian.org/zh_CN/Apache


# 安装PHP及所需的模块：

``` bash
apt-get install php7.0 libapache2-mod-php7.0 php7.0-cli php7.0-mcrypt php7.0-intl php7.0-mysql php7.0-curl php7.0-gd php7.0-soap php7.0-xml php7.0-zip -y
```

查看PHP版本

``` bash
php -v
```

显示如下

PHP 7.0.33-0+deb9u3 (cli) (built: Mar  8 2019 10:01:24) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2017 Zend Technologies
    with Zend OPcache v7.0.33-0+deb9u3, Copyright (c) 1999-2017, by Zend Technologies
	
	
测试是否安装成功 
创建`test.php`脚本，保存到`/var/www/html`目录下,脚本内容为：

``` bash
<?php
echo "Hellow World";
?>
```

浏览器输入localhost/test.php，浏览器返回Hello World则环境配置成功




# 设置域名虚拟主机

使用Apache Web服务器时，您可以使用虚拟主机 （类似于Nginx中的服务器块）来封装配置详细信息并从单个服务器托管多个域。 我们将设置一个名为example.com的域名，但您应将其替换为您自己的域名 。 要了解有关使用DigitalOcean设置域名的更多信息，请参阅我们的DigitalOcean DNS简介 。

Debian 9上的Apache默认启用了一个服务器块，配置为从/var/www/html目录中提供文档。 虽然这适用于单个站点，但如果您托管多个站点，它可能会变得难以处理。 不要修改/var/www/html ，让我们在example.com网站的/var/www创建一个目录结构，将/var/www/html保留为默认目录，如果客户端请求没有匹配任何其他网站。

按如下所示为example.com创建目录，使用-p标志创建任何必需的父目录：

``` bash
sudo mkdir -p /var/www/example.com/html
```

接下来，使用$USER环境变量分配目录的所有权：

``` bash
sudo chown -R $USER:$USER /var/www/example.com/html
```

如果您尚未修改unmask值，则Web根目录的权限应该是正确的，但您可以通过键入以下内容来确保：

``` bash
sudo chmod -R 755 /var/www/example.com
```

接下来，使用nano或您喜欢的编辑器创建一个示例index.html页面：

``` bash
nano /var/www/example.com/html/index.html
```

在里面，添加以下示例HTML：

``` bash
/var/www/example.com/html/index.html
<html>
    <head>
        <title>Welcome to Example.com!</title>
    </head>
    <body>
        <h1>Success!  The example.com server block is working!</h1>
    </body>
</html>
```

完成后保存并关闭文件。

为了使Apache能够提供此内容，必须使用正确的指令创建虚拟主机文件。 不要直接修改位于`/etc/apache2/sites-available/000-default.conf`的默认配置文件，而是在`/etc/apache2/sites-available/example.com.conf`创建一个新文件：

``` bash
sudo nano /etc/apache2/sites-available/example.com.conf
```

粘贴在以下配置块中，类似于默认配置块，但为我们的新目录和域名更新：

``` bash
nano /etc/apache2/sites-available/example.com.conf
```

键入以下内容来


``` bash
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

请注意，我们已将DocumentRoot更新为新目录，将ServerAdmin更新为example.com站点管理员可以访问的电子邮件。 我们还添加了两个指令： ServerName ，它建立了应该与此虚拟主机定义匹配的基本域; ServerAlias ，它定义了应该匹配的更多名称，就像它们是基本名称一样。

完成后保存并关闭文件。

让我们使用a2ensite工具启用该文件：

``` bash
sudo a2ensite example.com.conf
```

禁用000-default.conf定义的默认站点：

``` bash
sudo a2dissite 000-default.conf
```

接下来，让我们测试一下配置错误：

``` bash
sudo apache2ctl configtest
```

您应该看到以下输出：

Syntax OK

重启Apache以实现更改：

``` bash
sudo systemctl restart apache2
```

Apache现在应该为您的域名服务。 您可以通过导航到http:// example.com来测试这一点，您应该在其中看到如下内容：

# 其它

* /var/www/html ：实际的Web内容默认只包含您之前看到的默认Apache页面，它是在/var/www/html目录下提供的。 这可以通过更改Apache配置文件来更改。
* /etc/apache2 ：Apache配置目录。 所有Apache配置文件都驻留在此处。
* /etc/apache2/apache2.conf ：主要的Apache配置文件。 可以对其进行修改以更改Apache全局配置。 该文件负责加载配置目录中的许多其他文件。
* /etc/apache2/ports.conf ：此文件指定Apache将监听的端口。 默认情况下，Apache在端口80上监听，并在启用提供SSL功能的模块时另外监听端口443。
* /etc/apache2/sites-available/ ：可以存储每站点虚拟主机的目录。 Apache不会使用此目录中的配置文件，除非它们链接到sites-enabled目录。 通常，所有服务器块配置都在此目录中完成，然后通过使用a2ensite命令链接到其他目录来启用。
* /etc/apache2/sites-enabled/ ：存储已启用的每站点虚拟主机的目录。 通常，这些是通过使用a2ensite链接到sites-available目录中的配置文件来创建的。 Apache在启动或重新加载以编译完整配置时读取此目录中的配置文件和链接。
* /etc/apache2/conf-available/ ， /etc/apache2/conf-enabled/ ：这些目录与sites-available和sites-enabled目录具有相同的关系，但用于存储不属于a的配置片段虚拟主机。 可以使用a2enconf命令启用conf-available目录中的文件，并使用a2enconf命令禁用这些a2disconf 。
* /etc/apache2/mods-available/ ， /etc/apache2/mods-enabled/ ：这些目录分别包含可用和已启用的模块。 以.load结尾的文件包含用于加载特定模块的片段，而以.conf结尾的文件包含这些模块的配置。 可以使用a2enmod和a2dismod命令启用和禁用模块。
* /var/log/apache2/access.log ：默认情况下，除非将Apache配置为执行其他操作，否则对Web服务器的每个请求都将记录在此日志文件中。
* /var/log/apache2/error.log ：默认情况下，所有错误都记录在此文件中。 Apache配置中的LogLevel指令指定错误日志将包含多少详细信息。


