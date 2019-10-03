---
title: "使用Aira2下载文件后自动上传到Google Drive网盘 onedrive"
date: 2018-12-22T23:25:41-04:00
publishdate: 2018-12-22
lastmod: 2018-12-22
draft: false
tags: ["Aira2"]
---

# 更新

``` bash
【2018.10.30】
这里分享下萌咖大佬的Aira2上传脚本，不过是精简版，全能版暂不分享，精简版包含以下功能：
1、脚本适用于Rclone挂载的网盘，比如Onedrive/Google Drive等。
2、判断上传文件的文件大小区间。
限制最低上传大小：可防止产生的.aria2后缀文件一起上传到网盘。
限制最高文件大小：适用于Onedrive等，官方限制上传不能超过15G，其它可自行更改其数值。
3、支持文件名中包含空格等特殊字符。
```


# 脚本说明: Aria2 一键安装管理脚本

系统支持: CentOS6+ / Debian6+ / Ubuntu14+

``` bash
wget -N --no-check-certificate https://raw.githubusercontent.com/ojbk8/doubi/master/aria2.sh && chmod +x aria2.sh && bash aria2.sh
```

> Aria2下载为什么会多出一个.aria2后缀的文件？

找到force-save参数，进行设置

`force-save=true` 会保存文件下载历史,但会在下载目录产生同名.aria2文件

`force-save=false`不产生同名.aria2文件,同时也不能能保存下载历史


# 使用方法

原理是当下载完后`aria2`会给脚本传3个参数`$1`、`$2`、`$3`分别为`gid`、文件数量、文件路径。我们对文件路径这个字符串处理一番就可以达到目的了。

新建脚本文件`rcloneupload.sh`并复制下面代码：

``` bash
#!/bin/bash
downloadpath='/root/Download' #需要上传的目录，Aria2下载目录
name='rclonename' #配置Rclone时填写的name
folder='/rclone' #网盘里的文件夹，留空为整个网盘。
MinSize='0.01k' #限制最低上传大小，默认10k，BT下载时可防止上传其他无用文件。会删除文件，谨慎设置。
MaxSize='15G' #限制最高文件大小，默认15G，OneDrive上传限制。

filepath=$3 #Aria2传递给脚本的原始路径，如果是单文件则为/root/Download/1.mp4，如果是文件夹则该值为文件夹内第一个文件比如/root/Download/a/b/1.mp4
rdp=${filepath#${downloadpath}/} #路径转换，去掉开头的下载路径。
path=${downloadpath}/${rdp%%/*} #文件或文件夹路径。如果是单个文件，应与原始路径一致。

if [ $2 -eq 0 ]
	then
		exit 0
fi

while true; do
if [ "$path" = "$filepath" ] && [ $2 -eq 1 ] #如果下载的是单个文件
	then
	rclone move -v "$filepath" ${name}:${folder} --min-size $MinSize --max-size $MaxSize
	rm -vf "$filepath".aria2 #删除残留的.aria.2文件
	exit 0
elif [ "$path" != "$filepath" ] #如果下载的是文件夹
	then
	while [[ "`ls -A "$path/"`" != "" ]]; do
	rclone move -v "$path" ${name}:"${folder}"/"${rdp%%/*}" --min-size $MinSize --max-size $MaxSize --delete-empty-src-dirs
	rclone delete -v "$path" --max-size $MinSize #删除多余的文件
	rclone rmdirs -v "$downloadpath" --leave-root #删除空目录，--delete-empty-src-dirs参数已实现，加上无所谓。
	done
	rm -vf "$path".aria2 #删除残留的.aria2文件
	exit 0
fi
done
```

或者


``` bash
#!/bin/bash

GID="$1";
FileNum="$2";
File="$3";
MinSize="5"  #限制最低上传大小，默认5k
MaxSize="157286400"  #限制最高文件大小(单位k)，默认15G
RemoteDIR="/RATS/";  #rclone挂载的本地文件夹，最后面保留/
LocalDIR="/download/";  #Aria2下载目录，最后面保留/

if [[ -z $(echo "$FileNum" |grep -o '[0-9]*' |head -n1) ]]; then FileNum='0'; fi
if [[ "$FileNum" -le '0' ]]; then exit 0; fi
if [[ "$#" != '3' ]]; then exit 0; fi

function LoadFile(){
  IFS_BAK=$IFS
  IFS=$'\n'
  if [[ ! -d "$LocalDIR" ]]; then return; fi
  if [[ -e "$File" ]]; then
    FileLoad="${File/#$LocalDIR}"
    while true
      do
        if [[ "$FileLoad" == '/' ]]; then return; fi
        echo "$FileLoad" |grep -q '/';
        if [[ "$?" == "0" ]]; then
          FileLoad=$(dirname "$FileLoad");
        else
          break;
        fi;
      done;
    if [[ "$FileLoad" == "$LocalDIR" ]]; then return; fi
    EXEC="$(command -v mv)"
    if [[ -z "$EXEC" ]]; then return; fi
    Option=" -f";
    cd "$LocalDIR";
    if [[ -e "$FileLoad" ]]; then
      ItemSize=$(du -s "$FileLoad" |cut -f1 |grep -o '[0-9]*' |head -n1)
      if [[ -z "$ItemSize" ]]; then return; fi
      if [[ "$ItemSize" -le "$MinSize" ]]; then
        echo -ne "\033[33m$FileLoad \033[0mtoo small to spik.\n";
        return;
      fi
      if [[ "$ItemSize" -ge "$MaxSize" ]]; then
        echo -ne "\033[33m$FileLoad \033[0mtoo large to spik.\n";
        return;
      fi
      eval "${EXEC}${Option}" \'"${FileLoad}"\' "${RemoteDIR}";
    fi
  fi
  IFS=$IFS_BAK
}
LoadFile;
```




授权

``` bash
chmod +x rcloneupload.sh
```

然后再到Aria2配置文件中加上一行

``` bash
on-download-complete=/root/rcloneupload.sh
```

即可，后面为脚本的路径。最后重启Aria2生效。


转载自： https://www.moerats.com/archives/482/
