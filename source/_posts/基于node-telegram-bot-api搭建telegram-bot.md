---
title: 基于node-telegram-bot-api搭建telegram-bot
tags: telegram
categories: 编程技术
---



​	_开发环境：_

* node
* git
* macOS
* 🪜
* telegram账号及客户端



<!----more----->

#### 一. 创建telegram-bot

1. tg客户端搜索@BotFather，输入```/start```

   ![](https://qxr-bucket.oss-cn-beijing.aliyuncs.com/images/%E6%88%AA%E5%B1%8F2021-05-25%2011.58.06.png)

2. 输入```/newbot``` ，创建机器人并输入机器人名称

   ![](https://qxr-bucket.oss-cn-beijing.aliyuncs.com/images/%E6%88%AA%E5%B1%8F2021-05-25%2011.58.22.png)

3. 输入机器人的用户名

   ![](https://qxr-bucket.oss-cn-beijing.aliyuncs.com/images/%E6%88%AA%E5%B1%8F2021-05-25%2011.58.47.png)

4. 完成，获得 ```HTTP API```

   ![](https://qxr-bucket.oss-cn-beijing.aliyuncs.com/images/%E6%88%AA%E5%B1%8F2021-05-25%2011.59.19.png)



#### 二. Spolling实现

1. 新建文件目录，安装依赖```npm i node-telegram-bot-api && socks5-https-client```

   * node-telegram-bot-api：实现了 telegram-api 的 node 包。
   * socks5-https-client：用于 node 请求代理国外域名。

2. 新建 app.js

   ```
   const TelegramBot = require('node-telegram-bot-api');
   const Agent = require('socks5-https-client/lib/Agent');
   const token = ''; // 你获取到的http api
   
   const bot = new TelegramBot(token, { polling: true,
       request: { // 设置代理
           agentClass: Agent,
           agentOptions: {
               socksPort: 1086,  // 代理端口，根据你的系统配置设置
               socksPassword: '' // 你的梯子的密码
           }
       } });
   
   // Matches "/echo [whatever]"
   bot.onText(/\/echo (.+)/, (msg, match) => {
       const chatId = msg.chat.id;
       const resp = match[1]; // the captured "whatever"
       bot.sendMessage(chatId, resp);
   });
   
   bot.on('message', (msg) => {
       const chatId = msg.chat.id;
       bot.sendMessage(chatId, 'sppling response');
   });
   
   ```

3. ```node app.js```启动即可

   ![](https://qxr-bucket.oss-cn-beijing.aliyuncs.com/images/%E6%88%AA%E5%B1%8F2021-05-25%2013.13.45.png)

#### 三.  webhook实现

> Todo