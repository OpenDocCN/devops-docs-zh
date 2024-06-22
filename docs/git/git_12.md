# git-mv

> 原文： [`git-scm.com/docs/git-mv`](https://git-scm.com/docs/git-mv)

## 名称

git-mv - 移动或重命名文件，目录或符号链接

## 概要

```
git mv <options>…​ <args>…​
```

## 描述

移动或重命名文件，目录或符号链接。

```
git mv [-v] [-f] [-n] [-k] <source> <destination>
git mv [-v] [-f] [-n] [-k] <source> ... <destination directory>
```

在第一种形式中，它将<source>重命名为<destination>，<source>必须存在，并且可以是文件，符号链接或目录。在第二种形式中，最后一个参数必须是现有目录;给定的源将被移动到此目录中。

成功完成后会更新索引，但仍必须提交更改。

## 选项

```
 -f 
```

```
 --force 
```

即使目标存在，也强制重命名或移动文件

```
 -k 
```

跳过移动或重命名可能导致错误情况的操作。当源既不存在也不由 Git 控制时，将发生错误，或者除非给出`-f`，覆盖现有文件时也会发生错误。

```
 -n 
```

```
 --dry-run 
```

没做什么;只显示会发生什么

```
 -v 
```

```
 --verbose 
```

在移动文件时报告文件的名称。

## 子模

使用 gitfile 移动子模块（这意味着它们使用 Git 1.7.8 或更高版本克隆）将更新 gitfile 和 core.worktree 设置以使子模块在新位置工作。它还将尝试更新 [gitmodules [5]](https://git-scm.com/docs/gitmodules) 文件中的子模块<name>.path 设置并暂存该文件（除非使用-n）。

## BUGS

每次超级项目更新移动填充的子模块时（例如，当在移动之前和之后切换提交时），旧的子模块检出将保留在旧位置，并且空目录将出现在新位置。要在新位置再次填充子模块，用户必须在之后运行“git submodule update”。删除旧目录只有在使用 gitfile 时才是安全的，否则子模块的历史记录也将被删除。当实现递归子模块更新时，这两个步骤都将过时。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件