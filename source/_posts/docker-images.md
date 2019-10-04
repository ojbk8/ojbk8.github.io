---
title: "docker创建删除使用容器或镜像"
date: 2018-07-25T00:00:00+08:00
lastmod: 2018-07-25T00:00:00+08:00
draft: false
tags: ["docker"]
categories: ["教程"]
comments: false
---

*更多容器可以访问[docker](https://www.docker.com/)的[Docker Hub](https://hub.docker.com/)*

操作系统内核大于等于 3.10 的都可以安装最新版 Docker 查看内核版本

``` bash
uname -r
```

### 安装docker
``` bash
wget -qO- get.docker.com | bash 
```

安装完成后，运行下面的命令，验证是否安装成功。
``` bash
docker version 
```

<!--more-->

启动 Docker
``` bash
systemctl start docker 
```
查看 Docker 启动状态
``` bash
systemctl status docker
```
允许 Docker 开机自启
``` bash
systemctl enable docker 
```

解决`docker-compose: command not found`的方法

您还需要安装Docker Compose。见 [手册](https://docs.docker.com/compose/install/)。以下是您需要执行的命令

```
curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` > ./docker-compose
sudo mv ./docker-compose /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```

### Docker 清理命令
杀死所有正在运行的容器.
``` bash
alias dockerkill='docker kill $(docker ps -a -q)'
```

删除所有已经停止的容器.

``` bash
alias dockercleanc='docker rm $(docker ps -a -q)'
```

删除所有未打标签的镜像.

``` bash
alias dockercleani='docker rmi $(docker images -q -f dangling=true)'
```

删除所有已经停止的容器和未打标签的镜像.

``` bash
alias dockerclean='dockercleanc || true && dockercleani'
```

删除所有未运行 Docker 容器
docker rm $(docker ps -a -q)
删除所有 Docker 镜像
删除所有未打 tag 的镜像

docker rmi $(docker images -q | awk '/^<none>/ { print $3 }')
删除所有镜像

docker rmi $(docker images -q)
根据格式删除所有镜像

docker rm $(docker ps -qf status=exited)


docker images
docker tag IMAGE_ID  <hub-user>/<repo-name>[:<tag>]
docker login
#docker push 镜像到docker hub 的仓库
docker push <hub-user>/<repo-name>:<tag>
docker images
镜像的保存
docker save IMAGE_ID > newname.tar
镜像的导入
docker load < tnewname.tar

保存对容器的修改，示范
docker commit cb439fb2c714 mxnet/python:gpu

停用全部运行中的容器:

```
docker stop $(docker ps -q)
```

删除全部容器：

```
docker rm $(docker ps -aq)
```


一条命令实现停用并删除容器：

``` bash
docker stop $(docker ps -q) & docker rm $(docker ps -aq)
```

``` bash
docker system prune -y
docker image prune --force --all -y
```

更新docker 1.13 中增加了 `docker system prune`的命令，针对container、image可以使用`docker container prune`、`docker image prune`命令。

- `docker image prune --force --all`或者`docker image prune -f -a` : 删除所有不使用的镜像
- `docker container prune -f`: 删除所有停止的容器


docker中如何删除image（镜像）

``` bash
docker images
docker ps -a
```

查看docker的帮助会发现有两个与删除有关的命令rm和rmi

``` bash
rm Remove one or more containers
rmi Remove one or more images
```


``` bash
docker rmi ID
```


``` bash
docker pull ubuntu
docker run -t -i ubuntu /bin/bash
```

### 使用示例
1. nginx

``` bash
docker pull nginx
docker run --name some-nginx -d -p 8080:80 some-content-nginx
```

Then you can hit http://localhost:8080 or http://host-ip:8080 in your browser.


2. shadowsocks-libev-teddysun

``` bash
docker pull teddysun/shadowsocks-libev:alpine
```

在宿主机上创建配置文件
``` bash
echo "{
    "server":"0.0.0.0",
    "server_port":9000,
    "password":"password0",
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open":true,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp",
    "plugin":"obfs-server",
    "plugin_opts":"obfs=tls"
} " > /etc/shadowsocks-libev/config.json
```

启动容器

``` bash
docker run -d -p 9000:9000 -p 9000:9000/udp --name ss-libev -v /etc/shadowsocks-libev:/etc/shadowsocks-libev teddysun/shadowsocks-libev 
```

介绍

- docker run：开始运行一个容器。
- -d 参数：容器以后台运行并输出容器 ID。
- -p 参数：容器的 9000 端口映射到本机的 9000 端口。默认是映射 TCP，当需要映射 UDP 时，那就再追加一次 UDP 的映射。冒号后面是容器端口，冒号前面是宿主机端口，可以写成一致，也可以不一致。
- –name 参数：给容器分配一个识别符，方便将来的启动，停止，删除等操作。
- -v 参数：挂载卷(volume)，冒号后面是容器的路径，冒号前面是宿主机的路径，可以写成一致，也可以不一致。
- teddysun/shadowsocks-libev：这是拉取回来的镜像路径。
