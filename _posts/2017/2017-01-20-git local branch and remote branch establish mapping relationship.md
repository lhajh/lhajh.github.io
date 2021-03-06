---
layout: post
title: git本地分支和远程分支建立映射关系
categories: Git
description: git本地分支和远程分支建立映射关系
keywords: git, 本地分支, 远程分支, 映射关系
---

## 目标

本博文中 git 操作的目标为建立本地分支与远程分支的映射关系（或者为跟踪关系 track）。这样使用 `git pull` 或者 `git push` 时就不必每次都要指定从远程的哪个分支拉取合并和推送到远程的哪个分支了。

## 查看本地分支与远程分支的映射关系

使用以下命令（注意是双v）：
```
git branch -vv
```
可以获得如下信息：

![](/assets/images/posts/github/36bgdf.png)

可以看到分支 addFile 没有和远程分支建立任何映射，此时若执行如下拉取命令则不成功（因为 git 此时不知道拉取哪个远程分支和本地分支合并）：

![](/assets/images/posts/github/bgdf456.png)

同理，若此时执行如下推送命令同样不成功： 

![](/assets/images/posts/github/3ny5TY.png)

## 建立本地分支与远程分支的映射关系

建立当前分支与远程分支的映射关系:
```
git branch -u origin/addFile
或者使用命令：
git branch --set-upstream-to origin/addFile
```
得到结果如下： 

![](/assets/images/posts/github/2I8jt5.png)

查看当前本地分支与远程分支的映射关系结果如下： 

![](/assets/images/posts/github/3tyjk.png)

此时再次拉取，成功信息如下： 

![](/assets/images/posts/github/vb4rt.png)

再次推送，成功信息如下： 

![](/assets/images/posts/github/jj3ht.png)

## 撤销本地分支与远程分支的映射关系

撤销本地分支与远程分支的映射关系
```
git branch --unset-upstream
```
使用git branch -vv得到结果如下： 

![](/assets/images/posts/github/as3jy.png)

可以看到本地分支与远程分支的映射关系已经撤销。

## 问题思考：本地分支只能跟踪远程的同名分支吗？

答案是否定的，本地分支可以与远程不同名的分支建立映射关系，实验时分支结构如下：

![](/assets/images/posts/github/lt56we.png)

可以使本地分支 addFile 和远程分支 editFile 建立映射关系：

![](/assets/images/posts/github/zx3u8.png)

并且此时可以把本地分支addFile提交到远程分支editFile分支中去：

![](/assets/images/posts/github/mge8.png)

本文均转载自 [Git branch upstream](http://blog.csdn.net/tterminator/article/details/78108550)