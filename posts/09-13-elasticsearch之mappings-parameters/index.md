# 文档操作-mappings parameters

## 一 ignore_above

长度超过`ignore_above`设置的字符串将不会被索引或存储（个人认为会存储，但不会为该字段建立索引，也就是该字段不能被检索）。 对于字符串数组，`ignore_above`将分别应用于每个数组元素，并且不会索引或存储比`ignore_above`更长的字符串元素。

```
PUT w1
{
  "mappings": {
    "doc":{
      "properties":{
        "t1":{
          "type":"keyword",
          "ignore_above": 5
        },
        "t2":{
          "type":"keyword",
          "ignore_above": 10   ①
        }
      }
    }
  }
}
PUT w1/doc/1
{
  "t1":"elk",          ②
  "t2":"elasticsearch"  ③
}
GET w1/doc/_search   ④
{
  "query":{
    "term": {
      "t1": "elk"
    }
  }
}

GET w1/doc/_search  ⑤
{
  "query": {
    "term": {
      "t2": "elasticsearch"
    }
  }
}
```

①，该字段将忽略任何超过10个字符的字符串。

②，此文档已成功建立索引，也就是说能被查询，并且有结果返回。

③，该字段将不会建立索引，也就是说，以该字段作为查询条件，将不会有结果返回。

④，有结果返回。

⑤，则将不会有结果返回，因为`t2`字段对应的值长度超过了`ignove_above`设置的值。

该参数对于防止Lucene的术语字节长度限制也很有用，限制长度是`32766`。

注意，该ignore_above设置可以利用现有的领域进行更新[PUT地图API](https://www.elastic.co/guide/en/elasticsearch/reference/7.0/indices-put-mapping.html)。

对于值`ignore_above`是字符数，但Lucene的字节数为单位。如果您使用带有许多非ASCII字符的UTF-8文本，您可能需要设置限制，`32766 / 4 = 8191`因为UTF-8字符最多可占用4个字节。

如果我们观察上述示例中，我们可以看到在设置映射类型时，字段的类型是`keyword`，也就是说`ignore_above`参数仅针对于`keyword`类型有用。

那么如果字符串的类型是`text`时能用`ignore_above`吗，答案是能，但要特殊设置：

```
PUT w2
{
  "mappings": {
    "doc":{
      "properties":{
        "t1":{
          "type":"keyword",
          "ignore_above":5
        },
        "t2":{
          "type":"text",
          "fields":{
            "keyword":{
              "type":"keyword",
              "ignore_above": 10
            }
          }
        }
      }
    }
  }
}

PUT w2/doc/1
{
  "t1":"beautiful",
  "t2":"beautiful girl"
}

GET w2/doc/_search  ①
{
  "query": {
    "term": {
      "t1": {
        "value": "beautiful"
      }
    }
  }
}

GET w2/doc/_search  ②
{
  "query": {
    "term": {
      "t2": "beautiful"
    }
  }
}
```

①，不会有返回结果。

②，有返回结果，因为该字段的类型是`text`。

但是，当字段类型设置为`text`之后，`ignore_above`参数的限制就失效了。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-13-elasticsearch%E4%B9%8Bmappings-parameters/  

