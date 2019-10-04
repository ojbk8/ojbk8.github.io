---
title: "Linux下tar,unzip,7z,rar压缩带密码解压文件文件夹的使用"
date: 2017-10-01T22:31:12-04:00
draft: false
tags: [""]
Keywords: [""]
categories: ["教程"]
comments: false
---


### tar压缩

压缩多个文件或者文件夹的命令

``` bash
tar -czvf a.tar.gz(想压缩文件名) 源文件名1 源文件名2 源文件夹1 源文件夹2
```

参数：

- `c`创建一个压缩文件，如果只使用这个参数，不使用 z 参数，那么只会打包，不会压缩
- `x`解开一个压缩文件
- `z`是否使用 gzip 压缩或解压
- `j`是否使用 bzip2 压缩或解压
- `v`显示详细信息
- `f`指定压缩后的文件名，后面要直接跟文件名，所以将 f 参数放到最后

举几个例子：

将a文件夹打包成a.tar但是不压缩

``` bash
tar cvf a.tar arg
```

将a文件夹打包成a.tar.gz并使用 gzip 压缩

``` bash
tar czvf a.tar.gz arg
```

将a文件夹打包成a.tar.gz并使用bzip2压缩

``` bash
tar cjvf a.tar.bz2 arg
```

### tar解压

Linux下常见的压缩包格式有5种:zip tar.gz tar.bz2 tar.xz tar.Z

其中tar是种打包格式,gz和bz2等后缀才是指代压缩方式:gzip和bzip2

filename.zip的解压:

``` bash
unzip filename.zip
```

filename.tar.gz的解压:

``` bash
tar -zxvf filename.tar.gz
```

其中zxvf含义分别如下

- `z`gzip压缩格式
- `x`extract解压
- `v`verbose详细信息
- `f`file(file=archieve)文件

filename.tar.bz2的解压:

``` bash
tar -jxvf filename.tar.bz2
```

`j`指bzip2压缩格式

其它选项和tar.gz解压含义相同


filename.tar.xz的解压: 

``` bash
tar -Jxvf filename.tar.xz
```

注意`J`大写

 

filename.tar.Z的解压: 

``` bash
tar -Zxvf filename.tar.Z
```

注意`Z`大写

 

关于tar的详细命令可以

``` bash
tar --help
```

事实上, 从1.15版本开始tar就可以自动识别压缩的格式,故不需人为区分压缩格式就能正确解压

``` bash
tar -xvf filename.tar.gz
tar -xvf filename.tar.bz2
tar -xvf filename.tar.xz
tar -xvf filename.tar.Z
```

### 压缩zip解压unzip缩命令

安装

``` bash
apt-get install unzip
```

zip命令

``` bash
zip -r myfile.zip ./*
```

将当前目录下的所有文件和文件夹全部压缩成myfile.zip文件,－r表示递归压缩子目录下所有文件.

unzip命令

``` bash
unzip -o -d /home/sunny myfile.zip
```

把myfile.zip文件解压到 /home/sunny/

- `o`不提示的情况下覆盖文件；
- `d` -d /home/sunny 指明将文件解压缩到/home/sunny目录下；

其他

``` bash
zip -d myfile.zip smart.txt
```

删除压缩文件中smart.txt文件

``` bash
zip -m myfile.zip ./rpm_info.txt
```

向压缩文件中myfile.zip中添加rpm_info.txt文件

``` bash
zip -r filename.zip file1 file2 file3 /usr/work/school 
```

上面的命令把 file1、file2、 file3、以及 /usr/work/school 目录的内容（假设这个目录存在）压缩起来，然后放入 filename.zip 文件中。

解压unzip缩命令

``` bash
unzip filename.zip
```

### 7Z

linux安装7z命令

``` bash
apt-get install p7zip-full
```

带密码压缩

``` bash
7z -p a test.7z test.txt
```

`-p`添加密码

解压：

``` bash
7z x test.7z
```


解压缩7z文件

``` bash
7z x phpMyAdmin-3.3.8.1-all-languages.7z -r -o./
```

最新命令为7zr

参数含义：

1. `x`代表解压缩文件，并且是按原始目录树解压（还有个参数 e 也是解压缩文件，但其会将所有文件都解压到根下，而不是自己原有的文件夹下）
2. `r` 表示递归解压缩所有的子文件夹
3. `o`是指定解压到的目录，-o后是没有空格的，直接接目录。这一点需要注意。

压缩文件/文件夹

``` bash
7z a -r test.7z ./*
```

参数含义：

1. `a`代表添加文件／文件夹到压缩包
2. `t`是指定压缩类型，这里定为7z，可不指定，因为7za默认压缩类型就是7z。
3. `r`表示递归所有的子文件夹
4. `Mytest.7z`是压缩好后的压缩包名
5. `/opt/phpMyAdmin-3.3.8.1-all-languages/*`是压缩目标。

注意：7za不仅仅支持.7z压缩格式，还支持.tar.bz2等压缩类型的。如上所述，用-t指定即可。



### unrar

[RAR下载地址](http://www.rarsoft.com/download.htm) 64系统

安装unrar,rar

``` bash
wget https://www.rarlab.com/rar/rarlinux-x64-5.8.b2.tar.gz
tar xvf rarlinux-x64-5.8.b2.tar.gz
cd rar
cp rar /usr/local/rar
cp unrar /usr/local/unrar
ln -s /usr/local/rar /usr/local/bin/rar
ln -s /usr/local/unrar /usr/local/bin/unrar
```

安装完毕后就有了RAR和UNRAR这两个程序

解压有密码的rar压缩包

``` bash
unrar e -p filename.rar
```

输入密码后解压一个名为filename.rar的带密码压缩包

1. unrar v test.rar    #查看压缩文件中的文件
2. unrar x test.rar /tmp   #解压到指定文件夹
3. unrar e test.rar   #解压到当前文件夹

``` bash
rar -p a test.rar test.txt
```

带密码压缩会提示输入两次密码。

解压：

``` bash
unrar x test.rar
```

会提示输入密码。
