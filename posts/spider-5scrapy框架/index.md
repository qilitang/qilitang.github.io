# Scrapy框架

爬虫 Scrapy框架 使用
<!--more-->



## 1 Scrapy 介绍/架构

 Scrapy一个开源和协作的框架，其最初是为了页面抓取 (更确切来说, 网络抓取 )所设计的，使用它可以以快速、简单、可扩展的方式从网站中提取所需的数据。但目前Scrapy的用途十分广泛，可用于如数据挖掘、监测和自动化测试等领域，也可以应用在获取API所返回的数据(例如 Amazon Associates Web Services ) 或者通用的网络爬虫。

  Scrapy 是基于twisted框架开发而来，twisted是一个流行的事件驱动的python网络框架。因此Scrapy使用了一种非阻塞（又名异步）的代码来实现并发。整体架构大致如下

![](/images/20200412083213.png)

**Components：**

1. 引擎(EGINE)

   引擎负责控制系统所有组件之间的数据流，并在某些动作发生时触发事件。有关详细信息，请参见上面的数据流部分。

2. **调度器(SCHEDULER)**
   用来接受引擎发过来的请求, 压入队列中, 并在引擎再次请求的时候返回. 可以想像成一个URL的优先级队列, 由它来决定下一个要抓取的网址是什么, 同时去除重复的网址

3. **下载器(DOWLOADER)**
   用于下载网页内容, 并将网页内容返回给EGINE，下载器是建立在twisted这个高效的异步模型上的

4. **爬虫(SPIDERS)**
   SPIDERS是开发人员自定义的类，用来解析responses，并且提取items，或者发送新的请求

5. **项目管道(ITEM PIPLINES)**
   在items被提取后负责处理它们，主要包括清理、验证、持久化（比如存到数据库）等操作

6. 下载器中间件(Downloader Middlewares)

   位于Scrapy引擎和下载器之间，主要用来处理从EGINE传到DOWLOADER的请求request，已经从DOWNLOADER传到EGINE的响应response，你可用该中间件做以下几件事

   1. process a request just before it is sent to the Downloader (i.e. right before Scrapy sends the request to the website);
   2. change received response before passing it to a spider;
   3. send a new Request instead of passing received response to a spider;
   4. pass response to a spider without fetching a web page;
   5. silently drop some requests.

7. **爬虫中间件(Spider Middlewares)**
   位于EGINE和SPIDERS之间，主要工作是处理SPIDERS的输入（即responses）和输出（即requests）

[官网链接：https://docs.scrapy.org/en/latest/topics/architecture.html](https://docs.scrapy.org/en/latest/topics/architecture.html)



## 2 Scripy框架的安装和启动

安装

```python
#Windows平台
    1、pip3 install wheel #安装后，便支持通过wheel文件安装软件，wheel文件官网：https://www.lfd.uci.edu/~gohlke/pythonlibs
    3、pip3 install lxml
    4、pip3 install pyopenssl
    5、下载并安装pywin32：https://sourceforge.net/projects/pywin32/files/pywin32/
    6、下载twisted的wheel文件：http://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted
    7、执行pip3 install 下载目录\Twisted-17.9.0-cp36-cp36m-win_amd64.whl
    8、pip3 install scrapy

#Linux和 mac平台
    1、pip3 install scrapy
```

创建

```python
scrapy startproject #创建项目
scrapy genspider    #创建爬虫程序

```

启动

```python
scrapy crawl  爬虫名字 # 启动爬虫
scrapy crawl 爬虫名字 --nolog   # 不打印日志启动
```

从文件启动

```python
from scrapy.cmdline import execute
  # execute(['scrapy','crawl','chouti','--nolog'])
  execute(['scrapy','crawl','chouti'])
```



## 3 配置文件和目录介绍

目录介绍

```python
-crawl_chouti   # 项目名
  -crawl_chouti # 跟项目一个名，文件夹
    -spiders    # spiders：放着爬虫  genspider生成的爬虫，都放在这下面
    	-__init__.py
      -chouti.py # 抽屉爬虫
      -cnblogs.py # cnblogs 爬虫
    -items.py     # 对比django中的models.py文件 ,写一个个的模型类
    -middlewares.py  # 中间件（爬虫中间件，下载中间件），中间件写在这
    -pipelines.py   # 写持久化的地方（持久化到文件，mysql，redis，mongodb）
    -settings.py    # 配置文件
  -scrapy.cfg       # 不用关注，上线相关的
```

配置文件

```python
# 配置文件

ROBOTSTXT_OBEY = False   # 是否遵循爬虫协议，强行运行
USER_AGENT = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36'    # 请求头中的ua
LOG_LEVEL='ERROR' # 这样配置，程序错误信息才会打印，
	#启动爬虫直接 scrapy crawl 爬虫名   就没有日志输出
  # scrapy crawl 爬虫名 --nolog

```

爬虫文件

```python
class ChoutiSpider(scrapy.Spider):
    name = 'chouti'   # 爬虫名字
    allowed_domains = ['https://dig.chouti.com/']  # 允许爬取的域
    start_urls = ['https://dig.chouti.com/']   # 起始爬取的位置，爬虫一启动，会先向它发请求

    def parse(self, response):  # 解析，请求回来，自动执行parser，在这个方法中做解析
        print('---------------------------',response)
```

数据解析

- 使用bs4

  ```python
  # 1 解析，可以使用bs4解析
  from bs4 import BeautifulSoup
  soup=BeautifulSoup(response.text,'lxml')
  soup.find_all()
  ```

- 使用内置解析器

  ```python
  response.css  
  response.xpath
  
  # 解析 
    # 所有用css或者xpath选择出来的都放在列表中
    # 取第一个:extract_first()
    # 取出所有extract()
    # css选择器取文本和属性：
      response.css(".link-title::text")   
      response.css(".link-title::attr(href)")
    # xpath选择器取文本和属性
    	response.xpath('.//a[contains(@class,"link-title")/text()]')
      
      response.xpath('//a[contains(@class,"link-title")/@href]')
  ```

  

##  4 数据持久化

```python
# 方式一
	-1 parser解析函数，return 列表，列表套字典
  -2 scrapy crawl chouti -o aa.json   (支持：('json', 'jsonlines', 'jl', 'csv', 'xml', 'marshal', 'pickle')
```



```python

# 方式二 pipline的方式（管道）
   -1 在items.py中创建模型类
   -2 在爬虫中chouti.py，引入，把解析的数据放到item对象中（要用中括号）
   -3 yield item对象
   -4 配置文件配置管道
       ITEM_PIPELINES = {
        # 数字表示优先级（数字越小，优先级越大）
       'crawl_chouti.pipelines.CrawlChoutiPipeline': 300,
       'crawl_chouti.pipelines.CrawlChoutiRedisPipeline': 301，
    	}
  -5 pipline.py中写持久化的类

   				#  在保存数据之前执行的函数
          -spider_open
      	  # 在执行数据持久化之后执行的函数
          -spider_close
          -process_item（在这写保存到哪）
```



[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://192.168.0.120:18088/posts/spider-5scrapy%E6%A1%86%E6%9E%B6/  

