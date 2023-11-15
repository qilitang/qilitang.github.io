# nodejs 安装


## 一 nodejs介绍

Node.js 就是运行在服务端的 JavaScript。

Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。

Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

为什么要安装Node.js呢，下面用到的Grunt 工具是基于Node.js 使用的

下载地址：[https://nodejs.org/en/download/releases/](https://nodejs.org/en/download/releases/)

选择版本下载， 一直下一步确定即可，安装后进入命令行中 输入 :

```python
node -v 
# 显示版本号即安装成功
```

## 二 查看原来的镜像地址

**npm（node package manager）**：nodejs的包管理器，用于node插件管理（包括安装、卸载、管理依赖等）

```python
npm get registry
# 输出：https://registry.npmjs.org/
```

## 三 npm切换阿里源

```python
#切换阿里源
npm config set registry https://registry.npm.taobao.org/
#查看是否成功
npm config get registry
#或者
npm get registry
#可以看到输出
#https://registry.npm.taobao.org/
```

## 四 安装cmpm

**cnpm**:因为npm安装插件是从国外服务器下载，受网络的影响比较大，可能会出现异常，如果npm的服务器在中国就好了，所以我们乐于分享的淘宝团队干了这事。来自官网：“这是一个完整

npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。”

```python
npm install -g cnpm --registry=https://registry.npm.taobao.org
#查看是否安装成功
cnpm -v
#成功后可以使用cnpm代替npm命令
```

## 五 改变原有的环境变量

1、首先配置npm的全局模块的存放路径、cache的路径

```
npm config set prefix "路径"
npm config set cache "路径"
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/03-%E5%AE%89%E8%A3%85nodejs/  

