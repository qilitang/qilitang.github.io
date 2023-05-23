# Celery


celery 的简单使用
<!--more-->

## 官方

Celery 官网：http://www.celeryproject.org/

Celery 官方文档英文版：http://docs.celeryproject.org/en/latest/index.html

Celery 官方文档中文版：http://docs.jinkan.org/docs/celery/



## Celery异步任务框架

```python
"""
1）可以不依赖任何服务器，通过自身命令，启动服务(内部支持socket)
2）celery服务为为其他项目服务提供异步解决任务需求的
注：会有两个服务同时运行，一个是项目服务，一个是celery服务，项目服务将需要异步处理的任务交给celery服务，celery就会在需要时异步完成项目的需求

人是一个独立运行的服务 | 医院也是一个独立运行的服务
	正常情况下，人可以完成所有健康情况的动作，不需要医院的参与；但当人生病时，就会被医院接收，解决人生病问题
	人生病的处理方案交给医院来解决，所有人不生病时，医院独立运行，人生病时，医院就来解决人生病的需求
"""
```





## Celery架构

Celery的架构由三部分组成，消息中间件（message broker）、任务执行单元（worker）和 任务执行结果存储（task result store）组成。

![](/images/image-20200505214835683111.png)

### 消息中间件

Celery本身不提供消息服务，但是可以方便的和第三方提供的消息中间件集成。包括，RabbitMQ, Redis等等

### 任务执行单元

Worker是Celery提供的任务执行的单元，worker并发的运行在分布式的系统节点中。

### 任务结果存储

Task result store用来存储Worker执行的任务的结果，Celery支持以不同方式存储任务的结果，包括AMQP, redis等

### 使用场景

异步执行：解决耗时任务

延迟执行：解决延迟任务

定时执行：解决周期(周期)任务



## Celery的安装配置

pip install celery

消息中间件：RabbitMQ/Redis

app=Celery('任务名', broker='xxx', backend='xxx')

### 两种celery任务结构：提倡用包管理，结构更清晰

```python
# 如果 Celery对象:Celery(...) 是放在一个模块下的
# 1）终端切换到该模块所在文件夹位置：scripts
# 2）执行启动worker的命令：celery worker -A 模块名 -l info -P eventlet
# 注：windows系统需要eventlet支持，Linux与MacOS直接执行：celery worker -A 模块名 -l info
# 注：模块名随意


# 如果 Celery对象:Celery(...) 是放在一个包下的
# 1）必须在这个包下建一个celery.py的文件，将Celery(...)产生对象的语句放在该文件中
# 2）执行启动worker的命令：celery worker -A 包名 -l info -P eventlet
# 注：windows系统需要eventlet支持，Linux与MacOS直接执行：celery worker -A 模块名 -l info
# 注：包名随意
```

## Celery执行异步任务

### 包架构封装

```python
project
    ├── celery_task  	# celery包
    │   ├── __init__.py # 包文件
    │   ├── celery.py   # celery连接和配置相关文件，且名字必须交celery.py
    │   └── tasks.py    # 所有任务函数
    ├── add_task.py  	# 添加任务
    └── get_result.py   # 获取结果
```



### 基本使用

#### celery.py

```python
# 1）创建app + 任务

# 2）启动celery(app)服务：
# 非windows
# 命令：celery worker -A celery_task -l info
# windows：
# pip3 install eventlet
# celery worker -A celery_task -l info -P eventlet

# 3）添加任务：手动添加，要自定义添加任务的脚本，右键执行脚本

# 4）获取结果：手动获取，要自定义获取任务的脚本，右键执行脚本


from celery import Celery
broker = 'redis://127.0.0.1:6379/1'
backend = 'redis://127.0.0.1:6379/2'
app = Celery(broker=broker, backend=backend, include=['celery_task.tasks'])
```

#### tasks.py

```python
from .celery import app
import time
@app.task
def add(n, m):
    print(n)
    print(m)
    time.sleep(10)
    print('n+m的结果：%s' % (n + m))
    return n + m

@app.task
def low(n, m):
    print(n)
    print(m)
    print('n-m的结果：%s' % (n - m))
    return n - m
```

#### add_task.py

```python
from celery_task import tasks

# 添加立即执行任务
t1 = tasks.add.delay(10, 20)
t2 = tasks.low.delay(100, 50)
print(t1.id)


# 添加延迟任务
from datetime import datetime, timedelta
eta=datetime.utcnow() + timedelta(seconds=10)
tasks.low.apply_async(args=(200, 50), eta=eta)
```

#### get_result.py

```python
from celery_task.celery import app

from celery.result import AsyncResult

id = '21325a40-9d32-44b5-a701-9a31cc3c74b5'
if __name__ == '__main__':
    async = AsyncResult(id=id, app=app)
    if async.successful():
        result = async.get()
        print(result)
    elif async.failed():
        print('任务失败')
    elif async.status == 'PENDING':
        print('任务等待中被执行')
    elif async.status == 'RETRY':
        print('任务异常后正在重试')
    elif async.status == 'STARTED':
        print('任务已经开始被执行')
```



### 高级使用

#### celery.py

```python
# 1）创建app + 任务

# 2）启动celery(app)服务：
# 非windows
# 命令：celery worker -A celery_task -l info
# windows：
# pip3 install eventlet
# celery worker -A celery_task -l info -P eventlet

# 3）添加任务：自动添加任务，所以要启动一个添加任务的服务
# 命令：celery beat -A celery_task -l info

# 4）获取结果


from celery import Celery

broker = 'redis://127.0.0.1:6379/1'
backend = 'redis://127.0.0.1:6379/2'
app = Celery(broker=broker, backend=backend, include=['celery_task.tasks'])


# 时区
app.conf.timezone = 'Asia/Shanghai'
# 是否使用UTC
app.conf.enable_utc = False

# 任务的定时配置
from datetime import timedelta
from celery.schedules import crontab
app.conf.beat_schedule = {
    'low-task': {
        'task': 'celery_task.tasks.low',
        'schedule': timedelta(seconds=3),
        # 'schedule': crontab(hour=8, day_of_week=1),  # 每周一早八点
        'args': (300, 150),
    }
}
```

#### tasks.py

```python
from .celery import app

import time
@app.task
def add(n, m):
    print(n)
    print(m)
    time.sleep(10)
    print('n+m的结果：%s' % (n + m))
    return n + m


@app.task
def low(n, m):
    print(n)
    print(m)
    print('n-m的结果：%s' % (n - m))
    return n - m
```

#### get_result.py

```python
from celery_task.celery import app

from celery.result import AsyncResult

id = '21325a40-9d32-44b5-a701-9a31cc3c74b5'
if __name__ == '__main__':
    async = AsyncResult(id=id, app=app)
    if async.successful():
        result = async.get()
        print(result)
    elif async.failed():
        print('任务失败')
    elif async.status == 'PENDING':
        print('任务等待中被执行')
    elif async.status == 'RETRY':
        print('任务异常后正在重试')
    elif async.status == 'STARTED':
        print('任务已经开始被执行')
```



### django中使用

#### celery.py

```python
"""
celery框架django项目工作流程
1）加载django配置环境
2）创建Celery框架对象app，配置broker和backend，得到的app就是worker
3）给worker对应的app添加可处理的任务函数，用include配置给worker的app
4）完成提供的任务的定时配置app.conf.beat_schedule
5）启动celery服务，运行worker，执行任务
6）启动beat服务，运行beat，添加任务

重点：由于采用了django的反射机制，使用celery.py所在的celery_task包必须放置项目的根目录下
"""

# 一、加载django配置环境
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "luffyapi.settings.dev")

# 二、加载celery配置环境
from celery import Celery
# broker
broker = 'redis://127.0.0.1:6379/0'
# backend
backend = 'redis://127.0.0.1:6379/1'
# worker
app = Celery(broker=broker, backend=backend, include=['celery_task.tasks'])


# 时区
app.conf.timezone = 'Asia/Shanghai'
# 是否使用UTC
app.conf.enable_utc = False

# 任务的定时配置
from datetime import timedelta
from celery.schedules import crontab
app.conf.beat_schedule = {
    'update-banner-list': {
        'task': 'celery_task.tasks.update_banner_list',
        'schedule': timedelta(seconds=10),
        'args': (),
    }
}
```

#### tasks.py

```python
from .celery import app

from django.core.cache import cache
from home import models, serializers
from django.conf import settings
@app.task
def update_banner_list():
    queryset = models.Banner.objects.filter(is_delete=False, is_show=True).order_by('-orders')[:settings.BANNER_COUNT]
    banner_list = serializers.BannerSerializer(queryset, many=True).data
    # 拿不到request对象，所以头像的连接base_url要自己组装
    for banner in banner_list:
        banner['image'] = 'http://127.0.0.1:8000%s' % banner['image']

    cache.set('banner_list', banner_list, 86400)
    return True
```

















---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/celery/  

