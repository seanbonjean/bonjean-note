# docker相关

## Dockerfile

一个简单的Dockerfile示例：

```Dockerfile
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y python3 python3-pip
WORKDIR /running
COPY . .
RUN pip3 install --upgrade pip
RUN pip3 install --no-cache-dir -r requirements.txt
ENV CONTAINER_NAME="test"
ENV ENV_ARG="what u want"
CMD ["sh", "-c", "python3 main.py & python3 app.py"]
```

## docker CLI

* `docker pull [image_name]` 拉取镜像
* `docker push [image_name]` 推送镜像
* `docker build -t [image_name] .` 构建镜像，image_name格式形如`name:tag`，如`my_image:v1.0.0`，`my_image:latest`，`my_image:dev`
* `docker run -d -p [host_port]:[container_port] --name [container_name] [image_name]` 根据镜像部署容器，`-d`表示后台运行，`-it`表示交互式运行（即进入容器：将容器的输入输出绑定到当前终端）
* `docker ps` 列出所有正在运行的容器
* `docker start [container_id/container_name]` 启动容器，该容器必须曾docker run过，只是停止了
* `docker stop [container_id/container_name]` 停止容器
* `docker rm [container_id/container_name]` 删除容器
* `docker images` 列出所有镜像
* `docker rmi [image_id/image_name]` 删除镜像
* `docker tag [image_id/image_name] [new_image_name]` 修改镜像名称
* `docker exec -it [container_id/container_name] bash` 进入容器的一个新终端，可以直接Ctrl+D（等价于exit）退出
* `docker attach [container_id/container_name]` 连接到容器主进程（Dockerfile中CMD指定的命令），终止主进程(Ctrl-C)后容器会停止，应该用Ctrl-P + Ctrl-Q退出容器
* `docker logs [container_id/container_name]` 查看容器日志，`-f`持续更新日志输出，`-t`显示时间戳，`--tail 10`查看最后10行日志，`--since 10m`查看最近10min的日志
* `docker inspect [image_id/image_name/container_id/container_name/subnet_name]` 查看 镜像/容器/docker子网 信息
* `docker commit [container_id/container_name] [image_name]` 提交容器为一个镜像
* `docker save -o [file_name] [image_name]` 保存镜像为文件

## 环境变量的作用

可以通过在Dockerfile中定义环境变量，或是 `docker run` 命令的 `--env ENV_ARG=what u want` / `-e` 参数设置环境变量（命令中指定的环境变量会覆盖Dockerfile中定义的相同环境变量），往容器内传递参数，让容器产生不同的行为

比如，python可以通过 `env_value = os.environ.get('ENV_ARG')` 获取环境变量的值，从而根据env_value的值来产生不同的行为
