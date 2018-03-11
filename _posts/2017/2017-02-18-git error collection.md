---
layout: post
title: git 错误集合
categories: GitHub
description: git 错误集合
keywords: git, error
---

git 错误集合

## git pull报错：Auto Merge Failed; Fix Conflicts and Then Commit the Result.

### 出错场景

协同开发时，我们从远程服务器上 pull 下代码的时候，出现以下提示信息：  
`Auto Merge Failed; Fix Conflicts and Then Commit the Result.`

### 原因分析

分析 git pull 的原理，实际上 git pull 是分了两步走的:
1. 从远程 pull 下 origin/master 分支
2. 将远程的 origin/master 分支与本地 master 分支进行合并  
以上的错误，是出在了第二步，本地与远程文件合并时存在冲突

### 解决方法

方法一：如果我们确定远程的分支正好是我们需要的，而本地的分支上的修改比较陈旧或者不正确，那么可以直接丢弃本地分支内容，运行如下命令(看需要决定是否需要运行 git fetch 取得远程分支)：
```
$:git reset --hard origin/master
或者$:git reset --hard ORIG_HEAD
```
方法二：我们不能丢弃本地修改，因为其中的某些内容的确是我们需要的，此时需要对 unmerged 的文件进行手动修改，删掉其中冲突的部分，然后运行如下命令：
```
$:git add filename
$:git commit -m "message"
$:git push origin master
```
方法三：如果我们觉得合并以后的文件内容比较混乱，想要废弃这次合并，回到合并之前的状态，那么可以运行如下命令：  
`$:git reset --hard HEAD`

## git warning: LF will be replaced by CRLF in 解决办法

### 出错场景

在使用 git 的时候，每次执行
`$ git add "目录"`
`$ git add .`
都会提示这样一个警告消息：
`warning: LF will be replaced by CRLF in XXXXXXXXXXXXXX.`

### 原因分析

CRLF -- Carriage-Return Line-Feed 回车换行，就是回车(CR, ASCII 13, \r) 换行 (LF, ASCII 10, \n)。  
这两个 ACSII 字符不会在屏幕有任何输出，但在 Windows 中广泛使用来标识一行的结束。而在 Linux/UNIX 系统中只有换行符。  
也就是说在 windows 中的换行符为 CRLF， 而在 linux 下的换行符为：LF  
使用 git 来生成一个 rails 工程后，文件中的换行符为 LF， 当执行 git add .时，系统提示：LF 将被转换成 CRLF

### 解决方法

删除生成的 .git 文件
```
$ rm -rf .git  
$ git config --gobal core.autocrlf false  
```
这样系统就不会去进行换行符的转换了  
最后重新执行
```
$ git init
$ git add .
```
但我在实践中发现：直接运行 `$ git config --gobal core.autocrlf false`，不用删除生成的 .git 文件也可以，当然也不用执行 git init