---
layout: post
title: win7更换锁屏壁纸
categories: Windows
description: win7更换锁屏壁纸
keywords: win7, 锁屏, 壁纸
---

win7更换锁屏壁纸

1. 同时按下窗口键win+R，调出运行对话框，运行对话栏输入`Regedit`，点击确定按钮，进入注册表编辑器！
2. 进入注册表，找到以下项次HKEY_LOCAL_MACHINE/SOFTWARE/Microsoft/Windows/CurrentVersion/Authentication/LogonUI/Background.如下图所示
![](/assets/images/posts/windowsskill/c9bdddcec3fdfc03ce76999cd73f8794a4c2264c.jpg)
3. 右键单击OEMBackground，选择修改这个选项！将值修改为1，然后点击确定按钮！如下图所示
![](/assets/images/posts/windowsskill/ea85a94543a98226414e4e038982b9014b90ebcc.jpg)
4. 关闭注册表，继续Win+R，调出运行，输入`gpedit.msc`，打开组策略编辑器！
5. 按下列路径：计算机配置-管理模块-系统-登录，找到“始终使用自定义登录背景”项双击打开，如下图所示
![](/assets/images/posts/windowsskill/4e83cb628535e5ddeaeccafb75c6a7efcf1b6296.jpg)
6. 将组策略配置选择为已启动，点击确定按钮！
7. 进入X：\Windows\System32\oobe文件夹，注意X是你安装系统的盘符！找到info\backgrounds这个文件,没有的话直接新建这两层文件夹！ **将你想替换的图片命名为backgroundDefault，必须是JPg格式，注意不能修改名字，文件应少于256KB。** 同时需要管理员权限！OK，更换成功！
![](/assets/images/posts/windowsskill/9864a2315c6034a80677aec5c813495408237699.jpg)
