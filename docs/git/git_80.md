# git-update-index

> 原文： [`git-scm.com/docs/git-update-index`](https://git-scm.com/docs/git-update-index)

## 名称

git-update-index - 将工作树中的文件内容注册到索引

## 概要

```
git update-index
	     [--add] [--remove | --force-remove] [--replace]
	     [--refresh] [-q] [--unmerged] [--ignore-missing]
	     [(--cacheinfo <mode>,<object>,<file>)…​]
	     [--chmod=(+|-)x]
	     [--[no-]assume-unchanged]
	     [--[no-]skip-worktree]
	     [--[no-]fsmonitor-valid]
	     [--ignore-submodules]
	     [--[no-]split-index]
	     [--[no-|test-|force-]untracked-cache]
	     [--[no-]fsmonitor]
	     [--really-refresh] [--unresolve] [--again | -g]
	     [--info-only] [--index-info]
	     [-z] [--stdin] [--index-version <n>]
	     [--verbose]
	     [--] [<file>…​]
```

## 描述

修改索引或目录高速缓存。提到的每个文件都被更新到索引中，并且任何 _ 未合并 _ 或 _ 需要更新 _ 状态被清除。

另请参阅 [git-add [1]](https://git-scm.com/docs/git-add) ，以便以更加用户友好的方式对索引执行一些最常见的操作。

_git update-index_ 处理文件的方式可以使用各种选项进行修改：

## OPTIONS

```
 --add 
```

如果指定的文件不在索引中，则添加它。默认行为是忽略新文件。

```
 --remove 
```

如果指定的文件在索引中但缺少，则将其删除。默认行为是忽略已删除的文件。

```
 --refresh 
```

查看当前索引并通过检查 stat（）信息来检查是否需要合并或更新。

```
 -q 
```

安静。如果--refresh 发现索引需要更新，则默认行为是错误输出。无论如何，此选项使 _git update-index_ 继续。

```
 --ignore-submodules 
```

不要尝试更新子模块。只有在--refresh 之前传递时才会遵循此选项。

```
 --unmerged 
```

如果--refresh 在索引中找到未合并的更改，则默认行为是错误输出。无论如何，此选项使 _git update-index_ 继续。

```
 --ignore-missing 
```

在--refresh 期间忽略丢失的文件

```
 --cacheinfo <mode>,<object>,<path> 
```

```
 --cacheinfo <mode> <object> <path> 
```

直接将指定的信息插入索引。为了向后兼容，您还可以将这三个参数作为三个单独的参数提供，但鼓励新用户使用单参数表单。

```
 --index-info 
```

从 stdin 读取索引信息。

```
 --chmod=(+|-)x 
```

设置更新文件的执行权限。

```
 --[no-]assume-unchanged 
```

指定此标志时，不会更新为路径记录的对象名称。相反，此选项设置/取消设置路径的“假定未更改”位。当“假设未更改”位打开时，用户承诺不更改文件并允许 Git 假定工作树文件与索引中记录的文件匹配。如果要更改工作树文件，则需要取消设置该位以告知 Git。当在具有非常慢的 lstat（2）系统调用（例如 cifs）的文件系统上处理大项目时，这有时是有用的。

如果需要在索引中修改此文件，Git 将失败（优雅地），例如合并时提交;因此，如果上游更改了假定未跟踪文件，则需要手动处理该情况。

```
 --really-refresh 
```

与`--refresh`类似，但无条件地检查统计信息，而不考虑“假定未更改”设置。

```
 --[no-]skip-worktree 
```

指定其中一个标志时，不会更新为路径记录的对象名称。相反，这些选项设置和取消设置路径的“skip-worktree”位。有关详细信息，请参阅下面的“跳过工作树位”部分。

```
 --[no-]fsmonitor-valid 
```

指定其中一个标志时，不会更新为路径记录的对象名称。相反，这些选项设置和取消设置路径的“fsmonitor valid”位。有关详细信息，请参阅下面的“文件系统监视器”部分

```
 -g 
```

```
 --again 
```

在索引条目与`HEAD`提交的索引条目不同的路径上运行 _git update-index_ 本身。

```
 --unresolve 
```

如果意外清除，则恢复 _ 未合并 _ 或 _ 需要在合并期间更新文件的 _ 状态。

```
 --info-only 
```

不要在对象数据库中为所有&lt; file&gt;创建对象跟随这面旗帜的论据;只需将其对象 ID 插入索引即可。

```
 --force-remove 
```

即使工作目录仍有这样的文件，也要从索引中删除该文件。 （意味着 - 删除。）

```
 --replace 
```

默认情况下，当索引中存在文件`path`时， _git update-index_ 拒绝添加`path/file`的尝试。同样，如果存在文件`path/file`，则无法添加文件`path`。使用--replace 标志，将自动删除与添加的条目冲突的现有条目以及警告消息。

```
 --stdin 
```

而不是从命令行获取路径列表，从标准输入中读取路径列表。默认情况下，路径由 LF（即每行一个路径）分隔。

```
 --verbose 
```

报告从索引中添加和删除的内容。

```
 --index-version <n> 
```

将结果索引写入指定的磁盘格式版本。支持的版本为 2,3 和 4.当前默认版本为 2 或 3，具体取决于是否使用了额外功能，例如`git add -N`。

版本 4 执行简单的路径名压缩，在大型存储库上将索引大小减少 30％-50％，从而加快了加载时间。版本 4 相对年轻（2012 年 10 月首次发布于 1.8.0）。其他 Git 实现（如 JGit 和 libgit2）可能还不支持它。

```
 -z 
```

仅对`--stdin`或`--index-info`有意义;路径用 NUL 字符而不是 LF 分隔。

```
 --split-index 
```

```
 --no-split-index 
```

启用或禁用拆分索引模式。如果已启用拆分索引模式并再次给出`--split-index`，则$ GIT_DIR / index 中的所有更改都将推回到共享索引文件。

无论`core.splitIndex`配置变量的值如何，这些选项都会生效（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。但是当更改违反配置值时会发出警告，因为配置的值将在下次读取索引时生效，这将消除该选项的预期效果。

```
 --untracked-cache 
```

```
 --no-untracked-cache 
```

启用或禁用未跟踪的缓存功能。请在启用之前使用`--test-untracked-cache`。

无论`core.untrackedCache`配置变量的值如何，这些选项都会生效（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。但是当更改违反配置值时会发出警告，因为配置的值将在下次读取索引时生效，这将消除该选项的预期效果。

```
 --test-untracked-cache 
```

仅对工作目录执行测试以确保可以使用未跟踪的缓存。如果您真的想使用它，则必须使用`--untracked-cache`或`--force-untracked-cache`或`core.untrackedCache`配置变量手动启用未跟踪的缓存。如果测试失败，则退出代码为 1，并且消息说明根据需要不起作用的内容，否则退出代码为 0 并打印 OK。

```
 --force-untracked-cache 
```

与`--untracked-cache`相同。提供与旧版 Git 的向后兼容性，其中`--untracked-cache`曾暗示`--test-untracked-cache`，但此选项将无条件地启用扩展。

```
 --fsmonitor 
```

```
 --no-fsmonitor 
```

启用或禁用文件系统监视器功能。无论`core.fsmonitor`配置变量的值如何，这些选项都会生效（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。但是当更改违反配置值时会发出警告，因为配置的值将在下次读取索引时生效，这将消除该选项的预期效果。

```
 -- 
```

不要将任何更多的参数解释为选项。

```
 <file> 
```

要采取行动的文件。请注意以 _ 开头的文件。_ 被丢弃。这包括`./file`和`dir/./file`。如果您不想这样，那么使用更干净的名称。结束 _/_ 的目录和 _//_ 的路径也是如此

## 使用--REFRESH

`--refresh`不计算新的 sha1 文件或使模式/内容更改的索引更新。但是做的是将文件的统计信息与索引“重新匹配”，以便您可以刷新尚未更改的文件的索引但是 stat 条目的位置是过时了。

例如，你想在执行 _git read-tree_ 之后执行此操作，将 stat 索引详细信息与正确的文件链接起来。

## 使用--CACHEINFO 或--INFO-ONLY

`--cacheinfo`用于注册不在当前工作目录中的文件。这对于最小检出合并非常有用。

假装你在模式和 sha1 的路径上有一个文件，说：

```
$ git update-index --add --cacheinfo <mode>,<sha1>,<path>
```

`--info-only`用于注册文件而不将它们放在对象数据库中。这对仅状态存储库很有用。

`--cacheinfo`和`--info-only`的行为类似：索引已更新，但对象数据库未更新。当对象在数据库中但文件在本地不可用时，`--cacheinfo`很有用。当文件可用时，`--info-only`很有用，但您不希望更新对象数据库。

## 使用--INDEX-INFO

`--index-info`是一种更强大的机制，允许您从标准输入中提供多个条目定义，并专门为脚本设计。它可以采用三种格式的输入：

1.  模式 SP 类型 SP sha1 TAB 路径

    这种格式是将`git ls-tree`输出填充到索引中。

2.  模式 SP sha1 SP 阶段 TAB 路径

    这种格式是将更高阶的阶段放入索引文件中，并匹配 _git ls-files --stage_ 输出。

3.  模式 SP sha1 TAB 路径

    任何 Git 命令都不再生成此格式，但`update-index --index-info`将继续支持此格式。

要为索引放置更高的阶段条目，首先应通过为路径提供 mode = 0 条目，然后以第三种格式提供必要的输入行来删除路径。

例如，从这个索引开始：

```
$ git ls-files -s
100644 8a1218a1024a212bb3db30becd860315f9f3ac52 0       frotz
```

您可以将以下输入提供给`--index-info`：

```
$ git update-index --index-info
0 0000000000000000000000000000000000000000	frotz
100644 8a1218a1024a212bb3db30becd860315f9f3ac52 1	frotz
100755 8a1218a1024a212bb3db30becd860315f9f3ac52 2	frotz
```

输入的第一行输入 0 作为删除路径的模式;只要格式良好，SHA-1 无关紧要。然后第二行和第三行为该路径提供阶段 1 和阶段 2 条目。在上述之后，我们最终会得到：

```
$ git ls-files -s
100644 8a1218a1024a212bb3db30becd860315f9f3ac52 1	frotz
100755 8a1218a1024a212bb3db30becd860315f9f3ac52 2	frotz
```

## 使用“ASSUME UNCHANGED”BIT

Git 中的许多操作依赖于您的文件系统以实现高效的`lstat(2)`实现，因此可以便宜地检查工作树文件的`st_mtime`信息，以查看文件内容是否已从索引文件中记录的版本更改。不幸的是，一些文件系统效率低`lstat(2)`。如果您的文件系统是其中之一，则可以将“假设未更改”位设置为未更改的路径，以使 Git 不执行此检查。请注意，在路径上设置此位并不意味着 Git 将检查文件的内容以查看它是否已更改 - 它使 Git 省略任何检查并假设它已更改**而不是**。当您对工作树文件进行更改时，您必须通过在修改它们之前或之后删除“假定未更改”位来明确告知 Git。

要设置“假定未更改”位，请使用`--assume-unchanged`选项。要取消设置，请使用`--no-assume-unchanged`。要查看哪些文件设置了“假设未更改”，请使用`git ls-files -v`（参见 [git-ls-files [1]](https://git-scm.com/docs/git-ls-files) ）。

该命令查看`core.ignorestat`配置变量。如果是这样，使用`git update-index paths...`更新路径，并使用更新索引和工作树的其他 Git 命令更新路径（例如 _git apply --index_ ， _git checkout-index -u_ 和 _git read-tree -u_ ）自动标记为“假设不变”。注意，如果`git update-index --refresh`发现工作树文件与索引匹配，则“假定未更改”位为**而不是**设置（如果要将它们标记为“假设未更改”，请使用`git update-index --really-refresh`）。

## 例子

要仅更新和刷新已检出的文件：

```
$ git checkout-index -n -f -a && git update-index --ignore-missing --refresh
```

```
 On an inefficient filesystem with core.ignorestat set 
```

```
$ git update-index --really-refresh              (1)
$ git update-index --no-assume-unchanged foo.c   (2)
$ git diff --name-only                           (3)
$ edit foo.c
$ git diff --name-only                           (4)
M foo.c
$ git update-index foo.c                         (5)
$ git diff --name-only                           (6)
$ edit foo.c
$ git diff --name-only                           (7)
$ git update-index --no-assume-unchanged foo.c   (8)
$ git diff --name-only                           (9)
M foo.c
```

1.  强制 lstat（2）为匹配索引的路径设置“假定未更改”位。

2.  标记要编辑的路径。

3.  这样做 lstat（2）并找到索引匹配路径。

4.  这样做 lstat（2）并找到索引**而不是**匹配路径。

5.  将新版本注册到索引集“假定未更改”位。

6.  并假设不变。

7.  即使你编辑它。

8.  你可以告诉我事后的变化。

9.  现在它检查 lstat（2）并发现它已被更改。

## SKIP-WORKTREE BIT

Skip-worktree 位可以在一个（长）句子中定义：当读取条目时，如果它被标记为 skip-worktree，那么 Git 假装其工作目录版本是最新的并且改为读取索引版本。

详细说明，“阅读”意味着检查文件是否存在，读取文件属性或文件内容。工作目录版本可能存在或不存在。如果存在，其内容可能与索引版本匹配。写入不受此位影响，内容安全仍然是第一优先。请注意，Git _ 可以 _ 更新工作目录文件，标记为 skip-worktree，如果安全的话（即工作目录版本与索引版本匹配）

虽然这个位看起来类似于假设未改变的位，但它的目标与假设未改变的位不同。当两者都设置时，Skip-worktree 也优先于假定未更改的位。

## 分裂指数

此模式适用于具有非常大索引的存储库，旨在减少重复编写这些索引所需的时间。

在此模式下，索引分为两个文件：$ GIT_DIR / index 和$ GIT_DIR / sharedindex。&lt; SHA-1&gt;。更改将在$ GIT_DIR / index（拆分索引）中累积，而共享索引文件包含所有索引条目并保持不变。

当拆分索引中的条目数达到 splitIndex.maxPercentChange 配置变量指定的级别时，拆分索引中的所有更改都会被推回到共享索引文件中（请参阅 [git-config [1]](https://git-scm.com/docs/git-config) ）。

每次创建新的共享索引文件时，如果旧的共享索引文件的修改时间早于 splitIndex.sharedIndexExpire 配置变量指定的值，则删除旧的共享索引文件（请参阅 [git-config [1]](https://git-scm.com/docs/git-config) ）。

为了避免删除仍在使用的共享索引文件，每次创建或读取基于共享索引文件的新拆分索引时，其修改时间将更新为当前时间。

## UNTRACKED CACHE

此缓存旨在加速涉及确定未跟踪文件（例如`git status`）的命令。

此功能的工作原理是记录工作树目录的 mtime，然后忽略对 mtime 未更改的目录中的文件的读取目录和 stat 调用。为此，如果添加，修改或删除目录中的文件，则底层操作系统和文件系统必须更改目录的`st_mtime`字段。

您可以使用`--test-untracked-cache`选项测试文件系统是否支持该文件系统。 `--untracked-cache`选项用于在旧版本的 Git 中隐式执行该测试，但情况已不再如此。

如果要启用（或禁用）此功能，则使用`core.untrackedCache`配置变量（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）比使用`git update-index`选项更容易使用`git update-index`每个存储库，特别是如果您想在所使用的所有存储库中执行此操作，因为您可以在`$HOME/.gitconfig`中将配置变量设置为`true`（或`false`）一次，并使其影响您触摸的所有存储库。

更改`core.untrackedCache`配置变量时，下次命令读取索引时，会将未跟踪的高速缓存添加到索引中或从索引中删除;当使用`--[no-|force-]untracked-cache`时，未跟踪的缓存会立即添加到索引中或从索引中删除。

在 2.17 之前，未跟踪的缓存有一个错误，将带有符号链接的目录替换到另一个目录可能会导致错误地将 git 跟踪的文件显示为未跟踪。请参阅“状态：添加一个显示 core.untrackedCache 错误的失败测试”提交到 git.git。解决方法是（这可能适用于未来其他未发现的错误）：

```
$ git -c core.untrackedCache=false status
```

当涉及到未跟踪缓存的内部结构时，此错误也被证明会影响用文件替换目录的非符号链接情况，但是没有报告导致错误“git status”输出的情况。

还有一些情况，在 2.17 之前由 git 版本编写的现有索引将引用不再存在的目录，可能导致许多“无法打开目录”警告打印在“git status”上。这些是以前默默丢弃的现有问题的新警告。

与上述错误一样，解决方案是一次性使用`core.untrackedCache=false`执行“git status”以清除剩余的坏数据。

## 文件系统监控

此功能旨在加速具有大型工作目录的 repos 的 git 操作。

它使 git 能够与文件系统监视器一起工作（参见 [githooks [5]](https://git-scm.com/docs/githooks) 的“fsmonitor-watchman”部分），它可以告知它已经修改了哪些文件。这使得 git 可以避免必须 lstat（）每个文件来查找修改过的文件。

与未跟踪的缓存一起使用时，它可以通过避免扫描整个工作目录以查找新文件的成本来进一步提高性能。

如果要启用（或禁用）此功能，则使用`core.fsmonitor`配置变量（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）比使用`git update-index`选项更容易使用`git update-index`每个存储库，特别是如果您想在所使用的所有存储库中执行此操作，因为您可以在`$HOME/.gitconfig`中设置一次配置变量，并使其影响您触摸的所有存储库。

更改`core.fsmonitor`配置变量时，下次命令读取索引时，会在索引中添加或删除文件系统监视器。使用`--[no-]fsmonitor`时，会立即将文件系统监视器添加到索引中或从索引中删除。

## 组态

该命令用于表示`core.filemode`配置变量。如果您的存储库位于可执行位不可靠的文件系统上，则应将其设置为 _false_ （请参阅 [git-config [1]](https://git-scm.com/docs/git-config) ）。这会导致命令忽略文件系统中索引和文件模式中记录的文件模式的差异（如果它们仅在可执行位上不同）。在这样一个不幸的文件系统上，您可能需要使用 _git update-index --chmod =_ 。

很相似，如果`core.symlinks`配置变量设置为 _false_ （参见 [git-config [1]](https://git-scm.com/docs/git-config) ），则符号链接被检出为普通文件，并且此命令不会修改从符号链接到常规文件的记录文件模式。

该命令查看`core.ignorestat`配置变量。参见 _ 使用上面的“假设未改变”位 _ 部分。

该命令还会查看`core.trustctime`配置变量。当通过 Git 之外的某些东西定期修改 inode 更改时间（文件系统爬虫和备份系统使用 ctime 标记处理的文件）时，它会很有用（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

可以通过`core.untrackedCache`配置变量启用未跟踪的高速缓存扩展（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

## 也可以看看

[git-config [1]](https://git-scm.com/docs/git-config) ， [git-add [1]](https://git-scm.com/docs/git-add) ， [git-ls-files [1]](https://git-scm.com/docs/git-ls-files)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件