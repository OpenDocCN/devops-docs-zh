# git-read-tree

> 原文： [`git-scm.com/docs/git-read-tree`](https://git-scm.com/docs/git-read-tree)

## 名称

git-read-tree - 将树信息读入索引

## 概要

```
git read-tree [[-m [--trivial] [--aggressive] | --reset | --prefix=<prefix>]
		[-u [--exclude-per-directory=<gitignore>] | -i]]
		[--index-output=<file>] [--no-sparse-checkout]
		(--empty | <tree-ish1> [<tree-ish2> [<tree-ish3>]])
```

## 描述

读取由&lt; tree-ish&gt;给出的树信息。进入索引，但实际上并没有**更新**它“缓存”的任何文件。 （参见： [git-checkout-index [1]](https://git-scm.com/docs/git-checkout-index) ）

可选地，它可以将树合并到索引中，使用`-m`标志执行快进（即 2 路）合并或 3 路合并。与`-m`一起使用时，`-u`标志会使它还使用合并结果更新工作树中的文件。

普通合并由 _git read-tree_ 本身完成。当 _git read-tree_ 返回时，只有冲突路径处于未合并状态。

## OPTIONS

```
 -m 
```

执行合并，而不仅仅是读取。如果您的索引文件包含未合并的条目，则该命令将拒绝运行，表示您尚未完成之前的合并。

```
 --reset 
```

与-m 相同，除了丢弃未合并的条目而不是失败。

```
 -u 
```

成功合并后，使用合并结果更新工作树中的文件。

```
 -i 
```

通常，合并需要索引文件以及工作树中的文件与当前头部提交保持同步，以免丢失本地更改。此标志禁用对工作树的检查，并且在创建与当前工作树状态不直接相关的树的合并到临时索引文件时使用。

```
 -n 
```

```
 --dry-run 
```

检查命令是否会出错，而不更新索引或工作树中的文件是否真实。

```
 -v 
```

显示检出文件的进度。

```
 --trivial 
```

限制 _git read-tree_ 的三向合并只有在不需要文件级合并时才会发生，而不是为简单的情况解析合并并在索引中保留未解决的冲突文件。

```
 --aggressive 
```

通常， _git read-tree_ 的三向合并解决了真正琐碎案例的合并，并使索引中的其他案例无法解决，因此瓷器可以实现不同的合并策略。此标志使命令在内部解决了几个案例：

*   当一侧移除路径而另一侧离开路径未经修改时。决议是删除该路径。

*   当双方都删除一条路径。决议是删除该路径。

*   当双方都添加相同的路径时。决议是添加该路径。

```
 --prefix=<prefix> 
```

保留当前索引内容，并在`&lt;prefix&gt;`目录下读取指定 tree-ish 的内容。该命令将拒绝覆盖原始索引文件中已存在的条目。

```
 --exclude-per-directory=<gitignore> 
```

使用`-u`和`-m`选项运行命令时，合并结果可能需要覆盖当前分支中未跟踪的路径。该命令通常拒绝继续合并以避免丢失这样的路径。然而，这种安全阀有时会妨碍。例如，经常会发生另一个分支添加了一个文件，该文件曾经是分支中生成的文件，当您在运行`make`之后但在运行`make clean`之前尝试切换到该分支时，安全阀会触发删除生成的文件。此选项告诉命令读取每个目录的排除文件（通常是 _.gitignore_ ），并允许覆盖这样一个未跟踪但明确忽略的文件。

```
 --index-output=<file> 
```

而不是将结果写入`$GIT_INDEX_FILE`，而是在命名文件中写入结果索引。在命令运行时，原始索引文件使用与通常相同的机制锁定。该文件必须允许从通常索引文件旁边创建的临时文件重命名（2）;通常这意味着它需要与索引文件本身位于同一文件系统上，并且您需要对索引文件和索引输出文件所在目录的写入权限。

```
 --[no-]recurse-submodules 
```

使用--recurse-submodules 将通过递归调用 read-tree，根据超级项目中记录的提交更新所有初始化子模块的内容，同时将子模块 HEAD 设置为在该提交时分离。

```
 --no-sparse-checkout 
```

即使`core.sparseCheckout`为 true，也禁用稀疏检出支持。

```
 --empty 
```

而不是将树对象读入索引，只需清空它。

```
 <tree-ish#> 
```

要读取/合并的树对象的 id。

## 并构

如果指定了`-m`， _git read-tree_ 可以执行 3 种合并，如果只给出 1 棵树，则单个树合并，2 棵树的快进合并或 3 路合并如果提供 3 棵或更多树。

### 单树合并

如果仅指定了 1 个树，则 _git read-tree_ 的操作就好像用户没有指定`-m`，除非原始索引具有给定路径名的条目，并且路径的内容匹配在读取树时，使用索引中的统计信息。 （换句话说，索引的 stat（）s 优先于合并的树）。

这意味着如果你执行`git read-tree -m &lt;newtree&gt;`后跟`git checkout-index -f -u -a`， _git checkout-index_ 只检查真正改变的东西。

当 _git diff-files_ 在 _git read-tree_ 之后运行时，这用于避免不必要的错误命中。

### 两棵树合并

通常，这被调用为`git read-tree -m $H $M`，其中$ H 是当前存储库的头部提交，而$ M 是外部树的头部，它只是在$ H 之前（即我们处于快速前进状态） ）。

当指定了两个树时，用户告诉 _git read-tree_ 如下：

1.  当前索引和工作树是从$ H 派生的，但是用户可能会因为$ H 而对其进行局部更改。

2.  用户希望快进到$ M。

在这种情况下，`git read-tree -m $H $M`命令确保没有因“合并”而丢失本地更改。以下是“结转”规则，其中“I”表示索引，“clean”表示索引和工作树重合，“exists”/“nothing”表示指定提交中存在路径：

```
	I                   H        M        Result
       -------------------------------------------------------
     0  nothing             nothing  nothing  (does not happen)
     1  nothing             nothing  exists   use M
     2  nothing             exists   nothing  remove path from index
     3  nothing             exists   exists,  use M if "initial checkout",
				     H == M   keep index otherwise
				     exists,  fail
				     H != M

        clean I==H  I==M
       ------------------
     4  yes   N/A   N/A     nothing  nothing  keep index
     5  no    N/A   N/A     nothing  nothing  keep index

     6  yes   N/A   yes     nothing  exists   keep index
     7  no    N/A   yes     nothing  exists   keep index
     8  yes   N/A   no      nothing  exists   fail
     9  no    N/A   no      nothing  exists   fail

     10 yes   yes   N/A     exists   nothing  remove path from index
     11 no    yes   N/A     exists   nothing  fail
     12 yes   no    N/A     exists   nothing  fail
     13 no    no    N/A     exists   nothing  fail

	clean (H==M)
       ------
     14 yes                 exists   exists   keep index
     15 no                  exists   exists   keep index

        clean I==H  I==M (H!=M)
       ------------------
     16 yes   no    no      exists   exists   fail
     17 no    no    no      exists   exists   fail
     18 yes   no    yes     exists   exists   keep index
     19 no    no    yes     exists   exists   keep index
     20 yes   yes   no      exists   exists   use M
     21 no    yes   no      exists   exists   fail
```

在所有“保留索引”的情况下，索引条目保持原始索引文件中的状态。如果条目不是最新的， _git read-tree_ 在-u 标志下操作时保持工作树中的副本不变。

当 _git read-tree_ 的这种形式成功返回时，您可以通过运行`git diff-index --cached $M`来查看您所做的哪些“本地更改”。请注意，这不一定与`git diff-index --cached $H`在这样的两个树合并之前产生的内容相匹配。这是因为案例 18 和 19 ---如果你已经有$ M 的变化（例如，你可能通过电子邮件以补丁形式提取），`git diff-index --cached $H`会告诉你这个合并之前的变化，但在两树合并后它不会显示在`git diff-index --cached $M`输出中。

案例 3 有点棘手，需要解释。逻辑上，此规则的结果应该是在用户暂停路径删除然后切换到新分支时删除路径。然而，这将阻止初始检出发生，因此仅当索引的内容为空时才修改规则以使用 M（新树）。否则，只要$ H 和$ M 相同，就会保留路径的删除。

### 三向合并

每个“索引”条目都有两位“阶段”状态。阶段 0 是正常阶段，并且是您在任何正常使用中看到的唯一一个。

但是，当你用三棵树做 _git read-tree_ 时，“stage”从 1 开始。

这意味着你可以做到

```
$ git read-tree -m <tree1> <tree2> <tree3>
```

你将得到一个包含所有&lt; tree1&gt;的索引“stage1”中的条目，所有&lt; tree2&gt; “stage2”中的条目和所有&lt; tree3&gt; “stage3”中的条目。当执行将另一个分支合并到当前分支时，我们使用公共祖先树作为&lt; tree1&gt;，将当前分支头作为&lt; tree2&gt;，将另一个分支头作为&lt; tree3&gt;。

此外， _git read-tree_ 具有特殊情况逻辑，表示：如果您在以下状态中看到一个在所有方面都匹配的文件，它会“折叠”回“stage0”：

*   第 2 阶段和第 3 阶段是相同的;拿一个或另一个（没有区别 - 我们在第 2 阶段的分支机构和第 3 阶段的分支机构完成了相同的工作）

*   阶段 1 和阶段 2 是相同的，阶段 3 是不同的;进入第 3 阶段（我们在第 2 阶段的分支从第 1 阶段的祖先开始没有做任何事情，而第 3 阶段的分支在第 3 阶段工作）

*   阶段 1 和阶段 3 是相同的，阶段 2 是不同的阶段 2（我们做了什么，而他们什么也没做）

_git write-tree_ 命令拒绝写一个无意义的树，如果它看到一个不是第 0 阶段的单个条目，它会抱怨未合并的条目。

好吧，这听起来像是一个完全荒谬的规则集合，但它实际上正是你想要的快速合并。不同的阶段代表“结果树”（阶段 0，又名“合并”），原始树（阶段 1，又名“orig”），以及您尝试合并的两棵树（分别为阶段 2 和阶段 3）。

当您启动与已填充的索引文件的 3 向合并时，阶段 1,2 和 3 的顺序（因此三个&lt; tree-ish&gt;命令行参数的顺序）非常重要。以下是该算法的工作原理：

*   如果一个文件在所有三个树中以相同的格式存在，它将由 _git read-tree_ 自动折叠为“合并”状态。

*   具有 _ 任何 _ 差异的文件在三棵树中将永远保留为索引中的单独条目。决定如何删除非 0 阶段并插入合并版本取决于“瓷器策略”。

*   索引文件使用所有这些信息保存和恢复，因此您可以逐步合并事物，但只要它具有阶段 1/2/3 中的条目（即“未合并条目”），您就无法写入结果。所以现在合并算法最终变得非常简单：

    *   按顺序遍历索引，并忽略阶段 0 的所有条目，因为它们已经完成。

    *   如果您找到“stage1”，但没有匹配的“stage2”或“stage3”，您知道它已从两个树中删除（它只存在于原始树中），并删除该条目。

    *   如果找到匹配的“stage2”和“stage3”树，则删除其中一个，然后将另一个转换为“stage0”条目。删除任何匹配的“stage1”条目（如果它也存在）。 ..所有正常的琐碎规则..

您通常会使用 _git merge-index_ 与提供的 _git merge-one-file_ 来完成最后一步。该脚本在合并每个路径和成功合并结束时更新工作树中的文件。

当您使用已填充的索引文件启动 3 向合并时，假定它表示工作树中文件的状态，甚至可以在索引文件中包含未记录更改的文件。进一步假设该状态是从阶段 2 树“导出”的。如果在原始索引文件中找到与第 2 阶段不匹配的条目，则 3 向合并将拒绝运行。

这样做是为了防止您丢失正在进行的工作更改，并在不相关的合并提交中混合您的随机更改。为了说明，假设您从最后提交到存储库的内容开始：

```
$ JC=`git rev-parse --verify "HEAD⁰"`
$ git checkout-index -f -u -a $JC
```

你做了随机编辑，没有运行 _git update-index_ 。然后你注意到你的“上游”树的尖端已经提升，因为你从他身上拉了下来：

```
$ git fetch git://.... linus
$ LT=`git rev-parse FETCH_HEAD`
```

您的工作树仍然基于您的 HEAD（$ JC），但您已经进行了一些编辑。三向合并确保你没有添加或修改索引条目，因为$ JC，如果你没有，那么做正确的事情。所以按以下顺序：

```
$ git read-tree -m -u `git merge-base $JC $LT` $JC $LT
$ git merge-index git-merge-one-file -a
$ echo "Merge with Linus" | \
  git commit-tree `git write-tree` -p $JC -p $LT
```

您将提交的是$ JC 和$ LT 之间的纯合并，而不进行正在进行的工作更改，并且您的工作树将更新为合并的结果。

但是，如果工作树中的本地更改将被此合并覆盖， _git read-tree_ 将拒绝运行以防止您的更改丢失。

换句话说，不必担心仅在工作树中存在的内容。如果项目的一部分中没有参与合并，则您的更改不会干扰合并，并且保持不变。当他们**做**干扰时，合并甚至没有开始（ _git read-tree_ 大声抱怨并且在没有修改任何内容的情况下失败）。在这种情况下，您可以继续执行您正在执行的操作，并且当您的工作树已准备好（即您已完成正在进行的工作）时，再次尝试合并。

## SPARSE CHECKOUT

“稀疏检出”允许稀疏地填充工作目录。它使用 skip-worktree 位（参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) ）告诉 Git 工作目录中的文件是否值得查看。

_git read-tree_ 和其他基于合并的命令（ _git merge_ ， _git checkout_ ...）可以帮助维护 skip-worktree 位图和工作目录更新。 `$GIT_DIR/info/sparse-checkout`用于定义跳过工作树参考位图。当 _git read-tree_ 需要更新工作目录时，它会根据此文件重置索引中的 skip-worktree 位，该文件使用与.gitignore 文件相同的语法。如果条目与此文件中的模式匹配，则不会在该条目上设置 skip-worktree。否则，将设置 skip-worktree。

然后它将新的 skip-worktree 值与前一个值进行比较。如果 skip-worktree 从 set 变为 unset，它将添加相应的文件。如果它从未设置变为设置，则该文件将被删除。

虽然`$GIT_DIR/info/sparse-checkout`通常用于指定文件所在的文件，但您也可以使用否定模式指定 _ 而不是 _ 中的文件。例如，要删除文件`unwanted`：

```
/*
!unwanted
```

另一个棘手的问题是，当您不再需要稀疏检出时，完全重新填充工作目录。您不能只禁用“稀疏检出”，因为 skip-worktree 位仍在索引中，并且您的工作目录仍然是稀疏填充的。您应该使用`$GIT_DIR/info/sparse-checkout`文件内容重新填充工作目录，如下所示：

```
/*
```

然后你可以禁用稀疏检出。默认情况下， _git read-tree_ 和类似命令中的稀疏检出支持被禁用。您需要打开`core.sparseCheckout`才能获得稀疏的结帐支持。

## 也可以看看

[git-write-tree [1]](https://git-scm.com/docs/git-write-tree) ; [git-ls-files [1]](https://git-scm.com/docs/git-ls-files) ; [gitignore [5]](https://git-scm.com/docs/gitignore)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件