# selenium

爬虫 selenium 模块使用
<!--more-->

## 介绍

selenium最初是一个自动化测试工具,而爬虫中使用它主要是为了解决requests无法直接执行JavaScript代码的问题

- 可以操作浏览器(火狐，谷歌（建议你用谷歌），ie)，模拟人的行为（人可以干啥，代码控制就可以干啥）

## selenium的简单使用

```python
# pip3 install selenium

# 1 基本使用
from selenium import webdriver
# import time
# # 得到 一个谷歌浏览器对象
# # 代码不能直接操作浏览器，需要有一个浏览器驱动（配套的）
# # 下载谷歌浏览器驱动：http://npm.taobao.org/mirrors/chromedriver/
# # 谷歌浏览器驱动要跟谷歌版本对应
# # http://npm.taobao.org/mirrors/chromedriver/80.0.3987.106/   ：80.0.3987.149（正式版本）
# # 指定一下驱动的位置（相对路径/绝对路径）
# bro=webdriver.Chrome(executable_path='./chromedriver')
#
# bro.get("https://www.baidu.com")
#
# # 页面内容
# # ret.text 相当于它，可以使用bs4解析数据，或者用selenium自带的解析器解析
# print(bro.page_source)
# time.sleep(5)
# bro.close()
```

## selenium的高级用法

- 常用方法

  ```python
  bro=webdriver.Chrome(executable_path='./chromedriver')
  bro.get("https://www.baidu.com")
  ```

  

- 解析器

  ```python
  # 1、find_element_by_id  # id找
  # 2、find_element_by_link_text   # a标签上的文字找
  # 3、find_element_by_partial_link_text # a标签上的文字模糊
  # 4、find_element_by_tag_name        # 根据标签名字找
  # 5、find_element_by_class_name      # 根据类名字找
  # 6、find_element_by_name            # name='xx' 根据name属性找
  # 7、find_element_by_css_selector    # css选择器找
  # 8、find_element_by_xpath           #xpath选择器找
  ```

  在输入框中输入美女（自带的解析器，查找输入框空间）

  ```python
  # //*[@id="kw"]
  # input_search=bro.find_element_by_xpath('//*[@id="kw"]')
  input_search=bro.find_element_by_css_selector('#kw')
  ```

  写文字

  ```python
  input_search.send_keys("美女")
  ```

  查找搜索按钮

  ```python
  enter=bro.find_element_by_id('su')
  ```

  点击按钮

  ```python
  enter.click()
  ```

  关闭浏览器

  ```python
  bro.close()
  ```

- 小案例

  ```python
  import time
  bro=webdriver.Chrome(executable_path='./chromedriver')
  bro.get("https://www.baidu.com")
  #
  # 隐士等待(最多等待10s)
  # 只有控件没有加载出来，才会等，控件一旦加载出来，直接就取到
  bro.implicitly_wait(10)
  submit_button=bro.find_element_by_link_text('登录')
  submit_button.click()
  
  user_button=bro.find_element_by_id('TANGRAM__PSP_10__footerULoginBtn')
  user_button.click()
  
  user_input=bro.find_element_by_id('TANGRAM__PSP_10__userName')
  user_input.send_keys("ssssss@qq.com")
  
  pwd_input=bro.find_element_by_id('TANGRAM__PSP_10__password')
  pwd_input.send_keys("123456")
  
  
  submit_input=bro.find_element_by_id('TANGRAM__PSP_10__submit')
  submit_input.click()
  
  time.sleep(5)
  bro.close()
  ```

- 获取 cookie

  ```python
  # 登陆之后，拿到cookie：就可以自己搭建cookie池（requests模块发请求，携带者cookie）
  import time
  bro=webdriver.Chrome(executable_path='./chromedriver')
  bro.get("https://www.baidu.com")
  print(bro.get_cookies())
  bro.close()
  
  # #搭建cookie池和代理池的作用是什么？封ip ，封账号（弄一堆小号，一堆cookie）
  ```

- 无界面浏览器

  ```python
  from selenium.webdriver.chrome.options import Options
  chrome_options = Options()
  chrome_options.add_argument('window-size=1920x3000') #指定浏览器分辨率
  chrome_options.add_argument('--disable-gpu') #谷歌文档提到需要加上这个属性来规避bug
  chrome_options.add_argument('--hide-scrollbars') #隐藏滚动条, 应对一些特殊页面
  chrome_options.add_argument('blink-settings=imagesEnabled=false') #不加载图片, 提升速度
  chrome_options.add_argument('--headless') #浏览器不提供可视化页面. linux下如果系统不支持可视化不加这条会启动失败
  bro=webdriver.Chrome(executable_path='./chromedriver',options=chrome_options)
  bro.get("https://www.baidu.com")
  print(bro.get_cookies())
  bro.close
  ```

- 获取标签属性(重点)

  ```python
  print(tag.get_attribute('src'))
  print(tag.get_attribute('href'))
  ```

- 获取标签文本(重点)

  ```python
  print(tag.text)
  ```

- 获取标签ID，位置，名称，大小（了解）

  ```python
  print(tag.id)
  print(tag.location)
  print(tag.tag_name)
  print(tag.size)
  ```

- 显示等待与隐式等待

  ```python
  # 隐士等待(最多等待10s)
  bro.implicitly_wait(10)
  # 只有控件没有加载出来，才会等，控件一旦加载出来，直接就取到
  # 显示等待（每个控件，都要写等待），不要使用
  ```

- 元素交互操作

  ```python
  # 点击click，清空clear，输入文字send_keys
  ```

- 执行js

  ```python
  import time
  bro=webdriver.Chrome(executable_path='./chromedriver')
  #
  bro.get("https://www.cnblogs.com")
  # 执行js代码
  # bro.execute_script('alert(1)')
  # window.scrollTo(0,document.body.scrollHeight)
  # 使页面滚动到最低层
  bro.execute_script('window.scrollTo(0,document.body.scrollHeight)')
  time.sleep(5)
  bro.close()
  ```

- 模拟浏览器的前进后头

  ```python
  import time
  bro=webdriver.Chrome(executable_path='./chromedriver')
  
  bro.get("https://www.cnblogs.com")
  time.sleep(1)
  bro.get("https://www.baidu.com")
  time.sleep(1)
  bro.get("https://www.jd.com")
  
  #退到上一个
  bro.back()
  time.sleep(1)
  # 前进一下
  bro.forward()
  
  time.sleep(5)
  bro.close()
  ```

- 选项卡管理

  ```python
  import time
  from selenium import webdriver
  #
  browser=webdriver.Chrome(executable_path='./chromedriver')
  browser.get('https://www.baidu.com')
  browser.execute_script('window.open()')
  # 本质上是执行的js代码
  print(browser.window_handles) #获取所有的选项卡
  browser.switch_to_window(browser.window_handles[1])
  browser.get('https://www.taobao.com')
  time.sleep(2)
  browser.switch_to_window(browser.window_handles[0])
  browser.get('https://www.sina.com.cn')
  browser.close()
  ```

- 异常处理

  ```python
  from selenium import webdriver
  from selenium.common.exceptions import TimeoutException,NoSuchElementException,NoSuchFrameException
  try:
      browser=webdriver.Chrome(executable_path='./chromedriver')
      browser.get('http://www.baidu.com')
      browser.find_element_by_id("xxx")
  #
  except Exception as e:
  #     print(e)
  finally:
      browser.close()
  ```

### 小案例

```python
########
# 爬取京东商品信息
#######
from selenium import webdriver
import time
from selenium.webdriver.common.keys import Keys
bro=webdriver.Chrome(executable_path='./chromedriver')



def get_goods(bro):
    # find_elements_by_class_name  找所有
    # find_element_by_class_name   找一个
    li_list=bro.find_elements_by_class_name('gl-item')
    # ul_list=bro.find_elements_by_css_selector('.gl-item')
    for li in li_list:
        url=li.find_element_by_css_selector('.p-img>a').get_attribute('href')
        url_img=li.find_element_by_css_selector('.p-img img').get_attribute("src")
        if not url_img:
            url_img='https:'+li.find_element_by_css_selector('.p-img img').get_attribute("data-lazy-img")
        price=li.find_element_by_css_selector('.p-price i').text
        name=li.find_element_by_css_selector('.p-name em').text
        commit=li.find_element_by_css_selector('.p-commit a').text

        print('''
        商品名字：%s
        商品价格：%s
        商品图片地址：%s
        商品地址：%s
        商品评论数：%s
        '''%(name,price,url,url_img,commit))

    #查找下一页按钮
    next=bro.find_element_by_partial_link_text('下一页')
    time.sleep(1)
    next.click()
    #继续抓取下一页
    get_goods(bro)

try:
    bro.get('https://www.jd.com')
    #隐士等待
    bro.implicitly_wait(10)
    input_search=bro.find_element_by_id('key')
    input_search.send_keys("精品内衣")
    #模拟键盘操作(模拟键盘敲回车)
    input_search.send_keys(Keys.ENTER)
    get_goods(bro)

except Exception as e:
    print(e)
finally:
    bro.close()
```

[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://192.168.0.120:18088/posts/spider-4selenium/  

