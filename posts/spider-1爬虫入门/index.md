# Request模块

爬虫 requests 模块使用
<!--more-->

### 爬虫介绍

```python
# 1 本质：模拟发送http请求（requests）----》解析返回数据（re，bs4，lxml，json）---》入库（redis，mysql，mongodb）
# 2 app爬虫：本质一模一样
# 3 为什么python做爬虫最好：包多，爬虫框架：scrapy：性能很高的爬虫框架，爬虫界的django，大而全（爬虫相关的东西都集成了）
# 4 百度，谷歌，就是个大爬虫  在百度搜索，其实是去百度的服务器的库搜的，百度一直开着爬虫，一刻不停的在互联网上爬取，把页面存储到自己库中
# 5 全文检索：全文检索
```

### requests  模块

requests模块是基于urllib2 内置库的基础上封装的一个模块, 被广泛应用

-  安装方法

  `pip3 install requests`

#### 基本使用

- 返回值的属性

```python
import requests

# # 发送http请求
# # get,delete,post。。本质都是调用request函数
# ret=requests.get('https://www.cnblogs.com')
# print(ret.status_code) # 响应状态码
# print(ret.text)  # 响应体，转成了字符串
# print(ret.content) # 响应体，二进制
# ret=requests.post()\
# ret=requests.request("get",)
# ret=requests.delete()

## get 请求 带参数

```

-  get请求带参数

  方式一

  ```python
  ret = requests.get('https://www.baidu.com/',
                      headers={
                           'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36',
                                            })
  ```

  方式二（建议用方式二）中文会自动转码

  ```python
   ret=requests.get('http://0.0.0.0:8001/',params={'name':"美女",'age':18})
   print(ret.text)
  ```

- 带 headers

  ```python
  
  ret = requests.get('http://0.0.0.0:8001/?name=%E7%BE%8E%E5%A5%B3',
                     headers={
                         # 标志，什么东西发出的请求，浏览器信息，django框架，从哪取？（meta）
                         'User-Agent': 'request',
                        # 上一个页面的地址，图片防盗链
                         'Referer': 'xxx'
                    })
  print(ret)
  
  # 图片防盗链：如果图片的referer不是我自己的网站，就直接禁止掉
  <img src="https://www.lgstatic.com/lg-community-fed/community/modules/common/img/avatar_default_7225407.png">
  ```

- 带 cookie

  ```python
  # 带cookie,随机字符串(用户信息：也代表session)，不管后台用的token认证，还是session认证
  # 一旦登陆了，带着cookie发送请求，表示登陆了（下单，12306买票，评论）
  ```

  方式一

  ```python
  ret = requests.get('http://0.0.0.0:8001/?name=%E7%BE%8E%E5%A5%B3',
                     headers={
                         'cookie': 'key3=value;key2=value',
                     })
  ```

  方式二

  ```python
  ret = requests.get('http://0.0.0.0:8001/?name=%E7%BE%8E%E5%A5%B3',
                     cookies={"islogin":"xxx"})
  print(ret)
  ```

- 发送post 请求

  ```python
  5 发送post请求（注册，登陆），携带数据（body）
  data=None, json=None
  # data:urlencoded编码
  ret=requests.post('http://0.0.0.0:8001/',data={'name':"lqz",'age':18})
  # json:json编码
  import json
  data=json.dumps({'name':"lqz",'age':18})
  ret=requests.post('http://0.0.0.0:8001/',json=data)
  print(ret)
  # 注意：编码格式是请求头中带的，所有我可以手动修改，在headers中改
  ```

- session 对象

  ```python
  session=requests.session()
  # 跟requests.get/post用起来完全一样，但是它处理了cookie
  # 假设是一个登陆，并且成功
  session.post()
  # 再向该网站发请求，就是登陆状态，不需要手动携带cookie
  session.get("地址")
  ```

- 响应对象

  ```python
  print(respone.text)   # 响应体转成str
  print(respone.content) # 响应体二进制（图片，视频）
  print(respone.status_code) # 响应状态码
  print(respone.headers) # 响应头
  print(respone.cookies) # 服务端返回的cookie
  print(respone.cookies.get_dict()) # 转成字典
  print(respone.cookies.items())
  print(respone.url)  # 当次请求的地址
  
  
  print(respone.history) # 如果有重定向，放到一个列表中
  ret=requests.post('http://0.0.0.0:8001/')
  ret=requests.get('http://0.0.0.0:8001/admin')
  #不要误解
  ret=requests.get('http://0.0.0.0:8001/user')
  print(ret.history)
  
  print(respone.encoding) # 编码方式
  response.iter_content() # 视频，图片迭代取值
  with open("a.mp4",'wb') as f:
      for line in response.iter_content():
          f.write(lin
  ```

- 乱码问题

  ```python
  # 加载回来的页面，打印出来，乱码（我们用的是utf8编码），如果网站用gbk，
  ret.encoding='gbk'
  ret=requests.get('http://0.0.0.0:8001/user')
  # ret.apparent_encoding当前页面的编码
  ret.encoding=ret.apparent_encoding
  ```

- 解析json

  ```python
  # 返回数据，有可能是json格式，有可能是html格式
  ret=requests.get('http://0.0.0.0:8001/')
  print(type(ret.text))
  print(ret.text)
  #
  a=ret.json()
  print(a['name'])
  print(type(a))
  ```

- 使用代理 proxies={}

  ```python
  # 正向代理
  # django如何拿到客户端ip地址 META.get("REMOTE_ADDR")
  # 如何去获取代理，如何使用（用自己项目验收）
  # 使用代理有什么用
  ret=requests.get('http://0.0.0.0:8001/',proxies={'http':'地址'})
  print(type(ret.text))
  print(ret.text)
  ```

- 异常处理

  ```python
  # 用try except捕获一下 就用它就型了：Exception
  ```

- 上传文件 (爬虫用的比较少，后台写服务，)

  ```python
  file={'myfile':open("1.txt",'rb')}
  ret=requests.post('http://0.0.0.0:8001/',files=file)
  print(ret.content)
  ```





### 爬梨视频

```python
############
# 2 爬取视频
#############
#categoryId=9 分类id
#start=0 从哪个位置开始，每次加载12个
# https://www.pearvideo.com/category_loading.jsp?reqType=5&categoryId=9&start=0

import requests
import re
ret=requests.get('https://www.pearvideo.com/category_loading.jsp?reqType=5&categoryId=9&start=0')
# print(ret.text)
# 正则取解析
reg='<a href="(.*?)" class="vervideo-lilink actplay">'
video_urls=re.findall(reg,ret.text)
print(video_urls)
for url in video_urls:
    ret_detail=requests.get('https://www.pearvideo.com/'+url)
    reg='srcUrl="(.*?)",vdoUrl=srcUrl'
    mp4_url=re.findall(reg,ret_detail.text)[0] #type:str
    # 下载视频
    video_content=requests.get(mp4_url)
    video_name=mp4_url.rsplit('/',1)[-1]
    with open(video_name,'wb') as f:
        for line in video_content.iter_content():
            f.write(line)

```

### 模拟登陆

```python
############
# 3 模拟登陆某网站
#############

import requests

ret = requests.post('http://www.aa7a.cn/user.php',
                    data={
                        'username': '616564099@qq.com',
                        'password': 'lqz123',
                        'captcha': 'f5jn',
                        'remember': '1',
                        'ref': 'http://www.aa7a.cn/',
                        'act': 'act_login',
                    })
cookie=ret.cookies.get_dict()
print(cookie)
# 如果不出意外，咱么就登陆上了,再向首页发请求，首页返回的数据中就有616564099@qq.com
ret1=requests.get('http://www.aa7a.cn/',cookies=cookie)
# ret1=requests.get('http://www.aa7a.cn/')

print('616564099@qq.com' in ret1.text)


# 秒杀小米手机，一堆小号
# 定时任务：一到时间，就可以发送post请求，秒杀手机


# 以后碰到特别难登陆的网站，代码登陆不进去怎么办？

# 之所以要登陆，就是为了拿到cookie，下次发请求（如果程序拿不到cookie，自动登陆不进去）
# 就手动登陆进去，然后用程序发请求
```



























---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/spider-1%E7%88%AC%E8%99%AB%E5%85%A5%E9%97%A8/  

