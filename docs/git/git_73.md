# git-ls-files

> 原文： [`git-scm.com/docs/git-ls-files`](https://git-scm.com/docs/git-ls-files)

## 名称

git-ls-files - 显示索引和工作树中的文件信息

## 概要

```
git ls-files [-z] [-t] [-v] [-f]
		(--[cached|deleted|others|ignored|stage|unmerged|killed|modified])*
		(-[c|d|o|i|s|u|k|m])*
		[--eol]
		[-x <pattern>|--exclude=<pattern>]
		[-X <file>|--exclude-from=<file>]
		[--exclude-per-directory=<file>]
		[--exclude-standard]
		[--error-unmatch] [--with-tree=<tree-ish>]
		[--full-name] [--recurse-submodules]
		[--abbrev] [--] [<file>…​]
```

## 描述

这将目录高速缓存索引中的文件列表与实际工作目录列表合并，并显示两者的不同组合。

以下一个或多个选项可用于确定显示的文件：

## OPTIONS

```
 -c 
```

```
 --cached 
```

在输出中显示缓存的文件（默认）

```
 -d 
```

```
 --deleted 
```

在输出中显示已删除的文件

```
 -m 
```

```
 --modified 
```

在输出中显示已修改的文件

```
 -o 
```

```
 --others 
```

在输出中显示其他（即未跟踪）文件

```
 -i 
```

```
 --ignored 
```

仅显示输出中的忽略文件。在索引中显示文件时，仅打印与排除模式匹配的文件。显示“其他”文件时，仅显示与排除模式匹配的文件。标准忽略规则不会自动激活，因此至少需要一个`--exclude*`选项。

```
 -s 
```

```
 --stage 
```

在输出中显示暂存内容的模式位，对象名称和阶段编号。

```
 --directory 
```

如果整个目录被归类为“其他”，则只显示其名称（带斜杠）而不是其全部内容。

```
 --no-empty-directory 
```

不要列出空目录。没有--directory 没有效果。

```
 -u 
```

```
 --unmerged 
```

在输出中显示未合并的文件（强制 - 阶段）

```
 -k 
```

```
 --killed 
```

显示文件系统上由于文件/目录冲突而需要删除的文件，以使 checkout-index 成功。

```
 -z 
```

\ 0 输出行终止，不引用文件名。有关详细信息，请参阅下面的 OUTPUT。

```
 -x <pattern> 
```

```
 --exclude=<pattern> 
```

跳过与图案匹配的未跟踪文件。请注意，pattern 是 shell 通配符模式。有关详细信息，请参阅下面的 EXCLUDE PATTERNS。

```
 -X <file> 
```

```
 --exclude-from=<file> 
```

从&lt; file&gt;中读取排除模式;每行 1 个。

```
 --exclude-per-directory=<file> 
```

阅读仅适用于&lt; file&gt;中的目录及其子目录的其他排除模式。

```
 --exclude-standard 
```

添加标准 Git 排除项：.git / info / exclude，.gitignore 在每个目录中，以及用户的全局排除文件。

```
 --error-unmatch 
```

如果有任何&lt; file&gt;没有出现在索引中，将其视为错误（返回 1）。

```
 --with-tree=<tree-ish> 
```

使用--error-unmatch 扩展用户提供的&lt; file&gt;时（即路径模式）路径的参数，假装自从命名的&lt; tree-ish&gt;以来在索引中删除的路径。仍然存在。将此选项与`-s`或`-u`选项一起使用没有任何意义。

```
 -t 
```

此功能已被半弃用。出于脚本目的， [git-status [1]](https://git-scm.com/docs/git-status) `--porcelain`和 [git-diff-files [1]](https://git-scm.com/docs/git-diff-files) `--name-status`几乎总是优秀的替代品，用户应该看一下 [git-status [1]](https://git-scm.com/docs/git-status) `--short`或 [git-diff [1]](https://git-scm.com/docs/git-diff) `--name-status`，用于更加用户友好的替代品。

此选项在每行的开头标识具有以下标记（后跟空格）的文件状态：

```
 H 
```

缓存

```
 S 
```

跳 worktree

```
 M 
```

未合并

```
 R 
```

删除/删除

```
 C 
```

改性的/改变

```
 K 
```

被杀

```
 ? 
```

其他

```
 -v 
```

与`-t`类似，但对于标记为 _ 的文件使用小写字母假设不变 _（参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) ）。

```
 -f 
```

与`-t`类似，但对于标记为 _fsmonitor 有效 _ 的文件使用小写字母（参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) ）。

```
 --full-name 
```

从子目录运行时，该命令通常输出相对于当前目录的路径。此选项强制路径相对于项目顶级目录输出。

```
 --recurse-submodules 
```

递归调用存储库中每个子模块上的 ls-files。目前只支持--cached 模式。

```
 --abbrev[=<n>] 
```

不显示完整的 40 字节十六进制对象行，而是仅显示部分前缀。可以使用--abbrev =&lt; n&gt;指定非默认位数。

```
 --debug 
```

在描述文件的每一行之后，添加有关其缓存条目的更多数据。这旨在显示尽可能多的手动检查信息;确切的格式可能随时改变。

```
 --eol 
```

显示&lt; eolinfo&gt;和&lt; eolattr&gt;的文件。 &lt; eolinfo&gt;是当“text”属性为“auto”（或未设置且 core.autocrlf 不为 false）时 Git 使用的文件内容标识。 &lt; eolinfo&gt;是“-text”，“none”，“lf”，“crlf”，“mixed”或“”。

“”表示该文件不是常规文件，它不在索引中或在工作树中不可访问。

&lt; eolattr&gt;是签出或提交时使用的属性，它是“”，“ - text”，“text”，“text = auto”，“text eol = lf”，“text eol = crlf”。由于支持 Git 2.10“text = auto eol = lf”和“text = auto eol = crlf”。

&lt; eolinfo&gt;都是在索引（“i /&lt; eolinfo&gt;”）和工作树（“w /&lt; eolinfo&gt;”）中显示常规文件，然后是（“attr /&lt; eolattr&gt;”）。

```
 -- 
```

不要将任何更多的参数解释为选项。

```
 <file> 
```

要显示的文件。如果没有给出文件，则显示与其他指定条件匹配的所有文件。

## OUTPUT

_git ls-files_ 只输出文件名，除非指定了`--stage`，在这种情况下输出：

```
[<tag> ]<mode> <object> <stage> <file>
```

_git ls-files --ee_ 将显示 i /&lt; eolinfo&gt;&lt; SPACES&gt; w /&lt; eolinfo&gt;&lt; SPACES&gt; attr /&lt; eolattr&gt;&lt; SPACE *&gt;&lt; TAB&gt; ;&lt;文件&gt;

_git ls-files --unmerged_ 和 _git ls-files --stage_ 可用于检查未合并路径的详细信息。

对于未合并的路径，索引不是记录单个模式/ SHA-1 对，而是记录最多三个这样的对;一个来自阶段 1 中的树 O，阶段 2 中的 A 和阶段 3 中的 B.一个用户（或瓷器）可以使用该信息来查看最终应该在路径上记录的内容。 （有关状态的更多信息，请参见 [git-read-tree [1]](https://git-scm.com/docs/git-read-tree) ）

如果没有`-z`选项，则会引用具有“异常”字符的路径名，如配置变量`core.quotePath`所述（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。使用`-z`，文件名逐字输出，行以 NUL 字节终止。

## 排除模式

_git ls-files_ 可以在遍历目录树时使用“排除模式”列表，并查找文件以显示指定标志 - 其他或--ignored 的时间。 [gitignore [5]](https://git-scm.com/docs/gitignore) 指定排除模式的格式。

这些排除模式来自这些地方，依次为：

1.  命令行标志--exclude =&lt; pattern&gt;指定单个模式。模式的排序顺序与它们在命令行中出现的顺序相同。

2.  命令行标志--exclude-from =&lt; file&gt;指定包含模式列表的文件。模式的排序顺序与文件中出现的顺序相同。

3.  命令行标志--exclude-per-directory =&lt; name&gt;指定每个目录 _git ls-files_ 检查的文件名，通常是`.gitignore`。更深层目录中的文件优先。模式的排序顺序与文件中出现的顺序相同。

在命令行中使用--exclude 或从--exclude-from 指定的文件读取的模式相对于目录树的顶部。从--exclude-per-directory 指定的文件读取的模式是相对于模式文件出现的目录。

## 也可以看看

[git-read-tree [1]](https://git-scm.com/docs/git-read-tree) ， [gitignore [5]](https://git-scm.com/docs/gitignore)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件