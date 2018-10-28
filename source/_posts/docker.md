---

title: docker
date: 2018-10-25 22:50:21
categories: "大数据"
tags:
    - docker

description: docker学习

---


# docker
容器是镜像的运行状态
比如 启动nginx 将端口映射到本机8080 -d为立即返回，采用守护线程方式执行
docker run -p 8080:80 -d nuginx
将
docker cp file nginxID://usr/share/nginx/html容器位置

要永久保存，需提交
docker commit -m 'xxx' nginxID  新image名字  会保存为一个新的image
进入交互式容器环境
docker exec -it containerID /bin/bash

查看正在运行的容器
docker ps
删除镜像
docker rmi  imageID                  docker rm 删除运行结束的容器、历史
停止
docker stop containerID


获取镜像   docker run 会自动获取
docker build  更加dockerfile 创建image
docker images 列出image
docker run 运行容器


# dickerfile
from alpine:latest
...
CMD  echo "dfasfdasf"

docker buile -t "镜像名字" .

FROM  :  基础镜像 
RUN   ： 执行命令
ADD    ： 条件文件  可以添加ftp
COPY   ：  拷贝文件
CMD   ：   执行命令 如果没有ENTRYPOINT 则就是入口命令，否则作为ENTRYPOINT的参数
EXPOSE    ：  暴露端口
WORKDIR :    指定路径
MAINTAINER  ： 维护者
ENV：    设定环境变量
ENTRYPOINT:  容器入口
USER    :   指定用户
VOLUME  :    mount point
例子：

FROM ubuntu 
MAINTAINER TUWEI
RUN apt-get update
RUN apt-get install -y nginx
COPY index.html /var/www/html
ENTRYPONT ["/usr/sbin/nginx", "-g" ,"daemon off"]
EXPOSE 80

# 本地目录 映射到 容器的目录  一旦映射了 两边的文件修改便同步了
docker run -p 80:80 -d -v $PWD/html:/user/share/nginx/html nginx
将当期目录下的html目录映射到容器 里的nginx的html目录


创建容器 基础镜像为ubuntu
docker create -v $PWD/data:/var/mydata --name data_container ubuntu 

在启动一个容器
docker run -it --volume-from data_container ubuntu /bin/bash

现在 本地的 $PWD/data -> /var/mydata 而 /var/mydata 又被另一个容器所挂载

# 镜像仓库
国内仓库： daocloud 阿里





