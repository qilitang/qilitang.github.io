# python 内置函数


# 内置函数
由python提供的函数功能, 就是内置函数, 常见内置函数有以下几种. 
本节示例都是和匿名函数连用

## max 
去一个可迭代对象的最大值 , 是按照 ASCII 码的先后顺序进行比较.   
参数:  
max(可迭代对象, key= 比较对象)

示例1
```python
l = [2, 3, 6, 8, 1, 10]

res = max(l)

print(res)

```
示例2
```python
#  要求, 取出薪资最大的人员
dict1 = {
    'tank': 1000,
    'egon':/ 500,
    'sean': 200,
    'jason': 500
}

res= max(dict1, key = lambda x:dict1[x])
print(res)


```
##  min 函数
略过 和max 方法相同, 取最小值. 


## sorted  方法  排序
将 可迭代对象按照升序进行排列

```python

dict1 = {
    'tank': 1000,
    'egon': 500,
    'sean': 200,
    'jason': 500
}
# 以 key 进行排序
dict2 = sorted(dict1)

print(dict2)

# 以 value 进行排序, 这就需要使用到匿名函数
dict2 = sorted(dict1.items(), lambda x:x[1])


#  次函数按照key 在ASCII码顺序进行排列, 返回的是keys 组成的列表

```

## map  映射

-  参数1  函数对象
-  参数2  可迭代对象  
 示例
```python
l = [1, 2, 3, 4]

s = map(lambda x:x+2, l)
print(s)
#  返回值是个 <map object at 0x000001DF3C878978>
<!--用list 取出值-->

print(list(s)
# [3, 4, 5, 6]
```

## reduce  合并
- 参数1  函数对象
- 参数2  可迭代对象
- 参数3  默认初始值
```python
res = reduce(lambda x, y: x + y, range(1, 101), 0)
print(res)

# 5050

```
## filter  过滤

- 参数1  函数对象
- 参数2  可迭代对象

满足条件的 返回.


```python



name_list = ['egon_dsb', 'jason_dsb',
             'sean_dsb', '大饼_dsb', 'tank']
filter_obj = filter(lambda name: name.endswith('_dsb'), name_list)
print(filter_obj)
print(list(filter_obj)) 
```




---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-14-%E5%86%85%E7%BD%AE%E5%87%BD%E6%95%B0/  

