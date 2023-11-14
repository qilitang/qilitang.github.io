# python 面向对象


#  面向对象编程
我们以前已经学了面向过程编程思想.  那什么是面向对象编程呢? 首先我们要搞明白 什么是对象, 然后才能去了解什么是面向对象编程.

## 对象的概念
面向对象的核心是 "对象" 二字, 而对象的精髓在于 "整合",   
即 对象是 "特征与技能"的结合体.   
**要了解对象为何物，必须把自己当成上帝，上帝眼里世间存在的万物皆为对象，不存在的也可以创造出来。** 
  面向对象的程序设计好比如来设计西游记，如来要解决的问题是把经书传给东土大唐，如来想了想解决这个问题需要四个人：唐僧，沙和尚，猪八戒，孙悟空，每个人都有各自的特征和技能（这就是对象的概念，特征和技能分别对应对象的数据属性和方法属性），然而这并不好玩，于是如来又安排了一群妖魔鬼怪，为了防止师徒四人在取经路上被搞死，又安排了一群神仙保驾护航，这些都是对象。然后取经开始，师徒四人与妖魔鬼怪神仙交互着直到最后取得真经。如来根本不会管师徒四人按照什么流程去取),对象是特征与技能的结合体，基于面向对象设计程序就好比在创造一个世界，你就是这个世界的上帝，存在的皆为对象，不存在的也可以创造出来，与面向过程机械式的思维方式形成鲜明对比，面向对象更加注重对现实世界的模拟，是一种“上帝式”的思维方式。

-  优点:  
**解决了程序的扩展性。对某一个对象单独修改，会立刻反映到整个体系中，如对游戏中一个人物参数的特征和技能修改都很容易。**

- 缺点:  
    - 1. 编程的复杂度远高于面向过程，不了解面向对象而立即上手基于它设计程序，极容易出现过度设计的问题。一些扩展性要求低的场景使用面向对象会徒增编程难度，比如管理linux系统的shell脚本就不适合用面向对象去设计，面向过程反而更加适合。

    - 2. 无法向面向过程的程序设计流水线式的可以很精准的预测问题的处理流程与结果，面向对象的程序一旦开始就由对象之间的交互解决问题，即便是上帝也无法准确地预测最终结果。于是我们经常看到对战类游戏，新增一个游戏人物，在对战的过程中极容易出现阴霸的技能，一刀砍死3个人，这种情况是无法准确预知的，只有对象之间交互才能准确地知道最终的结果。
    
- 应用场景:  
**需求经常变化的软件，一般需求的变化都集中在用户层，互联网应用，企业内部软件，游戏等都是面向对象的程序设计大显身手的好地方** 
面向对象的程序设计并不是全部。对于一个软件质量来说，面向对象的程序设计只是用来解决扩展性  
![](https://gitee.com/qilitang/Img/raw/master/img/1036857-20170301105507938-1164072796.png)


## 类与对象
类即类别、种类，是面向对象设计最重要的概念，对象是特征与技能的结合体，而类则是一系列对象相似的特征与技能的结合体  

- **在现实世界中：先有对象，再有类**

世界上肯定是先出现各种各样的实际存在的物体，然后随着人类文明的发展，人类站在不同的角度总结出了不同的种类，如人类、动物类、植物类等概念

也就说，**对象是具体的存在，而类仅仅只是一个概念，并不真实存在**


- **在程序中：务必保证先定义类，后产生对象**

这与函数的使用是类似的，先定义函数，后调用函数，类也是一样的，在程序中需要先定义类，后调用类

不一样的是，调用函数会执行函数体代码返回的是函数体执行的结果，而调用类会产生对象，返回的是对象  
```python


#在现实世界中，站在老男孩学校的角度：先有对象，再有类
对象1：李坦克
    特征:
        学校=oldboy
        姓名=李坦克
        性别=男
        年龄=18
    技能：
        学习
        吃饭
        睡觉

对象2：王大炮
    特征:
        学校=oldboy
        姓名=王大炮
        性别=女
        年龄=38
    技能：
        学习
        吃饭
        睡觉

对象3：牛榴弹
    特征:
        学校=oldboy
        姓名=牛榴弹
        性别=男
        年龄=78
    技能：
        学习
        吃饭
        睡觉


现实中的老男孩学生类
    相似的特征:
        学校=oldboy
    相似的技能：
        学习
        吃饭
        睡觉

在现实世界中：先有对象，再有类
```
在程序中，务必保证：先定义（类），后使用（产生对象）
```python

PS:
  1. 在程序中特征用变量标识，技能用函数标识
  2. 因而类中最常见的无非是：变量和函数的定义

#程序中的类
class OldboyStudent:
    school='oldboy'
    def learn(self):
        print('is learning')
        
    def eat(self):
        print('is eating')
    
    def sleep(self):
        print('is sleeping')
  


#注意：
  1.类中可以有任意python代码，这些代码在类定义阶段便会执行
  2.因而会产生新的名称空间，用来存放类的变量名与函数名，可以通过OldboyStudent.__dict__查看
  3.对于经典类来说我们可以通过该字典操作类名称空间的名字（新式类有限制），但python为我们提供专门的.语法
  4.点是访问属性的语法，类中定义的名字，都是类的属性

#程序中类的用法
.:专门用来访问属性，本质操作的就是__dict__
OldboyStudent.school #等于经典类的操作OldboyStudent.__dict__['school']
OldboyStudent.school='Oldboy' #等于经典类的操作OldboyStudent.__dict__['school']='Oldboy'
OldboyStudent.x=1 #等于经典类的操作OldboyStudent.__dict__['x']=1
del OldboyStudent.x #等于经典类的操作OldboyStudent.__dict__.pop('x')


#程序中的对象
#调用类，或称为实例化，得到对象
s1=OldboyStudent()
s2=OldboyStudent()
s3=OldboyStudent()

#如此，s1、s2、s3都一样了，而这三者除了相似的属性之外还各种不同的属性，这就用到了__init__
#注意：该方法是在对象产生之后才会执行，只用来为对象进行初始化操作，可以有任意代码，但一定不能有返回值
class OldboyStudent:
    ......
    def __init__(self,name,age,sex):
        self.name=name
        self.age=age
        self.sex=sex
    ......


s1=OldboyStudent('李坦克','男',18) #先调用类产生空对象s1，然后调用OldboyStudent.__init__(s1,'李坦克','男',18)
s2=OldboyStudent('王大炮','女',38)
s3=OldboyStudent('牛榴弹','男',78)


#程序中对象的用法
#执行__init__,s1.name='牛榴弹'，很明显也会产生对象的名称空间
s2.__dict__
{'name': '王大炮', 'age': '女', 'sex': 38}

s2.name #s2.__dict__['name']
s2.name='王三炮' #s2.__dict__['name']='王三炮'
s2.course='python' #s2.__dict__['course']='python'
del s2.course #s2.__dict__.pop('course')

在程序中：先定义类，后产生对象

```

##  **！！！细说__init__方法！！！**
```

#方式一、为对象初始化自己独有的特征
class People:
    country='China'
    x=1
    def run(self):
        print('----->', self)

# 实例化出三个空对象
obj1=People()
obj2=People()
obj3=People()

# 为对象定制自己独有的特征
obj1.name='egon'
obj1.age=18
obj1.sex='male'

obj2.name='lxx'
obj2.age=38
obj2.sex='female'

obj3.name='alex'
obj3.age=38
obj3.sex='female'

# print(obj1.__dict__)
# print(obj2.__dict__)
# print(obj3.__dict__)
# print(People.__dict__)





#方式二、为对象初始化自己独有的特征
class People:
    country='China'
    x=1
    def run(self):
        print('----->', self)

# 实例化出三个空对象
obj1=People()
obj2=People()
obj3=People()

# 为对象定制自己独有的特征
def chu_shi_hua(obj, x, y, z): #obj=obj1,x='egon',y=18,z='male'
    obj.name = x
    obj.age = y
    obj.sex = z

chu_shi_hua(obj1,'egon',18,'male')
chu_shi_hua(obj2,'lxx',38,'female')
chu_shi_hua(obj3,'alex',38,'female')





#方式三、为对象初始化自己独有的特征
class People:
    country='China'
    x=1

    def chu_shi_hua(obj, x, y, z): #obj=obj1,x='egon',y=18,z='male'
        obj.name = x
        obj.age = y
        obj.sex = z

    def run(self):
        print('----->', self)


obj1=People()
# print(People.chu_shi_hua)
People.chu_shi_hua(obj1,'egon',18,'male')

obj2=People()
People.chu_shi_hua(obj2,'lxx',38,'female')

obj3=People()
People.chu_shi_hua(obj3,'alex',38,'female')




# 方式四、为对象初始化自己独有的特征
class People:
    country='China'
    x=1

    def __init__(obj, x, y, z): #obj=obj1,x='egon',y=18,z='male'
        obj.name = x
        obj.age = y
        obj.sex = z

    def run(self):
        print('----->', self)

obj1=People('egon',18,'male') #People.__init__(obj1,'egon',18,'male')
obj2=People('lxx',38,'female') #People.__init__(obj2,'lxx',38,'female')
obj3=People('alex',38,'female') #People.__init__(obj3,'alex',38,'female')


# __init__方法
# 强调：
#   1、该方法内可以有任意的python代码
#   2、一定不能有返回值
class People:
    country='China'
    x=1

    def __init__(obj, name, age, sex): #obj=obj1,x='egon',y=18,z='male'
        # if type(name) is not str:
        #     raise TypeError('名字必须是字符串类型')
        obj.name = name
        obj.age = age
        obj.sex = sex


    def run(self):
        print('----->', self)


# obj1=People('egon',18,'male')
obj1=People(3537,18,'male')

# print(obj1.run)
# obj1.run() #People.run(obj1)
# print(People.run)

！！！__init__方法之为对象定制自己独有的特征
```
**PS~~**  

**1. 站的角度不同，定义出的类是截然不同的，详见面向对象实战之需求分析**

**2. 现实中的类并不完全等于程序中的类，比如现实中的公司类，在程序中有时需要拆分成部门类，业务类......**

**3. 有时为了编程需求，程序中也可能会定义现实中不存在的类，比如策略类，现实中并不存在，但是在程序中却是一个很常见的类**

### 类的特殊属性(了解即可)


```
#python为类内置的特殊属性
类名.__name__# 类的名字(字符串)
类名.__doc__# 类的文档字符串
类名.__base__# 类的第一个父类(在讲继承时会讲)
类名.__bases__# 类所有父类构成的元组(在讲继承时会讲)
类名.__dict__# 类的字典属性
类名.__module__# 类定义所在的模块
类名.__class__# 实例对应的类(仅新式类中)

```

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-17-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%BC%96%E7%A8%8B/  

