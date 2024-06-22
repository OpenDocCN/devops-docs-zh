# git-branch

> 原文： [`git-scm.com/docs/git-branch`](https://git-scm.com/docs/git-branch)

## 名称

git-branch - 列出，创建或删除分支

## 概要

```
git branch [--color[=<when>] | --no-color] [-r | -a]
	[--list] [-v [--abbrev=<length> | --no-abbrev]]
	[--column[=<options>] | --no-column] [--sort=<key>]
	[(--merged | --no-merged) [<commit>]]
	[--contains [<commit]] [--no-contains [<commit>]]
	[--points-at <object>] [--format=<format>] [<pattern>…​]
git branch [--track | --no-track] [-f] <branchname> [<start-point>]
git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branchname>]
git branch --unset-upstream [<branchname>]
git branch (-m | -M) [<oldbranch>] <newbranch>
git branch (-c | -C) [<oldbranch>] <newbranch>
git branch (-d | -D) [-r] <branchname>…​
git branch --edit-description [<branchname>]
```

## 描述

如果给出`--list`，或者没有非选项参数，则列出现有分支;当前分支将以星号突出显示。选项`-r`列出远程跟踪分支，选项`-a`显示本地和远程分支。如果给出`<pattern>`，则将其用作 shell 通配符以将输出限制为匹配的分支。如果给出了多个模式，则如果它与任何模式匹配，则显示分支。注意，提供`<pattern>`时，必须使用`--list`;否则该命令被解释为创建分支。

使用`--contains`，仅显示包含命名提交的分支（换句话说，提示提交是指定提交的后代的分支），`--no-contains`将其反面。使用`--merged`，将仅列出已合并到命名提交的分支（即，可以从命名提交到提示提交的分支）。使用`--no-merged`将仅列出未合并到命名提交的分支。如果<commit>参数缺失它默认为`HEAD`（即当前分支的尖端）。

命令的第二个表单创建一个名为<branchname>的新分支头。它指向当前的`HEAD`，或者是<start-point>如果有给予的话。关于<start-point>举一个特殊的例子，你可以使用简写的`"A...B"`取得 A 和 B 的共同父节点，如果他们只存在一个。你可以使用 A、B 中的一个，默认是`HEAD`

请注意，这将创建新分支，但不会将工作树切换到它;使用“git checkout< newbranch>”切换到新分支。

当本地分支从远程跟踪分支启动时，Git 设置分支（特别是`branch.<name>.remote`和`branch.<name>.merge`配置条目），以便 _git pull_ 能适当地从远程跟踪分支合并本地。可以通过全局`branch.autoSetupMerge`配置标志更改此行为。可以使用`--track`和`--no-track`选项覆盖该设置，稍后使用`git branch --set-upstream-to`进行更改。

使用`-m`或`-M`选项，<oldbranch>将重命名为<newbranch>。如果<oldbranch>有一个相应的 reflog，它被重命名为匹配<newbranch>，并创建一个 reflog 条目来记住分支重命名。如果<newbranch>存在，-M 必须用于强制重命名发生。

`-c`和`-C`选项具有与`-m`和`-M`完全相同的语义，除了将分支与其配置重命名，并且 reflog 将被复制到新名称。

使用`-d`或`-D`选项，`<branchname>`将被删除。您可以指定多个分支进行删除。如果分支当前有 reflog，那么 reflog 也将被删除。

将`-r`与`-d`一起使用以删除远程跟踪分支。请注意，如果远程存储库中不再存在远程跟踪分支，或者如果 _git fetch_ 配置为不再获取它们，则删除它们才有意义。另请参阅 [git-remote [1]](https://git-scm.com/docs/git-remote) 的 _prune_ 子命令，了解清除所有过时的远程跟踪分支的方法。

## 选项

```
 -d 
```

```
 --delete 
```

删除分支。分支必须在其上游分支中完全合并，或者如果没有使用`--track`或`--set-upstream-to`设置上游，则必须在`HEAD`中合并。

```
 -D 
```

`--delete --force`的快捷方式。

```
 --create-reflog 
```

创建分支的 reflog。这将激活记录对分支引用所做的所有更改，从而可以使用基于日期的 sha1 表达式，例如“<branchname> @ {yesterday}”。请注意，在非裸存储库中，默认情况下，`core.logAllRefUpdates`配置选项通常会启用 reflog。否定形式`--no-create-reflog`仅覆盖较早的`--create-reflog`，但目前不会否定`core.logAllRefUpdates`的设置。

```
 -f 
```

```
 --force 
```

重置<branchname>到<startpoint>，即使<branchname>已存在。没有`-f`， _git branch_ 拒绝更改现有分支。与`-d`（或`--delete`）结合使用，允许删除分支，而不管其合并状态如何。与`-m`（或`--move`）结合使用，即使新分支名称已存在，也允许重命名分支，这同样适用于`-c`（或`--copy`）。

```
 -m 
```

```
 --move 
```

移动/重命名分支和相应的 reflog。

```
 -M 
```

`--move --force`的快捷方式。

```
 -c 
```

```
 --copy 
```

复制分支和相应的 reflog。

```
 -C 
```

`--copy --force`的快捷方式。

```
 --color[=<when>] 
```

颜色分支以突出显示当前，本地和远程跟踪分支。该值必须为 always（默认值），never 或 auto。

```
 --no-color 
```

即使配置文件提供默认的颜色输出，也要关闭分支颜色。与`--color=never`相同。

```
 -i 
```

```
 --ignore-case 
```

排序和过滤分支不区分大小写。

```
 --column[=<options>] 
```

```
 --no-column 
```

在列中显示分支列表。有关选项语法，请参阅配置变量 column.branch。没有选项的`--column`和`--no-column`分别相当于 _always_ 和 _never_。

此选项仅适用于非详细模式。

```
 -r 
```

```
 --remotes 
```

列出或删除（如果与-d 一起使用）远程跟踪分支。

```
 -a 
```

```
 --all 
```

列出远程跟踪分支和本地分支。

```
 -l 
```

```
 --list 
```

列出分支。使用可选的`<pattern>...`，例如`git branch --list 'maint-*'`，仅列出与模式匹配的分支。

```
 -v 
```

```
 -vv 
```

```
 --verbose 
```

在列表模式下，显示每个头的 sha1 和提交主题行，以及与上游分支（如果有）的关系。如果给出两次，也打印上游分支的名称（另请参见`git remote show <remote>`）。

```
 -q 
```

```
 --quiet 
```

在创建或删除分支时更安静，禁止出现非错误消息。

```
 --abbrev=<length> 
```

在输出列表中更改 sha1 的最小显示长度。默认值为 7，可以通过`core.abbrev`配置选项覆盖。

```
 --no-abbrev 
```

在输出列表中显示完整的 sha1，而不是缩写它们。

```
 -t 
```

```
 --track 
```

创建新分支时，设置`branch.<name>.remote`和`branch.<name>.merge`配置条目以将起点分支标记为新分支的“上游”。此配置将告诉 git 显示`git status`和`git branch -v`中两个分支之间的关系。此外，它在没有参数的情况下指示`git pull`在检出新分支时从上游拉出。

当起点是远程跟踪分支时，此行为是默认行为。如果希望`git checkout`和`git branch`始终表现得像`--no-track`一样，请将 branch.autoSetupMerge 配置变量设置为`false`。如果在起点是本地或远程跟踪分支时需要此行为，请将其设置为`always`。

```
 --no-track 
```

即使 branch.autoSetupMerge 配置变量为 true，也不要设置“上游”配置。

```
 --set-upstream 
```

由于此选项具有令人困惑的语法，因此不再支持它。请改用`--track`或`--set-upstream-to`。

```
 -u <upstream> 
```

```
 --set-upstream-to=<upstream> 
```

设置<branchname>的跟踪信息，以便<upstream>被认为是<branchname>的上游分支。如果<branchname>没有指定上游分支，则默认为当前分支。

```
 --unset-upstream 
```

删除<branchname>的上游信息。如果未指定分支，则默认为当前分支。

```
 --edit-description 
```

打开编辑器并编辑文本以解释分支的用途，供各种其他命令使用（例如`format-patch`，`request-pull`和`merge`（如果已启用））。可以使用多行解释。

```
 --contains [<commit>] 
```

仅列出包含指定提交的分支（如果未指定，则为 HEAD）。意味着`--list`。

```
 --no-contains [<commit>] 
```

仅列出不包含指定提交的分支（如果未指定，则为 HEAD）。意味着`--list`。

```
 --merged [<commit>] 
```

仅列出其提示可从指定提交到达的分支（如果未指定，则为 HEAD）。意味着`--list`，与`--no-merged`不兼容。

```
 --no-merged [<commit>] 
```

仅列出其提示无法从指定的提交访问的分支（如果未指定，则为 HEAD）。意味着`--list`，与`--merged`不兼容。

```
 <branchname> 
```

要创建或删除的分支的名称。新分支名称必须通过 [git-check-ref-format [1]](https://git-scm.com/docs/git-check-ref-format) 定义的所有检查。其中一些检查可能会限制分支名称中允许的字符。

```
 <start-point> 
```

新的分支头将指向此提交。它可以作为分支名称，commit-id 或标记给出。如果省略此选项，则将使用当前 HEAD。

```
 <oldbranch> 
```

要重命名的现有分支的名称。

```
 <newbranch> 
```

现有分支的新名称。与<branchname>相同的限制应用。

```
 --sort=<key> 
```

根据给定的密钥排序。前缀`-`按值的降序排序。您可以使用--sort =<key>选项多次，在这种情况下，最后一个键成为主键。支持的键与`git for-each-ref`中的键相同。排序顺序默认为`branch.sort`变量（如果存在）配置的值，或基于完整 refname（包括`refs/...`前缀）的排序。这首先列出分离的 HEAD（如果存在），然后是本地分支，最后是远程跟踪分支。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

```
 --points-at <object> 
```

仅列出给定对象的分支。

```
 --format <format> 
```

在分支 ref 和它指向的对象中插入显示`%(fieldname)`的字符串。格式与 [git-for-each-ref [1]](https://git-scm.com/docs/git-for-each-ref) 的格式相同。

## 配置

仅在列出分支时，当使用或引用`--list`时，才会遵守`pager.branch`。默认是使用 pager。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

## 例子

```
 Start development from a known tag 
```

```
$ git clone git://git.kernel.org/pub/scm/.../linux-2.6 my2.6
$ cd my2.6
$ git branch my2.6.14 v2.6.14   (1)
$ git checkout my2.6.14
```

1.  这一步和下一步可以通过“checkout -b my2.6.14 v2.6.14”组合成一个步骤。

```
 Delete an unneeded branch 
```

```
$ git clone git://git.kernel.org/.../git.git my.git
$ cd my.git
$ git branch -d -r origin/todo origin/html origin/man   (1)
$ git branch -D test                                    (2)
```

1.  删除远程跟踪分支“todo”，“html”和“man”。下次 _fetch_ 或 _pull_ 将再次创建它们，除非你不配置它们。参见 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 。

2.  即使“master”分支（或当前检出的任何分支）没有来自 test 分支的所有提交，也要删除“test”分支。

## 注意

如果要创建要立即签出的分支，则可以更轻松地使用 git checkout 命令及其`-b`选项来创建分支并使用单个命令将其签出。

选项`--contains`，`--no-contains`，`--merged`和`--no-merged`有四个相关但不同的用途：

*   `--contains <commit>`用于在所有分支中查找包含指定<commit>的分支。如果<commit>是被重新定义或修改的，需要特别注意。

*   `--no-contains <commit>`与此相反，即不包含指定的<commit>的分支。

*   `--merged`用于查找可以安全删除的所有分支，因为这些分支完全被 HEAD 包含。

*   `--no-merged`用于查找要合入 HEAD 的候选分支，因为这些分支未被 HEAD 完全包含。

## 也可以看看

[git-check-ref-format [1]](https://git-scm.com/docs/git-check-ref-format) ， [git-fetch [1]](https://git-scm.com/docs/git-fetch) ， [git-remote [1]](https://git-scm.com/docs/git-remote) ，“了解历史：什么是分支？“在 Git 用户手册中。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件