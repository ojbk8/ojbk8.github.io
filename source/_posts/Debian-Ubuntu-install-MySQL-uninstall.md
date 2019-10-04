---
title: "Debian和Ubuntu安装最新的MySQL修改密码卸载清理"
date: 2018-07-24T21:35:31-04:00
publishdate: 2018-07-24
lastmod: 2018-07-24
draft: false
tags: ["MySQL","Debian","Ubuntu"]
comments: false
categories: 教程
---

Debian 9上安装最新的MySQL

在Debian 9中，MySQL项目的社区分支MariaDB被打包为默认的MySQL变体。虽然MariaDB在大多数情况下运行良好，但如果您需要仅在Oracle的MySQL中找到的功能，则可以从MySQL开发人员维护的存储库中安装和使用软件包。

# 安装MySQL

首先添加MySQL软件库

MySQL开发人员提供了一个.deb包，用于处理配置和安装官方MySQL软件存储库。一旦设置了存储库，我们就可以使用Ubuntu的标准apt命令来安装该软件。我们将使用wget下载此.deb文件，然后使用该dpkg命令进行安装。

首先，在Web浏览器中加载[MySQL下载页面](https://dev.mysql.com/downloads/repo/apt/)。找到右下角的“ 下载”按钮，然后单击下一页。此页面将提示您登录或注册Oracle Web帐户。我们可以跳过这一点，而是寻找说不用的链接，只需启动我的下载。右键单击该链接并选择“ 复制链接地址”（此选项的措辞可能不同，具体取决于您的浏览器）。

现在我们要下载文件了。在您的服务器上，移动到您可以写入的目录。使用wget下载文件，记住粘贴刚刚复制的地址代替下面突出显示的部分：


``` bash
cd /tmp
wget https://dev.mysql.com/get/mysql-apt-config_0.8.13-1_all.deb
```

该文件现在应该下载到我们当前的目录中`ls`列出文件以确保：

您应该看到列出的文件名：

``` bash
mysql-apt-config_0.8.13-1_all.deb
```

现在我们准备安装：

``` bash
dpkg -i mysql-apt-config*
```

dpkg将会被用于安装，删除和检查.deb软件包。该-i标志表示我们要从指定的文件安装。

在安装过程中，您将看到一个配置屏幕，您可以在其中指定您喜欢的MySQL版本，以及为其他MySQL相关工具安装存储库的选项。默认值将添加最新稳定版MySQL的存储库信息，而不是其他任何内容。这就是我们想要的，所以使用向下箭头导航到Ok菜单选项并点击ENTER。

该包现在将完成添加存储库。刷新apt包缓存以使新软件包可用：

``` bash
dpkg-reconfigure mysql-apt-config
apt-get update
```

添加了存储库并使用我们的软件包缓存进行了新近更新

安装MySQL

我们现在可以使用apt安装最新的MySQL服务器软件包：

``` bash
apt install mysql-server
```

状态查询

``` bash
systemctl status mysql
```

# 修改MYSQL的初使密码

``` bash
mysql -u root -p
```

用安装时设置的密码登陆

``` bash
use mysql
```

开始修改密码

``` bash
update user set authentication_string=password('yournewpassword') where user='root';
```

退出

``` bash
exit
```

经过我们修改MYSQL的初使密码后，方可以顺利登陆phpMyAdmin管理

用SET PASSWORD命令

``` bash
mysql -u root
mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('yournewpassword');
```

最后重启一下MySQL

``` bash
systemctl restart mysql
```

# 卸载清理MYSQL

首先你可以通过

``` bash
dpkg --get-selections | grep mysql
```

命令罗列出你电脑上安装的和MySQL相关的软件，然后purge卸载

``` bash
apt-get --purge remove mysql-server
apt-get --purge remove mysql-client
apt-get --purge remove mysql-common
apt-get --purge remove mysql-apt-config
apt-get --purge remove mysql-community-client
apt-get --purge remove mysql-community-server
```

上面列出的安装MYSQL组件列表会不一样，请根据自己的实际操作。

最后再通过下面的命令清理残余：

``` bash
apt-get autoremove
apt-get autoclean
rm /etc/mysql/ -R
rm /var/lib/mysql/ -R
```

至此卸载清理工作全部完成
