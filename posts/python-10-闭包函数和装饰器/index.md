# python 闭包函数及装饰器


## 闭包函数
什么是闭包函数呢?
- 闭: 封闭
- 包: 包裹  
 比如手机是闭包函数（内层函数），被手机包装盒 (外层函数) 包裹起来，
手机可以使用包装盒中的东西，内层函数可以引用外层函数的名字。
- 闭包函数必须在函数内部定义  
- 闭包函数可以引用外层函数的名字. 




**闭包函数是 函数嵌套,  函数对象, 名称空间与作用域的  结合体**



```
 def func(y):
    x = 100
    # inner是闭包函数
    def inner():
        print(x)
        print(y)
    return inner

inner = func()
inner()
```

##  装饰器
### 什么是装饰器？  

- 装饰: 装饰，修饰  。
- 器: 工具。  
- 装饰的工具。  

**“开放封闭”： 装饰器必须要遵循 “开放封闭” 原则**。  

- 开放:  
    对函数功能的添加是开放的。

- 封闭:  
    对函数功能修改是封闭的。

### 装饰器的作用？
- 在不修改被装饰对象源代码与调用方式的前提下, 添加新的功能。  
**装饰器的定义必须遵循  
不修改被装饰对象源代码  
不修改被装饰对象调用方式**

#### 为什么使用装饰器
可以解决代码冗余问题，提高代码的可扩展性。



#### 代码示例:

##### - 无装饰器实现运行时间统计
```
def download_movie():
    print('开始下载电影...')
    # 模拟电影下载时间 3秒
    time.sleep(3)  # 等待3秒
    print('电影下载成功...')
    return '小泽.mp4'
    
    
    
start_time = time.time()  # 获取当前时间戳
download_movie()
end_time = time.time()  # 获取当前时间戳
print(f'消耗时间: {end_time - start_time}')
# 问题: 多个被装饰对象，需要写多次统计时间的代码，导致代码冗余。

```


##### -  使用函数实现

```
def time_record(func):
    start_time = time.time()  # 获取当前时间戳
    # 写死了,该功能只能给一个函数使用
    # download_movie()
    func()
    end_time = time.time()  # 获取当前时间戳
    print(f'消耗时间: {end_time - start_time}')
    return None

# 被装饰对象的调用方式
res = time_record(download_movie)
print(res)  # None

#  问题, 更改了调用方式.
```

### 装饰器: 初级版
```python


def time_record(func):
    def inner(*args, **kwargs):
        # 统计开始
        start_time = time.time()

        # 被装饰对象， 问题1: 有返回值， 问题2: 不确定参数的 个数
        res = func(*args, **kwargs)  # func() ---> download_movie()
        # 当被统计的函数执行完毕后，获取当前时间
        end_time = time.time()
        # 统计结束，打印统计时间
        print(f'消耗时间: {end_time - start_time}')

        return res

    return inner


download_movie = time_record(download_movie)  # inner
# name = 'egon'
# download_movie(name)
download_movie(name='egon')

```
### 装饰器, 终极版.  
考虑被装饰对象,有参数,有返回值的情况.

```python

def time_record(func):  # 参数用于接收被装饰对象
    def inner(*args, **kwargs)  # 接收所有的被装饰对象的参数
        # 调用前添加功能
        res = func(*args, **kwargs)  # 调用被装饰对象, 并接收返回值.
        # 调用后添加功能
        return res  # 返回被装饰对象的返回值
    
    return inner  #  返回inner 
    

```
## 装饰器语法糖, 
### 装饰器使用的两种方式. 
-  装饰器使用得时候 , 需要两步, 
    -  调用装饰器函数, 并将被装饰对象传入,将返回值用 "调用函数"的名字赋值 
    -  调用被重新赋值后的 "名字"
-  代码演示
```python
download_movie= time_record(*args, **kwargs)
download_movie()

```
### 语法糖
即在被装饰对象的上方 用@+装饰器 来进行使用装饰器   
语法糖是python解释器提供的,  用来简便使用装饰器的一种方式.

**注意, 使用语法糖时 ,  装饰器的定义要在被装饰对象之前, 不然会报错.**


## 装饰器的叠加. 
当一个被装饰对象, 需要多个功能时, 应该使用多个不同功能的装饰器, 而==不是在一个装饰器中添加多个功能==. 


###  装饰器叠加原则. 
-  **装饰时 是 从下往上**
-  **调用时 是 从上往下**

## 有参装饰
当我们需要判断用户的权限,  就需要获取用户的身份信息, 并传入装饰器中,进行判断,  这就需要有参装饰器. 代码示例如下.
### 有参装饰器的定义
```python
def outher(需要传入的参数)
    def wrapper(func):  # 参数用于接收被装饰对象
        def inner(*args, **kwargs)  # 接收所有的被装饰对象的参数
            # 调用前添加功能
            res = func(*args, **kwargs)  # 调用被装饰对象, 并接收返回值.
            # 调用后添加功能
            return res  # 返回被装饰对象的返回值
        
        return inner  #  返回inner 
    return wrapper
```

### 有参装饰器的调用
```python
def outher(需要传入的参数)
    def wrapper(func):  # 参数用于接收被装饰对象
        def inner(*args, **kwargs)  # 接收所有的被装饰对象的参数
            # 调用前添加功能
            res = func(*args, **kwargs)  # 调用被装饰对象, 并接收返回值.
            # 调用后添加功能
            return res  # 返回被装饰对象的返回值
        
        return inner  #  返回inner 
    return wrapper


@outer(传入参数)
def func():
    pass
    
func()
```





## warps 的使用. 
在每个函数内, 我们都是需要写注释 , 来表明此函数所实现的功能,和用法. 使用装饰器的时候,  也是需要添加注释, 表明装饰器实现的是某个功能.   
- 当装饰器和被装饰对象同时有注释的时候, 调用被装饰对象的.__doc__() 方法就不能看到其注释说明. 
- 代码示例 
```python

from functools import wraps


def wrapper(func):
    def inner(*args, **kwargs):
        '''
        this is the doc from inner
        :param args:
        :param kwargs:
        :return:
        '''
        res = func()
        return res
    return inner


@wrapper
def func1():
    '''
    this is doc from func1
    :return:
    '''
    pass


print(func1.__doc__)


# 输出结果

        this is the doc from inner
        :param args:
        :param kwargs:
        :return:
        



```
由此可见,  在使用装饰器后, 被装饰对象的注释被污染, 因此可以用, wraps 进行修复. 



```python


from functools import wraps


def wrapper(func):
    @wraps(func)
    def inner(*args, **kwargs):
        '''
        this is the doc from inner
        :param args:
        :param kwargs:
        :return:
        '''
        res = func()
        return res
    return inner


@wrapper
def func1():
    '''
    this is doc from func1
    :return:
    '''
    pass


print(func1.__doc__)


```

在inner的上方使用@wraps(func) 并将被装饰函数传入wraps , 即可

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-10-%E9%97%AD%E5%8C%85%E5%87%BD%E6%95%B0%E5%92%8C%E8%A3%85%E9%A5%B0%E5%99%A8/  

