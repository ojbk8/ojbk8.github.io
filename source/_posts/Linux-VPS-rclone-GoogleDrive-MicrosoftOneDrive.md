---
title: "在Linux_VPS上使用rclone挂载GoogleDrive云端硬盘和MicrosoftOneDrive"
date: 2018-06-13T23:59:54-04:00
publishdate: 2018-06-13
lastmod: 2018-06-13
draft: false
tags: ["GoogleDrive","rclone","OneDrive"]

---

[rclone](https://rclone.org/)可以在Linux上挂载包括

``` bash
Rclone is a command line program to sync files and directories to and from:

Alibaba Cloud (Aliyun) Object Storage System (OSS)  
Amazon Drive   (See note)
Amazon S3  
Backblaze B2  
Box  
Ceph  
DigitalOcean Spaces  
Dreamhost  
Dropbox  
FTP  
Google Cloud Storage  
Google Drive  
HTTP  
Hubic  
Jottacloud  
IBM COS S3  
Koofr  
Memset Memstore  
Mega  
Microsoft Azure Blob Storage  
Microsoft OneDrive  
Minio  
Nextcloud  
OVH  
OpenDrive  
Openstack Swift  
Oracle Cloud Storage  
ownCloud  
pCloud  
put.io  
QingStor  
Rackspace Cloud Files  
Scaleway  
SFTP  
Wasabi  
WebDAV  
Yandex Disk  
The local filesystem  
```

且不会占用本地硬盘空间

# 安装rclone  

以Debian为例

``` bash
apt-get install fuse -y
curl https://rclone.org/install.sh | sudo bash
```

运行以下命令开始配置rclone

``` bash
rclone config
```

配置文件路径

``` bash
/root/.config/rclone/rclone.conf
```

## rclone挂载Google Drive

``` bash
root@MonstrousPointed-VM:~# rclone config
Current remotes:

Name                 Type
====                 ====
gdname               drive
odname               onedrive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> n   #选择n新建
name> googledrive   #名字随意例如googledrive
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / A stackable unification remote, which can appear to merge the contents of s                                                                                        everal remotes
   \ "union"
 2 / Alias for a existing remote
   \ "alias"
 3 / Amazon Drive
   \ "amazon cloud drive"
 4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dr                                                                                        eamhost, IBM COS, Minio, etc)
   \ "s3"
 5 / Backblaze B2
   \ "b2"
 6 / Box
   \ "box"
 7 / Cache a remote
   \ "cache"
 8 / Dropbox
   \ "dropbox"
 9 / Encrypt/Decrypt a remote
   \ "crypt"
10 / FTP Connection
   \ "ftp"
11 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
12 / Google Drive
   \ "drive"
13 / Hubic
   \ "hubic"
14 / JottaCloud
   \ "jottacloud"
15 / Koofr
   \ "koofr"
16 / Local Disk
   \ "local"
17 / Mega
   \ "mega"
18 / Microsoft Azure Blob Storage
   \ "azureblob"
19 / Microsoft OneDrive
   \ "onedrive"
20 / OpenDrive
   \ "opendrive"
21 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
22 / Pcloud
   \ "pcloud"
23 / QingCloud Object Storage
   \ "qingstor"
24 / SSH/SFTP Connection
   \ "sftp"
25 / Webdav
   \ "webdav"
26 / Yandex Disk
   \ "yandex"
27 / http Connection
   \ "http"
Storage> 12  #选12选定Google Drive
** See help for drive backend at: https://rclone.org/drive/ **

Google Application Client Id
Setting your own is recommended.
See https://rclone.org/drive/#making-your-own-client-id for how to create your o                                                                                        wn.
If you leave this blank, it will use an internal key which is low performance.
Enter a string value. Press Enter for the default ("").
client_id>
Google Application Client Secret
Setting your own is recommended.
Enter a string value. Press Enter for the default ("").
client_secret>
Scope that rclone should use when requesting access from drive.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / Full access all files, excluding Application Data Folder.
   \ "drive"
 2 / Read-only access to file metadata and file contents.
   \ "drive.readonly"
   / Access to files created by rclone only.
 3 | These are visible in the drive website.
   | File authorization is revoked when the user deauthorizes the app.
   \ "drive.file"
   / Allows read and write access to the Application Data folder.
 4 | This is not visible in the drive website.
   \ "drive.appfolder"
   / Allows read-only access to file metadata but
 5 | does not allow any access to read or download file content.
   \ "drive.metadata.readonly"
scope> 1
ID of the root folder
Leave blank normally.
Fill in to access "Computers" folders. (see docs).
Enter a string value. Press Enter for the default ("").
root_folder_id>
Service Account Credentials JSON file path
Leave blank normally.
Needed only if you want use SA instead of interactive login.
Enter a string value. Press Enter for the default ("").
service_account_file>
Edit advanced config? (y/n)
y) Yes
n) No
y/n> n
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes
n) No
y/n> n
If your browser doesn't open automatically go to the following link: https://accounts.google.com/o/oauth2/auth?access_type=offline&client_id=202xxxxxx5644.apps.gc6d8875f163240ea0bf58
#注意网址不能有空格，在浏览器中打开并按提示完成授权，获得code码
Enter verification code> xxxxxxxxxxxxx  #这里填写你刚才获得的code码
Configure this as a team drive?
y) Yes
n) No
y/n> y
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
Current remotes:

Name                 Type
====                 ====
gd                   googledrive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q

```

## rclone挂载Microsoft OneDrive

### 客户端授权

在本地Windows电脑上下载[rclone](https://rclone.org/downloads) [点我下载rclone客户端](https://downloads.rclone.org/v1.47.0/rclone-v1.47.0-windows-386.zip)
然后解压出来，进入文件夹，按住`shift`键后右击`在此处打开命令窗口`键入

``` bash
rclone authorize "onedrive"
```

这时会自动打开浏览器按提示登陆授权，当网页显示

``` bash
Success!
All done. Please go back to rclone.
```

回到CMD界面

``` bash
C:\Users\Users>cd /d d:\rclone
d:\rclone>rclone authorize "onedrive"
2018/12/24 19:14:35 NOTICE: Config file "C:\\Users\\Users\\.config\\rclone\\rclone.conf" not found - using defaults
If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth
Log in and authorize rclone for access
Waiting for code...
Got code
Paste the following into your remote machine --->
{"access_token":"EwBwA8l6BxxxxxxxxxxxxacbtK!VgGItbS7sxc","expiry":"2018-12-24T20:15:31.0560991+08:00"}
<---End paste
d:\rclone>
```

其中中括号在内就是VPS挂载所需要的result值，包括**{}**中括号在内

``` bash
{"access_token":"EwBwA8l6BxxxxxxxxxxxxacbtK!VgGItbS7sxc","expiry":"2018-12-24T20:15:31.0560991+08:00"}
```

### linux上挂载Microsoft OneDrive

``` bash
rclone config
```

开始配置

``` bash
root@MonstrousPointed-VM:~# rclone config
Current remotes:
e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> n
name> onedrive    #名字自定义
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / A stackable unification remote, which can appear to merge the contents of several remotes
   \ "union"
 2 / Alias for a existing remote
   \ "alias"
 3 / Amazon Drive
   \ "amazon cloud drive"
 4 / Amazon S3 Compliant Storage Providers (AWS, Ceph, Dreamhost, IBM COS, Minio)
   \ "s3"
 5 / Backblaze B2
   \ "b2"
 6 / Box
   \ "box"
 7 / Cache a remote
   \ "cache"
 8 / Dropbox
   \ "dropbox"
 9 / Encrypt/Decrypt a remote
   \ "crypt"
10 / FTP Connection
   \ "ftp"
11 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
12 / Google Drive
   \ "drive"
13 / Hubic
   \ "hubic"
14 / JottaCloud
   \ "jottacloud"
15 / Local Disk
   \ "local"
16 / Mega
   \ "mega"
17 / Microsoft Azure Blob Storage
   \ "azureblob"
18 / Microsoft OneDrive
   \ "onedrive"
19 / OpenDrive
   \ "opendrive"
20 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
21 / Pcloud
   \ "pcloud"
22 / QingCloud Object Storage
   \ "qingstor"
23 / SSH/SFTP Connection
   \ "sftp"
24 / Webdav
   \ "webdav"
25 / Yandex Disk
   \ "yandex"
26 / http Connection
   \ "http"
Storage> 18  #Microsoft OneDrive
** See help for onedrive backend at: https://rclone.org/onedrive/ **

Microsoft App Client Id
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_id>
Microsoft App Client Secret
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_secret>
Edit advanced config? (y/n)
y) Yes
n) No
y/n> n
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes
n) No
y/n> n
For this to work, you will need rclone available on a machine that has a web browser available.
Execute the following on your machine:
        rclone authorize "onedrive"
Then paste the result below:
result> {"access_token":"EwBwA8l6BAAURxxxxxxxxxxxxxxxxxxxxxxxxxxxCMszTZEI27EacbtK!VgGItbS7sxc","expiry":"2018-12-24T20:15:31.0560991+08:00"}
2018/12/24 06:39:03 ERROR : Failed to save new token in config file: section 'od100' not found
Choose a number from below, or type in an existing value
 1 / OneDrive Personal or Business
   \ "onedrive"
 2 / Root Sharepoint site
   \ "sharepoint"
 3 / Type in driveID
   \ "driveid"
 4 / Type in SiteID
   \ "siteid"
 5 / Search a Sharepoint site
   \ "search"
Your choice> 1
Found 1 drives, please select the one you want to use:
0:  (personal) id=xxxxxxxxxxxxxx
Chose drive to use:> 0  #0: 这里输入0
Found drive 'root' of type 'personal', URL: https://onedrive.live.com/?cid=e164800991cb8f79
Is that okay?
y) Yes
n) No
y/n> y
--------------------
[onedrive]
type = onedrive
token = {"access_token":"EwBwA8l6BAAURxxxxxxxxxxxxxxxxxxxxxxxxxxxCMszTZEI27EacbtK!VgGItbS7sxc","expiry":"2018-12-24T20:15:31.0560991+08:00"}
drive_id = xxxxxxxxxxxxxx
drive_type = personal  #personal个人版,Business挂载方法类似。
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
Current remotes:

Name                 Type
====                 ====
onedrive                onedrive
#这里能看到上面自定义名称说明成功了
e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
# 退出
```

# 配置rclone开机自动挂载为本地磁盘

``` bash
echo "#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
NAME_BIN="rclone"
### BEGIN INIT INFO
# Provides:          rclone
# Required-Start:    $all
# Required-Stop:     $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start rclone at boot time
# Description:       Enable rclone by daemon.
### END INIT INFO

NAME="rclone_name" #rclone name名
REMOTE='/rclone' #远程文件夹，Google Drive网盘里的挂载的一个文件夹
LOCAL='/mnt/gdrive' #VPS本地挂载目录

Green_font_prefix="\033[32m" && Red_font_prefix="\033[31m" && Green_background_prefix="\033[42;37m" && Red_background_prefix="\033[41;37m" && Font_color_suffix="\033[0m"
Info="${Green_font_prefix}[信息]${Font_color_suffix}"
Error="${Red_font_prefix}[错误]${Font_color_suffix}"
RETVAL=0

check_running(){
	PID="$(ps -C $NAME_BIN -o pid= |head -n1 |grep -o '[0-9]\{1,\}')"
	if [[ ! -z ${PID} ]]; then
		return 0
	else
		return 1
	fi
}
do_start(){
	check_running
	if [[ $? -eq 0 ]]; then
		echo -e "${Info} $NAME_BIN (PID ${PID}) 正在运行..." && exit 0
	else
		fusermount -zuq $LOCAL >/dev/null 2>&1
		mkdir -p $LOCAL
		sudo /usr/bin/rclone mount $NAME:$REMOTE $LOCAL --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 >/dev/null 2>&1 &
		sleep 2s
		check_running
		if [[ $? -eq 0 ]]; then
			echo -e "${Info} $NAME_BIN 启动成功 !"
		else
			echo -e "${Error} $NAME_BIN 启动失败 !"
		fi
	fi
}
do_stop(){
	check_running
	if [[ $? -eq 0 ]]; then
		kill -9 ${PID}
		RETVAL=$?
		if [[ $RETVAL -eq 0 ]]; then
			echo -e "${Info} $NAME_BIN 停止成功 !"
		else
			echo -e "${Error} $NAME_BIN 停止失败 !"
		fi
	else
		echo -e "${Info} $NAME_BIN 未运行"
		RETVAL=1
	fi
	fusermount -zuq $LOCAL >/dev/null 2>&1
}
do_status(){
	check_running
	if [[ $? -eq 0 ]]; then
		echo -e "${Info} $NAME_BIN (PID $(echo ${PID})) 正在运行..."
	else
		echo -e "${Info} $NAME_BIN 未运行 !"
		RETVAL=1
	fi
}
do_restart(){
	do_stop
	do_start
}
case "$1" in
	start|stop|restart|status)
	do_$1
	;;
	*)
	echo "使用方法: $0 { start | stop | restart | status }"
	RETVAL=1
	;;
esac
exit $RETVAL" >> /etc/init.d/rcloned
```

修改一下内容：

``` bash
NAME="rclone_name" #rclone name名
REMOTE='/rclone' #远程文件夹，Google Drive网盘里的挂载的一个文件夹
LOCAL='/mnt/gdrive' #VPS本地挂载目录
```

然后

``` bash
chmod +x /etc/init.d/rcloned
update-rc.d -f rcloned defaults
bash /etc/init.d/rcloned start
```

重启一下查看是否成功

``` bash
df -h
```

![](/images/df-honedrivegdrive.png)


如果上面脚本的挂载失败,可以手动挂载，以onedrive为例:

先新建一个目录,用于挂载

``` bash
mkdir -p /mnt/onedrive
```

挂载命令

``` bash
rclone mount onedrive: /mnt/onedrive --allow-other --allow-non-empty 
--vfs-cache-mode writes
```


# 相关命令

``` bash
### 文件上传
rclone copy /home/backup gdrive:backup # 本地路径 配置名字:谷歌文件夹名字
### 文件下载
rclone copy gdrive:backup /home/backup
### 列表
rclone ls gdrive:backup
rclone lsl gdrive:backup # 比上面多一个显示上传时间
rclone lsd gdrive:backup # 只显示文件夹
### 新建文件夹
rclone mkdir gdrive:backup
### 挂载
rclone mount gdrive:mm /root/mm &
### 卸载
fusermount -u  /root/mm
```

#### 其他 ####

``` bash
rclone config - 以控制会话的形式添加rclone的配置，配置保存在.rclone.conf文件中。
rclone copy - 将文件从源复制到目的地址，跳过已复制完成的。
rclone sync - 将源数据同步到目的地址，只更新目的地址的数据。   –dry-run标志来检查要复制、删除的数据
rclone move - 将源数据移动到目的地址。
rclone delete - 删除指定路径下的文件内容。
rclone purge - 清空指定路径下所有文件数据。
rclone mkdir - 创建一个新目录。
rclone rmdir - 删除空目录。
rclone check - 检查源和目的地址数据是否匹配。
rclone ls - 列出指定路径下所有的文件以及文件大小和路径。
rclone lsd - 列出指定路径下所有的目录/容器/桶。
rclone lsl - 列出指定路径下所有文件以及修改时间、文件大小和路径。
rclone md5sum - 为指定路径下的所有文件产生一个md5sum文件。
rclone sha1sum - 为指定路径下的所有文件产生一个sha1sum文件。
rclone size - 获取指定路径下，文件内容的总大小。.
rclone version - 查看当前版本。
rclone cleanup - 清空remote。
rclone dedupe - 交互式查找重复文件，进行删除/重命名操作。
```



挂载服务器

``` bash
apt-get install -y nload htop fuse p7zip-full
```
 
# 挂载为磁盘

``` bash
rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
```
 
# 卸载磁盘

``` bash
fusermount -qzu LocalFolder
```


