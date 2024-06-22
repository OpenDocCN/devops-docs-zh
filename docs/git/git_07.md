# git-status

> 原文： [`git-scm.com/docs/git-status`](https://git-scm.com/docs/git-status)
> 
> 贡献者：[honglyua](https://github.com/honglyua)

## 名称

git-status - 显示工作树状态

## 概要

```
git status [<options>…​] [--] [<pathspec>…​]
```

## 描述

显示索引文件与当前 HEAD 提交之间存在差异的路径，工作树与索引文件之间存在差异的路径，以及工作树中未由 Git 跟踪的路径（和不会被[gitignore[5]](https://git-scm.com/docs/gitignore)忽略的路径 ）。第一个显示的是你 _ 将 _ 通过运行`git commit`提交的内容;第二个和第三个显示的是你在运行`git commit`之前运行 _git add_ 来提交的内容。

## 选项

```
 -s 
```

```
 --short 
```

以短格式输出。

```
 -b 
```

```
 --branch 
```

显示分支和跟踪信息，即使是在短格式下。

```
 --show-stash 
```

显示当前隐藏的条目数。

```
 --porcelain[=<version>] 
```

以易于解析的格式为脚本提供输出。这类似于短输出，但在 Git 版本中保持稳定，无论用户配置如何。请参阅下文了解详情。

version 参数用于指定格式版本。这是可选的，默认为原始版本 _v1_ 格式。

```
 --long 
```

以长格式输出。这是默认值。

```
 -v 
```

```
 --verbose 
```

除了已更改的文件的名称之外，还显示要提交的文本更改（即，类似于`git diff --cached`的输出）。如果指定`-v`两次，则还显示工作树中尚未暂存的更改（即，类似于`git diff`的输出）。

```
 -u[<mode>] 
```

```
 --untracked-files[=<mode>] 
```

显示未跟踪的文件。

mode 参数用于指定未跟踪文件的处理。它是可选的：它默认为 _all_，如果指定，它必须紧跟在选项上（例如`-uno`，但不是`-u no`）。

可能的选择是：

*   _no_ - 显示没有未跟踪的文件。

*   _normal_ - 显示未跟踪的文件和目录。

*   _all_ - 还显示未跟踪目录中的单个文件。

如果未使用`-u`选项，则会显示未跟踪的文件和目录（即与指定`normal`相同），以帮助您避免忘记添加新创建的文件。由于在文件系统中查找未跟踪文件需要额外的工作，因此在大型工作树中此模式可能需要一些时间。如果支持，请考虑启用未跟踪的缓存和拆分索引（请参阅`git update-index --untracked-cache`和`git update-index --split-index`），否则您可以使用`no`让`git status`更快地返回，而不显示未跟踪的文件。

可以使用 [git-config [1]](https://git-scm.com/docs/git-config) 中记录的 status.showUntrackedFiles 配置变量更改默认值。

```
 --ignore-submodules[=<when>] 
```

在查找更改时忽略对子模块的更改。<when>可以是“none”，“untracked”，“dirty”或“all”，这些都是默认值。使用“none”时，如果子模块包含未跟踪或修改的文件，或者其 HEAD 与超级项目中记录的提交不同，则可以使用“none”来修改子模块，并可用于覆盖[git-config [1]](https://git-scm.com/docs/git-config) 或 [gitmodules [5]](https://git-scm.com/docs/gitmodules)中都 _ignore_ 选项的任何设置。当使用“untracked”时，如果子模块仅包含未跟踪的内容（但仍会扫描修改的内容），则子模块不会被视为 dirty。使用“dirty”忽略对子模块工作树的所有更改，仅显示存储在超级项目中的提交的更改（这是 1.7.0 之前的行为）。使用“all”隐藏对子模块的所有更改（并在设置配置选项`status.submoduleSummary`时禁止子模块摘要的输出）。

```
 --ignored[=<mode>] 
```

同时显示被忽略的文件。

mode 参数用于指定忽略文件的处理。它是可选的：它默认为 _traditional_。

可能的选择是：

*   _traditional_ - 显示被忽略的文件和目录，除非指定了--untracked-files = all，在这种情况下，将显示被忽略目录中的单个文件。

*   _no_ - 显示没有被忽略的文件。

*   _matching_ - 显示与忽略模式匹配的被忽略的文件和目录。

当指定 _matching_ 模式时，将显示与忽略模式明确匹配的路径。如果目录与忽略模式匹配，则会显示该目录，但不会显示忽略目录中包含的路径。如果目录与忽略模式不匹配，但忽略了所有内容，则不显示该目录，但会显示所有内容。

```
 -z 
```

用 NUL 而不是 LF 终止条目。如果没有给出其他格式，这意味着`--porcelain=v1`输出格式。

```
 --column[=<options>] 
```

```
 --no-column 
```

在列中显示未跟踪的文件。有关选项语法，请参阅配置变量 column.status。没有选项的`--column`和`--no-column`分别相当于 _always_ 和 _never_。

```
 --ahead-behind 
```

```
 --no-ahead-behind 
```

显示或不显示分支相对于其上游分支领先或落后多少节点数。默认为 true。

```
 --renames 
```

```
 --no-renames 
```

无论用户配置如何，都可以打开/关闭重命名检测。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--no-renames`。

```
 --find-renames[=<n>] 
```

打开重命名检测，可选择设置相似性阈值。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--find-renames`。

```
 <pathspec>…​ 
```

参见 [gitglossary [7]](https://git-scm.com/docs/gitglossary) 中的 _pathspec_ 条目。

## 输出

此命令的输出旨在用作提交模板注释。默认的长格式设计很人性化，易读，详细和可描述性。其内容和输出格式随时可更改。

与许多其他 Git 命令不同，输出中提到的路径是相对于当前目录的，如果您在子目录中工作（这是故意的，以帮助剪切和粘贴）。请参阅下面的 status.relativePaths 配置选项。

### 短格式

在短格式中，每个路径的状态显示为这些形式之一

```
XY PATH
XY ORIG_PATH -> PATH
```

其中`ORIG_PATH`是重命名/复制内容的来源。只有在重命名或复制条目时才会显示`ORIG_PATH`。 `XY`是一个双字母状态代码。

字段（包括`->`）通过单个空格彼此分开。如果文件名包含空格或其他不可打印的字符，则该字段将以 C 字符串文字的方式引用：由 ASCII 双引号（34）字符包围，并使用内部特殊字符反斜杠转义。

对于具有合并冲突的路径，`X`和`Y`显示合并每一侧的修改状态。对于没有合并冲突的路径，`X`显示索引的状态，`Y`显示工作树的状态。对于未跟踪路径，`XY`为`??`。其他状态代码可以解释如下：

*   '' =未经修改

*   _M_ =修改

*   _A_ =已添加

*   _D_ =已删除

*   _R_ =重命名

*   _C_ =复制

*   _U_ =已更新但未合并

除非`--ignored`选项生效，否则不会列出忽略的文件，在这种情况下，`XY`为`!!`。

```
X          Y     Meaning
-------------------------------------------------
	 [AMD]   not updated
M        [ MD]   updated in index
A        [ MD]   added to index
D                deleted from index
R        [ MD]   renamed in index
C        [ MD]   copied in index
[MARC]           index and work tree matches
[ MARC]     M    work tree changed since index
[ MARC]     D    deleted in work tree
[ D]        R    renamed in work tree
[ D]        C    copied in work tree
-------------------------------------------------
D           D    unmerged, both deleted
A           U    unmerged, added by us
U           D    unmerged, deleted by them
U           A    unmerged, added by them
D           U    unmerged, deleted by us
A           A    unmerged, both added
U           U    unmerged, both modified
-------------------------------------------------
?           ?    untracked
!           !    ignored
-------------------------------------------------
```

子模块具有更多状态，但是不报告 M 子模块具有不同的 HEAD，而是在子模块已修改内容的索引中记录？子模块具有未跟踪的文件，因为子模块中的修改内容或未跟踪文件无法通过超级项目中的`git add`添加以准备提交。

_m_ 和 _？_ 递归应用。例如，如果子模块中的嵌套子模块包含未跟踪的文件，则报告为 _？_ 也是如此。

如果使用-b，则短格式状态前面有一行

```
## branchname tracking info
```

### Porcelain 格式版本 1

版本 1 中 porcelain 格式类似于短格式，但保证不会在 Git 版本之间以向后兼容的方式或基于用户配置进行更改。这使其成为脚本解析的理想选择。上面简短格式的描述也描述了 porcelain 格式，但有一些例外：

1.  用户的 color.status 配置不受认可;颜色永远都会关闭。

2.  用户的 status.relativePaths 配置不受认可;显示的路径始终相对于存储库根目录。

还有一种备用-z 格式建议用于机器解析。在该格式中，状态字段是相同的，但其他一些事情会发生变化。首先，-> 从重命名条目中省略, 并且字段顺序被反转（例如，_from ->_ 到变为 _from_）。其次，NUL（ASCII 0）跟在每个文件名后面，将空格替换为字段分隔符和终止换行符（但空格仍然将状态字段与第一个文件名分开）。第三，包含特殊字符的文件名不是特殊格式的;不执行引用或反斜杠转义。

任何子模块更改都会报告为已修改`M`而不是`m`或单个`?`。

### Porcelain 格式版本 2

版本 2 格式添加了有关工作树状态和更改项目的更多详细信息。版本 2 还定义了一组易于解析的可扩展可选标头。

标题行以“＃”开头，​​并添加以响应特定的命令行参数。解析器应该忽略它们无法识别的标头。

### 分支标题

如果给出`--branch`，则打印一系列标题行，其中包含有关当前分支的信息。

```
Line                                     Notes
------------------------------------------------------------
# branch.oid <commit> | (initial)        Current commit.
# branch.head <branch> | (detached)      Current branch.
# branch.upstream <upstream_branch>      If upstream is set.
# branch.ab +<ahead> -<behind>           If upstream is set and
					 the commit is present.
------------------------------------------------------------
```

### 跟踪条目修改

在标题之后，为跟踪的条目打印一系列行。可以根据变化的类型在三种不同线格式中的选择一种来描述条目。跟踪的条目以未定义的顺序打印;解析器应允许以任何顺序混合使用 3 种线型。

普通更改的条目具有以下格式：

```
1 <XY> <sub> <mH> <mI> <mW> <hH> <hI> <path>
```

重命名或复制的条目具有以下格式：

```
2 <XY> <sub> <mH> <mI> <mW> <hH> <hI> <X><score> <path><sep><origPath>
```

```
Field       Meaning
--------------------------------------------------------
<XY>        A 2 character field containing the staged and
	    unstaged XY values described in the short format,
	    with unchanged indicated by a "." rather than
	    a space.
<sub>       A 4 character field describing the submodule state.
	    "N..." when the entry is not a submodule.
	    "S<c><m><u>" when the entry is a submodule.
	    <c> is "C" if the commit changed; otherwise ".".
	    <m> is "M" if it has tracked changes; otherwise ".".
	    <u> is "U" if there are untracked changes; otherwise ".".
<mH>        The octal file mode in HEAD.
<mI>        The octal file mode in the index.
<mW>        The octal file mode in the worktree.
<hH>        The object name in HEAD.
<hI>        The object name in the index.
<X><score>  The rename or copy score (denoting the percentage
	    of similarity between the source and target of the
	    move or copy). For example "R100" or "C75".
<path>      The pathname.  In a renamed/copied entry, this
	    is the target path.
<sep>       When the `-z` option is used, the 2 pathnames are separated
	    with a NUL (ASCII 0x00) byte; otherwise, a tab (ASCII 0x09)
	    byte separates them.
<origPath>  The pathname in the commit at HEAD or in the index.
	    This is only present in a renamed/copied entry, and
	    tells where the renamed/copied contents came from.
--------------------------------------------------------
```

未合并的条目具有以下格式;第一个字符是“u”，用于区分普通的更改条目。

```
u <xy> <sub> <m1> <m2> <m3> <mW> <h1> <h2> <h3> <path>
```

```
Field       Meaning
--------------------------------------------------------
<XY>        A 2 character field describing the conflict type
	    as described in the short format.
<sub>       A 4 character field describing the submodule state
	    as described above.
<m1>        The octal file mode in stage 1.
<m2>        The octal file mode in stage 2.
<m3>        The octal file mode in stage 3.
<mW>        The octal file mode in the worktree.
<h1>        The object name in stage 1.
<h2>        The object name in stage 2.
<h3>        The object name in stage 3.
<path>      The pathname.
--------------------------------------------------------
```

### 其他项目

跟踪条目（如果需要），将打印一系列未跟踪的行，然后忽略在工作树中找到的项目。

未跟踪的项目具有以下格式：

```
? <path>
```

忽略的项目具有以下格式：

```
! <path>
```

### 路径名格式注释和-z

当给出`-z`选项时，路径名按原样打印，没有任何引号，行以 NUL（ASCII 0x00）字节终止。

如果没有`-z`选项，则会引用具有“异常”字符的路径名，如配置变量`core.quotePath`所述（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

## 配置

该命令符合`color.status`（或`status.color` - 它们的含义相同，后者保持向后兼容性）和`color.status.<slot>`配置变量以使其输出着色。

如果 config 变量`status.relativePaths`设置为 false，则显示的所有路径都相对于存储库根目录，而不是当前目录。

如果`status.submoduleSummary`设置为非零数字或为真（与-1 或无限数字相同），则将为长格式启用子模块摘要，并显示已修改子模块的提交摘要（请参阅 [git-submodule [1]](https://git-scm.com/docs/git-submodule) 的--summary-limit 选项)。请注意，当`diff.ignoreSubmodules`设置为 _all_ 时，或者仅对于那些`submodule.<name>.ignore=all`的子模块，将禁止所有子模块的 status 命令的摘要输出。要查看被忽略的子模块的摘要，您可以使用--ignore-submodules=dirty 命令行选项或 _git submodule summary_ 命令，该命令显示类似的输出但不遵循这些设置。

## 后台刷新

默认情况下，`git status`将自动刷新索引，从工作树更新缓存的统计信息并写出结果。写出更新的索引是一种并非严格必要的优化（`status`计算自身的值，但将它们写出来只是为了保证后续程序不重复的计算）。当`status`在后台运行时，写入期间保持的锁定可能与其他同时的进程冲突，导致它们失败。在后台运行`status`的脚本应考虑使用`git --no-optional-locks status`（详见 [git [1]](https://git-scm.com/docs/git) ）。

## 也可以看看

[gitignore [5]](https://git-scm.com/docs/gitignore)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件