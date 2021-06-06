---
title: js模块
tags: js
categories: program
---

> 原生javascript的一些对象的常用方法备份。
<!--more-->

### 一.Date对象
1. 在JavaScript中，Date对象用来表示日期和时间,能够获取系统当前时间。```js
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳
```

2. 创建一个指定日期和时间的Date对象
当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值，如果要创建一个指定日期和时间的Date对象。
* 可以传入指定的年月日构造一个Date对象```js
var d = new Date(2015, 5, 19, 20, 15, 30, 123);
d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)
```
* 或者创建一个指定日期和时间的方法是解析一个符合ISO 8601格式的字符串:```js
var d = Date.parse('2015-06-24T19:49:22.875+08:00');
d; // 1435146562875
// 它返回的不是Date对象，而是一个时间戳。不过有时间戳就可以很容易地把它转换为一个Date
var d = new Date(1435146562875);
d; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
d.getMonth(); // 5
```