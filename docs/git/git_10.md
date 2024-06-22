# git-reset

> 原文： [`git-scm.com/docs/git-reset`](https://git-scm.com/docs/git-reset)

## 名称

git-reset - 将当前 HEAD 重置为指定状态

## 概要

```
git reset [-q] [<tree-ish>] [--] <paths>…​
git reset (--patch | -p) [<tree-ish>] [--] [<paths>…​]
git reset [--soft | --mixed [-N] | --hard | --merge | --keep] [-q] [<commit>]
```

## 描述

在第一种和第二种形式中，将条目从`<tree-ish>`复制到索引。在第三种形式中，将当前分支头（`HEAD`）设置为`<commit>`，可选择修改索引和工作树以匹配。所有形式的`<tree-ish>` / `<commit>`默认为`HEAD`。

```
 git reset [-q] [<tree-ish>] [--] <paths>…​ 
```

此表单将所有`<paths>`的索引条目重置为`<tree-ish>`的状态。（它不会影响工作树或当前分支。）

这意味着`git reset <paths>`与`git add <paths>`相反。

运行`git reset <paths>`更新索引条目后，可以使用 [git-checkout [1]](https://git-scm.com/docs/git-checkout) 检查工作树索引中的内容。或者，使用 [git-checkout [1]](https://git-scm.com/docs/git-checkout) 并指定提交，您可以一次性将提交中的路径内容复制到索引和工作树。

```
 git reset (--patch | -p) [<tree-ish>] [--] [<paths>…​] 
```

在索引和`<tree-ish>`（默认为`HEAD`）之间的差异中交互式选择某块。选择的块与索引相反。

这意味着`git reset -p`与`git add -p`相反，即您可以使用它来选择性地重置代码块。请参阅 [git-add [1]](https://git-scm.com/docs/git-add) 的“交互模式”部分，了解如何操作`--patch`模式。

```
 git reset [<mode>] [<commit>] 
```

此表单将当前分支头重置为`<commit>`，并可能根据`<mode>`更新索引（将其重置为`<commit>`的树）和工作树。如果省略`<mode>`，则默认为`--mixed`。 `<mode>`必须是以下之一：

```
 --soft 
```

根本不接触索引文件或工作树（但将头节点重置为`<commit>`，就像所有模式一样）。这将保留所有已更改的文件“要提交的更改”，如`git status`所示。

```
 --mixed 
```

重置索引但不重置工作树（即，保留更改的文件但未标记为提交）并报告尚未更新的内容。这是默认操作。

如果指定了`-N`，则删除的路径将标记为意图添加（请参阅 [git-add [1]](https://git-scm.com/docs/git-add) ）。

```
 --hard 
```

重置索引和工作树。自`<commit>`以来对工作树中跟踪文件的任何更改都将被丢弃。

```
 --merge 
```

重置索引并更新工作树中`<commit>`和`HEAD`之间不同的文件，但保留索引和工作树之间不同的文件（即具有尚未添加的更改）。如果`<commit>`和索引之间不同的文件具有未暂存更改，则重置将中止。

换句话说，`--merge`执行类似`git read-tree -u -m <commit>`的操作，但会继承未合并的索引条目。

```
 --keep 
```

重置索引条目并更新工作树中`<commit>`和`HEAD`之间不同的文件。如果`<commit>`和`HEAD`之间不同的文件具有本地更改，则重置将中止。

如果你想撤消除分支上最新的提交， [git-revert [1]](https://git-scm.com/docs/git-revert) 是你的朋友。

## 选项

```
 -q 
```

```
 --quiet 
```

```
 --no-quiet 
```

保持安静，只报告错误。默认行为由`reset.quiet`配置选项设置。 `--quiet`和`--no-quiet`将覆盖默认行为。

## 例子

```
 Undo add 
```

```
$ edit                                     (1)
$ git add frotz.c filfre.c
$ mailx                                    (2)
$ git reset                                (3)
$ git pull git://info.example.com/ nitfol  (4)
```

1.  您正在愉快地处理某些事情，并发现这些文件中的更改处于良好状态。当您运行`git diff`时，您不希望看到它们，因为您计划处理其他文件，并且使用这些文件进行更改会分散注意力。

2.  有人要求你拉更新，这些变化听起来值得合并。

3.  但是，您已经弄脏了索引（即您的索引与`HEAD`提交不匹配）。但是你知道你要做的更新不会影响`frotz.c`或`filfre.c`，所以你还原这两个文件的索引变化。您在工作树中的更改仍然存在。

4.  然后你可以更新和合并，在工作树中仍然保留`frotz.c`和`filfre.c`。

```
 Undo a commit and redo 
```

```
$ git commit ...
$ git reset --soft HEAD^      (1)
$ edit                        (2)
$ git commit -a -c ORIG_HEAD  (3)
```

1.  当您记得刚刚提交的内容不完整，或者拼错了提交消息，或者两者兼而有之时，通常会这样做。在“重置”之前留下工作树。

2.  对工作树文件进行更正。

3.  “重置”将旧头复制到`.git/ORIG_HEAD`;通过从其日志消息开始重做提交。如果您不需要进一步编辑消息，则可以改为使用`-C`选项。

另请参见 [git-commit [1]](https://git-scm.com/docs/git-commit) 的`--amend`选项。

```
 Undo a commit, making it a topic branch 
```

```
$ git branch topic/wip     (1)
$ git reset --hard HEAD~3  (2)
$ git checkout topic/wip   (3)
```

1.  你做了一些提交，但意识到他们在`master`分支中还为时过早。您想在主题分支中继续抛光它们，因此从当前`HEAD`创建`topic/wip`分支。

2.  倒回 master 分支以摆脱这三个提交。

3.  切换到`topic/wip`分支并继续工作。

```
 Undo commits permanently 
```

```
$ git commit ...
$ git reset --hard HEAD~3   (1)
```

1.  最后三次提交（`HEAD`，`HEAD^`和`HEAD~2`）很糟糕，你不想再看到它们。如果你已经将这些提交给了其他人，那么**不要**这样做。 （有关这样做的含义，请参阅 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 中的“从上游返回的恢复”部分。）

```
 Undo a merge or pull 
```

```
$ git pull                         (1)
Auto-merging nitfol
CONFLICT (content): Merge conflict in nitfol
Automatic merge failed; fix conflicts and then commit the result.
$ git reset --hard                 (2)
$ git pull . topic/branch          (3)
Updating from 41223... to 13134...
Fast-forward
$ git reset --hard ORIG_HEAD       (4)
```

1.  尝试从上游更新导致了很多冲突;你现在还没有准备好花很多时间合并，所以你决定稍后再这样做。

2.  “pull”没有进行合并提交，因此作为`git reset --hard HEAD`同义词的`git reset --hard`清除了索引文件和工作树中的混乱。

3.  将主题分支合并到当前分支，这导致快进。

4.  但是你决定主题分支尚未准备好供公众使用。 “pull”或“merge”总是在`ORIG_HEAD`中保留当前分支的原始提示，因此重新设置它会使您的索引文件和工作树返回到该状态，并将分支的提示重置为该提交。

```
 Undo a merge or pull inside a dirty working tree 
```

```
$ git pull                         (1)
Auto-merging nitfol
Merge made by recursive.
 nitfol                |   20 +++++----
 ...
$ git reset --merge ORIG_HEAD      (2)
```

1.  即使您可能在工作树中进行了本地修改，当您知道另一个分支中的更改与它们不重叠时，您也可以安全地说出`git pull`。

2.  检查合并结果后，您可能会发现另一个分支的更改不能令人满意。运行`git reset --hard ORIG_HEAD`将让您回到原来的位置，但它会丢弃您不想要的本地更改。 `git reset --merge`保留您的本地更改。

```
 Interrupted workflow 
```

假设您在进行大量更改时被紧急修复请求打断。工作树中的文件尚未提交任何形状，但您需要到另一个分支进行快速修复。

```
$ git checkout feature ;# you were working in "feature" branch and
$ work work work       ;# got interrupted
$ git commit -a -m "snapshot WIP"                 (1)
$ git checkout master
$ fix fix fix
$ git commit ;# commit with real log
$ git checkout feature
$ git reset --soft HEAD^ ;# go back to WIP state  (2)
$ git reset                                       (3)
```

1.  这个提交将被吹走，所以扔掉日志消息就可以了。

2.  这将从提交历史记录中删除 _WIP_ 提交，并将工作树设置为创建快照之前的状态。

3.  此时，索引文件仍然包含您作为 _ 快照 WIP_ 提交的所有 WIP 更改。这将更新索引以将您的 WIP 文件显示为未提交。

另见 [git-stash [1]](https://git-scm.com/docs/git-stash) 。

```
 Reset a single file in the index 
```

假设您已将文件添加到索引中，但后来决定不想将其添加到提交中。您可以从索引中删除文件，同时使用 git reset 保留更改。

```
$ git reset -- frotz.c                      (1)
$ git commit -m "Commit files in index"     (2)
$ git add frotz.c                           (3)
```

1.  这会将文件从索引中删除，同时将其保留在工作目录中。

2.  这将提交索引中的所有其他更改。

3.  再次将文件添加到索引。

```
 Keep changes in working tree while discarding some previous commits 
```

假设您正在处理某些事情并且提交它，然后您继续工作，但现在您认为您工作树中的内容应该位于与您之前提交的内容无关的另一个分支中。您可以启动新分支并重置它，同时保持工作树中的更改。

```
$ git tag start
$ git checkout -b branch1
$ edit
$ git commit ...                            (1)
$ edit
$ git checkout -b branch2                   (2)
$ git reset --keep start                    (3)
```

1.  这将在`branch1`中提交您的第一次编辑。

2.  在理想世界中，您可能已经意识到，当您创建并切换到`branch2`（即`git checkout -b branch2 start`）时，较早的提交不属于新主题，但没有人是完美的。

3.  但是，切换到`branch2`后，可以使用`reset --keep`删除不需要的提交。

```
 Split a commit apart into a sequence of commits 
```

假设您已经创建了许多逻辑上独立的更改并将它们一起提交。然后，稍后您决定让每个逻辑块与其自己的提交相关联可能更好。您可以使用 git reset 来回滚历史记录而不更改本地文件的内容，然后使用`git add -p`以交互方式选择要包含在每个提交中的数据库，使用`git commit -c`预先填充提交消息。

```
$ git reset -N HEAD^                        (1)
$ git add -p                                (2)
$ git diff --cached                         (3)
$ git commit -c HEAD@{1}                    (4)
...                                         (5)
$ git add ...                               (6)
$ git diff --cached                         (7)
$ git commit ...                            (8)
```

1.  首先，将历史记录重置为一次提交，以便我们删除原始提交，但保留工作树中的所有更改。 -N 确保添加`HEAD`的任何新文件仍然被标记，以便`git add -p`找到它们。

2.  接下来，我们使用`git add -p`工具以交互方式选择要添加的差异。这将按顺序询问每个差异块，您可以使用简单的命令，例如“是，包括此”，“不包括此”，甚至是非常强大的“编辑”工具。

3.  一旦对您想要包含的代码块感到满意，您应该使用`git diff --cached`为第一次提交准备的内容验证。这显示了已移入索引并即将提交的所有更改。

4.  接下来，提交存储在索引中的更改。 `-c`选项指定从第一次提交中启动的原始消息预填充提交消息。这有助于避免重新输入。 `HEAD@{1}`是`HEAD`曾经在原始重置提交之前进行的提交的特殊表示法（1 更改前）。有关详细信息，请参阅 [git-reflog [1]](https://git-scm.com/docs/git-reflog) 。您还可以使用任何其他有效的提交引用。

5.  您可以多次重复步骤 2-4，将原始代码分解为任意数量的提交。

6.  现在，您已将许多更改拆分为自己的提交，并且可能不再使用`git add`的修补程序模式，以便选择所有剩余的未提交更改。

7.  再次检查以确认您已包含所需内容。您可能还希望验证 git diff 不会显示稍后要提交的任何剩余更改。

8.  最后创建最终提交。

## 讨论

下表显示了运行时会发生什么：

```
git reset --option target
```

根据文件的状态，使用不同的重置选项将`HEAD`重置为另一个提交（`target`）。

在这些表中，`A`，`B`，`C`和`D`是文件的某些不同状态。例如，第一个表的第一行表示如果文件在工作树中处于状态`A`，在索引中处于状态`B`，在`HEAD`上是状态`C`，在目标节点中是状态`D`，`git reset --soft target`将文件保留在状态`A`的工作树中和状态`B`的索引中。它将`HEAD`（即当前分支的尖端，如果你在其中）重置（即移动）到`target`（其文件处于状态`D`）。

```
working index HEAD target         working index HEAD
----------------------------------------------------
 A       B     C    D     --soft   A       B     D
			  --mixed  A       D     D
			  --hard   D       D     D
			  --merge (disallowed)
			  --keep  (disallowed)
```

```
working index HEAD target         working index HEAD
----------------------------------------------------
 A       B     C    C     --soft   A       B     C
			  --mixed  A       C     C
			  --hard   C       C     C
			  --merge (disallowed)
			  --keep   A       C     C
```

```
working index HEAD target         working index HEAD
----------------------------------------------------
 B       B     C    D     --soft   B       B     D
			  --mixed  B       D     D
			  --hard   D       D     D
			  --merge  D       D     D
			  --keep  (disallowed)
```

```
working index HEAD target         working index HEAD
----------------------------------------------------
 B       B     C    C     --soft   B       B     C
			  --mixed  B       C     C
			  --hard   C       C     C
			  --merge  C       C     C
			  --keep   B       C     C
```

```
working index HEAD target         working index HEAD
----------------------------------------------------
 B       C     C    D     --soft   B       C     D
			  --mixed  B       D     D
			  --hard   D       D     D
			  --merge (disallowed)
			  --keep  (disallowed)
```

```
working index HEAD target         working index HEAD
----------------------------------------------------
 B       C     C    C     --soft   B       C     C
			  --mixed  B       C     C
			  --hard   C       C     C
			  --merge  B       C     C
			  --keep   B       C     C
```

`reset --merge`用于在重置合并冲突时使用。任何 mergy 操作都应保证在工作树中涉及的合的文件，在开始合并之前，本地索引没有相应更改，并且它将合并结果写入工作树中。因此，如果我们看到索引和目标之间以及索引和工作树之间存在某些差异，那么这意味着当由于冲突导致合并失败后，我们不能通过 reset 操作将状态重置出来。这就是我们在这种情况下不允许`--merge`选项的原因。

在保留当前分支中的一些最后提交的同时保留工作树中的更改时，将使用`reset --keep`。如果我们要删除的提交中的更改与我们要保留的工作树中的更改之间可能存在冲突，则不允许重置。如果工作树和`HEAD`之间以及`HEAD`和目标之间存在变化，那么就不允许这样做。为了安全起见，当有未合并的条目时也不允许这样做。

下表显示了存在未合并条目时会发生什么：

```
working index HEAD target         working index HEAD
----------------------------------------------------
 X       U     A    B     --soft  (disallowed)
			  --mixed  X       B     B
			  --hard   B       B     B
			  --merge  B       B     B
			  --keep  (disallowed)
```

```
working index HEAD target         working index HEAD
----------------------------------------------------
 X       U     A    A     --soft  (disallowed)
			  --mixed  X       A     A
			  --hard   A       A     A
			  --merge  A       A     A
			  --keep  (disallowed)
```

`X`表示任何状态，`U`表示未合并的索引。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件