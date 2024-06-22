# git-update-server-info

> 原文： [`git-scm.com/docs/git-update-server-info`](https://git-scm.com/docs/git-update-server-info)

## 名称

git-update-server-info - 更新辅助信息文件以帮助虚拟服务器

## 概要

```
git update-server-info [--force]
```

## 描述

没有动态包生成的哑服务器必须在$ GIT_DIR / info 和$ GIT_OBJECT_DIRECTORY / info 目录中包含一些辅助信息文件，以帮助客户端发现服务器具有哪些引用和包。此命令生成此类辅助文件。

## OPTIONS

```
 -f 
```

```
 --force 
```

从头开始更新信息文件。

## OUTPUT

目前，该命令会更新以下文件。请参阅 [gitrepository-layout [5]](https://git-scm.com/docs/gitrepository-layout) ，了解它们的用途：

*   对象/信息/包

*   信息/裁判

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件