# Go 变量


## 变量的定义方式

```go
package main

import "fmt"

func main() {
	// 方式一
	// var 变量名 变量类型=变量值

  
	// 注意 变量定义后 必须使用, 不适使用报错
	//  导入的包必须使用 不适用报错
	var age int =18  // 定义并赋值
	var name string  // 定义
	name = "我是你爸爸"  //赋值
	fmt.Println(name, age)

  
	//  方式二 类型推导(不需要加类型)
	var salary = 1000
	fmt.Println(salary)
	fmt.Printf("salary的类型是:%T, 值是%d\n",salary,salary)  // Printf 表示格式化输出

	// 方式三  简略声明
	male := 100
	fmt.Println(male)
  
  // 同时定义多个变量(3种方式都可以)

	var a, b,c = 1,"2","3"
	fmt.Println(a,b,c)
  
}


```

**注意**

- 变量不能重复定义
- 变量要先定义后使用
- 简略声明方式定义特殊, 冒号前至少有一个未定义变量则不报错. 
- 变量类型是固定的. 
- 变量定义了, 必须使用,否则报错
- 包导入则必须使用, 不使用就报错

## 常量

```go
//  常量 : 恒定不变得量 , 建议全部大写
// 程序在运行过程中, 不会改变值, 例如: 数据库链接地址, 端口号等
package main

// 批量声明

const (
	notFound = 404
	statusOk = 200
)
// 当 出现 n2 n3 这样的没有被赋值的声明  则默认为 n1

const (
	n1 =100
	n2
	n3
)

func main() {
  //1 const关键字 常量名 常量类型 =值
  const A int =99

	//2 常量可以定义了不使用
	const B int =99

	//3 类型可以省略
	const C  =99
	fmt.Printf("%T",b)

	//4 同时定义多个常量
	const (
		AGE=19
		NAME="zyz"
		sex="男"
	)
	fmt.Println(AGE)
	
	//5 改变常量(不允许)
	// AGE=199
	//fmt.Println(AGE)

}
```

### Iota

`iota` 是go语言中的常量计数器, 只能在常量的表达式中使用.

`iota` 在const 关键字出现时 将被重置为 0 , const 中每新增一行常量声明将使`iota` 计数加一次,  类似 枚举. 在定义枚举时候很有用.

举例说明:

```go
const (
	n1 = iota
	n2
	n3
	n4
	n5
)

```

### 常见的几个iota 例子

使用`_` 跳过某些值

```go
const (
		n1 = iota //0
		n2        //1
		_
		n4        //3
	)
```

`iota`声明中间插队

```go
const (
		n1 = iota //0
		n2 = 100  //100
		n3 = iota //2
		n4        //3
	)
const n5 = iota //0
```

多个常量声明在`一行`

```go
const (
		a, b = iota + 1, iota + 2 //1,2
		c, d                      //2,3
		e, f                      //3,4
	)
```

定义数量级 （这里的`<<`表示左移操作，`1<<10`表示将1的二进制表示向左移10位，也就是由`1`变成了`10000000000`，也就是十进制的1024。同理`2<<2`表示将2的二进制表示向左移2位，也就是由`10`变成了`1000`，也就是十进制的8。

```go
const (
		_  = iota
		KB = 1 << (10 * iota)
		MB = 1 << (10 * iota)
		GB = 1 << (10 * iota)
		TB = 1 << (10 * iota)
		PB = 1 << (10 * iota)
	)
```



[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-02-%E5%8F%98%E9%87%8F/  

