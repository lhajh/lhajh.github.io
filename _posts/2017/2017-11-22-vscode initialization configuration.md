---
layout: post
title: VSCode 初始化配置
categories: [vscode]
description: VSCode 初始化配置
keywords: vscode
---

VSCode 初始化配置

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

- C + S + p or F1: 打开命令面板
- C + p: 快速打开文件
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
- S + A + O: 删除未使用的导入并对剩余的导入进行排序, 该命令适用于 JavaScript 和 TypeScript 的 ES6 模块。
- C + 鼠标左键: 多行编辑 (适用于每行编辑位置不一样)
- C + A + UA / DA: 多行编辑 (适用于每行编辑位置都一样)

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

- C + S + p or F1: 打开命令面板
- C + p: 快速打开文件
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
- S + O + 字母O: 删除未使用的导入并对剩余的导入进行排序, 该命令适用于 JavaScript 和 TypeScript 的 ES6 模块。
- C + 鼠标左键: 多行编辑 (适用于每行编辑位置不一样)
- C + O + UA / DA: 多行编辑 (适用于每行编辑位置都一样)

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

### [CSS region folding](https://code.visualstudio.com/updates/v1_23#_css-region-folding)

You can now use `/* #region */` and `/* #endregion */` to mark a region as foldable in CSS/SCSS/Less. In SCSS/Less, you can also use `// #region` and `// #endregion` as folding markers.

![css-folding](/assets/images/posts/vscode/css-folding.gif)

### [Markdown workspace symbol search](https://code.visualstudio.com/updates/v1_23#_markdown-workspace-symbol-search)

Markdown now has support for workspace symbol search. After opening a Markdown file for the first time, you can use (`⌘T`) to search through the headers of all Markdown files in the current workspace:

![markdown-workspace-symbol-search](/assets/images/posts/vscode/markdown-workspace-symbol-search.png)

## 参考资料

- [VsCode 中使用 Emmet 神器快速编写 HTML 代码](https://www.cnblogs.com/summit7ca/p/6944215.html)
- [微软 VS Code 开发技巧集锦](https://juejin.im/entry/5abca6d36fb9a028c675c96a)
- [VS Code Tips and Tricks](https://github.com/Microsoft/vscode-tips-and-tricks)
- [能让你开发效率翻倍的 VSCode 插件配置（上）](https://juejin.im/post/5a08d1d6f265da430f31950e)
- [能让你开发效率翻倍的 VSCode 插件配置（中）](https://juejin.im/post/5ad13d8a6fb9a028ce7c0721)
- [VS Code折腾记 - (1) 扯淡](https://juejin.im/post/586cf732128fe10066602d43)
- [VS Code折腾记 - (2) 快捷键大全，没有更全](https://juejin.im/post/586e5a5cb123db005d0f2bd1)
- [VS Code折腾记 - (3) 多图解VSCode基础功能](https://juejin.im/post/5880d3b9128fe10065ccaf27)
- [VS Code折腾记 - (4) 常用必备插件推荐【前端】](https://juejin.im/post/58a691f461ff4b006c4981a0)
- [VS Code折腾记 - (5) Angular 2+ && Typescript 2+必备插件推荐](https://juejin.im/post/58a6f518ac502e006cc4ee2a)
- [VS Code 折腾记 - (6) 基本配置/快捷键定义/代码片段的录入（snippet）](https://juejin.im/post/58aeeca22f301e006cf65c8b)
- [VS Code 折腾记 - (7) 内置Debug功能深入【调教angular-cli 最新版】](https://juejin.im/post/58c0c2e344d90400697213f2)
- [VS Code 折腾记 - (8) 新一波实用插件推荐（前端）/NG2+/TS2/Vue/React/Node/版本控制/主题](https://juejin.im/post/592542c58d6d810058025d06)
- [VS Code 折腾记 - (9) 新一轮前端插件(代码质量/正则/版本控制/NG/Vue/React)](https://juejin.im/post/59a61edc5188252428611c6a)
- [VS Code 折腾记 - (10) 你想发布自己捣鼓的snippets到VSCode插件市场!](https://juejin.im/post/5a198dd36fb9a04504079336)
- [VS Code 折腾记 - (11) 再来一波插件推荐!(代码片段,框架,Node,touchbar,TS,Git,数据库,python!!)](https://juejin.im/post/5a1b869351882533d022c7f5)
- [VS Code 折腾记 - (12) 春节前的最后一波插件推荐(前端/协作/主题)](https://juejin.im/post/5a704d84518825734f5300c8)