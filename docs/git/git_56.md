# git-gc

> 原文： [`git-scm.com/docs/git-gc`](https://git-scm.com/docs/git-gc)

## 名称

git-gc - 清理不必要的文件并优化本地存储库

## 概要

```
git gc [--aggressive] [--auto] [--quiet] [--prune=<date> | --no-prune] [--force] [--keep-largest-pack]
```

## 描述

在当前存储库中运行许多内务处理任务，例如压缩文件修订版（以减少磁盘空间并提高性能），删除可能从之前调用 _git add_ 创建的无法访问的对象，打包引用，修剪 reflog，rerere 元数据或陈旧的工作树。也可以更新提交图等辅助索引。

建议用户定期在每个存储库中运行此任务，以保持良好的磁盘空间利用率和良好的运行性能。

一些 git 命令可以自动运行 _git gc_ ;有关详细信息，请参见下面的`--auto`标志。如果您知道自己在做什么，并且您想要的是永久禁用此行为而无需进一步考虑，只需执行以下操作：

```
$ git config --global gc.auto 0
```

## OPTIONS

```
 --aggressive 
```

通常 _git gc_ 运行速度非常快，同时提供良好的磁盘空间利用率和性能。此选项将导致 _git gc_ 更积极地优化存储库，但代价是花费更多时间。这种优化的效果是持久的，所以这个选项只需要偶尔使用;每几百个变更集左右。

```
 --auto 
```

使用此选项， _git gc_ 检查是否需要任何内务处理;如果没有，它退出而不执行任何工作。执行可能会创建许多松散对象的操作后，某些 git 命令会运行`git gc --auto`。如果存储库中有太多松散的对象或太多的包，则需要内务处理。

如果松散对象的数量超过`gc.auto`配置变量的值，则使用`git repack -d -l`将所有松散对象合并为单个包。将`gc.auto`的值设置为 0 将禁用松散物体的自动包装。

如果包数超过`gc.autoPackLimit`的值，则使用 _ 的`-A`选项将现有包（标有`.keep`文件或超过`gc.bigPackThreshold`限制的包除外）合并为单个包中 git repack_ 。如果估计内存量不足以使`git repack`平稳运行并且未设置`gc.bigPackThreshold`，则也将排除最大包（这相当于使用`--keep-base-pack`运行`git gc`）。将`gc.autoPackLimit`设置为 0 将禁用包的自动合并。

如果由于许多松散的物体或包装而需要进行保养，则还将执行所有其他内务处理任务（例如，rerere，工作树，reflog ......）。

```
 --prune=<date> 
```

修剪早于日期的松散对象（默认为 2 周前，可由配置变量`gc.pruneExpire`覆盖）。 --prune =所有修剪松散的对象，无论其年龄如何，如果另一个进程同时写入存储库，则会增加损坏的风险;请参阅下面的“注意”。 --prune 默认开启。

```
 --no-prune 
```

不要修剪任何松散的物体。

```
 --quiet 
```

取消所有进度报告。

```
 --force 
```

即使可能在此存储库上运行另一个`git gc`实例，也强制`git gc`运行。

```
 --keep-largest-pack 
```

除最大包装外的所有包装和标有`.keep`文件的包装都合并为一个包装。使用此选项时，将忽略`gc.bigPackThreshold`。

## 组态

可以设置可选配置变量`gc.reflogExpire`以指示每个分支的 reflog 中的历史条目在此存储库中保持可用的时间。该设置表示为时间长度，例如 _90 天 _ 或 _3 个月 _。默认为 _90 天 _。

可以设置可选配置变量`gc.reflogExpireUnreachable`以指示不属于当前分支的历史 reflog 条目在此存储库中保持可用的时间长度。这些类型的条目通常是使用`git commit --amend`或`git rebase`的结果创建的，并且是修改或重组发生之前的提交。由于这些更改不是当前项目的一部分，因此大多数用户希望尽快使其过期。此选项默认为 _30 天 _。

上述两个配置变量可以赋予模式。例如，这仅将非默认到期值设置为远程跟踪分支：

```
[gc "refs/remotes/*"]
	reflogExpire = never
	reflogExpireUnreachable = 3 days
```

可选配置变量`gc.rerereResolved`表示您保留先前解决的冲突合并记录的时间长度。默认为 60 天。

可选配置变量`gc.rerereUnresolved`表示保留未解决的冲突合并记录的时间长度。默认为 15 天。

可选配置变量`gc.packRefs`确定 _git gc_ 是否运行 _git pack-refs_ 。这可以设置为“notbare”以在所有非裸存储库中启用它，或者可以将其设置为布尔值。默认为 true。

可选配置变量`gc.writeCommitGraph`确定 _git gc_ 是否应该运行 _git commit-graph write_ 。这可以设置为布尔值。默认为 false。

可选配置变量`gc.aggressiveWindow`控制在指定-aggressive 选项时优化存储库中对象的增量压缩所花费的时间。值越大，优化增量压缩所花费的时间就越多。有关详细信息，请参阅 [git-repack [1]](https://git-scm.com/docs/git-repack) 中--window 选项的文档。默认为 250。

类似地，可选配置变量`gc.aggressiveDepth`控制 [git-repack [1]](https://git-scm.com/docs/git-repack) 中的--depth 选项。默认为 50。

可选配置变量`gc.pruneExpire`控制未被引用的松散对象在被修剪之前必须有多长。默认为“2 周前”。

可选配置变量`gc.worktreePruneExpire`控制在`git worktree prune`删除之前过时工作树的年龄。默认为“3 个月前”。

## 笔记

_git gc_ 非常努力地不删除存储库中任何位置引用的对象。特别是，它不仅会保留当前分支和标记集引用的对象，还会保留由 _git filter-branch_ 在 refs / original /中保存的索引，远程跟踪分支，引用引用的对象。或者 reflogs（可以引用稍后修改或重绕的分支中的提交）。如果您希望某些对象被删除而它们不是，请检查所有这些位置，并确定在您的情况下删除这些引用是否有意义。

另一方面，当 _git gc_ 与另一个进程同时运行时，存在删除另一个进程正在使用但尚未创建引用的对象的风险。如果其他进程稍后添加对已删除对象的引用，则这可能只会导致其他进程失败或可能损坏存储库。 Git 有两个功能可以显着缓解这个问题：

1.  修改时间比`--prune`日期更新的任何对象以及从中可以访问的所有对象。

2.  将对象添加到数据库的大多数操作都会更新对象的修改时间（如果已存在），以便应用＃1。

但是，这些功能缺乏完整的解决方案，因此同时运行命令的用户必须承受一定的腐败风险（实际上似乎很低），除非他们用 _git config gc.auto 关闭自动垃圾收集。 0_ 。

## 挂钩

_git gc --auto_ 命令将运行 _pre-auto-gc_ 挂钩。有关更多信息，请参阅 [githooks [5]](https://git-scm.com/docs/githooks) 。

## 也可以看看

[git-prune [1]](https://git-scm.com/docs/git-prune) [git-reflog [1]](https://git-scm.com/docs/git-reflog) [git-repack [1]](https://git-scm.com/docs/git-repack) [git-rerere [1]](https://git-scm.com/docs/git-rerere)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件