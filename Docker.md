#  Docker

## Introduction

* missing files

* software version mismatch,

* different configuration settings

  Docker offers dependencies in an isolated environment
  
  

---



## Virtue Machines *vs* Container

**Container**: An isolated environment for running an application 

+ **lightweight  **

+ **use the OS of the host (share kernel)**



___



## Component

### Image

* A cut-down OS
* A runtime environment
* Application files
* Third-party libraries
* Environment variables

image -> docker hub -> test/produce

---

###  Container

+  An isolated and runtime instance of an image
+  Lightweight, portable, and can be easily moved between different environments

---

### Repository

+ Where Docker images are stored and managed
+ Docker Hub is the default public repository
+ Repositories allow versioning, tagging, and organizing images

![初识Docker](C:\Users\18217\Desktop\notes\01.png)





---



## Docker run

### 底层原理

Client-Server 结构， Docker守护进程运行在主机，通过Socket从客户端访问

Docker利用宿主机内核，OS, VM是Guest OS































---

## Useful Command

###  Linux

+ uname -r  查看系统内核版本

+ cat /etc/os-release 查看操作系统

### 帮助命令

+ docker version
+ docker info
+ docker images
+ docker 命令 --help

### 镜像命令

+ docker images
  + --all	-a				Show all images (default hides intermediate images)
  + --quite    -q          只显示镜像id
+ docker search 搜索镜像
  + --filter=STARS=3000   搜索3000星以上的
+ docker pull    下载镜像
  + 

### 容器命令

+ 

















### Dockerfile

`FROM node:alpine`

`COPY . /app`

`WORKDIR /app`

`CMD  node hello_docker.js`

### 

+ 











 
