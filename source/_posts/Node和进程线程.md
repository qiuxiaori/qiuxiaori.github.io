---
title: Node和进程线程
tags: node
categories: program
---

公司分享会的内容，随手记

<!--more-->

### 一.进程和线程

1. 进程是资源调度的单位
Node内置方法：process.pid 获取当前进程id
查看时会得到v8引擎创建的多个线程
v8引擎会把js代码编译成高效的机器码，创建多个线程，主线程获取代码，编译成机器码，有处理垃圾回收的线程，负责优化的线程

2.  线程是运算调度的单位
进程包含线程，线程不能脱离进程，轻型实体不占用资源，只需要一些保证独立执行的资源，程序和数据，独立调度和分派的独立单元，可并发执行

* 线程的状态
新生
就绪
运行
死亡
阻塞

3. 二者的区别

|  | 进程 | 线程 |
| ------------ | ------------ | ------------ |
| 根本区别 | 操作系统资源分配的基本单位 | 任务调度和执行的基本单位 |
| 通信方面 | 通信IPC | 通信直接读取进程数据段通信 |
| 包含关系 | 进程包含线程 | 线程不能脱离进程 |
| 开销   | 拥有独立的代码块，切换开销大 |  |


### node的多进程
1. child_process```js
// 创建一个子进程
spawn(command,args,option)
```

* exec() 方法
child_process.exec 使用子进程执行命令，缓存子进程的输出，并将子进程的输出以回调函数参数的形式返回。exec() 方法返回最大的缓冲区，并等待进程结束，一次性返回缓冲区的内容。```js
child_process.exec(command[, options], callback)
// 参数说明
command： 字符串， 将要运行的命令，参数使用空格隔开
options ：对象，可以是：
cwd ，字符串，子进程的当前工作目录
env，对象 环境变量键值对
encoding ，字符串，字符编码（默认： 'utf8'）
shell ，字符串，将要执行命令的 Shell（默认: 在 UNIX 中为/bin/sh， 在 Windows 中为cmd.exe， Shell 应当能识别 -c开关在 UNIX 中，或 /s /c 在 Windows 中。 在Windows 中，命令行解析应当能兼容cmd.exe）
timeout，数字，超时时间（默认： 0）
maxBuffer，数字， 在 stdout 或 stderr 中允许存在的最大缓冲（二进制），如果超出那么子进程将会被杀死 （默认: 200*1024）
killSignal ，字符串，结束信号（默认：'SIGTERM'）
uid，数字，设置用户进程的 ID
gid，数字，设置进程组的 ID
callback ：回调函数，包含三个参数error, stdout 和 stderr。
```

* spawn() 方法
child_process.spawn 使用指定的命令行参数创建新进程.```js
child_process.spawn(command[, args][, options])
// 参数说明
command： 将要运行的命令
args： Array 字符串参数数组
options Object
cwd String 子进程的当前工作目录
env Object 环境变量键值对
stdio Array|String 子进程的 stdio 配置
detached Boolean 这个子进程将会变成进程组的领导
uid Number 设置用户进程的 ID
gid Number 设置进程组的 ID
spawn() 方法返回流 (stdout &amp; stderr)，在进程返回大量数据时使用。进程一旦开始执行时 spawn() 就开始接收响应。
```
fork 方法
child_process.fork 是 spawn() 方法的特殊形式，用于创建进程
`child_process.fork(modulePath[, args][, options])`
参数
参数说明如下：
modulePath： String，将要在子进程中运行的模块
args： Array 字符串参数数组
options：Object
cwd String 子进程的当前工作目录
env Object 环境变量键值对
execPath String 创建子进程的可执行文件
execArgv Array 子进程的可执行文件的字符串参数数组（默认： process.execArgv）
silent Boolean 如果为true，子进程的stdin，stdout和stderr将会被关联至父进程，否则，它们将会从父进程中继承。（默认为：false）
uid Number 设置用户进程的 ID
gid Number 设置进程组的 ID
返回的对象除了拥有ChildProcess实例的所有方法，还有一个内建的通信信道。