# Go语言介绍及环境搭建






## go语言介绍

```python
#  诞生于09年, 是谷歌公司推出的.
 -python  1989年
 -java   1990年

# 缺点:
	生态不是特别好
```

- Go 语言特性

  ```python
  # 跨平台编译型语言
  # 语法接近C
  # 有垃圾回收机制(gc)
  # 支持面向对象和面向过程式的编程模式.(没有class 但是支持)
  ```

- Go 语言发展

  ```python
  # 目前最新的是1.14.2 版本. go语言向下兼容. 不用考虑版本问题
  ```

- Go 语言应用

  ```python
  # 国内的很多大型的互联网公司,  腾讯, 百度, 京东, 知乎等
  # 国外 谷歌. facebook
  ```

- Go 应用领域

  ```python
  # 服务开发
  # 并发
  # 分布式
  # 微服务方向
  
  ```

- Go 语言项目

  ```python
  # Docker
  # K8s (kubernetes)
  # 区块链
  ```

- Go 语言发展前景

  ```python
  # 新兴语言 最牛逼! 不解释
  ```

## 环境搭建

```python
# go 开发包安装
# go ide 安装(gplang, vscode, sublime text)

# 安装完成后 go version  查看版本


# go env 查看go环境变量
GO111MODULE=""    # 当此配置 为空或者off 则代表没有开启 go mode 模式, 用的是gopath 模式
GOROOT="/usr/local/Cellar/go/1.14.2_1/libexec"  #  go 开发安装路径
GOPATH="/Users/zhang/go"  #  代码的存放路径

# go mode 模式: 代码可以存放在任意路径
```

## Hello World

```go
// 单行注释
/*
多行注释
*/

package main  // 表示声明包, 每一个go代码第一行都必须写这个

import "fmt"  // 导入fmt包  类比  python中 import os.path

func main() {  // 声明 main 函数    函数体 放在 {}之中.
	fmt.Println("Hello, World") // Println 是输入至控制台, 类比python中的print
}

// 程序的执行入口是 main包下的 main函数
// 编译型语言都有入口, 对比 python中  一个py文件就是一个 main 函数.
// 一个项目只能有一个 main函数
```

### 常用命令

- build 

  ```python
  go build 文件   # 生成本机环境的可执行文件
  ```

- fmt 

  ```python
  go fmt 文件  # 代码格式化
  ```

- install

  ```python
  # 编译并安装包和依赖
  ```

- run

  ```python
  go run 文件  # 编译并运行go程序
  ```


### 规范

```go
	1 变量：go语言中变量区分大小写，建议用驼峰
		var Name="lqz"
		var name="lqz"
		fmt.Println(Name)
		fmt.Println(name)
	2 文件名：建议用下划线
	3 大写字母开头，表示导出字段（外部包可以使用）

	4 先导入第三方包，内置包，自己写的包
	5 不像python中缩进代表同一段代码块
			var a =10
		fmt.Println(a)
			   fmt.Println("xxx")
```





[toc]







---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/go-01%E8%AF%AD%E8%A8%80%E7%AE%80%E4%BB%8B%E5%8F%8A%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/  

