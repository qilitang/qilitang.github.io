# Go 接口 协程 信道 异常处理


## 1 接口

接口定义一个对象的行为。接口只指定了对象应该做什么，至于如何实现这个行为（即实现细节），则由对象本身去确定。

（python中 abc 模块  可以强制要求子类必须重写父类的方法 否则报错）

- python和go都属于鸭子类型，非侵入式接口
- java：侵入式接口

### 1、定义接口 （接口是一系列行为的集合：一系列方法的集合）

```go
type Duck interface {
	run()
	speak()
}
//只要结构体绑定了接口中的所有方法，这个结构体就叫实现了Duck接口
type TDuck struct {
	name string
	age int
	wife string
}
//实现TDuck接口(绑定接口中的所有方法)
func (t TDuck)Run()  {
  fmt.Println("我是唐老鸭，我的名字是",t.name,"我说人话")
}
func (t TDuck)Speak()  {
	fmt.Println("我是唐老鸭，我的名字是",t.name,"我学人走路")
}

//定义一个RDuck结构体
type RDuck struct {
	name string
	age int
}
//RDuck实现接口（实现接口中的所有方法）
func (t RDuck)Run()  {
	fmt.Println("我是普通肉鸭，我的名字是",t.name,"我阿嘎嘎叫")
}
func (t RDuck)Speak()  {
	fmt.Println("我是普通肉鸭，我的名字是",t.name,"我歪歪扭扭走路")
}

```

### 2、 实例化得到TDuck和 RDuck 两个对象

```go
t:=TDuck{"鸡哥",88,"凤姐"}
r:=RDuck{"普通鸭子",2}
t.Run()
t.Speak()
r.Run()
r.Speak()
//接口也是一个类型（可以定义一个变量是接口类型）
// 同一类事物的多种形态
var d DuckInterface
d=t
//d=r
d.Speak()
d.Run()
	//想再去t的属性，name，age，wife
test(t)   // {鸡哥 88 凤姐}
	//test(r)
```

### 3、 类型断言

```go
func test(d DuckInterface)  {
	//d.Run()
	//d.Speak()
	//var t TDuck
	//t=d.(TDuck)
	t:=d.(TDuck) //断言d是TDuck类型，如果正确，就会把d转成t
	fmt.Println(t.name)
	fmt.Println(t.wife)
	t.Speak()
	t.Run()
}
```

### 4、 类型选择

```go
func test(d DuckInterface)  {
	switch a:=d.(type) {
	case TDuck:
		fmt.Println(a.wife)
		fmt.Println("你是TDuck类型")
	case RDuck:
		fmt.Println(a.name)
		fmt.Println("你是RDuck类型")
	default:
		fmt.Println("不知道是什么类型")
	}
}
```

### 5、 空接口

```go
//-只要一个类型，实现接口所有的方法，就叫实现该接口
//-如果一个接口是空的，一个方法都没有，所有类型都实现了空接口
//-任意类型，都可以赋值给空接口类型
var a EmptyInterface
a=1
a="xx"
a=[3]int{1,2,3}

```

### 6、 接口类型空值（nil）

```go
var a EmptyInterface
fmt.Println(a)
```

### 7、有名 / 匿名空接口

```go
// 有名空接口
type EmptyInterface interface {
}
// 匿名空接口
func test(i interface{})  { 
}
```



### 8、 实现多个接口

```go
type DuckInterface1 interface {
	Run()
	Speak()
}

type HumanInterface interface {
	Drive()
}


//定义一个TDuck结构体
type TDuck1 struct {
	name string
	age int
	wife string
}
//TDuck实现接口（实现接口中的所有方法）
func (t TDuck1)Run()  {
	fmt.Println("我是唐老鸭，我的名字是",t.name,"我说人话")
}
func (t TDuck1)Speak()  {
	fmt.Println("我是唐老鸭，我的名字是",t.name,"我学人走路")
}
//实现HumanInterface接口
func (t TDuck1)Drive()  {
	fmt.Println("我是唐老鸭，我开车")
}

func main() {
	t:=TDuck1{"鸡哥",18,"凤姐"}
	var d DuckInterface1
	var h HumanInterface
	d=t
	d.Run()
	d.Speak()
	h=t
	h.Drive()
}


```



### 9、 接口嵌套

```go
type DuckInterface1 interface {
	Run()
	Speak()
}

type HumanInterface interface {
	DuckInterface1
	//相当于
	//Run()
	//Speak()
	Drive()
}
```

## 2 并发和并行

```python
# 并发：假如在他晨跑时，鞋带突然松了。于是他停下来，系一下鞋带，接下来继续跑
# 并行：如这个人在慢跑时，还在用他的 iPod 听着音乐
```



## 3 go 协程

go协程 -->  goroutine，并不是真正的协程（线程+协程，语言层面处理了，不需要开发者去关注）

```go
package main

import (
	"fmt"
	"time"
)

func hello()  {
	fmt.Println("go go go！")
}
func main() {
	fmt.Println("主函数开始")
	go hello() //通过go关键字，开启goroutine，并发执行
	go hello()
	go hello()
	go hello()

	fmt.Println("主函数结束")
	time.Sleep(2*time.Second)

}

```

## 4 信道

**goroutine直接通信**

**go语言不推崇共享变量方法做通信，而推崇管道通信channel（信道）**

### 1、 信道也是一个变量（需要指明运输类型）

```go
var a chan int //定义一个int类型信道
```

### 2、 信道的零值， nil类型， 它是一个引用

```go
fmt.Println(a)
```

### 3、 信道初始化

```go
var a chan int=make(chan int)
fmt.Println(a)
```

### 4、 信道放值和取值

```go
var a chan int=make(chan int)
a<-1   //放值，把1放到信道中
var b int=<-a    //取值

//<-a    //取值
```

### 5、 默认情况下，信道的放值和取值都是阻塞的(一次一个都放不进去)

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("开始")
	var a chan bool=make(chan bool)
	//信道是引用类型
	go hello1(a)
	<-a
	time.Sleep(2*time.Second)
}	

func hello1(a chan bool)  {
	fmt.Println("go go go")
	a<-true
	fmt.Println("xxx")
}
```

### 6、 信道案例

```go
package main
/*
输入 453  计算每一位的平方和和每一位的立方和的和
squares = (4 * 4) + (5 * 5) + (3 * 3)
cubes = (4 * 4 * 4) + (5 * 5 * 5) + (3 * 3 * 3)
output = squares + cubes
 */


import (
	"fmt"
)

func calcSquares(number int, squareop chan int) {
	sum := 0
	for number != 0 {
		digit := number % 10   //% 取余数 453对10取余数--》3--》5--》4
		sum += digit * digit
		number /= 10          // /除以  453除以10----》45--》5--》0
	}
	squareop <- sum
}

func calcCubes(number int, cubeop chan int) {
	sum := 0
	for number != 0 {
		digit := number % 10
		sum += digit * digit * digit
		number /= 10
	}
	cubeop <- sum
}

func main() {
	number := 4535
	sqrch := make(chan int)
	cubech := make(chan int)
	go calcSquares(number, sqrch)
	go calcCubes(number, cubech)
	squares, cubes := <-sqrch, <-cubech
	//squares:= <-sqrch
	//cubes := <-cubech
	fmt.Println("Final output", squares + cubes)
}
```

### 7、 死锁 、 单项信道 与 信道关闭

```go
package main

func main() {
	//1 死锁

	//var a chan int=make(chan int)
	//a<-1  //一直阻塞在这，报死锁错误
	//<-a     //一直阻塞在这，报死锁错误

	//2 单向信道（只写或者只读）
	//sendch := make(chan int) //定义一个可写可读信道
	//go sendData(sendch)
	//fmt.Println(<-sendch)  //只能往里写值，取值报错

	//3 信道的关闭 close
	//sendch := make(chan int)
	//close(sendch)

}
func sendData(sendch chan<- int) {  //转成只写信道
	sendch <- 10
	//<-sendch  //只要读就报错
}
```

```go
package main

import (
	"fmt"
)

func producer(chnl chan int) {
	for i := 0; i < 10; i++ {
		chnl <- i
	}
	close(chnl)
}
func main() {
	ch := make(chan int)
	go producer(ch)
	for v := range ch {  //如果信道没关闭，一直取值，直到没有值，会报死锁
		fmt.Println("Received ",v)
	}
}

```

## 5 缓冲信道

有缓冲的信道， 可以放多个值

### 1、 缓冲信道的定义和死锁问题

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
  
  var a chan int =make(chan int,2)  //长度为4的有缓冲信道
	a<-1
	a<-2
	//管子满了
	//a<-5  //报死锁错误
	//推断出无缓冲信道 var a chan int =make(chan int,0)
	fmt.Println(<-a)
	fmt.Println(<-a)
	//管子没有东西了，再取，报死锁
	//fmt.Println(<-a)
}
```

### 2、 信道的容量和长度

```go
//长度是目前管道中有几个值，容量是管道最多能容纳多少值
var a chan int =make(chan int,4)
fmt.Println(len(a))
fmt.Println(cap(a))
a<-1
a<-2
fmt.Println(len(a))
fmt.Println(cap(a))
```

### 3、 小案例 (通过信道实现goroutine的同步)

```go
var a chan int =make(chan int,3)
//b:=<-a
go test3(a)
fmt.Println(<-a)

fmt.Println(<-a)



func test3(a chan int)  {
	a<-1

	time.Sleep(time.Second*2)
	fmt.Println("假设我在运算")

	a<-2
	close(a)

}
```

### 4、 通过 waitgroup 实现同步

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
		no := 3
		var wg sync.WaitGroup  //值类型，没有初始化，有默认值
		for i := 0; i < no; i++ {
			wg.Add(1)
			go process(i, &wg)
		}
		wg.Wait() //add了几次，必须有几个wg.Done()对应，否则一直等在这
		fmt.Println("All go routines finished executing")
}

func process(i int, wg *sync.WaitGroup) {
	fmt.Println("started Goroutine ", i)
	time.Sleep(2 * time.Second)
	fmt.Printf("Goroutine %d ended\n", i)
	wg.Done()
}


```

## 6 异常处理

- defer:延迟执行，即便程序出现严重错误，也会执行
- panic：主动抛出异常 raise
- recover：恢复程序，继续执行

```go
//异常处理
package main

import "fmt"

//func main() {
//	f1()
//	f2()
//	f3()
//}
//func f1()  {
//	fmt.Println("f1")
//}
//
//func f2()  {
//	fmt.Println("f2")
//	//如果这个地方出了异常
//}
//func f3()  {
//	fmt.Println("f3")
//}

//defer:延迟执行，即便程序出现严重错误，也会执行
//panic：主动抛出异常 raise
//recover：恢复程序，继续执行

//func main() {
//	//defer fmt.Println("我最后才执行")  //先注册，等函数执行完成后，逆序执行defer注册的代码
//	//defer fmt.Println("ddddd")
//	defer func() {
//		fmt.Println("我最后才执行")
//	}()
//	defer func() {
//		fmt.Println("我最后才执行")
//		//出异常
//		//这个代码执行不到了
//	}()
//
//	fmt.Println("111")
//	fmt.Println("222")
//	panic("我出错了")  //主动抛出异常
//	//var a []int =make([]int,2,3)
//	//fmt.Println(a[9])
//	fmt.Println("这句话还会执行吗 ？不会了")
//}

//在defer中恢复程序，继续执行
func main() {

	f1()
	f2()
	f3()
}
func f1()  {
	fmt.Println("f1")
}

func f2()  {
	defer func() {
		//recover() //恢复程序继续执行
		if err:=recover();err!=nil{  //err 如果不为nil(空），表示出了异常
			fmt.Println(err)  //把异常信息打印出来
		}

	}()
	fmt.Println("f2")
	//如果这个地方出了异常
	panic("我出错了")

	fmt.Println("永远执行不到")
}
func f3()  {
	fmt.Println("f3")
}

// python中
//print('ssss')
//try:
//	print('ssss')
//	raise("xxxx")
//	print('我永远不会执行')
//except Exception as e:
//	print(e)
//finally:
//	print('我永远会执行')

//go 中
//print('ssss')
//defer func() {
//	if err:=recover();err!=nil{
//		fmt.Println(err)
//	}
//	//finally 写在这
//	print('我永远会执行')
//}()
//print('ssss')
//panic("xxxx")
//print('我永远不会执行')
```

[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-09-%E6%8E%A5%E5%8F%A3/  

