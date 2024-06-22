# git-rev-list

> 原文： [`git-scm.com/docs/git-rev-list`](https://git-scm.com/docs/git-rev-list)

## 名称

git-rev-list - 以反向时间顺序列出提交对象

## 概要

```
git rev-list [ --max-count=<number> ]
	     [ --skip=<number> ]
	     [ --max-age=<timestamp> ]
	     [ --min-age=<timestamp> ]
	     [ --sparse ]
	     [ --merges ]
	     [ --no-merges ]
	     [ --min-parents=<number> ]
	     [ --no-min-parents ]
	     [ --max-parents=<number> ]
	     [ --no-max-parents ]
	     [ --first-parent ]
	     [ --remove-empty ]
	     [ --full-history ]
	     [ --not ]
	     [ --all ]
	     [ --branches[=<pattern>] ]
	     [ --tags[=<pattern>] ]
	     [ --remotes[=<pattern>] ]
	     [ --glob=<glob-pattern> ]
	     [ --ignore-missing ]
	     [ --stdin ]
	     [ --quiet ]
	     [ --topo-order ]
	     [ --parents ]
	     [ --timestamp ]
	     [ --left-right ]
	     [ --left-only ]
	     [ --right-only ]
	     [ --cherry-mark ]
	     [ --cherry-pick ]
	     [ --encoding=<encoding> ]
	     [ --(author|committer|grep)=<pattern> ]
	     [ --regexp-ignore-case | -i ]
	     [ --extended-regexp | -E ]
	     [ --fixed-strings | -F ]
	     [ --date=<format>]
	     [ [ --objects | --objects-edge | --objects-edge-aggressive ]
	       [ --unpacked ]
	       [ --filter=<filter-spec> [ --filter-print-omitted ] ] ]
	     [ --missing=<missing-action> ]
	     [ --pretty | --header ]
	     [ --bisect ]
	     [ --bisect-vars ]
	     [ --bisect-all ]
	     [ --merge ]
	     [ --reverse ]
	     [ --walk-reflogs ]
	     [ --no-walk ] [ --do-walk ]
	     [ --count ]
	     [ --use-bitmap-index ]
	     <commit>…​ [ -- <paths>…​ ]
```

## 描述

列出通过遵循给定提交的`parent`链接可以访问的提交，但排除可以从它们前面的 _^_ 给出的提交可以访问的提交。默认情况下，输出按反向时间顺序给出。

您可以将其视为一组操作。在命令行上给出的提交形成一组可从其中任何一个提交的提交，然后从该组中减去从前面的 _^_ 给出的任何提交的提交。剩下的提交是命令输出中出现的。可以使用各种其他选项和路径参数来进一步限制结果。

因此，以下命令：

```
	$ git rev-list foo bar ^baz
```

表示“列出所有可从 _foo_ 或 _bar_ ，但不能从 _baz_ ”访问的提交。

特殊符号“_&lt; commit1&gt;_ .. _&lt; commit2&gt;_ ”可用作“^'&lt; commit1&gt;'的缩写 _&lt; commit2&gt;_ “。例如，以下任何一种都可以互换使用：

```
	$ git rev-list origin..HEAD
	$ git rev-list HEAD ^origin
```

另一个特殊符号是“_&lt; commit1&gt;_ ... _&lt; commit2&gt;_ ”，它对合并很有用。生成的提交集是两个操作数之间的对称差异。以下两个命令是等效的：

```
	$ git rev-list A B --not $(git merge-base --all A B)
	$ git rev-list A...B
```

_rev-list_ 是一个非常重要的 Git 命令，因为它提供了构建和遍历提交祖先图的能力。出于这个原因，它有许多不同的选项，使它可以被命令使用，如 _git bisect_ 和 _git repack_ 。

## OPTIONS

### 提交限制

除了使用说明书中解释的特殊符号指定应列出的提交范围之外，还可以应用其他提交限制。

除非另有说明，否则使用更多选项通常会进一步限制输出（例如，`--since=&lt;date1&gt;`限制提交比`&lt;date1&gt;`更新，并将其与`--grep=&lt;pattern&gt;`一起使用进一步限制其日志消息具有与`&lt;pattern&gt;`匹配的行的提交）。

请注意，这些是在提交排序和格式化选项之前应用的，例如`--reverse`。

```
 -<number> 
```

```
 -n <number> 
```

```
 --max-count=<number> 
```

限制要输出的提交数量。

```
 --skip=<number> 
```

在开始显示提交输出之前，跳过 _ 编号 _ 提交。

```
 --since=<date> 
```

```
 --after=<date> 
```

显示比特定日期更新的提交。

```
 --until=<date> 
```

```
 --before=<date> 
```

显示超过特定日期的提交。

```
 --max-age=<timestamp> 
```

```
 --min-age=<timestamp> 
```

将提交输出限制为指定的时间范围。

```
 --author=<pattern> 
```

```
 --committer=<pattern> 
```

将提交输出限制为具有与指定模式（正则表达式）匹配的作者/提交者标题行的输出。对于多个`--author=&lt;pattern&gt;`，选择作者与任何给定模式匹配的提交（类似于多个`--committer=&lt;pattern&gt;`）。

```
 --grep-reflog=<pattern> 
```

将提交输出限制为具有与指定模式（正则表达式）匹配的 reflog 条目的输出。如果有多个`--grep-reflog`，则选择其 reflog 消息与任何给定模式匹配的提交。除非正在使用`--walk-reflogs`，否则使用此选项是错误的。

```
 --grep=<pattern> 
```

将提交输出限制为具有与指定模式（正则表达式）匹配的日志消息的输出。如果有多个`--grep=&lt;pattern&gt;`，则会选择其消息与任何给定模式匹配的提交（但请参见`--all-match`）。

```
 --all-match 
```

将提交输出限制为匹配所有给定`--grep`的输出，而不是匹配至少一个的输出。

```
 --invert-grep 
```

将提交输出限制为具有与`--grep=&lt;pattern&gt;`指定的模式不匹配的日志消息的输出。

```
 -i 
```

```
 --regexp-ignore-case 
```

匹配正则表达式限制模式而不考虑字母大小写。

```
 --basic-regexp 
```

将限制模式视为基本正则表达式;这是默认值。

```
 -E 
```

```
 --extended-regexp 
```

考虑限制模式是扩展正则表达式而不是默认的基本正则表达式。

```
 -F 
```

```
 --fixed-strings 
```

将限制模式视为固定字符串（不要将模式解释为正则表达式）。

```
 -P 
```

```
 --perl-regexp 
```

将限制模式视为与 Perl 兼容的正则表达式。

对这些类型的正则表达式的支持是可选的编译时依赖性。如果 Git 没有编译并支持它们，那么提供此选项将导致它死亡。

```
 --remove-empty 
```

当给定路径从树中消失时停止。

```
 --merges 
```

仅打印合并提交。这与`--min-parents=2`完全相同。

```
 --no-merges 
```

不要打印具有多个父级的提交。这与`--max-parents=1`完全相同。

```
 --min-parents=<number> 
```

```
 --max-parents=<number> 
```

```
 --no-min-parents 
```

```
 --no-max-parents 
```

仅显示至少（或最多）许多父提交的提交。特别是，`--max-parents=1`与`--no-merges`相同，`--min-parents=2`与`--merges`相同。 `--max-parents=0`给出所有 root 提交和`--min-parents=3`所有章鱼合并。

`--no-min-parents`和`--no-max-parents`再次重置这些限制（无限制）。等效形式是`--min-parents=0`（任何提交具有 0 或更多父母）和`--max-parents=-1`（负数表示无上限）。

```
 --first-parent 
```

在看到合并提交时，仅遵循第一个父提交。在查看特定主题分支的演变时，此选项可以提供更好的概述，因为合并到主题分支往往只是关于不时调整到更新的上游，并且此选项允许您忽略引入的单个提交通过这样的合并你的历史。不能与--bisect 结合使用。

```
 --not 
```

反转所有后续修订说明符的 _^_ 前缀（或缺少）的含义，直到下一个`--not`。

```
 --all 
```

假设`refs/`中的所有引用与`HEAD`一起在命令行中列为 _&lt; commit&gt;_ 。

```
 --branches[=<pattern>] 
```

假设`refs/heads`中的所有引用都在命令行中列为 _&lt; commit&gt;_ 。如果 _&lt; pattern&gt;_ 给出，将分支限制为匹配给定 shell glob 的分支。如果模式缺乏 _？最后暗示 _， _*_ 或 _[_， _/ *_ 。

```
 --tags[=<pattern>] 
```

假设`refs/tags`中的所有引用都在命令行中列为 _&lt; commit&gt;_ 。如果 _&lt; pattern&gt;给出了 _，将标签限制为与给定 shell glob 匹配的标签。如果模式缺乏 _？最后暗示 _， _*_ 或 _[_， _/ *_ 。

```
 --remotes[=<pattern>] 
```

假设`refs/remotes`中的所有引用都在命令行中列为 _&lt; commit&gt;_ 。如果 _&lt; pattern&gt;给出了 _，将远程跟踪分支限制为与给定 shell glob 匹配的分支。如果模式缺乏 _？最后暗示 _， _*_ 或 _[_， _/ *_ 。

```
 --glob=<glob-pattern> 
```

假设所有的 refs 匹配 shell glob _&lt; glob-pattern&gt;_ 在命令行中列为 _&lt; commit&gt;_ 。领先的 _refs /_ 会在缺失时自动添加。如果模式缺乏 _？最后暗示 _， _*_ 或 _[_， _/ *_ 。

```
 --exclude=<glob-pattern> 
```

不包括引用匹配 _&lt; glob-pattern&gt;_ 否则会考虑下一个`--all`，`--branches`，`--tags`，`--remotes`或`--glob`。重复此选项会累积排除模式，直至下一个`--all`，`--branches`，`--tags`，`--remotes`或`--glob`选项（其他选项或参数不会清除累积模式）。

当应用于`--branches`，`--tags`或`--remotes`时，给出的模式不应以`refs/heads`，`refs/tags`或`refs/remotes`开始，并且当应用于`--glob`时，它们必须以`refs/`开头]或`--all`。如果打算使用尾随 _/ *_ ，则必须明确给出。

```
 --reflog 
```

假设 reflog 所提到的所有对象都在命令行中列为`&lt;commit&gt;`。

```
 --single-worktree 
```

默认情况下，当有多个工作树时，将通过以下选项检查所有工作树（参见 [git-worktree [1]](https://git-scm.com/docs/git-worktree) ）：`--all`，`--reflog`和`--indexed-objects`。此选项强制它们仅检查当前工作树。

```
 --ignore-missing 
```

在输入中看到无效的对象名称时，假装没有给出错误的输入。

```
 --stdin 
```

除了 _&lt; commit&gt;在命令行中列出 _，从标准输入中读取它们。如果看到`--`分隔符，请停止读取提交并开始读取路径以限制结果。

```
 --quiet 
```

不要将任何东西打印到标准输出。此表单主要用于允许调用者测试退出状态以查看一系列对象是否完全连接（或不完全连接）。它比将 stdout 重定向到`/dev/null`更快，因为输出不必格式化。

```
 --cherry-mark 
```

与`--cherry-pick`（见下文）相同，但标记等效提交与`=`而不是省略它们，而与`+`不等价。

```
 --cherry-pick 
```

当提交集受限于对称差异时，省略任何引用与“另一方”上的另一个提交相同的更改的提交。

例如，如果您有两个分支，`A`和`B`，通常只在一侧列出所有提交的方法是使用`--left-right`（请参阅下面`--left-right`选项说明中的示例） 。但是，它显示了从另一个分支中挑选出来的提交（例如，“b 上的第 3 个”可以从分支 A 中挑选出来）。使用此选项，将从输出中排除此类提交对。

```
 --left-only 
```

```
 --right-only 
```

列表仅在对称差异的相应侧提交，即仅在那些将被标记为`&lt;`的那些。 `&gt;` `--left-right`。

例如，`--cherry-pick --right-only A...B`省略了来自`B`的提交，这些提交位于`A`中，或者补丁等效于`A`中的提交。换句话说，这列出了`git cherry A B`的`+`提交。更确切地说，`--cherry-pick --right-only --no-merges`给出了确切的列表。

```
 --cherry 
```

`--right-only --cherry-mark --no-merges`的同义词;有用的是将输出限制在我们这一侧的提交中，并用`git log --cherry upstream...mybranch`标记已应用于分叉历史记录另一侧的输出，类似于`git cherry upstream mybranch`。

```
 -g 
```

```
 --walk-reflogs 
```

而不是走提交祖先链，将 reflog 条目从最新的条目转到较旧的条目。使用此选项时，您无法指定要排除的提交（即 _^ commit_ ， _commit1..commit2_ ， _commit1 ... commit2_ 表示法不能用过的）。

使用`oneline`以外的`--pretty`格式（出于显而易见的原因），这会导致输出从 reflog 中获取两行额外的信息。输出中的 reflog 指示符可能显示为`ref@{Nth}`（其中`Nth`是 reflog 中的反向时间顺序索引）或`ref@{timestamp}`（带有该条目的时间戳），具体取决于以下几条规则：

1.  如果起始点指定为`ref@{Nth}`，则显示索引格式。

2.  如果起始点指定为`ref@{now}`，则显示时间戳格式。

3.  如果两者均未使用，但在命令行中给出了`--date`，则以`--date`请求的格式显示时间戳。

4.  否则，显示索引格式。

在`--pretty=oneline`下，提交消息在同一行上以此信息为前缀。此选项不能与`--reverse`组合使用。另见 [git-reflog [1]](https://git-scm.com/docs/git-reflog) 。

```
 --merge 
```

合并失败后，show refs 触摸有冲突的文件，并且在所有头上都不存在要合并的文件。

```
 --boundary 
```

输出排除边界提交。边界提交以`-`为前缀。

```
 --use-bitmap-index 
```

尝试使用 pack 位图索引（如果有的话）来加速遍历。请注意，当使用`--objects`遍历时，树和 blob 将不会打印其关联路径。

```
 --progress=<header> 
```

在考虑对象时显示 stderr 的进度报告。每次进度更新都会打印`&lt;header&gt;`文本。

### 历史简化

有时您只对历史记录的某些部分感兴趣，例如修改特定&lt; path&gt;的提交。但 _ 历史简化 _ 有两个部分，一部分是选择提交，另一部分是如何做，因为有各种策略来简化历史。

以下选项选择要显示的提交：

```
 <paths> 
```

提交修改给定的&lt;路径&gt;被选中。

```
 --simplify-by-decoration 
```

选择某些分支或标记引用的提交。

请注意，可以显示额外的提交以提供有意义的历史记录。

以下选项会影响简化的执行方式：

```
 Default mode 
```

将历史简化为最简单的历史，解释树的最终状态。最简单的，因为如果最终结果相同（即合并具有相同内容的分支），它会修剪一些侧分支

```
 --full-history 
```

与默认模式相同，但不修剪某些历史记录。

```
 --dense 
```

仅显示选定的提交，并显示一些具有有意义的历史记录。

```
 --sparse 
```

显示简化历史记录中的所有提交。

```
 --simplify-merges 
```

`--full-history`的附加选项，用于从生成的历史记录中删除一些不必要的合并，因为没有选定的提交有助于此合并。

```
 --ancestry-path 
```

当给出一系列要显示的提交时（例如 _commit1..commit2_ 或 _commit2 ^ commit1_ ），只显示直接存在于 _commit1_ 之间的祖先链上的提交]和 _commit2_ ，即提交既是 _commit1_ 的后代，又是 _commit2_ 的祖先。

下面是更详细的解释。

假设您将`foo`指定为&lt; paths&gt;。我们将调用修改`foo`！TREESAME 的提交，以及其余的 TREESAME。 （在`foo`的差异滤波中，它们分别看起来不同且相等。）

在下文中，我们将始终参考相同的示例历史记录来说明简化设置之间的差异。我们假设您正在过滤此提交图中的文件`foo`：

```
	  .-A---M---N---O---P---Q
	 /     /   /   /   /   /
	I     B   C   D   E   Y
	 \   /   /   /   /   /
	  `-------------'   X
```

历史 A --- Q 的水平线被视为每次合并的第一个父级。提交是：

*   `I`是初始提交，其中`foo`存在内容“asdf”，文件`quux`存在，内容为“quux”。初始提交与空树进行比较，因此`I`是！TREESAME。

*   在`A`中，`foo`仅包含“foo”。

*   `B`包含与`A`相同的更改。它的合并`M`是微不足道的，因此对所有父母都是 TREESAME。

*   `C`不会更改`foo`，但是它的合并`N`会将其更改为“foobar”，因此它不是任何父级的 TREESAME。

*   `D`将`foo`设置为“baz”。它的合并`O`将`N`和`D`的字符串组合成“foobarbaz”;即，它不是任何父母的 TREESAME。

*   `E`将`quux`更改为“xyzzy”，其合并`P`将字符串组合为“quux xyzzy”。 `P`是 TREESAME 到`O`，但不是`E`。

*   `X`是一个独立的根提交，它添加了一个新文件`side`，`Y`修改了它。 `Y`是 TREESAME 到`X`。它的合并`Q`将`side`添加到`P`，`Q`将 TREESAME 添加到`P`，但不添加到`Y`。

`rev-list`根据是否使用`--full-history`和/或父改写（通过`--parents`或`--children`）来回溯历史记录，包括或排除提交。可以使用以下设置。

```
 Default mode 
```

如果对任何父母不是 TREESAME，则包含提交（尽管可以更改，请参阅下面的`--sparse`）。如果提交是合并，并且它是一个父级的 TREESAME，则只关注该父级。 （即使有几个 TREESAME 父母，也只能跟随他们中的一个。）否则，请跟随所有父母。

这导致：

```
	  .-A---N---O
	 /     /   /
	I---------D
```

请注意，仅遵循 TREESAME 父级的规则（如果有的话）将完全从考虑中删除`B`。 `C`被认为是通过`N`，但是是 TREESAME。 Root 提交与空树进行比较，因此`I`是！TREESAME。

父/子关系仅在`--parents`中可见，但这不会影响在默认模式下选择的提交，因此我们显示了父行。

```
 --full-history without parent rewriting 
```

此模式与默认值在一点上不同：始终跟随合并的所有父项，即使它是其中一个的 TREESAME。即使合并的多个方面包含了包含的提交，这并不意味着合并本身就是！在这个例子中，我们得到了

```
	I  A  B  N  D  O  P  Q
```

`M`被排除在外，因为对父母双方都是 TREESAME。 `E`，`C`和`B`都走了，但只有`B`是！TREESAME，所以没有出现。

请注意，如果没有父改写，就不可能谈论提交之间的父/子关系，因此我们将它们显示为断开连接。

```
 --full-history with parent rewriting 
```

只有普通提交才包括在内！TREESAME（虽然可以更改，但请参见下面的`--sparse`）。

合并始终包括在内。但是，它们的父列表会被重写：沿着每个父项删除不包含在其中的提交。这导致了

```
	  .-A---M---N---O---P---Q
	 /     /   /   /   /
	I     B   /   D   /
	 \   /   /   /   /
	  `-------------'
```

与`--full-history`比较而不重写上述内容。请注意，`E`被删除，因为它是 TREESAME，但是 P 的父列表被重写为包含`E`的父`I`。 `C`和`N`，`X`，`Y`和`Q`也发生了同样的情况。

除上述设置外，您还可以更改 TREESAME 是否影响包含：

```
 --dense 
```

如果他们不是任何父母的 TREESAME，则包括走路的提交。

```
 --sparse 
```

包括所有步行的提交。

请注意，如果没有`--full-history`，这仍然可以简化合并：如果其中一个父项是 TREESAME，我们只遵循那个，所以合并的其他方面永远不会走。

```
 --simplify-merges 
```

首先，以与父改写的`--full-history`相同的方式构建历史图（参见上文）。

然后根据以下规则将每个提交`C`简化为最终历史记录中的替换`C'`：

*   将`C'`设置为`C`。

*   将`C'`的每个父`P`替换为其简化`P'`。在这个过程中，删除作为其他父母或祖先的祖先的父母将 TREESAME 提交到空树，并删除重复项，但要注意永远不要删除我们所有父母的 TREESAME。

*   如果在此父级重写之后，`C'`是根或合并提交（具有零或&gt; 1 父级），边界提交或！TREESAME，它仍然存在。否则，它将替换为其唯一的父级。

通过与`--full-history`与父重写进行比较，可以最好地显示出这种效果。这个例子变成了：

```
	  .-A---M---N---O
	 /     /       /
	I     B       D
	 \   /       /
	  `---------'
```

注意`N`，`P`和`Q`与`--full-history`的主要差异：

*   `N`的父列表删除了`I`，因为它是另一个父`M`的祖先。仍然，`N`仍然是因为它是！TREESAME。

*   `P`的父列表同样删除了`I`。 `P`然后被完全删除，因为它有一个父母并且是 TREESAME。

*   `Q`的父列表将`Y`简化为`X`。然后删除`X`，因为它是 TREESAME 根。 `Q`然后被完全删除，因为它有一个父母并且是 TREESAME。

最后，还有第五种简化模式：

```
 --ancestry-path 
```

将显示的提交限制为直接在给定提交范围中的“from”和“to”提交之间的祖先链上的提交。即只显示“to”提交的祖先和“from”提交的后代的提交。

作为示例用例，请考虑以下提交历史记录：

```
	    D---E-------F
	   /     \       \
	  B---C---G---H---I---J
	 /                     \
	A-------K---------------L--M
```

常规 _D..M_ 计算作为`M`的祖先的提交集，但不包括作为`D`的祖先的提交。这对于从`D`以来导致`M`的历史发生了什么是有用的，因为“`D`中没有`M`具有什么`M`”。这个例子中的结果将是所有提交，除了`A`和`B`（当然还有`D`本身）。

当我们想知道`M`中的提交被`D`引入的 bug 污染并需要修复时，我们可能只想查看实际上 _D..M_ 的子集`D`的后代，即排除`C`和`K`。这正是`--ancestry-path`选项的作用。应用于 _D..M_ 范围，它会导致：

```
		E-------F
		 \       \
		  G---H---I---J
			       \
				L--M
```

`--simplify-by-decoration`选项允许您通过省略未由标记引用的提交来仅查看历史拓扑的大图。如果（1）它们被标记引用，或（2）它们改变命令行上给出的路径的内容，则提交被标记为！TREESAME（换句话说，保持在上述历史简化规则之后）。所有其他提交都标记为 TREESAME（可以简化）。

### Bisection Helpers

```
 --bisect 
```

将输出限制为一个提交对象，该提交对象大致位于包含和排除的提交之间。请注意，错误的二分法 ref `refs/bisect/bad`被添加到包含的提交（如果存在）中，并且良好的二分 refs `refs/bisect/good-*`被添加到被排除的提交（如果它们存在）。因此，假设`refs/bisect/`中没有引用，如果

```
	$ git rev-list --bisect foo ^bar ^baz
```

输出 _ 中点 _，两个命令的输出

```
	$ git rev-list foo ^midpoint
	$ git rev-list midpoint ^bar ^baz
```

将是大致相同的长度。因此，查找引入回归的更改将简化为二进制搜索：重复生成并测试新的“中点”，直到提交链长度为 1。不能与--first-parent 结合使用。

```
 --bisect-vars 
```

这计算与`--bisect`相同，只是不使用`refs/bisect/`中的 refs，除了输出准备好由 shell 评估的文本。这些行将中点修订的名称分配给变量`bisect_rev`，并将`bisect_rev`之后要测试的预期提交次数测试为`bisect_nr`，如果`bisect_rev`转动则预期要测试的提交次数如果`bisect_rev`证明对`bisect_bad`不好，那么预计要测试的提交数量，以及我们现在正在等待`bisect_all`的提交数量。

```
 --bisect-all 
```

这将输出包含和排除的提交之间的所有提交对象，按它们与包含和排除的提交的距离排序。不使用`refs/bisect/`中的参考号。首先显示最远离它们的距离。 （这是`--bisect`显示的唯一一个。）

这很有用，因为它可以很容易地选择一个好的提交来测试，当你想避免因某些原因测试它们时（例如它们可能无法编译）。

此选项可与`--bisect-vars`一起使用，在这种情况下，在所有已排序的提交对象之后，将存在与单独使用`--bisect-vars`时相同的文本。

### 提交订购

默认情况下，提交以反向时间顺序显示。

```
 --date-order 
```

在显示所有子项之前不显示父项，但在提交时间戳顺序中显示提交。

```
 --author-date-order 
```

在显示所有子项之前不显示父项，但在作者时间戳顺序中显示提交。

```
 --topo-order 
```

在显示所有子项之前不显示父项，并避免在多行历史记录中显示混合的提交。

例如，在这样的提交历史中：

```
    ---1----2----4----7
	\	       \
	 3----5----6----8---
```

其中数字表示提交时间戳的顺序，`git rev-list`和`--date-order`的朋友按时间戳顺序显示提交：8 7 6 5 4 3 2 1。

使用`--topo-order`，他们将显示 8 6 5 3 7 4 2 1（或 8 7 4 2 6 5 3 1）;为了避免将两个并行开发轨道的提交混合在一起，显示一些较旧的提交在较新的提交之前。

```
 --reverse 
```

输出要以相反顺序显示的提交（请参阅上面的“提交限制”部分）。不能与`--walk-reflogs`结合使用。

### 对象遍历

这些选项主要用于打包 Git 存储库。

```
 --objects 
```

打印列出的提交引用的任何对象的对象 ID。 `--objects foo ^bar`因此意味着“如果我有提交对象 _bar_ 而不是 _foo_ ”，则“发送我需要下载的所有对象 ID”。

```
 --in-commit-order 
```

按提交顺序打印树和 blob id。树和 blob id 在它们首次被提交引用后打印。

```
 --objects-edge 
```

与`--objects`类似，但也打印带有“ - ”字符的排除提交的 ID。 [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) 使用它来构建一个“瘦”包，它根据这些被排除的提交中包含的对象以分层的形式记录对象，以减少网络流量。

```
 --objects-edge-aggressive 
```

与`--objects-edge`类似，但它更难以以增加时间为代价查找排除的提交。这用来代替`--objects-edge`为浅层存储库构建“瘦”包。

```
 --indexed-objects 
```

假装索引使用的所有树和 blob 都在命令行中列出。请注意，您可能也想使用`--objects`。

```
 --unpacked 
```

仅适用于`--objects`;打印不在包中的对象 ID。

```
 --filter=<filter-spec> 
```

仅适用于`--objects*`之一;从打印对象列表中省略对象（通常是 blob）。 _&lt; filter-spec&gt;_ 可能是以下之一：

形式 _--filter = blob：none_ 省略所有 blob。

形式 _--filter = blob：limit =&lt; n&gt; [kmg]_ 省略大于 n 个字节或单位的 blob。 n 可能为零。后缀 k，m 和 g 可用于命名 KiB，MiB 或 GiB 中的单位。例如， _blob：limit = 1k_ 与 _blob 相同：limit = 1024_ 。

形式 _--filter =稀疏：oid =&lt; blob-ish&gt;_ 使用包含在 blob（或 blob-expression）_&lt; blob-ish&gt;中的稀疏检验规范。_ 省略在请求的 refs 上进行稀疏检出时不需要的 blob。

形式 _--filter = sparse：path =&lt; path&gt;_ 类似地使用&lt; path&gt;中包含的稀疏检出规范。

形式 _- 过滤器=树：&lt;深度&gt;_ 省略了从根树的深度&gt; =&lt; depth&gt;的所有斑点和树木。 （如果对象位于所遍历的提交的多个深度处，则为最小深度）。 &lt; depth&gt; = 0 将不包含任何树或 blob，除非在命令行中显式包含（或使用--stdin 时的标准输入）。 &lt; depth&gt; = 1 将仅包括由&lt; commit&gt;可到达的提交直接引用的树和 blob。或明确给定的对象。 &lt; depth&gt; = 2 类似于&lt; depth&gt; = 1，同时还包括从明确给定的提交或树中移除一个级别的树和 blob。

```
 --no-filter 
```

关闭之前的`--filter=`参数。

```
 --filter-print-omitted 
```

仅适用于`--filter=`;打印过滤器省略的对象列表。对象 ID 以“〜”字符为前缀。

```
 --missing=<missing-action> 
```

一个调试选项，以帮助未来的“部分克隆”开发。此选项指定如何处理丢失的对象。

表格 _--missing = error_ 请求如果遇到丢失的对象，则 rev-list 会停止并显示错误。这是默认操作。

如果遇到丢失的对象，表单 _--missing = allow-any_ 将允许对象遍历继续。缺少的对象将默默地从结果中省略。

形式 _--missing = allow-promisor_ 就像 _ 允许任何 _，但只允许对象遍历继续为 EXPECTED promisor 缺少对象。意外丢失的对象将引发错误。

表格 _- 丢失=打印 _ 就像 _ 允许任何 _，但也会打印缺失对象的列表。对象 ID 以“？”字符为前缀。

```
 --exclude-promisor-objects 
```

（仅供内部使用。）Prefilter 对象在 promisor 边界处遍历。这与部分克隆一起使用。这比`--missing=allow-promisor`更强，因为它限制了遍历，而不是仅仅消除有关丢失对象的错误。

```
 --no-walk[=(sorted|unsorted)] 
```

只显示给定的提交，但不要遍历他们的祖先。如果指定了范围，则无效。如果给出了参数`unsorted`，则提交将按命令行中给出的顺序显示。否则（如果`sorted`或没有给出参数），提交按提交时间以反向时间顺序显示。不能与`--graph`结合使用。

```
 --do-walk 
```

覆盖之前的`--no-walk`。

### 提交格式

使用这些选项， [git-rev-list [1]](https://git-scm.com/docs/git-rev-list) 的行为类似于更专业的提交日志工具系列： [git-log [1]](https://git-scm.com/docs/git-log) ， [git-show [1]](https://git-scm.com/docs/git-show) 和 [git-whatchanged [1]](https://git-scm.com/docs/git-whatchanged)

```
 --pretty[=<format>] 
```

```
 --format=<format> 
```

以给定格式打印提交日志的内容，其中 _&lt; format&gt;_ 可以是 _oneline_ ，_ 短 _，_ 培养基 _，_ 全 _，_ 更丰富 _，_ 之一电子邮件 _，_ 原始 _，_ 格式：&lt; string&gt;_ 和 _tformat：&lt; string&gt;_ 。当 _&lt; format&gt;_ 不属于上述情况，并且其中包含 _％占位符 _，其行为就像 _--pretty = tformat：&lt; format&gt;_ 给出了。

有关每种格式的一些其他详细信息，请参阅“PRETTY FORMATS”部分。当 _=&lt; format&gt;_ 部分省略，默认为 _medium_ 。

注意：您可以在存储库配置中指定默认的漂亮格式（请参阅 [git-config [1]](https://git-scm.com/docs/git-config) ）。

```
 --abbrev-commit 
```

而不是显示完整的 40 字节十六进制提交对象名称，而只显示部分前缀。可以使用“--abbrev =&lt; n&gt;”指定非默认位数（如果显示，也会修改 diff 输出）。

对于使用 80 列终端的人来说，这应该使“--pretty = oneline”更具可读性。

```
 --no-abbrev-commit 
```

显示完整的 40 字节十六进制提交对象名称。这否定了`--abbrev-commit`以及暗示它的选项，例如“--oneline”。它还会覆盖`log.abbrevCommit`变量。

```
 --oneline 
```

这是一起使用的“--pretty = oneline --abbrev-commit”的简写。

```
 --encoding=<encoding> 
```

提交对象在其编码头中记录用于日志消息的编码;此选项可用于告诉命令以用户首选的编码重新编码提交日志消息。对于非管道命令，默认为 UTF-8。请注意，如果一个对象声称在`X`中编码并且我们在`X`中输出，我们将逐字输出该对象;这意味着原始提交中的无效序列可能会复制到输出中。

```
 --expand-tabs=<n> 
```

```
 --expand-tabs 
```

```
 --no-expand-tabs 
```

执行选项卡扩展（将每个选项卡替换为足够的空格以填充到日志消息中的 _&lt; n&gt;_ 的倍数的下一个显示列），然后在输出中显示它。 `--expand-tabs`是`--expand-tabs=8`的简写，`--no-expand-tabs`是`--expand-tabs=0`的简写，它会禁用制表符扩展。

默认情况下，选项卡以相当格式展开，将日志消息缩进 4 个空格（即 _medium_ ，默认情况下， _full_ 和 _fulller_ ）。

```
 --show-signature 
```

通过将签名传递给`gpg --verify`并显示输出来检查已签名的提交对象的有效性。

```
 --relative-date 
```

`--date=relative`的同义词。

```
 --date=<format> 
```

仅对以人类可读格式显示的日期生效，例如使用`--pretty`时。 `log.date` config 变量为日志命令的`--date`选项设置默认值。默认情况下，日期显示在原始时区（提交者或作者）中。如果`-local`附加到格式（例如，`iso-local`），则使用用户的本地时区。

`--date=relative`显示相对于当前时间的日期，例如“2 小时前”。 `-local`选项对`--date=relative`无效。

`--date=local`是`--date=default-local`的别名。

`--date=iso`（或`--date=iso8601`）以类似 ISO 8601 的格式显示时间戳。与严格的 ISO 8601 格式的区别在于：

*   空格而不是`T`日期/时间分隔符

*   时区和时区之间的空间

*   时区的小时和分钟之间没有冒号

`--date=iso-strict`（或`--date=iso8601-strict`）以严格的 ISO 8601 格式显示时间戳。

+ `--date=rfc`（或`--date=rfc2822`）以 RFC 2822 格式显示时间戳，通常在电子邮件中找到。

+ `--date=short`仅以`YYYY-MM-DD`格式显示日期，但不显示时间。

+ `--date=raw`显示自纪元以来的秒数（1970-01-01 00:00:00 UTC），后跟一个空格，然后将时区显示为与 UTC 的偏移量（`+`或`-`与四位数;前两位是小时，后两位是分钟）。即，好像时间戳是用`strftime("%s %z")`格式化的。请注意，`-local`选项不会影响秒 - 自 - 纪元值（始终以 UTC 为单位），但会切换附带的时区值。

+ `--date=human`如果时区与当前时区不匹配则显示时区，如果匹配则不显示整个日期（即跳过“今年”日期的打印年份，但也跳过整个日期如果它是在过去几天，我们可以说它是什么工作日）。对于较旧的日期，小时和分钟也被省略。

+ `--date=unix`将日期显示为 Unix 纪元时间戳（自 1970 年以来的秒数）。与`--raw`一样，它始终为 UTC，因此`-local`无效。

+ `--date=format:...`将格式`...`输入系统`strftime`，％z 和％Z 除外，它们在内部处理。使用`--date=format:%c`以系统区域设置的首选格式显示日期。有关格式占位符的完整列表，请参阅`strftime`手册。使用`-local`时，正确的语法是`--date=format-local:...`。

+ `--date=default`是默认格式，类似于`--date=rfc2822`，但有一些例外：

*   星期几之后没有逗号

*   使用本地时区时，省略时区

```
 --header 
```

以 raw 格式打印提交的内容;每个记录用 NUL 字符分隔。

```
 --parents 
```

同时打印提交的父级（以“提交父级”的形式）。也可以启用父重写，请参阅上面的 _ 历史简化 _。

```
 --children 
```

同时打印提交的子项（以“提交子项...”的形式）。也可以启用父重写，请参阅上面的 _ 历史简化 _。

```
 --timestamp 
```

打印原始提交时间戳。

```
 --left-right 
```

标记可以从中获取提交的对称差异的哪一侧。左侧的提示以`&lt;`为前缀，右侧的提示以`&gt;`为前缀。如果与`--boundary`结合使用，则这些提交以`-`为前缀。

例如，如果您有此拓扑：

```
	     y---b---b  branch B
	    / \ /
	   /   .
	  /   / \
	 o---x---a---a  branch A
```

你会得到这样的输出：

```
	$ git rev-list --left-right --boundary --pretty=oneline A...B

	>bbbbbbb... 3rd on b
	>bbbbbbb... 2nd on b
	<aaaaaaa... 3rd on a
	<aaaaaaa... 2nd on a
	-yyyyyyy... 1st on b
	-xxxxxxx... 1st on a
```

```
 --graph 
```

在输出的左侧绘制提交历史的基于文本的图形表示。这可能会导致在提交之间打印额外的行，以便正确绘制图形历史记录。不能与`--no-walk`结合使用。

这使父进行重写，参见上面的 _ 历史简化 _。

默认情况下，这意味着`--topo-order`选项，但也可以指定`--date-order`选项。

```
 --show-linear-break[=<barrier>] 
```

当不使用--graph 时，所有历史分支都被展平，这使得很难看出两个连续的提交不属于线性分支。在这种情况下，此选项会在它们之间设置障碍。如果指定了`&lt;barrier&gt;`，则显示的是字符串而不是默认字符串。

```
 --count 
```

打印一个数字，说明将列出多少次提交，并禁止所有其他输出。与`--left-right`一起使用时，打印左右提交的计数，由制表符分隔。与`--cherry-mark`一起使用时，请忽略这些计数中的补丁等效提交，并打印由选项卡分隔的等效提交的计数。

## 漂亮的格式

如果提交是合并，并且如果漂亮格式不是 _oneline_ ，_ 电子邮件 _ 或 _raw_ ，则在 _ 作者之前插入另一行：_ 行。该行以“Merge：”开头，并且打印祖先提交的 sha1，用空格分隔。请注意，如果您限制了对历史记录的查看，则列出的提交可能不一定是**直接**父提交的列表：例如，如果您只对与某个目录或文件相关的更改感兴趣。

有几种内置格式，您可以通过设置漂亮的格式来定义其他格式。&lt; name&gt;将选项配置为另一种格式名称或 _ 格式：_ 字符串，如下所述（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。以下是内置格式的详细信息：

*   _oneline_

    ```
    &lt;sha1&gt; &lt;title line&gt;
    ```

    这是为了尽可能紧凑。

*   _ 短 _

    ```
    commit &lt;sha1&gt;
    Author: &lt;author&gt;
    ```

    ```
    &lt;title line&gt;
    ```

*   _ 中 _

    ```
    commit &lt;sha1&gt;
    Author: &lt;author&gt;
    Date:   &lt;author date&gt;
    ```

    ```
    &lt;title line&gt;
    ```

    ```
    &lt;full commit message&gt;
    ```

*   _ 全 _

    ```
    commit &lt;sha1&gt;
    Author: &lt;author&gt;
    Commit: &lt;committer&gt;
    ```

    ```
    &lt;title line&gt;
    ```

    ```
    &lt;full commit message&gt;
    ```

*   _ 更丰富 _

    ```
    commit &lt;sha1&gt;
    Author:     &lt;author&gt;
    AuthorDate: &lt;author date&gt;
    Commit:     &lt;committer&gt;
    CommitDate: &lt;committer date&gt;
    ```

    ```
    &lt;title line&gt;
    ```

    ```
    &lt;full commit message&gt;
    ```

*   _ 电子邮件 _

    ```
    From &lt;sha1&gt; &lt;date&gt;
    From: &lt;author&gt;
    Date: &lt;author date&gt;
    Subject: [PATCH] &lt;title line&gt;
    ```

    ```
    &lt;full commit message&gt;
    ```

*   _ 原始 _

    _raw_ 格式显示完整提交，与存储在提交对象中完全相同。值得注意的是，无论是否使用--abbrev 或--no-abbrev，SHA-1 都会完整显示，并且 _ 父 _ 信息显示真正的父提交，而不考虑移植或历史简化。请注意，此格式会影响提交的显示方式，但不会影响显示差异的方式，例如用`git log --raw`。要以原始 diff 格式获取完整对象名称，请使用`--no-abbrev`。

*   _ 格式：&lt; string&gt;_

    _ 格式：&lt; string&gt;_ 格式允许您指定要显示的信息。它的工作方式有点像 printf 格式，但有一个值得注意的例外，即你用 _％n_ 而不是 _\ n_ 获得换行符。

    例如，_ 格式：“％h 的作者是％an，％ar％n 标题是&gt;&gt;％s&lt;&lt;％n”_ 将显示如下内容：

    ```
    The author of fe6e0ee was Junio C Hamano, 23 hours ago
    The title was &gt;&gt;t4119: test autocomputing -p&lt;n&gt; for traditional diff input.&lt;&lt;
    ```

    占位符是：

    *   _％H_ ：提交哈希

    *   _％h_ ：缩写提交哈希

    *   _％T_ ：树形哈希

    *   _％t_ ：缩写树哈希

    *   _％P_ ：父哈希

    *   _％p_ ：缩写为父哈希值

    *   _％和 _：作者姓名

    *   _％aN_ ：作者姓名（尊重.mailmap，见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 或 [git-blame [1]](https://git-scm.com/docs/git-blame) ）

    *   _％ae_ ：作者电邮

    *   _％aE_ ：作者电子邮件（尊重.mailmap，见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 或 [git-blame [1]](https://git-scm.com/docs/git-blame) ）

    *   _％ad_ ：作者日期（格式尊重 - 日期=选项）

    *   _％aD_ ：作者日期，RFC2822 风格

    *   _％ar_ ：作者日期，相对

    *   的 _％：作者日期，UNIX 时间戳 _

    *   _％ai_ ：作者日期，ISO 8601 样格式

    *   _％aI_ ：作者日期，严格的 ISO 8601 格式

    *   _％cn_ ：提交者名称

    *   _％cN_ ：提交者名称（尊重.mailmap，见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 或 [git-blame [1]](https://git-scm.com/docs/git-blame) ）

    *   _％ce_ ：提交者电子邮件

    *   _％cE_ ：提交者电子邮件（尊重.mailmap，参见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 或 [git-blame [1]](https://git-scm.com/docs/git-blame) ）

    *   _％cd_ ：提交者日期（格式尊重 - 日期=选项）

    *   _％cD_ ：提交者日期，RFC2822 样式

    *   _％cr_ ：提交者日期，相对

    *   _％ct_ ：提交者日期，UNIX 时间戳

    *   _％ci_ ：提交者日期，类似 ISO 8601 的格式

    *   _％cI_ ：提交者日期，严格的 ISO 8601 格式

    *   _％d_ ：引用名称，如 [git-log [1]](https://git-scm.com/docs/git-log) 的--decorate 选项

    *   _％D_ ：没有“（”，“）”包装的引用名称。

    *   _％S_ ：在达到提交的命令行上给出的引用名称（如`git log --source`），仅适用于`git log`

    *   _％e_ ：编码

    *   _％s_ ：受试者

    *   _％f_ ：已清理的主题行，适用于文件名

    *   _％b_ ：身体

    *   _％B_ ：生体（未包裹的主体和身体）

    *   _％GG_ ：来自 GPG 的签名提交的原始验证消息

    *   _％G？_ ：显示好的（有效）签名“G”，坏签名显示“B”，有效期未知的好签名显示“U”，已过期的好签名显示“X”，“Y”代表由过期密钥签名的好签名，“R”表示由撤销密钥签名的好签名，“E”表示签名无法检查（例如缺少密钥），“N”表示没有签名

    *   _％GS_ ：显示签名提交的签名者姓名

    *   _％GK_ ：显示用于签署签名提交的密钥

    *   _％GF ​​_：显示用于签署签名提交的密钥的指纹

    *   _％GP_ ：显示主键的指纹，其子键用于签名提交的签名

    *   _％gD_ ：reflog 选择器，例如`refs/stash@{1}`或`refs/stash@{2 minutes ago`};格式遵循`-g`选项描述的规则。 `@`之前的部分是命令行中给出的 refname（因此`git log -g refs/heads/master`将产生`refs/heads/master@{0}`）。

    *   _％gd_ ：缩短了 reflog 选择器;与`%gD`相同，但 refname 部分缩短了人类的可读性（因此`refs/heads/master`变为`master`）。

    *   _％gn_ ：reflog 身份名称

    *   _％gN_ ：reflog 身份名称（尊重.mailmap，见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 或 [git-blame [1]](https://git-scm.com/docs/git-blame) ）

    *   _％ge_ ：reflog 身份电子邮件

    *   _％gE_ ：reflog 身份邮件（尊重.mailmap，见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 或 [git-blame [1]](https://git-scm.com/docs/git-blame) ）

    *   _％gs_ ：reflog 主题

    *   _％Cred_ ：将颜色切换为红色

    *   _％Cgreen_ ：将颜色切换为绿色

    *   _％Cblue_ ：将颜色切换为蓝色

    *   _％Creset_ ：重置颜色

    *   _％C（...）_：颜色规格，如 [git-config [1]](https://git-scm.com/docs/git-config) 的“CONFIGURATION FILE”部分中的值所述。默认情况下，仅在启用日志输出时显示颜色（通过`color.diff`，`color.ui`或`--color`，并且如果我们要去终端，则尊重前者的`auto`设置）。 `%C(auto,...)`被接受为默认的历史同义词（例如，`%C(auto,red)`）。即使没有启用颜色，指定`%C(always,...)`也会显示颜色（尽管只考虑使用`--color=always`为整个输出启用颜色，包括这种格式和其他任何 git 可能颜色的颜色）。单独`auto`（即`%C(auto)`）将打开下一个占位符的自动着色，直到再次切换颜色。

    *   _％m_ ：左（`&lt;`），右（`&gt;`）或边界（`-`）标记

    *   _％n_ ：换行符

    *   _%%_ ：原始 _％_

    *   _％x00_ ：从十六进制代码打印一个字节

    *   _％w（[&lt; w&gt; [，&lt; i1&gt; [，&lt; i2&gt;]]]）_：切换行换行，类似 [git-shortlog [1]的-w 选项](https://git-scm.com/docs/git-shortlog)。

    *   _％&lt;（&lt; N&gt; [，trunc | ltrunc | mtrunc]）_：使下一个占位符至少取 N 列，如果需要，在右边填充空格。如果输出长于 N 列，则可以选择在开头（ltrunc），中间（mtrunc）或结尾（trunc）截断。请注意，截断仅适用于 N&gt; = 2。

    *   _％&lt; |（&lt; N&gt;）_：使下一个占位符至少占用第 N 列，如果需要，在右边填充空格

    *   _％&gt;（&lt; N&gt;）_，_％&gt; |（&lt; N&gt;）_：与 _％相似，_％&lt; |（&lt; N&gt;）_，但左边的填充空格 _

    *   _％&gt;（&lt; N&gt;）_，_％&gt; |（&lt; N&gt;）_：类似于 _％&gt;（&lt; N&gt;分别是 _，_％&gt; |（&lt; N&gt;）_，除非下一个占位符占用的空间多于给定的空间并且左侧有空格，请使用这些空格

    *   _％&gt;&lt;（&lt; N&gt;）_，_％&gt;&lt; |（&lt; N&gt;）_：类似于 _％&lt;（&lt; N&gt; ）_，_％&lt; |（&lt; N&gt;）_，但填充两侧（即文本居中）

    *   ％（预告片[：options]）：显示 [git-interpret-trailers [1]](https://git-scm.com/docs/git-interpret-trailers) 解释的正文预告片。 `trailers`字符串后面可以跟冒号和零个或多个逗号分隔选项。如果给出了`only`选项，则省略拖车块中的非拖车线。如果给出`unfold`选项，则表现得就像给出了 interpre-trailer 的`--unfold`选项一样。例如，`%(trailers:only,unfold)`两者都做。

| 注意 | 一些占位符可能依赖于修订遍历引擎的其他选项。例如，`%g*` reflog 选项将插入一个空字符串，除非我们遍历 reflog 条目（例如，通过`git log -g`）。如果命令行中尚未提供`--decorate`，`%d`和`%D`占位符将使用“短”装饰格式。 |

如果在占位符的 _％_ 之后添加`+`（加号），则在扩展之前插入换行符当且仅当占位符扩展为非空字符串时。

如果在占位符的 _％_ 之后添加`-`（减号），则当且仅当占位符扩展为空字符串时，才会删除紧接在扩展之前的所有连续换行符。

如果在占位符的 _％_ 之后添加一个“空格”，则在扩展之前插入一个空格，当且仅当占位符扩展为非空字符串时。

*   _tformat：_

    _ 格式：_ 格式与 _ 格式完全相同：_，除了它提供“终结符”语义而不是“分隔符”语义。换句话说，每个提交都附加了消息终止符（通常是换行符），而不是在条目之间放置的分隔符。这意味着单行格式的最终​​输入将使用新行正确终止，就像“oneline”格式一样。例如：

    ```
    $ git log -2 --pretty=format:%h 4da45bef \
      | perl -pe '$_ .= " -- NO NEWLINE\n" unless /\n/'
    4da45be
    7134973 -- NO NEWLINE

    $ git log -2 --pretty=tformat:%h 4da45bef \
      | perl -pe '$_ .= " -- NO NEWLINE\n" unless /\n/'
    4da45be
    7134973
    ```

    此外，其中包含`%`的任何无法识别的字符串都被解释为它前面有`tformat:`。例如，这两个是等价的：

    ```
    $ git log -2 --pretty=tformat:%h 4da45bef
    $ git log -2 --pretty=%h 4da45bef
    ```

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件