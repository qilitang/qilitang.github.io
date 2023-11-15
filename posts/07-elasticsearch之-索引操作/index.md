# 索引操作


具体操作可以查看官方文档

[https://www.elastic.co/guide/en/elasticsearch/reference/7.5/indices.html](https://www.elastic.co/guide/en/elasticsearch/reference/7.5/indices.html)>

官方2版本的中文文档

[https://www.elastic.co/guide/cn/elasticsearch/guide/current/index-settings.html](https://www.elastic.co/guide/cn/elasticsearch/guide/current/index-settings.html)

## 一 索引初始化

```python
#新建一个lqz2的索引，索引分片数量为5，索引副本数量为1
PUT lqz2
{
  "settings": {
    "index":{
      "number_of_shards":5,
      "number_of_replicas":1
    }
  }
}
'''
number_of_shards
每个索引的主分片数，默认值是 5 。这个配置在索引创建后不能修改。
number_of_replicas
每个主分片的副本数，默认值是 1 。对于活动的索引库，这个配置可以随时修改。
'''
```

## 二 查询索引配置

```python
#获取lqz2索引的配置信息
GET lqz2/_settings
#获取所有索引的配置信息
GET _all/_settings
#同上
GET _settings
#获取lqz和lqz2索引的配置信息
GET lqz,lqz2/_settings
```

## 三 更新索引

```python
#修改索引副本数量为2
PUT lqz/_settings
{
  "number_of_replicas": 2
}
#如遇到报错：cluster_block_exception，因为
#这是由于ES新节点的数据目录data存储空间不足，导致从master主节点接收同步数据的时候失败，此时ES集群为了保护数据，会自动把索引分片index置为只读read-only
PUT  _all/_settings
{
"index": {
  "blocks": {
    "read_only_allow_delete": false
    }
  }
}
```

## 四 删除索引

```python
#删除lqz索引
DELETE lqz
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/07-elasticsearch%E4%B9%8B-%E7%B4%A2%E5%BC%95%E6%93%8D%E4%BD%9C/  

