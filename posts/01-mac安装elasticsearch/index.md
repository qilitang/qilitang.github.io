# 安装ElasticSearch

## 一 安装JDK环境

因为ElasticSearch是用Java语言编写的，所以必须安装JDK的环境，并且是JDK 1.8以上，具体操作步骤自行百度

安装完成查看java版本

```java
java -version
```

## 二 官网下载最新版本

下载地址[[https://www.elastic.co/cn/downloads/elasticsearch](https://www.elastic.co/cn/downloads/elasticsearch)],选择相应版本下载即可

## 三 下载其他版本

直接点击[https://www.elastic.co/cn/downloads/past-releases#elasticsearch](https://www.elastic.co/cn/downloads/past-releases#elasticsearch)

## 三 下载完成，启动

解压文件，切换到解压文件路径下，执行

```python
cd elasticsearch-<version> #切换到路径下
./bin/elasticsearch  #启动es
#如果你想把 Elasticsearch 作为一个守护进程在后台运行，那么可以在后面添加参数 -d 。
#如果你是在 Windows 上面运行 Elasticseach，你应该运行 bin\elasticsearch.bat 而不是 bin\elasticsearch
```

## 四 测试启动是否成功

在浏览器输入以下地址：[http://127.0.0.1:9200/](http://127.0.0.1:9200/)

即可看到如下内容：

```
{
  "name" : "lqzMacBook.local",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "G1DFg-u6QdGFvz8Z-XMZqQ",
  "version" : {
    "number" : "7.5.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "e9ccaed468e2fac2275a3761849cbee64b39519f",
    "build_date" : "2019-11-26T01:06:52.518245Z",
    "build_snapshot" : false,
    "lucene_version" : "8.3.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

## 五 关闭es

```python
#查看进程
ps -ef | grep elastic
#干掉进程
kill -9 2382（进程号）
#以守护进程方式启动es
elasticsearch -d
```

## 六 Docker 安装ES

### Docker 安装 es

- 拉取镜像

  ```bash
  sudo docker pull elasticsearch:7.2.0
  ```

  等待下载完成， （选择docker是因为 docker比较轻便 不会出现各种各样的 java环境问题）

- 创建容器

  ```bash
  # 在 Document 下创建 ElasticSearch 文件夹， 然后再创建 conf  data logs  文件夹 用于放置 es的数据， 配置 ， 日志文件
  mkdir ELasticSearch
  cd ElasticSearch
  mkdir conf
  mkdir logs
  mkdir data
  # 使用run 命令  创建 es 容器
  sudo docker run -id --name=es-a -p 9200:9200 -p 9300:9300 -v /home/zhang/Documents/ElasticSearch/data/data-a:/usr/share/elasticsearch/data -v /home/zhang/Documents/ElasticSearch/config/el-a.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /home/zhang/Documents/ElasticSearch/config/jvm.options:/usr/share/elasticsearch/config/jvm.options -e "discovery.type=single-node" -e "ES_JAVA_OPTS= -Xms1024m -Xmx1024m" elasticsearch:7.2.0
  
  
  
  ```
  

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.25.0-rc2/docker-compose-Linux-x86_64 -o /usr/local/bin/docker-composesudo chmod +x /usr/local/bin/docker-compose

sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.0-rc2/docker-compose-Linux-x86_64 -o /usr/local/bin/docker-composesudo chmod +x /usr/local/bin/docker-compose
```



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/01-mac%E5%AE%89%E8%A3%85elasticsearch/  

