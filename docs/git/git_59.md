# git-filter-branch

> 原文： [`git-scm.com/docs/git-filter-branch`](https://git-scm.com/docs/git-filter-branch)

## 名称

git-filter-branch - 重写分支

## 概要

```
git filter-branch [--setup <command>] [--subdirectory-filter <directory>]
	[--env-filter <command>] [--tree-filter <command>]
	[--index-filter <command>] [--parent-filter <command>]
	[--msg-filter <command>] [--commit-filter <command>]
	[--tag-name-filter <command>] [--prune-empty]
	[--original <namespace>] [-d <directory>] [-f | --force]
	[--state-branch <branch>] [--] [<rev-list options>…​]
```

## 描述

允许您通过重写&lt; rev-list options&gt;中提到的分支来重写 Git 修订历史记录，在每个修订版上应用自定义过滤器。这些过滤器可以修改每个树（例如，删除文件或对所有文件运行 perl 重写）或有关每个提交的信息。否则，将保留所有信息（包括原始提交时间或合并信息）。

该命令只会重写命令行中提到的 _ 正 _ refs（例如，如果你传递 _a..b_ ，则只会重写 _b_ ）。如果您未指定过滤器，则将重新提交提交而不进行任何更改，这通常无效。然而，这在将来用于补偿某些 Git 错误等方面可能是有用的，因此允许这样的使用。

**注**：该命令用于表示`refs/replace/`命名空间中的`.git/info/grafts`文件和引用。如果您定义了任何移植物或替换引物，则运行此命令将使它们成为永久性的。

**警告**！重写的历史将具有所有对象的不同对象名称，并且不会与原始分支会聚。您将无法在原始分支的顶部轻松推送和分发重写的分支。如果您不知道完整的含义，请不要使用此命令，并且无论如何都要避免使用它，如果简单的单个提交就足以解决您的问题。 （有关重写已发布历史记录的详细信息，请参阅 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 中的“从上游重新恢复”部分。）

始终验证重写版本是否正确：原始引用（如果与重写版本不同）将存储在命名空间 _refs / original /_ 中。

请注意，由于此操作的 I / O 非常昂贵，因此使用`-d`选项重定向磁盘上的临时目录可能是个好主意。在 tmpfs 上。据说加速非常明显。

### 过滤器

过滤器按以下列出的顺序应用。 &lt; command&gt;始终使用 _eval_ 命令在 shell 上下文中评估参数（出于技术原因，提交过滤器有明显的例外）。在此之前，`$GIT_COMMIT`环境变量将被设置为包含要重写的提交的 id。此外，GIT_AUTHOR_NAME，GIT_AUTHOR_EMAIL，GIT_AUTHOR_DATE，GIT_COMMITTER_NAME，GIT_COMMITTER_EMAIL 和 GIT_COMMITTER_DATE 取自当前提交并导出到环境中，以便影响由 [git-commit-tree 创建的替换提交的作者和提交者身份[ 1]过滤器运行后的](https://git-scm.com/docs/git-commit-tree)。

如果对&lt; command&gt;进行任何评估返回非零退出状态，整个操作将被中止。

_map_ 函数可用于获取“原始 sha1 id”参数，如果已经重写了提交，则输出“重写的 sha1 id”，否则输出“original sha1 id”;如果您的提交过滤器发出多次提交， _map_ 函数可以在单独的行上返回多个 ID。

## OPTIONS

```
 --setup <command> 
```

这不是为每次提交执行的实际过滤器，而是在循环之前进行一次设置。因此，尚未定义特定于提交的变量。由于技术原因，除了提交过滤器之外，可以在以下过滤器步骤中使用或修改此处定义的函数或变量。

```
 --subdirectory-filter <directory> 
```

只查看触及给定子目录的历史记录。结果将包含该目录（并且仅包含该目录）作为其项目根目录。意味着重新映射到祖先。

```
 --env-filter <command> 
```

如果您只需要修改将在其中执行提交的环境，则可以使用此过滤器。具体来说，您可能想要重写作者/提交者名称/电子邮件/时间环境变量（有关详细信息，请参阅 [git-commit-tree [1]](https://git-scm.com/docs/git-commit-tree) ）。

```
 --tree-filter <command> 
```

这是用于重写树及其内容的过滤器。参数在 shell 中计算，工作目录设置为签出树的根。然后按原样使用新树（自动添加新文件，自动删除消失文件 - 既没有.gitignore 文件也没有任何其他忽略规则**有任何影响**！）。

```
 --index-filter <command> 
```

这是重写索引的过滤器。它类似于树过滤器，但不检查树，这使得它更快。经常与`git rm --cached --ignore-unmatch ...`一起使用，请参见下面的示例。对于毛病例，请参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 。

```
 --parent-filter <command> 
```

这是用于重写提交的父列表的过滤器。它将在 stdin 上接收父字符串，并在 stdout 上输出新的父字符串。父字符串采用 [git-commit-tree [1]](https://git-scm.com/docs/git-commit-tree) 中描述的格式：初始提交为空，正常提交为“-p parent”，“ - p parent1 -p parent2 -p parent3” ...“用于合并提交。

```
 --msg-filter <command> 
```

这是用于重写提交消息的过滤器。参数在 shell 中使用标准输入上的原始提交消息进行评估;其标准输出用作新的提交消息。

```
 --commit-filter <command> 
```

这是执行提交的过滤器。如果指定了此过滤器，则将调用它，而不是 _git commit-tree_ 命令，其参数形式为“&lt; TREE_ID&gt; [（ - p&lt; PARENT_COMMIT_ID&gt;）...]”和 stdin 上的日志消息。提交标识在 stdout 上是预期的。

作为特殊扩展，提交过滤器可以发出多个提交 ID;在这种情况下，原始提交的重写子项将全部作为父项。

您也可以在此过滤器中使用 _ 地图 _ 便利功能，以及其他便利功能。例如，调用 _skip_commit“$ @”_ 将省略当前提交（但不会更改！如果你想要，请改用 _git rebase_ ）。

如果您不希望保持对单个父项的提交并且不对树进行更改，也可以使用`git_commit_non_empty_tree "$@"`而不是`git commit-tree "$@"`。

```
 --tag-name-filter <command> 
```

这是用于重写标记名称的过滤器。传递时，将为每个指向重写对象（或指向重写对象的标记对象）的标记 ref 调用它。原始标记名称通过标准输入传递，新标记名称在标准输出上是预期的。

原始标签不会被删除，但可以被覆盖;使用“--tag-name-filter cat”来简单地更新标签。在这种情况下，请务必小心并确保备份旧标签，以防转换发生冲突。

支持几乎正确的标记对象重写。如果标记附加了消息，则将使用相同的消息，作者和时间戳创建新的标记对象。如果标签附有签名，则签名将被删除。根据定义，不可能保留签名。这是“几乎”正确的原因，因为理想情况下，如果标签没有改变（指向同一个对象，具有相同的名称等），它应该保留任何签名。情况并非如此，签名将永远删除，买家要小心。也不支持更改作者或时间戳（或标记消息）。指向其他标记的标记将被重写以指向底层提交。

```
 --prune-empty 
```

某些过滤器将生成空提交，使树保持不变。如果 git-filter-branch 只有一个或零个非修剪父项，则该选项指示 git-filter-branch 删除这些提交;因此，合并提交将保持不变。此选项不能与`--commit-filter`一起使用，但通过在提交过滤器中使用提供的`git_commit_non_empty_tree`功能可以实现相同的效果。

```
 --original <namespace> 
```

使用此选项可设置将存储原始提交的命名空间。默认值为 _refs / original_ 。

```
 -d <directory> 
```

使用此选项可设置用于重写的临时目录的路径。应用树过滤器时，该命令需要临时将树检出到某个目录，这可能会在大型项目中占用相当大的空间。默认情况下，它在 _.git-rewrite /_ 目录中执行此操作，但您可以通过此参数覆盖该选项。

```
 -f 
```

```
 --force 
```

_git filter-branch_ 拒绝以现有的临时目录开始，或者当已经使用 _refs / original /_ 开始 refs 时，除非被强制。

```
 --state-branch <branch> 
```

此选项将导致在启动时从命名分支加载从旧对象到新对象的映射，并在退出时将其保存为该分支的新提交，从而实现大树的增量。如果 _&lt; branch&gt;_ 不存在它将被创建。

```
 <rev-list options>…​ 
```

_git rev-list_ 的参数。这些选项包含的所有正面参考都被重写。您也可以指定`--all`等选项，但必须使用`--`将它们与 _git filter-branch_ 选项分开。意味着重新映射到祖先。

### 重新映射到祖先

通过使用 [git-rev-list [1]](https://git-scm.com/docs/git-rev-list) 参数，例如路径限制器，您可以限制被重写的修订集。但是，命令行上的正数引用是有区别的：我们不会让它们被这些限制器排除在外。为此，它们被重写为指向最近的未被排除的祖先。

## 退出状态

成功时，退出状态为`0`。如果过滤器找不到任何要重写的提交，则退出状态为`2`。在任何其他错误上，退出状态可以是任何其他非零值。

## 例子

假设您要从所有提交中删除文件（包含机密信息或侵犯版权）：

```
git filter-branch --tree-filter 'rm filename' HEAD
```

但是，如果某个提交的树中没有该文件，则该树和提交的简单`rm filename`将失败。因此，您可能希望使用`rm -f filename`作为脚本。

将`--index-filter`与 _git rm_ 一起使用会产生明显更快的版本。与使用`rm filename`一样，如果提交树中没有该文件，`git rm --cached filename`将失败。如果你想“完全忘记”一个文件，它在输入历史记录时无关紧要，所以我们也添加了`--ignore-unmatch`：

```
git filter-branch --index-filter 'git rm --cached --ignore-unmatch filename' HEAD
```

现在，您将在 HEAD 中保存重写的历史记录。

要重写存储库，使其看起来好像`foodir/`是其项目根目录，并丢弃所有其他历史记录：

```
git filter-branch --subdirectory-filter foodir -- --all
```

因此，您可以将库子目录转换为自己的存储库。注意`--`将 _filter-branch_ 选项与修订选项分开，`--all`重写所有分支和标记。

要将提交（通常位于另一个历史记录的顶端）设置为当前初始提交的父级，以便将其他历史记录粘贴到当前历史记录之后：

```
git filter-branch --parent-filter 'sed "s/^\$/-p <graft-id>/"' HEAD
```

（如果父字符串为空 - 在我们处理初始提交时发生 - 将 graftcommit 添加为父级）。请注意，这假设具有单个根的历史记录（即，没有共同祖先发生的合并）。如果不是这种情况，请使用：

```
git filter-branch --parent-filter \
	'test $GIT_COMMIT = <commit-id> && echo "-p <graft-id>" || cat' HEAD
```

甚至更简单：

```
git replace --graft $commit-id $graft-id
git filter-branch $graft-id..HEAD
```

删除历史记录中“Darl McBribe”撰写的提交：

```
git filter-branch --commit-filter '
	if [ "$GIT_AUTHOR_NAME" = "Darl McBribe" ];
	then
		skip_commit "$@";
	else
		git commit-tree "$@";
	fi' HEAD
```

函数 _skip_commit_ 定义如下：

```
skip_commit()
{
	shift;
	while [ -n "$1" ];
	do
		shift;
		map "$1";
		shift;
	done;
}
```

移位魔法首先抛弃树 id 然后抛出-p 参数。请注意，此句柄正确合并！如果 Darl 在 P1 和 P2 之间提交了合并，它将被正确传播，并且合并的所有子节点将成为与 P1，P2 作为其父节点而不是合并提交的合并提交。

**注**提交引入的更改以及未被后续提交还原的更改仍将在重写的分支中。如果你想将 _ 更改 _ 和提交一起丢弃，你应该使用 _git rebase_ 的交互模式。

您可以使用`--msg-filter`重写提交日志消息。例如， _git svn_ 创建的存储库中的 _git svn-id_ 字符串可以通过以下方式删除：

```
git filter-branch --msg-filter '
	sed -e "/^git-svn-id:/d"
'
```

如果你需要将 _Acked-by_ 行添加到最后 10 个提交（其中没有一个是合并），请使用以下命令：

```
git filter-branch --msg-filter '
	cat &&
	echo "Acked-by: Bugs Bunny <bunny@bugzilla.org>"
' HEAD~10..HEAD
```

`--env-filter`选项可用于修改提交者和/或作者身份。例如，如果您发现由于配置错误的 user.email 导致您的提交具有错误的标识，则可以在发布项目之前进行更正，如下所示：

```
git filter-branch --env-filter '
	if test "$GIT_AUTHOR_EMAIL" = "root@localhost"
	then
		GIT_AUTHOR_EMAIL=john@example.com
	fi
	if test "$GIT_COMMITTER_EMAIL" = "root@localhost"
	then
		GIT_COMMITTER_EMAIL=john@example.com
	fi
' -- --all
```

要将重写限制为仅部分历史记录，请指定除新分支名称之外的修订范围。新分支名称将指向此范围的 _git rev-list_ 将打印的最高版本。

考虑这段历史：

```
     D--E--F--G--H
    /     /
A--B-----C
```

要仅重写提交 D，E，F，G，H，但仅保留 A，B 和 C，请使用：

```
git filter-branch ... C..H
```

要重写提交 E，F，G，H，请使用以下方法之一：

```
git filter-branch ... C..H --not D
git filter-branch ... D..H --not C
```

要将整个树移动到子目录中，或从中删除它：

```
git filter-branch --index-filter \
	'git ls-files -s | sed "s-\t\"*-&newsubdir/-" |
		GIT_INDEX_FILE=$GIT_INDEX_FILE.new \
			git update-index --index-info &&
	 mv "$GIT_INDEX_FILE.new" "$GIT_INDEX_FILE"' HEAD
```

## 收回存折的清单

git-filter-branch 可用于删除文件的子集，通常使用`--index-filter`和`--subdirectory-filter`的某种组合。人们期望生成的存储库小于原始存储库，但是你需要更多的步骤来实际使它变小，因为 Git 努力不会丢失你的对象，直到你告诉它。首先要确保：

*   如果 blob 在其生命周期内被移动，那么您确实删除了文件名的所有变体。 `git log --name-only --follow --all -- filename`可以帮助您找到重命名。

*   你真的过滤了所有的 refs：在调用 git-filter-branch 时使用`--tag-name-filter cat -- --all`。

然后有两种方法可以获得更小的存储库。更安全的方法是克隆，保持原始原封不动。

*   用`git clone file:///path/to/repo`克隆它。克隆将没有删除的对象。参见 [git-clone [1]](https://git-scm.com/docs/git-clone) 。 （请注意，使用普通路径进行克隆只会将所有内容硬链接！）

如果你真的不想克隆它，无论出于何种原因，请检查以下几点（按此顺序）。这是一种非常具有破坏性的方法，因此**会备份**或者返回克隆它。你被警告了。

*   删除 git-filter-branch 备份的原始引用：说`git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d`。

*   使用`git reflog expire --expire=now --all`使所有 reflogs 到期。

*   垃圾收集所有未引用的对象`git gc --prune=now`（或者如果你的 git-gc 不够新，不支持`--prune`的参数，请改用`git repack -ad; git prune`）。

## 笔记

git-filter-branch 允许您对 Git 历史记录进行复杂的 shell 脚本重写，但如果您只是 _ 删除不需要的数据 _（如大文件或密码），则可能不需要这种灵活性。对于那些操作，您可能需要考虑 [BFG Repo-Cleaner](http://rtyley.github.io/bfg-repo-cleaner/) ，一种基于 JVM 的 git-filter-branch 替代方案，对于这些用例通常至少快 10-50 倍，并且具有完全不同的特性：

*   一旦，任何特定版本的文件都会被准确清除 _。与 git-filter-branch 不同，BFG 不会根据历史记录中提交的位置或时间以不同方式处理文件。这个约束给出了 BFG 的核心性能优势，并且非常适合清理坏数据的任务 - 你不关心哪里有坏数据，你只是想让它 _ 消失 _。_

*   默认情况下，BFG 充分利用多核机器，并行清理提交文件树。 git-filter-branch 按顺序清除提交（即以单线程方式），尽管可以在针对每个提交执行的脚本中编写包含其自身并行性的过滤器。

*   [命令选项](http://rtyley.github.io/bfg-repo-cleaner/#examples)比 git-filter 分支更具限制性，专门用于删除不需要的数据的任务 - 例如：`--strip-blobs-bigger-than 1M`。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件