---
title: "推送发布到GitHub及错误提示"
date: 2019-03-29T00:00:00+08:00
lastmod: 2019-03-29T00:00:00+08:00
draft: false
tags: ["GitHub"]
categories: ["教程"]
comments: false
---


安装git

``` bash
apt-get install git -y
```

进入目录后初始化

``` bash
git init
```

# 排除不需要备份的文件和目录

在根目录下创建`.gitignore`文件，里面的文件或目录不会被上传到github. 例：

``` bash
public
themes
dev
.git
.DS_Store
*.bak
*_bak
*.old
*.log
```


# 使用 SSH 密钥登录 GitHub

``` bash
git config --global user.name "username"
git config --global user.email "username168@gmail.com"
ssh-keygen -t rsa -C 'username@gmail.com'
```

然后使用

``` bash
cat /root/.ssh/id_rsa.pub
```

把里面的文本全部复制到
https://github.com/username/branchname/settings/keys `Add deploy key`

``` bash
ssh -T git@github.com
```

测试是否连接成功

# 推送到github

``` bash
git commit -m "自定义描述"
git remote add origin git@github.com:username/branchname.git
git push -u origin master
```

# git push错误提示

使用`git push -u origin master`时弹出错误提示

* src refspec master does not match any.

``` bash
error: src refspec master does not match any.
error: failed to push some refs to 'git@github.com:username/branchname.git'
```

然后用如下方法解决了：

``` bash
git add .
git commit -m "write your meaaage"  
```

* ERROR: Repository not found.

``` bash
ERROR: Repository not found.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

解决方案是

``` bash
git remote set-url origin git@github.com:username/branchname.git
```

# 更新push

以后当有更新时，使用以下三步曲上传。

``` bash
git add .
git commit -m "自定义描述" 
git push -u origin master
```
