---
layout: post
title: iTerm2 用法与技巧
categories: [mac]
description: iTerm2 用法与技巧
keywords: mac, iTerm2
---

iTerm2 is a replacement for Terminal and the successor to iTerm. It works on Macs with macOS 10.10 or newer. iTerm2 brings the terminal into the modern age with features you never knew you always wanted.

## 安装 iTerm2

[下载地址](https://www.iterm2.com/downloads.html)

下载的是压缩文件，解压后是执行程序文件，你可以直接双击，或者直接将它拖到 Applications 目录下。

或者你可以直接使用 Homebrew 进行安装：

`brew cask install iterm2`

**在进行下面步骤前，想先安利一下这篇文章：[终极 Shell](http://macshuo.com/?p=676)，干货满满，有利于对下面操作的理解**

## 配置 iTerm2 主题

iTerm2 最常用的主题是 Solarized Dark theme，[下载地址](http://ethanschoonover.com/solarized)

下载的是压缩文件，你先解压一下，然后打开 iTerm2，按 `Command + ,` 键，打开 Preferences 配置界面，然后 `Profiles -> Colors -> Color Presets -> Import`，选择刚才解压的 `solarized->iterm2-colors-solarized->Solarized Dark.itermcolors` 文件，导入成功，最后选择 `Solarized Dark` 主题，就可以了。

![](/assets/images/posts/mac/fsj45.png)

如果你有强迫症，导入后压缩文件和解压后的文件可以删除

## 配置 Oh My Zsh

Oh My Zsh 是对主题的进一步扩展，[地址](https://github.com/robbyrussell/oh-my-zsh)，当然他的强大之处并不局限在主题这一处

一键安装：

`sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

安装好之后，需要把 Zsh 设置为当前用户的默认 Shell（这样新建标签的时候才会使用 Zsh）：

`chsh -s /bin/zsh`

执行命令后，会让你输入电脑的密码，输入即可。完成后，需要完全退出 iTerm2, 再次进入时，就已经从 bash 切换到 zsh 了。当然，如果你哪一天又想用 bash 了，也可以使用下列命令：

`chsh -s /bin/bash`

切换成功后，退出，再次进入的时候就切换 bash 成功了，相互切换是不是很方便呢？

如果你想看看自己的机子上装了哪些 shell，可以使用如下命令：

`cat /etc/shells`

我的显示如下：
```
/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```

然后，我们编辑 `vim ~/.zshrc` 文件，将主题配置修改为 `ZSH_THEME="agnoster"`。

![](/assets/images/posts/mac/45Ge5.png)

agnoster 是比较常用的 zsh 主题之一 (默认主题)，你可以挑选你喜欢的主题，[zsh 主题列表](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)

效果如下（配置了声明高亮）：

![](/assets/images/posts/mac/2fRt6.png)

不过我更喜欢 powerlevel9k 这个主题

![](/assets/images/posts/mac/powerlevel9k.png)

如果您更喜欢 Powerlevel9k 外观以及右侧的退出代码和时间戳等附加信息，请运行：

`git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k`

然后编辑你的 `~/.zshrc` 设置 `ZSH_THEME="powerlevel9k/powerlevel9k"`。

## 配置 Meslo 字体

使用上面的主题，需要 `Meslo` 字体支持，要不然会出现乱码的情况，字体下载地址：[Meslo LG M Regular for Powerline.ttf](https://github.com/powerline/fonts/blob/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf)

下载好之后，直接在 Mac OS 中安装即可。

然后打开 iTerm2，按 `Command + ,` 键，打开 Preferences 配置界面，然后 `Profiles -> Text -> Font -> Chanage Font`，选择 `Meslo LG M Regular for Powerline` 字体。

![](/assets/images/posts/mac/ui6H3.png)

**上图中，弹框左侧默认是选中 ` 简体中文 ` 的，需要选中上面的 `All Fonts` 才会出现 `Meslo LG M Regular for Powerline` 字体**

## 指令高亮

[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

指令高亮效果作用是当用户输入正确命令时指令会绿色高亮，错误时命令红色高亮.

先克隆 zsh-syntax-highlighting 项目，到 zsh 插件目录：

`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting`

然后编辑 `vim ~/.zshrc` 文件，找到 plugins 配置，增加 zsh-syntax-highlighting 插件。

![](/assets/images/posts/mac/ng56f.png)

source 一下

```
// Source ~/.zshrc to take changes into account:
source ~/.zshrc
```

错误的命令显示红色：

![](/assets/images/posts/mac/we34x.png)

## 历史命令自动建议填充

[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

这个功能是非常实用的，可以方便我们快速的敲历史命令。

使用方法：它根据命令历史记录建议输入命令。在键入命令时，建议命令在光标后呈现灰色显示，通过方向键 `→` 或 `End` 来替换命令行缓冲区的内容

配置步骤，先克隆 zsh-autosuggestions 项目，到指定目录：

`git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions`

然后编辑 `vim ~/.zshrc` 文件，找到 plugins 配置，增加 zsh-autosuggestions 插件。

有时候因为自动填充的颜色和背景颜色很相似，以至于自动填充没有效果，我们可以手动更改下自动填充的颜色配置，我修改的颜色值为：`586e75`，示例：

![](/assets/images/posts/mac/aoe56.png)

## 命令别名 Git

Oh My Zsh 提供了一套系统别名（alias），来达到相同的功能。比如 `gst` 作为 `git status` 的别名。而且 Git 插件是 Oh My Zsh 默认启用的，相当于你使用了 Oh My Zsh，你就拥有了一套高效率的别名，而且还是全球通用的。

下面是一些我常用的别名：

| Alias                | Command                                                                                                                                 |
|:---------------------|:---------------------------------------|
| gl                   | git pull                                                                                                                                |
| gaa                  | git add --all                                                                                                                           |
| gcmsg                | git commit -m                                                                                                                           |
| gcam                 | git commit -a -m                                                                                                                        |
| gp                   | git push                                                                                                                                |
| gco                  | git checkout                                                                                                                            |
| gm                   | git merge                                                                                                                               |
| gss                  | git status -s                                                                                                                           |
| grh                  | git reset HEAD                                                                                                                           |
| gba                  | git branch -a                                                                                                                           |
| gcf                  | git config --list                                                                                                                       |
| gcl                  | git clone --recursive                                                                                                                   |
| gd                   | git diff                                                                                                                                |
| ghh                  | git help                                                                                                                                |
| glg                  | git log --stat --color                                                                                                                  |


自带大部分 git 命令的缩写，命令内容可以参考[完整列表](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugin:git)或本地资源: `~/.oh-my-zsh/plugins/git/git.plugin.zsh`

## 单词跳转和单词删除

默认情况下，单词跳转（`option + → or ←`）和单词删除（`option + delete`）不起作用。要启用这些功能，需要手动开启下。

打开 iTerm2，按 `Command + ,` 键，打开 Preferences 配置界面，然后 `Profiles → Keys → Load Preset... → Natural Text Editing`，就可以了。

## 自定义提示样式

默认情况下，提示符显示为 `user @ hostname`，终端显示的时候会很不好看（上面图片中可以看到），我们可以手动去除。

编辑 `vim ~/.zshrc` 文件，增加 `DEFAULT_USER="user"` 配置 (DEFAULT_USER="user" 的 user 必须和提示符中的 user 匹配)，示例：

![](/assets/images/posts/mac/user.png)

我们可以通过 `whoami` 命令，查看当前用户

## iTerm2 快捷命令

|命令                                    |  说明
|---------------------------------------|---------------------------
|command + t                            |  新建标签
|command + w                            |  关闭标签
|command + 数字 / command + 左右方向键    |	 切换标签
|command + enter                        |	 切换全屏
|command + f                            |  查找
|command + d                            |  垂直分屏
|command + shift + d                    |	 水平分屏
|command + ;                            |  查看历史命令
|command + shift + h                    |	 查看剪贴板历史
|ctrl + u                               |	 清除当前行
|ctrl + l / command + r                 |	 清屏
|ctrl + a                               |	 到行首
|ctrl + e                               |	 到行尾
|ctrl + f/b                             |	 前进后退 (相当于左右方向键)
|ctrl + p                               |	 上一条命令
|ctrl + r                               |	 搜索命令历史
|ctrl + d                               |  删除当前光标的字符
|ctrl + h                               |  删除光标之前的字符
|ctrl + w                               |  删除光标之前的单词
|ctrl + k                               |  删除到文本末尾
|ctrl + t                               |  交换光标处文本
|Command + /                            |  查看当前终端中光标的位置

## iTerm2 特殊技巧

### 选中即复制

iTerm2 有 2 种好用的选中即复制模式。

- 一种是用鼠标，在 iTerm2 中，选中某个路径或者某个词汇，那么，iTerm2 就自动复制了。
- 另一种是无鼠标模式，command+f, 弹出 iTerm2 的查找模式，输入要查找并复制的内容的前几个字母，确认找到的是自己的内容之后，输入 tab，查找窗口将自动变化内容，并将其复制。如果输入的是 shift+tab，则自动将查找内容的左边选中并复制。

### 智能选中和打开

双击选中，三击选中整行，四击智能选中

选取某一部分，按住 Shift，再点击某处，可以选取整个矩形内的文本（类似 Windows 下按住 Shift 可以批量选取图标）

按住⌘键

1. 可以拖拽选中的字符串；
2. 点击 url：调用默认浏览器访问该网址；
3. 点击文件：调用默认程序打开文件；
4. 如果文件名是 filename:42，且默认文本编辑器是 MacVim 将会直接打开到这一行；
5. 点击文件夹：在 finder 中打开该文件夹；
6. 同时按住 opt 键，可以以矩形选中。

### Tab 窗口面板管理

Tab 纵向分割：`⌘+d`

![](/assets/images/posts/mac/3778142-ef77f7d6cb4a249a.png)

Tab 横向分割：`⌘+shift+d`

![](/assets/images/posts/mac/3778142-772688116f77080f.png)

切换 Tab 中的 pane：`⌘ + [`  或者 `⌘ + opt + arrow`

关闭 panel：`⌘ + w`

最大化 Tab 中的 pane，隐藏本 Tab 中的其他 pane：`⌘ + shift +enter` , 再次还原

新建 Tab ：`⌘ + t`

Tab 切换：`⌘ + arrow` 或者 `⌘ + shift + [`

改变 Tab 的顺序：`⌘ + shift + arrow`

快速切换到 Tab 上：`⌘ + Num`

最大化 Tab ： `⌘ + enter`  再次还原

窗口太多，可以使用 `⌘ + /` 快速定位到光标所在位置

![](/assets/images/posts/mac/3778142-aba0ba84c838f0ce.png)

一屏显示所有窗口：`⌘ + opt + e`

![](/assets/images/posts/mac/3778142-b1361d5421da667e.png)

### 标记跳转

类似编辑器的 mark 工具，iTerm2 也可以在命令行位置设置标记

设置标记：`⌘ + shift + m`

跳转到上个标记：`⌘ + shift + j`

## 参考资料

- [Mac OS 终端利器 iTerm2](https://www.cnblogs.com/xishuai/p/mac-iterm2.html)
- [关于 iTerm2 你不知道的一些事](https://www.jianshu.com/p/3436bcb17a03)
- [MAC 下使用 iTerm2 和 zsh](https://blog.csdn.net/u014102846/article/details/77964493)
- [zsh 全程指南](https://wdxtub.com/2016/02/18/oh-my-zsh/)
