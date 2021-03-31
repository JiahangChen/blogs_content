---
title: Docker
date: 2021-03-31 16:14:31
tags:
---

# Docker Learning

### 总结一下最近关于docker的内容。

我们可以先简单的把docker理解为一个简单的虚拟机，虚拟机即系统底下的子系统。
虚拟机允许你在当前系统下跑一个新的环境，可以把环境变量等各种配置隔离开来。但是需要给他额外分配一些内存和一块硬盘空间。

docker相较于虚拟机有以下几个优点：
* 不用特地给某个docker分配内存，但是可以设置一个内存使用限制，多数情况下内存可以共享，不会形成资源的浪费
* 适合分发软件，系统，配置和依赖包可以一同打包成镜像发放给用户，用户打开docker创建容器后即开即用。
* 系统相较于虚拟机来说小的多，只安装了依赖的包

docker使用中的坑也不少
我们以在window环境下为例，在window10最新版本下已经支持wsl2的子系统技术，docker里也集成来这个技术。所以我们可以先启动win10 wsl2模块再在商店里安装centos的子系统，这样docker就可以直接跑在这个子系统上了。

docker的使用方法非常简单，无非是**拉下一个镜像，通过镜像生成容器，使用容器**。当然你可以**将容器变成镜像，上传到网上给别人用**

以下是一些docker常用的代码：

`docker pull centos:7`

直接从网上进行一个镜像的pull

`docker run -itd --name centos_content -p 80:80 --privileged centos:7 init`

对于某一个镜像创建一个容器，--name可以给容易命名，-p可以进行端口转发，--privileged可以解决一些docker安装时的权限问题，init可以在docker中使用systemctl启动docker的服务

`docker exec -it centos_content /bin/bash`

进入容器，使用/bin/bash的bash，也可以使用/bin/sh

`docker start centos_content`

启动容器

`docker stop centos_content`

停止容器

`docker commit centos_content centos_my_image`

将当前的容器制作成镜像

`docker save -o /path/centos_my_image.tar centos_my_image`

把镜像保存到本地

`tar -zcvf /path/centos_my_image.tar.gz /path/centos_my_image.tar`

把镜像压缩

`tar -zxvf centos_my_image.tar.gz`

解压缩镜像

`docker load < centos_my_image.tar`

导入镜像


在使用win wsl2 的时候由于wsl2的默认目录在c盘所以一直导致c盘爆满的情况，这个方法可以把镜像和容器内容移动到D盘

`wsl -- shutdown`

停止wsl

`wsl -l -v -all`

显示目前在wsl中的子系统

`wsl --export docker-desktop-data D:\wsl\docker\tmp\docker-desktop-data.tar`

到处目前的容器和镜像到D盘并打包

`wsl --unregister docker-desktop-data`

解绑目前的docker数据

`wsl --import docker-desktop-data D:\wsl\docker\data\ D:\wsl\docker\tmp\docker-desktop-data.tar --version 2`

导入新目录下的数据，并设置目录在新目录

`D:\wsl\docker\tmp\docker-desktop-data.tar`

删除tar文件并重启

