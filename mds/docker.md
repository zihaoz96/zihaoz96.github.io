# Docker 是什么？

Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。它是目前最流行的 Linux 容器解决方案。

Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。

总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。

## Docker 的用途
Docker 的主要用途，目前有三大类。

- **提供一次性的环境** 比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。
- **提供弹性的云服务** 因为 Docker 容器可以随开随关，很适合动态扩容和缩容。
- **组建微服务架构**   通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。

## Docker 的安装

Docker 需要用户具有 sudo 权限，为了避免每次命令都输入sudo，可以把用户加入 Docker 用户组

```
$ sudo usermod -aG docker $USER
```

Docker 是服务器----客户端架构。命令行运行docker命令的时候，需要本机有 Docker 服务。如果这项服务没有启动，可以用下面的命令启动

```
# service 命令的用法
$ sudo service docker start

# systemctl 命令的用法
$ sudo systemctl start docker
```


## image 文件
image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。

```
# 列出本机的所有 image 文件。
$ docker image ls

# 删除 image 文件
$ docker image rm [imageName]

```

## Docker run常用参数
docker run命令会从 image 文件，生成一个正在运行的容器实例。
常用参数如下
- --name="myComtainer"      命名
- -p 81:80                  分配端口（主机：容器）
- -v                        映射文件，将静态文件public
- -d                        后台运行容器
