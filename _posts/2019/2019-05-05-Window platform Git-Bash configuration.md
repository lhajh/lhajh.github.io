---
layout: post
title: Window 平台 Git-Bash 的配置
categories: [Windows, Git]
description: Window 平台 Git-Bash 的配置
keywords: Windows, Git-Bash
---

通过这篇文章, 可以了解到:

- **为什么要使用 Git-Bash, 以及 Git-Bash 的优点**
- **Git-Bash 的外观主题配置**
- **Git-Bash 修改前缀 (隐藏用户@主机)**
- **Git-Bash 命令行基础**

## Git-Bash 的优点

在开发的过程中, 我们可能需要使用大量的命令行工具, 或者远程连接到服务器进行操作. 然而, 由于 Window 自带的 CMD 和 PowerShell 都不太好用, 而且在 Window 下的命令又与 Linux 和 MacOS 的命令不太一样, 如果需要经常跨平台操作, 学习 Window 的命令行命令无疑是增加了学习成本, 因此就有了需要一个能做到通用命令的工具.
在 Window 下使用类似 Linux 命令的工具有很多, 例如 WIndow10 上提供的 WSL(WIndow Sub Linux), CygWin 以及 Git-Bash.

### WSL

> The Windows Subsystem for Linux lets developers run Linux environments -- including most command-line tools, utilities, and applications -- directly on Windows, unmodified, without the overhead of a virtual machine.

WSL 是微软提供的一套可以运行 Linux 环境而又不用加载虚拟机的子系统. 使用 WSL 可以像使用 Ubuntu 一样的操作, 而且还能访问 WIndow 上的文件系统.

### CygWin

CygWin 是运行在 Window 平台的类 Unix 环境, CygWIn 通过将部分 Posix 条用转换成 Window 的 API 调用, 从而实现相关功能.

### Git-Bash

我们这篇文章介绍的 Git-Bash, 是 [Git 官网](https://git-scm.com/) 上提供的一个 Git 开发工具包里的一个命令行组件.
Git-Bash 源自 MinGW, 是一个用于开发原生 Window 应用的开发环境, 提供了针对 WIn32 应用的 GCC、GNU binutils 等工具.

我们可以根据自己的需求, 进行选择, 这里我选择 Git-Bash 的原因, 是我的开发需求主要为启动前端服务及提交代码, 因此选择 Git-Bash 较为方便、简单.

## Git-Bash 的主题配置

Git-Bash 原生的主题, 其实也并不难看, 更换 Git-Bash 的需求主要在于, 我的 Ubuntu 服务器端配置了 oh-my-zsh. 因此如果使用 Git-Bash 的默认主题进行 SSH 连接, 会因为字体问题而无法正常显示, 还有就是 Git-Bash 原生的主题配色和 MacOS 下 iTerm Solarized 主题配色差距甚大. 因此, 一是视觉上不同一, 看上去不习惯, 而是本着手贱的探索精神, 总希望做点特别的挑战, 就有了去修改 Git-Bash 主题的需求.

修改 Git-Bash 主题主要有两个困难, 一是 Git-Bash 自带的 Options 下 Text 设置有缺陷, 只有有限的字体可选, 一些系统上已经安装了的字体, 这里并没有得选择. 而要使用 Git-Bash SSH 连接使用了 agnoster 主题的 ZSH, 需要一种含特殊字符的字体 Powerline, 没有了这种字体, 就会出现乱码.

### Git-Bash 配色主题设置

配置方法很简单, 在 Git-Bash 中我们输入以下代码: `vi ~/.minttyrc` , 然后把以下内容添加到配置文件里面

```
BoldAsFont=yes
FontHeight=12
Scrollbar=none
Term=xterm-256color
BoldAsColour=yes
Black=7,54,66
Red=220,50,47
Green=133,153,0
Yellow=181,137,0
Blue=38,139,210
Magenta=211,54,130
Cyan=42,161,152
White=238,232,213
BoldBlack=0,43,54
BoldRed=203,75,22
BoldGreen=88,110,117
BoldYellow=101,123,131
BoldBlue=131,148,150
BoldMagenta=108,113,196
BoldCyan=147,161,161
BoldWhite=253,246,227
ForegroundColour=192,192,192
BackgroundColour=0,43,54
CursorColour=133,153,0
Columns=80
Rows=25

BoldAsFont=-1
Locale=zh_CN
Charset=UTF-8
Font=Consolas
Transparency=off
CursorType=block
CursorBlinks=yes
```

在  vim  编辑器下, 你可以使用方向键移动光标, 按 `i` 进入编辑模式, 编辑好后按 `esc` 退出编辑模式, 随后输入 `:wq`    并回车即可保存. 有关 Vim  的更多使用方法你可以参考[这篇教程](https://www.runoob.com/linux/linux-vim.html).

然后重启 Git-Bash, 即可看到新的主题配色, 以下是我的 Git-Bash 外观主题配置样例.

![](/assets/images/posts/git/5B71993FBCA98C3B9BF53A3AE063DA19.png)

## Git-Bash 修改前缀 (隐藏用户@主机)

有时候经常嫌一层一层目录实在太长太占地方, 而且截屏时也不方便把全路径显示出来. 所以需要隐藏起来会比较方便, 需要看全路径的话一句 `pwd` 就显示了.

在 Git-Bash 中我们输入以下代码: `vi ~/.bash_profile` , 然后把以下内容添加到配置文件里面

```zsh
# Shows Git branch name in prompt.
parse_git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
# Show User @ Name (still with git branch name)
# export PS1="\u@\h \W\[\033[32m\]\$(parse_git_branch)\[\033[00m\] $ "
# Or hide User @ Name (still with git branch name)
export PS1="\W\[\033[32m\]\$(parse_git_branch)\[\033[00m\] $ "
```

## Git-Bash 命令行基础

### 一、常见命令

| 操作             | 命令                                                                                        |
| ---------------- | ------------------------------------------------------------------------------------------- |
| 进入目录         | cd                                                                                          |
| 显示当前目录     | pwd                                                                                         |
| 创建目录         | mkdir 目录名                                                                                |
| 创建目录         | mkdir -p 目录路径                                                                           |
| 我是谁           | whoami                                                                                      |
| 查看路径         | ls 路径                                                                                     |
| 查看路径         | ls -a 路径(显示隐藏文件)                                                                    |
| 查看路径         | ls -l 路径(显示更多信息(drwx:d 是否是目录, rw 是否可以读写, x 是否可以执行))                |
| 查看路径         | ls -al 路径 (显示隐藏信息和详细信息)                                                        |
| 创建文件         | echo '1' > 文件路径 (并且把内容"1"写入文件)                                                 |
| 强制创建文件     | echo '1' >! 文件路径(只能用在 linux, windows 出错)强制重定向, 覆盖原来有的内容              |
| 追加文件内容     | echo '1' >> 文件路径(只能用在 linux, windows 出错)                                          |
| 创建文件         | touch 文件名                                                                                |
| 改变文件更新时间 | touch 文件名                                                                                |
| 复制文件         | cp 源路径 目标路径(可以覆盖原来有的文件)                                                    |
| 复制目录         | cp -r 源路径 目标路径                                                                       |
| 移动节点         | mv 源路径 目标路径(修改文件名)                                                              |
| 删除文件         | rm 文件路径                                                                                 |
| 强制删除文件     | rm -f 文件路径                                                                              |
| 删除目录         | rm -r 目录路径                                                                              |
| 强制删除目录     | rm -rf 目录路径                                                                             |
| 查看目录结构     | tree(windows 不支持)                                                                        |
| 建立软链接       | ln -s 真实文件 链接                                                                         |
| 下载文件         | curl -L [https://www.baidu.com](https://www.baidu.com/) > baidu.html                        |
| 拷贝网页         | wget -p -H -e robots=off [https://www.baidu.com](https://www.baidu.com/) (整个网页所有文件) |
| 磁盘占用         | df -kh                                                                                      |
| 当前目录大小     | du -sh .                                                                                    |
| 各文件大小       | du -h                                                                                       |
| 查看文件内容     | cat                                                                                         |

#### 需要注意的点

1. 各种符号代表的含义

- `~` : 用户目录: 我的电脑上就是 `/c/Users/ASUS`
- `/` : 根目录, 一个 `/` 就是根目录, 不管你有多少个硬盘, 他会把所有硬盘联合起来当作一块
- `.` : 一个点表示当前目录,
- `../` : 两个点表示父目录
- TAB 键可以补全文件名或者目录

2. 绝对路径与相对路径

- 如果一个路径是用 `/` 开始的, Windows 就从根目录开始找起(绝对路径), 否则就从当前目录开始找(相对路径).

### 二、命令行技巧

#### 1. 打开 Git Bash 自动运行

1. 首先在命令行输入 `vi ~/.bashrc` 创建一下这个文件
2. 按 `i` 进入编辑模式编辑这个文件, 内容为 `echo 'Hi'`
3. 你也可以用命令行编辑文件 `echo "echo 'hi'" >> ~/.bashrc`
4. 关闭退出 Git Bash, 然后打开 Git Bash, 是不是看到了 Hi, 这说明每次进入 Git Bash, 就会优先运行 `~/.bashrc` 里面的命令
5. 如果没有看到 Hi, 运行 `source ~/.bashrc` , 作用是执行 `~/.bashrc`
6. 重新编辑 `~/.bashrc` , 内容改为 `cd ~/Desktop` , 重启 Git Bash, 有没有发现默认就进入桌面目录了?
7. 你可以用 `~/.bashrc` 在进入 Git Bash 前执行任何命令, 十分方便.

#### 2. `alias` 命令行缩写

1. 在 `~/.bashrc` 里新增一行 `alias f="echo 'frank is awesome'"` , 等于号两边不能有空格, 你最好一个字都不要错.
2. 运行 `source ~/.bashrc` , 作用是执行 `~/.bashrc`
3. 运行 `f` , 就会看到 `frank is awesome`
4. 也就是说, 现在 `f` 就是 `echo 'frank is awesome'` 的缩写了, 利用这个技巧, 我们可以把很多常见的命令缩写一下, 比如:

```
   alias gl='git pull'
   alias gaa='git add .'
   alias gcmsg='git commit -m'
   alias gcam='git commit -a -m'
   alias gp='git push'
   alias gco='git checkout'
   alias gm='git merge'
   alias gss='git status -s'
   alias glol='git log –graph –pretty = format:’%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset’ –abbrev-commit'
   alias grh='git reset HEAD'
   alias gba='git branch -a'
   alias gcf='git config –list'
   alias gcl='git clone –recursive'
   alias gd='git diff'
   alias ghh='git help'

   alias ns='npm run serve'
   alias nb='npm run build'
   alias nd='npm run dev'
```

5. 保存退出, 然后运行 `source ~/.bashrc`

这样一来, 你的 Git 操作就会简单很多:

```zsh
gl
gaa
gcam 'msg'
gp
```

### 3. 解决.bashrc 文件每次打开终端都需要 source 的问题

这个问题困扰我很久, 我明明改了 `~/.bashrc` 文件, 每次打开 Git Bash 都要手动输入 `source ~/.bashrc` , 配置才会生效, 很是头疼, 于是我就研究了一下解决办法以及问题的原因是什么.

#### 解决方法

`vi ~/.bash_profile` 在文件内部输入:

```zsh
# Loading .bashrc File
source ~/.bashrc
```

在 `.bash_profile` 文件中自动加载 `.bashrc` 文件.

## 参考资料

- [Window 平台 Git-Bash 的主题配置 - 简书](https://www.jianshu.com/p/5acb8d8cef32)
- [Git Bash 主题配置 - 码农教程](http://www.manongjc.com/article/79147.html)
- [Bash shell / Zsh 里修改前缀 (隐藏用户@主机, 添加 Git 分支名称) - 简书](https://www.jianshu.com/p/ee442cb4d6c2)
- [Git Bash 命令行基础 - 马涛涛的博客——前端的征途 - SegmentFault 思否](https://segmentfault.com/a/1190000013736711)
- [解决.bashrc 文件每次打开终端都需要 source 的问题 - 简书](https://www.jianshu.com/p/c4946024b946)
