# Go if-else/ for 循环/ switch


### if else 

- 基本使用

  ```go
  package main
  
  import "fmt"
  // if else if else
  func main() {
  	a := 10
  	if a == 10 {
  		fmt.Println("13")
  	} else if a > 10 {
  		fmt.Println("a大于10")
  	} else {
  		fmt.Println("a小于10")
  	}
  }
  
  ```

  **不能换行（go语言每一行结尾，需要加一个;  ,每当换行，会自动加;）**

- 在条件里可以进行初始化操作,(作用域范围的区别)

  ```go
  package main
  
  import "fmt"
  func main() {
  	a := 10
  	
  	if  a < 10 {
  		fmt.Println("xxx")
  	} else {
  		fmt.Println("yyyy")
  	}
  
  }
  
  
  
  // 2
  package main
  
  import "fmt"
  
  func main() {
  
  	if  a:=10 ;a < 10 {
  		fmt.Println("xxx")
  	} else {
  		fmt.Println("yyyy")
  	}
  }
  ```

  

### for 循环

- 基本语法

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	// 基本语法
  	// for 初始化; 条件判断; 自增/自减 { 循环体内容}
  	// 打印1=9
  	for i:=1; i<10; i++{
  		fmt.Println(i)
  	}
  
  }
  ```

- 省略第一部分

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	// 2 省略第一部分（初始化）,作用域范围不一样
  	i:=0
  	for ;i<10 ; i++ {
  		fmt.Println(i)
  	}
  
  }
  ```

- 省略第三部分

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	//3 省略第三部分
  	for i:=0;i<10 ;  {
  		i++
  		fmt.Println(i)
  	}
  }
  ```

-  省略第一部分和第三部分

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	//4 省略第一和第三部分
  	i := 0
  	for ; i < 10; {
  		i++
  		fmt.Println(i)
  	}
  
  }
  ```

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	//for 条件 {}
  	i := 0
  	for i < 10 {
  		i++
  		fmt.Println(i)
  	}
    
    //  死循环
    	for true{
  		fmt.Println(1)
  	}
    // 死循环
    for {
      fmt.Println(1)
    }
  }
  ```

- break continue(和python 中一样)



###  switch 语句

```go

//switch
package main

func main() {
	// 1 switch 基本使用
	//a:=10
	//switch a {
	//case 1:
	//	fmt.Println("1")
	//case 2:
	//	fmt.Println(2)
	//case 9:
	//	fmt.Println(9)
	//case 10:
	//	fmt.Println("10")
	//}

	//2 default
	//a:=15
	//switch a {
	//case 1:
	//	fmt.Println("1")
	//case 2:
	//	fmt.Println(2)
	//case 9:
	//	fmt.Println(9)
	//case 10:
	//	fmt.Println("10")
	//default:
	//	fmt.Println("不知道")
	//}

	//3 多条件
	//a:=3
	//switch a {
	//case 1,2,3:
	//	fmt.Println("1")
	//case 4,5,6:
	//	fmt.Println(2)
	//case 7,9:
	//	fmt.Println(9)
	//case 10,16:
	//	fmt.Println("10")
	//default:
	//	fmt.Println("不知道")
	//}


	//4 无表达式
	//a:=3
	//switch  {
	//case a==1 || a==3:
	//	fmt.Println("1")
	//case a==4||a==5:
	//	fmt.Println(2)
	//default:
	//	fmt.Println("不知道")
	//}

	//5 fallthrough,无条件执行下一个case
	//a:=1
	//switch  {
	//case a==1 || a==3:
	//	fmt.Println("1")
	//	//fallthrough  //fallthrough 会无条件执行下一个case
	//case a==4||a==5:
	//	fmt.Println(2)
	//	fallthrough
	//default:
	//	fmt.Println("不知道")
	//}
}

```





[toc]



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-05-if-else%E8%AF%AD%E5%8F%A5/  

