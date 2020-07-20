---
title: ESLint使用简介
tags: 教程
categories: 
- 编程技术

---

> ESLint是一种用于识别和报告ECMAScript / JavaScript代码中的模式的工具。
<!--more-->
### 一.Eslint简介
Eslint的目标是使代码更加一致并避免错误在许多方面，它类似于JSLint和JSHint，但有一些例外：
ESLint使用Espree进行JavaScript解析。ESLint使用AST来评估代码中的模式。ESLint是完全可插拔的，每个规则都是一个插件，您可以在运行时添加更多。
### 二.安装使用
1. 下载插件
  可以选择全局安装和本地安装，但是使用的任何插件或可共享配置必须在本地安装
  ```
  npm i -g eslint                     //全局安装
  npm i eslint --save-dev             //本地安装```

2. 创建配置文件
  在文件根目录下新建（其他路径也可） .eslintrc.js并配置文件，也可以在命令行输入eslint --init根据提示初始化配置文件  
3. 配置文件
```
module.exports = {
    "env": {
        "browser": true,
        "commonjs": true,
        "es6": true,
        "node": true,
        "mocha": true
    },
//eslint插件
    "plugins": [],
    // recommend rules - 96
    "extends": "eslint:recommended",
//指定解析器
    "parser": "esprima",
    "parserOptions": {
        "sourceType": "module"
    },
//名称"semi"和"quotes"是ESLint 中规则的名称。第一个值是规则的错误级别，可以是以下值之一：

//"off"或0- 关闭规则
//"warn"或1- 将规则作为警告打开（不影响退出代码）
//"error"或2- 将规则作为错误打开（退出代码为1）
    "rules": {
        "linebreak-style": ["off", "unix"],
        "indent": ["off", 4],
        // Stylistic Issues
        // "quotes": ["warn", "single"],
        "keyword-spacing": ["warn", {
            "before": true,
            "after": true
        }],
        "semi": ["warn", "always"],
        // Best Practices
        "array-callback-return": ["warn"],
        "block-scoped-var": ["warn"],
        "default-case": ["warn"],
        "eqeqeq": ["warn", "smart"],
        "guard-for-in": ["warn"],
        "no-alert": ["warn"],
        "no-else-return": ["warn", {
            "allowElseIf": true
        }],
        "no-eval": ["warn"],
        "no-empty-pattern": ["error"],
        "no-extend-native": ["warn"],
        "no-labels:": ["off"],
        "require-await": ["warn"],
        // Possible Errors
        "no-console": ["warn", {
            allow: ["warn", "error"]
        }],
        "valid-jsdoc": ["warn", {
            requireReturn: false,
            requireParamDescription: false,
            requireReturnDescription: false
        }],
        // ECMAScript 6
        "no-var": ["warn"],
        "arrow-body-style": ["warn", "as-needed"],
        "arrow-spacing": ["warn", {
            "before": true,
            "after": true
        }],
        // Node.js and CommonJS
        "no-process-env": ["warn"],
        "no-use-before-define": ["warn", {
            "functions": true,
            "classes": true
        }],
    }
};
```


### 三.在vscode中的配置
1. 下载插件
  在插件中下载安装eslint  
2. 配置用户文件settings.json
  window电脑：文件 > 首选项 > 设置 打开 VSCode 配置文件
  mac电脑：code>首选项 >设置
~~~
{
    "explorer.confirmDelete": false,
    "editor.fontSize": 16,
    "javascript.updateImportsOnFileMove.enabled": "always",
    "workbench.colorTheme": "Tomorrow Night Blue",
    "workbench.iconTheme": "vscode-icons",
    "workbench.startupEditor": "newUntitledFile",
    "explorer.confirmDragAndDrop": false,
    "diffEditor.ignoreTrimWhitespace": false,
    "debug.allowBreakpointsEverywhere": true,
    "debug.console.fontSize": 14,
    "editor.tabSize": 4,
    "editor.formatOnSave": true,
    "eslint.autoFixOnSave": true,
    "prettier.eslintIntegration": true,
    "prettier.semi": false,
    "prettier.singleQuote": true,
    "javascript.format.insertSpaceBeforeFunctionParenthesis": true,

    "eslint.validate": ["javascript", "javascriptreact", "html", {
        "language": "vue",
        "autoFix": true
    }],
    "eslint.options": {
        "config-file": "/Users/zmy/Documents/git/koa-todolist/.eslintrc.js"
    },
    "files.associations": {
        "*.vue": "vue"
    },

    "search.exclude": {
        "**/node_modules": true,
        "**/bower_components": true,
        "**/dist": true
    },
    "emmet.syntaxProfiles": {
        "javascript": "jsx",
        "vue": "html",
        "vue-html": "html"
    },

    "editor.renderWhitespace": "boundary",
    "editor.minimap.enabled": true,
    "window.title": "${dirty}${activeEditorMedium}${separator}${rootName}",
    "editor.codeLens": true,
    "editor.snippetSuggestions": "top",
}

~~~

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

**设置完成后就能自动格式化代码并进行eslint检测了**