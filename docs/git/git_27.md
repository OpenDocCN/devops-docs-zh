# git-log

> 原文： [`git-scm.com/docs/git-log`](https://git-scm.com/docs/git-log)

## 名称

git-log - 显示提交日志

## 概要

```
git log [<options>] [<revision range>] [[--] <path>…​]
```

## 描述

显示提交日志。

该命令采用适用于`git rev-list`命令的选项来控制显示的内容和方式，以及适用于`git diff-*`命令的选项，以控制每个提交引入的更改的显示方式。

## OPTIONS

```
 --follow 
```

继续列出重命名以外的文件历史记录（仅适用于单个文件）。

```
 --no-decorate 
```

```
 --decorate[=short|full|auto|no] 
```

打印出所有提交的引用名称。如果指定 _ 短 _，则引用名称前缀 _refs / heads /_ ， _refs / tags /_ 和 _refs / remotes /_ 将不会打印。如果指定了 _full_ ，将打印完整的引用名称（包括前缀）。如果指定了 _auto_ ，那么如果输出到达终端，则 ref 名称显示为 _short_ ，否则不显示 ref 名称。默认选项为 _ 短 _。

```
 --decorate-refs=<pattern> 
```

```
 --decorate-refs-exclude=<pattern> 
```

如果没有给出`--decorate-refs`，假装好像所有参考都被包括在内。对于每个候选人，如果它与`--decorate-refs-exclude`给出的任何模式匹配或者与`--decorate-refs`给出的任何模式都不匹配，请不要将其用于装饰。

```
 --source 
```

打印出在每个提交到达的命令行上给出的引用名称。

```
 --use-mailmap 
```

使用 mailmap 文件将作者和提交者名称以及电子邮件地址映射到规范的真实姓名和电子邮件地址。见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 。

```
 --full-diff 
```

如果没有此标志，`git log -p &lt;path&gt;...`将显示触摸指定路径的提交，并显示相同指定路径的差异。这样，就会显示完全差异，以便接触指定的路径。这意味着“&lt; path&gt; ...”仅限制提交，并不限制这些提交的差异。

请注意，这会影响所有基于差异的输出类型，例如：那些由`--stat`等产生的

```
 --log-size 
```

在每次提交的输出中包含“日志大小&lt; number&gt;”行，其中&lt; number&gt;是以字节为单位的提交消息的长度。旨在通过允许它们提前分配空间来加速从`git log`输出读取日志消息的工具。

```
 -L <start>,<end>:<file> 
```

```
 -L :<funcname>:<file> 
```

跟踪“&lt; start&gt;，&lt; end&gt;”给出的行范围的演变（或&lt; file&gt;）中的（或函数名称 regex&lt; funcname&gt;）。你可能不会给任何 pathpec 限制器。目前这仅限于从单个修订开始的步行，即，您可能只提供零个或一个正修订参数。您可以多次指定此选项。

&lt;开始&gt;和&lt; end&gt;可以采取以下形式之一：

*   数

    如果&lt; start&gt;或者&lt; end&gt;是一个数字，它指定一个绝对行号（行数从 1 开始）。

*   /正则表达式/

    此表单将使用与给定 POSIX 正则表达式匹配的第一行。如果&lt; start&gt;是一个正则表达式，它将从前一个`-L`范围的末尾搜索，如果有的话，否则从文件的开头搜索。如果&lt; start&gt;是“^ / regex /”，它将从文件的开头搜索。如果&lt; end&gt;是一个正则表达式，它将从&lt; start&gt;给出的行开始搜索。

*   + offset 或-offset

    这仅适用于&lt; end&gt;并将在&lt; start&gt;给出的行之前或之后指定行数。

如果给出“：&lt; funcname&gt;”代替&lt; start&gt;和&lt; end&gt;，它是一个正则表达式，表示从匹配&lt; funcname&gt;的第一个 funcname 行到下一个 funcname 行的范围。 “：&lt; funcname&gt;”从上一个`-L`范围的末尾搜索（如果有的话），否则从文件的开头搜索。 “^：&lt; funcname&gt;”从文件的开头搜索。

```
 <revision range> 
```

仅显示指定修订范围内的提交。当没有&lt;修订范围&gt;如果指定，则默认为`HEAD`（即导致当前提交的整个历史记录）。 `origin..HEAD`指定从当前提交可以访问的所有提交（即`HEAD`），但不是`origin`。有关拼写&lt; revision range&gt;的完整列表，请参阅 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 的 _ 指定范围 _ 部分。

```
 [--] <path>…​ 
```

仅显示足以解释与指定路径匹配的文件的提交。有关详细信息和其他简化模式，请参见下面的 _ 历史简化 _。

当出现混淆时，路径可能需要以`--`作为前缀，以将它们与选项或修订范围分开。

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

当`--show-notes`生效时，来自注释的消息将被匹配，就像它是日志消息的一部分一样。

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
 --bisect 
```

假设好像已经列出了错误的二分法`refs/bisect/bad`并且好像它后面跟着`--not`并且良好的二分法在命令行上引用了`refs/bisect/good-*`。不能与--first-parent 结合使用。

```
 --stdin 
```

除了 _&lt; commit&gt;在命令行中列出 _，从标准输入中读取它们。如果看到`--`分隔符，请停止读取提交并开始读取路径以限制结果。

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
 --no-walk[=(sorted|unsorted)] 
```

只显示给定的提交，但不要遍历他们的祖先。如果指定了范围，则无效。如果给出了参数`unsorted`，则提交将按命令行中给出的顺序显示。否则（如果`sorted`或没有给出参数），提交按提交时间以反向时间顺序显示。不能与`--graph`结合使用。

```
 --do-walk 
```

覆盖之前的`--no-walk`。

### 提交格式

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
 --notes[=<treeish>] 
```

在显示提交日志消息时，显示注释提交的注释（请参阅 [git-notes [1]](https://git-scm.com/docs/git-notes) ）。当命令行中没有给出`--pretty`，`--format`或`--oneline`选项时，这是`git log`，`git show`和`git whatchanged`命令的默认值。

默认情况下，显示的注释来自`core.notesRef`和`notes.displayRef`变量（或相应的环境覆盖）中列出的注释 refs。有关详细信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 。

使用可选的 _&lt; treeish&gt;_ 参数，使用树形查找要显示的注释。树形可以在以`refs/notes/`开头时指定完整的引用名称;当它以`notes/`开始时，`refs/`和其他`refs/notes/`作为前缀以形成 ref 的全名。

可以组合多个--notes 选项来控制显示哪些音符。示例：“ - notes = foo”将仅显示“refs / notes / foo”中的注释; “--notes = foo --notes”将显示“refs / notes / foo”和默认音符 ref（s）中的两个音符。

```
 --no-notes 
```

不要显示笔记。这取消了上面的`--notes`选项，通过重置显示注释的注释列表。选项按命令行中给出的顺序进行解析，例如， “--notes --notes = foo --no-notes --notes = bar”只会显示“refs / notes / bar”中的注释。

```
 --show-notes[=<treeish>] 
```

```
 --[no-]standard-notes 
```

不推荐使用这些选项。请使用上面的--notes / - no-notes 选项。

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
 --parents 
```

同时打印提交的父级（以“提交父级”的形式）。也可以启用父重写，请参阅上面的 _ 历史简化 _。

```
 --children 
```

同时打印提交的子项（以“提交子项...”的形式）。也可以启用父重写，请参阅上面的 _ 历史简化 _。

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

### 差异格式化

下面列出了控制 diff 输出格式的选项。其中一些特定于 [git-rev-list [1]](https://git-scm.com/docs/git-rev-list) ，但是可以给出其他差异选项。有关更多选项，请参阅 [git-diff-files [1]](https://git-scm.com/docs/git-diff-files) 。

```
 -c 
```

使用此选项，合并提交的 diff 输出同时显示每个父项与合并结果的差异，而不是一次显示父项和结果之间的成对差异。此外，它仅列出从所有父母修改的文件。

```
 --cc 
```

这个标志意味着`-c`选项并通过省略不感兴趣的帅哥进一步压缩补丁输出，其中父母的内容只有两个变体，合并结果选择其中一个而不做修改。

```
 -m 
```

此标志使合并提交像常规提交一样显示完整差异;对于每个合并父项，将生成单独的日志条目和差异。一个例外是，当给出`--first-parent`选项时，只显示对第一个父项的差异;在这种情况下，输出表示合并带来的变化 _ 进入当前分支的 _。

```
 -r 
```

显示递归差异。

```
 -t 
```

在 diff 输出中显示树对象。这意味着`-r`。

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

    *   _％N_ ：提交备注

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

## 常见的 DIFF 选项

```
 -p 
```

```
 -u 
```

```
 --patch 
```

生成补丁（请参阅生成补丁的部分）。

```
 -s 
```

```
 --no-patch 
```

抑制差异输出。对于`git show`等默认显示补丁的命令，或取消`--patch`的效果很有用。

```
 -U<n> 
```

```
 --unified=<n> 
```

用&lt; n&gt;生成差异。上下文而不是通常的三行。意味着`-p`。

```
 --raw 
```

对于每个提交，使用原始 diff 格式显示更改摘要。请参阅 [git-diff [1]](https://git-scm.com/docs/git-diff) 的“RAW OUTPUT FORMAT”部分。这与以原始格式显示日志本身不同，您可以使用`--format=raw`来实现。

```
 --patch-with-raw 
```

`-p --raw`的同义词。

```
 --indent-heuristic 
```

启用改变差异块边界的启发式以使补丁更易于阅读。这是默认值。

```
 --no-indent-heuristic 
```

禁用缩进启发式。

```
 --minimal 
```

花些额外的时间来确保产生尽可能小的差异。

```
 --patience 
```

使用“耐心差异”算法生成差异。

```
 --histogram 
```

使用“histogram diff”算法生成 diff。

```
 --anchored=<text> 
```

使用“锚定差异”算法生成差异。

可以多次指定此选项。

如果源和目标中都存在一行，只存在一次，并以此文本开头，则此算法会尝试阻止它在输出中显示为删除或添加。它在内部使用“耐心差异”算法。

```
 --diff-algorithm={patience|minimal|histogram|myers} 
```

选择差异算法。变体如下：

```
 default, myers 
```

基本的贪心差异算法。目前，这是默认值。

```
 minimal 
```

花些额外的时间来确保产生尽可能小的差异。

```
 patience 
```

生成补丁时使用“耐心差异”算法。

```
 histogram 
```

该算法将耐心算法扩展为“支持低发生的共同元素”。

例如，如果将`diff.algorithm`变量配置为非默认值并想要使用默认值，则必须使用`--diff-algorithm=default`选项。

```
 --stat[=<width>[,<name-width>[,<count>]]] 
```

生成 diffstat。默认情况下，文件名部分将使用必要的空间，图形部分的其余部分将使用。最大宽度默认为终端宽度，如果未连接到终端，则为 80 列，并且可以被`&lt;width&gt;`覆盖。可以通过在逗号后面给出另一个宽度`&lt;name-width&gt;`来限制文件名部分的宽度。可以使用`--stat-graph-width=&lt;width&gt;`（影响生成统计图的所有命令）或设置`diff.statGraphWidth=&lt;width&gt;`（不影响`git format-patch`）来限制图形部分的宽度。通过给出第三个参数`&lt;count&gt;`，可以将输出限制为第一个`&lt;count&gt;`行，如果有更多，则可以将`...`限制为`...`。

也可以使用`--stat-width=&lt;width&gt;`，`--stat-name-width=&lt;name-width&gt;`和`--stat-count=&lt;count&gt;`单独设置这些参数。

```
 --compact-summary 
```

输出扩展标题信息的精简摘要，例如文件创建或删除（“新”或“消失”，如果是符号链接，则可选“+ l”）和模式更改（“+ x”或“-x”用于添加或删除 diffstat 中的可执行位）。信息放在文件名部分和图形部分之间。意味着`--stat`。

```
 --numstat 
```

与`--stat`类似，但显示十进制表示法中添加和删除的行数以及没有缩写的路径名，以使其更加机器友好。对于二进制文件，输出两个`-`而不是`0 0`。

```
 --shortstat 
```

仅输出`--stat`格式的最后一行，其中包含已修改文件的总数，以及已添加和已删除行的数量。

```
 --dirstat[=<param1,param2,…​>] 
```

输出每个子目录的相对更改量的分布。 `--dirstat`的行为可以通过以逗号分隔的参数列表传递来定制。默认值由`diff.dirstat`配置变量控制（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。可以使用以下参数：

```
 changes 
```

通过计算已从源中删除或添加到目标的行来计算 dirstat 数。这忽略了文件中纯代码移动的数量。换句话说，重新排列文件中的行不会像其他更改那样计算。这是没有给出参数时的默认行为。

```
 lines 
```

通过执行常规的基于行的差异分析来计算 dirstat 数字，并对移除/添加的行数进行求和。 （对于二进制文件，计算 64 字节块，因为二进制文件没有自然的线条概念）。这是比`changes`行为更昂贵的`--dirstat`行为，但它确实计算文件中重新排列的行与其他更改一样多。结果输出与您从其他`--*stat`选项获得的输出一致。

```
 files 
```

通过计算更改的文件数来计算 dirstat 数。在 dirstat 分析中，每个更改的文件都相同。这是计算上最便宜的`--dirstat`行为，因为它根本不需要查看文件内容。

```
 cumulative 
```

计算父目录的子目录中的更改。请注意，使用`cumulative`时，报告的百分比总和可能超过 100％。可以使用`noncumulative`参数指定默认（非累积）行为。

```
 <limit> 
```

整数参数指定截止百分比（默认为 3％）。贡献低于此百分比变化的目录不会显示在输出中。

示例：以下将计算已更改的文件，同时忽略少于已更改文件总量的 10％的目录，并在父目录中累计子目录计数：`--dirstat=files,10,cumulative`。

```
 --summary 
```

输出扩展标题信息的精简摘要，例如创建，重命名和模式更改。

```
 --patch-with-stat 
```

`-p --stat`的同义词。

```
 -z 
```

将提交与 NUL 分开，而不是使用新换行符。

此外，当给出`--raw`或`--numstat`时，不要使用路径名并使用 NUL 作为输出字段终止符。

如果没有此选项，则会引用具有“异常”字符的路径名，如配置变量`core.quotePath`所述（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

```
 --name-only 
```

仅显示已更改文件的名称。

```
 --name-status 
```

仅显示已更改文件的名称和状态。有关状态字母的含义，请参阅`--diff-filter`选项的说明。

```
 --submodule[=<format>] 
```

指定子模块的差异如何显示。指定`--submodule=short`时，使用 _ 短 _ 格式。此格式仅显示范围开头和结尾的提交名称。指定`--submodule`或`--submodule=log`时，使用 _log_ 格式。此格式列出 [git-submodule [1]](https://git-scm.com/docs/git-submodule) `summary`等范围内的提交。指定`--submodule=diff`时，使用 _diff_ 格式。此格式显示提交范围之间子模块内容更改的内联差异。如果未设置配置选项，则默认为`diff.submodule`或 _ 短 _ 格式。

```
 --color[=<when>] 
```

显示彩色差异。 `--color`（即没有 _=&lt;当&gt;_ ）与`--color=always`相同时。 _&lt; when&gt;_ 可以是`always`，`never`或`auto`之一。

```
 --no-color 
```

关掉彩色差异。它与`--color=never`相同。

```
 --color-moved[=<mode>] 
```

移动的代码行的颜色不同。 &lt;模式&gt;如果没有给出选项，默认为 _no_ ，如果给出没有模式的选项，则默认为 _zebra_ 。模式必须是以下之一：

```
 no 
```

移动的线条不会突出显示。

```
 default 
```

是`zebra`的同义词。这可能会在未来转变为更明智的模式。

```
 plain 
```

在一个位置添加并在另一个位置删除的任何行都将使用 _color.diff.newMoved_ 进行着色。类似地， _color.diff.oldMoved_ 将用于在 diff 中的其他位置添加的已删除行。此模式选择任何已移动的行，但在检查中确定是否在没有置换的情况下移动了代码块时，它不是很有用。

```
 blocks 
```

贪婪地检测至少 20 个字母数字字符的移动文本块。使用 _color.diff。{old，new} Moved_ 颜色绘制检测到的块。相邻的街区不能分开。

```
 zebra 
```

在 _ 块 _ 模式中检测移动文本块。使用 _color.diff。{old，new} Moved_ 颜色或 _color.diff。{old，new} MovedAlternative_ 绘制块。两种颜色之间的变化表示检测到新的块。

```
 dimmed-zebra 
```

与 _zebra_ 类似，但执行了移动代码的无趣部分的额外调暗。两个相邻街区的边界线被认为是有趣的，其余的是无趣的。 `dimmed_zebra`是不推荐使用的同义词。

```
 --no-color-moved 
```

关闭移动检测。这可用于覆盖配置设置。它与`--color-moved=no`相同。

```
 --color-moved-ws=<modes> 
```

这将配置在执行`--color-moved`的移动检测时如何忽略空白。这些模式可以以逗号分隔的列表给出：

```
 no 
```

执行移动检测时不要忽略空格。

```
 ignore-space-at-eol 
```

忽略 EOL 中的空白更改。

```
 ignore-space-change 
```

忽略空格量的变化。这会忽略行尾的空格，并将一个或多个空白字符的所有其他序列视为等效。

```
 ignore-all-space 
```

比较线条时忽略空格。即使一行有空格而另一行没有空格，这也会忽略差异。

```
 allow-indentation-change 
```

最初忽略移动检测中的任何空格，然后如果每行的空白变化相同，则仅将移动的代码块分组到块中。这与其他模式不兼容。

```
 --no-color-moved-ws 
```

执行移动检测时不要忽略空格。这可用于覆盖配置设置。它与`--color-moved-ws=no`相同。

```
 --word-diff[=<mode>] 
```

使用&lt; mode&gt;显示单词 diff。划定改变的单词。默认情况下，单词由空格分隔;见下面的`--word-diff-regex`。 &lt;模式&gt;默认为 _plain_ ，必须是以下之一：

```
 color 
```

仅使用颜色突出显示更改的单词。意味着`--color`。

```
 plain 
```

将单词显示为`[-removed-]`和`{+added+}`。如果它们出现在输入中，则不会尝试转义分隔符，因此输出可能不明确。

```
 porcelain 
```

使用特殊的基于行的格式用于脚本使用。添加/删除/未更改的运行以通常的统一 diff 格式打印，从行开头的`+` / `-` /``字符开始并延伸到行尾。输入中的换行符由其自身行上的波浪号`~`表示。

```
 none 
```

再次禁用字差异。

请注意，尽管第一个模式的名称，但如果启用了颜色，则使用颜色突出显示所有模式中已更改的部分。

```
 --word-diff-regex=<regex> 
```

使用&lt; regex&gt;决定一个单词是什么，而不是将非空格的运行视为一个单词。除非已经启用，否则还暗示`--word-diff`。

&lt; regex&gt;的每个非重叠匹配被认为是一个词。这些匹配之间的任何内容都被视为空格并被忽略（！）以查找差异。您可能希望将`|[^[:space:]]`附加到正则表达式，以确保它匹配所有非空白字符。包含换行符的匹配项会在换行符处以静默方式截断（！）。

例如，`--word-diff-regex=.`会将每个字符视为一个单词，并相应地逐个字符地显示差异。

正则表达式也可以通过 diff 驱动程序或配置选项设置，参见 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 或 [git-config [1]](https://git-scm.com/docs/git-config) 。明确地覆盖任何差异驱动程序或配置设置。 Diff 驱动程序覆盖配置设置。

```
 --color-words[=<regex>] 
```

相当于`--word-diff=color`加（如果指定了正则表达式）`--word-diff-regex=&lt;regex&gt;`。

```
 --no-renames 
```

关闭重命名检测，即使配置文件提供默认值也是如此。

```
 --check 
```

如果更改引入冲突标记或空白错误，则发出警告。什么被认为是空白错误由`core.whitespace`配置控制。默认情况下，尾随空格（包括仅由空格组成的行）和在行的初始缩进内紧跟着制表符的空格字符被视为空格错误。如果发现问题，则退出非零状态。与--exit-code 不兼容。

```
 --ws-error-highlight=<kind> 
```

突出显示差异的`context`，`old`或`new`行中的空白错误。多个值用逗号分隔，`none`重置先前的值，`default`将列表重置为`new`，`all`是`old,new,context`的简写。如果未指定此选项，并且未设置配置变量`diff.wsErrorHighlight`，则仅突出显示`new`行中的空白错误。空白错误用`color.diff.whitespace`着色。

```
 --full-index 
```

在生成补丁格式输出时，在“索引”行上显示完整的前映像和后映像 blob 对象名称，而不是第一个字符。

```
 --binary 
```

除`--full-index`外，还可输出可用`git-apply`应用的二进制差异。

```
 --abbrev[=<n>] 
```

而不是在 diff-raw 格式输出和 diff-tree 标题行中显示完整的 40 字节十六进制对象名称，而是仅显示部分前缀。这与上面的`--full-index`选项无关，后者控制 diff-patch 输出格式。可以使用`--abbrev=&lt;n&gt;`指定非默认位数。

```
 -B[<n>][/<m>] 
```

```
 --break-rewrites[=[<n>][/<m>]] 
```

将完整的重写更改分为删除和创建对。这有两个目的：

它影响了一个更改的方式，相当于一个文件的完全重写，而不是一系列的删除和插入混合在一起，只有几行恰好与文本作为上下文匹配，而是作为单个删除所有旧的后跟一个单个插入所有新内容，数字`m`控制-B 选项的这一方面（默认为 60％）。 `-B/70%`指定少于 30％的原始文本应保留在结果中，以便 Git 将其视为完全重写（即，否则生成的修补程序将是一系列删除和插入与上下文行混合在一起）。

当与-M 一起使用时，完全重写的文件也被视为重命名的源（通常-M 只考虑作为重命名源消失的文件），并且数字`n`控制 - 的这方面 - B 选项（默认为 50％）。 `-B20%`指定添加和删除的更改与文件大小的 20％或更多相比，有资格被选为可能的重命名源到另一个文件。

```
 -M[<n>] 
```

```
 --find-renames[=<n>] 
```

如果生成差异，则检测并报告每次提交的重命名。对于遍历历史记录时重命名后续文件，请参阅`--follow`。如果指定了`n`，则它是相似性指数的阈值（即与文件大小相比的添加/删除量）。例如，`-M90%`表示如果超过 90％的文件未更改，Git 应将删除/添加对视为重命名。如果没有`%`符号，则该数字将作为分数读取，并在其前面加上小数点。即，`-M5`变为 0.5，因此与`-M50%`相同。同样，`-M05`与`-M5%`相同。要将检测限制为精确重命名，请使用`-M100%`。默认相似性指数为 50％。

```
 -C[<n>] 
```

```
 --find-copies[=<n>] 
```

检测副本以及重命名。另见`--find-copies-harder`。如果指定了`n`，则其含义与`-M&lt;n&gt;`的含义相同。

```
 --find-copies-harder 
```

出于性能原因，默认情况下，仅当在同一变更集中修改了副本的原始文件时，`-C`选项才会查找副本。此标志使命令检查未修改的文件作为副本源的候选者。对于大型项目来说，这是一项非常昂贵的操作，因此请谨慎使用。提供多个`-C`选项具有相同的效果。

```
 -D 
```

```
 --irreversible-delete 
```

省略删除的原像，即只打印标题而不打印原像和`/dev/null`之间的差异。得到的贴片不适用于`patch`或`git apply`;这仅适用于那些希望在更改后专注于审阅文本的人。此外，输出显然缺乏足够的信息来反向应用这样的补丁，甚至手动，因此选项的名称。

与`-B`一起使用时，也省略删除/创建对的删除部分中的原像。

```
 -l<num> 
```

`-M`和`-C`选项需要 O（n ^ 2）处理时间，其中 n 是潜在的重命名/复制目标的数量。如果重命名/复制目标的数量超过指定的数量，此选项可防止重命名/复制检测运行。

```
 --diff-filter=[(A|C|D|M|R|T|U|X|B)…​[*]] 
```

仅选择已添加（`A`），复制（`C`），已删除（`D`），已修改（`M`），已重命名（`R`）的文件，其类型（即常规文件，符号链接，子模块，...）更改（`T`），未合并（`U`），未知（`X`），或已配对破碎（`B`）。可以使用过滤器字符的任何组合（包括无）。当`*`（全部或全部）添加到组合中时，如果有任何文件与比较中的其他条件匹配，则选择所有路径;如果没有与其他条件匹配的文件，则不会选择任何内容。

此外，这些大写字母可以降级为排除。例如。 `--diff-filter=ad`排除添加和删除的路径。

请注意，并非所有差异都可以包含所有类型。例如，从索引到工作树的差异永远不会有添加条目（因为差异中包含的路径集受限于索引中的内容）。同样，如果禁用了对这些类型的检测，则无法显示复制和重命名的条目。

```
 -S<string> 
```

查找改变文件中指定字符串出现次数（即添加/删除）的差异。用于脚本编写者的使用。

当你正在寻找一个确切的代码块（比如一个结构体）时，它很有用，并且想要知道该块首次出现以来的历史：迭代地使用该特征将原始图像中的有趣块反馈回`-S`，继续前进，直到你获得该块的第一个版本。

也搜索二进制文件。

```
 -G<regex> 
```

查找补丁文本包含与&lt; regex&gt;匹配的添加/删除行的差异。

为了说明`-S&lt;regex&gt; --pickaxe-regex`和`-G&lt;regex&gt;`之间的区别，请考虑在同一文件中使用以下 diff 进行提交：

```
+    return !regexec(regexp, two->ptr, 1, &regmatch, 0);
...
-    hit = !regexec(regexp, mf2.ptr, 1, &regmatch, 0);
```

虽然`git log -G"regexec\(regexp"`将显示此提交，但`git log -S"regexec\(regexp" --pickaxe-regex`不会（因为该字符串的出现次数没有改变）。

除非提供`--text`，否则将忽略没有 textconv 过滤器的二进制文件的补丁。

有关详细信息，请参阅 [gitdiffcore [7]](https://git-scm.com/docs/gitdiffcore) 中的 _pickaxe_ 条目。

```
 --find-object=<object-id> 
```

查找更改指定对象出现次数的差异。与`-S`类似，只是参数的不同之处在于它不搜索特定的字符串，而是搜索特定的对象 id。

该对象可以是 blob 或子模块提交。它意味着`git-log`中的`-t`选项也可以找到树。

```
 --pickaxe-all 
```

当`-S`或`-G`找到更改时，显示该更改集中的所有更改，而不仅仅是包含&lt; string&gt;中更改的文件。

```
 --pickaxe-regex 
```

对待&lt; string&gt;赋予`-S`作为扩展的 POSIX 正则表达式以匹配。

```
 -O<orderfile> 
```

控制文件在输出中的显示顺序。这会覆盖`diff.orderFile`配置变量（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。要取消`diff.orderFile`，请使用`-O/dev/null`。

输出顺序由&lt; orderfile&gt;中的 glob 模式的顺序决定。首先输出所有与第一个模式匹配的路径名的文件，然后输出所有与第二个模式（但不是第一个模式）匹配的路径名的文件，依此类推。路径名与任何模式都不匹配的所有文件都是最后输出的，就好像文件末尾有一个隐式匹配所有模式一样。如果多个路径名具有相同的等级（它们匹配相同的模式但没有早期模式），则它们相对于彼此的输出顺序是正常顺序。

&lt; orderfile&gt;解析如下：

*   空行被忽略，因此可以将它们用作分隔符以提高可读性。

*   以哈希（“`#`”）开头的行将被忽略，因此它们可用于注释。如果以散列开头，则将反斜杠（“`\`”）添加到模式的开头。

*   每个其他行包含一个模式。

模式与没有 FNM_PATHNAME 标志的 fnmatch（3）使用的模式具有相同的语法和语义，但如果删除任意数量的最终路径名组件与模式匹配，则路径名也匹配模式。例如，模式“`foo*bar`”匹配“`fooasdfbar`”和“`foo/bar/baz/asdf`”而不匹配“`foobarx`”。

```
 -R 
```

交换两个输入;也就是说，显示从索引或磁盘文件到树内容的差异。

```
 --relative[=<path>] 
```

从项目的子目录运行时，可以告诉它排除目录外的更改并使用此选项显示相对于它的路径名。当您不在子目录中时（例如，在裸存储库中），您可以通过给出&lt; path&gt;来命名哪个子目录以使输出相对。作为一个论点。

```
 -a 
```

```
 --text 
```

将所有文件视为文本。

```
 --ignore-cr-at-eol 
```

进行比较时，忽略行尾的回车。

```
 --ignore-space-at-eol 
```

忽略 EOL 中的空白更改。

```
 -b 
```

```
 --ignore-space-change 
```

忽略空格量的变化。这会忽略行尾的空格，并将一个或多个空白字符的所有其他序列视为等效。

```
 -w 
```

```
 --ignore-all-space 
```

比较线条时忽略空格。即使一行有空格而另一行没有空格，这也会忽略差异。

```
 --ignore-blank-lines 
```

忽略其行全部为空的更改。

```
 --inter-hunk-context=<lines> 
```

显示差异之间的上下文，直到指定的行数，从而融合彼此接近的帅哥。如果未设置配置选项，则默认为`diff.interHunkContext`或 0。

```
 -W 
```

```
 --function-context 
```

显示整个周围的变化功能。

```
 --ext-diff 
```

允许执行外部 diff 助手。如果使用 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 设置外部差异驱动程序，则需要将此选项与 [git-log [1]](https://git-scm.com/docs/git-log) 和朋友一起使用。

```
 --no-ext-diff 
```

禁止外部差异驱动程序。

```
 --textconv 
```

```
 --no-textconv 
```

在比较二进制文件时允许（或禁止）外部文本转换过滤器运行。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。由于 textconv 过滤器通常是单向转换，因此生成的差异适合人类使用，但无法应用。因此，默认情况下，textconv 过滤器仅针对 [git-diff [1]](https://git-scm.com/docs/git-diff) 和 [git-log [1]](https://git-scm.com/docs/git-log) 启用，但不适用于 [git-format-patch [ 1]](https://git-scm.com/docs/git-format-patch) 或差异管道命令。

```
 --ignore-submodules[=<when>] 
```

忽略差异生成中子模块的更改。 &lt;当&gt;可以是“none”，“untracked”，“dirty”或“all”，这是默认值。使用“none”时，如果子模块包含未跟踪或修改的文件，或者其 HEAD 与超级项目中记录的提交不同，则可以使用“无”来修改子模块，并可用于覆盖[中 _ignore_ 选项的任何设置 git-config [1]](https://git-scm.com/docs/git-config) 或 [gitmodules [5]](https://git-scm.com/docs/gitmodules) 。当使用“未跟踪”时，如果子模块仅包含未跟踪的内容（但仍会扫描修改的内容），则子模块不会被视为脏。使用“脏”忽略对子模块工作树的所有更改，仅显示存储在超级项目中的提交的更改（这是 1.7.0 之前的行为）。使用“all”隐藏子模块的所有更改。

```
 --src-prefix=<prefix> 
```

显示给定的源前缀而不是“a /”。

```
 --dst-prefix=<prefix> 
```

显示给定的目标前缀而不是“b /”。

```
 --no-prefix 
```

不显示任何源或目标前缀。

```
 --line-prefix=<prefix> 
```

为每行输出预先附加前缀。

```
 --ita-invisible-in-index 
```

默认情况下，“git add -N”添加的条目在“git diff”中显示为现有空文件，在“git diff --cached”中显示为新文件。此选项使条目在“git diff”中显示为新文件，在“git diff --cached”中不存在。可以使用`--ita-visible-in-index`恢复此选项。这两个选项都是实验性的，将来可以删除。

有关这些常用选项的更详细说明，另请参阅 [gitdiffcore [7]](https://git-scm.com/docs/gitdiffcore) 。

## 使用-p 生成补丁

当“git-diff-index”，“git-diff-tree”或“git-diff-files”使用`-p`选项运行时，“git diff”不带`--raw`选项或“git log”使用“-p”选项，它们不会产生上述输出;相反，他们生成一个补丁文件。您可以通过`GIT_EXTERNAL_DIFF`和`GIT_DIFF_OPTS`环境变量自定义此类修补程序的创建。

-p 选项产生的内容与传统的 diff 格式略有不同：

1.  它前面有一个“git diff”标题，如下所示：

    ```
    diff --git a/file1 b/file2
    ```

    除非涉及重命名/复制，否则`a/`和`b/`文件名是相同的。特别是，即使是创建或删除，`/dev/null`也是 _ 而不是 _ 来代替`a/`或`b/`文件名。

    当涉及重命名/复制时，`file1`和`file2`分别显示重命名/复制的源文件的名称和重命名/复制的文件的名称。

2.  它后跟一个或多个扩展标题行：

    ```
    old mode &lt;mode&gt;
    new mode &lt;mode&gt;
    deleted file mode &lt;mode&gt;
    new file mode &lt;mode&gt;
    copy from &lt;path&gt;
    copy to &lt;path&gt;
    rename from &lt;path&gt;
    rename to &lt;path&gt;
    similarity index &lt;number&gt;
    dissimilarity index &lt;number&gt;
    index &lt;hash&gt;..&lt;hash&gt; &lt;mode&gt;
    ```

    文件模式打印为 6 位八进制数，包括文件类型和文件权限位。

    扩展标头中的路径名不包括`a/`和`b/`前缀。

    相似性指数是未更改行的百分比，相异性指数是更改行的百分比。它是一个向下舍入的整数，后跟一个百分号。因此，100％的相似性索引值保留用于两个相等的文件，而 100％的相异性意味着旧文件中的任何行都不会成为新文件。

    索引行包括更改前后的 SHA-1 校验和。 &lt;模式&gt;如果文件模式没有改变，则包括在内;否则，单独的行表示旧模式和新模式。

3.  具有“异常”字符的路径名被引用，如配置变量`core.quotePath`所述（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

4.  输出中的所有`file1`文件在提交之前引用文件，并且所有`file2`文件在提交之后引用文件。将每个更改顺序应用于每个文件是不正确的。例如，此补丁将交换 a 和 b：

    ```
    diff --git a/a b/b
    rename from a
    rename to b
    diff --git a/b b/a
    rename from b
    rename to a
    ```

## 组合差异格式

在显示合并时，任何差异生成命令都可以使用`-c`或`--cc`选项生成 _ 组合差异 _。当显示与 [git-diff [1]](https://git-scm.com/docs/git-diff) 或 [git-show [1]](https://git-scm.com/docs/git-show) 的合并时，这是默认格式。另请注意，您可以为这些命令中的任何一个提供`-m`选项，以强制使用合并的各个父项生成差异。

_ 组合 diff_ 格式如下所示：

```
diff --combined describe.c
index fabadb8,cc95eb0..4866510
--- a/describe.c
+++ b/describe.c
@@@ -98,20 -98,12 +98,20 @@@
	return (a_date > b_date) ? -1 : (a_date == b_date) ? 0 : 1;
  }

- static void describe(char *arg)
 -static void describe(struct commit *cmit, int last_one)
++static void describe(char *arg, int last_one)
  {
 +	unsigned char sha1[20];
 +	struct commit *cmit;
	struct commit_list *list;
	static int initialized = 0;
	struct commit_name *n;

 +	if (get_sha1(arg, sha1) < 0)
 +		usage(describe_usage);
 +	cmit = lookup_commit_reference(sha1);
 +	if (!cmit)
 +		usage(describe_usage);
 +
	if (!initialized) {
		initialized = 1;
		for_each_ref(get_name);
```

1.  它前面有一个“git diff”标题，看起来像这样（当使用`-c`选项时）：

    ```
    diff --combined file
    ```

    或者像这样（当使用`--cc`选项时）：

    ```
    diff --cc file
    ```

2.  它后跟一个或多个扩展标题行（此示例显示了与两个父项的合并）：

    ```
    index &lt;hash&gt;,&lt;hash&gt;..&lt;hash&gt;
    mode &lt;mode&gt;,&lt;mode&gt;..&lt;mode&gt;
    new file mode &lt;mode&gt;
    deleted file mode &lt;mode&gt;,&lt;mode&gt;
    ```

    只有当&lt; mode&gt;中的至少一个出现时，`mode &lt;mode&gt;,&lt;mode&gt;..&lt;mode&gt;`行才会出现。与其他人不同。具有关于检测到的内容移动（重命名和复制检测）的信息的扩展标题被设计为与两个&lt; tree-ish&gt;的差异一起工作。并且不会被组合 diff 格式使用。

3.  接下来是两行的文件/文件头

    ```
    --- a/file
    +++ b/file
    ```

    与传统 _ 统一 _ diff 格式的双行标题类似，`/dev/null`用于表示创建或删除的文件。

4.  修改了块头格式以防止人们意外地将其馈送到`patch -p1`。创建组合差异格式用于审查合并提交更改，并不适用于应用。此更改类似于扩展 _ 索引 _ 标头中的更改：

    ```
    @@@ &lt;from-file-range&gt; &lt;from-file-range&gt; &lt;to-file-range&gt; @@@
    ```

    组合 diff 格式的块头中有（父项数+ 1）`@`个字符。

与传统的 _ 统一 _ 差异格式不同，后者显示两个文件 A 和 B，其中一列具有`-`（减去 - 出现在 A 中但在 B 中删除），`+`（加 - 缺少 A 但是添加到 B）或`" "`（空格 - 未更改）前缀，此格式将两个或多个文件 file1，file2，...与一个文件 X 进行比较，并显示 X 与每个文件 N 的不同之处。每个 fileN 的一列被添加到输出行之前，以指示 X 的行与它的不同之处。

N 列中的`-`字符表示该行出现在 fileN 中，但它不会出现在结果中。列 N 中的`+`字符表示该行出现在结果中，而 fileN 没有该行（换句话说，从该父项的角度添加了该行）。

在上面的示例输出中，函数签名已从两个文件中更改（因此，file1 和 file2 中的两个`-`删除加上`++`表示添加的一行未出现在 file1 或 file2 中）。另外八行与 file1 相同，但不出现在 file2 中（因此以`+`为前缀）。

当由`git diff-tree -c`显示时，它将合并提交的父项与合并结果进行比较（即 file1..fileN 是父项）。当由`git diff-files -c`显示时，它将两个未解析的合并父项与工作树文件进行比较（即 file1 是阶段 2 又名“我们的版本”，file2 是阶段 3 又名“他们的版本”）。

## 例子

```
 git log --no-merges 
```

显示整个提交历史记录，但跳过任何合并

```
 git log v2.6.12.. include/scsi drivers/scsi 
```

显示自版本 _v2.6.12_ 以来更改`include/scsi`或`drivers/scsi`子目录中的任何文件的所有提交

```
 git log --since="2 weeks ago" -- gitk 
```

在过去两周内将更改显示到文件 _gitk_ 。 `--`是必要的，以避免与名为 _gitk_ 的**分支**混淆

```
 git log --name-status release..test 
```

显示“test”分支中但尚未在“release”分支中的提交，以及每个提交修改的路径列表。

```
 git log --follow builtin/rev-list.c 
```

显示更改`builtin/rev-list.c`的提交，包括在文件被赋予其当前名称之前发生的提交。

```
 git log --branches --not --remotes=origin 
```

显示任何本地分支中的所有提交，但不显示 _ 原点 _ 的任何远程跟踪分支中的所有提交（您的原点没有）。

```
 git log master --not --remotes=*/master 
```

显示本地主服务器中但不在任何远程存储库主分支中的所有提交。

```
 git log -p -m --first-parent 
```

显示包含更改差异的历史记录，但仅显示“主分支”透视图，跳过来自合并分支的提交，并显示合并引入的完整更改差异。只有遵循严格的策略，在停留在单个集成分支上时合并所有主题分支才有意义。

```
 git log -L '/int main/',/^}/:main.c 
```

显示文件`main.c`中的函数`main()`如何随时间演变。

```
 git log -3 
```

将要显示的提交数限制为 3。

## 讨论

Git 在某种程度上是字符编码不可知的。

*   blob 对象的内容是未解释的字节序列。核心级别没有编码转换。

*   路径名以 UTF-8 规范化形式 C 编码。这适用于树对象，索引文件，ref 名称，以及命令行参数，环境变量和配置文件中的路径名（`.git/config`（参见 [git） -config [1]](https://git-scm.com/docs/git-config) ）， [gitignore [5]](https://git-scm.com/docs/gitignore) ， [gitattributes [5]](https://git-scm.com/docs/gitattributes) 和 [gitmodules [5]](https://git-scm.com/docs/gitmodules) ）。

    请注意，核心级别的 Git 仅将路径名称视为非 NUL 字节序列，没有路径名称编码转换（Mac 和 Windows 除外）。因此，即使在使用传统扩展 ASCII 编码的平台和文件系统上，使用非 ASCII 路径名也会起作用。但是，在此类系统上创建的存储库将无法在基于 UTF-8 的系统（例如 Linux，Mac，Windows）上正常工作，反之亦然。此外，许多基于 Git 的工具只是假设路径名为 UTF-8，并且无法正确显示其他编码。

*   提交日志消息通常以 UTF-8 编码，但也支持其他扩展 ASCII 编码。这包括 ISO-8859-x，CP125x 和许多其他，但 _ 不是 _ UTF-16/32，EBCDIC 和 CJK 多字节编码（GBK，Shift-JIS，Big5，EUC-x，CP9xx 等。 ）。

虽然我们鼓励提交日志消息以 UTF-8 编码，但核心和 Git 瓷器都不是为了强制项目使用 UTF-8。如果特定项目的所有参与者发现使用遗留编码更方便，Git 不会禁止它。但是，有一些事情需要牢记。

1.  _git commit_ 和 _git commit-tree_ 发出警告，如果提供给它的提交日志消息看起来不像有效的 UTF-8 字符串，除非你明确说你的项目使用了遗产编码。说这个的方法是在`.git/config`文件中使用 i18n.commitencoding，如下所示：

    ```
    [i18n]
    	commitEncoding = ISO-8859-1
    ```

    使用上述设置创建的提交对象在其`encoding`标题中记录`i18n.commitEncoding`的值。这是为了帮助其他人以后再看。缺少此标头意味着提交日志消息以 UTF-8 编码。

2.  _git log_ ， _git show_ ， _git blame_ 和朋友们查看提交对象的`encoding`头，并尝试将日志消息重新编码为除非另有说明，否则为 UTF-8。您可以使用`.git/config`文件中的`i18n.logOutputEncoding`指定所需的输出编码，如下所示：

    ```
    [i18n]
    	logOutputEncoding = ISO-8859-1
    ```

    如果您没有此配置变量，则使用`i18n.commitEncoding`的值。

请注意，我们故意选择在提交以在提交对象级别强制使用 UTF-8 时不重新编写提交日志消息，因为重新编码为 UTF-8 不一定是可逆操作。

## 组态

对于核心变量，请参见 [git-config [1]](https://git-scm.com/docs/git-config) ，对于差异生成，请参见 [git-diff [1]](https://git-scm.com/docs/git-diff) 。

```
 format.pretty 
```

`--format`选项的默认值。 （参见上面的 _ 漂亮格式 _。）默认为`medium`。

```
 i18n.logOutputEncoding 
```

显示日志时使用的编码。 （参见上面的 _ 讨论 _。）如果设置，则默认为`i18n.commitEncoding`的值，否则为 UTF-8。

```
 log.date 
```

人类可读日期的默认格式。 （比较`--date`选项。）默认为“默认”，表示写入`Sat May 8 19:35:34 2010 -0500`等日期。

如果格式设置为“auto：foo”并且正在使用寻呼机，则格式“foo”将用于日期格式。否则将使用“默认”。

```
 log.follow 
```

如果`true`，`git log`将像单个&lt;路径&gt;一样使用`--follow`选项。给出。这与`--follow`具有相同的限制，即它不能用于跟踪多个文件，并且在非线性历史记录上不能很好地工作。

```
 log.showRoot 
```

如果`false`，`git log`和相关命令不会将初始提交视为大创建事件。 `git log -p`输出中的任何根提交都将显示没有附加差异。默认值为`true`。

```
 log.showSignature 
```

如果`true`，`git log`和相关命令的作用就像传递了`--show-signature`选项一样。

```
 mailmap.* 
```

见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 。

```
 notes.displayRef 
```

除了`core.notesRef`或`GIT_NOTES_REF`设置的默认值之外，还使用`log`系列命令显示提交消息时的注释。见 [git-notes [1]](https://git-scm.com/docs/git-notes) 。

可以是未缩写的引用名称或 glob，可以多次指定。将为不存在的引用发出警告，但是会自动忽略与任何引用不匹配的 glob。

可以通过`--no-notes`选项禁用此设置，由`GIT_NOTES_DISPLAY_REF`环境变量覆盖，并由`--notes=&lt;ref&gt;`选项覆盖。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件