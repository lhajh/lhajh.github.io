---
layout: post
title: Mac 使用 Homebrew 安装 node
categories: Mac
description: Mac 使用 Homebrew 安装 node
keywords: mac
---

最近 vue-cli 升级 3.0 了, 就想着尝试一下, 发现需要 Node.js 8.9 或更高版本 (推荐 8.11.0+), 而自己本地 node 需要升级. 这才想起来当初是在官网下载的 pkg 版本安装的, 这个怎么升级? Windows 的话覆盖安装就可以了, 这个可以吗? 网上查了一下, 大部分都推荐使用 `n` 这个包来升级 node. 突然想到可以试试 Homebrew, 以前一直听人说这个管理软件非常棒, 正好趁着升级 node 搞一下

## [安装 Homebrew](https://lhajh.github.io/mac/2018/10/27/Mac-terminal-software-installation-tool-Homebrew.html)

既然现在电脑上已经有 node 了, 那下一步肯定是先卸载之了

## 卸载 node

### homebrew 安装的

直接一条命令

    brew uninstall node

### 官网下载 pkg 安装包的

一条命令

    sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}

### 其他路子安装的

```
lsbom -f -l -s -pf /var/db/receipts/org.nodejs.pkg.bom \| while read i; do sudo rm /usr/local/${i}done

sudo rm -rf /usr/local/lib/node \

    /usr/local/lib/node_modules \

    /var/db/receipts/org.nodejs.*
```

Tips：

这些东西删完了，node 就算删除了。

但是还有好多基于 node 安装的一堆软件和命令行工具，也需要重新安装，例如 react-native, supervisor,pm2 etc

需要删除 `/usr/local/bin` 下面相关的文件，其实它们只是些软连接，正主都在 `/usr/local/lib/node_modules/` 目录下。

在之前的步骤中，已经被删除了，但是按 tab 键还能找到，就是因为这些软连接还存在

## 安装 node

    brew install node

安装后有可能在终端中输入 node 找不到该命令，执行如下命令

    brew link node

得到如下结果

```zsh
Linking /usr/local/Cellar/node/11.3.0...
Error: Could not symlink include/node/common.gypi
Target /usr/local/include/node/common.gypi
already exists. You may want to remove it:
  rm '/usr/local/include/node/common.gypi'

To force the link and overwrite all conflicting files:
  brew link --overwrite node

To list all files that would be deleted:
  brew link --overwrite --dry-run node
localhost:wkdir meng$ brew link --overwrite node
Linking /usr/local/Cellar/node/11.3.0...
Error: Could not symlink include/node/common.gypi
/usr/local/include/node is not writable.
```

上面错误指出有一些文件已经存在, 如果 link node 会冲突, 需要你去删除这些文件

执行 `brew link --overwrite node` 会强制链接并覆盖所有冲突文件

执行 `brew link --overwrite --dry-run node` 会列出所有将被删除的文件

我们可以先执行一下 `brew link --overwrite --dry-run node` 看看需要删除哪些文件

```zsh
Would remove:
/usr/local/include/node/common.gypi
/usr/local/include/node/config.gypi
/usr/local/include/node/libplatform/libplatform-export.h
/usr/local/include/node/libplatform/libplatform.h
/usr/local/include/node/libplatform/v8-tracing.h
/usr/local/include/node/node.h
..........
# 由于需要删除的文件太多, 就不都列出来了
```

此时你可以选择手动依次找到这些文件并删除之(过来人告诉你贼拉麻烦)或者执行 `brew link --overwrite node` 强制覆盖冲突文件

删除上面的冲突的文件再运行

    brew link node

直到没有以下类似错误出现为止

```
Target /usr/local/***
already exists. You may want to remove it:
  rm '/usr/local/***'
```

但可能还有以下的错误:

1. 报错一：

    ```
    Linking /usr/local/Cellar/node/11.3.0... 
    Error: Could not symlink share/doc/node/gdbinit
    /usr/local/share/doc/node is not writable.
    ```

    解决方案：

    `sudo chown -R $(whoami) /usr/local/share`

2. 报错二：

    ```
    Linking /usr/local/Cellar/node/11.3.0... 
    Error: Could not symlink lib/dtrace/node.d
    /usr/local/lib/dtrace is not writable.
    ```

    解决方案：

    `sudo chown -R $(whoami) /usr/local/lib/dtrace`

## 安装指定版本的 node

mac 环境下,使用 homebrew 安装的 node, 默认是最高版本

那么使用 homebrew 如何安装指定版本的 node 呢?

  1. 如果之前使用 `brew install node` 安装过 node, 需要先执行 `brew unlink node` 来'解绑' node
  2. 查找可用的node版本 `brew search node`
  3. 安装你需要的版本, 比如 `brew install node@8`
  4. 然后 `brew link node@8`, 这一步可能会报错, 按照提示执行命令就 ok 了, 比如我最后执行的是 `brew link --overwrite --force node@8`. 还遇到过在 `~/.zshrc` 中添加环境变量: `export PATH="/usr/local/opt/node@8/bin:$PATH"`
  5. `node -v` 不出意外, 就安装好了你想要的 node 版本

好了, 关于 node 和 brew 本人目前就踩坑这么多了, 如有不足之处还望不吝赐教

## 参考资料

- [Mac 下彻底卸载 node 和 npm - Room - CSDN 博客](https://blog.csdn.net/shiquanqq/article/details/78032943)
- [Mac 卸载 node 并使用 brew 重新安装 - 简书](https://www.jianshu.com/p/78032a310ca6)
- [brew link node 失败 - xinghuowuzhao 的博客 - CSDN 博客](https://blog.csdn.net/xinghuowuzhao/article/details/77509327)
- [homebrew 安装指定版本 node - 简书](https://www.jianshu.com/p/c5c298486dbd)
