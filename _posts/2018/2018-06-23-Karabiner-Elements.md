---
layout: post
title: Karabiner-Elements
categories: Mac
description: Karabiner-Elements
keywords: mac, Karabiner-Elements
---

由于 Mac 的键盘和主流布局不一致，许多人都会遇到外接键盘布局不适应的情况；即便是 Mac 的内置键盘，也有人觉得其设计不够合理，不符合自己的工作习惯。好在键盘和使用者之间是可以磨合的，[Karabiner-Elements](https://pqrs.org/osx/karabiner/) 就是 Mac 上一款非常好用的开源改键利器，能让键盘顺着你的意愿来工作。

## 基本介绍

[安装](https://pqrs.org/osx/karabiner/document.html#usage)完后会有两个应用，一个是 `Karabiner-Elements`，另一个是 `Karabiner-EventViewer`。

第一个是改键用的主程序，第二个是键鼠事件的监听器，当你不知道目前键盘的某个按键对应的是哪个键位, 可以使用这个查看。

## Karabiner-Elements

### [Simple Modifications](https://pqrs.org/osx/karabiner/document.html#configuration-simple-modifications)

简单改键: 将某一个键位更改为另一个键位

用法:

1. 先在 `Target Device` 中选择需要修改的键盘, 可以选择 `For all devices` 或只针对某一个键盘
2. 点击 `Add item` 来增加一对按键映射
3. `From key` 是物理按键, `To key` 是你想映射的按键
4. 如果不知道键盘上某一个物理按键的名字, 可以在 `Karabiner-EventViewer` 查询
5. 修改完后也可以在 `Karabiner-EventViewer` 中查看是否修改成功

下图为我自己外接键盘的一些基本改键, 基本和 Mac 自带键盘键位保持一致

![](/assets/images/posts/mac/808112241.png)

### Function Keys

这个主要是修改功能键 `F1` - `F12` 键位的

用法也是先选择需要修改的键盘, 再修改物理按键为映射按键

注: **如果选中左下角的 `Use all F1, F2, etc. keys as standard function keys`, 则功能键使用默认功能, 上面设置的映射键位不会生效**

### [Complex Modifications](https://pqrs.org/osx/karabiner/document.html#configuration-complex-modifications)

复杂改键, 修改组合键位来实现自定义事件

这里主要介绍一下用 `Karabiner-Elements` 打造 `Vim` 风格的方向键。也许你对这个词感到陌生，还会奇怪，干嘛放着 MacBook 上好好的方向键不用？其实，Mac 自带键盘的方向键位于右下角，打字时把爪子挪过去点很不方便，而 `Vim` 式的操作中，可以挪用 `HJKL` 四枚按键，调教一下充当方向键，打字时手掌都不用抬起，就能实现光标的移动，很快捷。

1. 点击左下角的 `Add rule` 来添加组合键
2. 软件自带的 `Vim` 风格的方向键为 `right_command + hjkl` 来代替方向键, 在 `Examples` 中可以找到, 点击右侧的 `Enable` 即可加入到组合键位中, 但实际使用起来有点费劲
3. 在 [官网的 complex_modifications](https://pqrs.org/osx/karabiner/complex_modifications/#emulation-modes) 找到 `Vi Mode (rev 5)`, 展开后有详细的组合键位说明. 点击右侧的 `import` 来下载下来, 并导入到软件中.
4. 重复第一步和第二步, 你可以选择 `s` 作为触发键, 也可以选择 `d` 作为触发键, 或者两者都作为触发键
5. 无法正常使用? 其实官网展开后的键位使用说明最后有这么一句话: Please change your `simultaneous_threshold_milliseconds` setting in Karabiner-Elements → Complex Modifications → Parameters; A value between 150 and 500 is recommended for this mode. 说白了就是两个按键之间的等待值, 默认是 `50ms`, 但一般人没法做到组合键这么小的间隔, 就只能调高等待值, 我一般设置为 `300`, 既不影响打字, 也不影响组合键
6. 如果不想要某一个组合键, 点击右侧的 `Remove` 即可移除. 下面的 `Up` 和 `Down` 可以排序

### [Devices](https://pqrs.org/osx/karabiner/document.html#configuration-devices)

显示此 Mac 中当前连接了哪些键鼠设备

如果连接了外部键盘，也可以禁用 MacBook 内置键盘。

### Virtual Keyboard

### Profiles
