# ElasticSearch集成Django


介绍:  业务需要, 为了增加mysql一张数据库表的查询速率, 特实现es作为传统数据库使用的方案.

## django-elasticsearch 的使用

### 一 安装

```python
pip install django-elasticsearch-dsl
#  会同时安装 elasticsearch-dsl 和  elasticsearch
```

### 二 注册 + 配置

```python
INSTALLED_APPS = [
    ...
    'django_elasticsearch_dsl',
    ...
]

# Es 配置
ELASTICSEARCH_DSL={
    'default': {
        'hosts': 'localhost:9200'
    },
}
```

### 三 创建documents.py文件,做 orm 与 es 索引的映射

```python
# 注意, 在app 目录下创建 documents.py 文件   名字一定要对, 不然执行命令时 会找不到索引
from django_elasticsearch_dsl import Document, fields
from django_elasticsearch_dsl.registries import registry
from public_app.models import NcInfo

@registry.register_document
class NcInfoDocument(Document):  #  注意命名规范
     # 自定义索引字段类型  因为要作为mysql数据库的延伸 , 所以 需要自定义字段为keyword 类型. 否则会被es自动分词
    pk = fields.IntegerField()  
    base_product_name = fields.KeywordField()
    base_standard_model = fields.KeywordField()
    business_sla_category = fields.KeywordField()
    ds = fields.KeywordField()
    ip = fields.KeywordField()
    ops_azone_id = fields.KeywordField()
    ops_cluster_id = fields.KeywordField()
    ops_region_name = fields.KeywordField()
    business_nc_id = fields.KeywordField()
    business_instance_family = fields.KeywordField()

    class Index:
        name = 'nc_info'
        settings = {
            # 设置最大索引深度(**重要)  分页查询时要用到
            'max_result_window': 10000000,
            # 切片个数
            'number_of_shards':8,
            # 保存副本数
            'number_of_replicas':2
        }

    class Django:
        model = NcInfo  # 与此文档关联的模型
        # 要在Elasticsearch中建立索引的模型的字段
     	#  fields 置空 则会根据上方的对象的属性进行映射,  可直接写orm模型类字段名, 会根据orm中的字段类型进行自动选择文档字段类型
        fields = []
        # 执行迁移时的 每次从mysql中数据读取的条数. 
        queryset_pagination = 50000
```

### 四 执行迁移

```python
python manage.py search_index --rebuild
```

执行成功后 如下图所示

![image-20201215113559893](C:\Users\zhang\Desktop\工作记录\Elasticsearch 集成 django(传统数据库).assets\image-20201215113559893.png)

### 五 查看索引信息

![image-20201215151341060](C:\Users\zhang\Desktop\工作记录\Elasticsearch 集成 django(传统数据库).assets\image-20201215151341060.png)

## ElasticSearch 的搭建

### 一 安装JDK环境

因为ElasticSearch是用Java语言编写的，所以必须安装JDK的环境，并且是JDK 1.8以上，具体操作步骤自行百度

安装完成查看java版本

```
java -version
```

### 二 官网下载最新版本

下载地址[https://www.elastic.co/cn/downloads/elasticsearch],选择相应版本下载即可

![](https://tva1.sinaimg.cn/large/006tNbRwgy1g9kts83vzbj31o80tsjvo.jpg#alt=image)

### 三 下载其他版本

直接点击https://www.elastic.co/cn/downloads/past-releases#elasticsearch

![](https://tva1.sinaimg.cn/large/006tNbRwgy1g9kttpffz5j31320u0ags.jpg#alt=image)

### 三 下载完成，启动

解压文件，切换到解压文件路径下，执行

```
cd elasticsearch-<version> #切换到路径下
./bin/elasticsearch  #启动es
#如果你想把 Elasticsearch 作为一个守护进程在后台运行，那么可以在后面添加参数 -d 。

#如果你是在 Windows 上面运行 Elasticseach，你应该运行 bin\elasticsearch.bat 而不是 bin\elasticsearch
```

### 四 测试启动是否成功

在浏览器输入以下地址：[http://127.0.0.1:9200/](http://127.0.0.1:9200/)

即可看到如下内容：

```
{
  "name" : "lqzMacBook.local",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "G1DFg-u6QdGFvz8Z-XMZqQ",
  "version" : {
    "number" : "7.5.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "e9ccaed468e2fac2275a3761849cbee64b39519f",
    "build_date" : "2019-11-26T01:06:52.518245Z",
    "build_snapshot" : false,
    "lucene_version" : "8.3.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

### 五 关闭es

```
#查看进程
ps -ef | grep elastic
#干掉进程
kill -9 2382（进程号）
#以守护进程方式启动es
elasticsearch -d
```

### 六 Es 集群配置

#### node1

```yml
# 集群名称
cluster.name: online-virt-elasticsearch
# 节点名称
node.name: es-node-1
# 是否可以成为master节点
node.master: true
# 是否允许该节点存储数据,默认开启
node.data: true
# 网络绑定,写自己的主机
network.host: 0.0.0.0
# 设置对外服务的http端口，默认为9200,为和单机分开，我设置9201
http.port: 9200
# 设置节点间交互的tcp端口,默认是9300，为和单机分开，我设置9301
transport.tcp.port: 9401
# 手动指定可以成为 mater 的所有节点的 name 或者 ip，这些配置将会在第一次选举中进行计算
cluster.initial_master_nodes: ["es-node-1","es-node-2","es-node-3","es-node-4","es-node-5"]
 ##设置集群自动发现机器ip的集合
discovery.seed_hosts: ["localhost:9401","localhost:9402","localhost:9403", "localhost:9404","localhost:9405"]
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
#允许跨域访问
http.cors.enabled: true
http.cors.allow-origin: "*"
```

## 封装  elasticsearch  集成 drf  (只查询接口使用)

elasticsearch 自定义包封装

- 
`__init__` .py
```python
from .filterset import EsFilterSet
from .elasticsearch_generic import ElasticSearch
from .pagination import EsStandardPagination
```


- 
elasticsearch_generic.py
```python
# -*- coding: utf-8 -*-
from elasticsearch_dsl.document import IndexMeta
from . import filter_backend

class ElasticSearch(object):
    search_object = None
    es_filter_class = None
    es_paginate_class = None
    es_paginator = None

    def get_search_object(self):
        assert self.search_object is not None, (
                "'%s' should either include a `search_object` attribute, "
                "or override the `get_search_object()` method."
                % self.__class__.__name__
        )
        search_object = self.search_object
        if isinstance(search_object, IndexMeta):
            search_object = search_object.search()
        return search_object

    def filter_search_object(self, search_object):

        if self.es_filter_class:
            return filter_backend.EsFilterBackend().filter_search_object(self.request, search_object, self)
        return search_object

    def paginate_search(self, search_obj, sort=None):
        if self.es_paginate_class:
            self.es_paginator = self.es_paginate_class(search_obj, self.request, sort)
            return self.es_paginator.paginate

    def get_es_paginated_response(self, data):
        """
        Return a paginated style `Response` object for the given output data.
        """
        return self.es_paginator.get_paginated_response(data)
```


- 
filter_backend.py
```python

# -*- coding: utf-8 -*-
class EsFilterBackend(object):
    @staticmethod
    def get_es_filter_class(view, search_object=None):
        es_filter_class = getattr(view, 'es_filter_class', None)

        if es_filter_class:
            filter_model = es_filter_class.Meta.model
            assert issubclass(search_object._model, filter_model), \
                'EsFilterSet model %s does not match search_object model %s' % \
                (filter_model, search_object.model)
            return es_filter_class

    def filter_search_object(self, request, search_object, view):
        es_filter_class = self.get_es_filter_class(view, search_object)

        if es_filter_class:
            return es_filter_class(request.query_params, search_object=search_object, request=request).qs

        return search_object
```


- 
filters.py
```python
# -*- coding: utf-8 -*-
from django_elasticsearch_dsl import fields
from elasticsearch_dsl import Q


class Filter(object):
    field_class = fields.Field

    def __init__(self, field_name=None, lookup_expr='exact', **kwargs):
        self.field_name = field_name
        if field_name is None and 'name' in kwargs:
            self.field_name = kwargs.pop('name')
        self.lookup_expr = lookup_expr

    def filter(self, name, params):
        if not params.get(name):
            return
        if self.lookup_expr == 'term':
            param = params.get(name)
            key = 'term'
            if '&' in param:
                values = param.split('&')
                key = 'terms'
            elif ',' in param:
                values = param.split(',')
                key = 'terms'
            else:
                values = param
            query = Q(key, **{self.field_name: values})
        elif self.lookup_expr == 'gte' or self.lookup_expr == "lte":
            m = {self.lookup_expr: params.get(name)}
            q = {self.field_name: m}
            # query = {'range': q}
            query = Q('range', **q)
        else:
            raise Exception("{} can not set lookup_expr='{}'".format(self.field_name, self.lookup_expr))
        return query


class KeywordField(Filter):
    field_class = fields.KeywordField


class IntegerField(Filter):
    field_class = fields.IntegerField


class DateField(Filter):
    field_class = fields.DateField
```


- 
filterset.py
```python
# -*- coding: utf-8 -*-
import copy
from collections import OrderedDict
from elasticsearch_dsl import Q


class FilterSetOptions(object):
    def __init__(self, options=None):
        self.model = getattr(options, 'model', None)
        self.fields = getattr(options, 'fields', None)
        self.exclude = getattr(options, 'exclude', None)


class FilterSetMetaclass(type):
    def __new__(cls, name, bases, attrs):
        new_class = super(FilterSetMetaclass, cls).__new__(cls, name, bases, attrs)
        new_class._meta = FilterSetOptions(getattr(new_class, 'Meta', None))
        new_class.base_filters = new_class.get_filters()
        return new_class


class BaseFilterSet(object):

    def __init__(self, data=None, search_object=None, request=None):
        self.params = data
        self.search_object = search_object
        self.is_bound = data is not None
        self.request = request
        self.filters = copy.deepcopy(self.base_filters)

    @property
    def qs(self):
        if not hasattr(self, '_eqs'):
            if not self.is_bound:
                self._eqs = self.search_object
                return self._eqs
            _eqs = self.search_object
            must_query = []
            for name, filter_ in self.filters.items():
                query_ = filter_.filter(name, self.params)
                if query_:
                    must_query.append(query_)
            q = Q('bool', must=must_query)
            self._eqs = _eqs.query(q)
        return self._eqs

    @classmethod
    def get_filters(cls):
        filters = OrderedDict()
        if not cls._meta.model:
            return
        fields = cls._meta.fields
        for key in fields:
            assert getattr(cls, key, None), \
                'EsFilterSet filter field %s does not match  %s fields ' % \
                (key, cls.__name__)
            filters.update({key: getattr(cls, key)})
        return filters


class EsFilterSet(BaseFilterSet):
    __metaclass__ = FilterSetMetaclass
    pass
```


- 
pagination.py
```python
# -*- coding: utf-8 -*-

from utils.response import APIResponse


class EsStandardPagination(object):
    page_size = 20
    page_size_query_param = 'per_page'
    page_query_param = "page"  # url参数
    max_page_size = 100

    def __init__(self, search_object, request, sort=None):
        self.search_object = search_object
        self.params = request.query_params
        self.page = 1
        self.count = search_object.count()
        self.total_page = 0
        self.sort = {'pk': {'order': 'asc'}} if not sort else sort

    @property
    def paginate(self):
        self.page_size = int(self.params.get(self.page_size_query_param) if self.params.get(
            self.page_size_query_param) else self.page_size)
        self.page = int(self.params.get(self.page_query_param) if self.params.get(self.page_query_param) else 1)
        start = (self.page - 1) * self.page_size
        end = start + self.page_size
        s = self.search_object.sort(self.sort)
        self.total_page = (self.count // self.page_size) + 1
        queryset = s[start:end].to_queryset()
        return queryset

    def get_paginated_response(self, data):
        return APIResponse(data={
            "count": self.count,
            "page": self.page,
            "per_page": self.page_size,
            "total_page": self.total_page,
            # 'next': self.get_next_link(),
            # 'previous': self.get_previous_link(),
            "results": data
        })
```



## 封装包的使用, 快速写接口

### 视图

```python
# 导入自定义的 ElasticSearch 和 EsStandardPagination
from utils.elasticsearch import ElasticSearch, EsStandardPagination

# 继承  GenericAPIView 和 ElasticSearch
class NcInfoShowEsView(GenericAPIView, ElasticSearch):
    # 获取 search_object, 此为 documents 定义的文档类. 
    search_object = documents.NcInfoDocument.search()
    # 搜索类
    es_filter_class = es_filter.NcInfoFilter
    # 分页类
    es_paginate_class = EsStandardPagination

    def get(self, request):
        s = self.filter_search_object(self.get_search_object())
        # 分页组件会把分页后的es对象 转换成 queryset 对象. paginate_search 有两个参数, 一是查询后的 search对象, 第二个是排序参数, 默认是 {'pk': {'order': 'asc'}} pk为从orm 获取的 id,或 pk . 
        page = self.paginate_search(s)
        if page is not None:
            # 视图中 分页组件会把分页后的es对象 转换成 queryset 对象.  可以使用序列化类进行序列化. 完成
            serializer = serializers.NcInfoSerializer(page, many=True)
            return self.get_es_paginated_response(serializer.data)
        serializer = serializers.NcInfoSerializer(page, many=True)
        return APIResponse(data=serializer.data)
```

### 过滤组件

```python
from . import models
from utils.elasticsearch import EsFilterSet
from utils.elasticsearch import filters

# 继承自定义esFilterSet
class NcInfoFilter(EsFilterSet):
    # 前端携带参数来的key  对应的文档字段类型
    # field_name:  对应的es索引 文档中的字段
   	# lookup_expr: 过滤方式, 目前有 term方式 为精准匹配.  lte, gte  大于小于.
    base_product_name = filters.KeywordField(field_name='base_product_name', lookup_expr='term')
    business_sla_category = filters.KeywordField(field_name='business_sla_category', lookup_expr='term')
    ds = filters.KeywordField(field_name='ds', lookup_expr='term')

    class Meta:
        # 过滤所对应的orm表
        model = models.NcInfo
		# 参与过滤的字段, 需要在此注册, 否则不参与过滤. 
        fields = ['base_product_name', 'business_sla_category', 'ds', 'business_instance_family', 'ops_cluster_id',
                  'ops_region_name', 'ops_azone_id','base_standard_model']
```

### 聚合查询

```python
#  聚合查询 elasticsearch  没有提供很好的接口.  目前使用 collapse 进行聚合查询
class NcInfoOptionsView(GenericAPIView, ElasticSearch):
    """
    nc_info数据展示列表页筛选数据
    """
    search_object = documents.NcInfoDocument.search()
    es_filter_class = es_filter.NcInfoFilter

    def get(self, request, *args, **kwargs):
        params_map = {'standard_model_list': 'base_standard_model', 'region_name_list': 'ops_region_name',
                      'azone_id_list': 'ops_azone_id', 'cluster_id_list': 'ops_cluster_id',
                      'instance_family_list': 'business_instance_family'}
        results = {}
        all_s = self.get_search_object()
        sla_category_list = all_s.extra(collapse={'field': 'business_sla_category'})
        results.update({'sla_category_list': [getattr(i, 'business_sla_category', None) for i in sla_category_list]})
        product_name_list = all_s.extra(collapse={'field': 'base_product_name'})
        results.update({'product_name_list': [getattr(i, 'base_product_name', None) for i in product_name_list]})
        s = self.filter_search_object(all_s)
        for key, param in params_map.items():
            s = s.extra(collapse={'field': param})
            results.update({key: [getattr(i, param) for i in s[0:100]]})

        return APIResponse(data=results)

# 使用 search对象的 .extra()  进行聚合查询 具体请去看官方文档
```

## 使用 go-mysql-elasticsearch 对 mysql 数据库进行数据的同步.

**如果所有的数据都是通过orm 进行操作 则可以使用此方法, 不需要使用 go-mysql-elasticsearch **

django-elasticsearch-dsl 可以使用信号量对mysql数据自动更新同步进es  只需要在 settings.py 文件中 添加以下配置

```python
# 实时同步 用的是 django的信号量, 不需要额外的参数
ELASTICSEARCH_DSL_SIGNAL_PROCESSOR = 'django_elasticsearch_dsl.signals.RealTimeSignalProcessor'
```

### mysql 开启 binlog

```cnf
server_id = 7578
log_bin = /apsarapangu/disk8/mysql/mysql-bin/bin-log
binlog_format = ROW
```

### 下载go-mysql-elasticsearch

### 编译

```bash
cd /apsarapangu/disk8/go-mysql-elasticsearch
make
```

### 配置文件

配置文件在 项目目录的 etc文件夹下, river.toml

```toml
# MySQL address, user and password
# user must have replication privilege in MySQL.
my_addr = "127.0.0.1:3306"
my_user = "root"
my_pass = "virtweb@2017"
my_charset = "utf8"

# Set true when elasticsearch use https
#es_https = false
# Elasticsearch address
es_addr = "localhost:9200"
# Elasticsearch user and password, maybe set by shield, nginx, or x-pack
es_user = ""
es_pass = ""

# Path to store data, like master.info, if not set or empty,
# we must use this to support breakpoint resume syncing.
# TODO: support other storage, like etcd.
data_dir = "./var"

# Inner Http status address
stat_addr = "127.0.0.1:12800"
stat_path = "/metrics"

# pseudo server id like a slave
server_id = 1001

# mysql or mariadb
flavor = "mysql"

# mysqldump execution path
# if not set or empty, ignore mysqldump.
# mysqldump = "mysqldump"

# if we have no privilege to use mysqldump with --master-data,
# we must skip it.
#skip_master_data = false

# minimal items to be inserted in one bulk
bulk_size = 128

# force flush the pending requests if we don't have enough items >= bulk_size
flush_bulk_time = "200ms"

# Ignore table without primary key
skip_no_pk_table = false

# MySQL data source
[[source]]
schema = "virt_dashboard"

# Only below tables will be synced into Elasticsearch.
# "t_[0-9]{4}" is a wildcard table format, you can use it if you have many sub tables, like table_0000 - table_1023
# I don't think it is necessary to sync all tables in a database.
tables = ["nc_info"]

[[rule]]
schema = "virt_dashboard"
table = "nc_info"
index = "nc_info"
type = "_doc"
(virt-dashboard)
```

### 执行监控

```bash
./bin/go-mysql-elasticsearch --config=./etc/river.toml 
# 加 & 为后台运行
```

**注意**  在go-mysql-elasticsearch 目录下会 生成 var 文件夹.  里面有 监测mysql binlog 日志的记录, 当 go-mysql-elasticsearch 意外停止后 启动后会从停止的节点开始继续同步, 不必担心数据的丢失.

## elasticsearch-head 的使用

此工具需要npm

### 下载

### 安装依赖

```javascript
npm install
```

### 运行

```javascript
npm run start
```

运行成功后 会出现此界面

![image-20201215151450248](C:\Users\zhang\Desktop\工作记录\Elasticsearch 集成 django(传统数据库).assets\image-20201215151450248.png)

### Kibana 安装及使用

**Kibana 应该与 es 保持版本的一致**

自己百度吧. 很简单 不想写了.

### Kibana的配置

```yml
# Kibana is served by a back end server. This setting specifies the port to use.
server.port: 5601

server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
xpack.monitoring.ui.container.elasticsearch.enabled: false
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/21-elasticsearch-%E9%9B%86%E6%88%90-django%E4%BC%A0%E7%BB%9F%E6%95%B0%E6%8D%AE%E5%BA%93/  

