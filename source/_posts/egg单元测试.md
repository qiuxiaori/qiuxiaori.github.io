---
title: egg单元测试
tags: egg
categories: 编程技术
---

> 为什么要单元测试
它能带给我们很多保障：代码质量持续有保障，重构正确性保障，增强自信心，自动化运行

<!--more-->

### 一.egg单元测试框架及约定
1. 测试框架
egg使用 [Mocha](https://mochajs.org/)，功能非常丰富，支持运行在 Node.js 和浏览器中， 对异步测试支持非常友好。

2. 断言库
egg使用原始的 assert 作为默认的断言库。它的优点是：没有 API 就是最好的 API，不需要任何记忆，只需 assert 即可以及强大的错误信息反馈。

3. 目录结构
egg约定 test 目录为存放所有测试脚本的目录，测试所使用到的 fixtures 和相关辅助脚本都应该放在此目录下。测试脚本文件统一按 ${filename}.test.js 命名，必须以 .test.js 作为文件后缀。```
test
├── controller
│   └── home.test.js
├── hello.test.js
└── service
    └── user.test.js
```


4. 测试方式
只需要在 package.json 上配置好 scripts.test 即可。```
{
  "scripts": {
    "test": "egg-bin test"
  }
}
```
* 然后就可以按标准的 npm test 来运行测试了。
`npm test`

### 二.对象获取
1. mock
egg单独为框架抽取了一个测试 mock 辅助模块：egg-mock， 有了它就可以非常快速地编写一个 app 的单元测试，并且还能快速创建一个 ctx 来测试它的属性、方法和 Service 等。

2. app

* 测试运行之前首先要创建应用的一个 app 实例， 通过它来访问需要被测试的 Controller、Middleware、Service 等应用层代码。通过 egg-mock，结合 Mocha 的 before 钩子就可以便捷地创建出一个 app 实例。```js
// test/controller/home.test.js
const assert = require('assert');
const mock = require('egg-mock');
describe('test/controller/home.test.js', () => {
  let app;
  before(() => {
    // 创建当前应用的 app 实例
    app = mock.app();
    // 等待 app 启动成功，才能执行测试用例
    return app.ready();
  });
});
```

* 每一个测试文件都需要这样创建一个 app 实例非常冗余，因此 egg-mock 提供了一个 bootstrap 文件，可以直接从它上面拿到我们所常用的实例：```js
// test/controller/home.test.js
const { app, mock, assert } = require('egg-mock/bootstrap');
describe('test/controller/home.test.js', () => {
  // test cases
});
```

3. ctx
我们除了 app，还需要一种方式便捷地拿到 ctx，方便我们进行 Extend、Service、Helper 等测试。 我们已经通过上面的方式拿到了一个 app，结合 egg-mock 提供的 app.mockContext(options) 方法来快速创建一个 ctx 实例。```js
it('should get a ctx', () => {
  const ctx = app.mockContext();
  assert(ctx.method === 'GET');
  assert(ctx.url === '/');
});
```

### 三.Controller测试
1. app.mockContext()```js
// 创建一个默认的ctx对象
ctx = app.mockContext();
// 模拟ctx.name
ctx = app.mockContext({ name: 'ceshi' });
// 设置ctx的传输头
ctx = app.mockContext({ headers: {  'Content-Type' : 'multipart/form-data' } });
```

2. app.mockCsrf()
模拟取 CSRF token 的过程。 这样在使用 SuperTest 请求 app 就会自动通过 CSRF 校验。

3. app.mockSession```js
// 模拟session
app.mockSession({
        foo: 'bar',
        uid: 123,
      });
```

4. ctx.httpRequest()
* 该方法能发起一个真实的请求，以检测Controller参数的完整性。app.httpRequest() 是 [egg-mock](https://github.com/eggjs/egg-mock) 封装的 [SuperTest](https://github.com/visionmedia/supertest)。```js
await app.httpRequest()
        .get('/')
        .expect(200) // 期望返回 status 200
```

* 常用级联函数，按调用顺序介绍```js
// 发送get请求，post，put，等请求同理
get('/')
// 传递HTTP用户名和密码
auth('username', 'password')
// 设置传输头,Cookie或主体
set('Accept', 'application/json')
// 类型，表单
type('form')
// 发送的参数，多个参数用逗号隔开
send({ key: 'value' })
## 发送文件
// 使用attach发送文件时必须搭配field方法发送参数，不能用send，多个参数时级联多个field
field('name', 'my awesome avatar')
//发送的文件
attach('avatar', 'test/fixtures/avatar.jpg')
// 期望值
expect(200) // 期望返回 status 200
expect('hello world'); // 期望 body 是 hello world
expect(status[, fn])
expect(body[, fn])
expect(field, value[, fn])
```




