---
title: "hexo备份更换设备后同步编辑"
date: 2018-04-11T00:00:00+08:00
lastmod: 2018-04-11T00:00:00+08:00
draft: false
tags: ["hexo"]
categories: ["教程"]
comments: false
---

### 备份hexo原始配置文件
压缩打包备份原始的hexo配置文件scaffolds, source, themes 和 _config.yml
### 在新电脑上重新布署安装一次hexo

``` bash
cd /d
npm install hexo-cli -g
hexo init blog
cd blog
npm install
npm install hexo-deployer-git --save
git init
npm install hexo-generator-feed --save
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
npm install hexo-generator-searchdb --save
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

然后覆盖替换掉原有的文件scaffolds, source, themes 和 _config.yml

<!-- more -->
### hexo远程同步到github
[登陆GitHub](https://github.com/login)并创建分支为username.github.io 可以点击[Join GitHub](https://github.com/join?source=header-home)注册账号

``` bash
git config --global user.name "username"
git config --global user.email "youremail@gmail.com"
ssh-keygen -t rsa -C 'youremail@gmail.com'
```

Your public key has been saved in /c/Users/admin/.ssh/id_rsa.pub.
按提示一路回车即可以得到KEY,并收到邮件提醒。
用文本编辑器打开/c/Users/admin/.ssh/id_rsa.pub这个文件将里面的所有文本文字
复制到repo－Settings－Deploy keys－Add deploy key中，Title可以随便填写
回到Git Bash

``` bash
ssh -T git@github.com
```

测试是否连接成功，如果能看到

Hi username/username.github.io! You've successfully authenticated, but GitHub does not provide shell access.
即表示连接成功了。

``` bash
git remote add origin git@github.com:username/username.github.io.git
git commit -m "自定义描述"
git push -u origin master
```

### hexo开启github远程同步文件

``` bash
git add .
git commit -m ‘在新电脑上提交新文章’ #(引号内容可改)
git push
```

``` bash
hexo clean
hexo g
hexo d
```
