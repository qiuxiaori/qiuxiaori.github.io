---
title: 前端页面使用editor富文本编辑器
tags: 前端
categories: 编程技术
---
> 富文本编辑器，Rich Text Editor, 简称 RTE, 是一种可内嵌于浏览器，所见即所得的文本编辑器。富文本编辑器不同于文本编辑器，程序员可到网上下载免费的富文本编辑器内嵌于自己的网站或程序里，方便用户编辑文章或信息。

<!--more-->

目前常见的富文本编辑器有很多，比如[editor](https://github.com/pandao/editor.md),[showdown](https://github.com/showdownjs/showdown)等，个人觉得editor使用感不错，简单记录一下在editor的使用方式。

### 一.获取editor

1. Github download
`git clone git@github.com:pandao/editor.md.git`
或见[git主页](https://github.com/pandao/editor.md)
2. NPM install :
`npm install editor.md`
3. Bower install :
`bower install editor.md`

### 二.使用editor

```
  // 引入文件，href为存放下载的editor插件的路径
  <link rel="stylesheet" href="editor.md/css/editormd.min.css" />
  <script src="editor.md/editormd.min.js"></script>
  <script src="jquery.min.js"></script>
  // 在页面中插入
  <div id="editor">
      <!-- Tips: Editor.md can auto append a `<textarea>` tag -->
      <textarea style="display:none;">### Hello Editor.md !</textarea>
  </div>
  // 初始化editormd
  <script type="text/javascript">
      $(function() {
          var editor = editormd("editor", {
              // width: "100%",
              path : "editor.md/lib/"  // 存放下载的editor插件的lib路径
          });
      });
  </script>
```
### 三.编辑器初始化属性
重要属性大致介绍，相当于翻译了下注释哈哈
```
  {
    mode                 : "gfm",          // gfm or markdown[编辑器模式]
    name                 : "",             // Form element name for post[文章标题]
    value                : "",             // value for CodeMirror, if mode not gfm/markdown
    theme                : "",             // Editor.md self themes, before v1.5.0 is CodeMirror theme, default empty
    editorTheme          : "default",      // Editor area, this is CodeMirror theme at v1.5.0
    previewTheme         : "",             // Preview area theme, default empty
    markdown             : "",             // Markdown source code
    appendMarkdown       : "",             // if in init textarea value not empty, append markdown to textarea
    width                : "100%",
    height               : "100%",
    path                 : "./lib/",       // Dependents module file directory
    pluginPath           : "",             // If this empty, default use settings.path + "../plugins/"
    delay                : 300,            // Delay parse markdown to html, Uint : ms
    autoLoadModules      : true,           // Automatic load dependent module files
    watch                : true,
    placeholder          : "Enjoy Markdown! coding now...",
    gotoLine             : true,           // Enable / disable goto a line
    codeFold             : false,
    autoHeight           : false,
    autoFocus            : true,           // Enable / disable auto focus editor left input area
    autoCloseTags        : true,
    searchReplace        : true,           // Enable / disable (CodeMirror) search and replace function
    syncScrolling        : true,           // options: true | false | "single", default true
    readOnly             : false,          // Enable / disable readonly mode
    tabSize              : 4,
    indentUnit           : 4,
    lineNumbers          : true,           // Display editor line numbers
    lineWrapping         : true,
    autoCloseBrackets    : true,
    showTrailingSpace    : true,
    matchBrackets        : true,
    indentWithTabs       : true,
    styleSelectedText    : true,
    matchWordHighlight   : true,           // options: true, false, "onselected"
    styleActiveLine      : true,           // Highlight the current line
    dialogLockScreen     : true,
    dialogShowMask       : true,
    dialogDraggable      : true,
    dialogMaskBgColor    : "#fff",
    dialogMaskOpacity    : 0.1,
    fontSize             : "13px",
    saveHTMLToTextarea   : false,          // If enable, Editor will create a <textarea name="{editor-id}-html-code"> tag save HTML code for form post to server-side.
    disabledKeyMaps      : [],
    
    onload               : function() {},
    onresize             : function() {},
    onchange             : function() {},
    onwatch              : null,
    onunwatch            : null,
    onpreviewing         : function() {},
    onpreviewed          : function() {},
    onfullscreen         : function() {},
    onfullscreenExit     : function() {},
    onscroll             : function() {},
    onpreviewscroll      : function() {},
    
    imageUpload          : false,          // Enable/disable upload
    imageFormats         : ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
    imageUploadURL       : "",             // Upload url
    crossDomainUpload    : false,          // Enable/disable Cross-domain upload
    uploadCallbackURL    : "",             // Cross-domain upload callback url

    toc                  : true,           // Table of contents
    tocm                 : false,          // Using [TOCM], auto create ToC dropdown menu
    tocTitle             : "",             // for ToC dropdown menu button
    tocDropdown          : false,          // Enable/disable Table Of Contents dropdown menu
    tocContainer         : "",             // Custom Table Of Contents Container Selector
    tocStartLevel        : 1,              // Said from H1 to create ToC
    htmlDecode           : false,          // Open the HTML tag identification 
    pageBreak            : true,           // Enable parse page break [========]
    atLink               : true,           // for @link
    emailLink            : true,           // for email address auto link
    taskList             : false,          // Enable Github Flavored Markdown task lists
    emoji                : false,          // :emoji: , Support Github emoji, Twitter Emoji (Twemoji);
                                           // Support FontAwesome icon emoji :fa-xxx: > Using fontAwesome icon web fonts;
                                           // Support Editor.md logo icon emoji :editormd-logo: :editormd-logo-1x: > 1~8x;
    tex                  : false,          // TeX(LaTeX), based on KaTeX
    flowChart            : false,          // flowChart.js only support IE9+
    sequenceDiagram      : false,          // sequenceDiagram.js only support IE9+
    previewCodeHighlight : true,           // Enable / disable code highlight of editor preview area

    toolbar              : true,           // show or hide toolbar
    toolbarAutoFixed     : true,           // on window scroll auto fixed position
    toolbarIcons         : "full",         // Toolbar icons mode, options: full, simple, mini, See `editormd.toolbarModes` property.
    toolbarTitles        : {},
    toolbarHandlers      : {
        ucwords : function() {
            return editormd.toolbarHandlers.ucwords;
        },
        lowercase : function() {
            return editormd.toolbarHandlers.lowercase;
        }
    },
    toolbarCustomIcons   : {               // using html tag create toolbar icon, unused default <a> tag.
        lowercase        : "<a href=\"javascript:;\" title=\"Lowercase\" unselectable=\"on\"><i class=\"fa\" name=\"lowercase\" style=\"font-size:24px;margin-top: -10px;\">a</i></a>",
        "ucwords"        : "<a href=\"javascript:;\" title=\"ucwords\" unselectable=\"on\"><i class=\"fa\" name=\"ucwords\" style=\"font-size:20px;margin-top: -3px;\">Aa</i></a>"
    },
    toolbarIconTexts     : {},
    
    lang : {  // Language data, you can custom your language.
        name        : "zh-cn",
        description : "开源在线Markdown编辑器<br/>Open source online Markdown editor.",
        tocTitle    : "目录",
        toolbar     : {
            //...
        },
        button: {
            //...
        },
        dialog : {
            //...
        }
        //...
    }
  }
```
### 四.md文本转html
```
<link rel="stylesheet" href="editormd/css/editormd.preview.css" />
<div id="test-markdown-view">
    <!-- Server-side output Markdown text -->
    <textarea style="display:none;">### Hello world!</textarea>             
</div>
<script src="jquery.min.js"></script>
<script src="editormd/editormd.js"></script>
<script src="editormd/lib/marked.min.js"></script>
<script src="editormd/lib/prettify.min.js"></script>
<script type="text/javascript">
    $(function() {
	    var testView = editormd.markdownToHTML("test-markdown-view", {
            // markdown : "[TOC]\n### Hello world!\n## Heading 2", // Also, you can dynamic set Markdown text
            // htmlDecode : true,  // Enable / disable HTML tag encode.
            // htmlDecode : "style,script,iframe",  // Note: If enabled, you should filter some dangerous HTML tags for website security.
        });
    });
</script>
```