# Elasticsearch 与Kibana通讯及 es-head 开源管理项目

Elasticsearch 与Kibana通讯及 es-head 开源管理项目
<!--more-->
---

###  Docker 配置桥接

```bash
#　　创建网桥
docker network create -d bridge my-bridge
#  es 连接　网桥
docker network connect my-bridge es-a
#  kibana 连接网桥
docker network connect my-bridge kibana
```

### 修改kibana　配置文件

```bash
# 查看　es-a　　容器　所在　公用网桥中的　ip
sudo docker inspect es-a
 "my-bridge": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "9f05d30f9f9e"
                    ],
                    "Gateway": "172.19.0.1",
                    "IPAddress": "172.19.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:13:00:02",
                    "DriverOpts": {}
                }
# 可以看到　在网桥中的　ip　为　 "IPAddress": "172.19.0.2",
# kibana 配置文件　设置　es 容器所在的ip
elasticsearch.hosts: ["http://172.19.0.2:9200"]

# 设置完成后　需要重启容器　　才能生效（真香）
```

### 使用elasticsearch-head　开元项目对es进行管理

```bash
git clone https://github.com/mobz/elasticsearch-head.git
cd elasticsearch-head
npm install
npm run start
open http://localhost:9100/
```



### ElasticSearch　解决跨域问题(head　与　es 连接问题)

```bash
# 在es 配置文件中　加入以下配置　解决跨域问题
http.cors.enabled: true
http.cors.allow-origin: "*"
```





---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/elasticsearch-07-kibana-%E4%B8%8E-es-%E8%BF%9E%E6%8E%A5/  

