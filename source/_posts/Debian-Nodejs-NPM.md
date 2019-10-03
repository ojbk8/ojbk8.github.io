---
title: "Debian9安装最新版Node.js和NPM"
date: 2019-01-15T00:00:00+08:00
lastmod: 2019-01-15T00:00:00+08:00
draft: false
tags: ["Node.js","NPM"]
categories: ["教程"]

---

`Node.js`是一个基于Chrome V8 JavaScript引擎构建的平台.Nodejs可用于轻松构建快速，可扩展的网络应用程序。最新版本node.js ppa由其官方网站维护。我们可以将这个PPA添加到Debian 9（Stretch） Debian 8（Jessie）和Debian 7（Wheezy）系统中。这篇我抄来的文章可以帮助你在Debian 9/8/7系统上安装最新的Nodejs和NPM。

# 添加Node.js PPA
首先，您需要在我们的系统中由Nodejs官方网站提供node.js PPA。如果尚未安装，我们还需要安装python-software-properties软件包。您可以选择安装最新的Node.js版本或LTS版本。

最新版安装命令：

``` bash
curl -sL https://deb.nodesource.com/setup_9.x | sudo bash
```

安装LTS长期维护版：

``` bash
apt-get install curl python-software-properties
curl -sL https://deb.nodesource.com/setup_8.x |  bash
```

# 安装Node.js和NPM
添加所需的PPA文件后，可以安装Nodejs包。NPM也将与node.js一起安装。该命令还会在您的系统上安装许多其他相关软件包。

``` bash
apt-get install nodejs
```

# 检查Node.js和NPM版本
安装node.js后，验证并检查安装的版本。你可以在node.js 官方网站上找到关于当前版本的更多细节。

检查Node.js版本

``` bash
node -v 
```

检查npm版本

``` bash
npm -v 
```
