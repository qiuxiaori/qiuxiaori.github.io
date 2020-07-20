---
title: Hexo博客杂项
author: 秋小日
tags:
  - hexo
categories:
  - hexo
---

> 本文涉及到的知识点包: 博客一键发布，搜索引擎引擎收录及优化，github和coding站点部署等。

<!-- more -->

## 一. 一键发布

hexo 框架本身是基于node的，作为一名优秀的node工程师，当然可以添加一行命令，快速发布文章咯。
* 项目根目录 -> package.json 文件 -> scripts 参数添加以下命令:
``` js
  "build": "git add . && git commit -m 'update' && git push origin hexo -f && hexo clean && hexo d -g"
```
* hexo 是我编写博客的分支，站点使用的是master分支，只要执行 `npm run build` 就能一键推送并部署到master分支

## 二. 搜索引擎收录


## 三. SEO优化

## 四. 国内coding部署