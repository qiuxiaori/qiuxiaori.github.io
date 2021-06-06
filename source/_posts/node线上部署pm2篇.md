---
title: node线上部署pm2篇
tags: pm2
categories: program
---

> [PM2](http://pm2.keymetrics.io/docs/usage/quick-start/#42-starts)是守护进程管理器，可帮助您管理和保持应用程序在线。PM2入门很简单，它是一个简单直观的CLI，可通过NPM安装。

<!--more-->

### 一.安装使用

1. 安装
可以通过npm或yarn方式安装最新版pm2```js
npm install pm2@latest -g
# or
yarn global add pm2
```

2. 启动项目
`pm2 start [app.js]     # 项目入口文件`

* 可以传给cli一些配置来指定启动项```
--name <app_name>   // 指定启动的服务名称
--watch             // 监控项目变化，文件变更时自动重启，一般线上环境不监控
--max-memory-restart <200MB>  // 内存达到后重启服务
--log <log_path>    // 指定日志路径
```

3. 配置文件启动
* 在项目根目录添加启动配置文件pm2.config.js```js
'use strict';
module.exports = {
  apps: [{
    name: 'server', // 项目名
    script: './server.js', // 执行文件
    cwd: './', // 根目录
    args: 'one two', // 传递给脚本的参数
    interpreter: '', // 指定的脚本解释器
    interpreter_args: '', // 传递给解释器的参数      //
    exec_mode: 'cluster_mode', // 应用启动模式，支持fork和cluster模式
    instances: 1, // 应用启动实例个数，仅在cluster模式有效 默认为fork；或者 max
    max_memory_restart: '1G', // 最大内存限制数，超出自动重启
    error_file: `./logs/pm2/new Date()/app-err.log`, // 错误日志文件
    out_file: `./logs/pm2/new Date()/app-out.log`, // 正常日志文件
    merge_logs: true, // 设置追加日志而不是新建日志
    log_date_format: 'YYYY-MM-DD HH:mm:ss', // 指定日志文件的时间格式
    min_uptime: 1000, // 应用运行少于时间被认为是异常启动
    max_restarts: 30, // 最大异常重启次数，即小于min_uptime运行时间重启次数；
    autorestart: true, // 默认为true, 发生异常的情况下自动重启
    cron_restart: '', // crontab时间格式重启应用，目前只支持cluster模式;
    restart_delay: 60, // 异常重启情况下，延时重启时间      //
    watch: true, // 是否监听文件变动然后重启      //
    ignore_watch: [ //   // 不用监听的文件      //
      'node_modules', //   'logs',      //
      'news', //
      'run', //
      'test', //
      'typings', //
      'pm2.config', //
    ],
    env: {
      NODE_ENV: 'development',
    },
    env_production: {
      NODE_ENV: 'production',
    },
  }],
};
```
* 启动项目
`pm2 start pm2.config.js`

### 二.常用命令

1. 监控流程```
pm2 restart [app_name|id]      // 重启进程
pm2 reload [app_name|id]       // 0秒停机重载进程 (用于 NETWORKED 进程)
pm2 stop [app_name|id]         // 停止进程
pm2 delete [app_name|id]       // 杀死进程
pm2 show [app_name|id]         // 查看进程详情
// 如果不是指定项目名而是通过 all 可以操作所有进程
```

2. 获取进程列表
列出PM2管理的所有的状态：
`pm2 [list|ls|status]`

3. 显示日志
* 要实时显示日志：
`pm2 logs`
* 显示旧日志：
`pm2 logs --lines 200`

4. 基于终端的仪表板
监控每个node进程的cpu和内存使用情况
`pm2 monit`

5. 监控运行这些进程的机器
监控所有被PM2管理的进程,同时监控运行这些进程的机器的状态，它会自动在指定端口9615启动一个服务，我们可以在浏览器访问http://localhost:9615 查看详细信息。
`pm2 web`

6. 自动负载均衡```js
// 开启指定数目的进程/机器cpu核数对应的数目的进程运行
pm2 start server.js -i [number|max]
// 配置文件里对应的
"instance": [number|max]
```

### 三.监控可视化
1. 安装pm2-web
`npm install -g pm2-web`
默认pm2-web会自动启动一个端口8080
2. 配置文件启动
* 创建配置文件
`pm2-web --config pm2-web-config.json`
* 指定端口```js
// pm2-web-config.json
{
  "www": {
      "host": "localhost",
      "address": "0.0.0.0",
      "port": 6688
  }                         
}
```

### 四.在egg框架中使用pm2
1. 在项目根目录定义启动文件：```js
// server.js
const egg = require('egg');
const workers = Number(process.argv[2] || require('os').cpus().length);
egg.startCluster({
  workers,
  baseDir: __dirname,
});
```
2. 启动
`pm2 start server.js`



















