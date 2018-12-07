---
layout: post
title: git 解决每次 push 都需要输入用户名和密码
categories: Git
description: git 解决每次 push 都需要输入用户名和密码
keywords: git, SSH, push
---

每次 `push` 代码都需要输入用户名和密码，是不是神烦？

我们在使用 `git clone` 或其他命令的时候，有时候会遇到这类问题，如图：

![](/assets/images/posts/github/fg45S7h.png)

```bash
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

出现这个问题是因为没有在 GitHub 账号添加 SSH key。

等等，我是不是跑错剧场了，不是教解决每次 `push` 都需要输入用户名和密码这个问题吗？为什么进来换另一个问题了！！

不要着急，问题千千万，但解决问题的方法有可能是同一个。

就像这次，每次 `push` 都需要输入用户名和密码，是因为当初 `clone` 时，使用的是 `HTTPS` 方式克隆的，出于安全考虑需要每次都输入用户名和密码；而出现上面的问题，是因为 `clone` 时，使用的是 `SSH` 方式克隆的，使用此方式克隆，在克隆前会先到 GitHub 的 `SSH and GPG keys` 查找是否有本机的 `SSH Key`，如果有，直接克隆；如果没有，就会报上面错误。

这下就明白了吧，两种方式各有利弊，但第二种方式只需一次设置，终身受益，快来 get 新技能吧

## 使用 `SSH` 克隆

解决方法如下：

1.  首先重新在 git 设置一下身份的名字和邮箱 (不是必要的，可有可无。如果以前设置过，请忽略；如果忘记了名字和邮箱，可以在 `https://github.com/settings/profile` 中找到)

    ```bash
    git config --global user.name "username"
    git config --global user.email "user@email.com"
    ```

    **注：username 是你 GitHub 的用户名，user@email 是你 GitHub 的邮箱**

2.  在终端输入 `ssh-keygen v-t rsa -C "username"` (注：username 为你 git 上的用户名或者注册 git 的邮箱，也就是上一步设置的)如果执行成功。返回

    `bash Generating public/private rsa key pair. Enter file in which to save the key (/c/Users/username/.ssh/id_rsa):`

    首先，说明一下，这里的 `username` 是你电脑上的用户名，默认是 `Administrator`。在这里就是设置存储地址了。我们直接按回车

	以上是 Windows 系统的存储路径, Mac 的存储路径是 `/Users/username/.ssh/id_rsa`, 这里的 `username` 也是电脑的用户名

3.  然后会出现以下两种情况的一种：

    - 如果正常运行的话，会出现:`Enter passphrase (empty for no passphrase):`，这说明你是第一次生成 SSH Key，然后我们直接回车
    - 有的时候我们可能会出现:
    ```bash
    /c/Users/Administrator/.ssh/id_rsa already exists.
    Overwrite (y/n)?
    ```
    这说明你已经设置了存储地址，我们输入 `y` 覆盖，回车
4.  上面的任意两种情况之后，会出现 `Enter passphrase (empty for no passphrase):`

	第一次输入公钥密码 (推荐不用输入，直接回车，以便在 `clone、pull、push` 等不用输入公钥密码)

	第二次出现：`Enter same passphrase again:` 再次输入公钥密码，如果第一次没有输入，这次也不用输入，直接回车，结果如图：

	![](/assets/images/posts/github/Ig5g8j.png)

	如果一切顺利的话，可以在用户主目录里找到 `.ssh` 目录，里面有 `id_rsa` 和 `id_rsa.pub` 两个文件，这两个就是 `SSH Key` 的秘钥对，`id_rsa` 是私钥，不能泄露出去，`id_rsa.pub` 是公钥，可以放心地告诉任何人。

5.  登陆 GitHub，打开 "settings"，"SSH Keys" 页面：

	然后点击 `New SSH Key`，填上任意 `Title`，在 `Key` 文本框里粘贴 `id_rsa.pub` 文件的内容，最后点击 `Add SSH Key` 如下图：

	![](/assets/images/posts/github/hC4In5.png)

6.  由于现在只是在 GitHub 上添加了 `SSH Key`，本地仓库和远程仓库需要链接一下才能激活 `SSH Key`

	![](/assets/images/posts/github/Ppgy3wl.png)

	上图红线圈出的为没有激活的 `SSH Key`

	执行 `git clone url` 或任何可以和 GitHub 关联的命令就可以激活了。当然 url 是 SSH 格式的路径，如 `git@github.com:lhajh/lhajh.github.io.git`，而不是 HTTPS 格式的路径

## 使用 `HTTPS` 克隆

[Git 配置 credential helper，并使用 Http/Https 传输](https://blog.csdn.net/u012163684/article/details/52433645)

### 方法一：

1. 创建文件存储 git 用户名和密码

   在 `%HOME%` 目录中，一般为

   - Windows：C:/Users/Administrator 或 username
   - Mac OS X： /Users/username
   - Linux： /home/username

   创建文件名为 `.git-credentials`，由于不允许直接创建以 `.` 开头的文件，所以需要借助工具创建
   这里借助 git bash 进行，进如 `%HOME%` 目录，打开 git bash 客户端

   ```bash
   vim .git-credentials
   # 输入以下内容
   https://{username}:{password}@github.com
   ```

   `{username}` 和 `{password}` 是你的 github 的账号和密码

2. 修改 Git Config

   输入如下命令：

   ```bash
   git config –-global credential.helper store
   ```

   `store` 为永久存储，当然也可以设置临时的

   ```bash
   git config –global credential.helper cache
   ```

   默认为 15 分钟，如果想设置保存时间的话，可以输入：

   ```bash
   git config credential.helper 'cache –timeout=3600'
   ```

   这样就设置了一个小时的有效时间。

   执行完后查看 `%HOME%` 目录下的 `.gitconfig` 文件，会多了一项：

   ```bash
   [credential]
   	helper = store
   ```

   重新开启 git bash 会发现 git push 时不用再输入用户名和密码

### 方法二：

1. 在命令行输入命令:

   ```bash
   git config --global credential.helper store
   ```

   这一步会在用户目录下的 `.gitconfig` 文件最后添加：

   ```bash
   [credential]
   	helper = store
   ```

2. push 代码

   push 你的代码 (git push), 这时会让你输入用户名和密码, 这一步输入的用户名密码会被记住, 下次再 push 代码时就不用输入用户名密码!这一步会在用户目录下生成文件 `.git-credential` 记录用户名密码的信息。

方法一与方法二都是创建 `.git-credential` 文件并写入用户信息，一个是手动创建，一个命令创建。
