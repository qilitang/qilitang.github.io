# 动作链

爬虫 Scrapy高级 使用
<!--more-->
---
## 动作链

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
import time
bro=webdriver.Chrome(executable_path='./chromedriver')
bro.get('https://www.runoob.com/try/try.php?filename=jqueryui-api-droppable')

bro.implicitly_wait(10)

#切换frame（很少）
bro.switch_to.frame('iframeResult')

div=bro.find_element_by_xpath('//*[@id="draggable"]')

# 使用动作链
#得到一个动作练对象
action=ActionChains(bro)

# 使用动作链
#点击并且夯住
action.click_and_hold(div)

# 直接把上面的div移动到某个元素上
# action.move_to_element(元素控件)
# 移动x坐标，y坐标

# 三种移动方式
# action.move_by_offset() # 通过坐标
# action.move_to_element() # 到另一个标签
# action.move_to_element_with_offset() # 到另一个标签，再偏移一部分

for i in range(5):
    action.move_by_offset(10,10)

# 直接把上面的div移动到某个元素上的某个位置
# action.move_to_element_with_offset()

# 调用它，会动起来
action.perform()

time.sleep(1)

#释放动作链
action.release()
time.sleep(5)
bro.close()
```



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/spider-7%E5%8A%A8%E4%BD%9C%E9%93%BE%E8%87%AA%E5%8A%A8%E7%99%BB%E5%BD%9512306/  

