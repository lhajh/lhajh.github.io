---
layout: post
title: VSCode 初始化配置
categories: [vscode]
description: VSCode 初始化配置
keywords: vscode
---

VSCode 初始化配置

在官网 [Visual Studio Code](https://code.visualstudio.com/) 可以查看介绍和下载

## 代码片段

### 片段变量

在自定义代码段中, 您现在可以使用变量。变量的语法适用于 `$name` 简单变量和 `${name:default}` 具有默认值的变量。变量计算其值, 空字符串或其默认值（如果存在）。当变量未知时, 我们将其作为占位符插入。

可以使用以下变量：

  * `TM_SELECTED_TEXT` - 当前选定的文本或空字符串。
  * `TM_CURRENT_LINE` - 当前行的内容。
  * `TM_CURRENT_WORD` - 光标下的单词或空字符串的内容。
  * `TM_LINE_INDEX` - 基于零索引的行号。
  * `TM_LINE_NUMBER` - 基于单索引的行号。
  * `TM_FILENAME` - 当前文档的文件名。
  * `TM_DIRECTORY` - 当前文档的目录。
  * `TM_FILEPATH` - 当前文档的完整文件路径。

以下是使用单引号围绕所选文本的代码段示例, 或者, 如果未选择任何文本, 则插入 `type_here` -placeholder。

    "in quotes": { "prefix": "inq", "body": "'${TM_SELECTED_TEXT:${1:type_here}}'" }

示例:

此代码片段默认缩进是 2 个空格, 如需要修改为 4 个空格, 直接添加空格即可。

**注：不支持 tab 缩进, 使用 tab 缩进会报错**

### HTML5 代码片段

```json
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

```json
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

### 风格检查

- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
  - 语法检查工具
  - [Install](vscode:extension/dbaeumer.vscode-eslint)
- [TSLint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint)
  - TypeScript 语法检查工具
  - [Install](vscode:extension/eg2.tslint)
- [stylelint](https://marketplace.visualstudio.com/items?itemName=shinnn.stylelint)
  - style 语法检查工具
  - [Install](vscode:extension/shinnn.stylelint)
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
  - 实际上已经是代码格式化的工具标准, 支持格式化几乎所有的前端代码
  - [自以为是的代码格式化程序](https://prettier.io/)
  - [prettier 的配置选项（参数）官网直译](https://segmentfault.com/a/1190000012909159)
  - [vscode + prettier 专治代码洁癖](https://juejin.im/post/5a791d566fb9a0634853400e)
  - [在 vscode 中 vue 编码风格统一的方法介绍](http://www.php.cn/js-tutorial-405449.html)
  - [vscode 中编写 vue 项目标签属性如果格式化换行？](https://segmentfault.com/q/1010000012437190)
  - [Install](vscode:extension/esbenp.prettier-vscode)
- [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)
  - Vue 代码片段, 语法高亮, 格式化 .vue 文件, 包括里面的 CSS、JS, 至于模板即 HTML 部分, 官方维护者说没有比较好的工具支持, 默认是不格式化的
  - [Doc](https://vuejs.github.io/vetur)
  - [Install](vscode:extension/octref.vetur)
- [vue-format](https://marketplace.visualstudio.com/items?itemName=febean.vue-format)
  - 解决 Vetur 格式化 html 标签换行导致缩进异常问题
  - [Install](vscode:extension/febean.vue-format)
- [PostCSS Sorting](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-postcss-sorting)
  - 对 css 按一定规则属性进行排序
  - [Install](vscode:extension/mrmlnc.vscode-postcss-sorting)
- [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)
  - 格式化 JSON
  - [Install](vscode:extension/mohsen1.prettify-json)
- [JS-CSS-HTML-Formatter](https://marketplace.visualstudio.com/items?itemName=lonefy.vscode-JS-CSS-HTML-formatter)
  - 格式化插件, 保存后自动格式
  - [Install](vscode:extension/lonefy.vscode-JS-CSS-HTML-formatter)
- [vscode-pangu](https://marketplace.visualstudio.com/items?itemName=baurine.vscode-pangu)
  - 一款给中英文间加空格的 VS Code 插件
  - [Doc](https://github.com/baurine/vscode-pangu)
  - [如何实现一个给中英文间加空格的 VS Code 扩展](https://juejin.im/post/5a43718cf265da43310e1a34)
  - [Install](vscode:extension/baurine.vscode-pangu)

### 代码片段

- [JavaScript (ES6) code snippets](https://marketplace.visualstudio.com/items?itemName=xabikos.JavaScriptSnippets)
  - ES6 代码片段. 常用的类声明、ES 模块声明、CMD 模块导入等, 支持的缩写不下 20 种
  - [Install](vscode:extension/xabikos.JavaScriptSnippets)
- [VueHelper](https://marketplace.visualstudio.com/items?itemName=oysun.vuehelper)
  - [Doc](https://github.com/OYsun/vscode-VueHelper)
  - vue, vue-router 和 vuex 的代码提示
  - [Install](vscode:extension/oysun.vuehelper)
- [Vue VSCode Snippets](https://marketplace.visualstudio.com/items?itemName=sdras.vue-vscode-snippets)
  - vue 代码片段
  - [Install](vscode:extension/sdras.vue-vscode-snippets)
- [vscode-element-helper](https://marketplace.visualstudio.com/items?itemName=ElemeFE.vscode-element-helper)
  - [Doc](https://github.com/ElemeFE/vscode-element-helper)
  - 一款 ElementUi 的 VS Code 插件
  - [Install](vscode:extension/ElemeFE.vscode-element-helper)
- [HTML Snippets](https://marketplace.visualstudio.com/items?itemName=abusaidm.html-snippets)
  - 各种 HTML 标签片段, 如果你 [Emmet](#emmet-的应用) 玩的熟, 完全可以忽略这个
  - [Install](vscode:extension/abusaidm.html-snippets)

### 自动补全

- [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)
  - 文件路径补全, 在你用任何方式引入文件系统中的路径时提供智能提示和自动完成
  - [Install](vscode:extension/christian-kohler.path-intellisense)
- [Auto Close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)
  - 适用于 JSX、Vue、HTML, 在打开标签并且键入 `</` 的时候, 能自动补全要闭合的标签
  - [Install](vscode:extension/formulahendry.auto-close-tag)
- [Auto Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)
  - 适用于 JSX、Vue、HTML, 在修改标签名时, 能在你修改开始（结束）标签的时候修改对应的结束（开始）标签, 帮你减少 50% 的击键
  - [Install](vscode:extension/formulahendry.auto-rename-tag)
- [NPM Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense)
  - NPM 依赖补全, 在你引入任何 node_modules 里面的依赖包时提供智能提示和自动完成
  - [Install](vscode:extension/christian-kohler.npm-intellisense)
- [IntelliSense for CSS class names](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion)
  - CSS 类名补全, 会自动扫描整个项目里面的 CSS 类名并在你输入类名时做智能提示
  - [Install](vscode:extension/Zignd.html-css-class-completion)

### 功能增强

- [Vim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)
  - vim 被誉为『编辑器之神』, 一旦学会了 vim 的指法, 会让你终身受益, 至少在你敲代码的年代会收益, 毫无夸张, 它会让你摆脱烦人的, 在敲代码的时候频繁的移动鼠标, 这也是 vim 的设计理念之一 -- 脱离鼠标。
  - [每日一 Vim 笔记](https://lhajh.github.io/vim/2018/05/08/the-notes-of-a-daily-vim.html)
  - [每日一 Vim](https://lhajh.github.io/vim/2018/05/08/the-notes-of-a-daily-vim.html)
  - [Install](vscode:extension/vscodevim.vim)
- [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)
  - 基于 Gist 实现 VSCode 用户配置、快捷键配置、已安装插件列表等的备份和恢复功能
  - 无法跨平台恢复快捷键
  - [Install](vscode:extension/Shan.code-settings-sync)
- [Syncing](https://marketplace.visualstudio.com/items?itemName=nonoroazoro.syncing)
  - 基于 Gist 实现 VSCode 用户配置、快捷键配置、已安装插件列表等的备份和恢复功能
  - 鉴于 VSCode 从 1.27 版本开始提供了 [Platform Specific Keybindings](https://code.visualstudio.com/updates/v1_27#_platform-specific-keybindings) 功能, 如果手动将 Mac 和 windows 快捷键进行合并后, 可以跨平台恢复快捷键
  - [Install](vscode:extension/nonoroazoro.syncing)
- [GitLens — Git supercharged](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
  - Git 可视化工具, 自带 diff 功能, 能让我们在不离开编辑器,不执行任何命令的情况下知晓光标所在位置代码的修改时间、作者信息等
  - [Install](vscode:extension/eamodio.gitlens)
- [Nasc VSCode Touchbar](https://marketplace.visualstudio.com/items?itemName=felipe.nasc-touchbar)
  - 支持 MBP 的触摸条, 提供了挺多实用的功能点
  - [Install](vscode:extension/felipe.nasc-touchbar)

### 外观增强
- [Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)
  - 识别代码中的各种括号, 并且标记上不同的颜色, 方便你扫视到匹配的括号, 在括号使用非常多的情况下能环节眼部压力
  - [Install](vscode:extension/CoenraadS.bracket-pair-colorizer)
- [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)
  - 高亮各种 TODO、FIXME、HACK 之类的标记. 标记后面必须加冒号, 如 // TODO: 这是一个 todo
  - [Install](vscode:extension/wayou.vscode-todo-highlight)
- [Date & Time](https://marketplace.visualstudio.com/items?itemName=rid9.datetime)
  - 编辑器右下角显示时间, Mac 开发有用
  - [Install](vscode:extension/rid9.datetime)
- [VSCode Great Icons](https://marketplace.visualstudio.com/items?itemName=emmanuelbeziat.vscode-great-icons)
  - 侧边栏文件、文件夹图标
  - [Install](vscode:extension/emmanuelbeziat.vscode-great-icons)
- [Chinese (Simplified) Language Pack for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans)
  - 适用于 VS Code 1.23 以后的中文（简体）语言包
  - [Install](vscode:extension/MS-CEINTL.vscode-language-pack-zh-hans)

### 编码效率

- [Document This](https://marketplace.visualstudio.com/items?itemName=joelday.docthis)
  - 一键给代码中的类、函数加上注释, 支持函数声明、函数表达式、箭头函数等
  - [Install](vscode:extension/joelday.docthis)
- [vscode-fileheader](https://marketplace.visualstudio.com/items?itemName=mikey.vscode-fileheader)
  - 一键生成文件头部注释
  - [Install](vscode:extension/mikey.vscode-fileheader)
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
  - 这款插件能实时的识别单词拼写是否有误, 并给出提示
  - [Install](vscode:extension/streetsidesoftware.code-spell-checker)
- [Paste JSON as Code](https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype)
  - JSON 快速转换为其他语言的类型格式
  - [Install](vscode:extension/quicktype.quicktype)

### 其他插件

- [Vue Peek](https://marketplace.visualstudio.com/items?itemName=dariofuzinato.vue-peek)
  - 跳转到定义, 支持点击 template 里的组件名称, 但只能使用 ../ 引入才能识别跳转
  - [Install](vscode:extension/dariofuzinato.vue-peek)
- [Import Cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)
  - import 一个东西的时候, 可以计算该引入模块的大小
  - [Install](vscode:extension/wix.vscode-import-cost)
- [Autoprefixer](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-autoprefixer)
  - 为 css 属性添加浏览器兼容前缀,  webpack PostCSS 也具有相同功能
  - [Install](vscode:extension/mrmlnc.vscode-autoprefixer)
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
  - 启动一个本地服务, 用于测试调试
  - [Install](vscode:extension/ritwickdey.LiveServer)
- [View In Browser](https://marketplace.visualstudio.com/items?itemName=qinjia.view-in-browser)
  - html 在浏览器打开 html 页面
  - [Install](vscode:extension/qinjia.view-in-browser)
- [RegExp Preview and Editor](https://marketplace.visualstudio.com/items?itemName=le0zh.vscode-regexp-preivew)
  - 图形化正则表达式
  - [Install](vscode:extension/le0zh.vscode-regexp-preivew)

## 系统设置

```json
{
  // ------------------------ 样式 ------------------------
  // 字体
  "editor.fontSize": 20,
  // 系统需要安装 Fira Code 字体, 如果你需要这个字体
  // https://github.com/tonsky/FiraCode
  "editor.fontFamily": "Fira Code, Consolas, Menlo, Monaco, 'Courier New', monospace",
  "editor.fontLigatures": true,
  // 图标 插件名：VSCode Great Icons
  "workbench.iconTheme": "vscode-great-icons",
  // 主题 系统自带
  "workbench.colorTheme": "Monokai Dimmed",

  // ------------------------ 格式化代码 ------------------------
  // 按照下面配置好后, 先使用 vue-format(快捷键 opt/alt + shift + d) 格式化, 再用系统默认格式化快捷键(opt/alt + shift + f)格式
  // vue-format 默认是没有快捷键的, 需要自己配置
  // {
  //     "key": "alt+shift+d",
  //     "command": "extension.vueFormat",
  //     "when": "editorFocus"
  // }
  // vue-format 格式化 html, 快捷键格式化 js, css
  // 一个制表符等于的空格数
  "editor.tabSize": 2,
  // 启用后，保存文件时在文件末尾插入一个最终新行。
  "files.insertFinalNewline": true,
  // 启用后，保存文件时将删除在最终新行后的所有新行。
  "files.trimFinalNewlines": true,
  // 启用后，将在保存文件时剪裁尾随空格。
  "files.trimTrailingWhitespace": true,
  // eslint 保存自动格式化 插件名: ESLint
  // enables auto fix on save. Please note auto fix on save is only available if VS Code's
  // files.autoSave is either off, onFocusChange or onWindowChange. It will not work with afterDelay.
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "html",
      "autoFix": true
    },
    {
      "language": "vue",
      "autoFix": true
    }
  ],
  // prettier 设置 插件名: Prettier - Code formatter
  // 使用 'prettier-eslint' 而不是 'prettier'。其他设置只有在无法从 eslint 规则中推断时才会是后备选择。仅限 JavaScript 和 TypeScript
  "prettier.eslintIntegration": true,
  // 是否在每行末尾加一个分号
  "prettier.semi": false,
  // 使用单引号
  "prettier.singleQuote": true,
  // 换行字符串阈值
  "prettier.printWidth": 100,
  // 格式化 html 插件名: vue-format
  // 解决了 vetur 标签换行导致缩进异常问题
  // tag 的 attrs 大于该数值时，强制 attrs 换行，-1时不换行
  "vue-format.break_attr_limit": 1,
  // vetru 设置格式化 html 插件名: Vetur
  // 目前 vetur 的 template 格式化还没有很好的支持 prettier，应该说目前官方只推荐使用 js-beautify-html
  // "vetur.format.defaultFormatter.html": "js-beautify-html",
  // // Options for all default formatters
  // "vetur.format.defaultFormatterOptions": {
  //   "js-beautify-html": {
  //     // 对属性进行换行
  //     // auto: 仅在超出行长度时才对属性进行换行
  //     // force: 对除第一个属性外的其他每个属性进行换行
  //     // force-aligned: 对除第一个属性外的其他每个属性进行换行，并保持对齐
  //     // force-expand-multiline: 对每个属性进行换行
  //     "wrap_attributes": "force"
  //   }
  // },
  // 控制编辑器是否自动格式化粘贴的内容。格式化程序必须可用，并且能针对文档中的某一范围进行格式化。
  "editor.formatOnPaste": true,

  // ------------------------ 编辑器相关 ------------------------
  // 执行文字相关的导航或操作时将用作文字分隔符的字符(比如双击选中文字, 只会选中下面分隔符中的文字)
  "editor.wordSeparators": "`~!！@#$￥%^……&*(（)）=+[【{]】}\\、|;；:：'‘\"”,，.。<《>》/、?？",
  // 在“打开的编辑器”窗格中显示的编辑器数量。将其设置为 0 可隐藏窗格。
  "explorer.openEditors.visible": 0,
  // 自动保存（失焦保存）
  "files.autoSave": "onFocusChange",
  // 控制键入时是否应自动显示建议
  "editor.quickSuggestions": {
    "other": true,
    "comments": true, // 注释
    "strings": true // 字符串
  },
  // 面包屑导航
  // https://code.visualstudio.com/updates/v1_26#_breadcrumbs
  "breadcrumbs.enabled": true,
  // 在导航路径视图中仅显示当前符号
  "breadcrumbs.symbolPath": "last",
  // 控制资源管理器是否在把文件删除到废纸篓时进行确认。
  "explorer.confirmDelete": false,
  // 小地图最大宽度
  "editor.minimap.maxColumn": 80,
  // 控制是否显示工作台底部状态栏中的 Twitter 反馈 (笑脸图标)。
  "workbench.statusBar.feedback.visible": false,

  // ------------------------ 插件相关 ------------------------
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
  "docthis.authorName": "lhajh",

  // gitlens 指定应该禁止哪些消息 插件名: GitLens — Git supercharged
  "gitlens.advanced.messages": {
    "suppressCommitHasNoPreviousCommitWarning": false,
    "suppressCommitNotFoundWarning": false,
    "suppressFileNotUnderSourceControlWarning": false,
    "suppressGitVersionWarning": false,
    "suppressLineUncommittedWarning": false,
    "suppressNoRepositoryWarning": false,
    "suppressResultsExplorerNotice": false,
    "suppressShowKeyBindingsNotice": true,
    "suppressUpdateNotice": false,
    "suppressWelcomeNotice": true
  },
  // Specifies whether to show avatar images instead of status icons in the `GitLens File History` explorer
  "gitlens.historyExplorer.enabled": false,

  // element-helper 版本 插件名: vscode-element-helper
  "element-helper.version": "2.4",

  // 在默认不支持 Emmet 的语言中启用 Emmet 缩写功能。在此添加该语言与受支持的语言间的映射。
  "emmet.includeLanguages": {
    "vue-html": "html"
  },
  // vim 插件名: Vim
  // Override VSCode's copy command with our own copy command, which works better with VSCodeVim. Turn this off if copying is not working.
  "vim.overrideCopy": false,
  "vim.startInInsertMode": true
}
```

## 修改键盘快捷键

注：`+` 表示需要  点击下一个键时, 上一个或多个键仍是按下去状态；`空格` 表示点击下一个键时, 上一个或多个键可以放开

- C: command / ctrl
- S: shift
- O: option / alt
- D: Delete(Mac 自带键盘没有)
- B: Backspace(Mac 自带键盘的 delete)
- E: Enter(Mac 自带键盘的 return)
- UA: UpArrow(上箭头)
- DA: DownArrow(下箭头)
- CT: control

常用快捷键

- C + S + p or F1: 打开命令面板
- C + p: [快速打开文件](https://code.visualstudio.com/updates/vJanuary#_file-picker)
- O + S + /: 切换块注释
- C + /: 切换行注释
- C + d: 删除当前行
- C + UA: 向上复制行
- C + DA: 向下复制行
- O + UA: 向上移动行
- O + DA: 向下移动行
- C + S + \\: 匹配括号的闭合处，跳转
- 查找相关:
- C + f: 查找
- O + E: [查找后使用此快捷键可以立即选择所有查找结果](https://code.visualstudio.com/updates/May_2016#_select-all-find-matches)
- F3: 查找下一个
- S + F3: 查找上一个
- UA: 上一个搜索关键词 history.showPrevious
- DA: 下一个搜索关键词 history.showNext
- O + C + 字母 c: Mac 切换是否区分大小写 toggleSearchCaseSensitive
- O + C + w: Mac 切换是否全字匹配 toggleSearchWholeWord
- O + C + r: Mac 切换是否正则匹配 toggleSearchRegex
- O + 字母 c: windows 切换是否区分大小写 toggleSearchCaseSensitive
- O + w: windows 切换是否全字匹配 toggleSearchWholeWord
- O + r: windows 切换是否正则匹配 toggleSearchRegex
- Esc: 当焦点在查找弹窗组件时, 退出查找
- S + Esc: 当焦点不在查找弹窗组件时,退出查找
- C + h: 替换
- C + S + f: 在文件中查找
- [跳转搜索结果](https://code.visualstudio.com/updates/v1_9#_search-result-navigation)
- UA: 当焦点在搜索结果时, 跳转到上一个搜索结果
- DA: 当焦点在搜索结果时, 跳转到下一个搜索结果
- F4: 无论焦点是否在搜索结果, 跳转到下一个搜索结果
- S + F4: 无论焦点是否在搜索结果, 跳转到上一个搜索结果
- C + S + h: 在文件中替换
- C + E: 在下面插入行
- F8: 跳转到下一个 Error 或 Warning
- C + k v: 打开侧边预览
- S + O + f: 格式化文件
- C + k C + f: 格式化选中代码
- C + k C + x: 剪裁尾随空格
- C + i: 选中当前行
- C + ]: 行缩进
- C + [: 行减少缩进
- C + k C + [: 折叠所有子区域代码
- C + k C + ]: 展开所有子区域代码
- C + O + [: 折叠光标处最内部的未折叠区域
- C + O + ]: 展开光标处的折叠区域
- C + k C + 数字 0: 折叠编辑器中的所有区域
- C + k C + j: 展开编辑器中的所有区域
- CT + g / C + g: 转到行
- C + b: 切换侧栏可见性
- C + j / CT + `: 切换终端面板
- C + D: 删除右侧字符
- C + B: 删除左侧字符
- S + O + 字母 O: 删除未使用的导入并对剩余的导入进行排序, 该命令适用于 JavaScript 和 TypeScript 的 ES6 模块。
- O + 鼠标左键: 多行编辑 (适用于每行编辑位置不一样)
- S + O + UA / DA: 多行编辑 (适用于每行编辑位置都一样)

```json
// 将键绑定放入此文件中以覆盖默认值
[
  // 切换块注释
  {
    "key": "shift+alt+/",
    "command": "editor.action.blockComment",
    "when": "isMac && editorTextFocus && !editorReadonly"
  },
  {
    "key": "shift+alt+oem_2",
    "command": "editor.action.blockComment",
    "when": "isWindows && editorTextFocus && !editorReadonly"
  },
  {
    "key": "shift+alt+a",
    "command": "-editor.action.blockComment",
    "when": "editorTextFocus && !editorReadonly"
  },
  // 删除行
  {
    "key": "cmd+d",
    "command": "editor.action.deleteLines",
    "when": "isMac && editorTextFocus && !editorReadonly"
  },
  {
    "key": "shift+cmd+k",
    "command": "-editor.action.deleteLines",
    "when": "isMac && editorTextFocus && !editorReadonly"
  },
  {
    "key": "ctrl+d",
    "command": "editor.action.deleteLines",
    "when": "isWindows && textInputFocus && !editorReadonly"
  },
  {
    "key": "ctrl+shift+k",
    "command": "-editor.action.deleteLines",
    "when": "isWindows && textInputFocus && !editorReadonly"
  },
  // 向上复制行
  {
    "key": "cmd+up",
    "command": "editor.action.copyLinesUpAction",
    "when": "isMac && editorTextFocus && !editorReadonly"
  },
  {
    "key": "ctrl+up",
    "command": "editor.action.copyLinesUpAction",
    "when": "isWindows && editorTextFocus && !editorReadonly"
  },
  {
    "key": "shift+alt+up",
    "command": "-editor.action.copyLinesUpAction",
    "when": "editorTextFocus && !editorReadonly"
  },
  // 向下复制行
  {
    "key": "cmd+down",
    "command": "editor.action.copyLinesDownAction",
    "when": "isMac && editorTextFocus && !editorReadonly"
  },
  {
    "key": "ctrl+down",
    "command": "editor.action.copyLinesDownAction",
    "when": "isWindows && editorTextFocus && !editorReadonly"
  },
  {
    "key": "shift+alt+down",
    "command": "-editor.action.copyLinesDownAction",
    "when": "editorTextFocus && !editorReadonly"
  },
  // 替换
  {
    "key": "cmd+h",
    "command": "editor.action.startFindReplaceAction"
  },
  {
    "key": "alt+cmd+f",
    "command": "-editor.action.startFindReplaceAction"
  },
  // 向下多选多行编辑
  {
    "key": "shift+alt+down",
    "command": "cursorColumnSelectDown",
    "when": "textInputFocus"
  },
  {
    "key": "shift+alt+cmd+down",
    "command": "-cursorColumnSelectDown",
    "when": "textInputFocus"
  },
  {
    "key": "ctrl+shift+alt+down",
    "command": "-cursorColumnSelectDown",
    "when": "textInputFocus"
  },
  // 向上多选多行编辑
  {
    "key": "shift+alt+up",
    "command": "cursorColumnSelectUp",
    "when": "textInputFocus"
  },
  {
    "key": "shift+alt+cmd+up",
    "command": "-cursorColumnSelectUp",
    "when": "isMac && textInputFocus"
  },
  {
    "key": "ctrl+shift+alt+up",
    "command": "-cursorColumnSelectUp",
    "when": "isWindows && textInputFocus"
  },
  // 查找当前选中文字, 将查找到的相同文字选中并进入多光标编辑模式
  {
    "key": "alt+d",
    "command": "editor.action.addSelectionToNextFindMatch",
    "when": "editorFocus"
  },
  {
    "key": "cmd+d",
    "command": "-editor.action.addSelectionToNextFindMatch",
    "when": "isMac && editorFocus"
  },
  {
    "key": "ctrl+d",
    "command": "-editor.action.addSelectionToNextFindMatch",
    "when": "isWindows && editorFocus"
  },
  // vue-format 快捷键
  {
    "key": "alt+shift+d",
    "command": "extension.vueFormat",
    "when": "editorFocus"
  },
  // 切换是否区分大小写
  {
    "key": "ctrl+alt+c",
    "command": "workbench.action.terminal.toggleFindCaseSensitive",
    "when": "isWindows && terminalFindWidgetFocused"
  },
  {
    "key": "alt+c",
    "command": "-workbench.action.terminal.toggleFindCaseSensitive",
    "when": "isWindows && terminalFindWidgetFocused"
  },
  {
    "key": "ctrl+alt+c",
    "command": "workbench.action.terminal.toggleFindCaseSensitiveTerminalFocus",
    "when": "isWindows && terminalFocus"
  },
  {
    "key": "alt+c",
    "command": "-workbench.action.terminal.toggleFindCaseSensitiveTerminalFocus",
    "when": "isWindows && terminalFocus"
  },
  // 切换是否全字匹配
  {
    "key": "ctrl+alt+w",
    "command": "workbench.action.terminal.toggleFindWholeWord",
    "when": "isWindows && terminalFindWidgetFocused"
  },
  {
    "key": "alt+w",
    "command": "-workbench.action.terminal.toggleFindWholeWord",
    "when": "isWindows && terminalFindWidgetFocused"
  },
  {
    "key": "ctrl+alt+w",
    "command": "workbench.action.terminal.toggleFindWholeWordTerminalFocus",
    "when": "isWindows && terminalFocus"
  },
  {
    "key": "alt+w",
    "command": "-workbench.action.terminal.toggleFindWholeWordTerminalFocus",
    "when": "isWindows && terminalFocus"
  },
  // 切换是否正则匹配
  {
    "key": "ctrl+alt+r",
    "command": "workbench.action.terminal.toggleFindRegex",
    "when": "isWindows && terminalFindWidgetFocused"
  },
  {
    "key": "alt+r",
    "command": "-workbench.action.terminal.toggleFindRegex",
    "when": "isWindows && terminalFindWidgetFocused"
  },
  {
    "key": "ctrl+alt+r",
    "command": "workbench.action.terminal.toggleFindRegexTerminalFocus",
    "when": "isWindows && terminalFocus"
  },
  {
    "key": "alt+r",
    "command": "-workbench.action.terminal.toggleFindRegexTerminalFocus",
    "when": "isWindows && terminalFocus"
  },
  // 折叠光标处未折叠区域
  {
    "key": "ctrl+alt+oem_4",
    "command": "editor.fold",
    "when": "isWindows && editorTextFocus"
  },
  {
    "key": "ctrl+shift+oem_4",
    "command": "-editor.fold",
    "when": "editorTextFocus"
  },
  // 展开光标处的折叠区域
  {
    "key": "ctrl+alt+oem_6",
    "command": "editor.unfold",
    "when": "isWindows && editorTextFocus"
  },
  {
    "key": "ctrl+shift+oem_6",
    "command": "-editor.unfold",
    "when": "editorTextFocus"
  }
]
```

## 一些小技巧

### [region](https://code.visualstudio.com/updates/v1_17#_folding-regions)

* TypeScript/JavaScript: `//#region` and `//#endregion` and `//region` and `//endregion`

    ![Region Folding](https://code.visualstudio.com/assets/updates/1_17/region-folding.gif)
* C#: `#region` and `#endregion`
* C/C++: `#pragma region` and `#pragma endregion`
* F#: `//#region` and `//#endregion`
* Powershell: `#region` and `#endregion`
* VB: `#Region` and `#End Region`
* [CSS/SCSS/Less](https://code.visualstudio.com/updates/v1_23#_css-region-folding): `/* #region */` and `/* #endregion */`
* [SCSS/Less](https://code.visualstudio.com/updates/v1_23#_css-region-folding): `// #region` and `// #endregion`

    ![css-folding](/assets/images/posts/vscode/css-folding.gif)
* Markdown: 标题

    ![](/assets/images/posts/vscode/34ftg.gif)
* Coffeescript: `#region` and `#endregion`
* PHP: `#region` and `#endregion`
* Bat: `::#region` and `::#endregion`

Note: If you don't remember a folding marker, type `#` at the beginning of a line and you will get IntelliSense suggestions. Each language proposes completion proposals or snippets.

### [Markdown workspace symbol search](https://code.visualstudio.com/updates/v1_23#_markdown-workspace-symbol-search)

Markdown now has support for workspace symbol search. After opening a Markdown file for the first time, you can use (`⌘T`) to search through the headers of all Markdown files in the current workspace:

![markdown-workspace-symbol-search](/assets/images/posts/vscode/markdown-workspace-symbol-search.png)

### [Automatic member property suggestions](https://code.visualstudio.com/updates/v1_20#_automatic-member-property-suggestions)

Tired of typing `this.` to access class properties in JavaScript and TypeScript? Now you can just start typing to see available members.

![No more need to type this. to see property suggestions](https://code.visualstudio.com/assets/updates/1_20/ts-this-dot-pre.png)

Accept a member property suggestion, and VS Code automatically inserts the require `this.`.

![this. is automatically inserted when you suggest a property suggestion](https://code.visualstudio.com/assets/updates/1_20/ts-this-dot-post.png)

### [Platform specific keybindings](https://code.visualstudio.com/updates/v1_27#_platform-specific-keybindings)

It's now possible to enable keyboard shortcuts for specific operating systems using `isLinux`, `isMac` and `isWindows` within a keybinding's `when` clause:

```json
{
  "key": "ctrl+o",
  "command": "workbench.action.files.openFolder",
  "when": "!isMac"
},
{
  "key": "cmd+o",
  "command": "workbench.action.files.openFolder",
  "when": "isMac"
}
```

This makes it much easier to share your `keybindings.json` file across different machines.

### Tab completion

Editor Tab completion can now complete all kind of suggestions. After setting `"editor.tabCompletion": "on"`, pressing Tab will complete any prefix, not just snippets. Also, pressing Tab will insert the next suggestion and ⇧Tab will insert the previous suggestion.

![Tab completion](https://code.visualstudio.com/assets/updates/1_28/tabcompletion.gif)

### Navigate to last edit location

A new command **Go to Last Edit Location** (`workbench.action.navigateToLastEditLocation`) was added to quickly navigate to the last location in a file that was edited. The default keybinding is `⌘K ⌘Q`.

### Save without formatters

The new command **Save without Formatting** (`workbench.action.files.saveWithoutFormatting`) can be used to save a file without triggering any of the save participants (for example, formatters, remove trailing whitespace, final newline). The default keybinding is `⌘K S`. This is useful when editing files outside your normal projects, which may have different formatting conventions.

### IntelliSense locality bonus

Suggestions can now be sorted based on their distance to the cursor. Set `"editor.suggest.localityBonus": true` and you'll see, for example, function parameters showing up at the top of the IntelliSense list.

![Locality bonus](https://code.visualstudio.com/assets/updates/1_28/locality-bonus.png)

## Emmet 的应用

vscode 中集成了 Emmet。 Emmet 可以有效提升输入速度。正常情况下, 编写 HTML 或者 CSS 时, 需要输入很多字符。而现在有了 Emmet, 通过输入简写就行了。

- [Emmet 官网](https://emmet.io/)
- [Emmet in Visual Studio Code](https://code.visualstudio.com/docs/editor/emmet)
- [Emmet-前端开发神器](https://segmentfault.com/a/1190000007812543)

### 快速输入 HTML

如果熟悉 CSS 的语法, 你会发现 Emmet 就是很容易上手。

- `元素(Elements)`：生成一个 HTML 元素
- `>` ：生成子元素
- `+` ：生成元素的兄弟节点
- `*` ：生成多个相同的元素

你可以 `.` 或者 `#` 来修饰元素, 给元素加上 `class` 或者 `id`

比如我们输入 `div.test>h3.title+ul>li*3>span.text`, 效果如下。

```html
<div class="test">
  <h3 class="title"></h3>
  <ul>
    <li><span class="text"></span></li>
    <li><span class="text"></span></li>
    <li><span class="text"></span></li>
  </ul>
</div>
```

![](/assets/images/posts/vscode/v2-81e8.gif)

有些 HTML 元素有许多的属性, 在输入的过程中, 通过在标签后面加上 `:属性名` 就指定了元素的属性。

![](/assets/images/posts/vscode/v2-e904.gif)

### 快速输入 CSS

对于一些属性的名称较短的, 例如：`display` 与 `visibility`, 输入属性首字母与值的首字母即可。比如：`df` 是 `display: block;` , `dn` 是 `display: none;`。

对于一些属性的值是数值, 例如：`padding`, `margin`, `left`, `width` 等, 输入属性首字母与值即可。比如, `m1` 是 `margin: 1px;`。单位默认是 `px`, 不过你可以指定一下单位, 比如：`w2vw` 就是 `width: 2vw;`。当值是百分比时比较特殊, 字母 `p` 代表 `%`, 比如：`w5p` 就是 `width: 5%;`。

名称较长的属性往往含有连字符（-）, 输入连字符前后两个单词的首字母再加上值即可。比如：`pt10` 是 `padding-top: 10px;`。

![](/assets/images/posts/vscode/v2-3f4.gif)

默认情况下, 不能在 js 文件中使用 Emmet。在开发 React 项目时, 这会带来不便。所以, 再调整一下 系统设置。

```json
{
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  }
}
```

这里只是介绍一部分 Emmet 规则, 完整的列表点击[这里](https://docs.emmet.io/cheat-sheet/)。当你觉得有些输入很繁琐, 不妨查阅一下文档, 看看有无快捷输入的方式。

## 参考资料

- [VsCode 中使用 Emmet 神器快速编写 HTML 代码](https://www.cnblogs.com/summit7ca/p/6944215.html)
- [微软 VS Code 开发技巧集锦](https://zhuanlan.zhihu.com/p/34989844)
- [VS Code Tips and Tricks](https://github.com/Microsoft/vscode-tips-and-tricks)
- [能让你开发效率翻倍的 VSCode 插件配置（上）](https://juejin.im/post/5a08d1d6f265da430f31950e)
- [能让你开发效率翻倍的 VSCode 插件配置（中）](https://juejin.im/post/5ad13d8a6fb9a028ce7c0721)
- [VS Code 折腾记 - (1) 扯淡](https://juejin.im/post/586cf732128fe10066602d43)
- [VS Code 折腾记 - (2) 快捷键大全, 没有更全](https://juejin.im/post/586e5a5cb123db005d0f2bd1)
- [VS Code 折腾记 - (3) 多图解 VSCode 基础功能](https://juejin.im/post/5880d3b9128fe10065ccaf27)
- [VS Code 折腾记 - (4) 常用必备插件推荐【前端】](https://juejin.im/post/58a691f461ff4b006c4981a0)
- [VS Code 折腾记 - (5) Angular 2+ && Typescript 2+必备插件推荐](https://juejin.im/post/58a6f518ac502e006cc4ee2a)
- [VS Code 折腾记 - (6) 基本配置/快捷键定义/代码片段的录入（snippet）](https://juejin.im/post/58aeeca22f301e006cf65c8b)
- [VS Code 折腾记 - (7) 内置 Debug 功能深入【调教 angular-cli 最新版】](https://juejin.im/post/58c0c2e344d90400697213f2)
- [VS Code 折腾记 - (8) 新一波实用插件推荐（前端）/NG2+/TS2/Vue/React/Node/版本控制/主题](https://juejin.im/post/592542c58d6d810058025d06)
- [VS Code 折腾记 - (9) 新一轮前端插件(代码质量/正则/版本控制/NG/Vue/React)](https://juejin.im/post/59a61edc5188252428611c6a)
- [VS Code 折腾记 - (10) 你想发布自己捣鼓的 snippets 到 VSCode 插件市场!](https://juejin.im/post/5a198dd36fb9a04504079336)
- [VS Code 折腾记 - (11) 再来一波插件推荐!(代码片段,框架,Node,touchbar,TS,Git,数据库,python!!)](https://juejin.im/post/5a1b869351882533d022c7f5)
- [VS Code 折腾记 - (12) 春节前的最后一波插件推荐(前端/协作/主题)](https://juejin.im/post/5a704d84518825734f5300c8)
- [VS Code 折腾记 - (13) VS Live Share (可提高效率的代码实时协作插件)的使用姿势](https://juejin.im/post/5afd4f99f265da0b8455a404)
- [VS Code 折腾记 - (14) 再来推荐一波大前端适用系列 (Node/React/Vue/小程序/主题/代码体验等) 的插件](https://juejin.im/post/5b3d9378f265da0f6012eb65)
