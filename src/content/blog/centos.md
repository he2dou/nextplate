---
title: centos 服务器操作系统
date: 2022-04-04T05:00:00Z
author: harry
categories:
  - Technology
tags:
  - centos
---


CentOS是Linux发行版之一，它是来自于Red Hat Enterprise Linux依照开放源代码规定发布所编译而成。


# 第一节 
# centos7 升级内核的方法

默认centos7的内核版本是3.10，更新前，查看内核版本


```
uname -r
```

终端显示：`3.10.0-123.el7.x86_64 1. 2.`

目前最新的内核是3.18.3为stable，是2015年1月18日更新，具体查看 [https://www.kernel.org/](https://www.kernel.org/)

**下面是升级的方法：**

## 1、导入key



```shell
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
```

## 2、安装elrepo的yum源



```shell
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

## 3、安装内核

在yum的ELRepo源中，有mainline（3.18.3）这个内核版本


```shell
yum --enablerepo=elrepo-kernel install kernel-ml-devel kernel-ml -y
```

## 4、重启操作系统



```shell
reboot
```


3.10.0-123.el7.x86_64 1. 2. 重要：目前内核还是默认的版本，如果在这一步完成后你就直接reboot了，重启后使用的内核版本还是默认的3.10，不会使用新的3.18，想修改启动的顺序，需要进行下一步

## 5、查看默认启动顺序



```shell
awk -F' '$1=="menuentry " {print $2}' /etc/grub2.cfg
```

`CentOS Linux (3.18.3-1.el7.elrepo.x86_64) 7 (Core)`

`CentOS Linux, with Linux 3.10.0-123.el7.x86_64`

`CentOS Linux, with Linux 0-rescue-893b160e363b4ec7834719a7f06e67cf 1. 2. 3. 4.`

默认启动的顺序是从0开始，但我们新内核是从头插入（目前位置在0，而3.10的是在1），所以需要选择0，如果想生效最新的内核，需要

## 6、设置内核启动顺序



```shell
grub2-set-default 0
```

## 7、再次重启操作系统

然后reboot重启，使用新的内核，下面是重启后使用的内核版本



```shell
reboot
```

## 8、查看版本

**内核版本**
```shell
uname -r
```

`3.18.3-1.el7.elrepo.x86_64 1. 2.` 完成后内核已经是最新的了。

**系统版本**
```shell
cat /etc/redhat-release
```

CentOS Linux release 7.9.2009 (Core)


## 基础软件安装
```sh
yum update -y
yum install epel-release -y
yum install htop wget lrzsz python-pip -y

```