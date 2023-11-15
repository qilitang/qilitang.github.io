# 中文分词介绍

## 一 中文分词介绍

elasticsearch提供了几个内置的分词器：standard analyzer(标准分词器)、simple analyzer(简单分词器)、whitespace analyzer（空格分词器）、language analyzer（语言分词器）

而如果我们不指定分词器类型的话，elasticsearch默认是使用标准分词器的

我们需要下载中文分词插件，来实现中文分词

## 二 下载

地址为：[https://github.com/medcl/elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik)

安装方式参照【02-ElasticSearch之-插件介绍】

```python
#我们采用第二种，url安装
./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.4.2/elasticsearch-analysis-ik-7.4.2.zip
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/05-elasticsearch%E4%B9%8B-%E4%B8%AD%E6%96%87%E5%88%86%E8%AF%8D%E5%99%A8/  

