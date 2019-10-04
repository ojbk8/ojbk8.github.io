---
title: "手动配置安装WireGuard服务端"
date: 2019-02-08T00:00:00+08:00
lastmod: 2019-02-08T00:00:00+08:00
draft: false
tags: ["WireGuard"]
categories: ["教程"]
comments: false
---


### WireGuard简单介绍
WireGuard是通过UDP协议传输数据的，这意味着它可以搭建在被墙的服务器上使用，复活被墙IP！

更安全的加密
* Curve25519 目前最高水平的秘钥交换算法。
* ChaCha20 对称加解密算法，比 AES 更快更高效。
* Poly1305 是一种 MAC (Message Authentication Code) 标准，用于验证数据的完整性和消息的真实性。
* BLAKE2 一种更安全的 HASH 算法（类似的有 SHA1, SHA256, MD5）
* SipHash24 另一种 HASH 算法。
* HKDF 一种秘钥衍生算法。

### 前提要求
系统要求：Debian 8 / 9、Ubuntu 14.04 / 16.04 / 18.04 / 18.10
服务器要求：OpenVZ 虚拟化的服务器不支持安装该VPN，其他虚拟化均可。
注意：如果你用的是 Vultr、DO，且你本地没有 IPv6 地址,那就不要勾选 Enable IPv6否则可能客户端链接时可能会出错。
[CentOS7官方安装代码](https://www.wireguard.com/install/#red-hat-enterprise-linux-centos-module-tools)


<!--more-->


### Debian安装步骤

安装和 linux-image 内核版本相对于的 linux-headers 内核

``` bash
apt update
apt install linux-headers-$(uname -r) -y
```

Debian安装后使用以下命令检测linux-headers内核是否安装成功

``` bash
dpkg -l|grep linux-headers
```

安装WireGuard

``` bash
echo "deb http://deb.debian.org/debian/ unstable main" > /etc/apt/sources.list.d/unstable.list
echo -e 'Package: *\nPin: release a=unstable\nPin-Priority: 150' > /etc/apt/preferences.d/limit-unstable
apt update
apt install wireguard resolvconf -y
```
resolvconf 是用来指定DNS的，旧一些的系统可能没装。

### Ubuntu安装步骤
首先如果你是 Ubuntu 14.04 系统，那么请先安装 PPA
以下步骤仅限 Ubuntu 14.04 系统执行

``` bash
apt update
apt install software-properties-common -y
```

通过PPA工具添加WireGuard源

``` bash
add-apt-repository ppa:wireguard/wireguard
apt install wireguard
```

安装WireGuard

``` bash
apt update
apt install wireguard resolvconf -y
```

### 验证是否安装成功

使用以下命令

``` bash
lsmod | grep wireguard
modprobe wireguard && lsmod | grep wireguard
```

输出能看到wireguard即表示安装成功了

``` bash
wireguard             212992  0
ip6_udp_tunnel         16384  1 wireguard
udp_tunnel             16384  1 wireguard
```

### 配置

#### 生成密匙对
请先手动创建配置文件目录

``` bash
mkdir /etc/wireguard
```

然后开始生成 密匙对(公匙+私匙)

``` bash
wg genkey | tee sprivatekey | wg pubkey > spublickey
wg genkey | tee cprivatekey | wg pubkey > cpublickey
```

查看主网卡名称

``` bash
ip addr
```

#### 生成服务端配置文件
* 井号开头的是注释说明，用该命令执行后会自动过滤注释文字。
* 下面加粗的这一大段都是一个代码！请把下面几行全部复制，然后粘贴到 SSH软件中执行，不要一行一行执行！

``` bash
echo "[Interface]
# 服务器的私匙，对应客户端配置中的公匙（自动读取上面刚刚生成的密匙内容）
PrivateKey = $(cat sprivatekey)
# 本机的内网IP地址，一般默认即可，除非和你服务器或客户端设备本地网段冲突
Address = 10.0.0.1/24 
# 运行 WireGuard 时要执行的 iptables 防火墙规则，用于打开NAT转发之类的。
# 如果你的服务器主网卡名称不是 eth0 ，那么请修改下面防火墙规则中最后的 eth0 为你的主网卡名称。
PostUp   = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -A FORWARD -o wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
# 停止 WireGuard 时要执行的 iptables 防火墙规则，用于关闭NAT转发之类的。
# 如果你的服务器主网卡名称不是 eth0 ，那么请修改下面防火墙规则中最后的 eth0 为你的主网卡名称。
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -D FORWARD -o wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
# 服务端监听端口，可以自行修改
ListenPort = 443
# 服务端请求域名解析 DNS
DNS = 8.8.8.8
# 保持默认
MTU = 1420
# [Peer] 代表客户端配置，每增加一段 [Peer] 就是增加一个客户端账号，具体我稍后会写多用户教程。
[Peer]
# 该客户端账号的公匙，对应客户端配置中的私匙（自动读取上面刚刚生成的密匙内容）
PublicKey = $(cat cpublickey)
# 该客户端账号的内网IP地址
AllowedIPs = 10.0.0.2/32"|sed '/^#/d;/^\s*$/d' > wg0.conf

```

#### 生成客户端配置文件
* 井号开头的是注释说明，用该命令执行后会自动过滤注释文字。
* 下面加粗的这一大段都是一个代码！请把下面几行全部复制，然后粘贴到 SSH软件中执行，不要一行一行执行！

``` bash
echo "[Interface]
# 服务器的私匙，对应客户端配置中的公匙（自动读取上面刚刚生成的密匙内容）
PrivateKey = $(cat sprivatekey)
# 本机的内网IP地址，一般默认即可，除非和你服务器或客户端设备本地网段冲突
Address = 10.0.0.1/24 
# 运行 WireGuard 时要执行的 iptables 防火墙规则，用于打开NAT转发之类的。
# 如果你的服务器主网卡名称不是 eth0 ，那么请修改下面防火墙规则中最后的 eth0 为你的主网卡名称。
PostUp   = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -A FORWARD -o wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
# 停止 WireGuard 时要执行的 iptables 防火墙规则，用于关闭NAT转发之类的。
# 如果你的服务器主网卡名称不是 eth0 ，那么请修改下面防火墙规则中最后的 eth0 为你的主网卡名称。
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -D FORWARD -o wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
# 服务端监听端口，可以自行修改
ListenPort = 443
# 服务端请求域名解析 DNS
DNS = 8.8.8.8
# 保持默认
MTU = 1420
# [Peer] 代表客户端配置，每增加一段 [Peer] 就是增加一个客户端账号，具体我稍后会写多用户教程。
[Peer]
# 该客户端账号的公匙，对应客户端配置中的私匙（自动读取上面刚刚生成的密匙内容）
PublicKey = $(cat cpublickey)
# 该客户端账号的内网IP地址
AllowedIPs = 10.0.0.2/32"|sed '/^#/d;/^\s*$/d' > wg0.conf
 
# 上面加粗的这一大段都是一个代码！请把下面几行全部复制，然后粘贴到 SSH软件中执行，不要一行一行执行！
生成客户端配置文件
接下来就开始生成客户端配置文件：

# 井号开头的是注释说明，用该命令执行后会自动过滤注释文字。
# 下面加粗的这一大段都是一个代码！请把下面几行全部复制，然后粘贴到 SSH软件中执行，不要一行一行执行！
 
echo "[Interface]
# 客户端的私匙，对应服务器配置中的客户端公匙（自动读取上面刚刚生成的密匙内容）
PrivateKey = $(cat cprivatekey)
# 客户端的内网IP地址
Address = 10.0.0.2/24
# 解析域名用的DNS
DNS = 8.8.8.8
# 保持默认
MTU = 1420
[Peer]
# 服务器的公匙，对应服务器的私匙（自动读取上面刚刚生成的密匙内容）
PublicKey = $(cat spublickey)
# 服务器地址和端口，下面的 X.X.X.X 记得更换为你的服务器公网IP，端口请填写服务端配置时的监听端口
Endpoint = X.X.X.X:443
# 因为是客户端，所以这个设置为全部IP段即可
AllowedIPs = 0.0.0.0/0, ::0/0
# 保持连接，如果客户端或服务端是 NAT 网络(比如国内大多数家庭宽带没有公网IP，都是NAT)，那么就需要添加这个参数定时链接服务端(单位：秒)，如果你的服务器和你本地都不是 NAT 网络，那么建议不使用该参数（设置为0，或客户端配置文件中删除这行）
PersistentKeepalive = 25"|sed '/^#/d;/^\s*$/d' > client.conf
```

接下来你就可以将这个客户端配置文件/etc/wireguard/client.conf 通过SFTP、HTTP等方式下载到本地了

``` bash
cat /etc/wireguard/client.conf
```

### 剩余其他操作

#### 赋予配置文件夹权限

``` bash
chmod 777 -R /etc/wireguard
```

#### 打开防火墙转发功能

``` bash
echo 1 > /proc/sys/net/ipv4/ip_forward
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p
```

### 启动WireGuard
``` bash
wg-quick up wg0
```
### 停止WireGuard
``` bash
wg-quick down wg0
```
### 查询WireGuard状态
``` bash
wg
```
### 设置开机启动

注意：Ubuntu 14.04 系统默认是没有 systemctl 的，所以无法配置开机启动。

``` bash
systemctl enable wg-quick@wg0
```
### 取消开机启动
``` bash
systemctl disable wg-quick@wg0
```




