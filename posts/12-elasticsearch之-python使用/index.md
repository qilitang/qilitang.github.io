# 

```python
from elasticsearch import Elasticsearch

obj = Elasticsearch()
# 创建索引（Index）
result = obj.indices.create(index='user', body={"userid":'1','username':'lqz'},ignore=400)
# print(result)
# 删除索引
# result = obj.indices.delete(index='user', ignore=[400, 404])
# 插入数据
# data = {'userid': '1', 'username': 'lqz','password':'123'}
# result = obj.create(index='news', doc_type='politics', id=1, body=data)
# print(result)
# 更新数据
'''
不用doc包裹会报错
ActionRequestValidationException[Validation Failed: 1: script or doc is missing
'''
# data ={'doc':{'userid': '1', 'username': 'lqz','password':'123ee','test':'test'}}
# result = obj.update(index='news', doc_type='politics', body=data, id=1)
# print(result)


# 删除数据
# result = obj.delete(index='news', doc_type='politics', id=1)

# 查询
# 查找所有文档
query = {'query': {'match_all': {}}}
#  查找名字叫做jack的所有文档
# query = {'query': {'term': {'username': 'lqz'}}}

# 查找年龄大于11的所有文档
# query = {'query': {'range': {'age': {'gt': 11}}}}

allDoc = obj.search(index='news', doc_type='politics', body=query)
print(allDoc['hits']['hits'][0]['_source'])
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/12-elasticsearch%E4%B9%8B-python%E4%BD%BF%E7%94%A8/  

