       		
#	Docker安装配置
####Windows下Docker安装
1)	、下载Docker for Windows，按照指示安装（docker version命令查看docker版本）
2)	、Hyper-V：如果没有开启Hyper-V功能，可以在“启动或关闭windows功能”中添加
3)	、设置国内镜像仓库：Setting—Daemon----Registry mirrors(http://0522f34d.m.daocloud.io)

####	Linux下Docker安装（Ubantu 14.04）
1)	sudo apt-get update
2)	sudo apt-get install apt-transport-https ca-certificates
3)	sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
4)	编辑 /etc/apt/sources.list.d/docker.list 文件，添加 deb https://apt.dockerproject.org/repo ubuntu-trusty main
5)	sudo apt-get update
6)	sudo apt-get purge lxc-docker
7)	apt-cache policy docker-engine
8)	apt-get upgrade
9)	sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
10)	sudo apt-get install docker-engine
（这些命令还没实验过）
运行sudo service docker start 启动Docker守护进程，运行docker version 查看Docker版本
启动第一个Docker容器（docker  run hello world）
####	Docker基础命令
1.	docker pull:从仓库中拖镜像
2.	docker run:运行容器，如果当前要运行的容器对应的镜像不存在，会自动拉取
3.	docker stop：停止容器运行
4.	docker start：开始容器运行
5.	docker commit:提交容器到镜像
6.	docker images:查看当前有的镜像
7.	docker ps：查看当前启动的容器
8.	docker build:创建镜像
9.	docker load:加载镜像
10.	更多命令见附件（复制的）
三、	Docker镜像
  镜像(image)是动态的容器的静态表示（specification），包括容器所要运行的应用代码以及运行时的配置。镜像类似于虚拟机镜像，可以将它理解为一个面向Docker的只读模板
1.	创建镜像
1)	使用Dockerfile指令来创建一个新的镜像
在使用Dockerfile创建容器之前，需要先准备一个Dockerfile文件，然后运行docker build命令创建镜像

     # This dockerfile uses the Ubuntu image
     # VERSION 2
     # Author: docker_user
     # Command format: Instruction [arguments / command] …

    # 第一行必须指定基于的容器镜像
    FROM ubuntu:14.04
    # 维护者信息
    MAINTAINER docker_user docker_user@email.com

    # 镜像的操作指令
    RUN apt-get update

    RUN apt-get -y install ntp

    EXPOSE 5555
    # 容器启动时执行指令
    CMD ["/usr/sbin/ntpd"]

2)	、从已经创建的容器中更新镜像，并且提交这个镜像
docker commit –m “create images” –a “author info” old_id  new_name  
-m加一些改动信息 –a 指定作者相关信息，old_id  为旧容器ID ，new_name 新镜像的名字 
2.	搜索镜像 docker search [-s] IMAGE
3.	下载镜像 docker pull [OPTIONS] NAME [:TAG|@DIGEST] 
4.	查看已下载的镜像 docker images [-a]
5.	给镜像添加标签 docker tag [OPTIONS] IMAGE [:TAG] [REGISTRYHOST/][USERNAME/] NAME [:TAG]
6.	删除镜像 docker rmi [OPTIONS] IMAGE [IMAGE…]
     #按标签删除：多个标签，仅会删除当前标签，不会删除镜像
     #按ID删除：直接删除镜像
--f,--force 强制删除镜像
--no-prune 不删除untaged parents
7.	导入镜像 docker load –input Ubuntu_latest.atr或docker load<Ubuntu_laest.tar
--i,--input 从压缩包载入镜像