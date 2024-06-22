# git-reflog

> 原文： [`git-scm.com/docs/git-reflog`](https://git-scm.com/docs/git-reflog)

## 名称

git-reflog - 管理 reflog 信息

## 概要

```
git reflog <subcommand> <options>
```

## 描述

该命令采用各种子命令，并根据子命令使用不同的选项：

```
git reflog [show] [log-options] [<ref>]
git reflog expire [--expire=<time>] [--expire-unreachable=<time>]
	[--rewrite] [--updateref] [--stale-fix]
	[--dry-run | -n] [--verbose] [--all [--single-worktree] | <refs>…​]
git reflog delete [--rewrite] [--updateref]
	[--dry-run | -n] [--verbose] ref@{specifier}…​
git reflog exists <ref>
```

引用日志或“reflogs”记录在本地存储库中更新分支和其他引用的提示时。 Reflog 在各种 Git 命令中很有用，用于指定引用的旧值。例如，`HEAD@{2}`表示“HEAD 过去两次移动的地方”，`master@{one.week.ago}`表示“主要用于指向一周前的本地存储库”，依此类推。有关详细信息，请参阅 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 。

此命令管理 reflog 中记录的信息。

“show”子命令（在没有任何子命令的情况下也是默认命令）显示命令行中提供的引用的日志（或默认情况下为`HEAD`）。 reflog 包含所有最近的操作，此外`HEAD` reflog 记录分支切换。 `git reflog show`是`git log -g --abbrev-commit --pretty=oneline`的别名;有关详细信息，请参阅 [git-log [1]](https://git-scm.com/docs/git-log) 。

“expire”子命令修剪旧的 reflog 条目。超过`expire`时间的条目，或者早于`expire-unreachable`时间且当前提示无法访问的条目将从 reflog 中删除。这通常不会被最终用户直接使用 - 相反，请参阅 [git-gc [1]](https://git-scm.com/docs/git-gc) 。

“delete”子命令从 reflog 中删除单个条目。其参数必须是 _ 精确 _ 条目（例如“`git reflog delete master@{2}`”）。最终用户通常也不直接使用此子命令。

“exists”子命令检查 ref 是否具有 reflog。如果 reflog 存在则退出为零状态，如果不存在则退出为非零状态。

## OPTIONS

### `show`的选项

`git reflog show`接受`git log`接受的任何选项。

### `expire`的选项

```
 --all 
```

处理所有引用的 reflog。

```
 --single-worktree 
```

默认情况下，指定`--all`时，将处理来自所有工作树的 reflog。此选项仅将处理限制为来自当前工作树的 reflog。

```
 --expire=<time> 
```

修剪早于指定时间的条目。如果未指定此选项，则到期时间取自配置设置`gc.reflogExpire`，后者默认为 90 天。 `--expire=all`修剪条目，不论其年龄; `--expire=never`关闭可达条目的修剪（但参见`--expire-unreachable`）。

```
 --expire-unreachable=<time> 
```

修剪早于`&lt;time&gt;`的条目，无法从分支的当前提示访问。如果未指定此选项，则到期时间取自配置设置`gc.reflogExpireUnreachable`，后者默认为 30 天。 `--expire-unreachable=all`修剪无法访问的条目，无论其年龄如何; `--expire-unreachable=never`关闭无法访问的条目的早期修剪（但参见`--expire`）。

```
 --updateref 
```

如果前一个顶部条目被修剪，则更新对顶部 reflog 条目的值的引用（即&lt; ref&gt; @ {0}）。 （符号引用会忽略此选项。）

```
 --rewrite 
```

如果修剪了 reflog 条目的前任，则将其“旧”SHA-1 调整为等于其前面的条目的“新”SHA-1 字段。

```
 --stale-fix 
```

修剪任何指向“已损坏的提交”的 reflog 条目。破坏的提交是无法从任何参考提示访问的提交，它直接或间接地引用缺少的提交，树或 blob 对象。

该计算涉及遍历所有可到达对象，即它具有与 _git prune_ 相同的成本。它主要用于修复使用旧版 Git 进行垃圾收集而导致的损坏，这些版本不保护 reflog 所引用的对象。

```
 -n 
```

```
 --dry-run 
```

不要删除任何条目;只是展示会被修剪的东西。

```
 --verbose 
```

在屏幕上打印额外信息。

### `delete`的选项

`git reflog delete`接受选项`--updateref`，`--rewrite`，`-n`，`--dry-run`和`--verbose`，其含义与`expire`使用时的含义相同。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件