# python 面向对象之 绑定方法与非绑定方法


## 一 绑定方法

 类中定义的函数分为两大类：绑定方法和非绑定方法

 其中绑定方法又分为绑定到对象的对象方法和绑定到类的类方法。

 在类中正常定义的函数默认是绑定到对象的，而为某个函数加上装饰器@classmethod后，该函数就绑定到了类。

 类方法通常用来在```__init__```的基础上提供额外的初始化实例的方式

```python
# 配置文件settings.py的内容
HOST='127.0.0.1'
PORT=3306

# 类方法的应用
import settings
class MySQL:
    def __init__(self,host,port):
        self.host=host
        self.port=port
    @classmethod
    def from_conf(cls): # 从配置文件中读取配置进行初始化
        return cls(settings.HOST,settings.PORT)

>>> MySQL.from_conf # 绑定到类的方法
<bound method MySQL.from_conf of <class ‘__main__.MySQL'>>
>>> conn=MySQL.from_conf() # 调用类方法，自动将类MySQL当作第一个参数传给cls
```

绑定到类的方法就是专门给类用的，但其实对象也可以调用，只不过自动传入的第一个参数仍然是类，也就是说这种调用是没有意义的，并且容易引起混淆，这也是Python的对象系统与其他面向对象语言对象系统的区别之一，比如Smalltalk和Ruby中，绑定到类的方法与绑定到对象的方法是严格区分开的。

## 二 非绑定方法

为类中某个函数加上装饰器 @staticmethod 后，该函数就变成了非绑定方法，也称为静态方法。该方法不与类或对象绑定，类与对象都可以来调用它，但它就是一个普通函数而已，因而没有自动传值那么一说

```python
import uuid
class MySQL:
    def __init__(self,host,port):
        self.id=self.create_id()
        self.host=host
        self.port=port
    @staticmethod
    def create_id():
        return uuid.uuid1()

>>> conn=MySQL(‘127.0.0.1',3306)
>>> print(conn.id) #100365f6-8ae0-11e7-a51e-0088653ea1ec

# 类或对象来调用create_id发现都是普通函数，而非绑定到谁的方法
>>> MySQL.create_id
<function MySQL.create_id at 0x1025c16a8>
>>> conn.create_id
<function MySQL.create_id at 0x1025c16a8>
```

总结绑定方法与非绑定方法的使用：若类中需要一个功能，该功能的实现代码中需要引用对象则将其定义成对象方法、需要引用类则将其定义成类方法、无需引用类或对象则将其定义成静态方法。

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-20-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E4%B9%8B-%E7%BB%91%E5%AE%9A%E6%96%B9%E6%B3%95%E4%B8%8E%E9%9D%9E%E7%BB%91%E5%AE%9A%E6%96%B9%E6%B3%95/  

