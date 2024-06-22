# git-worktree

> 原文： [`git-scm.com/docs/git-worktree`](https://git-scm.com/docs/git-worktree)

## 名称

git-worktree - 管理多个工作树

## 概要

```
git worktree add [-f] [--detach] [--checkout] [--lock] [-b <new-branch>] <path> [<commit-ish>]
git worktree list [--porcelain]
git worktree lock [--reason <string>] <worktree>
git worktree move <worktree> <new-path>
git worktree prune [-n] [-v] [--expire <expire>]
git worktree remove [-f] <worktree>
git worktree unlock <worktree>
```

## 描述

管理连接到同一存储库的多个工作树。

git 存储库可以支持多个工作树，允许您一次签出多个分支。使用`git worktree add`，新的工作树与存储库相关联。这个新的工作树称为“链接工作树”，而不是“git init”或“git clone”编写的“主工作树”。存储库有一个主要工作树（如果它不是裸存储库）和零个或多个链接工作树。完成链接的工作树后，使用`git worktree remove`将其删除。

如果在不使用`git worktree remove`的情况下删除工作树，则其关联的管理文件（位于下面的“详细信息”）最终将自动删除（请参阅 [git-config 中的`gc.worktreePruneExpire` [1] ]](https://git-scm.com/docs/git-config) ），或者您可以在主要或任何链接的工作树中运行`git worktree prune`来清理任何陈旧的管理文件。

如果链接的工作树存储在并非总是挂载的便携式设备或网络共享上，则可以通过发出`git worktree lock`命令来阻止其管理文件被修剪，可选择指定`--reason`来解释工作树被锁定的原因。

## COMMANDS

```
 add <path> [<commit-ish>] 
```

创建`&lt;path&gt;`并将`&lt;commit-ish&gt;`签出到其中。新的工作目录链接到当前存储库，共享除工作目录特定文件（如 HEAD，索引等）之外的所有内容。`-`也可以指定为`&lt;commit-ish&gt;`;它与`@{-1}`同义。

如果&lt; commit-ish&gt;是一个分支名称（称之为`&lt;branch&gt;`）并且未找到，并且既没有使用`-b`也没有`-B`或`--detach`，但是在一个远程中确实存在跟踪分支（称之为`&lt;remote&gt;`）使用匹配的名称，视为等效于：

```
$ git worktree add --track -b <branch> <path> <remote>/<branch>
```

如果分支存在于多个遥控器中，并且其中一个由`checkout.defaultRemote`配置变量命名，我们将使用该分支用于消除歧义，即使`&lt;branch&gt;`在所有遥控器中都不是唯一的。将其设置为例如如果`&lt;branch&gt;`不明确但存在于 _ 原点 _ 遥控器上，`checkout.defaultRemote=origin`总是从那里检出远程分支。另见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`checkout.defaultRemote`。

如果省略`&lt;commit-ish&gt;`并且既不使用`-b`也不使用`-B`和`--detach`，那么为方便起见，新工作树与以`$(basename &lt;path&gt;)`命名的分支（称为`&lt;branch&gt;`）相关联。如果`&lt;branch&gt;`不存在，将自动创建基于 HEAD 的新分支，就像给出`-b &lt;branch&gt;`一样。如果`&lt;branch&gt;`确实存在，它将在新的工作树中检出，如果它没有在其他任何地方检出，否则命令将拒绝创建工作树（除非使用`--force`）。

```
 list 
```

列出每个工作树的详细信息。首先列出主要工作树，然后列出每个链接的工作树。输出详细信息包括工作树是否裸露，当前检出的修订版，以及当前检出的分支（或者 _ 分离 HEAD_ ，如果没有）。

```
 lock 
```

如果工作树位于未始终安装的便携式设备或网络共享上，请将其锁定以防止其管理文件被自动修剪。这也可以防止它被移动或删除。 （可选）使用`--reason`指定锁定的原因。

```
 move 
```

将工作树移动到新位置。请注意，无法移动主工作树或包含子模块的链接工作树。

```
 prune 
```

修剪$ GIT_DIR / worktrees 中的工作树信息。

```
 remove 
```

删除一个工作树。只能删除干净的工作树（没有未跟踪的文件，也不会删除跟踪文件中的修改）。可以使用`--force`删除不干净的工作树或带子模块的工作树。无法删除主工作树。

```
 unlock 
```

解锁工作树，允许对其进行修剪，移动或删除。

## OPTIONS

```
 -f 
```

```
 --force 
```

默认情况下，`add`拒绝创建新的工作树，当`&lt;commit-ish&gt;`是分支名称并且已经被另一个工作树检出，或者`&lt;path&gt;`已经分配给某个工作树但是丢失了（例如，如果`&lt;path&gt;`被手动删除）。此选项会覆盖这些安全措施。要添加缺失但已锁定的工作树路径，请指定`--force`两次。

`move`拒绝移动锁定的工作树，除非指定了两次`--force`。

`remove`拒绝删除不干净的工作树，除非使用`--force`。要删除锁定的工作树，请指定`--force`两次。

```
 -b <new-branch> 
```

```
 -B <new-branch> 
```

使用`add`，从`&lt;commit-ish&gt;`开始创建一个名为`&lt;new-branch&gt;`的新分支，并将`&lt;new-branch&gt;`签出到新的工作树中。如果省略`&lt;commit-ish&gt;`，则默认为 HEAD。默认情况下，`-b`拒绝创建新分支（如果已存在）。 `-B`会覆盖此保护措施，将`&lt;new-branch&gt;`重置为`&lt;commit-ish&gt;`。

```
 --detach 
```

使用`add`，在新工作树中分离 HEAD。请参见 [git-checkout [1]](https://git-scm.com/docs/git-checkout) 中的“DETACHED HEAD”。

```
 --[no-]checkout 
```

默认情况下，`add`检出`&lt;commit-ish&gt;`，但是，`--no-checkout`可用于抑制检出以进行自定义，例如配置稀疏检出。请参见 [git-read-tree [1]](https://git-scm.com/docs/git-read-tree) 中的“稀疏检出”。

```
 --[no-]guess-remote 
```

使用`worktree add &lt;path&gt;`，不使用`&lt;commit-ish&gt;`，而不是从 HEAD 创建新分支，如果在一个与`&lt;path&gt;`的基本名称匹配的远程中存在跟踪分支，则将新分支基于远程跟踪分支，并标记远程跟踪分支作为新分支的“上游”。

也可以使用`worktree.guessRemote`配置选项将其设置为默认行为。

```
 --[no-]track 
```

创建新分支时，如果`&lt;commit-ish&gt;`是分支，则将其标记为新分支的“上游”。如果`&lt;commit-ish&gt;`是远程跟踪分支，则这是默认值。有关详细信息，请参阅 [git-branch [1]](https://git-scm.com/docs/git-branch) 中的“--track”。

```
 --lock 
```

创建后保持工作树锁定。这相当于`git worktree add`之后的`git worktree lock`，但没有竞争条件。

```
 -n 
```

```
 --dry-run 
```

使用`prune`时，不要删除任何东西;只需报告它将删除的内容。

```
 --porcelain 
```

使用`list`，以易于解析的格式输出脚本。无论用户配置如何，这种格式在 Git 版本中都将保持稳定。请参阅下文了解详情。

```
 -q 
```

```
 --quiet 
```

使用 _ 添加 _，禁止反馈消息。

```
 -v 
```

```
 --verbose 
```

使用`prune`，报告所有删除。

```
 --expire <time> 
```

使用`prune`，仅使未使用的工作树超过&lt; time&gt;。

```
 --reason <string> 
```

使用`lock`解释工作树被锁定的原因。

```
 <worktree> 
```

工作树可以通过路径来识别，无论是相对的还是绝对的。

如果工作树路径中的最后一个路径组件在工作树中是唯一的，则可以使用它来识别工作树。例如，如果你只有两个工作树，在“/ abc / def / ghi”和“/ abc / def / ggg”，那么“ghi”或“def / ghi”足以指向前工作树。

## REFS

在多个工作树中，一些参考树可以在所有工作树之间共享，一些参考树是本地的。一个例子是 HEAD 对于所有工作树都是不同的。本节介绍共享规则以及如何从另一个工作树访问 refs。

通常，所有伪引用都是每个工作树，并且所有以“refs /”开头的引用都是共享的。伪引用类似 HEAD，直接在 GIT_DIR 下而不是在 GIT_DIR / refs 内。这有一个例外：refs / bisect 中的 refs 和不共享 refs / worktree。

仍然可以通过两个特殊路径（main-worktree 和 worktree）从另一个工作树访问每个工作树的引用。前者允许访问主工作树的每个工作树参考，而后者访问所有链接的工作树。

例如，main-worktree / HEAD 或 main-worktree / refs / bisect / good 分别解析为与主工作树的 HEAD 和 refs / bisect / good 相同的值。类似地，worktrees / foo / HEAD 或 worktrees / bar / refs / bisect / bad 与 GIT_COMMON_DIR / worktrees / foo / HEAD 和 GIT_COMMON_DIR / worktrees / bar / refs / bisect / bad 相同。

要访问 refs，最好不要直接查看 GIT_DIR。而是使用诸如 [git-rev-parse [1]](https://git-scm.com/docs/git-rev-parse) 或 [git-update-ref [1]](https://git-scm.com/docs/git-update-ref) 之类的命令，它们将正确处理 refs。

## 配置文件

默认情况下，存储库“config”文件在所有工作树之间共享。如果配置变量`core.bare`或`core.worktree`已经存在于配置文件中，它们将仅应用于主工作树。

为了具有特定于工作树的配置，您可以打开“worktreeConfig”扩展名，例如：

```
$ git config extensions.worktreeConfig true
```

在此模式下，特定配置保留在`git rev-parse --git-path config.worktree`指向的路径中。您可以使用`git config --worktree`在此文件中添加或更新配置。较旧的 Git 版本将拒绝使用此扩展名访问存储库。

请注意，在此文件中，`core.bare`和`core.worktree`的例外消失了。如果您之前在$ GIT_DIR / config 中有它们，则必须将它们移动到主工作树的`config.worktree`。您也可以借此机会查看并移动您不想共享的其他配置到所有工作树：

*   永远不要共享`core.worktree`和`core.bare`

*   除非您确定始终对所有工作树使用稀疏检出，否则建议每个工作树使用`core.sparseCheckout`。

## 细节

每个链接的工作树在存储库的$ GIT_DIR / worktrees 目录中都有一个私有子目录。私有子目录的名称通常是链接工作树路径的基本名称，可能附加一个数字以使其唯一。例如，当`$GIT_DIR=/path/main/.git`命令`git worktree add /path/other/test-next next`在`/path/other/test-next`中创建链接的工作树时，还会创建`$GIT_DIR/worktrees/test-next`目录（如果已经`test-next`，则创建`$GIT_DIR/worktrees/test-next1`）。

在链接的工作树中，$ GIT_DIR 设置为指向此私有目录（例如示例中的`/path/main/.git/worktrees/test-next`），并且$ GIT_COMMON_DIR 设置为指向主工作树的$ GIT_DIR（例如`/path/main/.git`）。这些设置在位于链接工作树顶部目录的`.git`文件中进行。

通过`git rev-parse --git-path`的路径分辨率使用$ GIT_DIR 或$ GIT_COMMON_DIR，具体取决于路径。例如，在链接的工作树`git rev-parse --git-path HEAD`中返回`/path/main/.git/worktrees/test-next/HEAD`（不是`/path/other/test-next/.git/HEAD`或`/path/main/.git/HEAD`），而`git rev-parse --git-path refs/heads/master`使用$ GIT_COMMON_DIR 并返回`/path/main/.git/refs/heads/master`，因为 refs 在所有工作树之间共享，refs /除外平分和参考/工作树。

有关详细信息，请参阅 [gitrepository-layout [5]](https://git-scm.com/docs/gitrepository-layout) 。经验法则是，当您需要直接访问$ GIT_DIR 内的某些内容时，不要对路径是属于$ GIT_DIR 还是$ GIT_COMMON_DIR 做出任何假设。使用`git rev-parse --git-path`获取最终路径。

如果手动移动链接的工作树，则需要更新条目目录中的 _gitdir_ 文件。例如，如果链接的工作树移动到`/newpath/test-next`并且其`.git`文件指向`/path/main/.git/worktrees/test-next`，则将`/path/main/.git/worktrees/test-next/gitdir`更新为引用`/newpath/test-next`。

要防止$ GIT_DIR / worktrees 条目被修剪（这在某些情况下很有用，例如当条目的工作树存储在便携式设备上时），请使用`git worktree lock`命令，该命令添加名为 _ 的文件锁定 _ 到条目的目录。该文件包含纯文本的原因。例如，如果链接的工作树的`.git`文件指向`/path/main/.git/worktrees/test-next`，则名为`/path/main/.git/worktrees/test-next/locked`的文件将阻止`test-next`条目被修剪。有关详细信息，请参阅 [gitrepository-layout [5]](https://git-scm.com/docs/gitrepository-layout) 。

启用 extensions.worktreeConfig 时，在`.git/config`之后读取配置文件`.git/worktrees/&lt;id&gt;/config.worktree`。

## 列表输出格式

worktree list 命令有两种输出格式。默认格式显示包含列的单行详细信息。例如：

```
$ git worktree list
/path/to/bare-source            (bare)
/path/to/linked-worktree        abcd1234 [master]
/path/to/other-linked-worktree  1234abc  (detached HEAD)
```

### 瓷器格式

瓷器格式每个属性都有一行。列出的属性标签和值由单个空格分隔。布尔属性（如 _ 裸 _ 和 _ 分离 _）仅作为标签列出，仅当值为真时才存在。工作树的第一个属性始终是`worktree`，空行表示记录的结尾。例如：

```
$ git worktree list --porcelain
worktree /path/to/bare-source
bare

worktree /path/to/linked-worktree
HEAD abcd1234abcd1234abcd1234abcd1234abcd1234
branch refs/heads/master

worktree /path/to/other-linked-worktree
HEAD 1234abc1234abc1234abc1234abc1234abc1234a
detached
```

## 例子

您正处于重构阶段，您的老板进来并要求您立即修复。您通常可以使用 [git-stash [1]](https://git-scm.com/docs/git-stash) 临时存储您的更改，但是，您的工作树处于混乱状态（使用新的，移动的和删除的文件以及其他零碎的部分）散落在你周围，你不想冒任何干扰它的风险。相反，您创建一个临时链接工作树来进行紧急修复，完成后将其删除，然后恢复您之前的重构会话。

```
$ git worktree add -b emergency-fix ../temp master
$ pushd ../temp
# ... hack hack hack ...
$ git commit -a -m 'emergency fix for boss'
$ popd
$ git worktree remove ../temp
```

## BUGS

一般的多次检出仍然是实验性的，对子模块的支持是不完整的。建议不要对超级项目进行多次检出。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件