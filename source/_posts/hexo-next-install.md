---
title: 简要安装hexo和next主题
date: 2019-01-18 19:28:47
tags: ["hexo"]
comments: false

---

以下以Windows平台为例
### 下载安装Node和git
[Git - Downloads](https://git-scm.com/downloads) 和 [下载 | Node.js](https://nodejs.org/zh-cn/download/)
### 安装hexo
打开Git Bash依次执行

``` bash
npm install hexo-cli -g
cd /d  #这里以D盘为例
hexo init blog
cd blog #/d/blog为hexo的根目录
npm install
hexo server
```

应该就能在浏览器中打开(http://localhost:4000) 看到效果了。

<!--more-->

### hexo远程同步到github
登陆[GitHub](https://github.com)并创建分支为username.github.io 可以点击[Join GitHub](https://github.com/join?source=header-home)

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
安装 hexo-deployer-git

``` bash
npm install hexo-deployer-git --save
```

1. 先同步hexo生成的静态文件修改hexo根目录下的_config.yml配置文件

``` bash
deploy: 
  type: git
  repo: https://github.com/username/username.github.io
  branch: master
  message: hexo生成的静态文件
```

然后使用以下命令github同步

``` bash
hexo clean
hexo g
hexo d
```

2. 为了日后换电脑编辑同步github更方便，这里强烈建议先后依次2个branch分支

``` bash
deploy: 
  type: git
  repo: https://github.com/username/username.github.io
  branch: hexo
  message: hexo原始配置文件
```

然后使用以下命令github同步

``` bash
hexo clean
hexo g
hexo d
```

- repo:库（Repository）地址
- branch:分支名称。如果您使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。
- message:自定义提交信息

使用详情参考[Hexo](https://hexo.io/zh-cn/)
### 安装next主题 
定位到 Hexo 站点目录下

``` bash
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

修改站点根目录下的_config.yml配置文件,启用 NexT 主题

``` bash
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

详情参考: [开始使用 - NexT 使用文档](https://theme-next.iissnan.com/getting-started.html#theme-settings)
