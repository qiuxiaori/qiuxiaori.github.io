---
title: FFmpeg插件的使用
tags: 文件
categories: 编程技术
---

> FFmpeg 是一个自由软件，可以运行音频和视频多种格式的录影、转换、流功能，包含了libavcodec——这是一个用于多个项目中音频和视频的解码器库，以及libavformat——一个音频与视频格式转换库。 “FFmpeg”这个单词中的“FF”指的是“Fast Forward”。[ffmpeg](https://www.npmjs.com/package/ffmpeg)插件提供了一组函数和实用程序来抽象ffmpeg的命令行用法。

<!--more-->

当你发现，，实际开发中会有各种各样奇怪的需求，上传文件之类的都算是小事情了，从视频中截取帧这种，，也只能想办法实现了😢。幸好node的插件库很丰富，ffmpeg这款插件就可以满足我们对视频操作的一些基本需求，据我简单分析，插件应该是调用了node子进程模块的excu()方法，直接调用了ffmpeg软件的控制台命令，对于其他插件没有提供的方法可以参照这个思路实现。

### 一.安装ffmpeg
1. 安装ffmpeg软件
使用ffmpeg插件首先需要电脑有ffmpeg软件环境，安装ffmpeg需要brew环境。
* 安装brew,执行以下命令
  `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
  这个过程没有vpn的话有点慢，可以搜索其他安装方式，这里不细讲。

* 安装ffmpeg，
`brew install ffmpe`
 这个过程也要安装很多东西，有点久，还可以到官网下载安装包，不过需要配置，自行谷歌吧。

2. 安装插件
在项目中执行下面👇的命令
`npm install ffmpeg`

3. 引入插件
`var ffmpeg = require('ffmpeg');`

4. 使用插件创建实例
ffmpeg插件中所有的方法都可以使用回调函数或Promise两种方式调用。
* 回调函数方式```js
  new ffmpeg('/path/to/your_movie.avi', function (err, video) {
   if (!err) {
       console.log('The video is ready to be processed');
   } else {
       console.log('Error: ' + err);
    }
  });
```

* Promise方式```js
  var process = new ffmpeg('/path/to/your_movie.avi');
  process.then(function (video) {
    console.log('The video is ready to be processed');
  }, function (err) {
    console.log('Error: ' + err);
  });
```

### 二.Video对象
#### video对象属性
每次创建新实例时，此库都会提供一个新对象来检索视频信息，ffmpeg配置以及进行必要转换的所有方法：```js
  var process = new ffmpeg('/path/to/your_movie.avi');
  process.then(function (video) {
    console.log(video.metadata);   // Video metadata
    console.log(video.info_configuration);    // FFmpeg configuration
  }, function (err) {
    console.log('Error: ' + err);
  });
```

#### video对象方法
video对象预设一组方法，可以独立于预设配置进行独立操作。
1. **video.fnExtractSoundToMP3(destionationFileName，callback)**
这个方法会把视频的音频流提取为mp3文件。
**参数：**
* destionationFileName：要导出的文件的完整路径：
`/path/to/your_audio_file.mp3`

* callback: (可选）如果在进程结束时指定，它将返回新音频文件的路径：
`function (error, file)`
**例：**```js
  var process = new ffmpeg('/path/to/your_movie.avi');
  process.then(function (video) {
    // Callback mode
   video.fnExtractSoundToMP3('/path/to/your_audio_file.mp3', function (error, file) {
    if (!error)
    console.log('Audio file: ' + file);
  });
  }, function (err) {
   console.log('Error: ' + err);
  });
```

2. **video.fnExtractFrameToJPG(destinationFolder,settings, callback)**
这个方法就是帮助实现这次需求的主要方法啦，它能截取视频的一个或多个帧，返回给我们一个图像数组。
**参数：**
* destinationFolder:生成的帧的目标文件夹
`/path/to/you_file`
* setting:( 可选）更改默认设置的设置：```js
  {
    start_time: number,        // 开始的时间,插件时间久远，已不支持
    duration_time: number,     // 持续的时间
    frame_rate: number,        // 每秒的帧数
    size: number,              // 截取图片的尺寸
    number: number,            // 要截取的帧数
    every_n_frames: number,    // 间隔几帧截取
    every_n_seconds: number,   // 间隔几秒截取，不支持
    every_n_percentage: number,// 间隔百分比截取，不支持
    keep_pixel_aspect_ratio: true,// Mantain the original pixel video aspect ratio
    keep_aspect_ratio: true, // Mantain the original aspect ratio
    padding_color: 'black',  // Padding color
    file_name: null,
  }
```

* callback: (可选）如果在进程结束时指定将返回创建的帧的路径列表：
`function (error, file)`
**例：**```js
  video.fnExtractFrameToJPG('/path/to/save_your_frames', {
      frame_rate : 1,
      number : 5,
      file_name : 'my_frame_%t_%s'
    }, function (error, files) {
      if (!error)
          console.log('Frames: ' + files);
  });
```

3. **video.fnAddWatermark（watermarkPath，newFilepath，settings，callback**
这个方法负责为正在开发的视频添加水印。您可以指定图像位置的确切位置。
**参数：**
* watermarkPath: 存储图像的完整路径
`/path/to/retrieve/watermark_file.png`
* newFilepath: (可选）新视频的名称。如果未指定，将由该函数创建.
`/path/to/save/your_file_video.mp4`
* settings: (可选）更改默认设置的设置.```js
  {
    position: "SW",      // Position: NE NC NW SE SC SW C CE CW
    margin_nord: null,   // Margin nord
    margin_sud: null,    // Margin sud
    margin_east: null,   // Margin east
    margin_west: null,   // Margin west
  };
* callback: (可选）如果在流程结束时指定，它将返回包含水印的新视频的路径.
`function (error, file)`
**例:**```js
  video.fnAddWatermark('/path/to/retrieve/watermark_file.png', '/path/to/save/your_file_video.mp4', {
    position : 'SE'
  }, function (error, file) {
    if (!error)
      console.log('New video file: ' + file);
  });
```

4. **其他方法见npm[ffmpeg](https://www.npmjs.com/package/ffmpeg)。**







