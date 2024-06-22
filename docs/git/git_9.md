# git-commit

> 原文： [`git-scm.com/docs/git-commit`](https://git-scm.com/docs/git-commit)

## 名称

git-commit - 记录对存储库的更改

## 概要

```
git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>] [--amend]
	   [--dry-run] [(-c | -C | --fixup | --squash) <commit>]
	   [-F <file> | -m <msg>] [--reset-author] [--allow-empty]
	   [--allow-empty-message] [--no-verify] [-e] [--author=<author>]
	   [--date=<date>] [--cleanup=<mode>] [--[no-]status]
	   [-i | -o] [-S[<keyid>]] [--] [<file>…​]
```

## 描述

创建一个新的提交，其中包含索引的当前内容和描述更改的给定日志消息。新提交是 HEAD 的直接子代，通常是当前分支的尖端，并且分支被更新为指向它（除非没有分支与工作树相关联，在这种情况下 HEAD 是“分离的”，如 [git-checkout [1]](https://git-scm.com/docs/git-checkout) ）。

要提交的内容可以通过以下几种方式指定：

1.  通过使用 [git-add [1]](https://git-scm.com/docs/git-add) 在使用 _commit_ 命令之前逐步“添加”对索引的更改（注意：甚至修改后的文件必须“添加”）;

2.  通过使用 [git-rm [1]](https://git-scm.com/docs/git-rm) 从工作树和索引中删除文件，再次使用 _commit_ 命令之前;

3.  通过将文件列为 _commit_ 命令的参数（没有--interactive 或--patch switch），在这种情况下，提交将忽略索引中暂存的更改，而是记录列出的文件的当前内容（必须已经为 Git 所知）;

4.  通过使用-a 开关和 _commit_ 命令自动“添加”来自所有已知文件的更改（即已在索引中列出的所有文件）并自动“rm”索引中的文件已从工作树中删除，然后执行实际提交;

5.  通过使用--interactive 或--patch 开关和 _commit_ 命令，在完成操作之前，逐个确定哪些文件或数据库应该是提交的一部分，以及索引中的内容。请参阅 [git-add [1]](https://git-scm.com/docs/git-add) 的“交互模式”部分，了解如何操作这些模式。

`--dry-run`选项可用于通过提供相同的参数集（选项和路径）来获取上述任何内容对下一次提交所包含内容的摘要。

如果你提交然后在那之后立即发现错误，你可以使用 _git reset_ 从中恢复。

## OPTIONS

```
 -a 
```

```
 --all 
```

告诉命令自动暂存已修改和删除的文件，但是没有告诉 Git 的新文件不会受到影响。

```
 -p 
```

```
 --patch 
```

使用交互式修补程序选择界面选择要提交的更改。有关详细信息，请参阅 [git-add [1]](https://git-scm.com/docs/git-add) 。

```
 -C <commit> 
```

```
 --reuse-message=<commit> 
```

获取现有提交对象，并在创建提交时重用日志消息和作者信息（包括时间戳）。

```
 -c <commit> 
```

```
 --reedit-message=<commit> 
```

与 _-C_ 类似，但是使用`-c`调用编辑器，以便用户可以进一步编辑提交消息。

```
 --fixup=<commit> 
```

构造一个与`rebase --autosquash`一起使用的提交消息。提交消息将是指定提交的主题行，前缀为“fixup！”。有关详细信息，请参阅 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 。

```
 --squash=<commit> 
```

构造一个与`rebase --autosquash`一起使用的提交消息。提交消息主题行取自指定的提交，前缀为“squash！”。可与其他提交消息选项一起使用（`-m` / `-c` / `-C` / `-F`）。有关详细信息，请参阅 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 。

```
 --reset-author 
```

当与-C / -c / - 修改选项一起使用时，或者在冲突的挑选之后提交时，声明生成的提交的作者现在属于提交者。这也更新了作者的时间戳。

```
 --short 
```

进行干运行时，请以短格式输出。有关详细信息，请参阅 [git-status [1]](https://git-scm.com/docs/git-status) 。意味着`--dry-run`。

```
 --branch 
```

即使是短格式显示分支和跟踪信息。

```
 --porcelain 
```

进行干运行时，请以瓷质格式输出。有关详细信息，请参阅 [git-status [1]](https://git-scm.com/docs/git-status) 。意味着`--dry-run`。

```
 --long 
```

进行干运行时，请以长格式输出。意味着`--dry-run`。

```
 -z 
```

```
 --null 
```

显示`short`或`porcelain`状态输出时，逐字打印文件名并使用 NUL 而不是 LF 终止输入。如果没有给出格式，则表示`--porcelain`输出格式。如果没有`-z`选项，则会按照配置变量`core.quotePath`的说明引用带有“异常”字符的文件名（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

```
 -F <file> 
```

```
 --file=<file> 
```

从给定文件中获取提交消息。使用 _-_ 从标准输入读取信息。

```
 --author=<author> 
```

覆盖提交作者。使用标准`A U Thor &lt;author@example.com&gt;`格式指定显式作者。否则&lt; author&gt;假定是一种模式，用于搜索该作者的现有提交（即 rev-list --all -i --author =&lt; author&gt;）;然后从找到的第一个这样的提交中复制提交作者。

```
 --date=<date> 
```

覆盖提交中使用的作者日期。

```
 -m <msg> 
```

```
 --message=<msg> 
```

使用给定的&lt; msg&gt;作为提交消息。如果给出了多个`-m`选项，则它们的值将作为单独的段落连接在一起。

`-m`选项与`-c`，`-C`和`-F`互斥。

```
 -t <file> 
```

```
 --template=<file> 
```

编辑提交消息时，使用给定文件中的内容启动编辑器。 `commit.template`配置变量通常用于向命令隐式提供此选项。希望引导参与者提供有关在消息中以什么顺序写入内容的一些提示的项目可以使用此机制。如果用户在不编辑消息的情况下退出编辑器，则中止提交。当通过其他方式给出消息时，例如，这没有效果。使用`-m`或`-F`选项。

```
 -s 
```

```
 --signoff 
```

在提交日志消息的末尾由提交者添加逐行签名。签收的含义取决于项目，但它通常证明提交者有权在同一许可下提交此作品并同意开发者原产地证书（参见 [`developercertificate.org/`](http://developercertificate.org/) ] 欲获得更多信息）。

```
 -n 
```

```
 --no-verify 
```

此选项绕过 pre-commit 和 commit-msg 挂钩。另见 [githooks [5]](https://git-scm.com/docs/githooks) 。

```
 --allow-empty 
```

通常记录与其唯一父提交具有完全相同的树的提交是错误的，并且该命令阻止您进行此类提交。此选项绕过安全性，主要供外部 SCM 接口脚本使用。

```
 --allow-empty-message 
```

与--allow-empty 类似，此命令主要供外部 SCM 接口脚本使用。它允许您使用空提交消息创建提交，而不使用 [git-commit-tree [1]](https://git-scm.com/docs/git-commit-tree) 等管道命令。

```
 --cleanup=<mode> 
```

此选项确定在提交之前应如何清除提供的提交消息。 _&lt; mode&gt;_ 可以是`strip`，`whitespace`，`verbatim`，`scissors`或`default`。

```
 strip 
```

剥去前导和尾随空行，尾随空格，注释和折叠连续的空行。

```
 whitespace 
```

与`strip`相同，但不删除#commentary。

```
 verbatim 
```

根本不要更改消息。

```
 scissors 
```

与`whitespace`相同，但是如果要编辑消息，则截断下面找到的行的所有内容（包括）都将被截断。 “`#`”可以使用 core.commentChar 进行自定义。

```
# ------------------------ >8 ------------------------
```

```
 default 
```

如果要编辑消息，则与`strip`相同。否则`whitespace`。

可以通过`commit.cleanup`配置变量更改默认值（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

```
 -e 
```

```
 --edit 
```

从带有`-F`的文件，带有`-m`的命令行和带有`-C`的提交对象获取的消息通常用作未修改的提交日志消息。此选项允许您进一步编辑从这些来源获取的消息。

```
 --no-edit 
```

使用选定的提交消息而不启动编辑器。例如，`git commit --amend --no-edit`修改提交而不更改其提交消息。

```
 --amend 
```

通过创建新提交替换当前分支的提示。记录的树像往常一样准备（包括`-i`和`-o`选项和显式路径规范的效果），当没有其他消息时，原始提交的消息用作起始点而不是空消息通过`-m`，`-F`，`-c`等选项从命令行指定。新提交与当前提交具有​​相同的父级和作者（`--reset-author`选项可以对此进行反对）。

这是一个粗略的等价物：

```
	$ git reset --soft HEAD^
	$ ... do something else to come up with the right tree ...
	$ git commit -c ORIG_HEAD
```

但可以用来修改合并提交。

如果修改已发布的提交，则应了解重写历史记录的含义。 （参见 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 中的“从上游重新恢复”部分。）

```
 --no-post-rewrite 
```

绕过重写后的钩子。

```
 -i 
```

```
 --include 
```

在到目前为止提交暂存内容之前，还要在命令行上分配路径的内容。这通常不是您想要的，除非您得出冲突的合并。

```
 -o 
```

```
 --only 
```

通过获取命令行上指定的路径的更新工作树内容进行提交，忽略已为其他路径暂存的任何内容。如果在命令行上给出了任何路径，这是 _git commit_ 的默认操作模式，在这种情况下，可以省略此选项。如果此选项与`--amend`一起指定，则不需要指定任何路径，这可以用于修改最后一次提交而不提交已经暂存的更改。如果与`--allow-empty`一起使用，则也不需要路径，并且将创建空提交。

```
 -u[<mode>] 
```

```
 --untracked-files[=<mode>] 
```

显示未跟踪的文件。

mode 参数是可选的（默认为 _all_ ），用于指定未跟踪文件的处理;当-u 未使用时，默认为 _ 正常 _，即显示未跟踪的文件和目录。

可能的选择是：

*   _ 没有 _ - 显示没有未跟踪的文件

*   _ 正常 _ - 显示未跟踪的文件和目录

*   _ 全部 _ - 还显示未跟踪目录中的单个文件。

可以使用 [git-config [1]](https://git-scm.com/docs/git-config) 中记录的 status.showUntrackedFiles 配置变量更改默认值。

```
 -v 
```

```
 --verbose 
```

显示 HEAD 提交与提交消息模板底部提交的内容之间的统一差异，以帮助用户通过提醒提交的更改来描述提交。请注意，此 diff 输出的行前缀不是 _＃_。此差异不会是提交消息的一部分。请参见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`commit.verbose`配置变量。

如果指定了两次，则另外显示将提交的内容与 worktree 文件之间的统一差异，即对跟踪文件的未分级更改。

```
 -q 
```

```
 --quiet 
```

禁止提交摘要消息。

```
 --dry-run 
```

不要创建提交，而是显示要提交的路径列表，具有未提交的本地更改的路径以及未跟踪的路径。

```
 --status 
```

使用编辑器准备提交消息时，在提交消息模板中包含 [git-status [1]](https://git-scm.com/docs/git-status) 的输出。默认为 on，但可用于覆盖配置变量 commit.status。

```
 --no-status 
```

使用编辑器准备默认提交消息时，请勿在提交消息模板中包含 [git-status [1]](https://git-scm.com/docs/git-status) 的输出。

```
 -S[<keyid>] 
```

```
 --gpg-sign[=<keyid>] 
```

GPG 签名提交。 `keyid`参数是可选的，默认为提交者标识;如果指定，它必须粘在没有空格的选项上。

```
 --no-gpg-sign 
```

Countermand `commit.gpgSign`配置变量，设置为强制每个提交都被签名。

```
 -- 
```

不要将任何更多的参数解释为选项。

```
 <file>…​ 
```

在命令行上提供文件时，该命令将提交指定文件的内容，而不记录已暂存的更改。这些文件的内容也会在之前的演出之上进行下一次提交。

## 日期格式

`GIT_AUTHOR_DATE`，`GIT_COMMITTER_DATE`环境变量和`--date`选项支持以下日期格式：

```
 Git internal format 
```

它是`&lt;unix timestamp&gt; &lt;time zone offset&gt;`，其中`&lt;unix timestamp&gt;`是自 UNIX 纪元以来的秒数。 `&lt;time zone offset&gt;`是 UTC 的正偏移或负偏移。例如，CET（比 UTC 早 1 小时）是`+0100`。

```
 RFC 2822 
```

RFC 2822 描述的标准电子邮件格式，例如`Thu, 07 Apr 2005 22:13:13 +0200`。

```
 ISO 8601 
```

ISO 8601 标准规定的时间和日期，例如`2005-04-07T22:13:13`。解析器也接受空格而不是`T`字符。

| 注意 | 此外，日期部分以下列格式接受：`YYYY.MM.DD`，`MM/DD/YYYY`和`DD.MM.YYYY`。 |

## 例子

录制自己的作品时，工作树中已修改文件的内容会暂时存储到名为“索引”的暂存区域，并带有 _git add_ 。一个文件只能在索引中但不能在工作树中恢复为使用`git reset HEAD -- &lt;file&gt;`的最后一次提交的文件，这会有效地恢复 _git add_ 并阻止对该文件的更改参与下一次提交。在使用这些命令逐步构建要提交的状态之后，`git commit`（没有任何路径名参数）用于记录到目前为止已经暂存的内容。这是命令的最基本形式。一个例子：

```
$ edit hello.c
$ git rm goodbye.c
$ git add hello.c
$ git commit
```

您可以告诉`git commit`注意在工作树中跟踪其内容的文件的更改，并为您执行相应的`git add`和`git rm`，而不是在每次更改后暂存文件。也就是说，如果您的工作树中没有其他更改，则此示例与前面的示例相同：

```
$ edit hello.c
$ rm goodbye.c
$ git commit -a
```

命令`git commit -a`首先查看您的工作树，注意您已修改了 hello.c 并删除了 goodbye.c，并为您执行必要的`git add`和`git rm`。

在对多个文件进行暂存更改后，您可以通过为`git commit`提供路径名来更改记录更改的顺序。给出路径名时，该命令进行提交，仅记录对命名路径所做的更改：

```
$ edit hello.c hello.h
$ git add hello.c hello.h
$ edit Makefile
$ git commit Makefile
```

这使得提交记录了对`Makefile`的修改。 `hello.c`和`hello.h`暂挂的更改不包含在生成的提交中。然而，他们的变化并没有丢失 - 他们仍然上演并且只是被阻止。完成上述步骤后，如果您这样做：

```
$ git commit
```

第二次提交将按预期记录对`hello.c`和`hello.h`的更改。

合并后（由 _git merge_ 或 _git pull_ 启动）因冲突而停止，已经暂停合并的路径为您提交，并且冲突的路径保持未合并状态。你必须首先检查哪些路径与 _git status_ 有冲突，并且在你的工作树中手动修复它们之后，你会像往常一样使用 _git add_ 来调整结果：

```
$ git status | grep unmerged
unmerged: hello.c
$ edit hello.c
$ git add hello.c
```

解决冲突并暂存结果后，`git ls-files -u`将停止提及冲突的路径。完成后，运行`git commit`以最终记录合并：

```
$ git commit
```

与记录您自己的更改的情况一样，您可以使用`-a`选项来保存输入。一个区别是，在合并解析期间，您不能将`git commit`与路径名一起使用来更改提交更改的顺序，因为合并应记录为单个提交。实际上，命令拒绝在给定路径名时运行（但请参阅`-i`选项）。

## 讨论

虽然不是必需的，但最好使用一个简短（小于 50 个字符）的行来概括更改，然后是空白行，然后是更详尽的描述。提交消息中第一个空白行的文本被视为提交标题，并且该标题在整个 Git 中使用。例如， [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 将提交转换为电子邮件，它使用主题行上的标题和正文中的其余提交。

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

## 环境和配置变量

用于编辑提交日志消息的编辑器将从`GIT_EDITOR`环境变量，core.editor 配置变量，`VISUAL`环境变量或`EDITOR`环境变量（按此顺序）中选择。有关详细信息，请参阅 [git-var [1]](https://git-scm.com/docs/git-var) 。

## 挂钩

该命令可以运行`commit-msg`，`prepare-commit-msg`，`pre-commit`，`post-commit`和`post-rewrite`挂钩。有关详细信息，请参阅 [githooks [5]](https://git-scm.com/docs/githooks) 。

## FILES

```
 $GIT_DIR/COMMIT_EDITMSG 
```

此文件包含正在进行的提交的提交消息。如果在创建提交之前由于错误而退出`git commit`，则用户提供的任何提交消息（例如，在编辑器会话中）将在此文件中可用，但将在下一次调用[COD1 时]覆盖]。

## 也可以看看

[git-add [1]](https://git-scm.com/docs/git-add) ， [git-rm [1]](https://git-scm.com/docs/git-rm) ， [git-mv [1]](https://git-scm.com/docs/git-mv) ， [git-merge [1]](https://git-scm.com/docs/git-merge) ， [git-commit-tree [1]](https://git-scm.com/docs/git-commit-tree)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件