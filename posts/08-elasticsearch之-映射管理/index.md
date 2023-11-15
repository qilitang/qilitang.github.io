# 映射介绍



在Elasticsearch 6.0.0或更高版本中创建的索引只包含一个mapping type。 在5.x中使用multiple mapping types创建的索引将继续像以前一样在Elasticsearch 6.x中运行。 Mapping types将在Elasticsearch 7.0.0中完全删除

## 一 映射介绍

在创建索引的时候，可以预先定义字段的类型及相关属性

Es会根据Json数据源的基础类型，猜测你想要映射的字段，将输入的数据转变成可以搜索的索引项。

Mapping是我们自己定义的字段数据类型，同时告诉es如何索引数据及是否可以被搜索

作用：会让索引建立的更加细致和完善

### 1.1 字段数据类型

string类型：text，keyword

数字类型：long，integer，short，byte，double，float

日期类型：data

布尔类型：boolean

binary类型：binary

复杂类型：object（实体，对象），nested（列表）

geo类型：geo-point，geo-shape（地理位置）

专业类型：ip，competion（搜索建议）

### 1.2 映射参数
| 属性 | 描述 | 适合类型 |
| --- | --- | --- |
| store | 值为yes表示存储，no表示不存储，默认为no | all |
| index | yes表示分析，no表示不分析，默认为true | text |
| null_value | 如果字段为空，可以设置一个默认值，比如"NA"（传过来为空，不能搜索，na可以搜索） | all |
| analyzer | 可以设置索引和搜索时用的分析器，默认使用的是standard分析器，还可以使用whitespace，simple。都是英文分析器 | all |
| include_in_all | 默认es为每个文档定义一个特殊域_all,它的作用是让每个字段都被搜索到，如果想让某个字段不被搜索到，可以设置为false | all |
| format | 时间格式字符串模式 | date |


## 二 创建索引

text类型会取出词做倒排索引,keyword不会被分词，原样存储，原样匹配

mapping类型一旦确定，以后就不能修改了

```python
#6.x的版本没问题
PUT books
{
  "mappings": {
    "book":{
      "properties":{
        "title":{
          "type":"text",
         	"analyzer": "ik_max_word"
        },
        "price":{
          "type":"integer"
        },
        "addr":{
          "type":"keyword"
        },
        "company":{
          "properties":{
            "name":{"type":"text"},
            "company_addr":{"type":"text"},
            "employee_count":{"type":"integer"}
          }
        },
        "publish_date":{"type":"date","format":"yyy-MM-dd"}
       
      }
    }
  }
}
```

7.x版本以后

```python
PUT books
{
  "mappings": {
    "properties":{
      "title":{
        "type":"text",
        "analyzer": "ik_max_word"
      },
      "price":{
        "type":"integer"
      },
      "addr":{
        "type":"keyword"
      },
      "company":{
        "properties":{
          "name":{"type":"text"},
          "company_addr":{"type":"text"},
          "employee_count":{"type":"integer"}
        }
      },
      "publish_date":{"type":"date","format":"yyy-MM-dd"}
      
    }
    
  }
}
```

插入数据测试：

```python
PUT books/_doc/1
{
  "title":"大头儿子小偷爸爸",
  "price":100,  
  "addr":"北京天安门",
  "company":{
    "name":"我爱北京天安门",
    "company_addr":"我的家在东北松花江傻姑娘",
    "employee_count":10
  },
  "publish_date":"2019-08-19"
}
#测试数据2
PUT books/_doc/2
{
  "title":"白雪公主和十个小矮人",
  "price":"99", #写字符串会自动转换
  "addr":"黑暗森里",
  "company":{
    "name":"我的家乡在上海",
    "company_addr":"朋友一生一起走",
    "employee_count":10
  },
  "publish_date":"2018-05-19"
}
```

## 三 查看索引

```python
#查看books索引的mapping
GET books/_mapping
#获取所有的mapping
GET _all/_mapping
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/08-elasticsearch%E4%B9%8B-%E6%98%A0%E5%B0%84%E7%AE%A1%E7%90%86/  

