---
layout:     post
title:      "Docker-Nginx-SpringBoot"
subtitle:   ""
date:       2017-07-01
author:     "Will"
header-img: "img/docker/docker.jpg"
catalog: true
tags:
    - Docker
    - Nginx
    - Spring
---

> “walk beside you ”


## 前言 

　　有意义就是好好活，好好活就是做很多很多有意义的事 。
                                ------  许三多

---

## 正文

　　前面弄过Docker，弄过SpringBoot、这次准备在自己的服务器上把自己的SpringBoot程序用Docker部署起来，并使用Nginx做代理。　　

## 制作Docker镜像

　　制作Docker镜像有两种方式:
   1. 使用**docker commit** 命令来制作镜像
   * 首先**docker run** 来启动一个镜像
   * 然后在这个镜像基础上添加自己的应用
   * 使用**docker commit**提交修改后的镜像

   2. 使用**Dockerfile** 制作镜像
   * 需要创建一个描述镜像的Dockerfile文件，通过此文件来生产新的镜像，例如：

   ```
    RUN apk add --no-cache tzdata \
    	#设置时区
    	&& echo "${TIME_ZONE}" > /etc/timezone \
    	&& ln -sf /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime \
    # 依赖镜像，在此镜像基础上实现新的镜像
    # 这里使用 jdk 8
    FROM frolvlad/alpine-oraclejdk8:slim

    # 维护者信息，名字和邮箱地址
    MAINTAINER will <wuxiao@smallfat.cn>

    # 镜像的一个简单说明，Version指明该镜像的版本号，方便维护；
    LABEL Description="My SpringBoot Image."  Version="1.0"

    # VOLUME命令用于让你的容器访问宿主机上的目录。
    # VOLUME /tmp

    # ADD命令有两个参数：源和目标。
    # 它的基本作用是从源系统的文件系统上复制文件到目标容器的文件系统。
    # 如果源是一个URL，那该URL的内容将被载并复制到容器中。
    ADD springBoot_001-1.0-SNAPSHOT.jar springBoot.jar

    # ENV命令用于设置环境变量。这些变量以”key=value”的形式存在，并可以在容器内被脚本或者程序调用。
    # ENV JAVA_OPTS=""
    ENV  TIME_ZONE=Asia/Shanghai

    # RUN指令告诉docker在镜像内执行命令
    #设置时区
    RUN ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone

    RUN sh -c 'touch /springBoot.jar'

    # ENTRYPOINT 帮助你配置一个容器使之可执行化
    ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /springBoot.jar" ]

   ```

   * 把程序的jar包与Dockerfile文件放到同一目录下，然后执行   ``` docker build -t myspringbot:1.0 .``` 主意后面有个点

   ```
   [docker@iZj6c7kcgwxvww1z95n59eZ springboot]$ docker build -t myspringbot:1.0 .
   Sending build context to Docker daemon 14.12 MB
   Step 1 : FROM frolvlad/alpine-oraclejdk8:slim
    ---> 491f45037124
   Step 2 : MAINTAINER will <wuxiao@smallfat.cn>
    ---> Using cache
    ---> 8315ac604f55
   Step 3 : LABEL Description "My SpringBoot Image." Version "1.0"
    ---> Using cache
    ---> dca65c6e488b
   Step 4 : ADD springBoot_001-1.0-SNAPSHOT.jar springBoot.jar
    ---> Using cache
    ---> eddaa5c1bef6
   Step 5 : ENV TIME_ZONE Asia/Shanghai
    ---> Using cache
    ---> 589ff3f8934a
   Step 6 : RUN ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone
    ---> Running in de540a05a30d
    ---> d966df8a6a7e
   Removing intermediate container de540a05a30d
   Step 7 : RUN sh -c 'touch /springBoot.jar'
    ---> Running in 775c5f207238
    ---> 2c19958adbdd
   Removing intermediate container 775c5f207238
   Step 8 : ENTRYPOINT sh -c java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /springBoot.jar
    ---> Running in b0423a03d3d3
    ---> 200bba26816c
   Removing intermediate container b0423a03d3d3
   Successfully built 200bba26816c
   ```

   * 执行命令 ```docker images``` 会看到新建的镜像
   * 执行命令 ```docker run -p 9091:9091 --name myspringbot  -d  myspringbot:1.0```

## 安装Nginx

　　这里使用Docker的Nginx的镜像来安装Nginx
   * 执行命令 ```docker pull nginx```
   * 执行命令

   ```
    docker run -p 80:80 -p 443:443 --name nginx -v /home/docker/nginx/data:/usr/share/nginx -v /home/docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /home/docker/nginx/conf/conf.d:/etc/nginx/conf.d -v /home/docker/nginx/conf/cert:/etc/nginx/cert -v /home/docker/nginx/logs:/var/log/nginx  -d nginx

   ```

　　这里把 **80**、**443** 分别映射到主机的 **80**、**443** 。
把Ngibx的数据文件 **/usr/share/nginx**、配置文件 **/etc/nginx/nginx.conf**,**/etc/nginx/conf.d**、
证书文件 **/etc/nginx/cert** 、日志文件 **/var/log/nginx**  分别映射到主机对应的目录下（按照需要来设置就行）
   * 在Nginx配置文件中添加

   ```
    location ^~ /home/ {
            proxy_pass http://127.0.0.1:9091;
        }

   ```

   * 重启nginx镜像  ``` docker  restart nginx ```
   * 此时我们在访问nginx地址后面加上 **/home** 即可访问到我们的SpringBoot

---

## 后记

　　Docker、SpringBoot、Nginx 都是微服务中重要的组成部分，这里就简单的实现下他们之间使用。

　　

