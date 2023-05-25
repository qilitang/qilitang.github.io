# Linux 安装 Elasticsearch

在linux 上安装 elasticsearch
<!--more-->
---

### 前言

由于elasticsearch依赖java环境，所以，我们首先要安装java jdk。
这里我们使es和kibana的版本保持一致，环境如下：

- centos7.3
- java1.8
- elasticsearch6.7.0
- kibana6.7.0
- ik6.7.0

另外，要检查一下防火墙是否关闭：

```bash
Copyfirewall-cmd --state        # 检查防火墙是否关闭
systemctl stop firewalld.service    # 停止firewall
systemctl disable firewalld.service # 禁止开机启动
```

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
> URL: https://www.qilitang.top/posts/elasticsearch-03-linux%E5%AE%89%E8%A3%85/  

