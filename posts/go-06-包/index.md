# Go 包


### 包

```go
//1 在同一个包下（文件夹下），包名必须一致
//2 以后，包名就是文件夹的名字
//3 同一个包下，同名函数只能有一个（init除外）
//4 一个包（当成一个文件），同一包下的函数，直接调用即可
//5 导包的位置，从src路径开始
//6 包只要在src路径下就可以导入
//7 大写表示导出，在外部包可以使用，小写只能再包内部适应
//8 使用第三方包：go get github.com/astaxie/beego  （放到gopath的src路径下）
    package main
    import "github.com/astaxie/beego"

    func main() {
      beego.Run()
    }
```

### mode模式

```go
//1 包导入 import . "github.com/astaxie/beego"  类似于python中form xx import *
//2 包导入 import _ "github.com/go-sql-driver/mysql"  触发init的执行，但是不试用包内的函数
//3 包导入 import f "fmt"   重命名，以后直接用f

//4 对比python中__init__.py  
//在代码中导入模块
import xx   实质上触发__init__.py  的执行(在__init__.py中也可以执行其他代码，对应到go中就是init函数)
一般情况下，在__init__.py写from xx import 会使用到的函数，导过来
以后再用 xx.函数()

//5 go mod没有之前，可以设置多个gopath，开发某个项目，切到不同的gopath，类似于虚拟环境

//6 go env
	-GO111MODULE="off"  表示go mod模式是关闭的，用gopath
	-一旦开启了go mod模式，代码不需要放在src路径下（任意位置都可以）
	-在项目路径下要有 go.mod 文件（文件里记录了，go版本，项目依赖的包，包的版本）
	-写代码，建自己的包即可
	-一般情况下，项目有一个main.go  内部是main包，main函数，整个程序的入口
```

[toc]

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-06-%E5%8C%85/  

