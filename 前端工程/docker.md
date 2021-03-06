## 介绍
* [https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html]
* docker在隔离的容器中运行进程，容器是在主机上运行的进程。主机可以是本地的也可以是远程的。
* 运行的容器进程是隔离的，因为它具有自己的文件系统，网络以及和主机分离的自己的隔离进程树

## docker
* Docker属于linux容器的一种封装，提供简单易用的容器使用接口。
* Docker将应用程序和程序的依赖大宝在一个文件里面，运行这个文件就会生成一个模拟容器。
* 程序在这个模拟容器里面运行就像在真实的物理机上运行一样。

## docker的用途
1. 提供一次性的环境，例如本地测试他人的软件，持续集成的时候提供单元测试和构建的环境。
2. 提供弹性的云服务，因为docker容器可以随开随关，适合动态扩容和缩容。
3. 组建微服务架构。通过多个容器，一台机器可以跑多个服务，所以可以模拟出微服务架构。

## image镜像
* Docker把应用程序及其依赖，打包在image镜像文件中，只有通过镜像文件，才能生成Docker容器。image文件可以看作是容器的模板。
* Docker根据image文件生成容器的实例，同一个image文件，可以生成多个同时运行的容器实例。
---
* image镜像文件是二进制文件。在实际开发中，一个image文件往往通过继承另一个image文件，加上一些个性化设置而生成。
* image文件是通用的，一台机器的image文件可以拷贝到另一个机器上，照样可以使用。

## 容器文件
* image文件生成的容器实例，本身也是一个文件，称为容器文件。
* 容器文件可以暂停，可以删除。但是暂停运行的容器文件还是会占据硬盘空间

## 流程
* 对于前端来说，使用docker的主要步骤是：
* （先创建一个vue项目hello-world,端口采用vue默认端口8080）
1、编写Dockerfile文件
````html
FROM node:14.16.0
WORKDIR /app
COPY ["package.json", "package-lock.json*", "./"]
RUN npm install
COPY . .
CMD npm run serve
````
* 配置运行命令
2. 配置.dockerignore文件
* 也就是被docker命令所忽略的目录
`node_modules`
3. 生成镜像
* docker build --tag hello-world .
4. 生成容器
* docker container run -p 8080:8080 hello-world
* `然后就可以在8080端口看到项目运行的结果`

## echo
* echo命令的作用是打印字符串或者输出字符串到文件流中
* 例如`echo hello > /root/.npmrc`就是把字符串hello输出到/root/.npmrc
* 例如`echo hello >> /root/.npmrc`也是把字符串hello输出到/root/.npmrc
* 但是一个箭头和两个箭头的相同点在于
* 如果文件不存在，那么一个箭头> 和两个箭头>> 都会创建一个新的文件；
* 区别在于：
* 如果文件存在，那么一个箭头>是先把文件数据清空再写入文件中
* 但是两个箭头>>是追加到文件中
```markdown
* 例子1：
  echo hello > test.js
创建文件test.js，内容为hello

* 例子2：
  echo hello > test2.js
  创建文件test2.js，内容为hello

* 例子3：
  echo world > test.js
  文件test.js内容为world

* 例子4：
  echo world >> test2.js
  文件test2.js内容为hello \n world
```

#### docker run命令
* run命令分为两种模式：分离模式和前台模式（默认）
1. 分离模式也叫后台模式，特点是使用docker container run -d 来运行分离模式后，
* 容器不会再监听我们在docker container run这个命令的终端，所以我们在这个终端输入命令不会被监听
* 只能通过网络和共享卷组来进行交互。
2. 前台模式（默认），不加-d就是前台模式。我们可以在终端看到容器的所有输出
---
* `-it`参数，`分为-i和-t`,-i表示交互，没有-i,则bash不需要交互就会推出；-t是分配终端，没有-t,则docker进去后没法和bash进行交互
`docker container run -it hello-world`
