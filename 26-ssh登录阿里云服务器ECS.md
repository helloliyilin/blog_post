---
title: ssh登录阿里云服务器ECS
date: 2024-01-19 14:21:42
tags: linux
---

## 1. 购买云服务器和配置

首先你要有一台云服务器，可以在阿里云购买一台，第一次使用有免费使用1年的云服务器ECS。

![image-20240119142432043](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119142432043.png)

我已经购买了，直接进入管理控制台。

![image-20240119142512947](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119142512947.png)

一个实列代表一台云服务器。

![image-20240119142728601](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119142728601.png)

蓝色的`i-7xv46utz28q4k2a4nr1v`是你的实例id，该值是唯一的，点击进去查看详细信息。

![image-20240119143106519](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119143106519.png)

重置密码后你就可以远程连接使用了，一般默认进入的是root权限的用户。

![image-20240119143231624](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119143231624.png)

## 2. 远程接入云服务器

### 2.1 使用MobaXterm软件登录

你可以使用任何支持ssh协议的软件远程接入服务器，我这里使用的是MobaXterm。

![image-20240119143601305](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119143601305.png)

![image-20240119143722767](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119143722767.png)

### 2.2 使用wsl2远程接入

> SL2，全称“Windows Subsystem for Linux 2”，是Windows Subsystem for Linux的升级版。它是一种在Windows上运行Linux环境的技术，其特点在于提供了更好的文件系统性能和更完全的Linux系统内核支持。WSL2运用虚拟化技术，在轻量级的虚拟机 (VM) 中运行Linux内核，但同时仍保留了与WSL1相似的操作体验。

其实就是一个在windows操作系统模拟的linux的平台。具体怎么下载安装，自行百度。

使用ssh登录

```
ssh 用户名@公网ip
```



![image-20240119144629955](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119144629955.png)

我们可以在登录的服务器里创建一个普通用户。

```
adduser 用户名
```

创建普通用户完成后，给这个用户设置一个密码，这个密码可以是和root密码一样也可以不一样。

```
passwd 用户名
```

创建完的普通用户的位置在`/home`目录下。

![image-20240119145103362](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119145103362.png)

退出云服务器可以使用命令`exit`，然后我们可以使用ssh+普通用户远程连接云服务器了。

![image-20240119145452488](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240119145452488.png)

