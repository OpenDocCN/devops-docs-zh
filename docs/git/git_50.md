# git-format-patch

> 原文： [`git-scm.com/docs/git-format-patch`](https://git-scm.com/docs/git-format-patch)

## 名称

git-format-patch - 准备电子邮件提交补丁

## 概要

```
git format-patch [-k] [(-o|--output-directory) <dir> | --stdout]
		   [--no-thread | --thread[=<style>]]
		   [(--attach|--inline)[=<boundary>] | --no-attach]
		   [-s | --signoff]
		   [--signature=<signature> | --no-signature]
		   [--signature-file=<file>]
		   [-n | --numbered | -N | --no-numbered]
		   [--start-number <n>] [--numbered-files]
		   [--in-reply-to=Message-Id] [--suffix=.<sfx>]
		   [--ignore-if-in-upstream]
		   [--rfc] [--subject-prefix=Subject-Prefix]
		   [(--reroll-count|-v) <n>]
		   [--to=<email>] [--cc=<email>]
		   [--[no-]cover-letter] [--quiet] [--notes[=<ref>]]
		   [--interdiff=<previous>]
		   [--range-diff=<previous> [--creation-factor=<percent>]]
		   [--progress]
		   [<common diff options>]
		   [ <since> | <revision range> ]
```

## 描述

每次提交时，将每个提交的补丁准备在一个文件中，格式化为类似于 UNIX 邮箱格式。此命令的输出便于电子邮件提交或与 _git am_ 一起使用。

有两种方法可以指定要操作的提交。

1.  单个提交&lt; since&gt;，指定通往当前分支的提示的提交，这些提交不在历史记录中，导致&lt; since&gt;要输出。

2.  通用&lt;修订范围&gt;表达式（参见 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 中的“指定修订”部分）表示指定范围内的提交。

在单个&lt; commit&gt;的情况下，第一个规则优先。要应用第二个规则，即从历史开始直到&lt; commit&gt;格式化所有内容，请使用`--root`选项：`git format-patch --root &lt;commit&gt;`。如果您只想格式化&lt; commit&gt;本身，您可以使用`git format-patch -1 &lt;commit&gt;`执行此操作。

默认情况下，每个输出文件从 1 开始按顺序编号，并使用提交消息的第一行（为路径名安全性进行按摩）作为文件名。使用`--numbered-files`选项，输出文件名将只是数字，而不会附加提交的第一行。除非指定了`--stdout`选项，否则输出文件的名称将打印到标准输出。

如果指定了`-o`，则输出文件将在&lt; dir&gt;中创建。否则，它们将在当前工作目录中创建。可以使用`format.outputDirectory`配置选项设置默认路径。 `-o`选项优先于`format.outputDirectory`。要将补丁存储在当前工作目录中，即使`format.outputDirectory`指向其他位置，也请使用`-o .`。

默认情况下，单个补丁的主题是“[PATCH]”，后跟从提交消息到第一个空行的串联（参见 [git-commit [1]](https://git-scm.com/docs/git-commit) 的讨论部分） 。

当输出多个补丁时，主题前缀将改为“[PATCH n / m]”。要强制为单个补丁添加 1/1，请使用`-n`。要忽略主题中的色块编号，请使用`-N`。

如果给出`--thread`，`git-format-patch`将生成`In-Reply-To`和`References`标题，以使第二个和后续的补丁邮件显示为对第一个邮件的回复;这也会生成一个`Message-Id`标题来引用。

## OPTIONS

```
 -p 
```

```
 --no-stat 
```

生成没有任何 diffstats 的普通补丁。

```
 -U<n> 
```

```
 --unified=<n> 
```

用&lt; n&gt;生成差异。上下文而不是通常的三行。

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
 --no-renames 
```

关闭重命名检测，即使配置文件提供默认值也是如此。

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

检测重命名。如果指定了`n`，则它是相似性指数的阈值（即与文件大小相比的添加/删除量）。例如，`-M90%`表示如果超过 90％的文件未更改，Git 应将删除/添加对视为重命名。如果没有`%`符号，则该数字将作为分数读取，并在其前面加上小数点。即，`-M5`变为 0.5，因此与`-M50%`相同。同样，`-M05`与`-M5%`相同。要将检测限制为精确重命名，请使用`-M100%`。默认相似性指数为 50％。

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

```
 -<n> 
```

从最顶层&lt; n&gt;准备补丁。提交。

```
 -o <dir> 
```

```
 --output-directory <dir> 
```

使用&lt; dir&gt;存储生成的文件，而不是当前的工作目录。

```
 -n 
```

```
 --numbered 
```

名称输出为 _[PATCH n / m]_ 格式，即使只有一个补丁。

```
 -N 
```

```
 --no-numbered 
```

以 _[PATCH]_ 格式输出名称。

```
 --start-number <n> 
```

开始在&lt; n&gt;处对补丁进行编号。而不是 1。

```
 --numbered-files 
```

输出文件名将是一个简单的数字序列，不附加提交的默认第一行。

```
 -k 
```

```
 --keep-subject 
```

不要从提交日志消息的第一行剥离/添加 _[PATCH]_ 。

```
 -s 
```

```
 --signoff 
```

使用您自己的提交者标识将`Signed-off-by:`行添加到提交消息中。有关详细信息，请参阅 [git-commit [1]](https://git-scm.com/docs/git-commit) 中的签收选项。

```
 --stdout 
```

以 mbox 格式将所有提交打印到标准输出，而不是为每个提交创建文件。

```
 --attach[=<boundary>] 
```

使用`Content-Disposition: attachment`创建多部分/混合附件，第一部分是提交消息，第二部分是补丁本身。

```
 --no-attach 
```

禁用附件的创建，覆盖配置设置。

```
 --inline[=<boundary>] 
```

使用`Content-Disposition: inline`创建多部分/混合附件，第一部分是提交消息，第二部分是补丁本身。

```
 --thread[=<style>] 
```

```
 --no-thread 
```

控制添加`In-Reply-To`和`References`标题以使第二个和后续邮件显示为对第一个邮件的回复。还控制`Message-Id`标头的生成以供参考。

可选的&lt; style&gt;参数可以是`shallow`或`deep`。 _ 浅 _ 线程使每个邮件都回复到系列的头部，其中头部是从求职信，`--in-reply-to`和第一个补丁邮件中按顺序选择的。 _ 深 _ 线程使每封邮件都回复上一封邮件。

除非设置了`format.thread`配置，否则默认值为`--no-thread`。如果指定`--thread`时没有样式，则默认为`format.thread`指定的样式（如果有），否则为`shallow`。

请注意， _git send-email_ 的默认设置是自行编排电子邮件。如果希望`git format-patch`处理线程，则需要确保为`git send-email`禁用线程。

```
 --in-reply-to=Message-Id 
```

使第一封邮件（或所有带有`--no-thread`的邮件）显示为对给定 Message-Id 的回复，这可以避免破坏线程以提供新的补丁系列。

```
 --ignore-if-in-upstream 
```

请勿在&lt; until&gt; ..&lt; since&gt;中包含与提交相匹配的修补程序。这将检查从&lt; since&gt;可到达的所有补丁。但不是来自&lt; until&gt;并将它们与正在生成的补丁进行比较，并忽略任何匹配的补丁。

```
 --subject-prefix=<Subject-Prefix> 
```

而不是主题行中的标准 _[PATCH]_ 前缀，而是使用 _[&lt; Subject-Prefix&gt;]_ 。这允许对补丁系列进行有用的命名，并且可以与`--numbered`选项组合使用。

```
 --rfc 
```

`--subject-prefix="RFC PATCH"`的别名。 RFC 表示“征求意见”;发送实验补丁进行讨论而不是应用时使用此功能。

```
 -v <n> 
```

```
 --reroll-count=<n> 
```

将该系列标记为该主题的第 n 次迭代。输出文件名前面有`v&lt;n&gt;`，主题前缀（默认情况下为“PATCH”，但可通过`--subject-prefix`选项配置）附加了“v&lt; n&gt;”。例如。 `--reroll-count=4`可能会生成[主题：[PATCH v4 1/20]添加 makefile“的`v4-0001-add-makefile.patch`文件。

```
 --to=<email> 
```

将`To:`标头添加到电子邮件标头中。这是对任何已配置标头的补充，可以多次使用。否定形式`--no-to`丢弃到目前为止添加的所有`To:`标题（从配置或命令行）。

```
 --cc=<email> 
```

将`Cc:`标头添加到电子邮件标头中。这是对任何已配置标头的补充，可以多次使用。否定形式`--no-cc`丢弃到目前为止添加的所有`Cc:`标题（从配置或命令行）。

```
 --from 
```

```
 --from=<ident> 
```

在每个提交电子邮件的`From:`标题中使用`ident`。如果提交的作者标识在文本上与提供的`ident`不同，则在原始作者的消息正文中放置`From:`标题。如果没有给出`ident`，请使用提交者标识。

请注意，此选项仅在您实际发送电子邮件并希望将自己标识为发件人时才有用，但保留原始作者（并且`git am`将正确选取体内标题）。另请注意，`git send-email`已经为您处理了此转换，如果将结果输入`git send-email`，则不应使用此选项。

```
 --add-header=<header> 
```

向电子邮件标头添加任意标头。这是对任何已配置标头的补充，可以多次使用。例如，`--add-header="Organization: git-foo"`。否定形式`--no-add-header`丢弃**到目前为止从配置或命令行添加的所有**（`To:`，`Cc:`和自定义）标题。

```
 --[no-]cover-letter 
```

除了补丁之外，还生成一个包含分支描述，短信和整体 diffstat 的求职信文件。您可以在发送之前在文件中填写说明。

```
 --interdiff=<previous> 
```

作为审阅者辅助工具，请在封面信中插入一个 interdiff，或作为单补丁系列的单个补丁的注释，显示补丁系列的先前版本与当前正在格式化的系列之间的差异。 `previous`是一个单一的修订版，命名前一个系列的提示，它与正在格式化的系列共享一个共同的基础（例如`git format-patch --cover-letter --interdiff=feature/v1 -3 feature/v2`）。

```
 --range-diff=<previous> 
```

作为评论者的帮助，将一个范围差异（参见 [git-range-diff [1]](https://git-scm.com/docs/git-range-diff) ）插入到求职信中，或作为单补丁系列的单个补丁的评论，显示之间的差异补丁系列的先前版本和当前正在格式化的系列。 `previous`可以是命名前一系列提示的单个修订，如果它与正在格式化的系列共享一个公共基础（例如`git format-patch --cover-letter --range-diff=feature/v1 -3 feature/v2`），或者如果系列的两个版本不相交，则为修订范围（例如`git format-patch --cover-letter --range-diff=feature/v1~3..feature/v1 -3 feature/v2`）。

请注意，传递给命令的 diff 选项会影响`format-patch`的主要产品的生成方式，并且它们不会传递给用于生成封面信函材料的基础`range-diff`机器（这可能在将来发生变化）。

```
 --creation-factor=<percent> 
```

与`--range-diff`一起使用，通过调整创建/删除成本软糖因子，调整与先前和当前系列补丁之间的提交匹配的启发式。有关详细信息，请参阅 [git-range-diff [1]](https://git-scm.com/docs/git-range-diff) ）。

```
 --notes[=<ref>] 
```

在三个虚线后添加注释（参见 [git-notes [1]](https://git-scm.com/docs/git-notes) ）进行提交。

这种情况的预期用例是为不属于提交日志消息的提交编写支持说明，并将其包含在补丁提交中。虽然可以在`format-patch`运行之后但在发送之前简单地编写这些解释，但将它们保留为 Git 注释允许它们在补丁系列的版本之间进行维护（但请参阅 [git 中`notes.rewrite`配置选项的讨论） -notes [1]](https://git-scm.com/docs/git-notes) 使用此工作流程）。

```
 --[no-]signature=<signature> 
```

为每条生成的消息添加签名。根据 RFC 3676，签名通过一条带有“ - ”的行与主体分开。如果省略签名选项，则签名默认为 Git 版本号。

```
 --signature-file=<file> 
```

就像--signature 一样工作，除了从文件中读取签名。

```
 --suffix=.<sfx> 
```

不使用`.patch`作为生成的文件名的后缀，而是使用指定的后缀。一种常见的替代方案是`--suffix=.txt`。保留此空将删除`.patch`后缀。

请注意，前导字符不必是点;例如，您可以使用`--suffix=-patch`来获取`0001-description-of-my-change-patch`。

```
 -q 
```

```
 --quiet 
```

不要将生成的文件的名称打印到标准输出。

```
 --no-binary 
```

不要输出二进制文件中的更改内容，而是显示这些文件发生更改的通知。使用此选项生成的修补程序无法正确应用，但它们仍可用于代码审查。

```
 --zero-commit 
```

在每个补丁的 From 头中输出一个全零散列，而不是提交的散列。

```
 --base=<commit> 
```

记录基础树信息以识别修补程序系列适用的状态。有关详细信息，请参阅下面的“基础树信息”部分。

```
 --root 
```

将修订参数视为&lt; revision range&gt;，即使它只是一次提交（通常将其视为&lt; since&gt;）。请注意，指定范围中包含的根提交始终格式化为创建修补程序，与此标志无关。

```
 --progress 
```

在生成修补程序时显示有关 stderr 的进度报告。

## 组态

您可以指定要添加到每封邮件的额外邮件标题行，主题前缀和文件后缀的默认值，输出多个补丁时的编号补丁，添加“收件人”或“抄送：”标题，配置附件和签署补丁配置变量。

```
[format]
	headers = "Organization: git-foo\n"
	subjectPrefix = CHANGE
	suffix = .txt
	numbered = auto
	to = <email>
	cc = <email>
	attach [ = mime-boundary-string ]
	signOff = true
	coverletter = auto
```

## 讨论

由 _git format-patch_ 生成的补丁是 UNIX 邮箱格式，带有固定的“魔术”时间戳，表示文件是从格式补丁而不是真实邮箱输出的，如下所示：

```
From 8f72bad1baf19a53459661343e21d6491c3908d3 Mon Sep 17 00:00:00 2001
From: Tony Luck <tony.luck@intel.com>
Date: Tue, 13 Jul 2010 11:42:54 -0700
Subject: [PATCH] =?UTF-8?q?[IA64]=20Put=20ia64=20config=20files=20on=20the=20?=
 =?UTF-8?q?Uwe=20Kleine-K=C3=B6nig=20diet?=
MIME-Version: 1.0
Content-Type: text/plain; charset=UTF-8
Content-Transfer-Encoding: 8bit

arch/arm config files were slimmed down using a python script
(See commit c2330e286f68f1c408b4aa6515ba49d57f05beae comment)

Do the same for ia64 so we can have sleek & trim looking
...
```

通常情况下，它会被放置在 MUA 的草稿文件夹中，编辑后添加及时的评论，不应该在三个破折号后进入更改日志，然后作为消息发送，在我们的示例中，其主体以“arch / arm 配置文件”开头...”。在接收端，读者可以在 UNIX 邮箱中保存有趣的补丁，并将其应用于 [git-am [1]](https://git-scm.com/docs/git-am) 。

当补丁是正在进行的讨论的一部分时， _git format-patch_ 生成的补丁可以调整以利用 _git am --scissors_ 功能。在您对讨论的回复之后出现了一条仅包含“`-- &gt;8 --`”（剪刀和穿孔）的行，然后删除了不必要的标题字段的补丁：

```
...
> So we should do such-and-such.

Makes sense to me.  How about this patch?

-- >8 --
Subject: [IA64] Put ia64 config files on the Uwe Kleine-K��nig diet

arch/arm config files were slimmed down using a python script
...
```

以这种方式发送补丁时，大多数情况下您都在发送自己的补丁，因此除了“`From $SHA1 $magic_timestamp`”标记外，您还应该省略补丁文件中的`From:`和`Date:`行。修补程序标题可能与修补程序响应的讨论主题不同，因此您可能希望保留 Subject：行，就像上面的示例一样。

### 检查修补程序损坏

如果没有正确设置许多邮件程序将破坏空白。以下是两种常见的腐败类型：

*   没有 _ 任何 _ 空格的空上下文行。

*   非空上下文行，在开头有一个额外的空格。

测试 MUA 设置是否正确的一种方法是：

*   除了使用不包含列表和维护者地址的 To：和 Cc：行之外，完全按照您的方式发送补丁。

*   将该修补程序保存为 UNIX 邮箱格式的文件。比如叫它 a.patch。

*   适用它：

    ```
    $ git fetch &lt;project&gt; master:test-apply
    $ git checkout test-apply
    $ git reset --hard
    $ git am a.patch
    ```

如果它没有正确应用，可能有各种原因。

*   补丁本身并不干净。那是 _ 坏 _，但与你的 MUA 没什么关系。在这种情况下重新生成之前，您可能需要使用 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 来修改补丁。

*   MUA 破坏了你的补丁; “我”会抱怨补丁不适用。查看.git / rebase-apply /子目录，查看 _ 补丁 _ 文件包含的内容，并检查上面提到的常见损坏模式。

*   在此期间，检查 _ 信息 _ 和 _ 最终提交 _ 文件。如果 _final-commit_ 中的内容不是您希望在提交日志消息中看到的内容，那么接收器最终可能会在应用您的修补程序时手动编辑日志消息。诸如“嗨，这是我的第一个补丁。\ n”在补丁电子邮件中的内容应该出现在表示提交消息结束的三个虚线之后。

## 特定于 MUA 的提示

以下是有关如何使用各种邮件程序成功提交内联补丁的一些提示。

### GMail 的

GMail 无法在网络界面中关闭换行，因此它会破坏您发送的任何电子邮件。但是，您可以使用“git send-email”并通过 GMail SMTP 服务器发送补丁，或使用任何 IMAP 电子邮件客户端连接到 Google IMAP 服务器并通过该服务器转发电子邮件。

有关使用 _git send-email_ 通过 GMail SMTP 服务器发送补丁的提示，请参阅 [git-send-email [1]](https://git-scm.com/docs/git-send-email) 的示例部分。

有关使用 IMAP 界面提交的提示，请参阅 [git-imap-send [1]](https://git-scm.com/docs/git-imap-send) 的示例部分。

### 雷鸟

默认情况下，Thunderbird 会将电子邮件包装起来并将其标记为 _format = flowed_ ，这两种情况都会导致 Git 无法使用该电子邮件。

有三种不同的方法：使用附加组件来关闭换行，配置 Thunderbird 以不破坏补丁，或者使用外部编辑器来防止 Thunderbird 破坏补丁。

#### 方法＃1（附加）

安装可从 [`addons.mozilla.org/thunderbird/addon/toggle-word-wrap/`](https://addons.mozilla.org/thunderbird/addon/toggle-word-wrap/) 获得的 Toggle Word Wrap 附加组件。它添加了一个菜单项“启用 Word Wrap”作曲家的“选项”菜单，您可以勾选。现在你可以像你一样编写消息（剪切+粘贴， _git format-patch_ | _git imap-send_ 等），但你必须在任何地方手动插入换行符您键入的文本。

#### 方法＃2（配置）

三个步骤：

1.  将您的邮件服务器组成配置为纯文本：编辑...帐户设置...组合＆amp;寻址，取消选中“以 HTML 格式撰写邮件”。

2.  将常规合成窗口配置为不换行。

    在 Thunderbird 2 中：Edit..Preferences..Composition，将纯文本消息包装在 0

    在 Thunderbird 3：Edit..Preferences..Advanced..Config 编辑器。搜索“mail.wrap_long_lines”。切换它以确保它设置为`false`。此外，搜索“mailnews.wraplength”并将值设置为 0。

3.  禁用 format = flowed：Edit..Preferences..Advanced..Config Editor。搜索“mailnews.send_plaintext_flowed”。切换它以确保它设置为`false`。

完成之后，你应该能够像你一样编写电子邮件（剪切+粘贴， _git 格式补丁 _ | _git imap-send_ 等），补丁将不被破坏。

#### 方法＃3（外部编辑）

需要以下 Thunderbird 扩展：来自 [`aboutconfig.mozdev.org/`](http://aboutconfig.mozdev.org/) 的 AboutConfig 和来自[的外部编辑 http://globs.org/articles.php?lng=en&pg= 8](http://globs.org/articles.php?lng=en&pg=8)

1.  使用您选择的方法将补丁准备为文本文件。

2.  在打开撰写窗口之前，请使用编辑→帐户设置取消选中要用于发送修补程序的帐户的“撰写和寻址”面板中的“以 HTML 格式撰写邮件”设置。

3.  在主要的 Thunderbird 窗口中，之前的 _ 打开补丁的撰写窗口，使用工具→about：config 将以下内容设置为指示的值：_

    ```
    	mailnews.send_plaintext_flowed  =&gt; false
    	mailnews.wraplength             =&gt; 0
    ```

4.  打开撰写窗口，然后单击外部编辑器图标。

5.  在外部编辑器窗口中，读入补丁文件并正常退出编辑器。

附注：可以使用 about：config 和以下设置执行第 2 步，但尚未尝试过任何人。

```
	mail.html_compose                       => false
	mail.identity.default.compose_html      => false
	mail.identity.id?.compose_html          => false
```

contrib / thunderbird-patch-inline 中有一个脚本可以帮助您以简单的方式包含 Thunderbird 的补丁。要使用它，请执行上述步骤，然后将脚本用作外部编辑器。

### KMail 的

这应该可以帮助您使用 KMail 内联提交补丁。

1.  准备补丁作为文本文件。

2.  单击“新邮件”。

3.  转到 Composer 窗口中的“选项”下，确保未设置“自动换行”。

4.  使用消息→插入文件...并插入补丁。

5.  回到撰写窗口：在邮件中添加您希望的任何其他文本，完成寻址和主题字段，然后按发送。

## 基础树信息

基础树信息块用于维护人员或第三方测试人员，以了解补丁系列适用的确切状态。它由 _ 基础提交 _ 组成，这是一个众所周知的提交，它是项目历史中其他人工作的稳定部分的一部分，以及零个或多个 _ 先决条件补丁 _，飞行中众所周知的补丁尚未成为 _ 基础提交 _ 的一部分，需要在应用补丁之前以拓扑顺序应用于 _ 基础提交 _ 之上。

_base commit_ 显示为“base-commit：”，后跟提交对象名称的 40-hex。 _ 先决条件补丁 _ 显示为“prerequisite-patch-id：”，后跟 40-hex _ 补丁 ID_ ，可以通过`git patch-id --stable`命令传递补丁获得。

想象一下，除了公共提交 P 之外，你从其他人那里应用了着名的补丁 X，Y 和 Z，然后构建了你的三个补丁系列 A，B，C，历史就像：

```
---P---X---Y---Z---A---B---C
```

使用`git format-patch --base=P -3 C`（或其变体，例如使用`--cover-letter`或使用`Z..C`而不是`-3 C`来指定范围），基本树信息块显示在命令输出的第一条消息的末尾（要么第一个补丁，或求职信），像这样：

```
base-commit: P
prerequisite-patch-id: X
prerequisite-patch-id: Y
prerequisite-patch-id: Z
```

对于非线性拓扑，例如

```
---P---X---A---M---C
    \         /
     Y---Z---B
```

您还可以使用`git format-patch --base=P -3 C`为 A，B 和 C 生成补丁，并在第一条消息的末尾附加 P，X，Y，Z 的标识符。

如果在 cmdline 中设置`--base=auto`，它将自动跟踪基本提交，基本提交将是远程跟踪分支的提示提交和 cmdline 中指定的修订范围的合并基础。对于本地分支，您需要在使用此选项之前通过`git branch --set-upstream-to`跟踪远程分支。

## 例子

*   在版本 R1 和 R2 之间提取提交，并使用 _git am_ 将它们应用于当前分支之上以挑选它们：

    ```
    $ git format-patch -k --stdout R1..R2 | git am -3 -k
    ```

*   提取当前分支中但不在原始分支中的所有提交：

    ```
    $ git format-patch origin
    ```

    对于每个提交，在当前目录中创建单独的文件。

*   从项目开始以来，提取导致 _ 起源 _ 的所有提交：

    ```
    $ git format-patch --root origin
    ```

*   与前一个相同：

    ```
    $ git format-patch -M -B origin
    ```

    此外，它可以检测并处理重命名并智能地完成重写以生成重命名补丁。重命名补丁可减少文本输出量，并且通常可以更轻松地进行查看。请注意，非 Git“补丁”程序将无法理解重命名补丁，因此仅在您知道收件人使用 Git 应用补丁时才使用它。

*   从当前分支中提取三个最顶层的提交，并将它们格式化为可通过电子邮件发送的补丁：

    ```
    $ git format-patch -3
    ```

## 也可以看看

[git-am [1]](https://git-scm.com/docs/git-am) ， [git-send-email [1]](https://git-scm.com/docs/git-send-email)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件