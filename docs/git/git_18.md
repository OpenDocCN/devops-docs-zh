# git-stash

> 原文： [`git-scm.com/docs/git-stash`](https://git-scm.com/docs/git-stash)

## 名称

git-stash - 将更改存储在脏工作目录中

## 概要

```
git stash list [<options>]
git stash show [<stash>]
git stash drop [-q|--quiet] [<stash>]
git stash ( pop | apply ) [--index] [-q|--quiet] [<stash>]
git stash branch <branchname> [<stash>]
git stash [push [-p|--patch] [-k|--[no-]keep-index] [-q|--quiet]
	     [-u|--include-untracked] [-a|--all] [-m|--message <message>]
	     [--] [<pathspec>…​]]
git stash clear
git stash create [<message>]
git stash store [-m|--message <message>] [-q|--quiet] <commit>
```

## 描述

如果要记录工作目录和索引的当前状态，但想要返回到干净的工作目录，请使用`git stash`。该命令将保存您的本地修改并恢复工作目录以匹配`HEAD`提交。

此命令隐藏的修改可以使用`git stash list`列出，使用`git stash show`进行检查，并使用`git stash apply`恢复（可能在不同的提交之上）。不带任何参数调用`git stash`等同于`git stash push`。默认情况下，存储被列为“ _branchname_ 上的 WIP ...”，但您可以在创建存储时在命令行上提供更具描述性的消息。

您创建的最新存储存储在`refs/stash`中;在此引用的 reflog 中可以找到较旧的 stashes，并且可以使用通常的 reflog 语法命名（例如`stash@{0}`是最近创建的 stash，`stash@{1}`是之前的那个，`stash@{2.hours.ago}`也是可能的）。也可以通过仅指定存储索引来引用状态（例如，整数`n`等同于`stash@{n}`）。

## OPTIONS

```
 push [-p|--patch] [-k|--[no-]keep-index] [-u|--include-untracked] [-a|--all] [-q|--quiet] [-m|--message <message>] [--] [<pathspec>…​] 
```

将本地修改保存到新的 _ 存储条目 _ 并将它们回滚到 HEAD（在工作树和索引中）。 &lt; message&gt; part 是可选的，并提供描述以及 stashed 状态。

要快速创建快照，可以省略“推送”。在此模式下，不允许使用非选项参数来防止拼写错误的子命令生成不需要的存储条目。对此的两个例外是`stash -p`，它作为`stash push -p`和 pathspecs 的别名，在双连字符`--`之后允许消除歧义。

当 pathspec 被赋予 _git stash push_ 时，新的存储条目仅记录与 pathspec 匹配的文件的修改状态。然后，索引条目和工作树文件也仅针对这些文件回滚到 HEAD 中的状态，从而保留与 pathspec 不匹配的文件。

如果使用`--keep-index`选项，则已添加到索引的所有更改都将保持不变。

如果使用`--include-untracked`选项，所有未跟踪的文件也会被隐藏，然后使用`git clean`清除，使工作目录处于非常干净的状态。如果使用`--all`选项，则除了未跟踪的文件外，还会隐藏和清除被忽略的文件。

使用`--patch`，您可以交互式地从 HEAD 和工作树之间的差异中选择要存储的数据。构建存储条目，使其索引状态与存储库的索引状态相同，并且其工作树仅包含您以交互方式选择的更改。然后，从您的工作树中回滚所选更改。请参阅 [git-add [1]](https://git-scm.com/docs/git-add) 的“交互模式”部分，了解如何操作`--patch`模式。

`--patch`选项意味着`--keep-index`。您可以使用`--no-keep-index`覆盖它。

```
 save [-p|--patch] [-k|--[no-]keep-index] [-u|--include-untracked] [-a|--all] [-q|--quiet] [<message>] 
```

不推荐使用此选项，而使用 _git stash push_ 。它与“stash push”的不同之处在于它不能采用 pathspecs，并且任何非选项参数都构成了消息。

```
 list [<options>] 
```

列出您当前拥有的存储条目。列出每个 _ 存储条目 _ 及其名称（例如`stash@{0}`是最新条目，`stash@{1}`是之前的条目，等等），条目的当前分支名称，以及条目所基于的提交的简短描述。

```
stash@{0}: WIP on submit: 6ebd0e2... Update git-stash documentation
stash@{1}: On master: 9cc0589... Add git-stash
```

该命令采用适用于 _git log_ 命令的选项来控制显示的内容和方式。参见 [git-log [1]](https://git-scm.com/docs/git-log) 。

```
 show [<stash>] 
```

将存储条目中记录的更改显示为隐藏内容与首次创建存储条目时的提交之间的差异。如果没有给出`&lt;stash&gt;`，则显示最新的一个。默认情况下，该命令显示 diffstat，但它将接受 _git diff_ 已知的任何格式（例如，`git stash show -p stash@{1}`以补丁形式查看第二个最新条目）。您可以使用 stash.showStat 和/或 stash.showPatch 配置变量来更改默认行为。

```
 pop [--index] [-q|--quiet] [<stash>] 
```

从存储列表中删除单个隐藏状态并将其应用于当前工作树状态之上，即执行`git stash push`的反向操作。工作目录必须与索引匹配。

应用国家可能会因冲突而失败;在这种情况下，它不会从隐藏列表中删除。您需要手动解决冲突并随后手动调用`git stash drop`。

如果使用`--index`选项，则尝试不仅恢复工作树的更改，还尝试恢复索引的更改。但是，如果存在冲突（存储在索引中，因此您无法再像以前那样应用更改），则可能会失败。

如果没有给出`&lt;stash&gt;`，则假定为`stash@{0}`，否则`&lt;stash&gt;`必须是`stash@{&lt;revision&gt;}`形式的参考。

```
 apply [--index] [-q|--quiet] [<stash>] 
```

与`pop`类似，但不要从隐藏列表中删除状态。与`pop`不同，`&lt;stash&gt;`可能是任何看似由`stash push`或`stash create`创建的提交的提交。

```
 branch <branchname> [<stash>] 
```

创建并检出一个名为`&lt;branchname&gt;`的新分支，从最初创建`&lt;stash&gt;`的提交开始，将`&lt;stash&gt;`中记录的更改应用于新的工作树和索引。如果成功，`&lt;stash&gt;`是`stash@{&lt;revision&gt;}`形式的参考，则它会丢弃`&lt;stash&gt;`。如果没有给出`&lt;stash&gt;`，则应用最新的一个。

如果运行`git stash push`的分支已经发生了足够的变化，使得`git stash apply`因冲突而失败，这将非常有用。由于存储条目应用于`git stash`运行时 HEAD 的提交之上，因此它恢复原始存储状态而没有冲突。

```
 clear 
```

删除所有存储条目。请注意，这些条目将进行修剪，并且可能无法恢复（请参阅下面的 _ 示例 _ 以获取可能的策略）。

```
 drop [-q|--quiet] [<stash>] 
```

从存储条目列表中删除单个存储条目。如果没有给出`&lt;stash&gt;`，它将删除最新的一个。即`stash@{0}`，否则`&lt;stash&gt;`必须是`stash@{&lt;revision&gt;}`形式的有效存储日志引用。

```
 create 
```

创建一个存储条目（这是一个常规提交对象）并返回其对象名称，而不将其存储在 ref 命名空间中的任何位置。这对脚本非常有用。它可能不是你想要使用的命令;看到上面的“推”。

```
 store 
```

存储通过 _git stash 创建的给定存储创建 _（这是一个悬空的合并提交）在存储引用中，更新存储 reflog。这对脚本非常有用。它可能不是你想要使用的命令;看到上面的“推”。

## 讨论

存储条目表示为提交，其树记录工作目录的状态，其第一个父项是创建条目时`HEAD`的提交。第二个父树的树在创建条目时记录索引的状态，并且它成为`HEAD`提交的子代。祖先图如下所示：

```
       .----W
      /    /
-----H----I
```

其中`H`是`HEAD`提交，`I`是记录索引状态的提交，`W`是记录工作树状态的提交。

## 例子

```
 Pulling into a dirty tree 
```

当您处于某种状态时，您会发现存在可能与您正在进行的操作相关的上游更改。当您的本地更改不与上游的更改冲突时，一个简单的`git pull`将让您继续前进。

但是，在某些情况下，您的本地更改会与上游更改发生冲突，`git pull`会拒绝覆盖您的更改。在这种情况下，您可以隐藏您的更改，执行拉动，然后取消暂停，如下所示：

```
$ git pull
 ...
file foobar not up to date, cannot merge.
$ git stash
$ git pull
$ git stash pop
```

```
 Interrupted workflow 
```

当你处于中间状态时，你的老板会进来并要求你立即修理。传统上，您将提交临时分支以存储您的更改，并返回到原始分支以进行紧急修复，如下所示：

```
# ... hack hack hack ...
$ git checkout -b my_wip
$ git commit -a -m "WIP"
$ git checkout master
$ edit emergency fix
$ git commit -a -m "Fix in a hurry"
$ git checkout my_wip
$ git reset --soft HEAD^
# ... continue hacking ...
```

您可以使用 _git stash_ 来简化上述操作，如下所示：

```
# ... hack hack hack ...
$ git stash
$ edit emergency fix
$ git commit -a -m "Fix in a hurry"
$ git stash pop
# ... continue hacking ...
```

```
 Testing partial commits 
```

如果要从工作树中的更改中进行两次或更多次提交，并且希望在提交之前测试每个更改，则可以使用`git stash push --keep-index`：

```
# ... hack hack hack ...
$ git add --patch foo            # add just first part to the index
$ git stash push --keep-index    # save all other changes to the stash
$ edit/build/test first part
$ git commit -m 'First part'     # commit fully tested change
$ git stash pop                  # prepare to work on all other changes
# ... repeat above five steps until one commit remains ...
$ edit/build/test remaining parts
$ git commit foo -m 'Remaining parts'
```

```
 Recovering stash entries that were cleared/dropped erroneously 
```

如果您错误地删除或清除了存储条目，则无法通过正常的安全机制恢复它们。但是，您可以尝试以下咒语来获取仍在存储库中但不再可访问的存储条目列表：

```
git fsck --unreachable |
grep commit | cut -d\  -f3 |
xargs git log --merges --no-walk --grep=WIP
```

## 也可以看看

[git-checkout [1]](https://git-scm.com/docs/git-checkout) ， [git-commit [1]](https://git-scm.com/docs/git-commit) ， [git-reflog [1]](https://git-scm.com/docs/git-reflog) ， [git-reset [1]](https://git-scm.com/docs/git-reset)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件