# Docker

> 帮助命令

```shell
docker version
docker info
docker -v
docker --help
```

> 镜像命令

```shell
# 搜索镜像
docker search 镜像名称
# 下载镜像
docker pull 镜像名称:版本号
# 查看镜像
docker images
# 删除镜像
docker rmi 镜像名称/镜像ID
# 删除所有镜像
docker rmi `docker images -aq`
```

> 容器命令

```shell
docker run [可选参数] 镜像ID/镜像名称
# 参数说明
--name    容器别名
-d        后台运行
-it       交互模式
-p        映射端口
-P        随机指定端口
-v        挂载容器数据卷
-e        容器参数配置
# 删除容器
docker rm 容器ID/容器名称
# 强制删除
docker rm -f 容器ID/容器名称
# 停止容器
docker stop 容器ID/容器名称
# 启动停止的容器
dockeer start 容器ID/容器名称
# 重启容器
dockeer restart 容器ID/容器名称
```

> 常用命令

```shell
# 进入容器
docker exec -it 容器ID /bin/bash

# 从容器内拷贝文件到宿主机
docker cp 容器ID:容器内的路径 目的的主机路径

```

> Dockerfile

```dockerfile
# 基础镜像
FROM centos
# 维护者
MAINTAINER 2634058628@qq.com
# RUN 镜像构建需要运行的命令
# ADD 添加文件到镜像中
# COPY 类型ADD
# WORKDIR 工作目录
# ENTRYPOINT 类似CMD
# EXPOSE 保留端口配置
# ENV 配置参数
# ONBUILD 当构建一个被继承的DockerFile时运行命令，父镜像在被子镜像继承后，父镜像的ONBUILD就会触发
# 容器卷
VOLUME ["/data/CentOS","/home"]
# 运行时执行的命令
CMD echo "success"
CMD /bin/bash
```


> 自定义网络配置

```shell
# --driver 网络模式 --subnet 子网 --gateway 网关
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
# 查看网络配置
docker network inspect mynet
```

> DockerCompose

```yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

```shell
# 启动 -d 后台运行
docker-compose up -d
# 重新编译
docker-compose up --build
```
