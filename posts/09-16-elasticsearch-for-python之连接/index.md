# 文档操作-python连接

## 一 前言

现在，我们来学习Python如何操作elasticsearch。

## 二 依赖下载

首先，我们必须**拥有Python的环境**，如何搭建Python环境，请[参阅](https://www.cnblogs.com/Neeo/p/9681427.html)。

要用Python来操作elasticsearch，首先安装Python的elasticsearch包：

```
pip install elasticsearch
pip install elasticsearch==6.3.1
# 豆瓣源
pip install -i https://pypi.doubanio.com/simple/ elasticsearch
```

## 三 Python连接elasticsearch

Python连接elasticsearch有以下几种连接方式：

```
from elasticsearch import  Elasticsearch
# es = Elasticsearch()    # 默认连接本地elasticsearch
# es = Elasticsearch(['127.0.0.1:9200'])  # 连接本地9200端口
es = Elasticsearch(
    ["192.168.1.10", "192.168.1.11", "192.168.1.12"], # 连接集群，以列表的形式存放各节点的IP地址
    sniff_on_start=True,    # 连接前测试
    sniff_on_connection_fail=True,  # 节点无响应时刷新节点
    sniff_timeout=60    # 设置超时时间
)
```

## 四 配置忽略响应状态码

```
es = Elasticsearch(['127.0.0.1:9200'],ignore=400)  # 忽略返回的400状态码
es = Elasticsearch(['127.0.0.1:9200'],ignore=[400, 405, 502])  # 以列表的形式忽略多个状态码
```

## 五 一个简单的示例

```
from elasticsearch import  Elasticsearch
es = Elasticsearch()    # 默认连接本地elasticsearch
print(es.index(index='py2', doc_type='doc', id=1, body={'name': "张开", "age": 18}))
print(es.get(index='py2', doc_type='doc', id=1))
```

第1个print为创建`py2`索引，并插入一条数据，第2个print查询指定文档。

查询结果如下：

```
{'_index': 'py2', '_type': 'doc', '_id': '1', '_version': 1, 'result': 'created', '_shards': {'total': 2, 'successful': 1, 'failed': 0}, '_seq_no': 0, '_primary_term': 1}
{'_index': 'py2', '_type': 'doc', '_id': '1', '_version': 1, 'found': True, '_source': {'name': '张开', 'age': 18}}
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-16-elasticsearch-for-python%E4%B9%8B%E8%BF%9E%E6%8E%A5/  

