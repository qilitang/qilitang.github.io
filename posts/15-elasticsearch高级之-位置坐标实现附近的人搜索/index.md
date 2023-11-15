# ElasticSearch位置做不及附近的人搜索

## 一 创建mapping

```python
PUT test
{
  "mappings": {
    "test":{
      "properties": {
        "location":{
          "type": "geo_point"
        }
      }
    }
  }
}
```

## 二 导入数据

```python
POST test/test
{
  "location":{
    "lat":12,
    "lon":24
  }
}
```

## 三 查询

### 3.1根据给定两个点组成的矩形，查询矩形内的点

```python
GET test/test/_search
{
  "query": {
    "geo_bounding_box": {
      "location": {
        "top_left": {
          "lat": 28,
          "lon": 10
        },
        "bottom_right": {
          "lat": 10,
          "lon": 30
        }
      }
    }
  }
}
```

### 3.2根据给定的多个点组成的多边形，查询范围内的点

```python
GET test/test/_search
{
  "query": {
    "geo_polygon": {
      "location": {
        "points": [
          {
            "lat": 11,
            "lon": 25
          },
          {
            "lat": 13,
            "lon": 25
          },
          {
            "lat": 13,
            "lon": 23
          },
          {
            "lat": 11,
            "lon": 23
          }
        ]
      }
    }
  }
}
```

### 3.3查询给定1000KM距离范围内的点

```python
GET test/test/_search
{
  "query": {
    "geo_distance": {
      "distance": "1000km",
      "location": {
        "lat": 12,
        "lon": 23
      }
    }
  }
}
```

### 3.4查询距离范围区间内的点的数量

```python
GET test/test/_search
{
  "size": 0, 
  "aggs": {
    "myaggs": {
      "geo_distance": {
        "field": "location",
        "origin": {
          "lat": 52.376,
          "lon": 4.894
        },
        "unit": "km", 
        "ranges": [
          {
            "from": 50, 
            "to": 30000
          }
        ]
      }
    }
  }
}
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/15-elasticsearch%E9%AB%98%E7%BA%A7%E4%B9%8B-%E4%BD%8D%E7%BD%AE%E5%9D%90%E6%A0%87%E5%AE%9E%E7%8E%B0%E9%99%84%E8%BF%91%E7%9A%84%E4%BA%BA%E6%90%9C%E7%B4%A2/  

