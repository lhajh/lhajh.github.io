---
layout: post
title: 如何科学使用 GitHub 服务
categories: [GitHub]
description: 如何科学使用 GitHub 服务
keywords: GitHub
---

首先说明一下, 这里的 `科学` 是 `如何科学上网` 中的意思, 本篇并不介绍如何使用 `GitHub` 服务

1. 打开 [Dns检测-Dns查询 - 站长工具](http://tool.chinaz.com/dns)

2. 在检测输入栏中输入需要科学使用的网址, 如: [github.com](http://github.com)

3. 把检测列表里可以 `ping` 通的 `IP` 输入到 `hosts` 里，并对应写上 `github` 官网域名, 如: `192.30.253.118 gist.github.com`

4. 各个系统 host 所在文件夹:

    ```
    Windows 系统位于 C:\Windows\System32\drivers\etc\

    Android（安卓）系统hosts位于 /etc/

    Mac（苹果电脑）系统hosts位于 /etc/

    iPhone（iOS）系统hosts位于 /etc/

    Linux 系统 hosts 位于 /etc/

    绝大多数 Unix 系统都是在 /etc/
    ```

5. 如果修改后提示没有管理员权限, 可以拷贝 `hosts` 文件到别的目录下, 修改完后替换即可; 或者直接使用命令行加 `sudo` 修改

6. 如果 `hosts` 没有生效:

    ```
    Windows
    开始 -> 运行 -> 输入 cmd -> 在 CMD 窗口输入: ipconfig /flushdns

    Linux
    终端输入: sudo rcnscd restart

    Mac OS X
    终端输入: sudo killall -HUP mDNSResponder

    其他: 断网, 再开网;
    终极方法: 重启机器;
    ```

7. 一些常用的 `hosts`:

    204.232.175.78 [documentcloud.github.io](http://documentcloud.github.io/)

    207.97.227.239 [github.com](https://github.com/)

    192.30.253.118 [gist.github.com](https://gist.github.com/)

    192.30.253.119 [gist.github.com](https://gist.github.com/)

    107.22.3.110 [githubstatus.com](https://www.githubstatus.com/)

    204.232.175.78 [services.github.com](https://services.github.com/)

8. 参考资料: [GitHub 在国内无法访问之后，如何自救？ - 知乎](https://www.zhihu.com/question/20732532)
