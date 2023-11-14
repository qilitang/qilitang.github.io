# python 常用模块


## time 模块
```python
time 模块是内置时间模块
time.time() 是当前时间戳 返回的是float 类型的时间 是从1970 年一月一号到现在的秒数
```
格式化时间输出
```python
time.strftime()  格式化时间输出 传入格式

    %Y  Year with century as a decimal number.
    %m  Month as a decimal number [01,12].
    %d  Day of the month as a decimal number [01,31].
    %H  Hour (24-hour clock) as a decimal number [00,23].
    %M  Minute as a decimal number [00,59].
    %S  Second as a decimal number [00,61].
    %z  Time zone offset from UTC.
    %a  Locale's abbreviated weekday name.
    %A  Locale's full weekday name.
    %b  Locale's abbreviated month name.
    %B  Locale's full month name.
    %c  Locale's appropriate date and time representation.
    %I  Hour (12-hour clock) as a decimal number [01,12].
    %p  Locale's equivalent of either AM or PM.

```
时间对象获取
```python
obj = time.localtime()  获取时间对象
print(obj)
#  得到结果
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=17, tm_hour=21, tm_min=39, tm_sec=48, tm_wday=6, tm_yday=321, tm_isdst=0)

可以从时间对象中 获取指定的数据.
```
获取时间对象的格式化时间, 可以是未来/过去的时间对象

```
获取当前格式化时间

print(time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())
# 个人感觉这是脱了裤子放屁, 多此一举

在strftime内第二个参数, 可以传入之前的时间对象, 把此过去/未来的时间对象格式化

```
字符串格式时间转换为时间对象
```python

res = time.strptime('2018-12-13', '%Y:%m:%d')
这个返回值就是时间对象, 两个参数, 第一个是字符串时间格式, 第二个是格式化时间样式, 返回值是此格式化时间的对象

```
## datatime 模块

```
# 获取当前年月日
obj_data = datetime.date.today()
# 获取当前年月日,时分秒
obj_datatime = datatime.datatime()

# 两者得到的都是时间对象, 可以分别取出 年, 月, 日, 时, 分, 秒
obj_datatime.weekday()  计算今天是周几

```
时区
```python
# 获取当前时区的时间
datetime.datetime.now()
# 获取UTC时间(格林尼治)
datetime.datetime.utcnow()
```

日期/时间计算
```
# 首先获取时间对象. 
obj_time = datatime.timedelta(7)
# 默认 是天
# 可以指定 week, mount等

# 获取日期时间
current_date = datetime.datetime.now()
日期时间 = 日期时间 +/- 时间对象

时间对象 = 日期时间 +/- 日期时间

获取七天后时间

later_time = current_time + time_obj

```

## random 随机模块
```python
random.random()

随机0-1之间的浮点数
random.randomint(n,m)
n,m 之间的整数
random.choice(可迭代对象) 随机取出可迭代对象的一个元素

random.shuffle(可迭代对象), 对可迭代对象中的元素乱序


```
## os 模块 和操作系统交互
```python
# 获取当前执行文件的目录
os.path.dirname(__file__)
# 路径要用常量

# 进行路径拼接
os.path.join(路径,字符串)

# 判断“文件/文件夹”是否存在：若文件存在返回True，若不存在返回False
os.path.exists(路径)

# 判断是否是文件夹 True/False
os.path.isdir(路径)

# 创建文件夹
os.mkdir(路径)

# 删除空文件夹
os.rmdir(路径)

# 获取某个文件夹类所有文件的名字
os.listdir(路径)

# 获取可迭代对象的索引即元素的对应关系, 对象有一个个的元组(索引, 元素)
enumerate(可迭代对象)  

```
## sys 模块
```python
# 获取当前python解释器的环境变量路径
sys.path

# 将当前项目添加到环境变量中
BASE_PATH = os.path.dirname(os.path.dirname(__file__))
sys.path.append(BASE_PATH)

# 获取cmd终端的命令行  python3 py文件 用户名 密码
print(sys.argv)  # 返回的是列表['']

```

## hashlib
加密模块
内置很多算法, 常用的是MD5

```python
# 首先实例化 一个对象
m = hashlib.md5()
m.update(bytes 类型的字符串)
res = m.hexdigest()


# res 是经过加密算法得到的32位的位字符串.


可以对密码进行加盐处理

略过

```
## 序列化
- 序列化  
我们把对象(变量)从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling，在其他语言中也被称之为serialization，marshalling，flattening等等，都是一个意思。

### 为什么要序列化?

- 1：持久保存状态

需知一个软件/程序的执行就在处理一系列状态的变化，在编程语言中，'状态'会以各种各样有结构的数据类型(也可简单的理解为变量)的形式被保存在内存中。

内存是无法永久保存数据的，当程序运行了一段时间，我们断电或者重启程序，内存中关于这个程序的之前一段时间的数据（有结构）都被清空了。

在断电或重启程序之前将程序当前内存中所有的数据都保存下来（保存到文件中），以便于下次程序执行能够从文件中载入之前的数据，然后继续执行，这就是序列化。

具体的来说，你玩使命召唤闯到了第13关，你保存游戏状态，关机走人，下次再玩，还能从上次的位置开始继续闯关。或如，虚拟机状态的挂起等。

- 2：跨平台数据交互

序列化之后，不仅可以把序列化后的内容写入磁盘，还可以通过网络传输到别的机器上，如果收发的双方约定好实用一种序列化的格式，那么便打破了平台/语言差异化带来的限制，实现了跨平台数据交互。

反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling。
### json & pickle 序列化
#### json

如果我们要在不同的编程语言之间传递对象，就必须把对象序列化为标准格式，比如XML，但更好的方法是序列化为JSON，因为JSON表示出来就是一个字符串，可以被所有语言读取，也可以方便地存储到磁盘或者通过网络传输。JSON不仅是标准格式，并且比XML更快，而且可以直接在Web页面中读取，非常方便。

JSON表示的对象就是标准的JavaScript语言的对象，JSON和Python内置的数据类型对应如下：
![](https://gitee.com/qilitang/Img/raw/master/img/877318-20160911105642628-530508765.png)
![](https://gitee.com/qilitang/Img/raw/master/img/1036857-20170215164644722-1590025858.png)

```python

import json
 
dic={'name':'alvin','age':23,'sex':'male'}
print(type(dic))#<class 'dict'>
 
j=json.dumps(dic)
print(type(j))#<class 'str'>
 
 
f=open('序列化对象','w')
f.write(j)  #-------------------等价于json.dump(dic,f)
f.close()
#-----------------------------反序列化<br>
import json
f=open('序列化对象')
data=json.loads(f.read())#  等价于data=json.load(f)
```
注意

```python

import json
#dct="{'1':111}"#json 不认单引号
#dct=str({"1":111})#报错,因为生成的数据还是单引号:{'one': 1}

dct='{"1":"111"}' 
print(json.loads(dct))

#conclusion:
#        无论数据是怎样创建的，只要满足json格式，就可以json.loads出来,不一定非要dumps的数据才能loads

 注意点
 
```
![](https://gitee.com/qilitang/Img/raw/master/img/1036857-20170215164644722-1590025858.png)

```
import pickle
 
dic={'name':'alvin','age':23,'sex':'male'}
 
print(type(dic))#<class 'dict'>
 
j=pickle.dumps(dic)
print(type(j))#<class 'bytes'>
 
 
f=open('序列化对象_pickle','wb')#注意是w是写入str,wb是写入bytes,j是'bytes'
f.write(j)  #-------------------等价于pickle.dump(dic,f)
 
f.close()
#-------------------------反序列化
import pickle
f=open('序列化对象_pickle','rb')
 
data=pickle.loads(f.read())#  等价于data=pickle.load(f)
 
 
print(data['age'])
```

Pickle的问题和所有其他编程语言特有的序列化问题一样，就是它只能用于Python，并且可能不同版本的Python彼此都不兼容，因此，只能用Pickle保存那些不重要的数据，不能成功地反序列化也没关系。


## collections 模块

- python默认八大数据:
            - 整型
                - 浮点型
                - 字符串
                - 字典
                - 元组
                - 列表
                - 集合
                - 布尔
collections模块:
- 提供一些python八大数据类型 “以外的数据类型” 。


具名元组:
        具名元组 只是一个名字。
        应用场景:
            - 坐标
            -

        ``` from collections import namedtuple```
- 有序字典:
    - python中字典默认是无序

    - collections中提供了有序的字典

    ```from collections import OrderedDict```

示例
```
# 具名元组
from collections import namedtuple

# 传入可迭代对象是有序的
# 应用:坐标
# 将'坐标'变成 “对象” 的名字
point = namedtuple('坐标', ['x', 'y'])  # 第二个参数既可以传可迭代对象
point = namedtuple('坐标', ('x', 'y'))  # 第二个参数既可以传可迭代对象
point = namedtuple('坐标', 'x y')  # 第二个参数既可以传可迭代对象

# 会将 1 ---> x,   2 ---> y
# 传参的个数，要与namedtuple第二个参数的个数一一对应
p = point(1, 3)  # 本质上传了4个，面向对象讲解
print(p)
print(type(p))

```
### openpyxl 模块

```
openpyxl模块：第三方模块
    - 可以对Excle表格进行操作的模块

    - 下载:
        pip3 install openpyxl

    - Excel版本:
        2003之前:
            excle名字.xls

        2003以后:
            excle名字.xlsx

    - 清华源: https://pypi.tuna.tsinghua.edu.cn/simple

    - 配置永久第三方源:
        D:\Python36\Lib\site-packages\pip\_internal\models\index.py

```
示例
```
# 写入数据
from openpyxl import Workbook
# 获取Excel文件对象
wb_obj = Workbook()

wb1 = wb_obj.create_sheet('python13期工作表1', 1)
wb2 = wb_obj.create_sheet('python13期工作表2', 2)

# 修改工作表名字: 为python13期工作表1标题修改名字 ---》 tank大宝贝
print(wb1.title)
wb1.title = 'tank大宝贝'
print(wb1.title)

# 为第一张工作表添加值
# wb1['工作簿中的表格位置']
wb1['A10'] = 200
wb1['B10'] = 1000
wb1['C10'] = '=SUM(A10:B10)'


wb2['A1'] = 100

# 生成Excel表格
wb_obj.save('python13期.xlsx')
print('excel表格生成成功')
```
读取数据

```
 # 读取数据
from openpyxl import load_workbook
wb_obj = load_workbook('python13期.xlsx')
print(wb_obj)

# # wb_obj['表名']
wb1 = wb_obj['tank大宝贝']
print(wb1['A10'].value)
wb1['A10'] = 20
print(wb1['A10'].value)



```
[toc]
## subprocess 模块
用于和系统的cmd 终端进行交互. 方法比较简单
```python



import subprocess

while True:
    cmd = input(">>:").strip()
    # 输入终端命令字符串
    res = subprocess.Popen(cmd, 
                        shell=True,
                           stderr=subprocess.PIPE,
                           stdout=subprocess.PIPE,
                           )
    #  参数介绍.
    传入命令字符串,
    shell = True , 打开 cmd 终端功能
    stderr 接收命令错误的信息
    stdout  接收命令正确的返回信息
    实例化对象用stdout.read() 方法接收信息
    print(res.stdout.read().decode("gbk"))
    res.stderr.read()


```

## re模块.
- 一：什么是正则？

　正则就是用一些具有特殊含义的符号组合到一起（称为正则表达式）来描述字符或者字符串的方法。或者说：正则就是用来描述一类事物的规则。




![](https://gitee.com/qilitang/Img/raw/master/img/1036857-20170529203214461-666088398.png)


- 字符组:
```
    - [0-9] 可以匹配到一个0-9的字符  
    - [9-0]: 报错, 必须从小到大  
    - [a-z]: 从小写的a-z  
    - [A-Z]: 从大写A-Z  
    - [z-A]: 错误,   只能从小到大，根据ascii表来匹配大小。
    - [A-z]: 总大写的A到小写的z。
注意: 顺序必须要按照ASCII码数值的顺序编写。
    - [^...]: 表示取反的意思。
    - [^ab]: 代表只去ab以外的字符。
    - [^a-z]: 取a-z以外的字符。
```

re 模块较重要的方法:
```

re模块三种比较重要的方法:
    - findall()： ----> []
        可以匹配 "所有字符" ，拿到返回的结果，返回的结果是一个列表。
        'awfwaghowiahioawhio'  # a
        ['a', 'a', 'a', 'a']
        
    - search()：----> obj ----> obj.group()
        'awfwaghowiahioawhio'  # a
        在匹配一个字符成功后，拿到结果后结束，不往后匹配。
        'a'
    
    - match()：----> obj ----> obj.group()
        'awfwaghowiahioawhio'  # a
        'a'
        'wfwaghowiahioawhio'  # a
         None
        从匹配字符的开头匹配,若开头不是想要的内容，则返回None。
```


## 爬虫四步原理
- 1.发送请求: requests
- 2.获取响应数据: 对方机器直接返回的
- 3.解析并提取想要的数据: re
- 4.保存提取后的数据: with open()
## 爬蟲三部曲:
- 1.发送请求
- 2.解析数据
- 3.保存数据


``` 自己写的代码


import requests
import re
import os
my_path = os.path.join(os.path.dirname(__file__),"妹子图")

if not os.path.exists(my_path):
    os.mkdir(my_path)
# 爬虫的四大步骤
"""
1 . 发送请求,
2 . 接收返回数据
3 . 数据处理
4 . 数据保存
"""


#  返回每个类所有的URL
def send_request(url):
    response = requests.get(url)
    data = response.text
    url_data = re.findall('<div class="boxa"> <a href="//(.*?)"', data, re.S)
    data_url = []
    for i in url_data:
        data_url.append(f"https://{i}")
    return data_url


def send_url(img_url):
    response = requests.get(img_url)
    if response.status_code == 200:
        data = response.text
        url_data = re.findall('<a href="//(.*?)@!w1200".*?data-med=', data, re.S)
        url_data.pop(0)
    return url_data


def save(res, num, url):
    data_img = requests.get(f"http://{url}")
    with open(f'{my_path}\{res}{num}.jpg', "wb")as f:
        f.write(data_img.content)
list1 = ["xinggan","qingchun","xiaohua","chemo","qipao"]

if __name__ == '__main__':

    while True:
        for num, s in enumerate(list1):
            print(num,s)
        res = input("请输入要爬取类别序号>>:").strip()
        res1 = int(res)
        res2 = list1[res1]
        print("开始爬取")
        url = f"https://www.plmm.com.cn/{res2}/flow/"
        url_data = send_request(url)
        q = 0
        for i in url_data:
            s = send_url(i)
            for m in s:
                save(res,q, m)
                q +=1
```

## loging 模块的使用

loging 配置模板

``` python
logging配置
"""
import os

import logging.config

# 定义三种日志输出格式 开始
standard_format = '[%(asctime)s][%(threadName)s:%(thread)d][task_id:%(name)s][%(filename)s:%(lineno)d]' \
                  '[%(levelname)s][%(message)s]' #其中name为getlogger指定的名字

simple_format = '[%(levelname)s][%(asctime)s][%(filename)s:%(lineno)d]%(message)s'

id_simple_format = '[%(levelname)s][%(asctime)s] %(message)s'

# 定义日志输出格式 结束
# ****************注意1: log文件的目录
BASE_PATH = os.path.dirname(os.path.dirname(__file__))
logfile_dir = os.path.join(BASE_PATH, 'log_dir')
# print(logfile_dir)

# ****************注意2: log文件名
logfile_name = 'user.log'

# 如果不存在定义的日志目录就创建一个
if not os.path.isdir(logfile_dir):
    os.mkdir(logfile_dir)

# log文件的全路径
logfile_path = os.path.join(logfile_dir, logfile_name)

# ****************注意3: log配置字典
LOGGING_DIC = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'standard': {
            'format': standard_format
        },
        'simple': {
            'format': simple_format
        },
    },
    'filters': {},
    'handlers': {
        #打印到终端的日志
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',  # 打印到屏幕
            'formatter': 'simple'
        },
        # 打印到文件的日志,收集info及以上的日志
        'default': {
            'level': 'DEBUG',
            'class': 'logging.handlers.RotatingFileHandler',  # 保存到文件
            'formatter': 'standard',
            'filename': logfile_path,  # 日志文件
            'maxBytes': 1024*1024*5,  # 日志大小 5M
            'backupCount': 5,
            'encoding': 'utf-8',  # 日志文件的编码，再也不用担心中文log乱码了
        },
    },
    'loggers': {
        #logging.getLogger(__name__)拿到的logger配置
        '': {
            'handlers': ['default', 'console'],  # 这里把上面定义的两个handler都加上，即log数据既写入文件又打印到屏幕
            'level': 'DEBUG',
            'propagate': True,  # 向上（更高level的logger）传递
        },
    },
}



```

注意配置

```
def get_logger(user_type):
    # 1.加载log配置字典到logging模块的配置中
    logging.config.dictConfig(LOGGING_DIC)
    # 2.获取日志对象
    logger = logging.getLogger(user_type)
    return logger

logger = get_logger('user')
logger.info('学习不要浮躁，一步一个脚印!')


```

以上记牢即可

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-16-%E5%B8%B8%E7%94%A8%E6%A8%A1%E5%9D%97/  

