---
title: 每天一个 vscode 小技巧
tags: vscode
categories: program
---

又学会了没用的知识呢..
<!-- more -->
### 一.mac终端用vscode打开文件

1. 安装code命令
vsode中 `comand+shift+p` -> 搜索 `code` -> 选择 'shell 在Path中安装code命令'
2. 使用
命令行输入 `code [文件夹路径]`
3. 大功告成🎉

### 二.mac完整卸载vscode

1. 退出 VSCode 应用
2. 输入如下指令，删除 VSCode 的设置和配置
```sudo rm -rf $HOME/Library/Application\ Support/Code```
3. 输入如下指令，删除 VSCode 的插件
```sudo rm -rf $HOME/.vscode```
4. 从 Application 中移除 VSCode
