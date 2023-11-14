# python 数据类型


# 数据类型
##  一. 什么是数据类型?
数据类型就是用来描述事物某个特征最恰当的方式,  
例如: 
- 用整数描述年龄
- 用浮点数描述小数
- 用字符串(文字类型)描述一些事物的信息. 
等等等等

##  二. 类型有哪些? 怎么用?
### 2.1. 整型: int 类型
- 作用:记录 年龄,电话号, QQ号, 等数字
- 定义  

```
age = 18  #  age = int(18)
print(age,type(age)
# 输出结果
18 <class 'int'>
```
定义int 类型, 是由int()这个功能函数实现的

### 2.2浮点数:float 类型

- 作用:记录工资,体重,身高等非整数类型数据  

- 定义

```
height = 1.81  # height = float(1.81)
print(height,type(height))
#  输出结果
1.81 <class 'float'>

```
定义 int 类型,是由float()这个函数来实现的. 

####  数字类型之间的运算
- 整型和浮点数 做数字运算, python 会自动将整型转换为浮点数类型.

```
a = 10
b = 1.5
c = a*b
print(c,type(c))
#  输出结果为
15.0 <class 'float'>
```

### 字符串:str 类型
- 作用: 用来记录一段描述信息.  例如: 公司简介, 名称等信息
- 定义
可以用' '," ",''' '''定义一个字符串

```
name = "Evgeny"
name1 = 'Evgeny'
info = '''
company: 俄讯通文化有限公司
成立时间: 2017年
'''
print(name,type(name),
name1,type(name1),
info,type(info))
#  输出结果
Evgeny <class 'str'>
Evgeny <class 'str'> 
company: 俄讯通文化有限公司
成立时间: 2017年
 <class 'str'>
```
区别:
    '' 和 ""  本质上没有区别, 当同时用到的时候, 需要进行区分, 例如
    
```
print("my name is "Evgeny"")
# 输出结果

    print("my name is "Evgeny"")
                            ^
SyntaxError: invalid syntax

```

这样就会报错 因为 python 解释器无法识别 哪两个"" 是一组. 所以会报错, 
正确用法是 

```
print("my name is 'Evgeny'")
```
- '''str''' 可以进行换行操作, 示例如上. 

#### str 字符串类型的运算
- 字符串之间可以相加

```
a = "my"
b = "name"
print(a+b)
#  输出结果为
myname
```
  <font size=2 color = "orange">  # 注, 字符串相加的底层操作是: </font>  
        - 先申请一个新的内存空间  
        - 将两个字符串value 拷贝,然后存入到新得内存空间  
        <font size=2 color = "red">
        要尽量避免这种操作, 效率低下. 推荐占位符方法.  </font>
        
- 字符串可以与整型进行乘法操作  



```
print("="*20)
# 输出结果
====================
```
可以做分割线使用.

### 列表
- 作用: 用来描述一个事物的同一种特性的不同数据, 例如: 人的爱好
- 定义:
  
```
l = [1,2.3,"hello",[10,25] # l = list([1,2,3,"hello",[10,25])

print(l)
# 输出结果
[1,2.3,"hello",[10,25]
```
由示例课件, 列表中的元素可以为整型, 字符串, 浮点数 乃至于一个列表


#### 列表数据的取出,及索引
定义好数据之后,最终的目的是为了使用.    
所以 如何正确取出列表中的数据,就变得很重要,  
列表数据是按照以"0"为起始的下表索引. 具体请看示例

```
l = [10,22,33,"hello",[10,25] #  定义一个列表
print(l[0])
# 输出结果
10
```
- 如何取出列表内列表的值?
请看下面例子

```
students_info= [["wxx",18,"henan"],["lxx",24,"hunan"]]
# 找到第二个人的省份
print(students_info[1][2])
# 输出结果
hunan

```

由示例可见, 想找到固定信息, 只需要按照索引,一级一级的向下找即可.

### 字典
字典是以key:value 这样成组键值对出现的集合体. 
- 作用
当一个事物有多种的特性, 特性又有多种状态的情况下, 列表已经不能满足我们的需要, 因位列表值得取用是按照列表的位置索引取用, 当数据量很大时, 取出的值并不能看出是属于哪种特性.  
```
# 举个栗子.
info_evgeny = ["evgeny",18,"man","company",]
#  如上所示, 当想取出 这个人的"年龄" 信息时 , 该如何快速取出? 
#  当然可以用info_evgeny[1] 取出,  
```
看了上面的代码, 我们就会发现一个问题. 这个列表只有四个值,  我们能一眼看出年龄所在列表的位置,  当 这个列表包晗的不仅仅只有这四个, 我们不能一眼看出位置索引时 , 我们应该怎么办?   
那么  新的数据类型, 字典 dict 就出现了. 

- 定义
定义列表有两种方式, 
```
# 第一种方式
dict1 = {"name":"evgeny","age":18,"gender":"man","name_company":"company"}
print(dict1)
# 输出结果
{'name': 'evgeny', 'age': 18, 'gender': 'man', 'name_company': 'company'} 
# 第二种方式
dict2 = dict({"name":"evgeny","age":18,"gender":"man","name_company":"company"})
# 输出结果
{'name': 'evgeny', 'age': 18, 'gender': 'man', 'name_company': 'company'}

```

- 使用
当字典中取某个值时, 只需要知道这组值得key, 就可以

```
dict1 = {"name":"evgeny","age":18,"gender":"man","name_company":"company"}
age = dict1["age"]
print(age)
# 输出结果
18

```

### 布尔bool
什么是布尔类型, 
- 作用
为了记录真假 这两种状态
- 定义

```
is_ok = True
is_ok = False
```
- 使用
通常用在if while 语句, 用作判断条件. 

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-05-%E5%9F%BA%E7%A1%80%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/  

