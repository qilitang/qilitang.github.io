# 使用docker安装Elasticsearch 集群

什么是ElasticSearch
<!--more-->
---

## Elasticsearch

以7.2.0为例

### 挂载

在宿主机上创建Elasticsearch的数据存储、config配置目录
eg:

```bash
# 在 Document 下创建 ElasticSearch 文件夹， 然后再创建 conf  data logs  文件夹 用于放置 es的数据， 配置 ， 日志文件
mkdir ELasticSearch
cd ElasticSearch
mkdir config
mkdir logs
mkdir data
```

> 新建用户 adduser es
> 数据存储需要赋予权限：sudo chown -R 1000:1000 ~/ELasticSearch/data

### Elasticsearch配置

在 ~/ELasticSearch/data/config文件夹下新建elasticsearch.yml、 jvm.options

具体内容可以参考：
elasticsearch.yml

```
cluster.name: business-log
node.name: es-b-188
http.port: 9200
transport.tcp.port: 9301
network.bind_host: 10.133.0.188
network.publish_host: 10.133.0.188
```

jvm.options

```
## JVM configuration

################################################################
## IMPORTANT: JVM heap size
################################################################
##
## You should always set the min and max JVM heap
## size to the same value. For example, to set
## the heap to 4 GB, set:
##
## -Xms4g
## -Xmx4g
##
## See https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html
## for more information
##
################################################################

# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms26g
-Xmx26g

################################################################
## Expert settings
################################################################
##
## All settings below this section are considered
## expert settings. Don't tamper with them unless
## you understand what you are doing
##
################################################################

## GC configuration
#-XX:+UseConcMarkSweepGC
#-XX:CMSInitiatingOccupancyFraction=75
#-XX:+UseCMSInitiatingOccupancyOnly

## G1GC Configuration
# NOTE: G1GC is only supported on JDK version 10 or later.
# To use G1GC uncomment the lines below.
#-XX:-UseConcMarkSweepGC
#-XX:-UseCMSInitiatingOccupancyOnly
-XX:+UseG1GC
-XX:InitiatingHeapOccupancyPercent=75

## DNS cache policy
# cache ttl in seconds for positive DNS lookups noting that this overrides the
# JDK security property networkaddress.cache.ttl; set to -1 to cache forever
-Des.networkaddress.cache.ttl=60
# cache ttl in seconds for negative DNS lookups noting that this overrides the
# JDK security property networkaddress.cache.negative ttl; set to -1 to cache
# forever
-Des.networkaddress.cache.negative.ttl=10

## optimizations

# pre-touch memory pages used by the JVM during initialization
-XX:+AlwaysPreTouch

## basic

# explicitly set the stack size
-Xss1m

# set to headless, just in case
-Djava.awt.headless=true

# ensure UTF-8 encoding by default (e.g. filenames)
-Dfile.encoding=UTF-8

# use our provided JNA always versus the system one
-Djna.nosys=true

# turn off a JDK optimization that throws away stack traces for common
# exceptions because stack traces are important for debugging
-XX:-OmitStackTraceInFastThrow

# flags to configure Netty
-Dio.netty.noUnsafe=true
-Dio.netty.noKeySetOptimization=true
-Dio.netty.recycler.maxCapacityPerThread=0

# log4j 2
-Dlog4j.shutdownHookEnabled=false
-Dlog4j2.disable.jmx=true

-Djava.io.tmpdir=${ES_TMPDIR}

## heap dumps

# generate a heap dump when an allocation from the Java heap fails
# heap dumps are created in the working directory of the JVM
-XX:+HeapDumpOnOutOfMemoryError

# specify an alternative path for heap dumps; ensure the directory exists and
# has sufficient space
-XX:HeapDumpPath=data

# specify an alternative path for JVM fatal error logs
-XX:ErrorFile=logs/hs_err_pid%p.log

## JDK 8 GC logging

8:-XX:+PrintGCDetails
8:-XX:+PrintGCDateStamps
8:-XX:+PrintTenuringDistribution
8:-XX:+PrintGCApplicationStoppedTime
8:-Xloggc:logs/gc.log
8:-XX:+UseGCLogFileRotation
8:-XX:NumberOfGCLogFiles=32
8:-XX:GCLogFileSize=64m

# JDK 9+ GC logging
9-:-Xlog:gc*,gc+age=trace,safepoint:file=logs/gc.log:utctime,pid,tags:filecount=32,filesize=64m
# due to internationalization enhancements in JDK 9 Elasticsearch need to set the provider to COMPAT otherwise
# time/date parsing will break in an incompatible way for some date patterns and locals
9-:-Djava.locale.providers=COMPAT
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119
```

### 启动

创建docker-compose.yml文件，内容如下：

```yml
version: '2.2'
services:
  elasticsearch:
    image: elasticsearch:7.2.0
    restart: always
    container_name: es-a
    network_mode: host
    volumes:
      - /home/zhang/Documents/ElasticSearch/data-a:/usr/share/elasticsearch/data
      - /home/zhang/Documents/ElasticSearch/config/el-a.yml:/usr/share/elasticsearch/config/elasticsearch.yml
      - /home/zhang/Documents/ElasticSearch/config/jvm.options:/usr/share/elasticsearch/config/jvm.options
    environment:
      - bootstrap.memory_lock=true
      - ES_JAVA_OPTS= -Xms64m -Xmx128m
    ulimits:
      memlock:
        soft: -1
        hard: -1
```

#### 启动：

```
docker-compose up -d
```

```bash
# 在使用  docker-compose 创建容器的过程中  创建端口映射不成功  所以使用一下命令创建， 可解决此问题。
sudo docker run -id --name=es-a -p 9200:9200 -p 9300:9300 -v /home/zhang/Documents/ElasticSearch/data/data-a:/usr/share/elasticsearch/data -v /home/zhang/Documents/ElasticSearch/config/el-a.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /home/zhang/Documents/ElasticSearch/config/jvm.options:/usr/share/elasticsearch/config/jvm.options -e "discovery.type=single-node" -e "ES_JAVA_OPTS= -Xms1024m -Xmx1024m" elasticsearch:7.2.0
```



### 备注

elasticsearch.yml、jvm.options、docker-compose.yml等每个节点都需要配置，其中elasticsearch.yml、docker-compose.yml具体节点信息需要替换。





---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/elasticsearch-04-docker-%E9%9B%86%E7%BE%A4/  

