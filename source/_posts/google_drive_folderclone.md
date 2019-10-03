---
title: "Folderclone谷歌google共享云端硬盘转存相互转移拷贝复制文件的正确姿势"
date: 2019-09-18T01:21:39-04:00
draft: false
tags: ["drive","folderclone"]
Keywords: [""]
categories: ["教程"]
top: true
---

## 关于Folderclone

Folderclone，增加了服务帐户的TD成员和上载数据TB的，在使用某种算法每个服务帐户（750GB /天）的载配额。
基本上我们可以通过一个项目在TD中添加100个服务帐户。因此，每天可以复制的最大数据是每个项目最大750GB * 100 = 75TB（每天）。

首先计算您每天要复制的数据大小，取决于创建的项目数量。每个项目最多可以创建100个服务帐户。所以每个项目的复制容量是75tb。复制需要至少2个项目。

本教程中TD = Team drive和GD = Gdrive文件夹

详情请查看

https://github.com/Spazzlo/folderclone

https://github.com/SameerkumarP/Multifolderclone

https://blackpearl.biz/threads/7408/

https://blackpearl.biz/threads/7892/



## 新建项目


在Google云端控制台上设置2个项目转到[此处](https://console.developers.google.com) 
在【Google Cloud Platform】 【服务条款】中勾选【同意并继续】

![](/images/folderclone/GoogleCloudPlatform.png)

我们必须创建2个新项目，项目名称随便，例如我的项目名称是“foldercloneA”和“foldercloneB”。 

### 新建项目foldercloneA

在[Google云端控制台](https://console.developers.google.com) 【选择项目】【新建项目】

![](/images/folderclone/folderclone1.png)

![](/images/folderclone/foldercloneA1.png)


在【API和服务】【库】里面搜索【Google Drive API】和【Identity and Access Management (IAM) API】并启用它们。


![](/images/folderclone/apis.png)

![](/images/folderclone/GoogleDriveAPI.png)

![](/images/folderclone/Identity_and_Access_Management_IAM_API.png)


【导航菜单】【IAM和管理】【IAM】

![](/images/folderclone/apis1.png)

【服务账号】【创建服务账号】

![](/images/folderclone/apis2.png)

【服务帐号详情】【服务帐号名称】随便填  比如我填写的是

![](/images/folderclone/iam_admin_serviceaccounts.png)

【服务帐号权限（可选）】【请选择一个角色】【Project】【所有者】

![](/images/folderclone/iam_admin_serviceaccounts1.png)

【+添加其它角色】

![](/images/folderclone/iam_admin_serviceaccounts2.png)

【请选择一个角色】【Service Accounts】【Service Account Admin 】

![](/images/folderclone/iam_admin_serviceaccounts3.png)

2个角色【Project-所有者】和【Service Account Admin 】【继续】

![](/images/folderclone/iam_admin_serviceaccounts4.png)

【创建密钥】【密钥类型】选择默认的【JSON】【创建】

![](/images/folderclone/iam_admin_serviceaccounts5.png)

私钥已保存到您的计算机
folderclonea-253301-XXXXXXf.json 可用来访问您的云端资源，请妥善保存。

![](/images/folderclone/iam_admin_serviceaccounts6.png)

打开刚才创建下载的文件folderclonea-253301-XXXXXXf.json找到client_email把里面生成的邮箱记下来

![](/images/folderclone/folderclone_client_email.png)

比如我的是【folderclone@folderclonea-253301.iam.gserviceaccount.com】


### 新建项目foldercloneB

在[Google云端控制台](https://console.developers.google.com)【创建项目】【foldercloneB】【选择项目】并切换到项目foldercloneB



添加API【Google Drive API】和【Identity and Access Management (IAM) API】并启用它们。方法跟新建项目foldercloneA一样

【导航菜单】【IAM和管理】【IAM】【添加】项目foldercloneA文件folderclonea-253301-XXXXXXf.json里的邮箱地址

![](/images/folderclone/foldercloneB_add_iam_admin.png)

![](/images/folderclone/foldercloneB_add_iam_admin1.png)

![](/images/folderclone/foldercloneB_add_iam_admin2.png)

并记录保存好foldercloneB_ID

![](/images/folderclone/foldercloneB_ID.png)

比如我这里的是foldercloneb-253302

## 安装Python并配置依赖环境

使用64-bit的Windows  

### 下载Python并安装

1. [下载Python 3.7.4 64-bit](https://www.python.org/ftp/python/3.7.4/python-3.7.4-amd64.exe) 并按如下图所示安装

![](/images/folderclone/install_python1.png)

![](/images/folderclone/install_python2.png)

![](/images/folderclone/install_python3.png)

### 安装依赖环境

在C:\Program Files\Python37目录中按【Shift+鼠标右键】

![](/images/folderclone/Python99.png)

打开CMD安装以下依赖环境

``` bash
python -m pip install --upgrade pip
pip install oauth2client
pip install google-api-python-client
pip install progress
pip install progressbar2
pip install  httplib2shim
```


## 设置folderclone

项目地址https://github.com/Spazzlo/folderclone  

### 设置folderclone文件

1. [本站下载Multifolderclone-master.zip](/uploads/Multifolderclone-master.zip) 或者克隆[下载folderclone](https://github.com/Spazzlo/folderclone/archive/master.zip)到本地 解压  移动文件夹到您想要的地方

2. 在该目录下新建文件夹名为CONTROLLER

![](/images/folderclone/CONTROLLER.png)

3. 把项目foldercloneA文件folderclonea-253301-XXXXXXf.json复制到文件夹CONTROLLER目录下

![](/images/folderclone/CONTROLLER1.png)



### 生成并创建folderclone用户及配置文件

进入folderclone文件夹目录

![](/images/folderclone/folderclone_add_user.png)

运行以下命令

``` bash
python serviceaccountfactory.py
```

然后手动输入foldercloneB_ID空格加数字100回车，回车

输入生成的自定义邮箱前缀，比如我用的是users

比如我这里的是foldercloneb-253302 100

![](/images/folderclone/folderclone_users_config.png)

等待完成后会生成文件夹accounts里面会看到1到199的用户配置文件

![](/images/folderclone/accounts.png)

![](/images/folderclone/accounts1.png)

这时我们可以压缩保存整个文件夹了。


## 共享云端硬盘添加成员

1. 查看【共享云端硬盘】的ID

![](/images/folderclone/team_ID.png)

进入【共享云端硬盘】添加新成员项目folderclone-253301-XXXXXXf.json找到client_email里面的邮箱为管理员

![](/images/folderclone/team_add_admin.png)

进入folderclone文件夹目录

![](/images/folderclone/folderclone_add_user.png)

执行python masshare.py -d YYYYYY  其中YYYYYY替换成【共享云端硬盘】的ID

``` bash
python masshare.py -d YYYYYY
```

![](/images/folderclone/folderclone_add_user1.png)

![](/images/folderclone/folderclone_add_user2.png)


## 拷贝文件到共享云端硬盘

先获取共享链接，且设置成知道此链接的任何人都可以查看

![](/images/folderclone/googledrivesharelink.png)

例如分享链接https://drive.google.com/open?id=1WyhcL9GbMQbtlhrmEGVSIGAltE_G5bRA

![](/images/folderclone/sharefileID.png)

id=1WyhcL9GbMQbtlhrmEGVSIGAltE_G5bRA

如果是别人分享的链接，先右击选择添加到我的云端硬盘

![](/images/folderclone/add_my_drive.png)

 查看【共享云端硬盘】的ID

![](/images/folderclone/team_ID.png)

进入folderclone文件夹目录

![](/images/folderclone/Multifolderclone-master.png)

在CMD窗口执行

``` bash
python multifolderclone.py -s 源文件ID -d TD-ID
```


例如

``` bash
python multifolderclone.py -s 1WyhcL9GbMQbtlhrmEGVSIGAltE_G5bRA -d 0ANSyUOxsS0C7Uk9PVA
```

## 【共享云端硬盘】文件转移到【我的云端硬盘】

借助【共享云端硬盘】作为文件中转站，我们可以将【共享云端硬盘】里面的文件或文件夹【移至】其它【共享云端硬盘】或者【我的云端硬盘】

![](/images/folderclone/yizhi.png)

![](/images/folderclone/tixing.png)

文件所有者会变成我移动者且会占用空间大小

![](/images/folderclone/tixing555.png)

注意只能移至  相当于剪切  并不能复制


## GD目标文件夹的准备

比如我想要在【我的云端硬盘】里的某个文件夹里面复制转存文件，具体操作如下

### 安装扩展程序Email Extractor

[下载Chrome](https://dl.google.com/tag/s/dl/chrome/install/googlechromestandaloneenterprise64.msi) 浏览器，然后安装扩展程序[Email Extractor](https://chrome.google.com/webstore/detail/email-extractor/jdianbbpnakhcmfkcckaboohfgnngfcc?hl=en-US) 

![](/images/folderclone/add_EmailExtractor.png)


![](/images/folderclone/EmailExtractor.png)

### 获取源【共享云端硬盘】成员邮箱

切换到目标【共享云端硬盘】然后点击扩展程序【Email Extractor】【Copy all】复制【共享云端硬盘】所有成员的邮箱。

![](/images/folderclone/TDEmailExtractor.png)

### 创建共享目录文件夹添加成员

![](/images/folderclone/mydriveshare.png)

在【共享对象】里面用【Ctrl+V】粘贴刚才复制的成员邮箱

![](/images/folderclone/mydriveshareaddusers.png)

![](/images/folderclone/mydriveshareaddusers1.png)

现在您的GD目标文件夹已准备就绪。

## GD到GD文件传输

必须先完成上面的GD目标文件夹的准备，然后使用以下代码

``` bash
python multifolderclone.py -s ZZZZZZ -d DDDDDD
```

用源文件夹ID替换ZZZZZZ。
并将DDDDDD替换为目标文件夹（您刚设置的文件夹）

- 必须将共享文件夹添加到驱动器中
- 源文件夹的公共链接必须处于活动状态，否则服务帐户无法访问源文件夹数据。

## TD到GD文件传输

必须先完成上面的GD目标文件夹的准备，然后使用以下代码

``` bash 
python multifolderclone.py -s ZZZZZZ -d DDDDDD
```

用TD中的源文件夹ID替换ZZZZZZ，
用目标文件夹替换DDDDDD（您刚设置的文件夹）

1. 目标GD所在的帐户present必须是您正在传输文件的TD的管理员。
2. 应在TD和GD中使用相同的服务帐户。

## TD到TD文件传输

现在我们要对2个TD进行处理。一个是源TD，一个是目的地TD。

在根据我的原始帖子设置folderclone时，您必须使用这两个不同的TD ID运行masshare.py两次。

``` bash
python masshare.py -d 源TDid
```

``` bash
python masshare.py -d 目的地TDid
```

之后，为了在TD到TD之间传输数据，请使用以下代码

``` bash
python multifolderclone.py -s ZZZZZZ -d DDDDDD
```

- 将ZZZZZZ替换为源TD中的源文件夹ID 
- 将DDDDDD替换为目标TD中的目标文件夹。


要记住的事项

1. 必须将相同的服务帐户添加到两个TD。
2. 您将使用的源文件夹，必须生成公共链接。
3. 在所有情况下，必须将源共享文件夹添加到驱动器中

## 注意事项

1. folderclone在拷贝文件夹数目多的时候不全会丢文件，文件夹较少就不会，比如就在那么几个文件夹，哪怕每个文件夹目录下有成千上万个文件也没事。
2. 运行CMD窗口不要太多，最好最多就俩个吧，多了会大概率丢文件。
3. 速度比【Copy, URL to Google Drive】快
4. Folderclone丢失文件，拷贝不全不完整怎么办？删除重来或者用rclone copy校验补全
5. 文件刚分享或者刚移动文件(夹)不久，不要立刻使用folderclone，有时间延迟，最好适量等侍一会，否则会出现很多空文件夹或者丢失文件太离谱，我一般会隔一天再操作。
6. 使用[sstap](/uploads/sstap-beta-1.0.9.7.exe.7z)全局富强上网。
