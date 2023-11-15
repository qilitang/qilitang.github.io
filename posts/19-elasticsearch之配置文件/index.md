# ElasticSearch配置文件

## 一 前言

在`elasticsearch\config`目录下，有三个核心的配置文件：

- elasticsearch.yml，es相关的配置。
- jvm.options，Java jvm相关参数的配置。
- log4j2.properties，日志相关的配置，因为es采用了[log4j](https://baike.baidu.com/item/log4j/480673?fr=aladdin)的日志框架。

这里以elasticsearch6.5.4版本为例，并且由于版本不同，配置也不太也一样，仅作参考！

## 二 elasticsearch.yml

### 2.1 Cluster

- 配置集群名称，由多个es实例组成的集群，有一个共同的名称。

```
cluster.name: my-application
```

- 集群端口设置。

```
transport.tcp.port: 9300
```

- 防止同一个shard的主副本存在同一个物理机上。

```
cluster.routing.allocation.same_shard.host:true
```

- 初始化数据恢复时，并发恢复线程的个数，默认是4个。

```
cluster.routing.allocation.node_initial_primaries_recoveries: 4
```

- 添加删除节点或者负载均衡时并发恢复线程的个数。默认是4个。

```
cluster.routing.allocation.node_concurrent_recoveries: 4
```

### 2.2 Node

- 节点名称配置，一个es实例其实是一个es进程，在集群中被称为节点。如果一个服务器上配置集群，各节点的名称不能重复。

```
node.name: node-1
```

- 为节点添加自定义属性，

```
node.attr.rack: r1
```

- 该节点是否有资格成为主节点，默认为true。

```
node.master: true
```

- 设置节点是否存储数据。

```
node.data: true
```

- 设置默认主分片的个数，默认为5片，需要说明的是，主分片一经分配则无法更改。

```
index.number_of_shards: 5
```

- 设置默认复制分片的个数，默认一个主分片对应一个复制分片，需要说明的是，复制分片可以手动调整。

```
index.number_of_replicas: 1
```

- 设置数据恢复时限制的带宽，默认0及不限制。

```
indices.recovery.max_size_per_ser: 0
```

- 设置这个参数来限制从其它分片恢复数据时最大同时打开并发流的个数，默认为5。

```
indices.recovery.concurrent_streams: 5
```

- 设置数据恢复时限制的带宽，默认0及不限制。

```
indices.recovery.max_size_per_ser: 0
```

- 设置这个参数来限制从其它分片恢复数据时最大同时打开并发流的个数，默认为5。

```
indices.recovery.concurrent_streams: 5
```

### 2.3 Paths

- 存储数据路径设置，多个路径以英文状态的逗号分隔，默认根目录下的`conf`目录。

```
path.data: /path/to/data
# path.data: /path/to/data1,/path/to/data1
```

- 设置临时文件存储路径，默认是es目录下的work目录。

```
path.work: /path/to/work
```

- 日志文件路径，默认为根目录下的`logs`目录。

```
path.logs: /path/to/logs
```

- 设置日志文件的存储路径，默认是es目录下的`logs`目录。

```
path.logs: /path/to/logs
```

- 设置插件的存放路径，默认是es目录下的`plugins`目录。

```
path.plugins: /path/to/plugins
```

### 2.4 Network

- 为es实例绑定特定的IP地址。

```
network.host: 192.168.0.1
```

上面的设置可以拆分为两个参数。

```
network.bind_host: 192.168.0.1 	# 设置绑定的ip地址，ipv4或ipv6都可以
network.publish_host: 192.168.0.1  # 设置其它节点和该节点交互的ip地址，如果不设置它会自动判断，值必须是个真实的ip地址
```

- 为es实例设置特定的端口，默认为9200端口。

```
http.port: 9200
```

### 2.5 Discovery

- 设置是否打开多播发现节点，默认是true。

```
discovery.zen.ping.multicast.enabled: true
```

- 配置es[单播发现列表](https://www.cnblogs.com/Neeo/articles/10567476.html#%E5%8D%95%E6%92%AD)，在es启动时，通过这个列表发现别的es实例，从而加入集群。

```
discovery.zen.ping.unicast.hosts: ["host1", "host2"]
discovery.zen.ping.unicast.hosts: ["10.0.0.1", "10.0.0.3:9300", "10.0.0.6[9300-9400]"]
```

- `discovery.zen.minimum_master_nodes`设置是告诉集群有多少个节点有资格成为主节点，一般的规则是集群节点数除以2（向下取整）再加一。比如3个节点集群要设置为2，这个试着是为了防止[脑裂问题](https://www.cnblogs.com/Neeo/articles/10567476.html#%E4%BB%80%E4%B9%88%E6%98%AF%E8%84%91%E8%A3%82)。
- 设置集群中自动发现其它节点时ping连接超时时间，默认为3秒，对于比较差的网络环境可以高点的值来防止自动发现时出错。

```
discovery.zen.ping.timeout: 3s
```

### 2.6 Memory

- 启动时锁定内存，默认为true，因为当jvm开始swapping时es的效率  会降低，所以要保证它不swap，可以把ES_MIN_MEM和ES_MAX_MEM两个环境变量设置成同一个值，并且保证机器有足够的内存分配给es。 同时也要允许elasticsearch的进程可以锁住内存，linux下可以通过ulimit -l unlimited命令

```
bootstrap.memory_lock: true
```

- 禁止swapping交换。

```
bootstrap.mlockall: true
```

### 2.7 Gateway

- 设置是否压缩tcp传输时的数据。默认是false不压缩。

```
transport.tcp.compress: true
```

- 设置内容的最大容量，默认是100mb。

```
http.max_content_length: 100mb
```

- 是否使用http协议对外提供服务。默认为true。

```
http.enabled: false
```

- 设置gateway的类型，默认为本地文件系统，也可以设置分布式文件系统、Hadoop的HDFS或者AWS的都可以。

```
gateway.type: local
```

- 在完全重新启动集群之后阻塞初始恢复，直到启动N个节点为止，详情参见[Recovery](https://www.cnblogs.com/Neeo/articles/10843759.html)

```
gateway.recover_after_nodes: 3
```

- 设置初始化数据恢复进程的超时时间。默认是5分钟。

```
gateway.recover_after_time: 5m
```

- 设置该集群中节点的数量，默认为2个，一旦这N个节点启动，就会立即进行数据恢复。

```
gateway.expected_nodes: 2
```

### 2.8 Various

- 删除索引时需要显式名称。

```
action.destructive_requires_name: true
```

## 三 jvm.options

- 设置jvm堆的大小，最大值和最小值，应该是一致的，并且应该根据你的物理内存决定。

```
-Xms1g     # 设置最小堆为1g
-Xmx1g		# 设置最大堆为1g
```

## 四 log4j2.properties

这个配置文件，我们一般不修改其配置。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/19-elasticsearch%E4%B9%8B%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/  

