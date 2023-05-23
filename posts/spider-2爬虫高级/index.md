# 爬虫入门/bs4

爬虫 bs4 模块使用
<!--more-->


### bs4的使用

从html或者xml中提取数据的python库，修改xml

```python
# 安装 pip3 install beautifulsoup4
# 使用
from bs4 import BeautifulSoup
# 实例化得到对象，传入要解析的文本，解析器
# html.parser内置解析器，速度稍微慢一些，但是不需要装第三方模块
# lxml：速度快一些，但是需要安装 pip3 install lxml
soup=BeautifulSoup(ret.text,'html.parser')
# soup=BeautifulSaoup(open('a.html','r'))
# find（找到的第一个）
# find_all(找到的所有)
```

遍历文档树

```python
from bs4 import BeautifulSoup
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"id="id_p"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
# pip3 install lxml
soup=BeautifulSoup(html_doc,'lxml')
# 美化
# print(soup.prettify())

```

- Tag 对象

  ```python
  from  bs4.element import Tag
  #1、用法（通过.来查找，只能找到第一个）
  # head=soup.head
  # title=head.title
  # # print(head)
  # print(title)
  ```

- 获取标签名称

  ```python
  # p=soup.body
  # print(type(p))
  # print(p.name)
  ```

- 获取标签属性

  ```python
  p=soup.p
  # 方式一
  # 获取class属性,可以有多个，拿到列表
  print(p['class'])
  print(p['id'])
  print(p.get('id'))
  ```

  ```python
  # 方式二
  print(p.attrs['class'])
  print(p.attrs.get('id'))
  ```

- 获取标签内容

  ```python
  p=soup.p
  print(p.text) # 所有层级都拿出来拼到一起
  print(p.string) # 只有一层，才能去除
  print(list(p.strings)) # 把每次都取出来，做成一个生成器
  ```

- 嵌套选择

  ```python
  title=soup.head.title
  print(title)
  ```

- 子节点 子孙节点

  ```python
  p1=soup.p.children   # 迭代器
  p2=soup.p.contents  # 列表
  print(list(p1))
  print(p2)
  ```

- 父节点 祖先节点

  ```python
  p1=soup.p.parent  # 直接父节点
  p2=soup.p.parents
  print(p1)
  # print(len(list(p2)))
  print(list(p2))
  ```

- 兄弟节点

  ```python
  print(soup.a.next_sibling) #下一个兄弟
  print(soup.a.previous_sibling) #上一个兄弟
  print(list(soup.a.next_siblings)) #下面的兄弟们=>生成器对象
  print(soup.a.previous_siblings) #上面的兄弟们=>生成器对象
  ```

查找文档树

```python
# 查找文档树（find，find_all），速度比遍历文档树慢
# 两个配合着使用（soup.p.find()）
```

**五种过滤器**(# 以find为例)

- 字符串

  ```python
  #1 字符串查找 引号内是字符串 
  p=soup.find(name='p')
  p=soup.find(name='body')
  print(p)
  ```

  ```python
  # 查找类名是title的所有标签,class是关键字，class_
  ret=soup.find_all(class_='title')
  href属性为http://example.com/elsie的标签
  ret=soup.find_all(href='http://example.com/elsie')
  找id为xx的标签
  ret=soup.find_all(id='id_p')
  print(ret)
  ```

- 正则表达式

  ```python
  import re
  # reg=re.compile('^b')
  # ret=soup.find_all(name=reg)
  #找id以id开头的标签
  reg=re.compile('^id')
  ret=soup.find_all(id=reg)
  print(ret)
  ```

- 列表

  ```python
  ret=soup.find_all(name=['body','b'])
  ret=soup.find_all(id=['id_p','link1'])
  ret=soup.find_all(class_=['id_p','link1'])
  # and 关系
  ret=soup.find_all(class_='title',name='p')
  print(ret)
  ```

- True

  ```python
  # 所有有名字的标签
  ret=soup.find_all(name=True)
  #所有有id的标签
  ret=soup.find_all(id=True)
  # 所有有herf属性的
  ret=soup.find_all(href=True)
  print(ret)
  ```

- 方法

  ```python
  def has_class_but_no_id(tag):
      return tag.has_attr('class') and not tag.has_attr('id')
  print(soup.find_all(has_class_but_no_id))
  ```

- 其他使用

  ```python
  ret=soup.find_all(attrs={'class':"title"})
  ret=soup.find_all(attrs={'id':"id_p1",'class':'title'})
  print(ret)
  ```

- 拿到标签取属性, 去text

  ```python
  ret=soup.find_all(attrs={'id':"id_p",'class':'title'})
  print(ret[0].text)
  ```

- limit(限制条数)

  ```python
  soup.find()  # 就是find_all limit=1
  ret=soup.find_all(name=True,limit=2)
  print(len(ret))
  ```

- recursive

  ```python
  recursive=False (只找儿子)不递归查找，只找第一层
  ret=soup.body.find_all(name='p',recursive=False)
  print(ret)
  ```

  

### css 和 xpath 选择器

- css 选择器

  ```python
  # 重点
  
  # Tag对象.select("css选择器")
  #  #ID号
  #  .类名
  #   div>p：儿子 和div p：子子孙孙
  #   找div下最后一个a标签 div a:last-child
  ```

  ```python
  
  # bs4：自己的选择器，css选择器
  # lxml：css选择器，xpath选择器
  # selenium：自己的选择器，css选择器，xpath选择器
  # scrapy框架：自己的选择器，css选择器，xpath选择器
  # #select('.article')
  ```

  ```python
  #该模块提供了select方法来支持css,详见官网:https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html#id37
  html_doc = """
  <html><head><title>The Dormouse's story</title></head>
  <body>
  <p class="title">
      <b>The Dormouse's story</b>
      Once upon a time there were three little sisters; and their names were
      <a href="http://example.com/elsie" class="sister" id="link1">
          <span>Elsie</span>
      </a>
      <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
      <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
      <div class='panel-1'>
          <ul class='list' id='list-1'>
              <li class='element'>Foo</li>
              <li class='element'>Bar</li>
              <li class='element'>Jay</li>
          </ul>
          <ul class='list list-small' id='list-2'>
              <li class='element'><h1 class='yyyy'>Foo</h1></li>
              <li class='element xxx'>Bar</li>
              <li class='element'>Jay</li>
          </ul>
      </div>
      and they lived at the bottom of a well.
  </p>
  <p class="story">...</p>
  """
  from bs4 import BeautifulSoup
  soup=BeautifulSoup(html_doc,'lxml')
  
  
  ```

  ```python
  ### css 选择器
  print(soup.p.select('.sister'))
  print(soup.select('.sister span'))
  
  print(soup.select('#link1'))
  print(soup.select('#link1 span'))
  
  print(soup.select('#list-2 .element.xxx'))
  
  print(soup.select('#list-2')[0].select('.element')) #可以一直select,但其实没必要,一条select就可以了
  
  ```

  ```python
  # xpath选择
  # / 从根节点选取  /a   从根节点开始，往下找a标签（子）
  # //从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置  //a 从根节点开始找a标签（子子孙孙中所有a）
  # . 	选取当前节点。
  # .. 	选取当前节点的父节点。
  # @ 	选取属性。
  
  
  ########
  # 2 xpath选择器
  ########
  
  
  # XPath 是一门在 XML 文档中查找信息的语言
  
  # xpath选择
  # / 从根节点选取  /a   从根节点开始，往下找a标签（子）
  # //从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置  //a 从根节点开始找a标签（子子孙孙中所有a）
  # 取值 /text()
  # 取属性 /@属性名
  
  
  # //*[@id="auto-channel-lazyload-article"]/ul[1]
  # //ul[1]
  # //*[@id="focus-1"]/div[1]/ul/li[3]/h2
  
  # #focus-1 > div.focusimg-pic > ul > li:nth-child(3) > h2
  doc='''
  <html>
   <head>
    <base href='http://example.com/' />
    <title>Example website</title>
   </head>
   <body>
    <div id='images'>
     <a href='image1.html' id="xxx">Name: My image 1 <br /><img src='image1_thumb.jpg' /></a>
     <h5>test</h5>
     <a href='image2.html'>Name: My image 2 <br /><img src='image2_thumb.jpg' /></a>
     <a href='image3.html'>Name: My image 3 <br /><img src='image3_thumb.jpg' /></a>
     <a href='image4.html'>Name: My image 4 <br /><img src='image4_thumb.jpg' /></a>
     <a href='image5.html' class='li li-item' name='items'>Name: My image 5 <br /><img src='image5_thumb.jpg' /></a>
     <a href='image6.html' name='items'><span><h5>test</h5></span>Name: My image 6 <br /><img src='image6_thumb.jpg' /></a>
    </div>
   </body>
  </html>
  '''
  
  
  from lxml import etree
  html=etree.HTML(doc)  # 传字符串
  # html=etree.parse('search.html',etree.HTMLParser())  # 文件
  
  # 1 所有节点
  # a=html.xpath('//*')
  # 2 指定节点（结果为列表）
  # a=html.xpath('//head')
  
  # 3 子节点，子孙节点
  # a=html.xpath('//div/a')
  
  # a=html.xpath('//body/a') #无数据
  # a=html.xpath('//body//a')
  
  # 4 父节点
  # a=html.xpath('//body//a[@href="image1.html"]/..')
  # a=html.xpath('//body//a[@href="image1.html"]')
  # a=html.xpath('//body//a[1]/..')
  # 也可以这样
  # a=html.xpath('//body//a[1]/parent::*')
  
  
  # 5 属性匹配
  # a=html.xpath('//body//a[@href="image1.html"]')
  
  # 6 文本获取   标签后加：/text() ********重点
  # a=html.xpath('//body//a[@href="image1.html"]/text()')
  # a=html.xpath('//body//a/text()')
  
  # 7 属性获取  标签后：/@href   ********重点
  # a=html.xpath('//body//a/@href')
  # # 注意从1 开始取（不是从0）
  # a=html.xpath('//body//a[3]/@href')
  # 8 属性多值匹配
  #  a 标签有多个class类，直接匹配就不可以了，需要用contains
  # a=html.xpath('//body//a[@class="li"]')
  # a=html.xpath('//body//a[@href="image1.html"]')
  # a=html.xpath('//body//a[contains(@class,"li")]')
  # a=html.xpath('//body//a[contains(@class,"li")]/text()')
  # a=html.xpath('//body//a[contains(@class,"li")]/@name')
  
  
  
  # 9 多属性匹配 or 和 and （了解）
  # a=html.xpath('//body//a[contains(@class,"li") or @name="items"]')
  # a=html.xpath('//body//a[contains(@class,"li") and @name="items"]/text()')
  # a=html.xpath('//body//a[contains(@class,"li")]/text()')
  
  
  # 10 按序选择
  # a=html.xpath('//a[2]/text()')
  # a=html.xpath('//a[2]/@href')
  # 取最后一个（了解）
  # a=html.xpath('//a[last()]/@href')
  # a=html.xpath('//a[last()]/text()')
  # 位置小于3的
  # a=html.xpath('//a[position()<3]/@href')
  # a=html.xpath('//a[position()<3]/text()')
  # 倒数第二个
  # a=html.xpath('//a[last()-2]/@href')
  
  
  # 11 节点轴选择
  # ancestor：祖先节点
  # 使用了* 获取所有祖先节点
  # a=html.xpath('//a/ancestor::*')
  
  
  # # 获取祖先节点中的div
  # a=html.xpath('//a/ancestor::div')
  # a=html.xpath('//a/ancestor::div/a[2]/text()')
  # attribute：属性值
  # a=html.xpath('//a[1]/attribute::*')
  # a=html.xpath('//a[1]/@href')
  # child：直接子节点
  # a=html.xpath('//a[1]/child::*')
  # a=html.xpath('//a[1]/img/@src')
  # descendant：所有子孙节点
  # a=html.xpath('//a[6]/descendant::*')
  
  # following:当前节点之后所有节点(递归)
  # a=html.xpath('//a[1]/following::*')
  # a=html.xpath('//a[1]/following::*[1]/@href')
  # following-sibling:当前节点之后同级节点（同级）
  # a=html.xpath('//a[1]/following-sibling::*')
  # a=html.xpath('//a[1]/following-sibling::a')
  # a=html.xpath('//a[1]/following-sibling::*[2]')
  # a=html.xpath('//a[1]/following-sibling::*[2]/@href')
  
  print(a)
  ```

  





---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://192.168.0.120:18088/posts/spider-2%E7%88%AC%E8%99%AB%E9%AB%98%E7%BA%A7/  

