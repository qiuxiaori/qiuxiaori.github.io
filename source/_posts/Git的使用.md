---
title: Git的使用
tags: 教程
categories: 编程技术
---

> git 是一个分散式版本管理工具，最初由林纳斯·托瓦兹创作，于 2005 年以 GPL 发布。最初目的是为更好地管理 Linux 内核开发而设计。

<!--more-->

### 一.安装 Git

官网下载 git mac 安装包，按照提示安装,终端中输入 git 出现如图提示信息即为安装成功。

### 二.注册账号

如果没有 github 账号到官网[https://github.com/](https://github.com/)上注册账号并通过邮箱验证。

### 三.添加 SSH 密匙

1. 获取 ssh key

```
//设置用户名和电子邮箱
git config --global user.name "xxxxx"
git config --global user.email "xxxxx@qq.com"

//通过终端命令创建ssh密钥
ssh-keygen -t rsa -C "xxxxx@qq.com"

//回车后显示，默认回车即可
Last login: Sat Jan  6 14:12:16 on ttys000
WMBdeMacBook-Pro:~ WENBO$ ssh-keygen -t rsa -C "1050794513@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/WENBO/.ssh/id_rsa):
/Users/WENBO/.ssh/id_rsa already exists.
Overwrite (y/n)? n
WMBdeMacBook-Pro:~ WENBO$

//生成ssh key后在用户目录下打开.ssh文件夹，id_rsa.pub文件中的内容就是我们要的ssh key
//如果在目录中看不到.ssh文件夹，可以通过命令行操作文件
open .ssh/id_rsa.pub
```

2. 在设置中添加得到的 SSH 密匙，如图。

### 四.创建远程仓库

点击右上角加号新建仓库

### 五.创建本地仓库

5.1 初始化仓库

```
//cd到项目所在目录
//把项目目录初始化为一个可管理的git仓库
git init
```

5.2 添加要管理的文件

```
git add example.txt

//通过add . 命令可以添加目录中所有文件
git add .
```

5.3 提交添加的文件到版本库中

```
git commit -m "对本次提交做的描述，必选"
```

### 六. 本地仓库内容推送到远程仓库

6.1  ​​ 把本地仓库关联到远程仓库

```
//在终端输入（依然是项目目录）,后边是你的远程仓库的git地址
//origin相当于对远程仓库的称呼
git remote add origin git@github.com:zmyCris/fisdhad-f.git
```


6.2 推送

使用 push 命令推送  
  由于远程库是空的，我们第一次推送 master 分支时，加上了-u 参数，Git 不但会把本地的 master 分支内容推送的远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来，在以后的推送或者拉取时就可以简化命令，不用加上-u 参数。

```
git push -u origin master
```

### 七.回滚

当我们想要撤销提交回滚到之前的某次提交时就要用到 reset 命令了，reset 命令能够把仓库回滚到指定的某次提交时的状态，也就是说我们如果要撤销 a 提交，就要选择它的前一次提交。

1. 查看提交的 id
   使用下面的命令查看 git 仓库日志，拿到 commit 后边的 id，就是我们等会要操作的对象。```
   git log
   // ctrl+z 退出日志模式

````
![gitlog](/images/git01.jpeg)

2. 回滚提交
* 回滚提交并将修改置为已commit
`git reset --soft [c89936abefaf3a23a0ffc541f75db00f19cd6e8b]`

* 回滚提交将修改置为未commit
`git reset --mixed [c89936abefaf3a23a0ffc541f75db00f19cd6e8b]`

* 回滚并删除修改的内容，谨慎操作
`git reset --hard [c89936abefaf3a23a0ffc541f75db00f19cd6e8b]`

3. 同步远程仓库
本地回滚后远程仓库还是回滚前的状态，这时我们执行以下命令要强制远程仓库同步本地仓库。
`git push -f [name]`

### 八.删除分支
分支的删除分为删除本地分支和删除远程分支
1. 删除本地分支
`git branch -D [name]`
2. 删除远程分支
`git push origin --delete [name]`

### 九.合并分支
假如我们在master分支外创建了一个dev分支，工作完成后想要把dev分支的内容同步到master分支，就要用到merge命令。```js
// 切换到master分支
git checkout master
// 合并dev分支
git merge dev
````
