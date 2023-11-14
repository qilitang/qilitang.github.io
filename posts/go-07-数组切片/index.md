# Go 数组/切片/map


## 数组

### 1、定义

```go
//1 基本使用：定义
//定义了一个大小为3的int类型数组
//数组在定义阶段，大小和类型就固定了
var a [3]int    //只定义，没有初始化
fmt.Println(a)
// 因int 类型的0值是0 所以， 未初始化数组是[0,0,0]
```

### 2、使用数组

```go
var a [3]int
a[2]=100
fmt.Println(a) // [0,0,100]
fmt.Println(a[0]) // 100
```

### 3、定义并赋值

```go
// 方式1
var a [3]int=[3]int{1,2,3}
// 方式2
var a =[3]int{1,2,3}
// 方式3
a := [3]int{1, 2, 3}

//只给第2个位置设为99
a := [3]int{2:99}
// 进阶版
a := [3]int{2:99,1:88}
fmt.Println(a)

```

### 4、数组的大小是类型的一部分

```go
	//这两个不是一个类型
	var a [2]int
	var b [3]int
	a=b // 类型不一样 无法赋值
	fmt.Println(a>b)  // 类型不一样 无法比较
```

### 5、数组是值类型（当参数传递到函数中，修改不会改变原来的值）

```go
// go语言中，都是copy传递
/*
	python中都是引用传递，一切皆对象，就是地址，当做参数传递是把地址传过去了
	python中比较特殊：可变类型和不可变类型
	 */
var a [3]int=[3]int{5,6,7}
fmt.Println(a)
test1(a)
fmt.Println(a)



func test(a int){
  a[0]=999
  fmt.Println(a)
}
```

### 6、数组长度（len()）

```go
var a [3]int=[3]int{5,6,7}
fmt.Println(len(a))
```

### 7、循环数组

```go
//方式一
var a [3]int=[3]int{5,6,7}
for i:=0;i<len(a);i++{
	fmt.Println(a[i])
}
//方式二
range:是一个关键字
var a [3]int=[3]int{5,6,7}
for i,v:=range a{
	fmt.Println(i)  //索引
	fmt.Println(v)  //数组的值
}
//函数如果返回两个值，必须用两个值来接收
//range可以用一个值来接收，如果用一个值来接收，就是索引
for i:=range a{
	fmt.Println(i)  //索引
}
//只取值，不取索引
for _,v:=range a{
	fmt.Println(v)  //值
}

```

### 8、多维数组

```go
var a [3][2]int=[3][2]int{{1,2},{4,5},{9,70}}
	//a[1][0]=999
	fmt.Println(a)
```



## 切片

切片是由数组建立的一种方便、灵活且功能强大的包装（Wrapper）。切片本身不拥有任何数据。它们只是对现有数组的引用。
切片底层依附于数组

### 1、创建切片

```go
//  先创建数组
var a [9]int = [9]int{1,2,3,4,5,6,7,8,9}
// []int  就是切片的一种类型
var b []int
b = a[0:3]  // 前闭后开
// 没有 -1  没有步长
```

### 2、使用切片

```go
fmt.Println(b[0])
```

### 3、切片的修改会影响底层数组， 数组的修改也会影响切片

```go
	var a [9]int=[9]int{1,2,3,4,5,6,7,8,9}
	var b []int=a[0:3]  //前闭后开

	fmt.Println(a)
	fmt.Println(b)
	a[0]=999
	b[2]=888
	fmt.Println(a)
	fmt.Println(b)
```

### 4、切片的长度和容量

```go
var a [9]int=[9]int{1,2,3,4,5,6,7,8,9}
var b []int=a[2:3]  //前闭后开

fmt.Println(len(b))
// 切片的长度是1
//切片容量是9，意思是，可以往里追加值，追加成9个
fmt.Println(cap(b))
```

**注意** 切片的容量不是源数组的长度， 是源数组从开始切的位置到数组末尾的长度

### 5、追加值

```go
var a [9]int=[9]int{1,2,3,4,5,6,7,8,9}
var b []int=a[2:3]  //前闭后开
b=append(b,1)
b=append(b,11,22,33,44,55)
fmt.Println(len(b))
fmt.Println(cap(b))
fmt.Println(b)
fmt.Println(a)
//到了数组的尾部，继续追加值
b=append(b,999)
fmt.Println(len(b))
fmt.Println(cap(b))  //容量是14
//总结1：当切片追加值，超过了切片容量，切片容量会翻倍，在原来容量基础上乘以2
//b=append(b,222,333,444,555,666,7,8)
//fmt.Println(len(b))
//fmt.Println(cap(b))  //容量是14
//总结2：一旦超过了原数组， 就会重新申请数组，把数据copy到新数组，切片和原数组就没有关系了
//fmt.Println(a)
//fmt.Println(b)
//a[8]=7777
//fmt.Println(a)
//fmt.Println(b)
```

### 6、通过 make创建切片（底层也是依附于数组）

```go
var a []int
// 切片的零值是什么？ nil类型： 是所有引用类型的空值
fmt.Println(a)
if a==nil{
	fmt.Println("我是空的")
}
var a []int=make([]int,3,4)  // 3是长度，4是容量

a=append(a,55)
fmt.Println(a)
fmt.Println(len(a))  // 3
fmt.Println(cap(a))  // 4

```

### 7、切片定义并赋初值

```go
var a []int=[]int{1,2,3}
fmt.Println(a)
fmt.Println(len(a))
fmt.Println(cap(a))
```

### 8、切片是引用类型，当参数传递， 会修改掉原来的值

```go
var a []int=[]int{1,2,3}
fmt.Println(a)
test3(a)
fmt.Println(a)
```

### 9、多维切片

```go
var a [][]int=make([][]int,2,3)
fmt.Println(a)
fmt.Println(a[0]==nil)

a[0][0]=999  // 这样赋值会报错， 因为make 只是初始化了最外层， 即第一层[]  第二层[] 没有初始化。
// 解决， 可以使用for循环进行初始化

a[0]=make([]int,2,3)
a[0][1]=999
fmt.Println(a)
a[1][0]=99999

// 定义并赋初始值常用
var a [][]int=[][]int{{1,2,3},{4,5,6,7,7,8}}
//跟上面不一样
var a [][3]int=[][3]int{{1,2,3},{4,5,6}}
fmt.Println(a)

```

### 10、切片拷贝

```go
var a[]int = make([]int,3,4)
var b[]int=make([]int,2,6)
a[0]=11
a[1]=22
a[2]=33
b[0]=999
copy(b,a)
fmt.Println(b)

```



### 11、切片越界

```go
var a[]int = make([]int,3,4)
a[0]=11
a[1]=22
a[2]=33
a=append(a,000)
// 中括号取值， 只能取到长度， 不能取到容量大小
fmt.Println(a[3])
```

## Maps: hash, 字典  (key:value)

### 1、map的定义和使用

```go
// map[key]value 类型： key 的类型必须是可hash的， key值： 数字， 字符串
// map的零值是 nil， 它是一个引用类型
var a map[int]string
fmt.Println(a)
if a==nil{
	fmt.Println("我是空的")
}
```

### 2、map的使用

```go
// 定义未初始化
var map[int]string 
// 定义并初始化 使用make
var map[int]string = make(map[int]string)
// 使用[] 添加/修改， 
a[1]="zhang"
a[2]:"wang"
a[3]:"qilitang"

// a["xx"] key 值不能乱写
fmt.Println(a)
```

### 3、获取元素

```go
var a map[int]string=make(map[int]string)
fmt.Println(a[0])  //取出value值的空值  ""

```

**统一的方案来判断value值是否存在**

```go
//a[0] 可以返回两个值，一个是value值（可能为空），另一个是true或false
var a map[int]int=make(map[int]int)
a[0]=0
v,ok:=a[0]
fmt.Println(v)
fmt.Println(ok)
```

### 4、map 删除元素

```go
var a map[int]int=make(map[int]int)
a[1]=11
a[2]=22
fmt.Println(a)
//根据key删（内置函数）
delete(a,1)
fmt.Println(a)

```

### 5、map长度

```go
var a map[int]int=make(map[int]int)
fmt.Println(len(a))
a[1]=11
a[2]=22
fmt.Println(len(a))
```

### 6、map是引用类型

```go
var a map[int]int=make(map[int]int)
a[1]=11

test4(a)
fmt.Println(a)



func test4(a map[int]int)  {
	a[1]=999
	fmt.Println(a)
}
```

### 7、 map的相等性

```go
var a map[string]string=make(map[string]string)
a["name"]="zhang"
var b map[string]string=make(map[string]string)
b["name"]="zhang"
//  不能判断， map只能跟nil比较
if a==nil {

}
```

### 8、循环map

```go
// 需要借助内置range循环
var a map[string]string=map[string]string{"name":"zhang","age":"19","sex":"男"}

	for k,v:=range a{
		fmt.Println(k)
		fmt.Println(v)
	}


```

### 9、map是无序的

```go
// 如何让map变为有序
```





[toc]



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-07-%E6%95%B0%E7%BB%84%E5%88%87%E7%89%87/  

