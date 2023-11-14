# 函数




##  函数

go为编译型语言, 需要先编译后执行, 所以 go中函数不会像python 一样需要先定义后执行, go 中函数 可以写在任意位置

- 语法

  ```go
  // 语法 func(关键字) 函数名() {}
  ```

- 无参 无返回值函数

  ```go
  package main
  import "fmt"
  func main() {
  	t1()
  }
  // 无参数, 无返回值
  func t1() {
  	fmt.Println("231")
  }
  ```

- 有参数 无返回值

  ```go
  package main
  
  import "fmt"
  
  func main() {
  	t1(132)
  
  
  }
  // 有参数 无返回值
  // go中函数没有关键字参数, 所有的都是位置参数, 也没有默认参数
  func t1( i int) {
  	fmt.Println(i)
  }
  ```

- 多个参数 无返回值

  ```go
  package main
  
  import "fmt"
  
  func main() {
  	t1(132,456)
  
  }
  // go中函数没有关键字参数, 所有的都是位置参数, 也没有默认参数
  func t1( i int, m int) {
  	fmt.Println(i)
  }
  // 如果参数为同一个类型, 可以简写
  
  func t1( i,m int) {
  	fmt.Println(i)
  }
  
  ```

- 多个参数有一个返回值(需要指明返回值的类型)

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	i := t1(132, 456)
  	fmt.Println(i)
  }
  // 多个参数 有一个返回值
  func t1(i,m int) int{
  	return i+m
  }
  ```

- 多个参数 多个返回值

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	i,m := t1(132, 456)
  	fmt.Println(i,m)
  }
  // 多个参数 有一个返回值
  func t1(i,m int) (int,int){
    c := i+m
    d := i*m
    return c,d
  }
  ```

- 补充

  ```go
  
  //func test(a,b int)(){}
  //func test(a,b int)(int){}
  //func test(a,b int)(int,string,int){}
  //func test(a,b int)int{}
  
  ```

- 可变长参数

  ```go
  // 可变长参数
  package main
  
  import "fmt"
  
  func main() {
  	i, m := t1(132, 456, 321)
  	fmt.Println(i, m)
  }
  func t1(m int, i ...int) (int, int) {
  	c := m + i[0]
  	d := i[0] * i[1]
  	return c, d
  }
  
  ```

  函数是一等公民**(头等函数), 在go 中函数也是一个类型

- 返回值时函数类型

  ```go
  package main
  
  import "fmt"
  
  func main() {
  	i := t1()
  	i()
  }
  
  func t1() func() {
  	var a func()=func (){
  		fmt.Println("我是内层函数")
  	}
  	return a
  }
  
  //func t1() func() {
  //	return func (){
  //		fmt.Println("我是内层函数")
  //	}
  //}
  ```

- 闭包函数(定义在函数内部, 对外部作用域的引用)

  闭包函数的本质: 多了一种传参方式

  ```go
  func t1(b int) func() {
  	a:= func() {
  		fmt.Println(b)
  	}
  	return a
  }
  // 在go 语言中没有装饰器语法糖, 需要自己手动实现装饰器
  ```

- 闭包函数高级

  ```go
  func t1(b int) func(x,y int) {
  	var a func(x,y int)
  	a= func(x,y int) {
  		fmt.Println(b+x+y)
  	}
  	return a
  }
  //  禁止套娃!
  ```

- 匿名空接口

  ```go
  // 可边长参数补充
  func Println(a ...interface{}) (n int, err error) {
  	return Fprintln(os.Stdout, a...)
  }
  //  a ... interface{}  为 匿名空接口
  ```

- 补充， 命名返回值

  

  ```go
  
  func test(x,y int)(a int)  {
  	a=x+y
  	return
  }
  //  在指定返回值数据类型时， 可以指定要返回的内部名称变量。  
  //  上面例子中 就是将返回值命名给a
  ```

- 给类型重命名

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	var a MyFunc
  	a(1,2)
  }
  //func(x,y int) 类型命名为MyFunc
  type MyFunc func(x,y int)
  
  func test() MyFunc {
  	return func(x,y int) {
  		fmt.Println(x+y)
  		fmt.Println("xxx")
  	}
  
  }
  ```

  

###  装饰器的实现

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	test := director(test)
	a:=test()
	fmt.Println(a)
}

func director(fun func() int) func() int{
	a := func() int {
		start_time := time.Now().Unix()
		fmt.Println(start_time)
		time.Sleep(time.Second)
		res := fun()

		end_time := time.Now().Unix()
		fmt.Println(end_time)
		return res
	}
	return a
}

func test() int{
	fmt.Println("我是主函数")
	return 0
}
// 套娃套到头疼!
```



[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-04-%E5%87%BD%E6%95%B0/  

