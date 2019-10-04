---
title: "静态网站生成器hugo安装配置主题"
date: 2018-07-19T00:00:00+08:00
lastmod: 2018-07-19T00:00:00+08:00
draft: false
tags: ["hugo"]
categories: ["教程"]
comments: false
---

## install-hugo

下载[Hugo](https://github.com/gohugoio/hugo/releases) 对应平台解压得到二进制文件


### Linux安装hugo

以64bit为例，此安装方法适用于debian ubuntu centos openwrt等X64位的系统

``` bash
wget https://github.com/gohugoio/hugo/releases/download/v0.54.0/hugo_0.54.0_Linux-64bit.tar.gz
tar zxvf hugo_0.54.0_Linux-64bit.tar.gz
mv hugo /usr/bin/hugo
```

### Windows安装hugo

下载[hugo_extended_0.54.0_Windows-64bit.zip](https://github.com/gohugoio/hugo/releases/download/v0.54.0/hugo_extended_0.54.0_Windows-64bit.zip) 并解压缩

``` bash
C:\Hugo\bin
 - hugo.exe
 - LICENSE
 - README.md
```

添加到PATH

``` bash
set PATH=%PATH%;C:\Hugo\bin
```

### 验证是否安装成功

``` bash
hugo version
```

能看到Hugo版本号说明已经成功安装好了hugo

``` bash
C:\Hugo>set PATH=%PATH%;C:\Hugo\bin

C:\Hugo>hugo version
Hugo Static Site Generator v0.54.0/extended windows/amd64 BuildDate: unknown

C:\Hugo>
```

## Hugo新建网站

使用以下命令新建站点

``` bash
hugo new site blog
```

Hugo会自动新建名为`blog`的文件夹

``` bash
C:\HUGO\BLOG
├─archetypes
├─content
├─data
├─layouts
├─static
└─themes
```

文件夹描述相关

- `archetypes`使用hugu new post生成新文章的模板，可以自定义里面的值
- `content` 存放网站内容
- `data` 存储Hugo在生成您的网站时可以使用的配置文件
- `layouts` 以.html文件的形式存储模板，指定如何将内容的视图呈现到静态网站中
- `static` 存储所有静态内容：图像，CSS，JavaScript等，当Hugo构建您的站点时，静态目录中的所有资产都将按原样复制
- `themes` 存放主题
- `config.toml` 配置文件

进入`blog`的文件夹

``` bash
hugo
```

生成静态网站`public` 

``` bash
hugo server
```

## 主题安装

先去[Hugo官方主题](https://themes.gohugo.io)中选择一个自己中意的

通过git submodule add以Git子模块的方式下载（推荐）

进入网站文件夹，使用git init初始化后再下载主题

``` bash
cd example.com
git init
git submodule add https://github.com/olOwOlo/hugo-theme-even.git themes/even
```

通过git clone下载（此方式下载的主题带有.git和网站本身的Git版本控制产生冲突）

进入网站文件夹，使用git clone直接下载

``` bash
git clone https://github.com/olOwOlo/hugo-theme-even themes/even
```


复制主题的配置文件

下载完后，找到主题目录`/themes/even/exampleSite`下的配置文件`config.toml`，将该文件复制到网站文件夹下替换原来的`config.toml`，根据需求更改配置文件即可。也可以同时将`/themes/even/exampleSite`下的全部文件复制到网站目录下，`/themes/even/exampleSite/content`包含了一些示例文章可作参考
把文章模版`/themes/even/archetypes/default.md`覆盖掉`/archetypes/default.md`

创建一篇文章

``` bash
hugo new post/my-first-post.md
```


文章会保存在coneten/post目录下

``` bash
hugo server -D
```


