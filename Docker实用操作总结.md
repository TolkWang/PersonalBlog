# docker实用操作总结

重要概念: 镜像 容器 仓库

##### Centos7下安装docker(亲测可用)

yum update

yum install -y yum-utils device-mapper-persistent-data lvm2

yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

yum list docker-ce --showduplicates | sort -r

yum install docker-ce-18.03.1.ce

systemctl start docker

##### docker常用命令

docker ps 查看当前正在运行的容器

docker ps -a 查看所有容器的状态

docker start/stop id/name 启动/停止某个容器

docker attach id 进入某个容器(使用exit退出后容器也跟着停止运行)

docker exec -ti id 启动一个伪终端以交互式的方式进入某个容器（使用exit退出后容器不停止运行）

docker images 查看本地镜像

docker rm id/name 删除某个容器

docker rmi id/name 删除某个镜像

docker run --name test -ti ubuntu /bin/bash 复制ubuntu容器并且重命名为test且运行，然后以伪终端交互式方式进入容器，运行bash

docker build -t soar/centos:7.1 . 通过当前目录下的Dockerfile创建一个名为soar/centos:7.1的镜像

docker run -d -p 2222:22 --name test soar/centos:7.1 以镜像soar/centos:7.1创建名为test的容器，并以后台模式运行，并做端口映射到宿主机2222端口，P参数重启容器宿主机端口会发生改变

##### dockerFile文件规则

This my first nginx Dockerfile

Version 1.0

Base images 基础镜像

FROM centos

#MAINTAINER 维护者信息
MAINTAINER tianfeiyu 

#ENV 设置环境变量
ENV PATH /usr/local/nginx/sbin:$PATH

#ADD  文件放在当前目录下，拷过去会自动解压
ADD nginx-1.8.0.tar.gz /usr/local/  
ADD epel-release-latest-7.noarch.rpm /usr/local/  

#RUN 执行以下命令 
RUN rpm -ivh /usr/local/epel-release-latest-7.noarch.rpm
RUN yum install -y wget lftp gcc gcc-c++ make openssl-devel pcre-devel pcre && yum clean all
RUN useradd -s /sbin/nologin -M www

#WORKDIR 相当于cd
WORKDIR /usr/local/nginx-1.8.0 

RUN ./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_ssl_module --with-pcre && make && make install

RUN echo "daemon off;" >> /etc/nginx.conf

#EXPOSE 映射端口
EXPOSE 80

#CMD 运行以下命令
CMD ["nginx"]