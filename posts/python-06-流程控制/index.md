# python 流程控制


## 流程控制

流程控制即 控制流程,  具体指控制程序的执行流程,  而程序的流程分为三种, 

- 顺序结构(以前写的代码都是顺序结构)
- 分支结构(if 判断)
- 循环结构(while 和 for 循环)

## 分支结构

### 什么是分支结构
分支结构就是根据条件判断的真假去执行不同的代码块.

### 为什么要用分支结构
人类某些时候要根据条件决定做什么事情, 比如, 今天下雨, 带伞.   
是最基本的逻辑判断.

### 如何使用分支结构

#### if 语法
用if关键字实现分支结构.

```python
if 条件1:  
    代码块1
    ......
elif 条件2:
    代码块2
    ......
elif 条件3:
    代码块3
    ......
else:
    代码块4.
#  如果以上条件符合, 则执行下面相应的代码.以上代码块能且只能执行一块. 


# 1. python 用相同的缩进(四个空格表示一个缩进) 来标识一组代码.  同一组代码块
# 2.条件可以是任意表达式, 但执行结果必须为bool类型
    # 在if 判断中的所有的数据类型都会转换为布尔类型
    # None, 0  ,[],"",{} , 换算成bool值都为False  , 非空即真
```
#### if 应用案例

```
# 案例1
# 如果: 女人年龄>3岁, 那么叫阿姨
age = input("输入年龄>>:")
if age >30:
    print("叫阿姨!"")

```

```
# 案例2
# 如果 年龄>30 叫阿姨, 否则叫姐姐
age = input("输入年龄>>:")
if age >30:
    print("叫阿姨!"")
else:
    print("叫姐姐1")
```

```python
# 案例3
# 如果女人年龄>= 18 并且<= 22 岁 并且身高>170 体重<100 并且是漂亮的. 那么表白. 否则. 阿姨好
age = input("输入年龄>>:")
height = input("目测身高>>:")
is_beauty = True
weight = input("目测体重>>:")

if 18<= age <= 22 and height >170 and weight < 100 and is_beauty:
    print("表白")
    
else:
    print("叫阿姨")

```
#### 练习登录
```python
username_from_db = "zhang"
password_from_db = 123
count = 0
tag = True
while tag:
    username = input("please input your username>>:").strip()
    password = input("please input your password>>:").strip()
    if username == username_from_db and password == password_from_db:
        print("登录成功")
        while tag:
            cmd = input(">>:")
            if cmd == "exit":
                tag = False
            else:
                print(f"调用{cmd}功能")
    else:
        print("用户名或密码错误,请重新收入")
        count += 1

    if count == 3:
        print("输错超过三次, 账号已锁定")
        tag = False
```
## 循环结构

### while 循环语法

python中 有while 与for 两中循环机制, 其中while 称之为条件循环.
```python
while 条件:
    代码1
    代码2
    ......
    
    
# while 的运行步骤, 
# 步骤1 : 如果条件成立,则依次执行代码1,代码2 ... 
# 步骤2:  执行完毕后, 再次判断条件,如果条件仍然成立,则再次执行代码1,代码2,代码3

```
案例1  
用户认证
```python
username_from_db = "zhang"
password_from_db = 123
count = 0
while count <3:
    username = input("please input your username>>:").strip()
    password = input("please input your password>>:").strip()
    if username == username_from_db and password == password_from_db:
        print("登录成功")
    else:
        print("用户名或密码错误,请重新收入")
        count +=1

```
案例二   while + break 的使用  
使用while 循环后,  当登录成功, 则需要后续操作, 需要结束本层循环 , 那么就需要break.  
- break 结束本层循环. 结束后 循环内代码都不执行. 
```python
username_from_db = "zhang"
password_from_db = 123
# 记录验证错误次数
count = 0
while count <3:
    username = input("please input your username>>:").strip()
    password = input("please input your password>>:").strip()
    if username == username_from_db and password == password_from_db:
        print("登录成功")
        break # 用于结束本层循环
    else:
        print("用户名或密码错误,请重新收入")
        count +=1
        
```

案例三,  while 循环嵌套+ break  
如果while 循环嵌套了很多层, 想要退出每层循环,则需要在每层都有一个break 

```python
username_from_db = "zhang"
password_from_db = 123
count = 0
while True:
    username = input("please input your username>>:").strip()
    password = input("please input your password>>:").strip()
    if username == username_from_db and password == password_from_db:
        print("登录成功")
        while True:  # 第二层循环
            cmd = input(">>:")
            if cmd == "exit":
                break  # 退出第二层循环
            else:
                print(f"调用{cmd}功能")
    else:
        print("用户名或密码错误,请重新收入")
        count += 1

    if count == 3:
        print("输错超过三次, 账号已锁定")
        break  # 退出第一场循环


```

案例四, while 循环嵌套, + tag 的使用   
针对嵌套多层的while循环, 如果当满足某个条件就退出所有循环,  就可以使用tag , 即 将tag 初始值为 True , 当满足某个条件后 tag 更改为False , 就会退出所有层循环  
``` python
username_from_db = "zhang"
password_from_db = 123
count = 0
tag = True
while tag:
    username = input("please input your username>>:").strip()
    password = input("please input your password>>:").strip()
    if username == username_from_db and password == password_from_db:
        print("登录成功")
        while tag:
            cmd = input(">>:")
            if cmd == "exit":
                tag = False
            else:
                print(f"调用{cmd}功能")
    else:
        print("用户名或密码错误,请重新收入")
        count += 1

    if count == 3:
        print("输错超过三次, 账号已锁定")
        tag = False

```

案例五  while + continue 的使用.   
- break 代表结束本层循环,  
- continue 代表结束本次循环, 进入下一次循环  
```python
num = 10
while num >1:
    num -=1
    if num ==6:
        continue #  结束到本次循环, 后面的代码则不会再运行, 直接进入下一次循环. 
    print(num)
# 结果 6 则不打印, 
```
案例六,  while 与 else 的使用 .  
当循环正常结束后,  执行else 后语句, 用来验证 循环是否正常结束.  
```python
count = 0
while count <5:
    count +=1
    print(f"这是第{count}次循环")
else:
    print("循环正常结束")
print("*"*50)

#  输出
这是第1次循环
这是第2次循环
这是第3次循环
这是第4次循环
这是第5次循环
循环正常结束
**************************************************
```
如果执行过程中用break , 则不会执行else语句. 

```python
count = 0
while count <5:
    count +=1
    if count ==3:
        break
    print(f"这是第{count}次循环")
else:
    print("循环正常结束")
print("*"*50)
#  输出结果
这是第1次循环
这是第2次循环
**************************************************
```

### for 循环语法.

循环结构的第二种实现方法是 for 循环, for 循环可以做的事情 while循环都可以实现,之所以使用for 是因为, for循环在遍历取值时, 更为简洁. 
- for 循环语法如下
```python

for 变量名 in 可迭代对象:  #  可迭代对象为, 字典,列表, 字符串, 等..
    代码1
    代码2
    
# 例子
for i in [1,2,3,4]:
    print(i)

#  输出结果
1
2
3
4
#  循环是, 将列表[1,2,3,4] 中的值分别取出并赋值给i 然后打印i  直到列表值取完.
```
案例1 , 打印数字1-5  
```python
# for 版本
for count in range(6): 
    print(count)
    
# while 版本
count = 0
while count<6:
    print(count)
    count +=1

# range 有三个参数,  start = 其实数字  end = 结束数字. sep =步长
# range 顾头不顾尾.

```
遍历字典, 得到的是字典中的key 而不是 值.  
```python
for k in {"name":"Evgeny", "age":18}
    print(k)

#  输出结果
name
age


```
案例 三, for 循环嵌套. 

```python
# 使用for 循环嵌套的方式打印以下图形
****
****
****

for i in range(3):
    for j in range(4):
        print("*", end = "")
    print()  # 表示换行

```
注意: break 与continue 也可以用于for循环, 使用发放与while循环相同. 



[toc]



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-06-%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/  

