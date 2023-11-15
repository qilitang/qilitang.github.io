# 文档操作-查询

```
查询分类：
基本查询：使用es内置查询条件进行查询
组合查询：把多个查询组合在一起进行复合查询
过滤：查询的同时，通过filter条件在不影响打分的情况下筛选数据
```

## 一 基本查询

```python
#添加映射
PUT lago
{
  "mappings": {
    "properties":{
      "title":{
        "stort":true,
        "type":"text",
        "analyzer":"ik_max_word"
      },
      "company_name":{
         "stort":true,
        	"type":"keyword",
      },
      "desc":{
        "type":"text"
      },
      "comments":{
        "type":"integer"
      },
      "add_time":{
        "type":"date",
        "format":"yyy-MM-dd"
      }
      
    }
    
  }
}
#测试数据
POST lago/job
{
  "title":"python django 开发工程师",
  "company_name":"美团科技有限公司",
  "desc":"对django熟悉，掌握mysql和非关系型数据库，网站开发",
  "comments:200,
  "add_time":"2018-4-1"
}
POST lago/job
{
  "title":"python数据分析",
  "company_name":"百度科技有限公司",
  "desc":"熟悉python基础语法，熟悉数据分析",
  "comments:5,
  "add_time":"2018-10-1"
}
POST lago/job
{
  "title":"python自动化运维",
  "company_name":"上海华为",
  "desc":"熟悉python基础语法，精通Linux",
  "comments:90,
  "add_time":"2019-9-18"
}
```

### 1.1 match查询

```python
GET lagou/job/_search
{
  "query":{
    "match":{
      "title":"python"
    }
  }
}
#因为title字段做了分词，python都能搜索出来
#搜索python网站也能搜索出来，把python和网站分成两个词
#搜索爬取也能搜索到，把爬和取分词，去搜索
#只搜取 搜不到
```

### 1.2 term查询

```python
GET lagou/_search
{
  "query":{
    "term":{
      "title":"python"
    }
  }
}
#会拿着要查询的词不做任何处理，直接查询
#用python爬虫，查不到，用match就能查到
{
  "query":{
    "term":{
      "company_name":"美团"
    }
  }
}
#通过美团，就查询不到
```

### 1.3 terms查询

```python
GET lagou/_search
{
  "query":{
    "terms":{
      "title":["工程师","django","运维"]
    }
  }
}
#三个词，只要有一个，就会查询出来
```

### 1.4 控制查询的返回数量（分页）

```python
GET lagou/_search
{
  "query":{
    "match":{
      "title":"python"
    }
  }，
  "form":1,
  "size":2
}
#从第一条开始，大小为2
```

### 1.5 match_all 查询

```python
GET lagou/_search
{
  "query":{
    "match_all":{}
  }
}
#所有数据都返回
```

### 1.6 match_phrase查询

```python
GET lagou/_search
{
  "query":{
    "match_phrase":{
      "title":{
        "query":"python系统",
        "slop":6
      }
    }
  }
}
#短语查询， 
#会把查询条件python和系统分词，放到列表中，再去搜索的时候，必须满足python和系统同时存在的才能搜出来
#"slop":6 ：python和系统这两个词之间最小的距离
```

### 1.7 multi_match

```python
GET lagou/_search
{
  "query":{
    "multy_match":{
   			"query":"python",
      	"fields":["title","desc"]
    }
  }
}
#可以指定多个字段
#比如查询title和desc这个两个字段中包含python关键词的文档
#"fields":["title^3","desc"]:权重，title中的python是desc中的三倍
```

### 1.8 指定返回的字段

```python
GET lagou/_search
{
  "query":{
    "stored_fields":["title","company_name"]
    "match":{
   			"title":"python"
    }
  }
}
#只返回title和company_name字段
#"stored_fields":["title","company_name",'dsc'],不会返回dsc，因为我们要求stroed_fields，之前desc字段设为false（默认），不会显示
```

### 1.9 sort 结果排序

```python
GET lagou/_search
{
  "query":{
 			"match_all":{}
  },
  "sort":[
    {
      "comments":{
        "order":"desc"
      }
    }
  ]
}
#查询所有文档，按comments按desc降序排序
```

### 1.10 range范围查询

```python
GET lagou/_search
{
  "query":{
 			"range":{
        "comments":{
          "gte":10,
          "lte":20,
          "boost":2.0
        }
      }
  }
}
#指定comments字段大于等于10，小于等于20
#boost：权重

GET lagou/_search
{
  "query":{
 			"range":{
        "add_time":{
          "gte":"2019-10-11",
          "lte":"now",
        }
      }
  }
}
#对时间进行查询
```

### 1.11 wildcard查询

```python
GET lagou/_search
{
  "query":{
    "wildcard":{
      "title":{
        "value":"pyth*n",
        "boost":2.0
      }
    }
  }
}
#模糊查询，title中，有pyth任意值n得都能查出来
```

## 二 组合查询

### 2.1 bool查询

```python
#bool查询包括must should must_not filter
'''
bool:{
	"filter":[],   字段过滤
	"must":[],     所有查询条件都满足
	"should":[],   满足一个或多个
	"must_not":{}  都不满足于must相反
}
'''
# 建立测试数据
POST lago/testjob/_bulk
{"index":{"_id":1}}
{"salary":10,"title":"Python"}
{"index":{"_id":2}}
{"salary":20,"title":"Scrapy"}
{"index":{"_id":3}}
{"salary":30,"title":"Django"}
{"index":{"_id":4}}
{"salary":30,"title":"Elasticsearch"}
```

### 2.2 简单过滤查询

```python
#select * from testjob where salary=20
GET lagou/testjob/_search
{
 "query":{
   	"bool":{
       "must":{
         "match_all":{}
       },
       "filter":{
         "term":{
           "salary":20
         }
       }
     }
 }
}
```

### 1.3 查询多个值

```python
#查询薪资是10k或20k的
GET lagou/testjob/_search
{
  "query":{
    	"bool":{
        "must":{
          "match_all":{}
        },
        "filter":{
          "terms":{
            "salary":[10,20]
          }
        }
      }
  }
}
#select * from testjob where title="python"
GET lagou/testjob/_search
{
  "query":{
    	"bool":{
        "must":{
          "match_all":{}
        },
        "filter":{
          "term":{
            "title":"Python"
          }
        }
      }
  }
}
#title 是text字段，会做大小写转换，term不会预处理，拿着大写Python去查查不到
#可以改成小写，或者用match来查询
'''
   "filter":{
          "match":{
            "title":"Python"
          }
        }
'''
#查看分析器解析结果
GET _analyze
{
  "analyzer":"ik_max_word",
  "text":"python网络开发工程师"
}
```

### 1.4 bool过滤查询，可以做组合过滤查询

```python
#select * from testjob where (salary=20 or title=Python) and (salary!=30)
#查询薪资等于20k或者工作为python的工作，排除价格为30k的
{
  "query":{
    "bool":{
      "should":[
        {"term":{"salary":20}},
        {"term":{"title":"python"}}
      ],
      "must_not":{
        "term":{"salary":30}
      }
    }
  }
}
#select * from testjob where title=python or (title=django and salary=30)
{
  "query":{
    "bool":{
      "should":[
        {"term":{"title":"python"}},
        {
          "bool":{
            "must":[
              {"term":{"title":"django"}},
              {"term":{"salary":30}}
            ]
          }
        }
      ]
    }
  }
}
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/10-elasticsearch%E4%B9%8B-%E6%9F%A5%E8%AF%A2/  

