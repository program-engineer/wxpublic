## 一、使用 export 和 import

### 1.查看本机的容器

这两个命令是通过容器来导入、导出镜像。首先我们使用 **docker ps -a** 命令查看本机所有的容器。

### 2.导出镜像

（1）使用 **docker export** 命令根据容器 **ID** 将镜像导出成一个文件。

```
docker export 容器ID > server.tar
```


（2）上面命令执行后，可以看到文件已经保存到当前的 **docker** 终端目录下。

### 3.导入镜像

（1）使用 **docker import** 命令则可将这个镜像文件导入进来。

```
docker import - new__server < server.tar
```


（2）执行 **docker images** 命令可以看到镜像确实已经导入进来了。


## 二、使用 save 和 load

### 1.查看本机的容器

这两个命令是通过镜像来保存、加载镜像文件的。首先我们使用 **docker images** 命令查看本机所有的镜像。

### 2.保存镜像

（1）下面使用 **docker save** 命令根据 **ID** 将镜像保存成一个文件。

```
docker save 0fdf2b4c26d3 > hangge_server.tar
```

（2）我们还可以同时将多个 **image** 打包成一个文件，比如下面将镜像库中的 **postgres** 和 **mongo** 打包：

```
docker save -o images.tar postgres:9.6 mongo:3.4
```

### 3.载入镜像

使用 **docker load** 命令则可将这个镜像文件载入进来。

```
docker load < hangge_server.tar
```