# Docker 容器间通讯


## Docker容器间通信

Docker会为每个创建的容器自动分配一个**虚拟IP**，这个虚拟IP无法从容器外侧进行访问，只是Docker环境内部容器间彼此通信的标识，容器间的网络是互通互联的。

**但是通过这种虚拟IP方式，进行容器间的通信，有一些局限性**。因为在Docker中创建一个容器非常容易，比如说新创建的MySQL容器可能会替代之前创建的MySQL容器，而此时Tomcat服务器中配置的是之前的MySQL容器的Ip地址，因此随着新容器的创建，之前Tomcat配置的Ip地址需要不断的变更，并重启服务。当有很多Tomcat服务器同时连接MySQL数据库时，这个修改工作就是一个很大的工作量。

**那这个问题如何解决呢？**

容器创建时进行命名，配置连接时不在配置IP地址，而是配置容器的名字，这样以后再创建容器时，只要名字不变，就可以自动连接到新创建的容器。



**举例说明：**

**（1）创建web服务**

```bash
# 创建容器 运行Tomcat服务 --name 为容器命名



docker run -d --name webserver tomcat
```

**（2）创建数据库服务（模拟）**

```bash
docker run -d --name database -it centos /bin/bash
```

注：命令添加中的-**d -it** 和**/bin/bash** 是因为centos容器创建完成后，默认就进入退出状态。如果想让其保持运行状态，后台必须保持一个进程在运行，/bin/bash表示启动容器后启动bash。

**（3）查看正在运行的容器**

![img](https://img-blog.csdnimg.cn/20200812155425803.png)

（4）查看连个容器的虚拟IP地址

通过docker inspect [OPTIONS] NAME|ID 命令查看

**docker inspect :** 获取容器/镜像的元数据。

- 首先查看webserver容器的Ip地址

```bash
docker inspect webserver
```

![img](https://img-blog.csdnimg.cn/20200812160157328.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjE3Nzc4,size_16,color_FFFFFF,t_70)

- 查看database容器的IP地址

```bash
docker inspect database
```

![img](https://img-blog.csdnimg.cn/20200812160243197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjE3Nzc4,size_16,color_FFFFFF,t_70)

**（5）进入webserver容器内容，测试网络**

```bash
docker exec -it webserver /bin/bash
```

![img](https://img-blog.csdnimg.cn/20200812160553340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjE3Nzc4,size_16,color_FFFFFF,t_70)

但是直接ping database却是不通的，因为我们并没有进行相应的网络设置，docker并不知道database和哪个IP是对应的，因此我们可以在创建webserver容器时指定webserver到database的单向通信。

### 1、容器间单向通信

移除上面创建的web容器，并重新创建

```bash
# 强制移除webserver容器

docker rm -f webserver

# 重新创建webserver容器 --link 要链接的容器名称 （--link 可以使新生成的容器链接到已在运行的容器）

docker run -d --name webserver --link database tomcat



 
```

![img](https://img-blog.csdnimg.cn/20200812162320893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjE3Nzc4,size_16,color_FFFFFF,t_70)

Tomcat中的数据库连接配置文件中的**url**中的**IP地址**即可换成**database**

### 2、容器间双向通信

![img](https://img-blog.csdnimg.cn/2020081216451796.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjE3Nzc4,size_16,color_FFFFFF,t_70)

图中的网桥充当了Docker容器和外界宿主机之间的通信源，使得容器内部也可以访问互联网

例如在webserver内部可以访问百度

![img](https://img-blog.csdnimg.cn/20200812164801515.png)

网桥也可以用于容器内部，实现容器间的双向通信。不仅如此网桥还有另一个重要作用，就是对容器从网络层面进行分组

（1）通过【docker network ls 】列出当前网络状况明细

![img](https://img-blog.csdnimg.cn/20200812165323923.png)

上图中列出的bridge即为容器和外界通信的桥梁，如果我们想实现容器内部的双向通信也可以创建一个网桥，具体操作过程如下：

（2）创建网桥

```bash
# 创建网桥

docker network create -d bridge my-bridge

# 执行命令进行查看

docker network ls
```

![img](https://img-blog.csdnimg.cn/20200812170200462.png)

（3）将需要通信的容器添加到网桥中

```bash
docker network connect my-bridge webserver

docker network connect my-bridge database
```

（4）进入容器进行验证是否能够互相通信

![img](https://img-blog.csdnimg.cn/20200812170457927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjE3Nzc4,size_16,color_FFFFFF,t_70)![img](https://img-blog.csdnimg.cn/20200812170540602.png)

网桥的原理：创建一个网桥，就相当于在宿主机上安装了一个虚拟网卡，这个虚拟网卡也承担了网关的作用。虚拟网卡和物理网卡之间需要做地址转换

![img](https://img-blog.csdnimg.cn/20200812170908689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjE3Nzc4,size_16,color_FFFFFF,t_70)

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/docker-%E5%AE%B9%E5%99%A8%E9%97%B4%E9%80%9A%E8%AE%AF/  

