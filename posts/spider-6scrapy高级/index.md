# Scrapy高级

爬虫 Scrapy高级 使用
<!--more-->
---

## scrapy 请求传参

```python
# 1 放 ：yield Request(url,callback=self.parser_detail,meta={'item':item})
# 2 取：response.meta.get('item')
# 3 指定解析函数 callback=self.解析函数. 
```

```python
class CnblogSpider(scrapy.Spider):
    name = 'cnblog'
    # allowed_domains = ['https://www.cnblogs.com/']
    start_urls = ['https://www.cnblogs.com/sitehome/p/1']
    num = 0

    # post_list > div:nth-child(19) > div
    def parse(self, response):
        article_list = response.css(".post_item_body")

        for article in article_list:
            item = CrawlCnblogsItem()
            title = article.css(".titlelnk::text").extract_first()
            url = article.css(".titlelnk::attr(href)").extract_first()
            desc = article.css("p::text").extract()[-1]
            author = article.css(".post_item_foot>a::text").extract_first()
            comment = article.xpath(".//*[contains(@class,'post_item_foot')]/span[1]/a/text()").extract_first()
            view = article.xpath(".//*[contains(@class,'post_item_foot')]/span[2]/a/text()").extract_first()
            item["title"] = title.strip()
            item["url"] = url
            item["desc"] = desc
            item["author"] = author.strip()
            item["comment"] = comment.strip()
            item["view"] = view.strip()

            yield item
        nextpage = f'https://www.cnblogs.com{response.css(".pager> a:last-child::attr(href)").extract_first()}'
        if nextpage and (self.num < 10):
            yield Request(url=nextpage, callback=self.parse)

```

## 提高爬取效率

```python
- 在配置文件中进行相关的配置即可:(默认还有一套setting)
#1 增加并发：
默认scrapy开启的并发线程为32个，可以适当进行增加。在settings配置文件中修改CONCURRENT_REQUESTS = 100值为100,并发设置成了为100。
#2 提高日志级别：
在运行scrapy时，会有大量日志信息的输出，为了减少CPU的使用率。可以设置log输出信息为INFO或者ERROR即可。在配置文件中编写：LOG_LEVEL = ‘INFO’
# 3 禁止cookie：
如果不是真的需要cookie，则在scrapy爬取数据时可以禁止cookie从而减少CPU的使用率，提升爬取效率。在配置文件中编写：COOKIES_ENABLED = False
# 4禁止重试：
对失败的HTTP进行重新请求（重试）会减慢爬取速度，因此可以禁止重试。在配置文件中编写：RETRY_ENABLED = False
# 5 减少下载超时：
如果对一个非常慢的链接进行爬取，减少下载超时可以能让卡住的链接快速被放弃，从而提升效率。在配置文件中进行编写：DOWNLOAD_TIMEOUT = 10 超时时间为10s
```



## 下载中间件



```python
# 2大中间件：下载中间件，爬虫中间件

# 1 写在middlewares.py中（名字随便命名）
# 2 配置生效（）
		SPIDER_MIDDLEWARES = {
   'cnblogs_crawl.middlewares.CnblogsCrawlSpiderMiddleware': 543,
}
	DOWNLOADER_MIDDLEWARES = {
   'cnblogs_crawl.middlewares.CnblogsCrawlDownloaderMiddleware': 543,
}
  
# 2 下载中间件
	-process_request：（请求去，走）
  			# - return None: 继续处理当次请求，进入下一个中间件
        # - return Response： 当次请求结束，把Response丢给引擎处理（可以自己爬，包装成Response）
        # - return Request ： 相当于把Request重新给了引擎，引擎再去做调度
        # - 抛异常：执行process_exception
  -process_response：（请求回来，走）
  		  # - return a Response object ：继续处理当次Response，继续走后续的中间件
        # - return a Request object：重新给引擎做调度
        # - or raise IgnoreRequest ：process_exception
  -process_exception：（出异常，走）
  		   # - return None: continue processing this exception
        # - return a Response object: stops process_exception() chain  ：停止异常处理链，给引擎（给爬虫）
        # - return a Request object: stops process_exception() chain ：停止异常处理链，给引擎（重新调度）
```



## 集成 selenium

```python
# 在爬虫已启动，就打开一个chrom浏览器，以后都用这一个浏览器来爬数据

# 1 在爬虫中创建bro对象
	bro = webdriver.Chrome(executable_path='./chromedriver')

# 2 中间件中使用：
  spider.bro.get(request.url)
  text=spider.bro.page_source
  response=HtmlResponse(url=request.url,status=200,body=text.encode('utf-8'))
  return response
# 3 关闭，在爬虫中
    def close(self, reason):
        self.bro.close()

```

## fack-useragent

```python
# 请求头中的user-agent
list=['','']

# pip3 install fake-useragent

# https://github.com/hellysmile/fake-useragent
from fake_useragent import UserAgent
ua=UserAgent(verify_ssl=False)
print(ua.random)
```

## 去重源码分析

```python
# 去重源码分析
# from scrapy.core.scheduler import Scheduler
# Scheduler下：def enqueue_request(self, request)方法判断是否去重
	if not request.dont_filter and self.df.request_seen(request):
   	Requests对象，RFPDupeFilter对象
# 如果要自己写一个去重类
	-写一个类，继承BaseDupeFilter类
  -重写def request_seen(self, request):
  -在setting中配置：DUPEFILTER_CLASS = '项目名.dup.UrlFilter'

# scrapy起始爬取的地址
    def start_requests(self):
        for url in self.start_urls:
            yield Request(url)
          
```

增量爬取

```python
-增量爬取（100链接，150个链接）
  -已经爬过的，放到某个位置（mysql，redis中：集合）
  -如果用默认的，爬过的地址，放在内存中，只要项目一重启，就没了，它也不知道我爬过那个了，所以要自己重写去重方案
-你写的去重方案，占得内存空间更小
	-bitmap方案
	-BloomFilter布隆过滤器
  
  
  
from scrapy.http import Request
from scrapy.utils.request import request_fingerprint

# 这种网址是一个
requests1=Request(url='https://www.baidu.com?name=lqz&age=19')
requests2=Request(url='https://www.baidu.com?age=18&name=lqz')

ret1=request_fingerprint(requests1)
ret2=request_fingerprint(requests2)
print(ret1)
print(ret2)

# bitmap去重  一个小格表示一个连接地址 32个连接，一个比特位来存一个地址
# https://www.baidu.com?age=18&name=lqz ---》44
# https://www.baidu.com?age=19&name=lqz ---》89
# c2c73dfccf73bf175b903c82b06a31bc7831b545假设它占4个bytes，4*8=32个比特位
# 存一个地址，占32个比特位
# 10个地址，占320个比特位
#计算机计量单位
# 比特位：只能存0和1
# 8个比特位是一个bytes
# 1024bytes=1kb
# 1024kb=1m
# 1024m=1g

# 布隆过滤器：原理和python中如何使用
    def request_seen(self, request):
        # 把request对象传入request_fingerprint得到一个值：aefasdfeasd
        # 把request对象，唯一生成一个字符串
        fp = self.request_fingerprint(request)
        #判断fp，是否在集合中，在集合中，表示已经爬过，return True，他就不会再爬了
        if fp in self.fingerprints:
            return True
        # 如果不在集合中，放到集合中
        self.fingerprints.add(fp)
        if self.file:
            self.file.write(fp + os.linesep)
```



## 分布式爬虫

```python
# 1 安装pip3 install scrapy-redis
# 1 原来的爬虫继承
from scrapy_redis.spiders import RedisSpider
class CnblogsSpider(RedisSpider):
  	#start_urls = ['http://www.cnblogs.com/']
    redis_key = 'myspider:start_urls'
    
# 2 在setting中配置
  SCHEDULER = "scrapy_redis.scheduler.Scheduler"
  DUPEFILTER_CLASS = "scrapy_redis.dupefilter.RFPDupeFilter"
  ITEM_PIPELINES = {
     'scrapy_redis.pipelines.RedisPipeline': 300
  }
# 3 多台机器上启动scrapy
# 4 向reids中发送起始url
lpush myspider:start_urls https://www.cnblogs.com

```



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/spider-6scrapy%E9%AB%98%E7%BA%A7/  

