# git-checkout

> 原文： [`git-scm.com/docs/git-checkout`](https://git-scm.com/docs/git-checkout)

## 名称

git-checkout - 切换分支或恢复工作树文件

## 概要

```
git checkout [-q] [-f] [-m] [<branch>]
git checkout [-q] [-f] [-m] --detach [<branch>]
git checkout [-q] [-f] [-m] [--detach] <commit>
git checkout [-q] [-f] [-m] [[-b|-B|--orphan] <new_branch>] [<start_point>]
git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] [--] <paths>…​
git checkout [<tree-ish>] [--] <pathspec>…​
git checkout (-p|--patch) [<tree-ish>] [--] [<paths>…​]
```

## 描述

更新工作树中的文件以匹配索引或指定树中的版本。如果没有给出路径， _git checkout_ 也会更新`HEAD`以将指定的分支设置为当前分支。

```
 git checkout <branch> 
```

要准备处理&lt; branch&gt;，请通过更新工作树中的索引和文件，并将 HEAD 指向分支来切换到它。保留对工作树中文件的本地修改，以便可以将它们提交到&lt; branch&gt;。

如果&lt; branch&gt;找不到但是在一个遥控器（称为&lt; remote&gt;）中确实存在一个具有匹配名称的跟踪分支，视为等同于

```
$ git checkout -b <branch> --track <remote>/<branch>
```

如果分支存在于多个遥控器中，并且其中一个由`checkout.defaultRemote`配置变量命名，我们将使用该分支用于消除歧义，即使`&lt;branch&gt;`在所有遥控器中都不是唯一的。将其设置为例如如果`&lt;branch&gt;`不明确但存在于 _ 原点 _ 遥控器上，`checkout.defaultRemote=origin`总是从那里检出远程分支。另见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`checkout.defaultRemote`。

你可以省略&lt; branch&gt;，在这种情况下命令退化为“检查当前分支”，这是一个带有相当昂贵的副作用的美化无操作，只显示当前分支的跟踪信息（如果存在） 。

```
 git checkout -b|-B <new_branch> [<start point>] 
```

指定`-b`会导致创建一个新分支，就像调用 [git-branch [1]](https://git-scm.com/docs/git-branch) 然后检出一样。在这种情况下，您可以使用`--track`或`--no-track`选项，这些选项将传递给 _git branch_ 。为方便起见，没有`-b`的`--track`意味着分支创建;请参阅下面的`--track`说明。

如果给出`-B`，则&lt; new_branch&gt;如果它不存在则被创建;否则，它被重置。这是交易的等价物

```
$ git branch -f <branch> [<start point>]
$ git checkout <branch>
```

也就是说，除非“git checkout”成功，否则不会重置/创建分支。

```
 git checkout --detach [<branch>] 
```

```
 git checkout [--detach] <commit> 
```

准备在&lt; commit&gt;之上工作，方法是在其上分离 HEAD（参见“DETACHED HEAD”部分），并更新工作树中的索引和文件。保留对工作树中文件的本地修改，以便生成的工作树将是提交中记录的状态加上本地修改。

当&lt; commit&gt;参数是分支名称，`--detach`选项可用于在分支的末端分离 HEAD（`git checkout &lt;branch&gt;`将检查该分支而不分离 HEAD）。

省略&lt; branch&gt;将 HEAD 分离到当前分支的顶端。

```
 git checkout [<tree-ish>] [--] <pathspec>…​ 
```

通过替换索引中的内容或&lt; tree-ish&gt;中的内容来覆盖工作树中的路径。 （通常是提交）。当&lt; tree-ish&gt;给出了与&lt; pathspec&gt;匹配的路径在索引和工作树中都会更新。

由于先前失败的合并，索引可能包含未合并的条目。默认情况下，如果您尝试从索引中检出此类条目，则结帐操作将失败，并且不会检出任何内容。使用`-f`将忽略这些未合并的条目。可以使用`--ours`或`--theirs`从索引中检出合并的特定一侧的内容。使用`-m`，可以放弃对工作树文件所做的更改，以重新创建原始冲突的合并结果。

```
 git checkout (-p|--patch) [<tree-ish>] [--] [<pathspec>…​] 
```

这类似于上面描述的“从索引或树状结构中检查工作树的路径”，但是允许您使用交互式界面显示“diff”输出并选择要在结果。有关`--patch`选项的说明，请参见下文。

## OPTIONS

```
 -q 
```

```
 --quiet 
```

安静，抑制反馈信息。

```
 --[no-]progress 
```

除非指定`--quiet`，否则默认情况下，标准错误流在连接到终端时会报告进度状态。无论`--quiet`如何，即使未连接到终端，该标志也会启用进度报告。

```
 -f 
```

```
 --force 
```

切换分支时，即使索引或工作树与 HEAD 不同，也要继续。这用于丢弃本地更改。

检查索引中的路径时，不要在未合并的条目上失败;相反，未合并的条目将被忽略。

```
 --ours 
```

```
 --theirs 
```

检查索引中的路径时，请查看阶段＃2（_ 我们的 _）或＃3（_ 他们的 _）的未合并路径。

请注意，在`git rebase`和`git pull --rebase`期间，_ 我们的 _ 和 _ 他们的 _ 可能会出现交换; `--ours`给出了更改被重新分配到分支的版本，而`--theirs`给出了保留您正在重新定位的工作的分支的版本。

这是因为`rebase`用于将远程历史记录视为共享规范的工作流程，并将您要重新分支的分支上的工作视为要集成的第三方工作，并且您暂时假设在 rebase 期间，规范历史的守护者的角色。作为规范历史的守护者，您需要将远程历史记录视为`ours`（即“我们共享的规范历史记录​​”），而您在分支上所做的事情为`theirs`（即“一个贡献者的工作”顶部“）。

```
 -b <new_branch> 
```

创建一个名为&lt; new_branch&gt;的新分支并在&lt; start_point&gt;开始;有关详细信息，请参阅 [git-branch [1]](https://git-scm.com/docs/git-branch) 。

```
 -B <new_branch> 
```

创建分支&lt; new_branch&gt;并在&lt; start_point&gt;开始;如果它已经存在，则将其重置为&lt; start_point&gt;。这相当于用“-f”运行“git branch”;有关详细信息，请参阅 [git-branch [1]](https://git-scm.com/docs/git-branch) 。

```
 -t 
```

```
 --track 
```

创建新分支时，请设置“上游”配置。有关详细信息，请参阅 [git-branch [1]](https://git-scm.com/docs/git-branch) 中的“--track”。

如果没有给出`-b`选项，则通过查看为相应远程配置的 refspec 的本地部分，从远程跟踪分支派生新分支的名称，然后将初始部分剥离到“ *”。这将告诉我们在分支“origin / hack”（或“remotes / origin / hack”，甚至“refs / remotes / origin / hack”）时使用“hack”作为本地分支。如果给定名称没有斜杠，或者上面的猜测结果为空名称，则中止猜测。在这种情况下，您可以使用`-b`明确指定名称。

```
 --no-track 
```

即使 branch.autoSetupMerge 配置变量为 true，也不要设置“上游”配置。

```
 -l 
```

创建新分支的 reflog;有关详细信息，请参阅 [git-branch [1]](https://git-scm.com/docs/git-branch) 。

```
 --detach 
```

不要检查分支机构来处理它，而是检查提交检查和可丢弃的实验。这是“git checkout&lt; commit&gt;”的默认行为何时&lt; commit&gt;不是分支名称。有关详细信息，请参阅下面的“DETACHED HEAD”部分。

```
 --orphan <new_branch> 
```

创建一个名为&lt; new_branch&gt;的新 _ 孤儿 _ 分支，从&lt; start_point&gt;开始。并切换到它。在这个新分支上进行的第一次提交将没有父项，它将成为与所有其他分支和提交完全断开的新历史的根。

调整索引和工作树，就像之前运行“git checkout&lt; start_point&gt;”一样。这允许您开始记录一组类似于&lt; start_point&gt;的路径的新历史记录。通过轻松运行“git commit -a”来进行 root 提交。

当您想要从提交中发布树而不公开其完整历史记录时，这可能很有用。您可能希望这样做以发布项目的开源分支，该分支的当前树是“干净的”，但其完整历史记录包含专有或其他受阻的代码。

如果要启动记录一组与&lt; start_point&gt;完全不同的路径的断开连接的历史记录，则应在创建孤立分支后立即清除索引和工作树，方法是运行“git rm -rf “。从工作树的顶层。之后，您将准备好准备新文件，重新填充工作树，从其他地方复制它们，提取 tarball 等。

```
 --ignore-skip-worktree-bits 
```

在稀疏检出模式中，`git checkout -- &lt;paths&gt;`将仅更新与&lt; paths&gt;匹配的条目。和$ GIT_DIR / info / sparse-checkout 中的稀疏模式。此选项忽略稀疏模式并添加&lt; paths&gt;中的所有文件。

```
 -m 
```

```
 --merge 
```

切换分支时，如果对当前分支和要切换到的分支之间的一个或多个文件进行本地修改，则该命令拒绝切换分支以保留上下文中的修改。但是，使用此选项，当前分支，工作树内容和新分支之间的三向合并已完成，您将进入新分支。

发生合并冲突时，冲突路径的索引条目将保持未合并状态，您需要解决冲突并使用`git add`标记已解析的路径（如果合并应导致路径删除，则为`git rm`）。

从索引检出路径时，此选项允许您在指定路径中重新创建冲突的合并。

```
 --conflict=<style> 
```

与上面的--merge 选项相同，但更改了冲突的帅哥的呈现方式，覆盖了 merge.conflictStyle 配置变量。可能的值是“merge”（默认）和“diff3”（除了“merge”样式显示的内容外，还显示原始内容）。

```
 -p 
```

```
 --patch 
```

以&lt; tree-ish&gt;之间的差异交互式选择帅哥。 （或索引，如果未指定）和工作树。然后将所选择的帅哥反向应用于工作树（并且如果指定了&lt; tree-ish&gt;，则为索引）。

这意味着您可以使用`git checkout -p`有选择地丢弃当前工作树中的编辑内容。请参阅 [git-add [1]](https://git-scm.com/docs/git-add) 的“交互模式”部分，了解如何操作`--patch`模式。

```
 --ignore-other-worktrees 
```

`git checkout`拒绝所需的 ref 已被另一个工作树检出。此选项使其无论如何都会检查引用。换句话说，ref 可以由多个工作树保存。

```
 --[no-]recurse-submodules 
```

使用--recurse-submodules 将根据超级项目中记录的提交更新所有初始化子模块的内容。如果子模块中的局部修改将被覆盖，则除非使用`-f`，否则检出将失败。如果没有使用（或--no-recurse-submodules），子模块的工作树将不会更新。就像 [git-submodule [1]](https://git-scm.com/docs/git-submodule) 一样，这将分离子模块 HEAD。

```
 --no-guess 
```

如果存在同名的远程跟踪分支，请勿尝试创建分支。

```
 <branch> 
```

分店结帐;如果它引用了一个分支（即一个名称，当它以“refs / heads /”为前缀时，是一个有效的引用），则检查该分支。否则，如果它引用了有效的提交，则您的 HEAD 将变为“已分离”，并且您不再处于任何分支上（有关详细信息，请参阅下文）。

您可以使用`"@{-N}"`语法来引用使用“git checkout”操作检出的第 N 个最后一个分支/提交。您也可以指定与`"@{-1}"`同义的`-`。

作为特殊情况，如果只有一个合并库，则可以使用`"A...B"`作为`A`和`B`的合并库的快捷方式。您最多可以省略`A`和`B`中的一个，在这种情况下，它默认为`HEAD`。

```
 <new_branch> 
```

新分支的名称。

```
 <start_point> 
```

用于启动新分支的提交的名称;有关详细信息，请参阅 [git-branch [1]](https://git-scm.com/docs/git-branch) 。默认为 HEAD。

```
 <tree-ish> 
```

从结帐的树（当给出路径时）。如果未指定，将使用索引。

## 脱落的头

HEAD 通常指的是命名分支（例如 _master_ ）。同时，每个分支指的是特定的提交。让我们看一下有三个提交的 repo，其中一个被标记，并且分支 _master_ 签出：

```
           HEAD (refers to branch 'master')
            |
            v
a---b---c  branch 'master' (refers to commit 'c')
    ^
    |
  tag 'v2.0' (refers to commit 'b')
```

在此状态下创建提交时，将更新分支以引用新提交。具体来说， _git commit_ 创建一个新提交 _d_ ，其父级是 _c_ ，然后更新分支 _master_ 以引用新提交 _d_ 。 HEAD 仍然指分支 _master_ ，所以现在间接指的是 commit _d_ ：

```
$ edit; git add; git commit

               HEAD (refers to branch 'master')
                |
                v
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b')
```

有时候能够检出不在任何命名分支尖端的提交，甚至创建一个未被命名分支引用的新提交。让我们来看看当我们签出提交 _b_ 时会发生什么（这里我们展示了两种可能的方法）：

```
$ git checkout v2.0  # or
$ git checkout master^^

   HEAD (refers to commit 'b')
    |
    v
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b')
```

请注意，无论我们使用哪个 checkout 命令，HEAD 现在直接引用 commit _b_ 。这被称为处于分离的 HEAD 状态。它简单地表示 HEAD 引用特定的提交，而不是引用命名的分支。让我们看看创建提交时会发生什么：

```
$ edit; git add; git commit

     HEAD (refers to commit 'e')
      |
      v
      e
     /
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b')
```

现在有一个新的提交 _e_ ，但它仅由 HEAD 引用。我们当然可以在此状态下添加另一个提交：

```
$ edit; git add; git commit

	 HEAD (refers to commit 'f')
	  |
	  v
      e---f
     /
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b')
```

实际上，我们可以执行所有正常的 Git 操作。但是，让我们来看看当我们检出大师时会发生什么：

```
$ git checkout master

               HEAD (refers to branch 'master')
      e---f     |
     /          v
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b')
```

重要的是要意识到，此时没有任何提交 _f_ 。最终提交 _f_ （并通过扩展提交 _e_ ）将被例程 Git 垃圾收集过程删除，除非我们在此之前创建引用。如果我们还没有离开 commit _f_ ，那么其中任何一个都会创建对它的引用：

```
$ git checkout -b foo   (1)
$ git branch foo        (2)
$ git tag foo           (3)
```

1.  创建一个新分支 _foo_ ，它指的是 commit _f_ ，然后更新 HEAD 以引用分支 _foo_ 。换句话说，在此命令之后我们将不再处于分离的 HEAD 状态。

2.  类似地创建一个新的分支 _foo_ ，它指的是 commit _f_ ，但让 HEAD 分离。

3.  创建一个新标签 _foo_ ，它指的是 commit _f_ ，让 HEAD 分离。

如果我们已经离开 commit _f_ ，那么我们必须首先恢复其对象名称（通常使用 git reflog），然后我们可以创建对它的引用。例如，要查看 HEAD 引用的最后两个提交，我们可以使用以下任一命令：

```
$ git reflog -2 HEAD # or
$ git log -g -2 HEAD
```

## 论据歧视

当只有一个参数给出且它不是`--`时（例如“git checkout abc”），并且当参数既是有效`&lt;tree-ish&gt;`（例如分支“abc”存在）又有效`&lt;pathspec&gt;`时（例如，文件或名称为“abc”的目录存在），Git 通常会要求您消除歧义。因为检查分支是一种常见的操作，然而，在这种情况下，“git checkout abc”将“abc”作为`&lt;tree-ish&gt;`。如果要从索引中检出这些路径，请使用`git checkout -- &lt;pathspec&gt;`。

## 例子

1.  以下序列检出`master`分支，将`Makefile`恢复为两个版本，错误地删除 hello.c，并从索引中取回它。

    ```
    $ git checkout master             (1)
    $ git checkout master~2 Makefile  (2)
    $ rm -f hello.c
    $ git checkout hello.c            (3)
    ```

    1.  开关分支

    2.  从另一个提交中取出一个文件

    3.  从索引中恢复 hello.c

    如果你想从索引中查看 _ 所有 _ C 源文件，你可以说

    ```
    $ git checkout -- '*.c'
    ```

    注意`*.c`周围的引号。文件`hello.c`也将被检出，即使它不再在工作树中，因为文件通配符用于匹配索引中的条目（不是由 shell 在工作树中）。

    如果您有一个名为`hello.c`的不幸分支，则此步骤将被混淆为切换到该分支的指令。你应该写：

    ```
    $ git checkout -- hello.c
    ```

2.  在错误的分支中工作后，将使用以下命令切换到正确的分支：

    ```
    $ git checkout mytopic
    ```

    但是，您的“错误”分支和正确的“mytopic”分支可能在您在本地修改的文件中有所不同，在这种情况下，上述签出将会失败，如下所示：

    ```
    $ git checkout mytopic
    error: You have local changes to 'frotz'; not switching branches.
    ```

    您可以为命令提供`-m`标志，该命令将尝试三向合并：

    ```
    $ git checkout -m mytopic
    Auto-merging frotz
    ```

    在这种三向合并之后，本地修改是 _ 而不是 _ 在您的索引文件中注册，因此`git diff`将显示自新分支的提示以来您所做的更改。

3.  在使用`-m`选项切换分支期间发生合并冲突时，您会看到如下内容：

    ```
    $ git checkout -m mytopic
    Auto-merging frotz
    ERROR: Merge conflict in frotz
    fatal: merge program failed
    ```

    此时，`git diff`显示与上一个示例中一样干净地合并的更改，以及冲突文件中的更改。编辑并解决冲突，并像往常一样用`git add`标记它：

    ```
    $ edit frotz
    $ git add frotz
    ```

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件