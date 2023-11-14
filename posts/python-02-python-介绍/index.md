# python介绍


## Python

Python是一种跨平台的[计算机程序设计语言]。 是一个高层次的结合了解释性、编译性、互动性和面向对象的脚本语言。最初被设计用于编写自动化脚本(shell)，随着版本的不断更新和语言新功能的添加，越多被用于独立的、大型项目的开发。

### Python简介及应用领域

Python是一种解释型脚本语言，可以应用于以下领域:

- Web 和 Internet开发
- 科学计算和统计
- 人工智能
- 桌面界面开发
- 软件开发
- 后端开发
- 网络爬虫

### 发展历程

自从20世纪90年代初Python语言诞生至今，它已被逐渐广泛应用于系统管理任务的处理和[Web](https://baike.baidu.com/item/Web/150564)编程。

Python的创始人为荷兰人吉多·范罗苏姆 [4] （Guido van Rossum）。1989年圣诞节期间，在[阿姆斯特丹](https://baike.baidu.com/item/阿姆斯特丹/2259975)，Guido为了打发[圣诞节](https://baike.baidu.com/item/圣诞节/127881)的无趣，决心开发一个新的脚本解释程序，作为ABC 语言的一种继承。之所以选中Python（大蟒蛇的意思）作为该编程语言的名字，是取自英国20世纪70年代首播的电视喜剧《蒙提.派森的飞行马戏团》（Monty Python's Flying Circus）。

ABC是由Guido参加设计的一种[教学](https://baike.baidu.com/item/教学)语言。就Guido本人看来，ABC 这种语言非常优美和强大，是专门为非专业程序员设计的。但是ABC语言并没有成功，究其原因，Guido 认为是其非开放造成的。Guido 决心在Python 中避免这一错误。同时，他还想实现在ABC 中闪现过但未曾实现的东西。

就这样，Python在Guido手中诞生了。可以说，Python是从ABC发展起来，主要受到了Modula-3（另一种相当优美且强大的语言，为小型团体所设计的）的影响。并且结合了[Unix shell](https://baike.baidu.com/item/Unix shell)和C的习惯。

Python [5] 已经成为最受欢迎的[程序设计语言](https://baike.baidu.com/item/程序设计语言/2317999)之一。自从2004年以后，python的使用率呈线性增长。Python 2于2000年10月16日发布，稳定版本是Python 2.7。Python 3于2008年12月3日发布，不完全兼容Python 2。 [4] 2011年1月，它被[TIOBE](https://baike.baidu.com/item/TIOBE)编程语言[排行榜](https://baike.baidu.com/item/排行榜/4895)评为2010年度语言。 [6] 

由于Python语言的[简洁](https://baike.baidu.com/item/简洁)性、易读性以及可扩展性，在国外用Python做科学计算的研究机构日益增多，一些知名大学已经采用Python来教授程序设计[课程](https://baike.baidu.com/item/课程)。例如[卡耐基梅隆大学](https://baike.baidu.com/item/卡耐基梅隆大学)的编程基础、麻省理工学院的计算机科学及编程导论就使用Python语言讲授。众多开源的科学计算软件包都提供了Python的调用[接口](https://baike.baidu.com/item/接口)，例如著名的计算机视觉库[OpenCV](https://baike.baidu.com/item/OpenCV)、三维可视化库VTK、医学图像处理库ITK。而Python专用的科学计算扩

[![标识](https://bkimg.cdn.bcebos.com/pic/faedab64034f78f092033e1079310a55b2191ccc?x-bce-process=image/resize,m_lfit,w_220,h_220,limit_1)](https://baike.baidu.com/pic/Python/407313/0/faedab64034f78f092033e1079310a55b2191ccc?fr=lemma&ct=single)标识

展库就更多了，例如如下3个十分经典的科学计算扩展库：NumPy、SciPy和matplotlib，它们分别为Python提供了快速数组处理、数值运算以及绘图功能。因此Python语言及其众多的扩展库所构成的开发环境十分适合[工程](https://baike.baidu.com/item/工程)技术、科研人员处理实验数据、制作图表，甚至开发科学计算[应用程序](https://baike.baidu.com/item/应用程序)。2018年3月，该语言作者在邮件列表上宣布Python 2.7将于2020年1月1日终止支持。用户如果想要在这个日期之后继续得到与Python 2.7有关的支持，则需要付费给商业供应商。

### 风格

Python在设计上坚持了清晰划一的风格，这使得Python成为一门易读、易维护，并且被大量用户所欢迎的、用途广泛的[语言](https://baike.baidu.com/item/语言/2291095)。

设计者开发时总的指导思想是，对于一个特定的问题，只要有一种最好的方法来解决就好了。这在由Tim Peters写的Python格言（称为The Zen of Python）里面表述为：There should be one-- and preferably only one --obvious way to do it. 这正好和[Perl](https://baike.baidu.com/item/Perl)语言（另一种功能类似的高级[动态语言](https://baike.baidu.com/item/动态语言)）的中心思想TMTOWTDI（There's More Than One Way To Do It）完全相反。

Python的作者有意的设计限制性很强的语法，使得不好的编程习惯（例如[if语句](https://baike.baidu.com/item/if语句)的下一行不向右缩进）都不能通过编译。其中很重要的一项就是Python的[缩进](https://baike.baidu.com/item/缩进/7337492)规则。

一个和其他大多数语言（如C）的区别就是，一个模块的界限，完全是由每行的首字符在这一行的位置来决定的（而C语言是用一对花括号[{}](https://baike.baidu.com/item/{})来明确的定出模块的边界的，与字符的位置毫无关系）。这一点曾经引起过争议。因为自从C这类的语言诞生后，语言的语法含义与字符的排列方式分离开来，曾经被认为是一种程序语言的进步。不过不可否认的是，通过强制程序员们[缩进](https://baike.baidu.com/item/缩进)（包括if，for和函数定义等所有需要使用模块的地方），Python确实使得程序更加清晰和美观。

### 第一个python程序

要开始写代码了, 好高兴呀...QAQ 

#### 运行python程序的两种方式

- 交互式运行
  ![](https://gitee.com/qilitang/Img/raw/master/img/1825659-20191009213335330-105384908.png)

- 命令行运行
  - 1 # 打开一个文本编辑工具，写入下述代码，并保存文件，此处文件的路径为D:\test.py。强调：python解释器执行程序是解释执行，解释的根本就是打开文件读内容，因此文件的后缀名没有硬性限制，但通常定义为.py结尾
    print('hello world')
    -2、打开cmd，运行命令，如下图 
    ![](https://gitee.com/qilitang/Img/raw/master/img/1825659-20191009213412510-667072455.png)

#### 注释

- "#"在行尾巧两个空格 然后输入# 再加一个空格 写注释内容. 

```
print("我最牛逼")  # 打印我最牛逼
```

- ''' 注释内容'''/ """ 注释内容"""

```
'''打印我最牛逼'''
print("我最牛逼")
```

pycharm 的注释的快捷键是 ctrl+?

ps: """也可以是字符串"""

```
print('''
我最牛逼!
我是最牛逼的!
我在自我暗示.
''')
```

两者区别是 """""" 可以换行.
代码的注释是在代码行首加上#和空格, 即这段代码被注释掉 

```
# print("注释掉我最牛逼")
# 上一行代码不执行
```



---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/python-02-python-%E4%BB%8B%E7%BB%8D/  

