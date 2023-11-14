# Docker 详解


## 1 Docker的安装

```bash
# 1 yum 包更新到最新
yum update
# 2 安装所需要的软件包, yum-util 提供yum-config-manager 功能, 另外两个是 devicemapper驱动以来的
yum install -y yum-utils device-mapper-persistent-data lvm2
# 3 设置yum 源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 4 安装docker 出现输入的界面都按y
yum install -y docker-ce
# 5 查看docker的版本, 验证是否安装成功
docker -v

```

## 2 Docker 架构

![](https://gitee.com/qilitang/Img/raw/master/img/20200319155859.png)

## 3 配置镜像加速器

![](https://gitee.com/qilitang/Img/raw/master/img/20200319160447.png)

![](https://gitee.com/qilitang/Img/raw/master/img/20200319160350.png)

## 4 Docker 服务相关命令

- 启动docker服务

  ```bash
  systemctl start docker
  ```

- 停止docker服务

  ```bash
  systemctl stop docker
  ```



- 重启docker服务

  ```python
  systemctl restart docker
  ```



- 查看docker服务状态

  ```bash
  systemctl status docker
  ```

- 开机启动docker服务

  ```bash
  systemctl enable docker
  ```

- 查看docker 日志

  ```bash
  docker logs 容器名
  ```



## 5 Docker 镜像相关命令

- 查看本地镜像

  ```bash
  docker images
  # REPOSITORY  指的是容器
  # TAG         指的是版本号
  # IMAGE ID    镜像的id
  # CREATED     创建时间
  # SIZE        大小
  ```

  ![](https://gitee.com/qilitang/Img/raw/master/img/20200319161406.png)

- 搜索镜像

  ```bash
  docker search redis
  ```

- 下载镜像

  ```bash
  docker pull redis
  # 不写版本号 默认是 lastest
  #  我们在hub.docker.com  搜索 软件版本号
  docker pull redis:5.0
  #  下载 5.0 版本的 redis
  ```

- 删除镜像

  ```python
  # id 删除
  docker rmi 镜像id
  # 版本删除
  docker rmi redis:版本
  # 查看所有镜像的id
  docker images -q
  # 删除所有镜像
  docker rmi `docker images -q`
  ```



## 6 Docker 容器相关命令

![](https://gitee.com/qilitang/Img/raw/master/img/20200319164153.png)

- 创建容器

  ```bash
  # 1 查看镜像
  docker images
  # 创建容器
  docker run -it --name=c1  mysql:5.7 /bin/bash
  # -i 表示 容器一直运行着
  # -t  表示  给容器加个伪终端
  # -d  以守护式 模式创建容器
  # -name 表示 起名字 后面可以 = 或者空格
  # 指定镜像 mysql:5.7
  # /bin/bash 表示进入容器时的初始化指令 默认是 /bin/bash
  ```

  ![](https://gitee.com/qilitang/Img/raw/master/img/20200319163047.png)

  ```bash
  # 使用exit 退出容器
  ```

  **注意** :

  ```bash
  # 使用 -it 创建的容器
  		# 创建成功会立马进入容器
  		# 在退出容器后会立马停止容器
  ```

  创建后台运行的容器

  ```bash
  #  创建的容器会在后台运行.
  docker run -id --name=m2 mysql:5.7
  ```



- 查看容器

  ```bash
  # 查看正在运行的容器
  docker ps
  # 查看所有容器
  docker ps -a
  ```

- 进入容器

  ```bash
  docker exec -it 容器名 /bin/bash
  ```

- 启动容器

  ```bash
  docker start 容器名
  ```

- 停止容器

  ```bash
  docker stop 容器名
  ```

- 删除容器

  ```bash
  docker rm  容器名
  # 开启的容器是不能被删除
  ```

- 容器其他信息

  ```bash
  docker inspect 容器名
  ```

- 查看容器日志

  ```bash
  docker logs 容器名
  ```

#### 容器启动错误解决

- Errcode: 13 - Permission denied)

  ```python
  #  在创建容器时 加上
  -–privileged=true
  ```



## 7 Docker 容器的 数据卷

- 数据卷的简介

  ![](https://gitee.com/qilitang/Img/raw/master/img/20200319165053.png)

- 配置数据卷

  ![](https://gitee.com/qilitang/Img/raw/master/img/20200319165302.png)



### 数据卷容器

- 简介

  ![](https://gitee.com/qilitang/Img/raw/master/img/20200319170418.png)

- 配置数据卷容器

  ![](https://gitee.com/qilitang/Img/raw/master/img/20200319170531.png)

- 数据卷目录 位置查找

  ```bash
  # 查找容器的具体信息
  docker inspect 容器名
  ```

  查找宿主机数据目录与容器对应目录

  ![](https://gitee.com/qilitang/Img/raw/master/img/20200319170911.png)

### 小结

![](https://gitee.com/qilitang/Img/raw/master/img/20200319171332.png)

## 8 Docker的应用部署

### Docker 部署 mysql



- 1 搜索 mysql

  ```python
  docker search mysql
  ```

- 2 拉取mysql 镜像

  ```bash
  docker pull  mysql:5.7
  ```

- 3 创建容器

  ```bash
  # 1 在/root 下创建mysql 目录 用于存储mysql数据信息
  mkdir ~/mysql
  cd ~/mysql
  ```

  ```bash
  docker run -id \
  -p 3306:3306 \
  --name=c_mysql \
  -v $PWD/conf:/etc/mysql/conf.d \
  -v $PWD/logs:/logs \
  -v $PWD/data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=951228 \
  mysql:5.7
  ```

  - 参数说明
    - -p 3307:3306  将容器的3306端口映射到宿主机的3307端口
    - -v $pwd/conf:/etc/mysql/conf.d  将主机当前目录下的conf/my.cnf 挂在到容器的 /etc.mysql/my.cnf 配置目录
    - -v $pwd/logs:/logs  将 主机当前目录下的logs 目录挂在到容器的/logs   日志目录
    - -v $pwd/data:/var/lib/mysql  讲主机当前目录下的data 目录 挂载到容器的 /var/lib/mysql   数据目录
    - -e MYSQL_ROOT_PASSWORD=123456  初始化root用户的密码

  - Mysql 5.7 默认配置文件

    ```python
    # For advice on how to change settings please see
    # http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html

    [mysqld]

    # Remove leading # and set to the amount of RAM for the most important data
    # cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
    # innodb_buffer_pool_size = 128M

    # Remove leading # to turn on a very important data integrity option: logging
    # changes to the binary log between backups.
    # log_bin

    # These are commonly set, remove the # and set as required.
    # basedir = .....
    # datadir = .....
    # port = .....
    # server_id = .....
    # socket = .....

    # Remove leading # to set options mainly useful for reporting servers.
    # The server defaults are faster for transactions and fast SELECTs.
    # Adjust sizes as needed, experiment to find the optimal values.
    # join_buffer_size = 128M
    # sort_buffer_size = 2M
    # read_rnd_buffer_size = 2M

    sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

    ```



  - docker 中配置默认路径

    ```python
    /usr/my.cnf
    ```



### Docker 部署 Tomcat

```bash
# 注意  需要进入 tomcat 容器内 把 webapps.disp 改名为 webapps , 不然访问不到主页
```

- 1 搜索tomcat镜像

  ```bash
  docker search tomcat
  ```

- 2 拉去tomcat 镜像

  ```bash
  docker pull tomcat
  ```

- 3 创建容器, 设置端口映射, 目录映射

  ```bash
  mkdir ~/tomcat
  cd ~/tomcat
  ```

  ```bash
  docker run -id --name=c_tomcat \
  -p 8080:8080 \
  -v $PWD:/usr/local/tomcat/webapps \
  tomcat
  ```

  - 参数解释同上



### Docker 部署 Nginx

```bash
# 注意,  首先要创建nginx.conf 文件 用于管理 容器的nginx
#  在 宿主nginx 文件夹下创建/conf/nginx.conf  用于映射 容器nginx配置文件
```

Nginx 原始配置文件

```python

```

- 搜索nginx镜像

  ```bash
  docker search nginx
  ```

- 拉去nginx 镜像

  ```bash
  docker pull nginx
  ```

- 创建nginx 容器

  ```bash
  mkdir ~/nginx
  cd mkdir ~/nginx
  # 创建 conf 文件夹
  mkdir conf
  # 在conf 文件夹下创建 nginx.conf 文件, 粘贴下面内容
  vim nginx.conf
  ```

- 准备好 nginx 配置文件

  ```python
  user  nginx;
  worker_processes  1;

  error_log  /var/log/nginx/error.log warn;
  pid        /var/run/nginx.pid;
  ```


  events {
      worker_connections  1024;
  }


  http {
      include       /etc/nginx/mime.types;
      default_type  application/octet-stream;

      log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '"$http_user_agent" "$http_x_forwarded_for"';
    
      access_log  /var/log/nginx/access.log  main;
    
      sendfile        on;
      #tcp_nopush     on;
    
      keepalive_timeout  65;
    
      #gzip  on;
    
      include /etc/nginx/conf.d/*.conf;
  }

  ```

- 创建nginx 容器

  ```bash
  docker run -id --name=c_nginx \
  -p 80:80 \
  -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf \
  -v $PWD/logs:/var/log/nginx \
  -v $PWD/html:/usr/share/nginx/html \
  nginx
  ```

  - 参数说明

    ```bash
    # -p 80:80  宿主机80 端口 映射 容器的80端口
    # -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf  使用宿主机 nginx.conf 配置文件映射到 容器的配置文件
    # -v $PWD/logs:/var/log/nginx  挂在log 目录文件
    # -v $PWD/html:/usr/share/nginx/html
    ```



### Docker 部署 Redis

- 1 搜索 Redis

  ```bash
  docker search redis
  ```

- 2 拉去镜像

  ```python
  docker pull redis:5.0
  ```

- 3 创建容器 映射端口

  ```python
  docker run -id --name=c_redis -p 6379:6379 redis:5.0
  ```




## 9 Docker 镜像原理

![](https://gitee.com/qilitang/Img/raw/master/img/20200320110833.png)



![](https://gitee.com/qilitang/Img/raw/master/img/20200320110932.png)

## 10 Docker 镜像制作

### 第一种方式

容器转镜像

![](https://gitee.com/qilitang/Img/raw/master/img/20200320111346.png)

### 第二种方式

dockerfile

![](https://gitee.com/qilitang/Img/raw/master/img/20200320123954.png)

关键字

![](https://gitee.com/qilitang/Img/raw/master/img/20200320124404.png)

![](https://gitee.com/qilitang/Img/raw/master/img/20200320124736.png)

#### Dockerfile 案例1

![](https://gitee.com/qilitang/Img/raw/master/img/20200320200048.png)

构建镜像

```bash
docker built -f centos_dockerfile -t itheima_centos:1.0
# -f  找到 dockerfile 文件
# -t  指定 新镜像的名称及版本
```

#### Dockerfile 案例2

![](https://gitee.com/qilitang/Img/raw/master/img/20200320205055.png)

## 11 Docker的服务编排

### 介绍

![](https://gitee.com/qilitang/Img/raw/master/img/20200320205303.png)

### Docker Compose

![](https://gitee.com/qilitang/Img/raw/master/img/20200320205430.png)

#### 1 安装 Docker Compose

```bash
# Compose 目前已经完全支持Linux , Macos 和Windows, 在我们安装Compose 之前, 需要先安装Docker, 下面我们以编译好的二进制包方式安装在Linux系统中.
curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose=`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
# 设置文件可执行权限
chmod +x /usr/local/bin/docker-compose
# 查看版本信息
docker-compose -version
```

#### 2 卸载 Docker Compose

```bash
# 而今之宝方式安装的, 删除二进制文件即可
rm /usr/local/bin/docker-compose
```

#### 3 使用

- 1 创建 docker-compose 目录

  ```bash
  mkdir ~/docker-compose
  cd ~/docker-compose
  ```

- 2  编写 docker-compose.yml文件

  ```bash
  version: '3'
  services:
  	nginx:
  		image: nginx
  		ports:
  		 - 80:80
  		links:
  		 - app
  		valumes:
  		 - ./nginx/conf.d:/etc/nginx/conf.d
  		app:
  		 image: app
  		 expose:
  		  - "8080"
  ```

- 3 创建./nginx/conf.d 目录

  ```bash
  mkdir -p ./nginx/conf.d
  # -p 表示是在父文件夹
  ```

- 4 在 ./nginx/conf.d 目录下 编写 nginx 的 conf 文件.  文件必须是 conf 文件 名字无所谓

  ```bash
  server {
  		listen 80;
  		access_log off;

  		location / {
  				proxy_pass http://app:8080;
  		}
  }
  ```

- 5 在 ~/docker-compose 目录下 使用 docker-compose 启动容器

  ```bash
  docker-compose up
  ```

- 6 测试访问

  ```
  直接访问 nginx
  ```



## 12 Docker 私有仓库

 ### 一  私有仓库的搭建

```bash
# 1 拉去私有仓库镜像
docker pull registry
# 2 启动私有仓库容器
docker run -id --name=registry -p 5000:5000 registry
# 3 打开浏览器 输入地址  http://私有仓库服务器ip:5000/v2/_catalog , 看到 {"repositories":[]} 表示私有仓库 搭建成功
# 4 修改daemon.json
vim /etc/docker/daemon.json
# 在上述稳健者添加一个key, 保存退出. 此步用于让docker 新人私有仓库地址; 注意 讲次有仓库服务器ip 修改为自己私有仓库服务器的真是ip
{"insecure-registries":["私有仓库服务器ip:5000"]}
# 5 重启 docker 服务
systemctl restart docker
docker start registry
```

### 二 将 镜像上传至私有仓库

```bash
# 1 标记镜像为私有仓库镜像
docker tag centos:7 私有仓库服务器ip"5000/contos:7

# 2 上传标记镜像
docker push 私有仓库服务器ip:5000/centos:7

```

### 三 私有仓库的镜像拉到本地

```bash
docker pull 仓库ip:5000/centos:7
# 拉取镜像到本地
```

## 13 Docker 与 虚拟机的对比

### Docker 容器虚拟化 与 创痛虚拟机比较

![](https://gitee.com/qilitang/Img/raw/master/img/20200320213651.png)

![](https://gitee.com/qilitang/Img/raw/master/img/20200320214019.png)

[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/docker/  

