# python 面向对象之 继承


## 继承的定义
继承是创建新类的一种方式,  这种方式解决了 代码冗余的问题.
- 在python中, 子类可以继承多个父类.
    -  父类又称之为 "基类" 或 "超类"
- 在其他语言中, 子类只能继承一个父类. 

```python
# 父类1
class Father1:
    pass

# 父类2
class Father2:
    pass

# 子类-- 单继承
class Son(Father1):
    pass


# 子类 多继承
class Son2(Father1, Father2):
    pass


son_obj = Son()

```
## 查看继承. 
是调用类的__bases__ 方法查看所继承的父类. 
```python
print(Son2.__bases__)

# 输出结果
(<class '__main__.Father1'>, <class '__main__.Father2'>)

```
返回的结果是一个元组, 包晗父类的信息

## 经典类与新式类.
什么是经典类, 什么是新式类?
object 是python3 中所有类的几类 , 它提供给了一些常见的方法入 ```__str__```
###  新式类
-  在python中, 所有继承object的 子类都属于新式类
-  python3 中所有的类都属于新式类, 默认全部继承object

### 经典类
- 所有没有继承object类的, 都属于经典类.
- 经典类只有在python2 中才有, 

##  抽象与继承
要想找到子类与父类之间的关系, 需要先进行抽象, 然后在类中找到相同的部分, 抽象成父类. 
-  类是由对象进行抽象, 找到对象之间的相同的关系,  然后抽象成类. 
-  父类是由类进行抽象, 找到类之间的相同关系, 然后抽象成父类

![image](https://gitee.com/qilitang/Img/raw/master/img/1036857-20170302062507110-1327024765.png)


##  继承与重用性
请先看下面例子
```python
class OldboyStudent:
    school = "oldboy"
    def __init__(self, name, age, sex):
        self.name = name
        self.age = age
        self.sex = sex
        
    def choose_course(self):
        pass
    
    
class OldboyTeacher():
    school = "oldboy"

    def __init__(self, name, age, sex):
        self.name = name
        self.age = age
        self.sex = sex
    
    def score(self):
        pass
    
```
两个类中, 都有相同的代码, 这样的话, 就会代码冗余 

两个类之中都有相同的部分, 那么就可以抽象出来一个父类

```python
class OldboyPeople:
    school = "oldboy"

    def __init__(self, name, age, sex):
        self.name = name
        self.age = age
        self.sex = sex
    
    
    

class OldboyStudent:

    def choose_course(self):
        pass


class OldboyTeacher():

    def score(self):
        pass

```
在开发程序的过程中，如果我们定义了一个类A，然后又想新建立另外一个类B，但是类B的大部分内容与类A的相同时

我们不可能从头开始写一个类B，这就用到了类的继承的概念。

通过继承的方式新建类B，让B继承A，B会‘遗传’A的所有属性(数据属性和函数属性)，实现代码重用


==提示：用已经有的类建立一个新的类，这样就重用了已经有的软件中的一部分设置大部分，大大减少了编程工作量，这就是常说的软件重用，不仅可以重用自己的类，也可以继承别人的，比如标准库，来定制新的数据类型，这样就是大大缩短了软件开发周期，对大型软件开发来说，意义重大.==

### 属性查找 **重点**
```python


class Foo:
    def f1(self):
        print('Foo.f1')

    def f2(self):
        print('Foo.f2')
        self.f1()

class Bar(Foo):
    def f1(self):
        print('Foo.f1')


b=Bar()
b.f2()
```
属性的查找 是先从对象的名称空间中查找, 然后再找子类.  

##  派生

当然子类也可以添加自己新的属性或者在自己这里重新定义这些属性（不会影响到父类），需要注意的是，一旦重新定义了自己的属性且与父类重名，那么调用新增的属性时，就以自己为准了。


以下为例.  当老师有自己的独有属性. 例如 老师有自己的工资, 那么就可以按照下面的方式进行修改

```python

class OldboyPeople:
    school = "oldboy"

    def __init__(self, name, age, sex):
        self.name = name
        self.age = age
        self.sex = sex
    
    

class OldboyTeacher():
    
    
    def __init__(self, name, age, sex, sal):
        self.name = name
        self.age = age
        self.sex = sex
        self.sal = sal
        
    
    
    def score(self):
        pass
        

```
但是有一个问题,  子类和父类都有很多的重复代码. 这样代码仍然很冗余. 如何解决这个问题呢?  
### 解决重用的代码冗余. 
- 方案1
重用父类的`__init__`的方法

```python
class OldboyPeople:
    school = "oldboy"

    def __init__(self, name, age, sex):
        self.name = name
        self.age = age
        self.sex = sex
    
    

class OldboyTeacher():
    
    
    def __init__(self, name, age, sex, sal):
        
        OldboyPeople.__init__(self, name, age, sex)
        self.sal = sal
        
    
    
    def score(self):
        pass
```

这种方法和调用类内的函数一样,  此时的__init__ 就是一个普通的函数, 使用的时候, 要将 self 传入. 


- 方案2  使用super()
super类是个特殊类, 调用此类可以直接指向父类的名称空间, 即, 在super().父类方法.  并且 不需要再手动传入 对象, 即self

```python
class OldboyPeople:
    school = "oldboy"

    def __init__(self, name, age, sex):
        self.name = name
        self.age = age
        self.sex = sex
    
    

class OldboyTeacher():
    
    
    def __init__(self, name, age, sex, sal):
        
        super().__init__(name, age, sex)
        self.sal = sal
        
    
    
    def score(self):
        pass
        
```


###  菱形继承(钻石继承)

python3 中, 新式类是以广度优先.
python2 中, 经典类是以深度优先

[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-19-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E4%B9%8B-%E7%BB%A7%E6%89%BF/  

