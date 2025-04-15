---
title: Docker
tags:
  - docker
date: 2025-01-02 10:37:11
categories:
  - 运维
cover: https://s2.loli.net/2025/01/02/FuPTHsQByR49dJe.webp
---

# Docker

## 1.简介

> 官方介绍
>
> **一个为增强开发而设计的强大平台**
>
> [Docker][docker] 的一系列集成工具为构建、安全管理和部署容器化应用程序提供了一站式解决方案。快速的本地开发、安全的镜像管理以及基于云的构建——这一切都包含在一个为现代软件开发设计的平台中。
>
> **总之，docker可以帮我们快速部署项目**

## 2.安装

### 1.卸载旧版

```sh
sudo yum remove docker \
        docker-client \
        docker-client-latest \
        docker-common \
        docker-latest \
        docker-latest-logrotate \
        docker-logrotate \
        docker-engine \
        docker-selinux 
```

### 2.设置仓库

安装工具

```Bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

添加docker的官方仓库

```sh
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### 3.安装docker

```sh
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

### 4.启动

启动docker

```sh
sudo systemctl start docker
```

设置开机自启

```sh
sudo systemctl enable docker
```

### 5.镜像加速

> [!warning]
>
> 目前国内一些大厂和大学的docker镜像加速由于某种不知名原因基本全部失效，想要配置镜像加速可以自行搜索国内任然可用的加速网站，`vim /etc/docker/daemon.json`
>
> ```json
> {
>     "registry-mirrors": [
>         "https://docker.1panel.dev"
>     ]
> }
> ```

## 3.快速开始

### 1.相关概念

#### 镜像

> **镜像（Image）**是一个**只读**的模板，包含了运行应用程序所需的**所有文件和配置**，镜像**类似于虚拟机的镜像**，是一个独立的文件系统，包括运行容器所需的数据，可以用来创建新的容器

#### 容器

> 容器是**镜像的运行实例**，类似于虚拟机，可以执行启动、停止、删除等操作。每个容器间是相互隔离的。
>
> 容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的命名空间。 

简单来说，**镜像运行起来就是容器**

### 2.相关命令

#### 命令图解

![image-20250102200533881](https://s2.loli.net/2025/01/02/ZqHkfNtBgUeM7CG.png)

#### run

```sh
docker run -d \
--name mysql-container \
-e MYSQL_ROOT_PASSWORD=12345 \
-p 3306:3306 \
mysql
```

> 命令解释：
>
> `docker run`：通过镜像创建并启动一个新的容器，如果没有镜像，会自动去dockerhub下载对应镜像
>
> `-d`：以守护进程模式（后台模式）启动容器，容器在**后台运行**，不会阻塞当前的命令行
>
> `--name mysql-container`：指定容器的名称为 `mysql-container`，可以在后续操作中通过该名称引用容器
>
> `-e MYSQL_ROOT_PASSWORD=root_password`：通过环境变量 `MYSQL_ROOT_PASSWORD` 设置 MySQL 的根用户密码为 `root_password`，不同的镜像有不同的环境变量
>
> `-p 3306:3306`：端口映射，<span alt='highlight'>-p 宿主机端口:容器端口</span>，因为容器是隔离的，运行起来的mysql服务无法从外部访问，将容器端口映射到宿主机端口，访问宿主机端口3306就能访问容器中的mysql，
>
> > [!warning] 
> >
> > 一个宿主机端口只能一个容器映射，如果运行了多个mysql容器，需要映射到其他端口 `-p 3307:3306`
>
> `mysql`：镜像名，指明你要部署什么容器(mysql、redis、nginx等)，完整写法为<span alt='highlight'>image:tag</span>，例如 `mysql:5.7`，指明使用**哪个镜像**的**哪个版本**，如果版本省略不写，默认使用最新版本，相当于`mysql:latest`

---

#### pull

从 docker hub 拉取镜像到本地

```sh
docker pull nginx
```

---

#### images

```sh
docker images
```

查看本地下载的镜像

![image-20250102183848123](https://s2.loli.net/2025/01/02/JTPsKbq3aNwgGfI.png)

---

#### rmi

删除镜像，注意：如果有容器依赖该镜像正在运行，则不能直接删除

```sh
docker rmi 
```

![image-20250102184000899](https://s2.loli.net/2025/01/02/bxCi4DUZ6Y2MuVv.png)

---

#### rm

```sh
docker rm my-nginx
```

删除容器，想删除运行中的容器添加 `-f` 参数强制删除

![image-20250102201857519](https://s2.loli.net/2025/01/02/bXAHBIEW958Y42f.png)

---

#### save

保存镜像到压缩文件，`-o nginx.tar` 表示输出到 nginx.tar 压缩包 

```sh
docker save nginx -o nginx.tar
```

![image-20250102184435900](https://s2.loli.net/2025/01/02/L6g4epQc8quJG3t.png)

---

#### load

从压缩包加载成镜像文件 `-i nginx.tar` 表示从 nginx.tar 文件读入

```sh
docker load -i nginx.tar
```

![image-20250102184900123](https://s2.loli.net/2025/01/02/VReKLHP5SzcnBgx.png)

---

#### ps

查看正在运行的容器，添加 `-a` 参数查看所有容器

![image-20250102190630137](https://s2.loli.net/2025/01/02/FLZOq5Su6REawCG.png)

---

#### start

启动容器，`restart`命令用于重启容器，也可以启动容器

![image-20250102194425302](https://s2.loli.net/2025/01/02/ZROaUfXv1HqgLr8.png)

---

#### stop

停止容器

![image-20250102194606100](https://s2.loli.net/2025/01/02/kXECV3pyuGl7WjL.png)

---

#### logs

```sh
docker logs mysql
```

查看容器的日志，添加 `-f` 参数可以持续监听日志

---

#### exec

```sh
docker exec -it mysql bash  		# 进入运行mysql的容器并打开一个控制台
docker exec -it redis redis-cli 	# 进入运行redis的容器并打开redis-cli命令行
```

进入容器执行命令，一个容器类似于一个linux系统，第二个参数指明**使用什么命令进行初始化**

![image-20250102200202814](https://s2.loli.net/2025/01/02/qJIHTfatDchXy7A.png)

---

### 4.数据卷

#### 1.概述

> **Docker 数据卷（Volumes）** 是一种持久化存储机制，用于保存和共享容器中的数据。挂载数据卷的主要目的是确保容器重启或重新创建时，数据不会丢失。使用数据卷，你可以将容器内部的数据存储在宿主机上或共享给多个容器使用。
>
> **将容器中的数据、配置等文件夹挂载到宿主机上某个文件夹，容器中并不保存数据和配置**
>
> 在容器中直接修改文件**很不方便**，尽管容器模拟的是一个linux系统，但是为了使体积更小一般只提供容器所需要的环境（甚至vi命令都没有），利用数据卷挂载，我们可以在宿主机上**方便地修改文件**，容器会使用宿主机上的文件

#### 2.挂载

使用`docker run`命令运行容器时，添加 `-v [宿主机目录/路径]:[容器内目录]` 参数使用数据卷挂载

- 容器内目录可以根据对应的镜像官方文档进行配置

- 宿主机目录可以自己指定
  - 数据卷挂载：**只指定目录，则文件夹会被放在 `/var/lib/docker/volumes/{目录}/_data` 下**
  - 绑定挂载：如果指定了具体路径，则使用你指定的路径 

只指定了目录，最终宿主机目录路径为 `/var/lib/docker/volumes/html/_data`

```sh
docker run -d  --name nginx -p 80:80 -v html:/usr/share/nginx/html nginx  
```

指定路径，最终宿主机目录即为你指定的目录

```sh 
docker run -d  --name nginx -p 80:80 -v /path/to/html:/usr/share/nginx/html nginx 
```

- `docker volume ls`: 查看所有数据卷
- `docker volume rm [数据卷]` : 删除数据卷
- `docker inspect [数据卷]`: 查看数据卷信息
- `docker volume prune`: 删除所有未使用的数据卷

> 下图查看了挂载的html文件夹路径，并在宿主机上修改，修改后的服务器页面内容发生变化，因为容器使用的就是宿主机上的文件

![image-20250102223323827](https://s2.loli.net/2025/01/02/JEuG85KVzwvFj1k.png)

示例：对mysql配置、数据、初始化目录分别挂载

```sh 
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -v /root/mysql/data:/var/lib/mysql \
  -v /root/mysql/conf:/etc/mysql/conf.d \
  -v /root/mysql/init:/docker-entrypoint-initdb.d \
  -e MYSQL_ROOT_PASSWORD=root \
  -e TZ="Asia/Shanghai" \
  mysql
```

> [!important]
>
> 这样做有什么用？挂载到宿主机上的目录可以持久化，即使容器被删除了，只要重新创建一个容器，同样挂载这三个文件夹，这个新的容器就能立刻恢复为之前的mysql数据库的状态



[docker]:https://docs.docker.com/
