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

好了, 关于 node 和 brew 本人目前就踩坑这么多了, 如有不足之处还望不吝赐教

## 参考资料

- [Mac 下彻底卸载 node 和 npm - Room - CSDN 博客](https://blog.csdn.net/shiquanqq/article/details/78032943)
- [Mac 卸载 node 并使用 brew 重新安装 - 简书](https://www.jianshu.com/p/78032a310ca6)
- [brew link node 失败 - xinghuowuzhao 的博客 - CSDN 博客](https://blog.csdn.net/xinghuowuzhao/article/details/77509327)
