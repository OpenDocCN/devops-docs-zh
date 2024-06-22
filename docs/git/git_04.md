# git-init

> 原文： [`git-scm.com/docs/git-init`](https://git-scm.com/docs/git-init)
> 
> 贡献者：[honglyua](https://github.com/honglyua)

## 名称

git-init - 创建一个空的 Git 存储库或重新初始化现有存储库

## 概要

```
git init [-q | --quiet] [--bare] [--template=<template_directory>]
	  [--separate-git-dir <git dir>]
	  [--shared[=<permissions>]] [directory]
```

## 描述

此命令创建一个空的 Git 存储库 - 基本上是一个`.git`目录，其中包含`objects`，`refs/heads`，`refs/tags`和模板文件的子目录。还创建了引用主分支 HEAD 的初始`HEAD`文件。

如果设置了`$GIT_DIR`环境变量，则它指定要使用的路径而不是`./.git`作为存储库的基础。

如果通过`$GIT_OBJECT_DIRECTORY`环境变量指定了对象存储目录，则在下面创建 sha1 目录 - 否则使用默认的`$GIT_DIR/objects`目录。

在现有存储库中运行 _git init_ 是安全的。它不会覆盖已存在的东西。重新运行 _git init_ 的主要原因是获取新添加的模板（或者如果给出了--separate-git-dir，则将存储库移动到另一个地方）。

## 选项

```
 -q 
```

```
 --quiet 
```

仅打印错误和警告消息;所有其他输出都将被抑制。

```
 --bare 
```

创建一个裸存储库。如果未设置`GIT_DIR`环境，则将其设置为当前工作目录。

```
 --template=<template_directory> 
```

指定将使用模板的目录。 （参见下面的“模板目录”部分。）

```
 --separate-git-dir=<git dir> 
```

不是将存储库初始化为`$GIT_DIR`或`./.git/`的目录，而是在其中创建包含实际存储库路径的文本文件。此文件充当与文件系统无关的 Git 符号链接到存储库。

如果这是重新初始化，则存储库将移动到指定的路径。

```
 --shared[=(false|true|umask|group|all|world|everybody|0xxx)] 
```

指定要在多个用户之间共享 Git 存储库。这允许属于同一组的用户进入该存储库。指定时，将设置配置变量“core.sharedRepository”，以便使用请求的权限创建`$GIT_DIR`下的文件和目录。未指定时，Git 将使用 umask（2）报告的权限。

该选项可以具有以下值，如果没有给出值，则默认为 _group_：

```
 umask (or false) 
```

使用 umask（2）报告的权限。未指定`--shared`时，使用默认值。

```
 group (or true) 
```

使存储库可写，（和 g + sx，因为 git group 可能不是所有用户的主要组）。这用于放宽其他安全的 umask（2）值的权限。请注意，umask 仍然适用于其他权限位（例如，如果 umask 是 _0022_ ，则使用 _group_ 将不会删除其他（非组）用户的读取权限）。有关如何准确指定存储库权限的信息，请参见 _0xxx_ 。

```
 all (or world or everybody) 
```

与 _group_ 相同，但使所有用户都可以读取存储库。

```
 0xxx 
```

_0xxx_ 是一个八进制数，每个文件都有模式 _0xxx_ 。 _0xxx_ 将覆盖用户的 umask（2）值（并且不仅松开 _group_ 和 _all_ 的权限）。 _0640_ 将创建一个可读取组的存储库，但不能写入组或其他人可访问的存储库。 _0660_ 将创建一个对当前用户和组可读写的 repo，但其他人无法访问。

默认情况下，配置标志`receive.denyNonFastForwards`在共享存储库中启用，因此您无法强制执行非快进推送。

如果您提供 _ 目录 _，则命令在其中运行。如果此目录不存在，则将创建该目录。

## 模板目录

模板目录中名称不以点开头的文件和目录将在创建后复制到`$GIT_DIR`。

模板目录将是以下之一（按顺序）：

*   使用`--template`选项给出的参数;

*   `$GIT_TEMPLATE_DIR`环境变量的内容;

*   `init.templateDir`配置变量;要么

*   默认模板目录：`/usr/share/git-core/templates`。

默认模板目录包括一些目录结构，建议“排除模式”（参见 [gitignore [5]](https://git-scm.com/docs/gitignore) ）和示例钩子文件。

默认情况下，示例钩子均已禁用。要启用其中一个示例钩子，请通过删除其`.sample`后缀来重命名它。

有关钩子执行的更多常规信息，请参见 [githooks [5]](https://git-scm.com/docs/githooks) 。

## 例子

```
 基于存量代码，初始化一个新 git 库
```

```
$ cd /path/to/my/codebase
$ git init      (1)
$ git add .     (2)
$ git commit    (3)
```

1.  创建一个/path/to/my/codebase/.git 目录。

2.  将所有现有文件添加到索引中。

3.  将原始状态记录为历史记录中的第一个提交。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件