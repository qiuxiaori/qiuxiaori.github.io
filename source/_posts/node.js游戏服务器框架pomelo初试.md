---
title: node.js游戏服务器框架pomelo初试
tags: pomelo
categories: program
---

> pomelo之所以简单易用、功能全面，并且具有高可扩展性、可伸缩性等特点，这与它的技术选型和方案设计是密不可分的。在研究大量游戏引擎设计思路基础上，结合以往游戏开发的经验，确定了pomelo框架的设计方案。

<!--more-->

### 一.初始化一个项目
1. 安装pomelo
   window操作系统下需要python（2.5 ——3.0）和C ++编译器支持，mac系统已经集成了python和c++，可以直接安装。命令行执行以下命令全局安装pomelo。
   ` npm install pomelo -g`

2. 创建项目
   在要创建项目的路径下执行：
   ` pomelo init ./project-name`

3. 安装依赖
   cd到项目文件中执行(window用户执行第二个命令)：
  `sh npm-install.sh/npm-install.bat`

4. 启动项目
  cd到game-server启动游戏服务器：
  `pomelo start`
  cd到web-server启动web服务器：
  `node app`
  都启动成功后使用浏览器（建议使用chrome）访问http://localhost:3001或http://127.0.0.1:3001，然后单击Test Game Server，提示游戏服务器正常表示服务器正在成功运行。

5. 停止项目
  game-server下执行即可停止游戏服务器：
  ` pomelo stop或pomelo kill       //建议使用第一个命令`
  web-server下ctrl+c即可停止web服务器







