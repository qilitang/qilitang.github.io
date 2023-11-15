# 文档操作-高亮查询

## 一 前言

如果返回的结果集中很多符合条件的结果，那怎么能一眼就能看到我们想要的那个结果呢？比如下面网站所示的那样，我们搜索`elasticsearch`，在结果集中，将所有`elasticsearch`高亮显示？

我们该怎么做呢？

## 二 准备数据

```
PUT lqz/doc/4
{
  "name":"石头",
  "age":29,
  "from":"gu",
  "desc":"粗中有细，狐假虎威",
  "tags":["粗", "大","猛"]
}
```

## 三 默认高亮显示

我们来查询：

```
GET lqz/doc/_search
{
  "query": {
    "match": {
      "name": "石头"
    }
  },
  "highlight": {
    "fields": {
      "name": {}
    }
  }
}
```

上例中，我们使用`highlight`属性来实现结果高亮显示，需要的字段名称添加到`fields`内即可，`elasticsearch`会自动帮我们实现高亮。

结果如下：

```
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 1.5098256,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "4",
        "_score" : 1.5098256,
        "_source" : {
          "name" : "石头",
          "age" : 29,
          "from" : "gu",
          "desc" : "粗中有细，狐假虎威",
          "tags" : [
            "粗",
            "大",
            "猛"
          ]
        },
        "highlight" : {
          "name" : [
            "<em>石</em><em>头</em>"
          ]
        }
      }
    ]
  }
}
```

上例中，`elasticsearch`会自动将检索结果用标签包裹起来，用于在页面中渲染。

## 四 自定义高亮显示

但是，你可能会问，我不想用`em`标签， 我这么牛逼，应该用个`b`标签啊！好的，`elasticsearch`同样考虑到你很牛逼，所以，我们可以自定义标签。

```
GET lqz/chengyuan/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  },
  "highlight": {
    "pre_tags": "<b class='key' style='color:red'>",
    "post_tags": "</b>",
    "fields": {
      "from": {}
    }
  }
}
```

上例中，在`highlight`中，`pre_tags`用来实现我们的自定义标签的前半部分，在这里，我们也可以为自定义的标签添加属性和样式。`post_tags`实现标签的后半部分，组成一个完整的标签。至于标签中的内容，则还是交给`fields`来完成。

```
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 0.5753642,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "chengyuan",
        "_id" : "1",
        "_score" : 0.5753642,
        "_source" : {
          "name" : "老二",
          "age" : 30,
          "sex" : "male",
          "birth" : "1070-10-11",
          "from" : "gu",
          "desc" : "皮肤黑，武器长，性格直",
          "tags" : [
            "黑",
            "长",
            "直"
          ]
        },
        "highlight" : {
          "name" : [
            "<b class='key' style='color:red'>老</b><b class='key' style='color:red'>二</b>"
          ]
        }
      }
    ]
  }
}
```

需要注意的是：**自定义标签中属性或样式中的逗号一律用英文状态的单引号表示，应该与外部`elasticsearch`语法的双引号区分开**。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-8-elasticsearch%E4%B9%8B%E9%AB%98%E4%BA%AE%E6%9F%A5%E8%AF%A2/  

