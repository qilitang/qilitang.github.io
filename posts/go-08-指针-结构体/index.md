# Go 指针/结构体/方法


## 指针

### 1、定义指针

```go
// 指向int类型的指针（指向什么类型指针，就是在什么类型前加星号）
var a *int
```

### 2、指针的零值

```go
fmt.Println(a) //nil
```

### 3、使用指针

```go
var a *int
var b int=100
// a指针指向b  （取谁的地址，就是在谁前面加一个&）
a=&b
fmt.Println(a)  // 0xc0000b4008
```

### 4、了解（骚操作）

```go
var a *int
var b **int= &a
var c ***int= &b
var d ****int =&c
fmt.Println(d)
```

### 5、 解引用 （把指针反解成值）（在指针变量前加*，表示解引用）

```GO
fmt.Println(*a)
fmt.Println(***d)
```

### 6、 向函数传递指针参数

```go
var a int =100
////在函数中把a的值改成101
fmt.Println(a)
test2(&a)
fmt.Println(a)


func test2(a *int)  {
	//解引用然后，自增1
	//*a++
	*a=*a+100
	fmt.Println(*a)
}
```

### 7、不要向函数传递数组指针， 而应该使用切片

```go
var a [4]int  = [4]int{1,2,3}
fmt.Println(a)
//test3(&a)
test4(a[:])
fmt.Println(a)
```

### 8、go 不支持指针运算(在c中可以通过数组指针的运算进行取值)

```go
var b int=100
var a *int=&b
//a++
```

### 9、 指针数组和数组指针

```go
// 指针数组 --> 数组里面放指针
var a int =10
var b int =20
var c int =30
var d [3]*int=[3]*int{&a,&b,&c}
fmt.Println(d)

// 数组指针  --> 指针数组指针
var a [3]int=[3]int{1,3,4}
var b *[3]int=&a
fmt.Println(b)
```

## 结构体

### 1、 什么是结构体

```go
//结构体,go语言中的面向对象
//结构体是用户定义的类型,表示若干个字段（Field）的集合
//类比面向对象中的类，只有属性，没有方法
```

### 2、 定义一个结构体

```go
// type关键字 结构体名字 struct关键字 {一个一个的字段}
type Person struct {
	name string
	age int
	sex string
}
```

### 3、使用结构体

```go
//（结构体的零值，是属性的零值)不是nil，它不是引用类型，是值类型
var a Person
fmt.Println(a)
```

### 4、 定义并初始化

```go
// 1
var a Person=Person{}
// 2
var a Person=Person{name:"lqz",age:19,sex:"男"}
// 3
var a =Person{name:"lqz",age:19,sex:"男"}
// 4
a :=Person{name:"lqz",age:19,sex:"男"}
// 5
a :=Person{sex:"男",name:"lqz",age:19}
// 6 
a :=Person{sex:"男",name:"lqz"}
	//按位置初始化(固定位置，并且都传)
a :=Person{"lqz",19,"男"}
fmt.Println(a)
```

### 5、 使用结构体

```go
var a Person=Person{name:"zhang",age:19,sex:"男"}
fmt.Println(a.name)
a.name="wang"
fmt.Println(a)
```

### 6、 匿名结构体（没有名字,没有type关键字，定义在函数内部）

```go
//什么情况下用？只使用一次，数据整合在一起
a:=struct {
  name string
  age int
  sex string
}{"lqz",19,"男"}
fmt.Println(a.name)
```

### 7、结构体的指针（结构体是值类型）

```go
var a *Person=&Person{"zhang",19,"男"}
fmt.Println(a)
//Go 语言允许我们在访问 字段时，可以使用 emp8.firstName 来代替显式的解引用 (*emp8).firstName
fmt.Println((*a).name)
fmt.Println(a.name)
```



### 8、 匿名字段（字段没有名字,不允许有多个同类型的字段）

**有什么用？用来做字段提升，面向对象中的继承**

```go
type Person struct {
	 string
	 int
}
//按位置实例化
a:=Person{"zhang",19}
	//按关键字实例化
a:=Person{int:19,string:"zhang"}
fmt.Println(a)
fmt.Println(a.string)
fmt.Println(a.int)


```

### 9、嵌套结构体（结构体套结构体）

**不管是结构体名字还是字段名字，大写字母开头，表示导出字段，表示在外部包可以使用**

```go
//面向对象封装（python __）
type Person struct {
	Name string
	Age int
	Sex string
	hobby Hobby
}
type Hobby struct {
	HobbyId int
	HobbyName string
}
```

### 10、 提升字段

```go
type Person struct {
	Name string
	Age int
	Sex string
	Hobby
}
type Hobby struct {
	HobbyId int
	HobbyName string
}

// 此处就相当于是python中 类的继承， 求中 Hobby 是父类  Person 是子类
// 提升字段可以直接在Person 结构体的实例中以点的方式进行访问
```

11、字段冲突

```go
type Person struct {
	Name string
	Age int
	Sex string
	Hobby
}
type Hobby struct {
	HobbyId int
	Name string
}
// 当父结构体（父类）Hobby 中有Name  Person中也有Name  当进行实例化的时候， 优先访问的是子结构体Person中的name  这也是子类重写父类的属性。
```



## 方法

结构体+方法就是实现面向对象中的类

### 1、 什么是方法

方法其实就是一个函数，在 func 这个关键字和方法名中间加入了一个特殊的接收器类型。接收器可以是结构体类型或者是非结构体类型。接收器是可以在方法的内部访问的

- python中什么是方法， 什么是函数？

  ```python
  """
  1、 方法是面向对象中的概念，对象绑定方法，类的绑定方法，可以自动传值，
  2、 类中不加任何装饰器定义的函数，是对象的绑定方法，对象来调用，可以自动传值，类也可以来掉，如果类来调用，他就是普通函数，有几个值就传几个值
  3、 类的绑定方法，类来调用，会把类自动传入
  """
  ```

### 2、方法的定义

```go
//定义一个结构体
type Person struct {
	name string
	age int
	sex string
}
//给结构体绑定方法(无参数)
func (p Person1)PrintName()  {
	fmt.Println(p.name)
}
// 绑定一个修改名字的方法(有参数)
func (p Person1)ChangeName(name string)  {
	p.name=name
	fmt.Println("--------",p)
}

//普通函数
func PrintName(p Person1)  {
	fmt.Println(p.name)
}

p:=Person1{"zhang",19,"男"}
p.PrintName() // zhang
```

### 3、 为什么要有结构体的绑定方法

[Go 不是纯粹的面向对象编程语言](https://golang.org/doc/faq#Is_Go_an_object-oriented_language)，而且Go不支持类。因此，基于类型的方法是一种实现和类相似行为的途径。

相同的名字的方法可以定义在不同的类型上，而相同名字的函数是不被允许的。假设我们有一个 `Square` 和 `Circle` 结构体。可以在 `Square` 和 `Circle` 上分别定义一个 `Area` 方法。见下面的程序。

### 4、值接收器和指针接收器

```go
//指针类型接收器，修改age的方法
func (p *Person1)ChangeAge(age int)  {
	p.age=age
	//(*p).age=age   //正常操作
	fmt.Println("--------",p)
}


p:=Person1{"zhang",19,"男"}
p.ChangeName("wang")  // 值接收器，改新的，不会影响原来的，不会影响当前的p对象
fmt.Println(p)
// 调用指针类型接收器的方法
p.ChangeAge(100)  //指针类型接收器，会影响原来的，会影响当前的p对象
fmt.Println(p)

```

### 5、值接收器和指针接收器的应用场景

```go
//用指针接收器的情况
	//1 方法内部的改变，影响调用者
	//2 当拷贝一个结构体的代价过于昂贵时 ,就用指针接收器
//其他情况，值接收器和指针接收器都可以
```



### 6、匿名字段的方法（重点）

```go
p:=Person1{"wang",19,"男",Hobby1{1,"篮球"}}
//匿名字段，属性可以提升
//匿名字段，方法也提升
p.printName()  // 也提升了  子类对象调用父类中的方法，子类中没有，就是调用父类的--》 对象.方法名
p.Hobby1.printName() // 正常操作 直接指名道姓的调用父类中的方法---》 super().方法名


// 子类中重写了父类的方法（嵌套结构体中有重名的方法）
p.printName()
```

### 7、在方法中使用值接收器 与 在函数中使用值参数

```go
type Person1 struct {
	name string
	age int
	sex string
}
//方法中使用值接收器
func (p Person1)printName()  {
	fmt.Println(p.name)
}
p:=Person1{"zhang",19,"男"}
p.printName()    // zhang
printName(p)

//在函数中使用值参数
func printName(p Person1)  {
	fmt.Println(p.name)
}
```

### 8、在方法中使用指针接收器 与 在函数中使用指针参数

```go
type Person1 struct {
	name string
	age int
	sex string
}
//在方法中使用指针接收器
func (p *Person1)printName()  {
	fmt.Println(p.name)
}
//在函数中使用指针参数
func printName(p *Person1)  {
  fmt.Println(p.name)  // printName(&a) 
}
```

### 9、在非结构体上的方法

```go
// 给数据类型加方法
// 错误方式
func (i int)add()  {
	i=i+i
}


// 自己定义类型， 可以绑定方法
type myInt int
func (i *myInt)add() {
  (*i)++
	fmt.Println(*i)
}

i:=10
i.add()
```

[toc]



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-08-%E6%8C%87%E9%92%88-%E7%BB%93%E6%9E%84%E4%BD%93/  

