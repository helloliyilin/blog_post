---
title: docket常用命令以及使用
date: 2023-11-20 17:35:40
tags: 教程
---

## 1. Docker的三个概念

1. 镜像（Image）：类似于虚拟机中的镜像，是一个包含有文件系统的面向Docker引擎的只读模板。任何应用程序运行都需要环境，而镜像就是用来提供这种运行环境的。例如一个Ubuntu镜像就是一个包含Ubuntu操作系统环境的模板，同理在该镜像上装上Apache软件，就可以称为Apache镜像。
2. 容器（Container）：类似于一个轻量级的沙盒，可以将其看作一个极简的Linux系统环境（包括root权限、进程空间、用户空间和网络空间等），以及运行在其中的应用程序。Docker引擎利用容器来运行、隔离各个应用。容器是镜像创建的应用实例，可以创建、启动、停止、删除容器，各个容器之间是是相互隔离的，互不影响。注意：镜像本身是只读的，容器从镜像启动时，Docker在镜像的上层创建一个可写层，镜像本身不变。
3. 仓库（Repository）：类似于代码仓库，这里是镜像仓库，是Docker用来集中存放镜像文件的地方。注意与注册服务器（Registry）的区别：注册服务器是存放仓库的地方，一般会有多个仓库；而仓库是存放镜像的地方，一般每个仓库存放一类镜像，每个镜像利用tag进行区分，比如Ubuntu仓库存放有多个版本（12.04、14.04等）的Ubuntu镜像。

## 2. Docker的安装和卸载

1. 在linux的Ubuntu 发行版安装：

对于非 Gnome 桌面环境，必须安装：`gnome-terminal`

```
sudo apt install gnome-terminal
```

卸载适用于 Linux 的 Docker Desktop 的技术预览版或测试版。跑：

```
sudo apt remove docker-desktop
```

在 Ubuntu 上安装 Docker Desktop 的推荐方法：

```
sudo apt-get update
sudo apt-get install ./docker-desktop-<version>-<arch>.deb
```

2.  在 Windows 上安装 Docker Desktop

+ win10我们建议使用家庭版或专业版 22H2（内部版本 19045）或更高版本，或者企业版或教育版 22H2（内部版号 19045）或更高版本。

+  Windows 上打开 WSL 2 功能
+ 在 BIOS 中启用硬件虚拟化。

## 3. Docker中关于镜像的基本操作

我们从官方注册服务器（[https://hub.docker.com](https://link.zhihu.com/?target=https%3A//hub.docker.com)）的仓库中pull下ubuntu的镜像，前边说过，每个仓库会有多个镜像，用tag标示，如果不加tag，默认使用latest镜像。

```
docker search ubuntu    # 查看ubuntu镜像是否存在
docker pull ubuntu      # 利用pull命令获取镜像
docker images			# 查看当前系统中的images信息
```

以上是下载一个已有镜像，此外有两种方法可以帮助你新建自有镜像。

（1）利用镜像启动一个容器后进行修改 ==> 利用commit提交更新后的副本.

```
docker run -it ubunu:latest /bin/bash    # 启动一个容器
git --version    		# 此时的容器中没有git
bash: git: command not found
apt-get update
apt install git		    # 利用apt安装git
git --version   		# 此时的容器中已经装有git了
```

此时利用exit退出该容器，然后查看docker中运行的程序（容器）：

```
docker ps -a
```

这里将容器转化为一个镜像，即执行commit操作，完成后可使用docker images查看:其中`72f1a8a0e394`是容器的id号，`helloliyilin/ubuntu`是镜像的名称，`git`是镜像的标识。

```
docker commit -m "ubuntu with git"  72f1a8a0e394 helloliyilin/ubuntu:git
```

```
[root@xxx ~]# docker images
REPOSITORY              TAG      IMAGE ID         	CREATED              SIZE
helloliyilin/ubuntu     git      52166e4475ed        5 seconds ago       358.1 MB
ubuntu                  latest   0584b3d2cf6d        9 days ago          196.5 MB
```

此时Docker引擎中就有了我们新建的镜像helloliyilin/ubuntu:git，此镜像和原有的Ubuntu镜像区别在于多了个Git工具。此时我们利用新镜像创建的容器，本身就自带git了。

