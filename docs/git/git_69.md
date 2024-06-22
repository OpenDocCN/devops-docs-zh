# git-count-objects

> 原文： [`git-scm.com/docs/git-count-objects`](https://git-scm.com/docs/git-count-objects)

## 名称

git-count-objects - 计算解压缩的对象数及其磁盘消耗

## 概要

```
git count-objects [-v] [-H | --human-readable]
```

## 描述

这会计算解压缩的目标文件的数量和它们消耗的磁盘空间，以帮助您确定何时是重新打包的好时机。

## OPTIONS

```
 -v 
```

```
 --verbose 
```

报告更详细：

count：松散物体的数量

size：松散对象消耗的磁盘空间，以 KiB 为单位（除非指定了-H）

in-pack：包内对象的数量

size-pack：包消耗的磁盘空间，以 KiB 为单位（除非指定了-H）

prune-packable：包中也存在的松散物体的数量。可以使用`git prune-packed`修剪这些对象。

garbage：对象数据库中既不是有效的松散对象也不是有效包的文件数

size-garbage：垃圾文件占用的磁盘空间，以 KiB 为单位（除非指定-H）

alternate：备用对象数据库的绝对路径;可能会出现多次，每条路径一行。请注意，如果路径包含不可打印的字符，则它可能被双引号括起来并包含 C 样式的反斜杠转义序列。

```
 -H 
```

```
 --human-readable 
```

以人类可读格式打印尺寸

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件