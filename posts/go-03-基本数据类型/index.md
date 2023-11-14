# 基本数据类型


## 基本数据类型

### 整型

整型分为以下两个大类： 按长度分为：int8、int16、int32、int64 对应的无符号整型：uint8、uint16、uint32、uint64

其中，`uint8`就是我们熟知的`byte`型，`int16`对应C语言中的`short`型，`int64`对应C语言中的`long`型。

```go
有符号整型：（长度不同，表示的数字范围不一样）
    -int:在32位机器上是int32，在64位机器上是int64
    -int8 ：8个bit位，一个字节，正负 2的7次方-1的范围
    -int16: 正负 2的15次方-1的范围
    -int32:
    -int64:
无符号整数：
    -uint8 ：2的8次方-1
    -uint16：
    -uint32
    -uint64
		复数：
			-complex类型：（了解，不知道更好）实部和虚部
		其他：
			-byte：uint8的别名
			-rune:int32的别名

```
#### 特殊整型

```go
int : 在32位机器上是int32，在64位机器上是int64
uint : 在32位机器上是uint32，在64位机器上是uint64
uintptr:  无符号整型, 用于存放一个指针
```

**注意**  在使用`int` 和`uint`类型时, 不能假定它是32位或者64位整型, 而是要考虑int 和uint 在不同平台的差异

**注意事项**   获取对象的长度的内建`len()`函数返回的长度可以根据不同平台的字节长度进行变化。实际使用中，切片或 map 的元素数量等都可以用`int`来表示。在涉及到二进制传输、读写文件的结构描述时，为了保持文件的结构不会受到不同编译目标平台字节长度的影响，不要使用`int`和 `uint`。

#### 进制转换

```go
package main
 
import "fmt"
 
func main(){
	// 十进制
	var a int = 10
	fmt.Printf("%d \n", a)  // 10
	fmt.Printf("%b \n", a)  // 1010  占位符%b表示二进制
 
	// 八进制  以0开头
	var b int = 077
	fmt.Printf("%o \n", b)  // 77
 
	// 十六进制  以0x开头
	var c int = 0xff
	fmt.Printf("%x \n", c)  // ff
	fmt.Printf("%X \n", c)  // FF
}
```

### 浮点型

Go语言支持两种浮点型数：`float32`和`float64`。这两种浮点型数据格式遵循`IEEE 754`标准： `float32` 的浮点数的最大范围约为 `3.4e38`，可以使用常量定义：`math.MaxFloat32`。 `float64` 的浮点数的最大范围约为 `1.8e308`，可以使用一个常量定义：`math.MaxFloat64`。

打印浮点数时，可以使用`fmt`包配合动词`%f`，代码如下：

```go
package main
import (
        "fmt"
        "math"
)
func main() {
        fmt.Printf("%f\n", math.Pi)
        fmt.Printf("%.2f\n", math.Pi)
}
```

### 复数

complex64和complex128

```go
var c1 complex64
c1 = 1 + 2i
var c2 complex128
c2 = 2 + 3i
fmt.Println(c1)
fmt.Println(c2)
```

复数有实部和虚部，complex64的实部和虚部为32位，complex128的实部和虚部为64位。

### 布尔值

Go语言中以`bool`类型进行声明布尔型数据，布尔型数据只有`true（真）`和`false（假）`两个值。

**注意：**

1. 布尔类型变量的默认值为`false`。
2. Go 语言中不允许将整型强制转换为布尔型.
3. 布尔型无法参与数值运算，也无法与其他类型进行转换。

### 字符串

Go语言中的字符串以原生数据类型出现，使用字符串就像使用其他原生数据类型（int、bool、float32、float64 等）一样。 Go 语言里的字符串的内部实现使用`UTF-8`编码。 字符串的值为`双引号(")`中的内容，可以在Go语言的源码中直接添加非ASCII码字符，

用双引号包裹的内容,反引号 ``
			反引号可以换行

**注意**   双引号包含的是字符串   单引号包含的是字符

例如：

```go
var a string= "我是你爸爸"
```

### 字符串转义符

Go 语言的字符串常见转义符包含回车、换行、单双引号、制表符等，如下表所示。

| 转义符 | 含义                               |
| :----- | :--------------------------------- |
| `\r`   | 回车符（返回行首）                 |
| `\n`   | 换行符（直接跳到下一行的同列位置） |
| `\t`   | 制表符                             |
| `\'`   | 单引号                             |
| `\"`   | 双引号                             |
| `\\`   | 反斜杠                             |

举个例子，我们要打印一个Windows平台下的一个文件路径：

```go
package main
import (
    "fmt"
)
func main() {
    fmt.Println("str := \"c:\\Code\\lesson1\\go.exe\"")
}
```

### 字符串的常用操作

| 方法                                | 介绍           |
| :---------------------------------- | :------------- |
| len(str)                            | 求长度         |
| +或fmt.Sprintf                      | 拼接字符串     |
| strings.Split                       | 分割           |
| strings.contains                    | 判断是否包含   |
| strings.HasPrefix,strings.HasSuffix | 前缀/后缀判断  |
| strings.Index(),strings.LastIndex() | 子串出现的位置 |
| strings.Join(a[]string, sep string) | join操作       |



4 补充类型零值

```go
// 补充：类型的默认值
	var a int
	var b float32
	var c string
	var d bool
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)
	fmt.Println(d)
```





##  数据类型的转换

```go
//类型转换(强类型:不通类型之间不能做运算)
	//类型转换
	//var a int =19
	//var b float32=18.1
	//float转成int类型，小数点后直接弃用，不是四舍五入
	//fmt.Println(a+int(b))
	//var b float32=18.9999
	//fmt.Println(int(b))

	//恶心的地方(了解)
	//var a int =199   //64为操作系统int64
	//var b int64=199
	////int和int64不是一个类型
	//fmt.Println(a+int(b))
```



[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-03-%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/  

