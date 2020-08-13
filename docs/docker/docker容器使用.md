# docker容器使用
### docker客户端

直接输入`docker`命令可以看到docker客户端的所有命令选项

```
$ docker
```

![](http://img.taycc.com/15579083474745.jpg)


通过命令`docker <command> --help`可以查看更深入的命令使用方法

```
$ docker stats --help
```

![](http://img.taycc.com/15579084828567.jpg)


###  运行一个web应用
前面我们运行的容器并没有一些什么特别的用处。

接下来让我们尝试使用 docker 构建一个 web 应用程序。

我们将在docker容器中运行一个 Python Flask 应用来运行一个web应用。

```
$ docker pull training/webapp  # 载入镜像
$ docker run -d -P training/webapp python app.py
```

参数说明：

> - -d:让容器在后台运行。
> 
> - -P:将容器内部使用的网络端口映射到我们使用的主机上。

![](http://img.taycc.com/15579087649016.jpg)
![](http://img.taycc.com/15579089139269.jpg)

### 查看web应用容器

使用`docker ps`来查看我们正在运行的容器：
![](http://img.taycc.com/15579090015280.jpg)

Docker开放了`5000`端口（默认 Python Flask 端口）映射到主机端口`32769`上。

这时我们可以通过浏览器访问WEB应用
![](http://img.taycc.com/15579091639138.jpg)

我们也可以通过 -p 参数来设置不一样的端口：

```
$ docker run -d -p 5000:5000 training/webapp python app.py
```

![](http://img.taycc.com/15579092640176.jpg)
容器内部的 5000 端口映射到我们本地主机的 5000 端口上。


### 网络端口的快捷方式
通过`docker ps`命令可以查看到容器的端口映射，docker还提供了另一个快捷方式`docker port`，使用`docker port`可以查看指定（ID 或者名字）容器的某个确定端口映射到宿主机的端口号。

上面我们创建的web应用容器ID为`82e5eeb4a925`名字为 `gifted_brahmagupta`。

我可以使用`docker port 82e5eeb4a925`或`docker port gifted_brahmagupta`来查看容器端口的映射情况。

![](http://img.taycc.com/15579093903509.jpg)

### 查看 WEB 应用程序日志
`docker logs [ID或者名字]`可以查看容器内部的标准输出。
![](http://img.taycc.com/15579096926981.jpg)

> - -f: 让 docker logs 像使用 tail -f 一样来输出容器内部的标准输出。

从上面，我们可以看到应用程序使用的是 5000 端口并且能够查看到应用程序的访问日志。

### 查看WEB应用程序容器的进程
我们还可以使用`docker top`来查看容器内部运行的进程
![](http://img.taycc.com/15579098103928.jpg)


### 检查 WEB 应用程序
使用`docker inspect`来查看Docker的底层信息。它会返回一个JSON文件记录着Docker容器的配置和状态信息。
![](http://img.taycc.com/15579098940994.jpg)

### 停止WEB应用容器
```
$ docker stop 82e5eeb4a925
```

### 重启WEB应用容器
已经停止的容器，我们可以使用命令`docker start`来启动。
```
$ docker start 82e5eeb4a925
```

docker ps -l 查询最后一次创建的容器：

```
$ docker ps -l 
```
![](http://img.taycc.com/15579101247211.jpg)
正在运行的容器，我们可以使用`docker restart`命令来重启

### 移除WEB应用容器
我们可以使用`docker rm`命令来删除不需要的容器
```
docker rm 82e5eeb4a925
```

删除容器时，容器必须是停止状态，否则会报如下错误
![](http://img.taycc.com/15579102313059.jpg)



<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>