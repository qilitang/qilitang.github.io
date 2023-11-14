# python 面向对象之 封装


##   组合
###  什么是组合?
- 在一个类中, 以另外一个类的对象作为数据属性, 称之为组合


###  组合有什么用?
- 组合与继承都是用来解决代码的重用性问题
- 继承是一种"是"的关系. 例如, 老师是人, 学生也是人 都有, 名字, 年龄 性别
- 组合是一种 "有" 的关系,
## 怎么用组合?
看下面代码

```python
# 示例

class People:
    def __init__(self, name, age, sex):
        self.name = name
        self.age = age
        self.sex = sex


class Teacher(People):

    def __init__(self, name, age, sex,):
        super().__init__(name, age, sex, )
        self.course = []

    def add_course(self, course):
        self.course.append(course)

    def show_course(self):
        for line in self.course:
            print(line.c_name, line.c_price, line.c_cycle)

class Course:

    def __init__(self, c_name, c_price, c_cycle):
        self.c_name = c_name
        self.c_price = c_price
        self.c_cycle = c_cycle
#  实例化处一个 python课程
python_obj = Course("python", 22000, "6 mon")
#  将python课程 关联到 老师对象中
tea1 = Teacher("七里塘", 18, "male")
tea1.add_course(python_obj)
#  查看组合内对象的属性
# print(tea1.course[0].c_name)

# 为了便于查看, 需要在类内定义方法.
tea1.show_course()

```

## 封装


### 1. 什么是封装
- 解释1:  
封装就是将一堆属性即方法封装起来.
    - 1: 定义类的过程就是封装, 由类产生对象,就是讲对象的属性和方法封装进对象体内.
- 解释2:  
也有说 在类内 定义属性和方法时 使用__属性/方法名 这种方式是封装.
2: 这种方式, 遵循的是
    - 在对象内部属性的使用是开放的,
    - 在对象外部使用内部属性或方法是封闭的

2. 为什么用封装?
        可以通过 “对象.” 的方式 “存放/获取” 属性或方法。
        对象拥有 "." 的机制。
        方便数据的存取。

3. 如何进行封装,
    在定义类的时候, 就进行了封装
"""

```python
示例
class User:
    x = 10
    def __init__(self):
        pass

```
### 访问限制机制:
#### 什么是访问限制机制:
- 在创建类的时候, 定义类内部的 属性和方法是, 使用__ 开头, 
- 凡是以__开头的属性/方法,都会被限制, 外部不能直接访问

==注意: 凡是在类内部定义__开头的属性或方法，都会变形为``` _类名__属性```/方法。==

#### 为什么要用 访问限制机制?

将一些隐私数据隐藏起来, 不让外部轻易获取 


如何实现?

```python
# 示例1

class User:
    _name = "王大"
    def tell_name(self):
        print(self._name)

obj = User()
# 直接用.属性名 方法 会找不到,
# print(obj.__name)
"""
Traceback (most recent call last):
  File "D:/Python_study/Project/oldboy/day22/封装.py", line 55, in <module>
    print(obj.__name)
AttributeError: 'User' object has no attribute '__name'
"""
#  所以只能通过接口调用
# obj.tell_name()
```
###  什么是property


property 是将类中的方法, 伪装成属性的方法. 

[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-18-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E4%B9%8B-%E5%B0%81%E8%A3%85/  

