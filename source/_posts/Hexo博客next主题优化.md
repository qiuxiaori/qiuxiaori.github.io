---
title: Hexo博客next主题优化
author: 秋小日
tags:
  - hexo
categories:
  - 编程技术
---

> 搭建好 hexo 博客后陆陆续续试用了几个主题，但是功能都不是很强大，最后我还是选择了集成服务较多，生态较好的[Next](https://theme-next.iissnan.com/)主题。

<!--more-->

### 主题安装
1. 下载方式
   克隆项目到项目 themes 目录下
   `git clone git@github.com:iissnan/hexo-theme-next.git`
2. 配置主题
   修改文件名为 next，在站点配置文件中把 theme 设置为 next 即可。
3. 基本配置项
   网站样式，边栏头像等见[官网](https://theme-next.iissnan.com/)。

{% note success %}
_深度优化_
{% endnote %}

### 一.设置 RSS
1. 安装插件
   `npm install --save hexo-generator-feed`
2. 站点配置文件搜索 plugins 添加如下
   `plugins: hexo-generate-feed`
3. 打开主题配置文件搜索 rss 并修改为如下
   `rss: /atom.xml`

### 二.样式修改
```
  theme/next/source/css/_custom/custom styles. // 用户可以在这个文件中自定义网站样式
  @media screen and (min-width:1200px) {

      body {
      background-image:url(/images/bg1.jpg);      //这一行的括号里填背景图片的路径，将图片重命名为background.jpg放在\themes\next\source\images下，也可填图片的链接。
      background-repeat: no-repeat;
      background-attachment:fixed;
        background-position:50% 50%;
      background-size: 100%;
    }

      #footer a {                                 // 配置页脚
          color:#eee;
      }

      .site-meta {
        background-color: #868181;
      }
      .main-inner {                               // 配置文章区域
        background: #fff;
        opacity: 0.9;
      }
      .header-inner {                             // 配置网站头
        opacity: 0.85;
      }
      .sidebar {                                  // 配置侧边栏
        opacity: 0.9;
        margin-left: -200px;
      }
      .posts-expand .post-eof {
        width: 100%;
      }
      .posts-expand .post-tags a {
        border-bottom: none;
      }
  }
```

### 三.评论功能
  我使用的是 github 提供的评论功能，需要在 git 上申请，再在主题配置中开启。

1. 注册 OAuth application
   在[github](https://github.com/settings/profile) 中进行注册
   点击左侧 Developer settings -> New Github App

 ```
  Application name:                  // 应用名称
  Homepage URL:                      // 网站 URL(填自己的博客主页地址)
  Application description:           // 描述
  Authorization callback URL:        // 网站 URL(填自己的博客主页地址)
  Webhook URL:                       // 网站 URL(填自己的博客主页地址)
```

   注册完成之后，会得到：Client ID 和 Client Secret,可以在 github 中建一个项目，专门用来存储你的博客评论

2. 配置 next 主题文件
   编辑主题配置文件：themes/next/\_config.yml，找到有关 gitment 的设置

```
  gitment:
    enable: true
    mint: true
    count: true
    lazy: false                     // 评论懒加载，如果true，则默认不展示评论，点击按钮查看评论
    cleanly: false
    language:
    github_user:                    // github 用户名
    github_repo: BlogComments       // 上一步新建存放评论的仓库SSH地址
    client_id: b8bad0exxxx          // 上面注册OAuth Application的Client ID
    client_secret: bcee560xxxxxx    // 上面注册OAuth Application申请的Client Secret
    proxy_gateway:
    redirect_protocol: # Protocol of redirect_uri with force_redirect_protocol when mint
```

### 四.日历云功能
参考文章: 1.[hexo-next 主题添加日历云](https://www.zhyong.cn/posts/1da9/)

### 五.文章计数
next主题集成了计数功能，将主题配置文件中`post_wordcount`配置项设置为true即可。

### 六.访客统计
  next 主题内置了多种第三方统计插件，我使用的是不蒜子统计

1. 开启插件
   在主题配置文件中找到 busuanzi 相关的配置，设置为 true
2. 修改 swig 文件
   因为不蒜子官网的域名过期，next 主题并未修改这个 js 引用。打开/theme/next/layout/\_third-party/analytics/busuanzi-counter.swig 文件。
   找到引用了如下 js 代码的 script 标签
   `https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js`
   修改为
   `https://busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js`

### 七.添加emoji表情
 hexo原生的markdown渲染引擎不支持emoji，所以我们替换为hexo-renderer-markdown-it引擎，同时hexo-renderer-markdown-it 没有内置的emoji插件，我们还要安装install markdown-it-emoji插件。
1. 安装替换
```
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it --save
npm install markdown-it-emoji --save
```
2. 配置
```// 站点配置文件_config.yml,添加如下配置
markdown:
  render:
    html: true         // 设置为true，否则无法转义html，导致<!--more-->渲染失败
    xhtmlOut: false
    breaks: true
    linkify: true
    typographer: true
    quotes: '“”‘’'
  plugins:
    - markdown-it-footnote
    - markdown-it-sup
    - markdown-it-sub
    - markdown-it-abbr
    - markdown-it-emoji     // 我们自行拓展的emoji插件
  anchors:
    level: 2
    collisionSuffix: 'v'
    permalink: true
    permalinkClass: header-anchor
    permalinkSymbol: '' 
```

### 八.可视化管理博客
  安装 hexo-admin插件就能本地访问[https://127.0.0.1:4000/admin](https://127.0.0.1:4000/admin "https://127.0.0.1:4000/admin")就可以方便的管理博客了。不过俺觉得admin这个插件页面太丑，markdown编辑器功能也很low，就自己写了一个node项目，随便搭了个页面用来管理博客。
1. 实现思路
  基于node的文件操作，在博客项目source/_post路径中写入和删除文件。
2. 项目地址
  https://github.com/qiuxiaori/hexo-admin
3. 主要配置
  //config/config.default.js:
  `config.url = "你自己的hexo项目的source/_post的绝对路径"`
4. 部署到github
  执行hexo clean,hexo g,hexo d手动更新到github。

### 九.在博文中添加图片
#### 绝对路径
当Hexo项目中只用到少量图片时，可以将图片都放在source/images文件夹中，通过markdown语法访问它们,图片既可以在首页内容中访问到，也可以在文章正文中访问到。

```// source/images/image.jpg
![](/images/image.jpg)
```

#### 相对路径
1. 配置文件
图片除了可以放在统一的images文件夹中，还可以放在文章自己的目录中。文章的目录可以通过配置站点文件_config.yml来生成。
`post_asset_folder: true`

2. 生成文件夹
将_config.yml文件中的配置项post_asset_folder设为true后，执行命令
`$ hexo new post_name`
 在source/_posts中会生成文章post_name.md和同名文件夹post_name。将图片资源放在post_name中，文章就可以使用相对路径引用图片资源了。```// _posts/post_name/image.jpg
![](image.jpg)
```

3. 首页显示
上述是markdown的引用方式，图片只能在文章中显示，但无法在首页中正常显示。如果希望图片在文章和首页中同时显示，可以使用标签插件语法。```// _posts/post_name/image.jpg
{% asset_img image.jpg This is an image %} 
```

### 十.添加天气插件
访问[天气API插件](https://www.tianqiapi.com/?action=iframe)官网，可以看到多款好用的插件样式，选择喜欢的复制代码到博客需要显示的页面并调整样式即可。

### 十一.添加顶部加载条
在next插件目录文件`/themes/next/layout/_partials/head.swig`的meta头部后添加代码
```// /themes/next/layout/_partials/head.swig
<script src="//cdn.bootcss.com/pace/1.0.2/pace.min.js"></script>
<link href="//cdn.bootcss.com/pace/1.0.2/themes/pink/pace-theme-flash.css" rel="stylesheet">
```

```
<style>
    .pace .pace-progress {
        background: #ff6700; /*进度条颜色*/
        height: 3px;
    }
    .pace .pace-progress-inner {
         box-shadow: 0 0 10px #1E92FB, 0 0 5px     #1E92FB; /*阴影颜色*/
    }
    .pace .pace-activity {
        border-top-color: #1E92FB;    /*上边框颜色*/
        border-left-color: #1E92FB;    /*左边框颜色*/
    }
</style>
```

### 十二.修改底部标签
打开/themes/next/layout/_macro/post.swig
搜索 `rel="tag">#`，将 # 换成 `<i class="fa fa-tag"></i>`或是图标库喜欢的icon

### 十三.note提示块
1. 配置文件
```
# Note tag (bs-callout).
note:
  # Note tag style values: // note提示块风格
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: simple
  icons: true  // 是否显示icon
  border_radius: 3 // 圆角
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0
```
2. 使用方法
共有六种note提示标签：default,primary,success,info,warning,danger
```
{% note [notename] %}
提示块标签
{% endnote %}
```
{% note info %}
示例：咱就是 info 提示块标签
{% endnote %}


### 十四.开启选项卡
1. 主题_config.yml配置文件设置选项卡
```
# Tabs tag.
tabs:
  enable: true  // 开启功能
  transition:
    tabs: true
    labels: true
  border_radius: 3  // 设置选项卡圆角
```
2. 使用方法
```
{% tabs tab,1 %} 名字为tab，默认在第1个选项卡，如果是-1则隐藏

<!-- tab -->
**选项卡 1** 
<!-- endtab -->

<!-- tab -->
**选项卡 2**
<!-- endtab -->

{% endtabs %}
```

{% tabs 俺是选项卡,1 %}

<!-- tab -->
😘
<!-- endtab -->

<!-- tab -->
✋
<!-- endtab -->

{% endtabs %}















