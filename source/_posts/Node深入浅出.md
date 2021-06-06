---
title: Node深入浅出
categories: program
tags: node
---



### 一. 什么是Node

##### Node.js是一个运行环境

**Node.js使用c++语言编写而成，是一个 javascript 语言运行环境。Node.js 采用了 Chrome 浏览器的 V8 引擎，性能很好，同时提供了很多系统级的 API。**浏览器端的 javascript 代码在运行时会收得到各种安全性的限制，对客户系统的操作有限，而 Node.js 是一个全面的后台运行时，为javascript 提供了其他语言能够实现的许多功能。

##### Node.js的特点

* 采用事件驱动,异步编程，为网络服务而设计
* 以单进程，单线程模式运行，**事件驱动机制**是Node.js通过内部单线程高效率的维护**事件循环队列**来实现的，没有多线程的资源占用和上下文切换开销。
* 支持的语言是 javascript。javascript的匿名函数和闭包特性非常适合事件驱动，异步编程。

---

### 三. 模块机制

##### CommonJS  规范

CommonJS规范的出现，目标是为了构建 Javascript 在包括Web服务器，桌面，命令行工具以及浏览器方面的生态系统。

CommonJS指定了解决这些问题的一些规范，而Node.js就是这些规范的一种实现。

**Node.js自身实现了 require 方法作为其引入模块的方法，同时 NPM 也基于 CommonJS 定义的包的规范，实现了依赖管理和模块自动安装等功能。**



##### 简单模块定义和使用

* 导出：如果模块返回的函数或变量不止一个，那它可以通过设定exports对象的属性来指明它们。但如果模块只返回一个函数或变量，则可以设定在module.exports属性。最终在程序里导出的是module.exports，exports只是对module.exports的一个全局引用，最初被定义为一个可以添加属性的空对象。所以exports.myFunc只是 module.exports.myFunc的简写。
* 引入：require('./fileName')。如果模块是 个目录，Node通常会在这个目录下找一个叫index.js的文件作为模块的入口。

##### 模块分类

* 原生模块：在源代码编译的时候编译进了二进制执行文件，加载的速度最快。原生模块都被定义在lib这个目录下。
* 文件模块：动态加载，加载速度比原生模块慢。
  * .js：通过fs模块同步读取js文件并编译执行
  * .node：通过c/c++进行编写的Addon，通过dlopen方法进行加载
  * .json：读取文件，调用JSON.parse解析加载

node对模块都进行了缓存，在第二次require时，不会有重复开销的。

##### 文件模块的加载

加载文件模块的工作主要由原生模块module完成和实现，该模块在启动时已经被加载，进程直接调用到 runMain 静态方法。

``` js
Module.runMain = function () {
  Module._load(process.argv[1], null, ture)
}
```

_load 静态方法在分析文件名后执行实例化

```js
var module = new Module(id, parent)
```

并根据文件路径缓存当前模块对象，该模块实例对象则根据文件名加载

```js
module.load(filename)
```

require方法实际上调用的就是load方法，load方法在载入编译缓存了module后，返回module的exports对象。

##### require方法中的文件查找策略

文件模块缓存->原生模块->文件加载(逐层查找module path)->npm包

----

### 四.事件机制

基于v8引擎实现的事件驱动IO。



---

### 五. 异步I/O实现

##### 异步I/O

在操作系统中，程序运行的空间分为内核空间和用户空间，我们提到的异步I/O，实质是用户空间的程序不用依赖内核空间中的I/O操作实际完成，即可进行后续任务。时间开销为m+n的两个任务会减小为max(m,n)

##### 必要性

避免了多线程的资源占用和上下文切换的开销。

##### 理想的异步I/O模型

理想的模型是在应用程序发起异步调用而不需要进行轮训，进而处理下一个任务，只需要在I/O完成后通过信号或事回调将数据传递给应用程序。

* linux：libeio
* windows：IOCP

node提供了libuv来作为抽象封装层，平台判断在一层完成。node在编译期间会判断平台条件，选择性编译unix目录或事win目录下的源文件到目标程序中。

##### Node.js的异步I/O模型

