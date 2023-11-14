# python GIL全局解释器锁


## GIL 全局解释器锁

### 什么是GIL 全局解释器锁

```python
"""
In CPython, the global interpreter lock, or GIL, is a mutex that prevents multiple native threads from executing Python bytecodes at once. This lock is necessary mainly because CPython’s memory management is not thread-safe. (However, since the GIL exists, other features have grown to depend on the guarantees that it enforces.)
"""
```

GIL 本质就是一把互斥锁， 本质上是将并发变为串行

每个进程内都会存在一把GIl ， 同一进程内的多个线程必须要抢到GIL才行使用Cpython解释器来执行自己的代码， 即 同一进程下的多个线程无法实现并行，但是可以实现并发。



在Cpython 解释器下， 如果想实现并行可以开启多个进行

### 为什么要有GIL

Python的内存管理不是线程安全的原因就是  垃圾回收机制。

因为Cpython 解释器的垃圾回收机制不是线程安全的

 



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-23-gil-%E5%85%A8%E5%B1%80%E8%A7%A3%E9%87%8A%E5%99%A8%E9%94%81/  

