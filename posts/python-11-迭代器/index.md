# python 迭代器


##  什么是迭代器? 
迭代器就是用来迭代取值的工具.
- 迭代  
迭代是重复反馈过程的活动, 其目的是为了逼近所需目标或结果, 每一次对过程的重复称之为一次"迭代", 而每一次迭代得到的结果会作为下一次迭代的初始值, 单纯的重复不是迭代.
- 示例1- 非迭代
```python
msg = 1
while True:
    print(msg)
```
- 示例2 迭代
```python
msg = "w shi ni baba"
index= 0
while index < len(msg):
    print(msg[index])
    index += 1

```

### 可迭代对象
通过索引取值的方法实现很简单, 那么 当遇到没有索引的 集合, 字典时, 我们该怎么办呢? 这我们就需要一种不依赖索引的取值方式, 这就需要用到迭代器.
- 可迭代对象定义  
从语法角度讲, 内置有.__iter__() 方法的对象都是可迭代对象,    
字符串, 列表, 元组, 字典, 集合, 打开的文件, 都是可迭代对象

```python
l = [1, 2, 3, 4,]
l.__iter__()
等等

......
```

### 迭代器对象
可迭代对象调用内置.__iter__() 方法后返回的对象就是迭代器对象. 
```python
l = [1, 2, 3, 4,]
iter_l = l.__iter__()

```
iter_l 就是迭代器对象.
迭代器对象都有内置的.__next__()方法

==对于可迭代对象, 可以用内置的 iter() 方法, 和 next()方法进行取值 ==

```python
list1 = [1, 2, 3, 4]
i = iter(list1)
print(next(i))
print(next(i))
print(next(i))
print(next(i))
```


### 优点
- 为序列和非序列提供了一个统一取值的方法.
- 惰性计算: 迭代器对象本身是一个数据流, 只有在使用时才会用__next__() 计算出下一个值, 就迭代器本身来说,同一时刻在内存中只有一个值, 因此可以存放无限大的数据流.

### 缺点
- 除非取尽 , 否则无法获取数据的长度. 
- 只能取一个值, 不能回到取值开始, 
## for 循环原理
代码示例

```python
 list1 = [1, 2, 3, 4, 5]
 # list1 是可迭代对象, 所以可以用__iter__() 方法产生迭代器对象
 
 iter_list1 = list1.__iter__()
# 循环用迭代器对象的__next__()方法取值

while True:
    print(iter_list1.__next__())
    
# 输出结果
1
Traceback (most recent call last):
2
3
4
5
  File "D:/Python_study/Project/oldboy/day12/text.py", line 8, in <module>
    print(iter_list1.__next__())
StopIteration

# 但是当迭代器对象内的值取尽的时候, python解释器会抛出异常, 这样就需要用到异常处理

```

for 循环原理 异常处理
```python


 list1 = [1, 2, 3, 4, 5]
 # list1 是可迭代对象, 所以可以用__iter__() 方法产生迭代器对象
 
 iter_list1 = list1.__iter__()
# 循环用迭代器对象的__next__()方法取值

while True:
    try:
        print(iter_list1.__next__())
        
    except StopIteration:
        break

# 这就是for 循环原理
```

[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-11-%E8%BF%AD%E4%BB%A3%E5%99%A8/  

