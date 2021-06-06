---
title: 使用Hexo搭建一个博客[Mac版]
tags: hexo
categories: 
- program
---

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
<!--more-->
### 一.搭建博客
1. 开发环境
   * [Node.js](https://nodejs.org/en/)   下载对应平台系统安装包按提示安装，在命令行输入下边代码，提示版本号即为安装成功。
    `node -v或 node --version`
   * [Git](https://git-scm.com/download)   git是一个版本管理工具，安装后就可以使用git命令管理项目了。
2. 安装Hexo
  命令行运行以下代码
  `npm install -g hexo-cli`
3. 初始化博客框架```js
  hexo init [blog_name]         //在要创建博客的路径下,blog_name为你的博客名称
  cd [blog_name]                //移动到博客项目
  npm install                   //安装依赖
  hexo server                   //启动博客```
  浏览器打开[http://localhost:4000/](http://localhost:4000/)看到你的博客主页了。

### 二.修改主题
1. 博客搭好后可以按照喜好配置主题，个人比较喜欢以下几款：
  * [icalm](git@github.com:nameoverflow/hexo-theme-icalm.git) 
  * [paperbox](git@github.com:sun11hexo-theme-paperbox.git) 
  * [archer](git@github.com:fi3ework/hexo-theme-archer.git)
  * 更多主题见[官网主题](https://hexo.io/themes/)
2. 使用方法
  * 下载主题并放到项目themes文件夹下
  * 找到项目根目录 _config.yml文件
  * 找到theme属性，修改为主题对应的文件夹名称即可

### 三.部署到github
**默认已有github账号，并添加过ssh，如果没有，自行谷歌**
1. 新建仓库
  仓库名为：<Github账号名称>.github.io
2. 配置项目
  在项目根目录的_config.yml文件最后添加如下代码：```js
  deploy:
  type: git 
  repo: git@github.com:<Github账号名称>/<Github账号名称>.github.io
  branch: master```
3. 部署项目
  * 安装hexo-deployer-git插件，在命令行运行以下命令：
  `npm install hexo-deployer-git --save`
  * 推送到GithubPages。
  在命令行（即Git Bash）依次输入以下命令， 返回INFO Deploy done: git即成功推送：```js
  hexo g
  hexo d```
4. 等待1分钟左右，浏览器访问网址： https://<Github账号名称>.github.io

### 四.多终端同步

### 五.可视化编辑文章
