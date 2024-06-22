# git-verify-pack 

> 原文： [`git-scm.com/docs/git-verify-pack`](https://git-scm.com/docs/git-verify-pack)

## 名称

git-verify-pack - 验证打包的 Git 存档文件

## 概要

```
git verify-pack [-v|--verbose] [-s|--stat-only] [--] <pack>.idx …​
```

## 描述

读取给定的 idx 文件，用于使用 _git pack-objects_ 命令创建的打包 Git 存档，并验证 idx 文件和相应的包文件。

## OPTIONS

```
 <pack>.idx …​ 
```

要验证的 idx 文件。

```
 -v 
```

```
 --verbose 
```

验证包后，显示包中包含的对象列表和 delta 链长度的直方图。

```
 -s 
```

```
 --stat-only 
```

不要验证包装内容;仅显示三角链长度的直方图。使用`--verbose`，还会显示对象列表。

```
 -- 
```

不要将任何更多的参数解释为选项。

## 输出格式

指定-v 选项时，使用的格式为：

```
SHA-1 type size size-in-packfile offset-in-packfile
```

对于未在包中进行分层的对象，以及

```
SHA-1 type size size-in-packfile offset-in-packfile depth base-SHA-1
```

对于已经完成的对象。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件