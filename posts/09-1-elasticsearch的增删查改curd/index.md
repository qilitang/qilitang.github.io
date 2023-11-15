# 文档操作-增删改查

## 一 CURD之Create

```python
PUT lqz/doc/1
{
  "name":"顾老二",
  "age":30,
  "from": "gu",
  "desc": "皮肤黑、武器长、性格直",
  "tags": ["黑", "长", "直"]
}
```

他明处貌似还有俩老婆：

```python
PUT lqz/doc/2
{
  "name":"大娘子",
  "age":18,
  "from":"sheng",
  "desc":"肤白貌美，娇憨可爱",
  "tags":["白", "富","美"]
}

PUT lqz/doc/3
{
  "name":"龙套偏房",
  "age":22,
  "from":"gu",
  "desc":"mmp，没怎么看，不知道怎么形容",
  "tags":["造数据", "真","难"]
}
```

家里红旗不倒，家外彩旗飘摇：

```python
PUT lqz/doc/4
{
  "name":"石头",
  "age":29,
  "from":"gu",
  "desc":"粗中有细，狐假虎威",
  "tags":["粗", "大","猛"]
}

PUT lqz/doc/5
{
  "name":"魏行首",
  "age":25,
  "from":"广云台",
  "desc":"仿佛兮若轻云之蔽月,飘飘兮若流风之回雪,mmp，最后竟然没有嫁给顾老二！",
  "tags":["闭月","羞花"]
}
```

注意：**当执行`PUT`命令时，如果数据不存在，则新增该条数据，如果数据存在则修改该条数据。**

咱们通过`GET`命令查询一下：

```python
GET lqz/doc/1
```

结果如下：

```python
{
  "_index" : "lqz",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 1,
  "found" : true,
  "_source" : {
    "name" : "顾老二",
    "age" : 30,
    "from" : "gu",
    "desc" : "皮肤黑、武器长、性格直",
    "tags" : [
      "黑",
      "长",
      "直"
    ]
  }
}
```

查询也没啥问题，但是你可能说了，人家老二是黄种人，怎么是黑的呢？好吧咱改改`desc`和`tags`：

```python
PUT lqz/doc/1
{
  "desc":"皮肤很黄，武器很长，性格很直",
  "tags":["很黄","很长", "很直"]
}
```

上例，我们仅修改了`desc`和`tags`两处，而`name`、`age`和`from`三个属性没有变化，我们可以忽略不写吗？查查看：

```python
GET lqz/doc/1
```

结果如下：

```python
{
  "_index" : "lqz",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 3,
  "found" : true,
  "_source" : {
    "desc" : "皮肤很黄，武器很长，性格很直",
    "tags" : [
      "很黄",
      "很长",
      "很直"
    ]
  }
}
```

哎呀，出事故了！修改是修改了，但结果不太理想啊，因为`name`、`age`和`from`属性都没啦！

注意：**`PUT`命令，在做修改操作时，如果未指定其他的属性，则按照指定的属性进行修改操作。**也就是如上例所示的那样，我们修改时只修改了`desc`和`tags`两个属性，其他的属性并没有一起添加进去。

很明显，这是病！dai治！怎么治？上车，咱们继续往下走！

## 二 CURD之Update

让我们首先恢复一下事故现场：

```python
PUT lqz/doc/1
{
  "name":"顾老二",
  "age":30,
  "from": "gu",
  "desc": "皮肤黑、武器长、性格直",
  "tags": ["黑", "长", "直"]
}
```

我们要将黑修改成黄：

```python
POST lqz/doc/1/_update
{
  "doc": {
    "desc": "皮肤很黄，武器很长，性格很直",
    "tags": ["很黄","很长", "很直"]
  }
}
```

上例中，我们使用`POST`命令，在`id`后面跟`_update`，要修改的内容放到`doc`文档（属性）中即可。

我们再来查询一次：

```python
GET lqz/doc/1
```

结果如下：

```python
{
  "_index" : "lqz",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 5,
  "found" : true,
  "_source" : {
    "name" : "顾老二",
    "age" : 30,
    "from" : "gu",
    "desc" : "皮肤很黄，武器很长，性格很直",
    "tags" : [
      "很黄",
      "很长",
      "很直"
    ]
  }
}
```

结果如上例所示，现在其他的属性没有变化，只有`desc`和`tags`属性被修改。

注意：**`POST`命令，这里可用来执行修改操作（还有其他的功能），`POST`命令配合`_update`完成修改操作，指定修改的内容放到`doc`中**。

写了这么多，我也发现我上面有讲的`不对`的地方——石头不是跟顾老二不清不楚，石头是跟小桃不清不楚！好吧，刚才那个数据是一个错误示范！我们这就把它干掉！

## 三CURD之Delete

```python
DELETE lqz/doc/4
```

很简单，通过`DELETE`命令，就可以删除掉那个错误示范了！

删除效果如下：

```python
{
  "_index" : "lqz",
  "_type" : "doc",
  "_id" : "4",
  "_version" : 4,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 4,
  "_primary_term" : 1
}
```

我们再来查询一遍：

```python
GET lqz/doc/4
```

结果如下：

```python
{
  "_index" : "lqz",
  "_type" : "doc",
  "_id" : "4",
  "found" : false
}
```

上例中，`found：false`表示查询数据不存在。

## 四 CURD之Retrieve

我们上面已经不知不觉的使用熟悉这种简单查询方式，通过 `GET`命令查询指定文档：

```python
GET lqz/doc/1
```

结果如下：

```python
{
  "_index" : "lqz",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 5,
  "found" : true,
  "_source" : {
    "name" : "顾老二",
    "age" : 30,
    "from" : "gu",
    "desc" : "皮肤很黄，武器很长，性格很直",
    "tags" : [
      "很黄",
      "很长",
      "很直"
    ]
  }
}
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-1-elasticsearch%E7%9A%84%E5%A2%9E%E5%88%A0%E6%9F%A5%E6%94%B9curd/  

