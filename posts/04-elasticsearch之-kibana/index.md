# Kibana介绍

## 一 Kibana介绍

Kibana 是一款开源的数据分析和可视化平台，它是 Elastic Stack 成员之一，设计用于和 Elasticsearch 协作。

您、可以使用 Kibana 对 Elasticsearch 索引中的数据进行搜索、查看、交互操作。

可以很方便的利用图表、表格及地图对数据进行多元化的分析和呈现

详情可见用户手册：

[https://www.elastic.co/guide/cn/kibana/current/index.html](https://www.elastic.co/guide/cn/kibana/current/index.html)

注意跟Elasticsearch版本兼容情况，详情见：

[https://www.elastic.co/cn/support/matrix#matrix_compatibility](https://www.elastic.co/cn/support/matrix#matrix_compatibility)

下载地址为：

[https://www.elastic.co/cn/downloads/past-releases](https://www.elastic.co/cn/downloads/past-releases)

## 二 下载Kibana

到相应地址，下载即可

![](https://tva1.sinaimg.cn/large/006tNbRwgy1g9kxtlb6vgj31ty0qan0r.jpg#alt=image-20191204195700775)

解压下载后的文件

## 三 修改配置文件

修改配置文件：vim 安装目录/config/kibana.yml

```python
# 更多配置信息，详见 https://www.elastic.co/guide/cn/kibana/current/settings.html
server.port: 5601
server.host: "127.0.0.1"
server.name: lqz
elasticsearch.hosts: ["http://localhost:9200/"]
```

## 四 启动

到安装目录下：

```python
./bin/kibana
#正常启动
```

##六 查看

在浏览器里访问：[http://localhost:5601/app/kibana](http://localhost:5601/app/kibana)

（如访问不到，尝试删除es中跟kibana相关的索引）

选择Dev Tools

![](https://tva1.sinaimg.cn/large/006tNbRwgy1g9kzwehg9oj30u010wn00.jpg#alt=image-20191204210857615)

在console中输入GET _settings ,查询可以看到如下

![](https://tva1.sinaimg.cn/large/006tNbRwgy1g9kzz3jspvj31fe0u0dom.jpg#alt=image-20191204211133809)


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/04-elasticsearch%E4%B9%8B-kibana/  

