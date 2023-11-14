# Docker-compose


### 安装 docker-compose

###  切换到root 用户

#### 下载

```bash
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

#### 修改权限

```bash
chmod +x /usr/local/bin/docker-compose
```

#### 检查是否成功

```bash
docker-compose -version	
```



此外，因为 docker-compose 使用 Python 编写，所以可以使用 pip 直接安装：

```
$ pip install -U docker-compose
1
```

------

## 第一个 Compose file

```
version: '3'
services:
  spider:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - $PWD:/code
      - /data1/datascience/scrapy-data:/data
    command: scrapy crawl comment
12345678910
```

说明：

- version 指定了 compose file 的版本，最新的版本为 3.4
- services 下定义服务，这里定义了一个爬虫服务 spider
- build 指定构建镜像上下文、Dockerfile 文件和 ARGS 等。
- volumes 用于创建卷并挂载，这里挂载了源码目录和数据存储目录
- command 指定服务启动时执行的命令，参考 Dockerfile COMMAND。

------

## Compose file 指令

先来看一份 docker-compose.yml 文件：

```
version: '2'
services:
  web:
    image: dockercloud/hello-world
    ports:
      - 8080
    networks:
      - front-tier
      - back-tier

  redis:
    image: redis
    links:
      - web
    networks:
      - back-tier

  lb:
    image: dockercloud/haproxy
    ports:
      - 80:80
    links:
      - web
    networks:
      - front-tier
      - back-tier
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock 

networks:
  front-tier:
    driver: bridge
  back-tier:
driver: bridge
```

可以看到一份标准配置文件应该包含 version、services、networks 三大部分，其中最关键的就是 services 和 networks 两个部分，下面先来看 services 的书写规则。

- **image**: 指定容器启动的镜像，对应 Dockerfile FROM。
  在 services 标签下的第二级标签是 web，这个名字是用户自己自定义，它就是服务名称。image 则是指定服务的镜像名称或镜像 ID。如果镜像在本地不存在，Compose 将会尝试拉取这个镜像。

  ```
  image: redis
  image: ubuntu:14.04
  image: tutum/influxdb
  image: example-registry.com:4000/postgresql
  image: a4bc65fd
  ```
  
- **build**: 服务除了可以基于指定的镜像，还可以基于一份 Dockerfile，在使用 up 启动之时执行构建任务，这个构建标签就是 build，它可以指定 Dockerfile 所在文件夹的路径。Compose 将会利用它自动构建这个镜像，然后使用这个镜像启动服务容器。

  最简单的直接利用 Dockerfile 构建，只需要指定上下文或者 Dockerfile 位置：

  ```
  version: '2'
  services:
    webapp:
      build: .
  ```
  

如果需要更细粒度的配置，需要使用 context, dockerfile, args, labels 等选项。

```
  # Dockerfile
  ARG buildno
  ARG password
  
  RUN echo "Build number: $buildno"
  RUN script-requiring-password.sh "$password"
  
  
  # docker-compose file
  version: '2'
  services:
    webapp:
      image: webapp:tag
      build:
        context: ./dir
        dockerfile: Dockerfile-alternate
        args:
          buildno: 1
          password: secret
```

- **command**: 使用 command 可以覆盖容器启动后默认执行的命令。

  ```
  command: bundle exec thin -p 3000
  
  command: ["bundle", "exec", "thin", "-p", "3000"]
  ```
  
- **container_name**: 指定运行时容器名称，而不使用默认格式（<项目名称><服务名称><序号>）。

  ```
      container_name: my-web-container
  ```
  
- **depends_on**: 指明服务之间依赖，解决了容器的依赖、启动先后的问题。

  ```
  version: '3'
  services:
    web:
      build: .
      depends_on:
        - db
        - redis
    redis:
      image: redis
    db:
      image: postgres
  ```
  
> 被依赖的服务会优先启动（显然）
  > 若服务启动时被依赖服务没有启动，则自动被启动

- **dns**: 指定 DNS 服务器。

  ```
  dns: 8.8.8.8
  dns:
    - 8.8.8.8
    - 9.9.9.9
  ```
  
- **tmpfs**: 挂载临时目录到容器内部，与 run 的参数一样效果：

  ```
  tmpfs: /run
  tmpfs:
    - /run
    - /tmp
  ```
  
- **entrypoint**: 在 Dockerfile 中有一个指令叫做 ENTRYPOINT 指令，用于指定接入点，在 docker-compose.yml 中可以定义接入点，覆盖 Dockerfile 中的定义。

  ```
  entrypoint: /code/entrypoint.sh
  ```
  
- **env_file**: 专门存放变量的文件。如果通过 docker-compose -f FILE 指定了配置文件，则 env_file 中路径会使用配置文件路径。如果有变量名称与 environment 指令冲突，则以后者为准。格式如下：

  ```
  env_file: .env
  
  env_file:
    - ./common.env
    - ./apps/web.env
    - /opt/secrets.env
  ```
  

文件格式如：

```
  RACK_ENV=development
```

  > 注意的是这里所说的环境变量是对宿主机的 Compose 而言的，如果在配置文件中有 build 操作，这些变量并不会进入构建过程中，如果要在构建中使用变量还是首选前面刚讲的 arg 标签。

- **environment**: 与上面的 env_file 标签完全不同，反而和 arg 有几分类似，这个标签的作用是设置镜像变量，它可以保存变量到镜像里面，也就是说启动的容器也会包含这些变量设置，这是与 arg 最大的不同。

  > 一般 arg 标签的变量仅用在构建过程中。而 environment 和 Dockerfile 中的 ENV 指令一样会把变量一直保存在镜像、容器中，类似 docker run -e 的效果。

  ```
  environment:
    RACK_ENV: development
    SHOW: 'true'
    SESSION_SECRET:
  
  environment:
    - RACK_ENV=development
    - SHOW=true
    - SESSION_SECRET
  ```
  
- **expose**: 这个标签与 Dockerfile 中的 EXPOSE 指令一样，用于指定暴露的端口，但是只是作为一种参考，实际上 docker-compose.yml 的端口映射还得 ports 这样的标签。

  ```
  expose:
    - "3000"
    - "8000"
  ```
  
- **external_links**: 在使用 Docker 过程中，我们会有许多单独使用`docker run`启动的容器，为了使 Compose 能够连接这些不在 docker-compose.yml 中定义的容器，我们需要一个特殊的标签，就是 external_links，它可以让 Compose 项目里面的容器连接到那些项目配置外部的容器（前提是外部容器中必须至少有一个容器是连接到与项目内的服务的同一个网络里面）。

  ```
  external_links:
    - redis_1
    - project_db_1:mysql
    - project_db_1:postgresql
  ```
  
- **extra_hosts**: 添加主机名的标签，就是往 /etc/hosts 文件中添加一些记录、

  ```
  extra_hosts:
    - "somehost:162.242.195.82"
    - "otherhost:50.31.209.229"
  ```
  
- **labels**: 向容器添加元数据，和 Dockerfile 的 LABEL 指令一个意思。

  ```
  labels:
    com.example.description: "Accounting webapp"
    com.example.department: "Finance"
    com.example.label-with-empty-value: ""
  labels:
    - "com.example.description=Accounting webapp"
    - "com.example.department=Finance"
    - "com.example.label-with-empty-value"
  ```
  
- **links**: 上面的 depends_on 标签解决的是**启动顺序**问题，这个标签解决的是**容器连接**问题，与 Docker client 的 –link 一样效果，会连接到其它服务中的容器。。

  ```
  web:
    links:
    - db
    - db:database
    - redis
  ```
  
> 使用的别名将会自动在服务容器中的 /etc/hosts 里创建。
  > 相应的环境变量也将被创建。

- **logging**: 用于配置日志服务。默认的 driver 是 json-file。

  ```
  logging:
    driver: syslog
    options:
      syslog-address: "tcp://192.168.0.42:123"
  ```
  
- **pid**: 将 PID 模式设置为主机 PID 模式，跟主机系统共享进程命名空间。容器使用这个标签将能够访问和操纵其他容器和宿主机的名称空间。

  ```
  pid: "host"
  ```
  
- **ports**: 映射端口的标签。使用 HOST:CONTAINER 格式或者只是指定容器的端口，宿主机会随机映射端口。

  ```
  ports:
    - "3000"
    - "8000:8000"
    - "49100:22"
    - "127.0.0.1:8001:8001"
  12345
  ```

  > 注意：当使用 HOST:CONTAINER 格式来映射端口时，如果你使用的容器端口小于 60 你可能会得到错误得结果，因为 YAML 将会解析 xx:yy 这种数字格式为 60 进制。所以建议采用字符串格式。

- **security_opt**: 为每个容器覆盖默认的标签。简单说来就是管理全部服务的标签。比如设置全部服务的 user 标签值为 USER:

  ```
  security_opt:
    - label:user:USER
    - label:role:ROLE
  123
  ```

- **stop_signal**: 设置另一个信号来停止容器。在默认情况下使用的是 SIGTERM 停止容器。设置另一个信号可以使用 stop_signal 标签。

  ```
  stop_signal: SIGUSR1
  1
  ```

- **volumes**: 挂载一个目录或者一个已存在的数据卷容器，可以直接使用 HOST:CONTAINER 这样的格式，或者使用 HOST:CONTAINER:ro 这样的格式，后者对于容器来说，数据卷是只读的，这样可以有效保护宿主机的文件系统。

  ```
  volumes:
    # 只是指定一个路径，Docker 会自动在创建一个数据卷（这个路径是容器内部的）。
    - /var/lib/mysql
  
    # 使用绝对路径挂载数据卷
    - /opt/data:/var/lib/mysql
  
    # 以 Compose 配置文件为中心的相对路径作为数据卷挂载到容器。
    - ./cache:/tmp/cache
  
    # 使用用户的相对路径（~/ 表示的目录是 /home/<用户目录>/ 或者 /root/）。
    - ~/configs:/etc/configs/:ro
  
    # 已经存在的命名的数据卷。
    - datavolume:/var/lib/mysql
  123456789101112131415
  ```

- **cap_add, cap_drop**: 添加或删除容器的内核功能。

- **devices**: 设备映射列表。与 Docker client 的`--device`参数类似。

  ```
  devices:
    - "/dev/ttyUSB0:/dev/ttyUSB0"
  12
  ```

- **extends**: 这个标签可以扩展另一个服务，扩展内容可以是来自在当前文件，也可以是来自其他文件，相同服务的情况下，后来者会有选择地覆盖原有配置。

  ```
  extends:
    file: common.yml
    service: webapp
  123
  ```

  用户可以在任何地方使用这个标签，只要标签内容包含 file 和 service 两个值就可以了。file 的值可以是相对或者绝对路径，如果不指定 file 的值，那么 Compose 会读取当前 YML 文件的信息。

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/docker-compose/  

