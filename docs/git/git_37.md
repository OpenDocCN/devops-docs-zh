# git-blame

> 原文： [`git-scm.com/docs/git-blame`](https://git-scm.com/docs/git-blame)

## 名称

git-blame - 显示修改版本和作者上次修改文件的每一行

## 概要

```
git blame [-c] [-b] [-l] [--root] [-t] [-f] [-n] [-s] [-e] [-p] [-w] [--incremental]
	    [-L <range>] [-S <revs-file>] [-M] [-C] [-C] [-C] [--since=<date>]
	    [--progress] [--abbrev=<n>] [<rev> | --contents <file> | --reverse <rev>..<rev>]
	    [--] <file>
```

## 描述

使用最后修改该行的修订版中的信息注释给定文件中的每一行。 （可选）从给定修订开始注释。

当指定一次或多次时，`-L`将注释限制为所请求的行。

在整个文件重命名中自动跟踪行的原点（目前没有选项可以关闭重命名 - 关闭）。要跟踪从一个文件移动到另一个文件的行，或跟踪从另一个文件复制和粘贴的行等，请参阅`-C`和`-M`选项。

该报告没有告诉您有关已删除或替换的行的任何信息;您需要使用 _git diff_ 等工具或以下段落中简要提到的“pickaxe”界面。

除了支持文件注释之外，Git 还支持在更改中发生代码片段时搜索开发历史记录。这使得可以跟踪何时将代码片段添加到文件，在文件之间移动或复制，最终删除或替换。它的工作原理是在 diff 中搜索文本字符串。搜索`blame_usage`的 pickaxe 接口的一个小例子：

```
$ git log --pretty=oneline -S'blame_usage'
5040f17eba15504bad66b14a645bddd9b015ebb7 blame -S <ancestry-file>
ea4c7f9bf69e781dd0cd88d2bccb2bf5cc15c9a7 git-blame: Make the output
```

## OPTIONS

```
 -b 
```

为边界提交显示空白 SHA-1。这也可以通过`blame.blankboundary`配置选项进行控制。

```
 --root 
```

不要将 root 提交视为边界。这也可以通过`blame.showRoot`配置选项进行控制。

```
 --show-stats 
```

在非责任输出结束时包括其他统计数据。

```
 -L <start>,<end> 
```

```
 -L :<funcname> 
```

仅注释给定的行范围。可以多次指定。允许重叠范围。

&lt;开始&gt;和&lt; end&gt;是可选的。 “-L&lt; start&gt;”或“-L&lt; start&gt;”跨越&lt; start&gt;到文件结束。 “-L，&lt; end&gt;”从文件开头跨越到&lt; end&gt;。

&lt;开始&gt;和&lt; end&gt;可以采取以下形式之一：

*   数

    如果&lt; start&gt;或者&lt; end&gt;是一个数字，它指定一个绝对行号（行数从 1 开始）。

*   /正则表达式/

    此表单将使用与给定 POSIX 正则表达式匹配的第一行。如果&lt; start&gt;是一个正则表达式，它将从前一个`-L`范围的末尾搜索，如果有的话，否则从文件的开头搜索。如果&lt; start&gt;是“^ / regex /”，它将从文件的开头搜索。如果&lt; end&gt;是一个正则表达式，它将从&lt; start&gt;给出的行开始搜索。

*   + offset 或-offset

    这仅适用于&lt; end&gt;并将在&lt; start&gt;给出的行之前或之后指定行数。

如果给出“：&lt; funcname&gt;”代替&lt; start&gt;和&lt; end&gt;，它是一个正则表达式，表示从匹配&lt; funcname&gt;的第一个 funcname 行到下一个 funcname 行的范围。 “：&lt; funcname&gt;”从上一个`-L`范围的末尾搜索（如果有的话），否则从文件的开头搜索。 “^：&lt; funcname&gt;”从文件的开头搜索。

```
 -l 
```

显示长转速（默认值：关闭）。

```
 -t 
```

显示原始时间戳（默认值：关闭）。

```
 -S <revs-file> 
```

使用 revs-file 的修订而不是调用 [git-rev-list [1]](https://git-scm.com/docs/git-rev-list) 。

```
 --reverse <rev>..<rev> 
```

走向历史而不是落后。这不显示出现一行的修订，而是显示一行存在的最后修订版。这需要一系列的修订，如 START..END，其中指责路径存在于 START 中。为方便起见，`git blame --reverse START`被视为`git blame --reverse START..HEAD`。

```
 -p 
```

```
 --porcelain 
```

以专为机器消耗而设计的格式显示。

```
 --line-porcelain 
```

显示瓷器格式，但输出每行的提交信息，而不仅仅是第一次引用提交。意思是 - 瓷器。

```
 --incremental 
```

以设计用于机器消耗的格式逐步显示结果。

```
 --encoding=<encoding> 
```

指定用于输出作者姓名和提交摘要的编码。将其设置为`none`会使非责任输出非转换数据。有关更多信息，请参阅 [git-log [1]](https://git-scm.com/docs/git-log) 手册页中有关编码的讨论。

```
 --contents <file> 
```

当&lt; rev&gt;如果未指定，该命令将从工作树副本开始向后注释更改。此标志使命令假装工作树副本具有指定文件的内容（指定`-`以使命令从标准输入读取）。

```
 --date <format> 
```

指定用于输出日期的格式。如果未提供--date，则使用 blame.date 配置变量的值。如果未设置 blame.date 配置变量，则使用 iso 格式。有关支持的值，请参阅 [git-log [1]](https://git-scm.com/docs/git-log) 中--date 选项的讨论。

```
 --[no-]progress 
```

默认情况下，当连接到终端时，会在标准错误流上报告进度状态。即使没有附加到终端，该标志也可以进行进度报告。不能将`--progress`与`--porcelain`或`--incremental`一起使用。

```
 -M[<num>] 
```

检测文件中移动或复制的行。当提交移动或复制一行行（例如原始文件有 A 然后是 B，并且提交将其更改为 B 然后 A）时，传统的 _ 指责 _ 算法仅注意到一半的移动和通常会将向上移动（即 B）的行归咎于父级，并将责任归咎于向下移动（即 A）到子提交的行。使用此选项，两组行都通过运行额外的检查通道归咎于父组。

&lt; NUM&gt;是可选的，但它是字母数字字符数的下限，Git 必须在文件中检测为移动/复制，以便将这些行与父提交相关联。默认值为 20。

```
 -C[<num>] 
```

除`-M`外，检测从同一提交中修改的其他文件移动或复制的行。当您重新组织程序并跨文件移动代码时，这非常有用。当此选项被给出两次时，该命令还会在创建文件的提交中查找其他文件的副本。当此选项被给出三次时，该命令还会在任何提交中查找来自其他文件的副本。

&lt; NUM&gt;是可选的，但它是字母数字字符数的下限，Git 必须检测它们在文件之间移动/复制，以便将这些行与父提交相关联。并且默认值为 40.如果给出多个`-C`选项，则&lt; num&gt;最后一个`-C`的参数将生效。

```
 -h 
```

显示帮助信息。

```
 -c 
```

使用与 [git-annotate [1]](https://git-scm.com/docs/git-annotate) 相同的输出模式（默认值：关闭）。

```
 --score-debug 
```

包括与文件之间的行移动相关的调试信息（参见`-C`）和文件中移动的行（参见`-M`）。列出的第一个数字是分数。这是检测为在文件之间或文件内移动的字母数字字符数。这必须高于 _git blame_ 的某个阈值才能考虑那些代码行被移动。

```
 -f 
```

```
 --show-name 
```

在原始提交中显示文件名。默认情况下，如果由于重命名检测而存在来自具有不同名称的文件的任何行，则会显示文件名。

```
 -n 
```

```
 --show-number 
```

显示原始提交中的行号（默认值：关闭）。

```
 -s 
```

从输出中抑制作者姓名和时间戳。

```
 -e 
```

```
 --show-email 
```

显示作者电子邮件而不是作者姓名（默认值：关闭）。这也可以通过`blame.showEmail`配置选项进行控制。

```
 -w 
```

在比较父版本和子版本时忽略空格以查找行的来源。

```
 --abbrev=<n> 
```

不使用默认的 7 + 1 十六进制数字作为缩写对象名称，而是使用&lt; n&gt; +1 位数。请注意，1 列用于标记边界提交的插入符号。

## 瓷器格式

在这种格式中，每一行都在标题之后输出;最小的标题有第一行有：

*   该行所属的提交的 40 字节 SHA-1;

*   原始文件中行的行号;

*   最终文件中行的行号;

*   在一行中，该行从与前一个提交不同的提交开始一组行，即该组中的行数。在后续行中，该字段不存在。

对于每个提交，此标题行后面至少跟随以下信息一次：

*   作者姓名（“作者”），电子邮件（“作者邮件”），时间（“作者时间”）和时区（“author-tz”）;类似的提交者。

*   提交行中的文件名。

*   提交日志消息的第一行（“摘要”）。

在上面的标题之后输出实际行的内容，以 TAB 为前缀。这是为了允许稍后添加更多标题元素。

瓷器格式通常会抑制已经看到的提交信息。例如，将显示归咎于同一提交的两行，但该提交的详细信息将仅显示一次。这样更有效，但可能需要读者保留更多状态。 `--line-porcelain`选项可用于输出每行的完整提交信息，从而允许更简单（但效率更低）的用法，例如：

```
# count the number of lines attributed to each author
git blame --line-porcelain file |
sed -n 's/^author //p' |
sort | uniq -c | sort -rn
```

## 指定范围

与旧版本的 git 中的 _git blame_ 和 _git 注释 _ 不同，注释的范围可以限制为行范围和修订范围。可以多次指定`-L`选项，该选项将注释限制为一系列线。

当你有兴趣找到文件`foo`的第 40-60 行的原点时，你可以像这样使用`-L`选项（它们的意思相同 - 从第 40 行开始要求 21 行）：

```
git blame -L 40,60 foo
git blame -L 40,+21 foo
```

您还可以使用正则表达式指定行范围：

```
git blame -L '/^sub hello {/,/^}$/' foo
```

它将注释限制在`hello`子程序的主体上。

如果您对版本低于 v2.6.18 的更改或对 3 周以上的更改不感兴趣，则可以使用与 _git rev-list_ 类似的修订版本说明符：

```
git blame v2.6.18.. -- foo
git blame --since=3.weeks -- foo
```

当修订范围说明符用于限制注释时，自范围边界以来没有更改的行（在上面的示例中，提交 v2.6.18 或超过 3 周的最近提交）被归咎于该范围边界承诺。

一种特别有用的方法是查看添加的文件是否具有通过现有文件的复制和粘贴创建的行。有时这表明开发人员很草率，并没有正确地重构代码。您可以先找到引入该文件的提交：

```
git log --diff-filter=A --pretty=short -- foo
```

然后使用`commit^!`表示法注释提交及其父项之间的更改：

```
git blame -C -C -f $commit^! -- foo
```

## 增量输出

使用`--incremental`选项调用时，该命令会在构建时输出结果。输出通常将首先谈论最近提交所触及的行（即，行将不按顺序注释）并且意图由交互式观看者使用。

输出格式类似于 Porcelain 格式，但它不包含正在注释的文件中的实际行。

1.  每个责备条目始终以以下行开头：

    ```
    &lt;40-byte hex sha1&gt; &lt;sourceline&gt; &lt;resultline&gt; &lt;num_lines&gt;
    ```

    行号从 1 开始计算。

2.  第一次提交显示在流中时，它有各种其他有关它的信息，在每行的开头打印出一个单词标记，描述额外的提交信息（作者，电子邮件，提交者，日期，摘要等） ）。

3.  与 Porcelain 格式不同，始终给出文件名信息并终止条目：

    ```
    "filename" &lt;whitespace-quoted-filename-goes-here&gt;
    ```

    因此，解析一些面向字和字的解析器（对于大多数脚本语言来说应该很自然）非常容易。

    | 注意 | 对于进行解析的人：为了使其更加健壮，只需忽略第一个和最后一个（“&lt; sha1&gt;”和“filename”行）之间的任何行，在这些行中您无法识别标记词（或关注那个特定的词） ）在“扩展信息”行的开头。这样，如果有添加的信息（如提交编码或扩展提交注释），责备查看器将无关紧要。 |

## 映射作者

如果文件`.mailmap`存在于存储库的顶层，或者位于 mailmap.file 或 mailmap.blob 配置选项所指向的位置，则它用于将作者和提交者名称以及电子邮件地址映射到规范的真实姓名和电子邮件地址。

在简单形式中，文件中的每一行都包含作者的规范实名，空格和提交中使用的电子邮件地址（由 _&lt;_ 和 _&gt;_ 括起来）映射到名称。例如：

```
Proper Name <commit@email.xx>
```

更复杂的形式是：

```
<proper@email.xx> <commit@email.xx>
```

允许 mailmap 仅替换提交的电子邮件部分，并且：

```
Proper Name <proper@email.xx> <commit@email.xx>
```

它允许 mailmap 替换与指定的提交电子邮件地址匹配的提交的名称和电子邮件，并且：

```
Proper Name <proper@email.xx> Commit Name <commit@email.xx>
```

它允许 mailmap 替换与指定的提交名称和电子邮件地址匹配的提交的名称和电子邮件。

示例 1：您的历史记录包含两位作者 Jane 和 Joe 的提交，其名称以多种形式出现在存储库中：

```
Joe Developer <joe@example.com>
Joe R. Developer <joe@example.com>
Jane Doe <jane@example.com>
Jane Doe <jane@laptop.(none)>
Jane D. <jane@desktop.(none)>
```

现在假设 Joe 希望他的中间名最初使用，而 Jane 更喜欢她的姓氏完全拼写出来。一个合适的`.mailmap`文件看起来像：

```
Jane Doe         <jane@desktop.(none)>
Joe R. Developer <joe@example.com>
```

注意如何不需要`&lt;jane@laptop.(none)&gt;`的条目，因为该作者的真实姓名已经正确。

示例 2：您的存储库包含以下作者的提交：

```
nick1 <bugs@company.xx>
nick2 <bugs@company.xx>
nick2 <nick2@company.xx>
santa <me@company.xx>
claus <me@company.xx>
CTO <cto@coompany.xx>
```

然后你可能想要一个看起来像这样的`.mailmap`文件：

```
<cto@company.xx>                       <cto@coompany.xx>
Some Dude <some@dude.xx>         nick1 <bugs@company.xx>
Other Author <other@author.xx>   nick2 <bugs@company.xx>
Other Author <other@author.xx>         <nick2@company.xx>
Santa Claus <santa.claus@northpole.xx> <me@company.xx>
```

将哈希 _＃_ 用于自己的行或电子邮件地址之后的注释。

## 也可以看看

[git-annotate [1]](https://git-scm.com/docs/git-annotate)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件