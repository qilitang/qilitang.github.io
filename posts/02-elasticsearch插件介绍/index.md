# Elasticsearch插件介绍


## 一 Elasticsearch插件介绍

es插件是一种增强Elasticsearch核心功能的途径。它们可以为es添加自定义映射类型、自定义分词器、原生脚本、自伸缩等等扩展功能。

es插件包含JAR文件，也可能包含脚本和配置文件，并且必须在集群中的每个节点上安装。安装之后，需要重启集群中的每个节点才能使插件生效。

es插件包含核心插件和第三方插件两种

## 二 核心插件

核心插件是elasticsearch项目提供的官方插件,都是开源项目。这些插件会跟着elasticsearch版本升级进行升级,总能匹配到对应版本的elasticsearch,这些插件是有官方团队和社区成员共同开发的。

官方插件地址： [https://github.com/elastic/elasticsearch/tree/master/plugins](https://github.com/elastic/elasticsearch/tree/master/plugins)

## 三 第三方插件

    第三方插件是有开发者或者第三方组织自主开发便于扩展elasticsearch功能,它们拥有自己的许可协议,在使用它们之前需要清除插件的使用协议,不一定随着elasticsearch版本升级, 所以使用者自行辨别插件和es的兼容性。

## 四 插件安装

elasticsearch的插件安装方式还是很方便易用的。

它包含了命令行和离线安装几种方式。

它包含了命令行,url,离线安装三种方式。

核心插件随便选择一种方式安装均可，第三方插件建议使用离线安装方式

**第一种：命令行**

```python
bin/elasticsearch-plugin install [plugin_name]
# bin/elasticsearch-plugin install analysis-smartcn  安装中文分词器
```

**第二种：url安装**

```python
bin/elasticsearch-plugin install [url]
#bin/elasticsearch-plugin install https://artifacts.elastic.co/downloads/elasticsearch-plugins/analysis-smartcn/analysis-smartcn-6.4.0.zip
```

**第三种：离线安装**

```python
#https://artifacts.elastic.co/downloads/elasticsearch-plugins/analysis-smartcn/analysis-smartcn-6.4.0.zip
#点击下载analysis-smartcn离线包
#将离线包解压到ElasticSearch 安装目录下的 plugins 目录下
#重启es。新装插件必须要重启es
```

**注意：插件的版本要与 ElasticSearch 版本要一致**


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/02-elasticsearch%E6%8F%92%E4%BB%B6%E4%BB%8B%E7%BB%8D/  

