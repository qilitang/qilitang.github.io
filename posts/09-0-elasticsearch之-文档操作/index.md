# 文档操作


## 一 新增文档

```python
#新增一个id为1的书籍（POST和PUT都可以）
POST lqz/_doc/1/_create
#POST lqz/_doc/1
#POST lqz/_doc 会自动创建id,必须用Post
{
  "title":"红楼梦",
  "price":12,
  "publish_addr":{
    "province":"黑龙江",
    "city":"鹤岗"
  },
  "publish_date":"2013-11-11",
  "read_num":199,
  "tag":["古典","名著"]
}
```

## 二 查询文档

```python
#查询lqz索引下id为7的文档
GET lqz/_doc/7
#查询lqz索引下id为7的文档，只要title字段
GET lqz/_doc/7?_source=title
#查询lqz索引下id为7的文档，只要title和price字段
GET lqz/_doc/7?_source=title,price
#查询lqz索引下id为7的文档，要全部字段
GET lqz/_doc/7?_source
```

## 三 修改文档

```python
#修改文档(覆盖修改)
PUT lqz/_doc/10
{
  "title":"xxxx",
  "price":333,
  "publish_addr":{
    "province":"黑龙江",
    "city":"福州"
  }
}
#修改文档，增量修改，只修改某个字段(注意是post)
POST lqz/_update/10
{
  "doc":{
    "title":"修改"
  }
}
```

## 四 删除文档

```python
#删除文档id为10的
DELETE lqz/_doc/10
```

## 五 批量操作之_mget

```python
#批量获取lqz索引_doc类型下id为2的数据和lqz2索引_doc类型下id为1的数据
GET _mget
{
  "docs":[
    {
      "_index":"lqz",
      "_type":"_doc",
      "_id":2
    },
    {
      "_index":"lqz2",
      "_type":"_doc",
      "_id":1
    }
    ]
}

#批量获取lqz索引下id为1和2的数据
GET lqz/_mget
{
  "docs":[
    {
      "_id":2
    },
    {
      "_id":1
    }
    ]
}
#同上
GET lqz/_mget
{
  "ids":[1,2]
}
```

## 六 批量操作之 bulk

```python
PUT test/_doc/2/_create
{
  "field1" : "value22"
}
POST _bulk
{ "index" : { "_index" : "test", "_id" : "1" } }
{ "field1" : "value1" }
{ "delete" : { "_index" : "test", "_id" : "2" } }
{ "create" : { "_index" : "test", "_id" : "3" } }
{ "field1" : "value3" }
{ "update" : {"_id" : "1", "_index" : "test"} }
{ "doc" : {"field2" : "value2"} }
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-0-elasticsearch%E4%B9%8B-%E6%96%87%E6%A1%A3%E6%93%8D%E4%BD%9C/  

