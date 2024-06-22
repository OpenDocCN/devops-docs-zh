# gitmodules

> 原文： [`git-scm.com/docs/gitmodules`](https://git-scm.com/docs/gitmodules)

## 名称

gitmodules - 定义子模块属性

## 概要

$ GIT_WORK_DIR / .gitmodules

## 描述

`.gitmodules`文件位于 Git 工作树的顶级目录中，是一个文本文件，其语法与 [git-config [1]](https://git-scm.com/docs/git-config) 的要求相匹配。

该文件包含每个子模块的一个子部分，子部分值是子模块的名称。该名称设置为添加子模块的路径，除非使用 _git 子模块添加 _ 的`--name`选项进行自定义。每个子模块部分还包含以下必需的键：

```
 submodule.<name>.path 
```

定义相对于 Git 工作树的顶级目录的路径，其中预期子模块将被检出。路径名称不得以`/`结尾。所有子模块路径在.gitmodules 文件中必须是唯一的。

```
 submodule.<name>.url 
```

定义可以从中克隆子模块存储库的 URL。这可以是准备传递给 [git-clone [1]](https://git-scm.com/docs/git-clone) 的绝对 URL，或者（如果它以./或../开头）相对于超级项目的原始存储库的位置。

此外，还有许多可选键：

```
 submodule.<name>.update 
```

定义命名子模块的默认更新过程，即超级项目中“git submodule update”命令如何更新子模块。这仅由`git submodule init`用于初始化同名的配置变量。这里允许的值是 _ 检出 _， _rebase_ ，_ 合并 _ 或 _ 无 _。有关其含义，请参阅 [git-submodule [1]](https://git-scm.com/docs/git-submodule) 中 _update_ 命令的说明。请注意，出于安全原因，此处有意忽略 _！命令 _ 表单。

```
 submodule.<name>.branch 
```

用于跟踪上游子模块中的更新的远程分支名称。如果未指定该选项，则默认为 _master_ 。 `.`的特殊值用于指示子模块中分支的名称应与当前存储库中当前分支的名称相同。有关详细信息，请参阅 [git-submodule [1]](https://git-scm.com/docs/git-submodule) 中的`--remote`文档。

```
 submodule.<name>.fetchRecurseSubmodules 
```

此选项可用于控制此子模块的递归获取。如果此选项也存在于超级项目的.git / config 中的子模块条目中，则该设置将覆盖.gitmodules 中的设置。通过使用“git fetch”和“git pull”的“ - [no-] recurse-submodules”选项，可以在命令行上覆盖这两个设置。

```
 submodule.<name>.ignore 
```

定义在什么情况下“git status”和 diff 系列将子模块显示为已修改。支持以下值：

```
 all 
```

子模块永远不会被视为已修改（但仍将显示在状态输出中并在提交时提交）。

```
 dirty 
```

将忽略对子模块工作树的所有更改，仅考虑子模块的 HEAD 与其在超级项目中的记录状态之间的已提交差异。

```
 untracked 
```

只有子模块中未跟踪的文件才会被忽略。将显示对跟踪文件的承诺差异和修改。

```
 none 
```

不会忽略对子模块的修改，显示所有已提交的差异以及对已跟踪和未跟踪文件的修改。这是默认选项。

如果此选项也存在于超级项目的.git / config 中的子模块条目中，则该设置将覆盖.gitmodules 中的设置。

可以使用“--ignore-submodule”选项在命令行上覆盖这两个设置。 _git 子模块 _ 命令不受此设置的影响。

```
 submodule.<name>.shallow 
```

设置为 true 时，除非用户明确要求非浅层克隆，否则此子模块的克隆将作为浅层克隆（历史深度为 1）执行。

## 例子

请考虑以下.gitmodules 文件：

```
[submodule "libfoo"]
	path = include/foo
	url = git://foo.com/git/lib.git
```

```
[submodule "libbar"]
	path = include/bar
	url = git://bar.com/git/lib.git
```

这定义了两个子模块，`libfoo`和`libbar`。这些预期将在路径 _include / foo_ 和 _include / bar_ 中检出，并且对于这两个子模块，指定了可用于克隆子模块的 URL。

## 也可以看看

[git-submodule [1]](https://git-scm.com/docs/git-submodule) [git-config [1]](https://git-scm.com/docs/git-config)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件