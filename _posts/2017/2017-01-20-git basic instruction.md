---
layout: post
title: git基本指令
categories: Blog
description: git基本指令
keywords: git, 指令
---

git 基本指令

## git 管理项目
* github:
	* 创建仓库
* 本地：
	* git config --global  user.name "xxx"  设置 github 账号
	* git config --global user.email "yyy"  设置 github 邮箱
	* 为项目添加 .gitignore , 并指定要忽略的文件/文件夹
	* 为项目添加 README.md 文件
	* git init  初始化本地仓库

## 将本地文件添加到 github
* git add * 从工作区添加到缓存区
* git commit -m "xxx" 从缓存区提交到版本区(本地)
* git remote add origin url  与 github 远程仓库进行关联
* git push origin master 将本地版本推送到远程仓库

## 从 github 拉取文件
* git pull origin master 将远程仓库的更新拉取到本地仓库

## 从 github 克隆文件
* git clone url 将远程仓库克隆到本地

## 提交到分支仓库
* git branch  	查询本地分支，当前分支前面会标一个 `*` 号。
* git branch -r 查询远程分支
* git branch -a 查询所有分支，包括本地和远程
* git branch xxx  	创建分支 xxx
* git checkout xxx  	切换到指定分支 xxx
* git checkout -b xxx 创建并切换到xxx分支，相当于前面两步
* git push origin xxx  	将当前分支 push 到远程的 xxx 分支
	* 远程没有分支名会自动创建
* git clone -b xxx url 	克隆某一个指定分支 xxx

## git 拉取远程分支并创建本地分支
1. 查看远程分支

	```
	git branch -r
	```
2. 拉取远程分支并创建本地分支

	方法一：

	```
	git checkout -b 本地分支名x origin/远程分支名x
	```

	使用该方式会在本地新建分支x，并自动切换到该本地分支x。采用此种方法建立的本地分支会和远程分支建立映射关系。

	方法二：

	```
	git fetch origin 远程分支名x:本地分支名x
	```

	使用该方式会在本地新建分支x，但是不会自动切换到该本地分支x，需要手动 checkout。采用此种方法建立的本地分支不会和远程分支建立映射关系。

3. 合并分支

	当拉取远程分支并创建本地分支后，可以使用 `git branch` 查看本地分支

	```
	$ git branch
	* 本地分支名x
  	master
	```
	现在我们可以对本地分支x作出修改并提交
	```
	$ git add readme.txt 
	$ git commit -m "branch test"
	[dev fec145a] branch test
	1 file changed, 1 insertion(+)
	```
	现在，本地分支x的工作完成，我们就可以切换回 master 分支：
	```
	$ git checkout master
	```
	切换回 master 分支后，再查看一下 readme.txt 文件，刚才添加的内容不见了！因为那个提交是在本地分支x 上，而 master 分支此刻的提交点并没有变

	现在，我们把本地分支x 的工作成果合并到 master 分支上：
	```
	$ git merge dev
	Updating d17efd8..fec145a
	Fast-forward
	readme.txt |    1 +
	1 file changed, 1 insertion(+)
	```
	`git merge` 命令用于合并指定分支到当前分支。合并后，再查看 readme.txt 的内容，就可以看到，和本地分支x 的最新提交是完全一样的。

	注意到上面的 Fast-forward 信息，Git 告诉我们，这次合并是“快进模式”，也就是直接把 master 指向本地分支x 的当前提交，所以合并速度非常快。当然，也不是每次合并都能 Fast-forward

4. 删除分支

	合并完成后，就可以放心地删除本地分支x 了：
	```
	$ git branch -d 本地分支x
	```
	删除后，查看 branch，就只剩下 master 分支了：
	```
	$ git branch
	* master
	```
	因为创建、合并和删除分支非常快，所以 Git 鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在 master 分支上工作效果是一样的，但过程更安全。


5. 本地分支和远程分支建立映射关系的作用

	参见 [Git branch upstream](https://lhajh.github.io/blog/2017/01/20/git%E6%9C%AC%E5%9C%B0%E5%88%86%E6%94%AF%E5%92%8C%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF%E5%BB%BA%E7%AB%8B%E6%98%A0%E5%B0%84%E5%85%B3%E7%B3%BB.html)


## 阮一峰关于git 5个重要（基本）的指令
网址： http://www.ruanyifeng.com/blog/2014/06/git_remote.html