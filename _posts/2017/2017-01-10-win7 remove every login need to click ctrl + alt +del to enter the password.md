---
layout: post
title: win7 去掉每次登录都需要按 crtl+alt+del 才能输入密码
categories: Windows
description: win7 去掉每次登录都需要按 crtl+alt+del 才能输入密码
keywords: win7, 登录, crtl+alt+del, 密码
---

win7 去掉每次登录都需要按 crtl+alt+del 才能输入密码

在每次登入系统时都需要按 Ctrl+Alt+Delete 组合键，显得非常麻烦，而这可能原因为组策略设置开机需要按组合键登入，安全登入勾选了要求按组合键登入，以下详解方法。

1. 同时按下窗口键 `win+R`，调出运行对话框，运行对话栏输入 `control userpasswords2`→【确定】
2. 点击【高级】→不勾选【要求用户按 Ctrl-Alt-Delete(R)】→【确定】

![](/assets/images/posts/windowsskill/20170824164909.png)

