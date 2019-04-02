---
layout: post
title: 在 Finder 中使用 VSCode 打开文件或文件夹
categories: [vscode]
description: 在 Finder 中使用 VSCode 打开文件或文件夹
keywords: vs-code
---

1. 找到自动操作(小机器人), 点击新建文稿或 `cmd + n`, 选择快速操作, 点击选取. 如图：

    ![](/assets/images/posts/vscode/9040244655.png)

2. 修改右上为下图:

    ![](/assets/images/posts/vscode/904020253.png)

3. 在左侧面板选择 `实用工具`, 然后找到 `运行 Shell 脚本`, 把它拽到右侧面板里

4. 右侧 传递输入 修改为: `作为自变量`, 然后修改 Shell 脚本:

    ```zsh
    for f in "$@"
    do
        open -a "Visual Studio Code" "$f"
    done
    ```

    如下图:

    ![](/assets/images/posts/vscode/9040251210.png)

5. 之后保存 `cmd + s`. 保存为 `Open With VSCode`. 名字可以随便起.

6. 选择电脑上任意一个文件或文件夹, 点击右键 `服务`, 就可以看到 `Open With VSCode` 菜单

7. 为了避免每次都要右键选择服务, 可以为这个服务设置一个快捷键. 具体设置看[这里](https://lhajh.github.io/mac/2018/04/25/Iterm2-usage-and-skills.html#%E6%96%B9%E6%B3%95%E4%BA%8C-iterm-%E8%87%AA%E5%B8%A6%E7%9A%84%E6%9C%8D%E5%8A%A1)

8. 参考资料: [Open With VSCode](https://blog.csdn.net/u013069892/article/details/83147239)
