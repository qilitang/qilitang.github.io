# python 生成器


##  生成器理论
### 1.什么是生成器？
- 生成的工具。
- 生成器是一个 "自定义" 的迭代器， - 本质上是一个迭代器。

### 2.如何实现生成器
但凡在函数内部定义了的yield，
调用函数时，函数体代码不会执行，
会返回一个结果，该结果就是一个生成器。

### yield:
- 每一次yield都会往生成器对象中添加一个值。
- yield只能在函数内部定义
- yield可以保存函数的暂停状态

### yield与return:
- 相同点:
    - 返回值的个数都是无限制的。

- 不同点:
    - return只能返回一次值，yield可以返回多个值


示例 自定义range
```python
def my_range(start, end=None, step=1):
    if end is None:
        end = start
        start = 0
    while start < end:
        yield start
        start += step


for i in my_range(1,11,2):
    print(i)
```

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-12-%E7%94%9F%E6%88%90%E5%99%A8/  

