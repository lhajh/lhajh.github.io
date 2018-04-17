---
layout: post
title: VSCode 初始化配置
categories: [vscode]
description: VSCode 初始化配置
keywords: vscode
---

VSCode初始化配置

## 代码片段

此代码片段默认缩进是 2 个空格，如需要修改为 4 个空格，直接添加空格即可。

注：不支持 tab 缩进，使用 tab 缩进会报错

### HTML5 代码片段

```
"html5": {
  "prefix": "html5",
  "body": [
    "<!DOCTYPE html>",
    "<html lang=\"en\">",
    "  <head>",
    "    <meta charset=\"UTF-8\">",
    "    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">",
    "    <meta http-equiv=\"X-UA-Compatible\" content=\"ie=edge\">",
    "    <title>${1:Document}</title>",
    "    <style type=\"text/css\">",
    "      ",
    "    </style>",
    "  </head>",
    "  <body>",
    "    $2",
    "  </body>",
    "</html>"
  ],
  "description": "HTML5"
}
```

### vue 代码片段

```
"initVue": {
	"prefix": "initVue",
	"body": [
		"<template>",
		"  <div class=\"$1\">",
		"    ",
		"  </div>",
		"</template>",
		"",
		"<script>",
		"  ",
		"  export default {",
		"    name: '',",
		"    components: {",
		"      ",
		"    },",
		"    props: {",
		"      ",
		"    },",
		"    data () {",
		"      return {",
		"        ",
		"      }",
		"    },",
		"    computed: {},",
		"    watch: {},",
		"    methods: {",
		"      ",
		"    },",
		"    created () {",
		"      ",
		"    },",
		"    mounted () {",
		"      ",
		"    }",
		"  }",
		"</script>",
		"",
		"<style lang=\"less\">",
		"  ",
		"</style>"
	],
	"description": "initVue"
}
```

## 插件

- Vetur
  - Vue 代码片段，语法高亮
  - [Doc](https://vuejs.github.io/vetur)
- ESLint
  - 语法检查
- JavaScript (ES6) code snippets
  - ES6 代码片段
- vscode-element-helper
  - 一款 ElementUi 的 VS Code 插件
  - [Doc](https://github.com/ElemeFE/vscode-element-helper)
- vscode-pangu
  - 一款给中英文间加空格的 VS Code 插件
  - [Doc](https://github.com/baurine/vscode-pangu)
  - [如何实现一个给中英文间加空格的 VS Code 扩展](https://juejin.im/post/5a43718cf265da43310e1a34)
- Document This
  - 为 js 中的函数自动生成注释
- vscode-fileheader
  - 文件头部注释
- GitLens
  - Git 可视化工具，自带 diff 功能
- Path Intellisense
  - 路径自动补全
- Bracket Pair Colorizer
  - 给括号添加对应颜色，利于阅读
- Date & Time
  - 编辑器右下角显示时间，Mac 开发有用
- VSCode Great Icons
  - 图标

以下为一些有点用处的插件

- Auto Close Tag
	- 标签自动闭合
- Auto Rename Tag
	- 前后标签同时修改
- Autoprefixer
	- 为 css 属性添加浏览器兼容前缀， webpack PostCSS 也具有相同功能
- JS-CSS-HTML-Formatter
	- 格式化插件，保存后自动格式，但会和 vue 脚手架安装的 ESLint 冲突
- Live Server
	- 启动一个本地服务，用于测试调试
- PostCSS Sorting
	- 对 css 按一定规则属性进行排序
- Prettify jSON
	- 格式化 JSON
- View In Browser
	- html 在浏览器打开 html 页面

## 系统设置

```
{
    // 字体
    "editor.fontSize": 20,
    "editor.fontFamily": "Consolas, Menlo, Monaco, 'Courier New', monospace",
    // 一个制表符等于的空格数
    "editor.tabSize": 2,
    // 在“打开的编辑器”窗格中显示的编辑器数量。将其设置为 0 可隐藏窗格。
    "explorer.openEditors.visible": 0,
    // 自动保存（失焦保存）
    "files.autoSave": "onFocusChange",
    // 图标 插件名：VSCode Great Icons
    "workbench.iconTheme": "vscode-great-icons",
    // 主题 系统自带
    "workbench.colorTheme": "Monokai Dimmed",
    // 文件头部注释 插件名：vscode-fileheader 快捷键：control + option + i
    // 文件作者
    "fileheader.Author": "lhajh",
    // 最后修改者
    "fileheader.LastModifiedBy": "lhajh",
    // 最后生成样式
    "fileheader.tpl": "/*\r\n * @Author: {author}\r\n * @File: 文件描述\r\n * @Date: {createTime}\r\n * @Last Modified by: {lastModifiedBy}\r\n * @Last Modified time: {updateTime}\r\n */\r\n",
    // 为函数生成注释 插件名：Document This 快捷键：control + option + d 两遍
    // 注释添加描述
    "docthis.includeDescriptionTag": true,
    // 注释添加作者
    "docthis.includeAuthorTag": true,
    "docthis.authorName": "lhajh"
}
```

## 修改键盘快捷键

注：`+` 表示需要点击下一个键时，上一个或多个键仍是按下去状态；`空格` 表示点击下一个键时，上一个或多个键可以放开

### Windows

- C: Ctrl
- S: Shift
- A: Alt
- D: Delete
- B: Backspace
- E: Enter
- UA: UpArrow(上箭头)
- DA: DownArrow(下箭头)

常用快捷键

- C + S + /: 切换块注释
- C + /: 切换行注释
- C + d: 删除当前行
- C + UA: 向上复制行
- C + DA: 向下复制行
- A + UA: 向上移动行
- A + DA: 向下移动行
- C + f: 查找
- F3: 查找下一个
- S + F3: 查找上一个
- C + h: 替换
- C + S + f: 在文件中查找
- C + S + h: 在文件中替换
- C + E: 在下面插入行
- C + k v: 打开侧边预览
- S + A + f: 格式化文件
- C + k C + f: 格式化选中代码
- C + k C + x: 剪裁尾随空格
- C + ]: 行缩进
- C + [: 行减少缩进
- C + g: 转到行
- C + b: 切换侧栏可见性
- C + j: 切换终端面板
- C + D: 删除右侧字符
- C + B: 删除左侧字符
- C + S + F5: 重启
- S + A + O: 删除未使用的导入并对剩余的导入进行排序, 该命令适用于JavaScript和TypeScript的ES6模块。

![](/assets/images/posts/vscode/ts-organize-imports.gif)

```
// 将键绑定放入此文件中以覆盖默认值
[
    {
        "key": "ctrl+shift+oem_2",
        "command": "editor.action.blockComment",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "shift+alt+a",
        "command": "-editor.action.blockComment",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "ctrl+d",
        "command": "editor.action.deleteLines",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "ctrl+shift+k",
        "command": "-editor.action.deleteLines",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "ctrl+up",
        "command": "editor.action.copyLinesUpAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "shift+alt+up",
        "command": "-editor.action.copyLinesUpAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "alt+up",
        "command": "editor.action.moveLinesUpAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "alt+up",
        "command": "-editor.action.moveLinesUpAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "ctrl+down",
        "command": "editor.action.copyLinesDownAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "shift+alt+down",
        "command": "-editor.action.copyLinesDownAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "alt+down",
        "command": "editor.action.moveLinesDownAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "alt+down",
        "command": "-editor.action.moveLinesDownAction",
        "when": "editorTextFocus && !editorReadonly"
    }
]
```
### Mac

- C: command
- S: shift
- O: option
- D: Delete(自带键盘没有)
- B: Backspace(自带键盘的 delete)
- E: Enter(自带键盘的 return)
- UA: UpArrow(上箭头)
- DA: DownArrow(下箭头)
- CT: control

常用快捷键

- O + S + /: 切换块注释
- C + /: 切换行注释
- C + d: 删除当前行
- C + UA: 向上复制行
- C + DA: 向下复制行
- O + UA: 向上移动行
- O + DA: 向下移动行
- C + f: 查找
- F3: 查找下一个
- S + F3: 查找上一个
- C + h: 替换
- C + S + f: 在文件中查找
- C + S + h: 在文件中替换
- C + E: 在下面插入行
- C + k v: 打开侧边预览
- S + O + f: 格式化文件
- C + k C + f: 格式化选中代码
- C + k C + x: 剪裁尾随空格
- C + ]: 行缩进
- C + [: 行减少缩进
- CT + g: 转到行
- C + b: 切换侧栏可见性
- C + j: 切换终端面板
- C + D: 删除右侧字符
- C + B: 删除左侧字符
- C + S + F5: 重启
- S + O + 字母O: 删除未使用的导入并对剩余的导入进行排序, 该命令适用于JavaScript和TypeScript的ES6模块。

```
// 将键绑定放入此文件中以覆盖默认值
[
    {
        "key": "shift+alt+/",
        "command": "editor.action.blockComment",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "shift+alt+a",
        "command": "-editor.action.blockComment",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "cmd+d",
        "command": "editor.action.deleteLines",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "shift+cmd+k",
        "command": "-editor.action.deleteLines",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "cmd+up",
        "command": "editor.action.copyLinesUpAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "shift+alt+up",
        "command": "-editor.action.copyLinesUpAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "alt+up",
        "command": "editor.action.moveLinesUpAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "alt+up",
        "command": "-editor.action.moveLinesUpAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "cmd+down",
        "command": "editor.action.copyLinesDownAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "shift+alt+down",
        "command": "-editor.action.copyLinesDownAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "alt+down",
        "command": "editor.action.moveLinesDownAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "alt+down",
        "command": "-editor.action.moveLinesDownAction",
        "when": "editorTextFocus && !editorReadonly"
    },
    {
        "key": "cmd+h",
        "command": "editor.action.startFindReplaceAction"
    },
    {
        "key": "alt+cmd+f",
        "command": "-editor.action.startFindReplaceAction"
    }
]
```

## 一些小技巧

- [VsCode中使用Emmet神器快速编写HTML代码](https://www.cnblogs.com/summit7ca/p/6944215.html)