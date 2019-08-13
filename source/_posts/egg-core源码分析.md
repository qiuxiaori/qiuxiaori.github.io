---
title: egg-core源码分析
tags: egg
categories: 编程技术
---

<!--more-->

### 一.egg-core 的结构

Egg.js 的大部分核心代码实现都在 egg-core 库 中，egg-core 主要 export 四个对象:

1. EggCore 类：继承于 Koa ，做一些初始化工作， EggCore 中最主要的一个属性是 loader ，也就是 egg-core 的导出的第二个类 EggLoader 的实例
2. EggLoader 类：整个框架目录结构（controller，service，middleware，extend，router.js）的加载和初始化工作都在该类中实现的，主要提供了几个 load 函数(loadPlugin，loadConfig，loadMiddleware，loadService，loadController，loadRouter 等)，这些函数会根据指定目录结构下文件输出形式不同进行适配，最终挂载输出内容。
3. BaseContextClass 类：这个类主要是为了我们在使用框架开发时，在 controller 和 service 作为基类使用，只有继承了该类，我们才可以通过 this.ctx 获取到当前请求的上下文对象
4. utils 对象：提供几个主要的函数，包括转换成中间件函数 middleware ，根据不同类型文件获取文件导出内容函数 loadFile 等

```
  // egg-core 源码 -> 导出的数据结构
  const EggCore = require('./lib/egg');
  const EggLoader = require('./lib/loader/egg_loader');
  const BaseContextClass = require('./lib/utils/base_context_class');
  const utils = require('./lib/utils');

  module.exports = {
    EggCore,
    EggLoader,
    BaseContextClass,
    utils,
  };
```
