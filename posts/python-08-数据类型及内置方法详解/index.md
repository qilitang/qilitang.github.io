# python 数据类型及内置方法详解


## 一 数字类型 int 与 float
记录数字类型的事物状态
### 1.1 定义

```
age = 18
# 定义整型
weight = 63.8
# 定义浮点型
```
### 1.2 类型转换
整型和浮点型之间可以相互转换.  
-  整型转换成浮点型
整型转换为浮点型后,会默认精确到小数点后一位, 以0 补足
```
a = 18
b = float(a)
print (a,type(a),b,type(b))
#  输出结果
18 <class 'int'> 
18.0 <class 'float'>
```

- 浮点型转换为整型

```
a = 18.333
b = int(a)
print (a,type(a),b,type(b))
#  输出结果为
18.333 <class 'float'> 
18 <class 'int'>
```
转换后, 浮点数的小数部分全部舍去 , 而不是四舍五入
- int() 可以转换由纯数字组成的字符串, 
- 如果字符串中含有非纯数字则会报错

```
a = "123"
a = int(a)
print(a,type(a))
# 输出结果
123 <class 'int'>

a = "12.3"
a = int(a)
print(a,type(a))
# 输出结果

    a = int(a)
ValueError: invalid literal for int() with base 10: '12.3'

```

- 进制之间的转换
    - 十进制转其他进制

```
# 十进制转二进制
a = bin(10)
```

``` python
#  十进制转八进制
a = oct(10)
```

```python
#  十进制转十六进制
a = hex(1005)
```
- 其他进制转十进制
```python
#  使用int() 方法, 
第一个参数是字符串,   是将其他进制类型的数字当字符串传入第一个参数, 
第二个参数是int类型,  指明第一个单数的进制

int("10101010111",2)

```



### 1.3 使用
-  主要用来做数学运算, 无其他特殊用途. 

## 二 字符串
字符串就是一串描述性文字.
### 2.1 定义
定义字符串的三种方式:
- a = "abc"
用双引号引起来 即可
- a = 'abc'
用单引号
- a = '''abc'''
用三引号

<font color = 'red'>注意: 三种方式不可混用, 当字符串中含有""时 , 外面的药用 '' 包裹, 即成对出现</font>

### 2.2 类型转换
-  任意类型可以转换为字符串

```
a = 18 
print(a,type(a)
# 输出结果
18 < class "int">

b = str(b)
print(b,type(b)
# 输出结果
18 < class "str">

```

### 2.3 使用
呵~ 真沙雕,  我就是个分隔符~~~~~~~~~~~~~~~
<br>
#### 2.3.1 优先掌握的操作
-  正反向取值
-  切片
-  统计长度
-  成员运算
-  移除首尾指定字符 默认移除首尾空格
-  切分以指定字符切分字符串,  默认是以空格
-  遍历,  可以遍历所有单个字符

```python

a = "hello word"



# 1.  正/反向索引取值
print(a[6])
print(a[-3])
# 输出结果 
w
o
# 和列表的索引取值一样  正向取值从0开始, 反向取值为:从右向左1 开始.
# 字符串只能取值, 而不能修改. 


# 2. 切片(顾头不顾尾,步长)
print(a[0:5:2])
    # 0 代表其实位置,  
    # 5 代表结束位置 (此位置不计入切片范围)
    # 2 代表步长
#  输出结果
hlo


# 2.1 反向切片
a[::-1]
# 表示从右往左依次取值
# 输出结果为
dlrow olleh


# 3 len 统计字符串长度
l = len(a)
print(l,type(l))
# 输出结果
11 <class 'int'>



# 4 成员运算 in / not in  返回False/ True

print("hello" in a)
#  输出结果
True
print("python" not in a)
#  输出结果
True


# 5 .strip() 移除字符串首尾指定字符, 默认为空格

s = "    help me    "
print(s.strip())
#  输出结果
help me


s = "******help me******"
print(s.strip("*"))
#  输出结果
help me


# 6 .split()  切分  括号内不指定字符, 则默认以空格切分

m = " w shi ni dad "
print(m.split())
#  输出结果
['w', 'shi', 'ni', 'dad']


m = "127.0.0.1"
print(m.split("."))
#  输出结果
['127', '0', '0', '1']

# 输出的结果是一个列表.


# 7 可以被遍历(循环取出)

q = "我是你爸爸!"
for i in q:
    print(i)
    
#  输出结果

我
是
你
爸
爸
!

```

#### 2.3.2 需要掌握的操作
- strip lstrip rstrip

```
s = "******help me******"
print(s.strip("*"))
#  输出结果
help me

# 左侧移除
print(s.lstrip("*"))
# 输出结果
help me******

# 右侧移除
print(s.rstrip("*"))
# 输出结果
******help me
```
- 2 .lower() .upper() 将所有字符转换为大写, 小写

```
m = "My Name Is Evgeny"
print(m.lower())
print(m.upper())
#  输出结果
my name is evgeny
MY NAME IS EVGENY
#  能转换的只有英文字母,  中文则不变

```
- 3  .startswith, .endswith  以什么开头, 以什么结尾

```
s = "hello Evgeny"
print(s.startswith("h"))
print(s.endswith("y"))
# 输出结构
True
True
```
- 4 格式化输出,  有单独的一章 ,不知道在哪儿? 
[点我, 点我, 亲爱哒](https://www.cnblogs.com/Qi-Litang/articles/11755675.html)


- 5 split rsplit  切分

```
# split 是从左往右进行切分, 可以指定切分次数

s = "我|是你爸爸"
s = s.split("|",1)
print(s)
#  输出结果
["我","是你爸爸"]


# rsplit 是从左往右进行切分, 可以指定切分次数
s = "我|是|你爸爸"
s = s.rsplit("|",1)
print(s)
#  输出结果
['我|是', '你爸爸']

```
- 6 join 在可迭代对象后加入多个字符


```
print("!".join("hello"))
#  输出结果
h!e!l!l!o
```
- 7.replace 替换指定字符


```
# 用新的字符替换字符串中旧的字符
st = 'my name is Evgeny, my age is 18!'  
# 将Evgeny的年龄由18岁改成20岁
st = st.replace('18', '73')  
# 语法:replace('旧内容', '新内容')
print(st)
# 输出结果
my name is Evgeny, my age is 20!

# 可以指定修改的个数
st = 'my name is Evgeny, my age is 18!'
st = str7.replace('my', 'MY',1) # 只把一个my改为MY
print(st)
'MY name is Evgeny, my age is 18!'
```

- .isdigit 判断是否是纯数字
  

```
# 判断字符串是否是纯数字组成，返回结果为True或False
str1 = '1990'
print(str1.isdigit())
True

str2 = '123.123'
print(str8.isdigit())
False

# .isdigit 不能判断浮点数
```


#### 2.3.3 了解操作

```

# 1.find,rfind,index,rindex,count
# 1.1 find：从指定范围内查找子字符串的起始索引，找得到则返回数字1，找不到则返回-1
msg='tony say hello'
msg.find('o',1,3)  # 在索引为1和2(顾头不顾尾)的字符中查找字符o的索引
输出结果
1

# 1.2 index:同find,但在找不到时会报错
msg.index('e',2,4) # 报错ValueError

#
1.3 rfind与rindex：略
1.4 count:统计字符串在大字符串中出现的次数
>>> msg = "hello everyone"
>>> msg.count('e')  # 统计字符串e出现的次数
4
>>> msg.count('e',1,6)  # 字符串e在索引1~5范围内出现的次数
1

# 2.center,ljust,rjust,zfill
>>> name='tony'
>>> name.center(30,'-')  # 总宽度为30，字符串居中显示，不够用-填充
-------------tony-------------
>>> name.ljust(30,'*')  # 总宽度为30，字符串左对齐显示，不够用*填充
tony**************************
>>> name.rjust(30,'*')  # 总宽度为30，字符串右对齐显示，不够用*填充
**************************tony
>>> name.zfill(50)  # 总宽度为50，字符串右对齐显示，不够用0填充
0000000000000000000000000000000000000000000000tony

# 3.expandtabs
>>> name = 'tony\thello'  # \t表示制表符(tab键)
>>> name
tony    hello
>>> name.expandtabs(1)  # 修改\t制表符代表的空格数
tony hello

# 4.captalize,swapcase,title
# 4.1 captalize：首字母大写
>>> message = 'hello everyone nice to meet you!'
>>> message.capitalize()
Hello everyone nice to meet you!  
# 4.2 swapcase：大小写翻转
>>> message1 = 'Hi girl, I want make friends with you!'
>>> message1.swapcase()  
hI GIRL, i WANT MAKE FRIENDS WITH YOU!  
#4.3 title：每个单词的首字母大写
>>> msg = 'dear my friend i miss you very much'
>>> msg.title()
Dear My Friend I Miss You Very Much 

# 5.is数字系列
#在python3中
num1 = b'4' #bytes
num2 = u'4' #unicode,python3中无需加u就是unicode
num3 = '四' #中文数字
num4 = 'Ⅳ' #罗马数字

#isdigt:bytes,unicode
>>> num1.isdigit()
True
>>> num2.isdigit()
True
>>> num3.isdigit()
False
>>> num4.isdigit() 
False

#isdecimal:uncicode(bytes类型无isdecimal方法)
>>> num2.isdecimal() 
True
>>> num3.isdecimal() 
False
>>> num4.isdecimal() 
False

#isnumberic:unicode,中文数字,罗马数字(bytes类型无isnumberic方法)
>>> num2.isnumeric() 
True
>>> num3.isnumeric() 
True
>>> num4.isnumeric() 
True

# 三者不能判断浮点数
>>> num5 = '4.3'
>>> num5.isdigit()
False
>>> num5.isdecimal()
False
>>> num5.isnumeric()
False

'''
总结:
    最常用的是isdigit,可以判断bytes和unicode类型,这也是最常见的数字应用场景
    如果要判断中文数字或罗马数字,则需要用到isnumeric。
'''

# 6.is其他
>>> name = 'tony123'
>>> name.isalnum() #字符串中既可以包含数字也可以包含字母
True
>>> name.isalpha() #字符串中只包含字母
False
>>> name.isidentifier()
True
>>> name.islower()  # 字符串是否是纯小写
True
>>> name.isupper()  # 字符串是否是纯大写
False
>>> name.isspace()  # 字符串是否全是空格
False
>>> name.istitle()  # 字符串中的单词首字母是否都是大写
False
```


## 三 列表
### 3.1 定义

```
# 定义：在[]内,用逗号分隔开多个任意数据类型的值
l1 = [1,'a',[1,2]]  # 本质:l1 = list([1,'a',[1,2]])
```

### 3.2 类型转换

```
# 但凡能被for循环遍历的数据类型都可以传给list()转换成列表类型，list()会跟for循环一样遍历出数据类型中包含的每一个元素然后放到列表中
>>> list('wdad') # 结果：['w', 'd', 'a', 'd'] 
>>> list([1,2,3]) # 结果：[1, 2, 3]
>>> list({"name":"jason","age":18}) #结果：['name', 'age']
>>> list((1,2,3)) # 结果：[1, 2, 3] 
>>> list({1,2,3,4}) # 结果：[1, 2, 3, 4]
```

### 3.3 使用
#### 3.3.1 优先掌握的操作

```
# 1.按索引存取值(正向存取+反向存取)：即可存也可以取  
# 1.1 正向取(从左往右)
>>> my_friends=['tony','jason','tom',4,5]
>>> my_friends[0]  
tony
# 1.2 反向取(负号表示从右往左)
>>> my_friends[-1]  
5
# 1.3 对于list来说，既可以按照索引取值，又可以按照索引修改指定位置的值，但如果索引不存在则报错
>>> my_friends = ['tony','jack','jason',4,5]
>>> my_friends[1] = 'martthow'
>>> my_friends
['tony', 'martthow', 'jason', 4, 5]


# 2.切片(顾头不顾尾，步长)
# 2.1 顾头不顾尾：取出索引为0到3的元素
>>> my_friends[0:4] 
['tony', 'jason', 'tom', 4]
# 2.2 步长：0:4:2,第三个参数2代表步长，会从0开始，每次累加一个2即可，所以会取出索引0、2的元素
>>> my_friends[0:4:2]  
['tony', 'tom']

# 3.长度
>>> len(my_friends)
5

# 4.成员运算in和not in
>>> 'tony' in my_friends
True
>>> 'xxx' not in my_friends
True

# 5.添加
# 5.1 append()列表尾部追加元素
>>> l1 = ['a','b','c']
>>> l1.append('d')
>>> l1
['a', 'b', 'c', 'd']

# 5.2 extend()一次性在列表尾部添加多个元素
>>> l1.extend(['a','b','c'])
>>> l1
['a', 'b', 'c', 'd', 'a', 'b', 'c']

# 5.3 insert()在指定位置插入元素
>>> l1.insert(0,"first")  # 0表示按索引位置插值
>>> l1
['first', 'a', 'b', 'c', 'alisa', 'a', 'b', 'c']

# 6.删除
# 6.1 del
>>> l = [11,22,33,44]
>>> del l[2]  # 删除索引为2的元素
>>> l
[11,22,44]

# 6.2 pop()默认删除列表最后一个元素，并将删除的值返回，
# 括号内可以通过加索引值来指定删除元素
>>> l = [11,22,33,22,44]
>>> res=l.pop()
>>> res
44
>>> res=l.pop(1)
>>> res
22

# 6.3 remove()括号内指名道姓表示要删除哪个元素，没有返回值
>>> l = [11,22,33,22,44]
>>> res=l.remove(22) # 从左往右查找第一个括号内需要删除的元素
>>> print(res)
None

# 7.reverse()颠倒列表内元素顺序
>>> l = [11,22,33,44]
>>> l.reverse() 
>>> l
[44,33,22,11]

# 8.sort()给列表内所有元素排序
# 8.1 排序时列表元素之间必须是相同数据类型，不可混搭，否则报错
>>> l = [11,22,3,42,7,55]
>>> l.sort()
>>> l 
[3, 7, 11, 22, 42, 55]  # 默认从小到大排序
>>> l = [11,22,3,42,7,55]
>>> l.sort(reverse=True)  # reverse用来指定是否跌倒排序，默认为False
>>> l 
[55, 42, 22, 11, 7, 3]
# 8.2 了解知识：
# 我们常用的数字类型直接比较大小，但其实，字符串、列表等都可以比较大小， 
# 原理相同：都是依次比较对应位置的元素的大小，如果分出大小，则无需比较下一个元素，比如
>>> l1=[1,2,3]
>>> l2=[2,]
>>> l2 > l1
True
# 字符之间的大小取决于它们在ASCII表中的先后顺序，越往后越大
>>> s1='abc'
>>> s2='az'
>>> s2 > s1 # s1与s2的第一个字符没有分出胜负，但第二个字符'z'>'b',所以s2>s1成立
True
# 所以我们也可以对下面这个列表排序
>>> l = ['A','z','adjk','hello','hea']
>>> l.sort()
>>> l
['A', 'adjk', 'hea', 'hello','z']

# 9.循环
# 循环遍历my_friends列表里面的值
for line in my_friends:
    print(line) 
'tony'
'jack'
'jason'
4
5
```

#### 3.3.2 了解操作

```
>>> l=[1,2,3,4,5,6]
>>> l[0:3:1] 
[1, 2, 3]  # 正向步长
>>> l[2::-1] 
[3, 2, 1]  # 反向步长

# 通过索引取值实现列表翻转
>>> l[::-1]
[6, 5, 4, 3, 2, 1]
```

## 四 元祖

### 4.1 作用
元组与列表类似，也是可以存多个任意类型的元素，不同之处在于元组的元素不能修改，即元组相当于不可变的列表，用于记录多个固定不允许修改的值，单纯用于取

### 4.2 定义方式

```
# 在()内用逗号分隔开多个任意类型的值
>>> countries = ("中国"，"美国"，"英国")  # 本质:countries = tuple("中国"，"美国"，"英国")
# 强调：如果元组内只有一个值，则必须加一个逗号，否则()就只是包含的意思而非定义元组
>>> countries = ("中国"，)  # 本质:countries = tuple("中国")
```

### 4.3 类型转换

```
# 但凡能被for循环的遍历的数据类型都可以传给tuple()转换成元组类型
>>> tuple('wdad') # 结果：('w', 'd', 'a', 'd') 
>>> tuple([1,2,3]) # 结果：(1, 2, 3)
>>> tuple({"name":"jason","age":18}) # 结果：('name', 'age')
>>> tuple((1,2,3)) # 结果：(1, 2, 3)
>>> tuple({1,2,3,4}) # 结果：(1, 2, 3, 4)
# tuple()会跟for循环一样遍历出数据类型中包含的每一个元素然后放到元组中
```

### 4.4 使用

```
>>> tuple1 = (1, 'hhaha', 15000.00, 11, 22, 33) 
# 1、按索引取值(正向取+反向取)：只能取，不能改否则报错！  
>>> tuple1[0]
1
>>> tuple1[-2]
22
>>> tuple1[0] = 'hehe'  # 报错：TypeError:

# 2、切片(顾头不顾尾，步长)
>>> tuple1[0:6:2] 
(1, 15000.0, 22)

# 3、长度
>>> len(tuple1)  
6

# 4、成员运算 in 和 not in
>>> 'hhaha' in tuple1 
True
>>> 'hhaha' not in tuple1  
False 

# 5、循环
>>> for line in tuple1:
...     print(line)
1
hhaha
15000.0
11
22
33
```

## 五 字典
### 定义方式
- 第一种方式  
用花括号, 将key:value 这样的键值对放入, 并以逗号隔开.
d1 = {"name":"zhang", "age":18}
-  第二种方式  使用dict方法,  
d1 = dict(name="zhang", age =18 )
- 第三种方法,  
    -  zip 拉链函数. 
```python
# zip 有两个参数 第一个和第二个参数都为可迭代对象, 工作方式, 
# 第一个参数取一个值, 第二个取一个, 组成一个元组, 返回一个可迭代对象
l1 = [1,2,3,4,5]
l2 = ["a","b","c"]
l3 = zip(l1,l2)
for i in l3:
    print(i)
    
# 输出结果
(1, 'a')
(2, 'b')
(3, 'c')
 
```
生成字典的方法

```python

l1 = [1,2,3,4,5]
l2 = ["a","b","c"]
l3 = zip(l1,l2)

d1 = dict(l3)
print(d1)

# 输出结果
{1: 'a', 2: 'b', 3: 'c'}

```

### 类型转换

定义方式已有提起
```python
{}.fromkeys(可迭代对象,值)
#  使用fromkeys创建一个字典,   
#  第一个参数,  可迭代对象,  必须所有元素为不可变类型.
#  创建的字典, 是以可迭代对象的每一个元素作为新字典的 key, 然后分别与同一个值配对.  完成创建
#  结果是, 整个字典的value是第二个参数

```


### 使用
#### 5.3.1 优先掌握的操作
操作无外乎 增删改查
- 增
```python

d1 = {"a":1,"b":2}
#  1. update   更新, 传递的参数是一个字典, 如果有, 则更新最新value, 如果没有, 则添加. 
d1 = {"a":1,"b":2}
d1.update({"b":3})
print(d1)
#  输出结果
{'a': 1, 'b': 3}

update 只能更新一组数据.  so 


#  2. []  方法

d1 = {"a":1,"b":2}

d1["c"] = 3
print(d1)

# 输出结果

{'a': 1, 'b': 2, 'c': 3}

# 3 setdefault() 两个参数, 在元组里包起来. 第一个是key  第二个是value  
当 有key 存在的时候,  返回原来所对应的值, 当key 不存在 添加, 并返回最新的value 值.



```
- 删


```python

# del 方法 万能删除. 
    pass
    

# pop   参数:   key ,  传入key 删除 对应键值对 . 并返回 对应的value值

d2 = {'a': 1, 'b': 2, 'c': 3}

s = d2.pop("a")
print(d2,s)

#  输出结果
{'b': 2, 'c': 3}
1

# popitem   无需传入参数,  随机删除一个键值对, 并返回由删除的key,value 组成的元组.
d2 = {'a': 1, 'b': 2, 'c': 3}

s = d2.popitem()
print(d2,s)

#  输出结果
{'a': 1, 'b': 2} 
('c', 3)

```
- 改
新增的时候, 已说, 


-  查  

```python
# 1 用.keys  查找字典所有key
d2 = {'a': 1, 'b': 2, 'c': 3}
print(d2.keys())

#  打印结果
dict_keys(['a', 'b', 'c'])

# 2 用 .values  查找字典所有的value
d2 = {'a': 1, 'b': 2, 'c': 3}
print(d2.values())
# 返回结果

dict_values([1, 2, 3])


# 3 用 items 查找字典所有的 key 和对应的value
d2 = {'a': 1, 'b': 2, 'c': 3}
print(d2.items())

# 返回结果

dict_items([('a', 1), ('b', 2), ('c', 3)])


# 4 get 方法, 传入key  查找对应的value .  如果没有key 则返回 None

d2 = {'a': 1, 'b': 2, 'c': 3}
s = d2.get("d")
print(s)

# 返回结果
None






```



#### 5.3.2 需要掌握的操作
以上大部分都需要掌握  
## 六 集合
### 6.1 作用
主要用于  查重, 和 成员运算
### 6.2 定义
set 是以花括号包裹,单个元素以逗号隔开的数据类型. 
特点:  
- 每个元素必须是不可变类型  
- 集合内没有重复元素 
- 集合内元素无序  
s1 = {1,2,3}

定义空集合
因为 集合和字典都是以{}包裹, 所以 定义空集合必须需要set 方法. 

s1 = set()

### 6.3  类型转换
只要是能被for 循环遍历的 是非可变类型的元素, 都能传入set 进行查重. 
### 6.4 使用
#### 6.4.1 关系运算

- | 并集,        求两个集合中所有的元素,  重复只留一个
- & 交集,      求两个集合中相交的部分. 
- "-" 差集,    前一个集合中与后一个集合不相交的部分. 
- ^ 对称差集, 两个集合中取出相交的元素.
#### 6.4.2 去重
- 去重有局限性, 只能对只有不可变元素的容器进行去重
- 去重后集合内元素是无序的. 



[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-08-%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E5%8F%8A%E5%86%85%E7%BD%AE%E6%96%B9%E6%B3%95%E8%AF%A6%E8%A7%A3/  

