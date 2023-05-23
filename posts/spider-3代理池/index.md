# 代理池

爬虫 代理池 模块使用
<!--more-->

## 免费代理池

```python
# https://github.com/jhao104/proxy_pool
# 收费的：提供给你一个接口，每掉一次这个接口，获得一个代理
# 免费：用爬虫爬取，免费代理，放到我的库中，flask，django搭一个服务（删除代理，自动测试代理可用性），每次发一个请求，获取一个代理

# 带你配置

# 1 下载，解压，用pycharm打开
# 2 安装依赖 pip install -r requirements.txt
# 3 配置Config/setting.py:
	DB_TYPE = getenv('db_type', 'redis').upper()
	DB_HOST = getenv('db_host', '127.0.0.1')
	DB_PORT = getenv('db_port', 6379)
	DB_PASSWORD = getenv('db_password', '')
# 4 本地启动redis-server

# 5 可以在cli目录下通过ProxyPool.py
	-python proxyPool.py schedule :调度程序，他会取自动爬取免费代理
  -python proxyPool.py webserver:启动api服务，把flask启动起来
```

## 验证码破解

```python
# 1 简单验证码，字母，数字
# 2 高级的，选择，你好，12306选择乒乓球，滑动验证（极验）

# 打码平台（自动破解验证码，需要花钱）云打码，超级鹰（12306）
http://www.yundama.com/
http://www.chaojiying.com/

# 注册账号，（充钱）把demo下载下来，运行即可
```

## 爬取段子发送给女朋友

```python

#####
# 1 爬取糗事百科，微信自动发送
#####
# https://www.qiushibaike.com/text/
# https://www.qiushibaike.com/text/page/1/

import requests
from bs4 import BeautifulSoup
ret=requests.get('https://www.qiushibaike.com/text/page/1/')
# print(ret.text)
ll=[]
soup=BeautifulSoup(ret.text,"lxml")
article_list=soup.find_all(name='div',id=True,class_='article')
for article in article_list:
    content=article.find(name='div',class_='content').span.text
    # content=article.find(name='div',class_='content').text
    # content=article.find(class_='content').text
    # print(content)
    # 入库
    #我们放到列表中
    ll.append(content)
print(ll)

# 微信自动发消息
# wxpy：实现了web微信的接口
# pip3 install wxpy
from wxpy import *
# 实例化得到一个对象，微信机器人对象
import random
bot=Bot(cache_path=True)

@bot.register() # 接收从指定好友发来的消息，发送者即recv_msg.sender为指定好友girl_friend
def recv_send_msg(recv_msg):
    print('收到的消息：',recv_msg.text) # recv_msg.text取得文本
    return random.choice(ll)

embed()
```



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/spider-3%E4%BB%A3%E7%90%86%E6%B1%A0/  

