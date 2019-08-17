---
title: egg框架中实现上传文件
tags: egg
categories: 编程技术
---
> 接了个公司的需求，根据上传文件自动生成数据条目，egg内置了multipart模块，之前没有使用过，就这次需求简要总结下使用方式和踩的坑。具体见egg-multipart的[官方教程](https://github.com/eggjs/egg-multipart)。
<!--more-->
### 一.egg-multipart
1. 配置使用
```
//安装egg-multipart
npm i egg-multipart
// config/config.default.js：
 exports.multipart = {
    mode: 'stream',                 // 上传文件模式,value:stream/file
    fileSize: '50mb',               // fileSize默认是10mb。如果上传大文件，应指定此配置
    whitelist: ['.png'],            // 会覆盖默认文件白名单，仅允许.png格式文件
    fileExtensions: [ '.xlsx' ],    // 可选，定制其他文件拓展名
  };
  // 默认内置白名单
  whitelist = [
  // images 支持
  '.jpg', '.jpeg','.png', '.gif', '.bmp', '.wbmp', '.webp','.tif','.psd',
  // text
  '.svg','.js', '.jsx', '.json','.css', '.less','.html', '.htm','.xml',
  // tar
  '.zip','.gz', '.tgz', '.gzip',
  // video
  '.mp3','.mp4','.avi',
];
```
 白名单里没有的文件格式一定要添加到fileExtensions中，否则会报下面的错误。
  `nodejs.Error: Invalid filename`
2. file模式
> 如果您不知道Node.js Stream工作方式，也许您应该使用该file模式开始。用法与bodyParser非常相似。
```
ctx.request.body：获取所有多部分字段和值，除了file文件。
ctx.request.files：包含file来自multipart请求的所有内容，它是一个Array对象。
```
3. stream模式
> 如果您熟悉Node.js Stream的工作原理，那么您应该使用该stream模式。
```
ctx.getFileStream(): 获取文件流
ctx.getFileStream().fields: 获取所有多部分字段和值，它是一个Array对象。
ctx.getFileStream({ requireFile: false }): 文件非必须
```
4. 文件的常用属性
```
fieldname: 文件名
encoding: 文件的编码格式
mime: 文件的mime
filepath: 文件的路径
```
### 二.具体实现
1. 安装插件
```
npm i await-stream-ready    // 用于异步操作文件
npm i stream-wormhole       // 关闭文件流
```
2. 引入模块
```
// loadService.js
const fs = require('fs');     // node的fs文件模块
const path = require('path'); // node的path路径模块
// 刚刚安装的插件
const awaitWriteStream = require('await-stream-ready').write;
const sendToWormhole = require('stream-wormhole');
```
3. 上传文件
```
// loadService.js
async addStoryByFile() {
  const { ctx, app } = this;
  const stream = await this.ctx.getFileStream(); // egg中获取上传文件的方法
  const filename = stream.filename;
  const target = path.join('app/public/uploads', filename); 
  if(!fs.exitsSync(url)) { // 如果路径不存在，则创建
    fs.mkdir(url);
  }
  const writeStream = fs.createWriteStream(target); // 创建文件流
  try {
    await awaitWriteStream(stream.pipe(writeStream)); // 异步写入文件
  } catch (err) {
    await sendToWormhole(stream); // 如果失败，关闭文件流
    // 其他操作
    // return ...
  }
}
```
### 三.关于egg中文件上传的单元测试踩坑记
  egg的单元测试本身是很简单的，但是文件上传有几个小坑，因为multipart插件要求传输头的Content-Type是multipart/form-data类型的，在测试时尝试了很多方法都没法把mock的ctx设置传输头。翻了很多文档，service层的文件上传测试依然没找到解决方法，希望日后能找到把，目前只找到了controller层测试文件上传的方法，如下：
```
'use strict';
const { app, assert, mock } = require('egg-mock/bootstrap');   // 引入egg封装好的mock模块
const fs = require('fs');                                                           //引入文件模块
describe('test/app/service/story.test.js', () => {
  it('should add story', async () => {
    const ctx = await app.httpRequest()
      .post('/api/project/story/addStoryByFile')
      .field('workspace_id', '62604516')              // 上传文件时不能使用send方法，要使用field发送参数，多个参数时级联field
      .field('iteration_id', '116260451600100098')
      .attach('file', '/Users/zmy/Downloads/爱云校研发管理v1.0/data/document.js');    // 要上传的文件路径,必须在field后调用
    assert(ctx.body.code === 0);                 // 返回的参数在result的body 中
  });
});
```

参考文献：

1. [egg github上的test实例](https://github.com/eggjs/examples)
2. [egg 官网单元测试](https://eggjs.org/zh-cn/core/unittest.html)
3. [egg单元测试mock api](https://github.com/eggjs/egg-mock#api)












