# git-diff-index

> 原文： [`git-scm.com/docs/git-diff-index`](https://git-scm.com/docs/git-diff-index)

## 名称

git-diff-index - 将树与工作树或索引进行比较

## 概要

```
git diff-index [-m] [--cached] [<common diff options>] <tree-ish> [<path>…​]
```

## 描述

将树对象中找到的 blob 的内容和模式与工作树中相应的跟踪文件或索引中的相应路径进行比较。当&lt; path&gt;存在参数，仅比较与这些模式匹配的路径。否则，将比较所有跟踪的文件。

## OPTIONS

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

以原始格式生成 diff。这是默认值。

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

当给出`--raw`，`--numstat`，`--name-only`或`--name-status`时，不要使用路径名并使用 NUL 作为输出字段终止符。

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
 --exit-code 
```

使用类似于 diff（1）的代码退出程序。也就是说，如果存在差异则退出 1，0 表示没有差异。

```
 --quiet 
```

禁用程序的所有输出。意味着`--exit-code`。

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
 <tree-ish> 
```

要反对的树对象的 id。

```
 --cached 
```

根本不考虑磁盘上的文件

```
 -m 
```

默认情况下，索引中记录但未签出的文件将报告为已删除。该标志使 _git diff-index_ 表示所有未签出的文件都是最新的。

## 原始输出格式

来自“git-diff-index”，“git-diff-tree”，“git-diff-files”和“git diff -raw”的原始输出格式非常相似。

这些命令都比较了两组东西;比较的不同之处是：

```
 git-diff-index <tree-ish> 
```

比较&lt; tree-ish&gt;以及文件系统上的文件。

```
 git-diff-index --cached <tree-ish> 
```

比较&lt; tree-ish&gt;和索引。

```
 git-diff-tree [-r] <tree-ish-1> <tree-ish-2> [<pattern>…​] 
```

比较两个参数命名的树。

```
 git-diff-files [<pattern>…​] 
```

比较索引和文件系统上的文件。

“git-diff-tree”命令通过打印正在比较的内容的哈希来开始输出。之后，所有命令都会为每个更改的文件打印一个输出行。

输出行以这种方式格式化：

```
in-place edit  :100644 100644 bcd1234 0123456 M file0
copy-edit      :100644 100644 abcd123 1234567 C68 file1 file2
rename-edit    :100644 100644 abcd123 1234567 R86 file1 file3
create         :000000 100644 0000000 1234567 A file4
delete         :100644 000000 1234567 0000000 D file5
unmerged       :000000 000000 0000000 0000000 U file6
```

也就是说，从左到右：

1.  一个冒号。

2.  “src”模式;如果创建或未合并，则为 000000。

3.  空间。

4.  “dst”模式;如果删除或未合并，则为 000000。

5.  空间。

6.  sha1 为“src”; 0 {40}如果创建或未合并。

7.  空间。

8.  sha1 为“dst”; 0 {40}如果创建，未合并或“查看工作树”。

9.  空间。

10.  状态，后跟可选的“分数”编号。

11.  使用`-z`选项时的选项卡或 NUL。

12.  “src”的路径

13.  使用`-z`选项时的选项卡或 NUL;仅适用于 C 或 R.

14.  “dst”的路径;仅适用于 C 或 R.

15.  使用`-z`选项时，LF 或 NUL 终止记录。

可能的状态字母是：

*   答：添加文件

*   C：将文件复制到新文件中

*   D：删除文件

*   M：修改文件的内容或模式

*   R：重命名文件

*   T：更改文件类型

*   U：文件已取消合并（您必须先完成合并才能提交）

*   X：“未知”更改类型（最有可能是错误，请报告）

状态字母 C 和 R 后面总是跟一个分数（表示移动或复制的源和目标之间的相似性百分比）。状态字母 M 之后可以是文件重写的分数（表示不相似的百分比）。

&lt; SHA1&gt;如果文件系统上的文件是新文件并且它与索引不同步，则显示为全 0。

例：

```
:100644 100644 5be4a4a 0000000 M file.c
```

如果没有`-z`选项，则会引用具有“异常”字符的路径名，如配置变量`core.quotePath`所述（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。使用`-z`，文件名逐字输出，行以 NUL 字节终止。

## 用于合并的 diff 格式

“git-diff-tree”，“git-diff-files”和“git-diff --raw”可以使用`-c`或`--cc`选项为合并提交生成 diff 输出。输出与上述格式的不同之处如下：

1.  每个父母都有一个冒号

2.  还有更多“src”模式和“src”sha1

3.  status 是每个父级的连接状态字符

4.  没有可选的“得分”号码

5.  单路径，仅适用于“dst”

例：

```
::100644 100644 100644 fabadb8 cc95eb0 4866510 MM	describe.c
```

请注意，_ 组合 diff_ 仅列出从所有父项修改的文件。

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

## 其他差异格式

`--summary`选项描述新添加，删除，重命名和复制的文件。 `--stat`选项将 diffstat（1）图形添加到输出。这些选项可以与其他选项结合使用，例如`-p`，用于人类消费。

当显示涉及重命名或副本的更改时，`--stat`输出通过组合路径名的公共前缀和后缀来紧凑地格式化路径名。例如，修改 4 行时将`arch/i386/Makefile`移动到`arch/x86/Makefile`的更改将显示如下：

```
arch/{i386 => x86}/Makefile    |   4 +--
```

`--numstat`选项提供 diffstat（1）信息，但设计用于更容易的机器消耗。 `--numstat`输出中的条目如下所示：

```
1	2	README
3	1	arch/{i386 => x86}/Makefile
```

也就是说，从左到右：

1.  添加的行数;

2.  标签;

3.  删除的行数;

4.  标签;

5.  pathname（可能带有重命名/复制信息）;

6.  换行符。

当`-z`输出选项生效时，输出格式为：

```
1	2	README NUL
3	1	NUL arch/i386/Makefile NUL arch/x86/Makefile NUL
```

那是：

1.  添加的行数;

2.  标签;

3.  删除的行数;

4.  标签;

5.  NUL（仅在重命名/复制时存在）;

6.  原像中的路径名;

7.  NUL（仅在重命名/复制时存在）;

8.  postimage 中的路径名（仅在重命名/复制时存在）;

9.  一个 NUL。

在重命名的情况下，preimage 路径之前的额外`NUL`是允许读取输出的脚本判断正在读取的当前记录是单路径记录还是重命名/复制记录而无需提前读取。读取添加和删除的行后，读取`NUL`将产生路径名，但如果是`NUL`，则记录将显示两个路径。

## 运营模式

您可以选择是否完全信任索引文件（使用`--cached`标志）或要求 diff 逻辑显示任何与 stat 状态不匹配的文件为“暂时更改”。这两项操作确实非常有用。

## 缓存模式

如果指定了`--cached`，则可以询问：

```
show me the differences between HEAD and the current index
contents (the ones I'd write using 'git write-tree')
```

例如，假设您已经处理了工作目录，更新了索引中的一些文件并准备提交。你想要确切地看到**你将提交什么**，而不必编写新的树对象并以这种方式进行比较，为此，你只需要做

```
git diff-index --cached HEAD
```

示例：假设我已将`commit.c`重命名为`git-commit.c`，并且我已经完成了`update-index`以使其在索引文件中生效。 `git diff-files`根本不会显示任何内容，因为索引文件与我的工作目录匹配。但是做一个 _git diff-index_ 会：

```
torvalds@ppc970:~/git> git diff-index --cached HEAD
-100644 blob    4161aecc6700a2eb579e842af0b7f22b98443f74        commit.c
+100644 blob    4161aecc6700a2eb579e842af0b7f22b98443f74        git-commit.c
```

你可以很容易地看到以上是重命名。

实际上，`git diff-index --cached` **应该**总是完全等同于实际执行 _git 写树 _ 并进行比较。除了这个，你只想检查你的位置的情况要好得多。

因此，当你问自己“我已经标记了什么以及与之前的树有什么区别”时，做一个`git diff-index --cached`基本上非常有用。

## 非缓存模式

“非缓存”模式采用不同的方法，并且可能更有用，因为它的作用不能用 _git write-tree_ + _git diff-模拟树 _。这就是默认模式。非缓存版本会询问以下问题：

```
show me the differences between HEAD and the currently checked out
tree - index contents _and_ files that aren't up to date
```

这显然是一个非常有用的问题，因为它告诉你**可以**提交什么。同样，输出将 _git diff-tree -r_ 输出与 tee 匹配，但有一个扭曲。

扭曲的是，如果某个文件与索引不匹配，我们就没有后备存储的东西，我们使用神奇的“全零”sha1 来表示。所以，假设你已经编辑了`kernel/sched.c`，但实际上还没有对它进行 _git update-index_ - 没有与新状态相关的“对象”，你得到：

```
torvalds@ppc970:~/v2.6/linux> git diff-index --abbrev HEAD
:100644 100664 7476bb... 000000...      kernel/sched.c
```

即，它表明树已经改变，`kernel/sched.c`不是最新的，可能包含新的东西。全零 sha1 意味着要获得真正的差异，您需要直接查看工作目录中的对象，而不是执行对象到对象的差异。

| 注意 | 与此类型的其他命令一样， _git diff-index_ 实际上根本不查看文件的内容。所以也许`kernel/sched.c`实际上并没有改变，只是你碰到了它。在任何一种情况下，都需要注意 _git update-index_ 才能使索引同步。 |

| 注意 | 您可以将文件混合显示为“已更新”和“在工作目录中仍然是脏的”。您总是可以告诉哪个文件处于哪个状态，因为“已更新”的文件显示有效的 sha1，而“与索引不同步”的文件将始终具有特殊的全零值 sha1。 |

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件