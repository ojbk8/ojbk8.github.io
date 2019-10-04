---
title: "SSH使用SSH-KEY登录并禁用密码登录"
date: 2017-07-12T00:18:24-04:00
draft: false
tags: ["SSH"]
Keywords: [""]
categories: ["未分类"]
comments: false
---

SSH安全登陆三步曲

1. 修改默认端口22为其它
2. 改用SSH-keys登陆
3. 禁止密码登陆

- 路径: ~/.ssh/
- 私钥: ~/.ssh/id_rsa
- 公钥: ~/.ssh/id_rsa.pub


# 使用Xshell生成密钥公钥

依次点击菜单栏中的【工具】【新建用户密钥生成向导】
【密钥类型】选择`RSA`【长度】`2048`可以自定义
然后一路【下一步】【保存为文件】

生成私钥
工具 -> 用户秘钥管理者
选中秘钥类型 -> 导出
至此，生成了一对 公钥-私钥 对。
id_rsa_2048.pub #公钥
id_rsa_2048 #私钥

# 配置Xshell使用密钥登陆

创建目录

``` bash
mkdir .ssh
chmod 700 .ssh
```

创建文件

``` bash
cd .ssh
touch authorized_keys
chmod 600 authorized_keys
```

写入公钥内容

``` bash
nano /.ssh/authorized_keys
```

把`id_rsa_2048.pub`中的文本内容全部复制到`/.ssh/authorized_keys`

修改SSH配置文件

``` bash
nano /etc/ssh/sshd_config
```

启用密钥验证

``` bash
RSAAuthentication yes
PubkeyAuthentication yes
AuthorsizedKeysFile .ssh/authorized_keys
```

`AuthorsizedKeysFile .ssh/authorized_keys`是指定公钥数据库文件


重启SSH服务生效

RHEL/CentOS系统

``` bash
service sshd restart
```
ubuntu系统

``` bash
service ssh restart
```

debian系统

``` bash
/etc/init.d/ssh restart
```

切换到使用密钥登陆，如果成功了就可以禁止密码登陆了

PasswordAuthentication no

# 其它

PermitRootLogin yes

`yes`允许用户root登陆`no`禁用

Port 22

默认22端口，可自定义修改

``` bash
sudo -i
```

切换到root用户权限

``` bash
passwd root
```

设置root用户的密码

关于putty的登陆设置，由于putty与ssh2的key文件不同，所以要用puttygen.exe转换一下。

运行puttygen.exe，选择Conversions->Import key，选择privateKey文件，

注意这里要选privatekey文件，而不是前面用的publickey文件，
最后选择"save private key"将密钥保存为.ppk文件，这个就是putty需要的key文件。

运行putty，填写完session信息后，到connection->ssh->auth中指定好privatekey文件即可，
当然也可以在connection->data中设置上auto-login username


`PermitRootLogin yes`使用root登陆提示如下

``` bash
login as: root
Authenticating with public key "imported-openssh-key"
Please login as the user "ubuntu" rather than the user "root".
```

需要在authorized_keys中有一个命令，如下所示。

``` bash
cat /root/.ssh/authorized_keys 
no-port-forwarding,no-agent-forwarding,no-X11-forwarding,command="echo 'Please login as the user \"ubuntu\" rather than the user \"root\".';echo;sleep 10"
```

删除此行，并保留它后面的ssh-rsa和密钥。

保存文件，然后重试。
