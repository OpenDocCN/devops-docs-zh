# git-rebase

> 原文： [`git-scm.com/docs/git-rebase`](https://git-scm.com/docs/git-rebase)

## 名称

git-rebase - 重新应用提交在另一个基本提示之上

## 概要

```
git rebase [-i | --interactive] [<options>] [--exec <cmd>] [--onto <newbase>]
	[<upstream> [<branch>]]
git rebase [-i | --interactive] [<options>] [--exec <cmd>] [--onto <newbase>]
	--root [<branch>]
git rebase --continue | --skip | --abort | --quit | --edit-todo | --show-current-patch
```

## 描述

如果&lt; branch&gt;如果指定， _git rebase_ 将在执行任何其他操作之前执行自动`git checkout &lt;branch&gt;`。否则它仍然在当前分支上。

如果&lt; upstream&gt;未指定，上游在分支中配置。&lt; name&gt; .remote 和 branch。将使用&lt; name&gt; .merge 选项（详见 [git-config [1]](https://git-scm.com/docs/git-config) ）和`--fork-point`假设选项。如果您当前不在任何分支上，或者当前分支没有配置上游，则 rebase 将中止。

由当前分支中的提交进行的所有更改，但不在&lt; upstream&gt;中。被保存到临时区域。这是`git log &lt;upstream&gt;..HEAD`显示的同一组提交;或`git log 'fork_point'..HEAD`，如果`--fork-point`有效（参见下面`--fork-point`的说明）;如果指定了`--root`选项，则按`git log HEAD`。

当前分支重置为&lt; upstream&gt;或&lt; newbase&gt;如果提供了--onto 选项。这与`git reset --hard &lt;upstream&gt;`（或&lt; newbase&gt;）具有完全相同的效果。 ORIG_HEAD 设置为在重置之前指向分支的尖端。

之前保存到临时区域的提交将按顺序逐个重新应用于当前分支。请注意，HEAD 中的任何提交都会引入与 HEAD 中的提交相同的文本更改。&lt; upstream&gt;被省略（即，将跳过已经在上游接受的具有不同提交消息或时间戳的补丁）。

合并失败可能会阻止此过程完全自动化。您必须解决任何此类合并失败并运行`git rebase --continue`。另一种选择是绕过导致合并失败的提交`git rebase --skip`。要查看原始&lt;分支&gt;并删除.git / rebase-apply 工作文件，改为使用命令`git rebase --abort`。

假设存在以下历史记录，并且当前分支是“主题”：

```
          A---B---C topic
         /
    D---E---F---G master
```

从这一点来看，以下任一命令的结果：

```
git rebase master
git rebase master topic
```

将会：

```
                  A'--B'--C' topic
                 /
    D---E---F---G master
```

**注意：**后一种形式只是`git checkout topic`的简写，后跟`git rebase master`。当 rebase 退出`topic`时，将保留签出分支。

如果上游分支已经包含您所做的更改（例如，因为您邮寄了上游应用的补丁），那么将跳过该提交。例如，在以下历史记录中运行`git rebase master`（其中`A'`和`A`引入相同的更改集，但具有不同的提交者信息）：

```
          A---B---C topic
         /
    D---E---A'---F master
```

将导致：

```
                   B'---C' topic
                  /
    D---E---A'---F master
```

以下是如何将基于一个分支的主题分支移植到另一个分支，假设您使用`rebase --onto`从后一分支分叉主题分支。

首先让我们假设您的 _ 主题 _ 基于分支 _ 下一个 _。例如，_ 主题 _ 中开发的功能取决于 _ 下一个 _ 中的某些功能。

```
    o---o---o---o---o  master
         \
          o---o---o---o---o  next
                           \
                            o---o---o  topic
```

我们想从分支 _master_ 分叉 _ 主题 _;例如，因为 _ 主题 _ 所依赖的功能被合并到更稳定的 _ 主 _ 分支中。我们希望我们的树看起来像这样：

```
    o---o---o---o---o  master
        |            \
        |             o'--o'--o'  topic
         \
          o---o---o---o---o  next
```

我们可以使用以下命令获取此信息：

```
git rebase --onto master next topic
```

--onto 选项的另一个例子是重新定义分支的一部分。如果我们有以下情况：

```
                            H---I---J topicB
                           /
                  E---F---G  topicA
                 /
    A---B---C---D  master
```

那么命令

```
git rebase --onto master topicA topicB
```

会导致：

```
                 H'--I'--J'  topicB
                /
                | E---F---G  topicA
                |/
    A---B---C---D  master
```

当 topicB 不依赖于 topicA 时，这很有用。

也可以使用 rebase 删除一系列提交。如果我们有以下情况：

```
    E---F---G---H---I---J  topicA
```

那么命令

```
git rebase --onto topicA~5 topicA~3 topicA
```

会导致删除提交 F 和 G：

```
    E---H'---I'---J'  topicA
```

如果 F 和 G 在某种程度上存在缺陷，或者不应该成为 topicA 的一部分，那么这很有用。注意 - -onto 和&lt; upstream&gt;的参数。参数可以是任何有效的 commit-ish。

如果发生冲突， _git rebase_ 将在第一个有问题的提交时停止，并在树中留下冲突标记。您可以使用 _git diff_ 来定位标记（&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;对于您编辑的每个文件，您需要告诉 Git 冲突已经解决，通常可以这样做

```
git add <filename>
```

手动解决冲突并使用所需的分辨率更新索引后，您可以继续使用

```
git rebase --continue
```

或者，您可以撤消 _git rebase_

```
git rebase --abort
```

## 组态

```
 rebase.useBuiltin 
```

设置为`false`以使用 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 的旧版 shellcript 实现。默认情况下是`true`，这意味着在 C 中使用内置的重写。

C 重写首先包含在 Git 版本 2.20 中。如果在重写中发现任何错误，此选项可用于重新启用旧版本。此选项和 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 的 shellscript 版本将在以后的某个版本中删除。

如果您发现某些理由将此选项设置为`false`而非一次性测试，则应将行为差异报告为 git 中的错误。

```
 rebase.stat 
```

是否显示自上次 rebase 以来上游改变的差异。默认为 False。

```
 rebase.autoSquash 
```

如果设置为 true，则默认启用`--autosquash`选项。

```
 rebase.autoStash 
```

设置为 true 时，在操作开始之前自动创建临时存储条目，并在操作结束后应用它。这意味着您可以在脏工作树上运行 rebase。但是，谨慎使用：成功重组后的最终存储应用程序可能会导致非平凡的冲突。 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 的`--no-autostash`和`--autostash`选项可以覆盖此选项。默认为 false。

```
 rebase.missingCommitsCheck 
```

如果设置为“warn”，git rebase -i 将在删除某些提交时打印警告（例如删除了一行），但是 rebase 仍将继续。如果设置为“error”，它将打印上一个警告并停止 rebase，然后可以使用 _git rebase --edit-todo_ 来纠正错误。如果设置为“忽略”，则不进行检查。要在没有警告或错误的情况下删除提交，请使用待办事项列表中的`drop`命令。默认为“忽略”。

```
 rebase.instructionFormat 
```

[git-log [1]](https://git-scm.com/docs/git-log) 中指定的格式字符串，用于交互式 rebase 期间的待办事项列表。格式将自动在格式之前添加长提交哈希。

```
 rebase.abbreviateCommands 
```

如果设置为 true，`git rebase`将在待办事项列表中使用缩写的命令名称，结果如下：

```
	p deadbee The oneline of the commit
	p fa1afe1 The oneline of the next commit
	...
```

代替：

```
	pick deadbee The oneline of the commit
	pick fa1afe1 The oneline of the next commit
	...
```

默认为 false。

```
 rebase.rescheduleFailedExec 
```

自动重新安排失败的`exec`命令。这仅在交互模式下（或提供`--exec`选项时）才有意义。这与指定`--reschedule-failed-exec`选项相同。

## OPTIONS

```
 --onto <newbase> 
```

创建新提交的起点。如果未指定--onto 选项，则起点为&lt; upstream&gt;。可以是任何有效的提交，而不仅仅是现有的分支名称。

作为特殊情况，如果只有一个合并库，则可以使用“A ... B”作为 A 和 B 的合并基础的快捷方式。您最多可以省略 A 和 B 中的一个，在这种情况下，它默认为 HEAD。

```
 <upstream> 
```

上游分支进行比较。可以是任何有效的提交，而不仅仅是现有的分支名称。默认为当前分支的已配置上游。

```
 <branch> 
```

工作分支;默认为 HEAD。

```
 --continue 
```

解决合并冲突后重新启动重定位过程。

```
 --abort 
```

中止 rebase 操作并将 HEAD 重置为原始分支。如果&lt; branch&gt;在 rebase 操作开始时提供，然后 HEAD 将重置为&lt; branch&gt;。否则，HEAD 将重置为启动 rebase 操作时的位置。

```
 --quit 
```

中止 rebase 操作但 HEAD 不会重置回原始分支。结果，索引和工作树也保持不变。

```
 --keep-empty 
```

在结果中保留不改变其父项的任何提交。

另见下面的不兼容的选项。

```
 --allow-empty-message 
```

默认情况下，使用空消息进行的 rebasing 提交将失败。此选项会覆盖该行为，允许对具有空消息的提交进行重新定位。

另见下面的不兼容的选项。

```
 --skip 
```

跳过当前修补程序重新启动重定位过程。

```
 --edit-todo 
```

在交互式 rebase 期间编辑待办事项列表。

```
 --show-current-patch 
```

在交互式 rebase 中显示当前补丁，或者由于冲突而停止 rebase。这相当于`git show REBASE_HEAD`。

```
 -m 
```

```
 --merge 
```

使用合并策略进行 rebase。当使用递归（默认）合并策略时，这允许 rebase 知道上游侧的重命名。

请注意，通过从&lt; upstream&gt;顶部的工作分支重放每个提交来进行 rebase 合并。科。因此，当合并冲突发生时，报告为 _ 我们的 _ 的那一方是迄今为止重新命名的系列，从&lt; upstream&gt;开始，而 _ 他们的 _ 是工作分支。换句话说，双方交换。

另见下面的不兼容的选项。

```
 -s <strategy> 
```

```
 --strategy=<strategy> 
```

使用给定的合并策略。如果没有`-s`选项 _，则使用 git merge-recursive_ 。这意味着--merge。

因为 _git rebase_ 重放来自&lt; upstream&gt;顶部的工作分支的每个提交。使用给定策略的分支，使用 _ 我们的 _ 策略简单地清空&lt; branch&gt;中的所有补丁，这没有多大意义。

另见下面的不兼容的选项。

```
 -X <strategy-option> 
```

```
 --strategy-option=<strategy-option> 
```

通过&lt; strategy-option&gt;通过合并策略。这意味着`--merge`，如果没有指定策略，则`-s recursive`。注意 _ 我们的 _ 和的逆转，如上所述`-m`选项。

另见下面的不兼容的选项。

```
 -S[<keyid>] 
```

```
 --gpg-sign[=<keyid>] 
```

GPG 签名提交。 `keyid`参数是可选的，默认为提交者标识;如果指定，它必须粘在没有空格的选项上。

```
 -q 
```

```
 --quiet 
```

安静。意味着--no-stat。

```
 -v 
```

```
 --verbose 
```

要冗长。意味着 - 停止。

```
 --stat 
```

显示自上次 rebase 以来上游更改的差异。 diffstat 也由配置选项 rebase.stat 控制。

```
 -n 
```

```
 --no-stat 
```

不要将 diffstat 显示为 rebase 过程的一部分。

```
 --no-verify 
```

此选项绕过 pre-rebase 挂钩。另见 [githooks [5]](https://git-scm.com/docs/githooks) 。

```
 --verify 
```

允许运行 pre-rebase 挂钩，这是默认值。此选项可用于覆盖--no-verify。另见 [githooks [5]](https://git-scm.com/docs/githooks) 。

```
 -C<n> 
```

确保至少&lt; n&gt;周围环境的线在每次更改之前和之后匹配。当存在较少的周围环境线时，它们都必须匹配。默认情况下，不会忽略任何上下文。

另见下面的不兼容的选项。

```
 --no-ff 
```

```
 --force-rebase 
```

```
 -f 
```

单独重放所有重新提交的提交，而不是快速转发未更改的提交。这可以确保重新分支的整个历史记录由新提交组成。

在恢复主题分支合并之后，您可能会发现这很有用，因为此选项使用新提交重新创建主题分支，因此可以成功重新合并而无需“恢复恢复”（请参阅​​ revert-a-faulty-merge 如何 - 详情请）。

```
 --fork-point 
```

```
 --no-fork-point 
```

使用 reflog 在&lt; upstream&gt;之间找到更好的共同祖先。和&lt; branch&gt;在计算由&lt; branch&gt;引入的提交时。

当--fork-point 激活时，将使用 _fork_point_ 代替&lt; upstream&gt;计算 rebase 的提交集，其中 _fork_point_ 是`git merge-base --fork-point &lt;upstream&gt; &lt;branch&gt;`命令的结果（参见 [git-merge-base [1]](https://git-scm.com/docs/git-merge-base) ）。如果 _fork_point_ 最终为空，则&lt; upstream&gt;将被用作后备。

如果是&lt; upstream&gt;或--root 在命令行中给出，则默认为`--no-fork-point`，否则默认为`--fork-point`。

```
 --ignore-whitespace 
```

```
 --whitespace=<option> 
```

这些标志被传递给应用补丁的 _git apply_ 程序（参见 [git-apply [1]](https://git-scm.com/docs/git-apply) ）。

另见下面的不兼容的选项。

```
 --committer-date-is-author-date 
```

```
 --ignore-date 
```

这些标志传递给 _git am_ 以轻松更改重新提交的提交日期（参见 [git-am [1]](https://git-scm.com/docs/git-am) ）。

另见下面的不兼容的选项。

```
 --signoff 
```

添加 Signed-off-by：预告片到所有重新提交的提交。请注意，如果给出了`--interactive`，那么只有标记为要挑选，编辑或重新编号的提交才会添加预告片。

另见下面的不兼容的选项。

```
 -i 
```

```
 --interactive 
```

列出即将重新定位的提交。让用户在变基之前编辑该列表。此模式也可用于拆分提交（请参阅下面的 SPLITTING COMMITS）。

可以通过设置配置选项 rebase.instructionFormat 来更改提交列表格式。自定义指令格式将自动将长提交哈希添加到格式之前。

另见下面的不兼容的选项。

```
 -r 
```

```
 --rebase-merges[=(rebase-cousins|no-rebase-cousins)] 
```

默认情况下，rebase 将简单地从 todo 列表中删除合并提交，并将重新提交的提交放入单个线性分支中。使用`--rebase-merges`，rebase 将通过重新创建合并提交来尝试保留要重新提交的提交中的分支结构。必须手动解决/重新应用这些合并提交中的任何已解决的合并冲突或手动修改。

默认情况下，或者当指定`no-rebase-cousins`时，没有`&lt;upstream&gt;`作为直接祖先的提交将保留其原始分支点，即 git 1 的`--ancestry-path`选项将被排除的提交默认情况下会保留原始血统。如果打开`rebase-cousins`模式，则此类提交将改为`&lt;upstream&gt;`（或`&lt;onto&gt;`，如果指定）。

`--rebase-merges`模式在精神上与`--preserve-merges`类似，但与此选项相反，在交互式 rebase 中效果很好：可以随意重新排序，插入和删除提交。

目前只能使用`recursive`合并策略重新创建合并提交;只能通过显式`exec git merge -s &lt;strategy&gt; [...]`命令使用不同的合并策略。

另请参见下面的“重新合并和不兼容的选项”。

```
 -p 
```

```
 --preserve-merges 
```

通过重放合并提交引入的提交来重新创建合并提交，而不是展平历史记录。不保留合并冲突解决方案或手动修改合并提交。

这在内部使用`--interactive`机器，但明确地将它与`--interactive`选项结合使用通常不是一个好主意，除非你知道你在做什么（参见下面的 BUGS）。

另见下面的不兼容的选项。

```
 -x <cmd> 
```

```
 --exec <cmd> 
```

附加“exec&lt; cmd&gt;”在每行创建最终历史记录中的提交之后。 &lt; CMD&gt;将被解释为一个或多个 shell 命令。任何失败的命令都会中断 rebase，退出代码为 1。

您可以通过使用`--exec`的一个实例和几个命令来执行多个命令：

```
git rebase -i --exec "cmd1 && cmd2 && ..."
```

或者通过提供多个`--exec`：

```
git rebase -i --exec "cmd1" --exec "cmd2" --exec ...
```

如果使用`--autosquash`，则不会为中间提交附加“exec”行，并且只会出现在每个 squash / fixup 系列的末尾。

这在内部使用`--interactive`机器，但可以在没有显式`--interactive`的情况下运行。

另见下面的不兼容的选项。

```
 --root 
```

重新引用从&lt; branch&gt;可到达的所有提交，而不是用&lt; upstream&gt;来限制它们。这允许您在树枝上重新定义根提交。与--onto 一起使用时，它将跳过&lt; newbase&gt;中已包含的更改（而不是&lt; upstream&gt;）而没有--onto 它将在每次变化时运行。当与--onto 和--preserve-merges 一起使用时，_ 所有 _ 根提交将被重写为&lt; newbase&gt;作为父母代替。

另见下面的不兼容的选项。

```
 --autosquash 
```

```
 --no-autosquash 
```

当提交日志消息以“squash！...”（或“fixup！...”）开头，并且 todo 列表中已经存在与相同`...`匹配的提交时，自动修改 rebase -i 的待办事项列表因此，标记为压缩的提交在修改提交之后立即生效，并将移动的提交的操作从`pick`更改为`squash`（或`fixup`）。如果提交主题匹配，或者`...`引用提交的哈希，则提交与`...`匹配。作为后备，提交主题的部分匹配也起作用。创建 fixup / squash 提交的推荐方法是使用 [git-commit [1]](https://git-scm.com/docs/git-commit) 的`--fixup` / `--squash`选项。

如果使用配置变量`rebase.autoSquash`默认启用`--autosquash`选项，则此选项可用于覆盖和禁用此设置。

另见下面的不兼容的选项。

```
 --autostash 
```

```
 --no-autostash 
```

在操作开始之前自动创建临时存储条目，并在操作结束后应用它。这意味着您可以在脏工作树上运行 rebase。但是，谨慎使用：成功重组后的最终存储应用程序可能会导致非平凡的冲突。

```
 --reschedule-failed-exec 
```

```
 --no-reschedule-failed-exec 
```

自动重新安排失败的`exec`命令。这仅在交互模式下（或提供`--exec`选项时）才有意义。

## 不兼容的选择

以下选项：

*   --committer-日期是执笔者最新

*   - 忽略日期

*   --whitespace

*   - 忽略空白

*   -C

与以下选项不兼容：

*   - 合并

*   - 战略

*   --strategy 选项

*   --allow 空消息

*   - [无糖] autosquash

*   --rebase，合并

*   --preserve-合并

*   - 互动

*   --exec

*   --keep 空

*   --edit-待办事项

*   - 与--onto 组合使用时

此外，以下两对选项不兼容：

*   --preserve-merges 和--interactive

*   --preserve-merges 和--signoff

*   --preserve-merges 和--rebase-merges

*   --rebase-merges 和--strategy

*   --rebase-merges 和--strategy-option

## 行为差异

后端的行为有一些微妙的差异。

### 空提交

无论提交是否为空（没有相对于其父开始的更改）或结束为空（所有更改已在其他提交中上游应用），am 后端将丢弃任何“空”提交。

默认情况下，交互式后端会丢弃提交，该提交开始为空，如果命中达到空的提交，则会暂停。交互式后端存在`--keep-empty`选项，允许它保持空的提交。

### 目录重命名检测

在合并和交互式后端中启用了目录重命名启发式扫描。由于缺少准确的树信息，在后端禁用目录重命名检测。

## 合并战略

合并机制（`git merge`和`git pull`命令）允许使用`-s`选项选择后端 _ 合并策略 _。一些策略也可以采用自己的选项，可以通过向`git merge`和/或`git pull`提供`-X&lt;option&gt;`参数来传递。

```
 resolve 
```

这只能使用 3 向合并算法解析两个头（即当前分支和您从中拉出的另一个分支）。它试图仔细检测纵横交错的合并模糊，并且通常被认为是安全和快速的。

```
 recursive 
```

这只能使用 3 向合并算法解析两个磁头。当有多个可用于 3 向合并的共同祖先时，它会创建共同祖先的合并树，并将其用作 3 向合并的参考树。据报道，这会导致更少的合并冲突，而不会因为从 Linux 2.6 内核开发历史记录中进行的实际合并提交所做的测试而导致错误。此外，这可以检测和处理涉及重命名的合并，但目前无法使用检测到的副本。这是拉动或合并一个分支时的默认合并策略。

_ 递归 _ 策略可以采用以下选项：

```
 ours 
```

这个选项通过支持 _ 我们的 _ 版本来强制冲突的帅哥干净地自动解决。来自与我们方不冲突的其他树的更改将反映到合并结果中。对于二进制文件，整个内容都来自我们这边。

这不应该与 _ 我们的 _ 合并策略混淆，后者甚至不会查看其他树包含的内容。它丢弃了另一棵树所做的一切，声明 _ 我们的 _ 历史记录中包含了所有发生的事情。

```
 theirs 
```

这与 _ 我们的 _ 相反;请注意，与 _ 我们的 _ 不同，没有 _ 他们的 _ 合并策略来混淆这个合并选项。

```
 patience 
```

使用此选项， _merge-recursive_ 花费一点额外的时间来避免由于不重要的匹配行（例如，来自不同函数的大括号）而有时发生的错误。当要合并的分支发生疯狂分歧时使用此选项。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--patience`。

```
 diff-algorithm=[patience|minimal|histogram|myers] 
```

告诉 _merge-recursive_ 使用不同的 diff 算法，这有助于避免由于不重要的匹配行（例如来自不同函数的大括号）而发生的错误。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--diff-algorithm`。

```
 ignore-space-change 
```

```
 ignore-all-space 
```

```
 ignore-space-at-eol 
```

```
 ignore-cr-at-eol 
```

为了进行三向合并，将具有指示类型的空白的行更改为未更改。与空行的其他更改混合的空白更改不会被忽略。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `-b`，`-w`，`--ignore-space-at-eol`和`--ignore-cr-at-eol`。

*   如果 _ 他们的 _ 版本只将空格更改引入一行，_ 我们的 _ 版本被使用;

*   如果 _ 我们的 _ 版本引入了空格更改，但 _ 他们的 _ 版本包含了实质性更改，_ 使用了他们的 _ 版本;

*   否则，合并以通常的方式进行。

```
 renormalize 
```

在解析三向合并时，这将运行虚拟签出并检入文件的所有三个阶段。此选项适用于将分支与不同的清除过滤器或行尾规范化规则合并时使用。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 中的“合并具有不同签入/签出属性的分支”。

```
 no-renormalize 
```

禁用`renormalize`选项。这会覆盖`merge.renormalize`配置变量。

```
 no-renames 
```

关闭重命名检测。这会覆盖`merge.renames`配置变量。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--no-renames`。

```
 find-renames[=<n>] 
```

打开重命名检测，可选择设置相似性阈值。这是默认值。这会覆盖 _merge.renames_ 配置变量。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--find-renames`。

```
 rename-threshold=<n> 
```

已弃用`find-renames=&lt;n&gt;`的同义词。

```
 subtree[=<path>] 
```

此选项是 _ 子树 _ 策略的更高级形式，其中策略猜测两个树在合并时必须如何移位以相互匹配。相反，指定的路径是前缀（或从头开始剥离），以使两个树的形状匹配。

```
 octopus 
```

这解决了具有两个以上磁头的情况，但拒绝执行需要手动解决的复杂合并。它主要用于将主题分支头捆绑在一起。这是拉动或合并多个分支时的默认合并策略。

```
 ours 
```

这会解析任意数量的头，但合并的结果树始终是当前分支头的树，实际上忽略了所有其他分支的所有更改。它旨在用于取代侧枝的旧发展历史。请注意，这与 _ 递归 _ 合并策略的-Xours 选项不同。

```
 subtree 
```

这是一种修改后的递归策略。当合并树 A 和 B 时，如果 B 对应于 A 的子树，则首先调整 B 以匹配 A 的树结构，而不是读取相同级别的树。这种调整也是对共同的祖先树进行的。

使用三向合并的策略（包括默认的 _ 递归 _），如果在两个分支上进行了更改，但稍后在其中一个分支上进行了更改，则该更改将出现在合并结果中;有些人发现这种行为令人困惑。之所以会发生这种情况，是因为在执行合并时只考虑头和合并基础，而不是单个提交。因此，合并算法将恢复的更改视为完全没有更改，而是替换更改的版本。

## 笔记

您应该了解在共享的存储库中使用 _git rebase_ 的含义。另请参阅下面的从上游回收中恢复。

当运行 git-rebase 命令时，它将首先执行“pre-rebase”挂钩（如果存在）。您可以使用此挂钩进行健全性检查，如果不合适则拒绝该挂钩。有关示例，请参阅模板 pre-rebase hook 脚本。

完成后，&lt; branch&gt;将是现在的分支。

## 交互模式

以交互方式重新绑定意味着您有机会编辑已重新生成的提交。您可以重新排序提交，并可以删除它们（清除坏的或其他不需要的补丁）。

交互模式适用于此类工作流程：

1.  有个好主意

2.  破解代码

3.  准备一系列提交

4.  提交

其中第 2 点由几个实例组成

a）经常使用

1.  完成值得承诺的事情

2.  承诺

b）独立修正

1.  意识到某些东西不起作用

2.  修复它

3.  提交它

有时在 b.2 中修复了这个问题。不能修改为它修复的不太完美的提交，因为该提交深深埋藏在补丁系列中。这正是交互式 rebase 的用途：在大量的“a”和“b”之后使用它，通过重新排列和编辑提交，并将多个提交压缩成一个。

使用您要保留的最后一次提交启动它：

```
git rebase -i <after-this-commit>
```

编辑器将被激活当前分支中的所有提交（忽略合并提交），这些提交在给定的提交之后。您可以将此列表中的提交重新排序到您的内容，然后您可以删除它们。该列表看起来或多或少像这样：

```
pick deadbee The oneline of this commit
pick fa1afe1 The oneline of the next commit
...
```

在线描述纯粹是为了您的乐趣; _git rebase_ 不会查看它们但是在提交名称（本例中为“deadbee”和“fa1afe1”），所以不要删除或编辑名称。

通过使用命令“edit”替换命令“pick”，您可以告诉 _git rebase_ 在应用该提交后停止，以便您可以编辑文件和/或提交消息，修改提交，并继续变基。

要中断 rebase（就像“编辑”命令一样，但不首先选择任何提交），使用“break”命令。

如果您只想编辑提交的提交消息，请使用命令“reword”替换命令“pick”。

要删除提交，请将命令“pick”替换为“drop”，或者只删除匹配的行。

如果要将两个或多个提交折叠成一个，请使用“squash”或“fixup”替换第二个和后续提交的命令“pick”。如果提交具有不同的作者，则折叠的提交将归因于第一次提交的作者。折叠提交的建议提交消息是第一次提交的提交消息和具有“squash”命令的提交消息的串联，但是省略了使用“fixup”命令提交的提交消息。

_git rebase_ 将在“pick”替换为“edit”或命令由于合并错误而失败时停止。完成编辑和/或解决冲突后，您可以继续`git rebase --continue`。

例如，如果要重新排序最后 5 次提交，那么 HEAD~4 的内容将成为新的 HEAD。要实现这一点，你可以像这样调用 _git rebase_ ：

```
$ git rebase -i HEAD~5
```

并将第一个补丁移动到列表的末尾。

如果您有这样的历史记录，您可能希望保留合并：

```
           X
            \
         A---M---B
        /
---o---O---P---Q
```

假设您要将从“A”开始到“Q”的侧分支重新绑定。确保当前 HEAD 为“B”，然后调用

```
$ git rebase -i -p --onto Q O
```

重新排序和编辑提交通常会创建未经测试的中间步骤。您可能希望通过运行测试来检查历史编辑没有破坏任何内容，或者至少使用“exec”命令（快捷键“x”）在历史记录的中间点重新编译。您可以通过创建像这样的待办事项列表来实现：

```
pick deadbee Implement feature XXX
fixup f1a5c00 Fix to feature XXX
exec make
pick c0ffeee The oneline of the next commit
edit deadbab The oneline of the commit after
exec cd subdir; make test
...
```

当命令失败时（即退出非 0 状态），交互式 rebase 将停止，以便您有机会解决问题。您可以继续`git rebase --continue`。

“exec”命令在 shell 中启动命令（在`$SHELL`中指定的命令，或者如果未设置`$SHELL`则在默认 shell 中启动），因此您可以使用 shell 功能（如“cd”，“&gt;”， “;”......）。该命令从工作树的根目录运行。

```
$ git rebase -i --exec "make test"
```

此命令允许您检查中间提交是否可编译。待办事项清单变得像这样：

```
pick 5928aea one
exec make test
pick 04d0fda two
exec make test
pick ba46169 three
exec make test
pick f4593f9 four
exec make test
```

## 分裂委员会

在交互模式下，您可以使用“编辑”操作标记提交。但是，这并不一定意味着 _git rebase_ 希望此编辑的结果恰好是一次提交。实际上，您可以撤消提交，也可以添加其他提交。这可以用于将提交拆分为两个：

*   使用`git rebase -i &lt;commit&gt;^`启动交互式 rebase，其中&lt; commit&gt;是您要拆分的提交。实际上，只要包含该提交，任何提交范围都可以。

*   使用“编辑”操作标记要拆分的提交。

*   编辑提交时，执行`git reset HEAD^`。结果是 HEAD 被一个重绕，索引也随之而来。但是，工作树保持不变。

*   现在将更改添加到您希望在第一次提交中拥有的索引。您可以使用`git add`（可能是交互式）或 _git gui_ （或两者）来做到这一点。

*   使用现在适当的提交消息提交 now-current 索引。

*   重复最后两步，直到工作树干净。

*   使用`git rebase --continue`继续变基。

如果您不完全确定中间修订版是否一致（它们是编译的，通过测试套件等），您应该使用 _git stash_ 来隐藏每次提交，测试后尚未提交的更改，如果需要修复，则修改提交。

## 从 UPSTREAM REBASE 恢复

重新定位（或任何其他形式的重写）其他人基于其工作的分支是一个坏主意：它下游的任何人都被迫手动修复其历史记录。本节介绍如何从下游的角度进行修复。然而，真正的解决方法是首先避免重新定位上游。

为了说明，假设您处于某人开发 _ 子系统 _ 分支的情况，并且您正在处理依赖于此 _ 子系统 _ 的 _ 主题 _。您最终可能会得到如下历史记录：

```
    o---o---o---o---o---o---o---o  master
	 \
	  o---o---o---o---o  subsystem
			   \
			    *---*---*  topic
```

如果 _ 子系统 _ 针对 _master_ 进行了重新设置，则会发生以下情况：

```
    o---o---o---o---o---o---o---o  master
	 \			 \
	  o---o---o---o---o	  o'--o'--o'--o'--o'  subsystem
			   \
			    *---*---*  topic
```

如果你现在像往常一样继续开发，并最终将 _ 主题 _ 合并到 _ 子系统 _，那么来自 _ 子系统 _ 的提交将永远保持重复：

```
    o---o---o---o---o---o---o---o  master
	 \			 \
	  o---o---o---o---o	  o'--o'--o'--o'--o'--M	 subsystem
			   \			     /
			    *---*---*-..........-*--*  topic
```

这样的副本通常是不受欢迎的，因为它们使历史变得混乱，使得更难以遵循。为了清理，您需要将 _ 主题 _ 上的提交移植到新的 _ 子系统 _ 提示，即 rebase _ 主题 _。这会产生涟漪效应：_ 主题 _ 下游的任何人也被迫改变，依此类推！

有两种修复方法，将在以下小节中讨论：

```
 Easy case: The changes are literally the same. 
```

如果 _ 子系统 _ rebase 是一个简单的 rebase 并且没有冲突，就会发生这种情况。

```
 Hard case: The changes are not the same. 
```

如果 _ 子系统 _ rebase 发生冲突，或使用`--interactive`省略，编辑，压缩或修复提交，则会发生这种情况;或者如果上游使用`commit --amend`，`reset`或`filter-branch`中的一个。

### 容易的情况

仅在 _ 子系统 _ 上的更改（基于差异内容的修补程序 ID）在 rebase _ 子系统 _ 之前和之后的字面上相同时才有效。

在这种情况下，修复很容易，因为 _git rebase_ 知道跳过已经存在于新上游的更改。所以，如果你说（假设你在 _ 话题 _）

```
    $ git rebase subsystem
```

你将最终得到固定的历史

```
    o---o---o---o---o---o---o---o  master
				 \
				  o'--o'--o'--o'--o'  subsystem
						   \
						    *---*---*  topic
```

### 艰难的案例

如果 _ 子系统 _ 更改与 rebase 之前的更改不完全相符，事情会变得更复杂。

| 注意 | 虽然即使在困难的情况下，“简单案件恢复”有时似乎也是成功的，但它可能会产生意想不到的后果。例如，通过`git rebase --interactive`删除的提交将**复活**！ |

我的想法是手动告诉 _git rebase_ “旧的 _ 子系统 _ 结束，你的 _ 主题 _ 开始了”，也就是说，他们之间的旧合并基础是什么。您必须找到一种方法来命名旧 _ 子系统 _ 的最后一次提交，例如：

*   使用 _ 子系统 _ reflog：在 _git fetch_ 之后，_ 子系统 _ 的旧提示位于`subsystem@{1}`。随后的提取将增加数量。 （参见 [git-reflog [1]](https://git-scm.com/docs/git-reflog) 。）

*   相对于 _ 主题 _ 的提示：知道你的 _ 主题 _ 有三次提交，_ 子系统 _ 的旧提示必须是`topic~3`。

然后，您可以通过说（对于 reflog 案例，假设您已经在 _ 主题 _ 上）将旧的`subsystem..topic`移植到新的提示：

```
    $ git rebase --onto subsystem subsystem@{1}
```

“硬案例”恢复的连锁反应特别糟糕： __ 主题 _ 下游的所有人 _ 现在也必须执行“硬案例”恢复！

## 重新合并

交互式 rebase 命令最初设计用于处理单个补丁系列。因此，从 todo 列表中排除合并提交是有意义的，因为开发人员可能在处理分支时合并当时的`master`，只是最终将所有提交重新绑定到`master`上（跳过合并提交） ）。

但是，开发人员可能想要重新创建合并提交的正当理由是：在处理多个相互关联的分支时保留分支结构（或“提交拓扑”）。

在下面的示例中，开发人员处理主题分支，该分支重构按钮的定义方式，以及使用该重构实现“报告错误”按钮的另一个主题分支。 `git log --graph --format=%s -5`的输出可能如下所示：

```
*   Merge branch 'report-a-bug'
|\
| * Add the feedback button
* | Merge branch 'refactor-button'
|\ \
| |/
| * Use the Button class for all buttons
| * Extract a generic Button class from the DownloadButton one
```

开发人员可能希望在保留分支拓扑的同时将这些提交重新绑定到较新的`master`，例如，当第一个主题分支预期比第二个主题分支更早地集成到`master`中时，比如解决与更改为使其成为`master`的 DownloadButton 类。

可以使用`--rebase-merges`选项执行此 rebase。它将生成一个如下所示的待办事项列表：

```
label onto

# Branch: refactor-button
reset onto
pick 123456 Extract a generic Button class from the DownloadButton one
pick 654321 Use the Button class for all buttons
label refactor-button

# Branch: report-a-bug
reset refactor-button # Use the Button class for all buttons
pick abcdef Add the feedback button
label report-a-bug

reset onto
merge -C a1b2c3 refactor-button # Merge 'refactor-button'
merge -C 6f5e4d report-a-bug # Merge 'report-a-bug'
```

与常规交互式 rebase 相比，除`pick`之外还有`label`，`reset`和`merge`命令。

执行该命令时，`label`命令将标签与当前 HEAD 相关联。这些标签创建为 worktree-local refs（`refs/rewritten/&lt;label&gt;`），将在 rebase 完成时删除。这样，链接到同一存储库的多个工作树中的 rebase 操作不会相互干扰。如果`label`命令失败，则立即重新安排，并提供有用的消息如何继续。

`reset`命令将 HEAD，索引和工作树重置为指定的修订版。它类似于`exec git reset --hard &lt;label&gt;`，但拒绝覆盖未跟踪的文件。如果`reset`命令失败，则会立即重新安排，并提供一条有用的消息，说明如何编辑待办事项列表（这通常在手动将`reset`命令插入待办事项列表并包含拼写错误时发生）。

`merge`命令会将指定的修订版合并到当时的 HEAD 中。使用`-C &lt;original-commit&gt;`，将使用指定合并提交的提交消息。当`-C`更改为小写`-c`时，成功合并后将在编辑器中打开该消息，以便用户可以编辑该消息。

如果`merge`命令因合并冲突以外的任何原因而失败（即合并操作甚至没有开始），则立即重新安排。

此时，`merge`命令将**始终**使用`recursive`合并策略进行常规合并，而`octopus`用于章鱼合并，无法选择不同的合并策略。要解决这个问题，可以使用`exec`命令显式调用`git merge`，使用标签是 worktree-local refs 的事实（例如，ref `refs/rewritten/onto`将对应于标签`onto`）。

注意：第一个命令（`label onto`）标记提交重新定位的修订版本;名称`onto`只是一个约定，作为`--onto`选项的点头。

也可以通过添加`merge &lt;merge-head&gt;`形式的命令从头开始引入全新的合并提交。此表单将生成暂定的提交消息，并始终打开编辑器以允许用户编辑它。这可能是有用的，例如当一个主题分支最终解决一个以上的问题，并希望分成两个甚至更多的主题分支。考虑这个待办事项列表：

```
pick 192837 Switch from GNU Makefiles to CMake
pick 5a6c7e Document the switch to CMake
pick 918273 Fix detection of OpenSSL in CMake
pick afbecd http: add support for TLS v1.3
pick fdbaec Fix detection of cURL in CMake on Windows
```

此列表中与 CMake 无关的一个提交很可能是通过修复切换到 CMake 引入的所有错误来实现的，但它解决了一个不同的问题。要将此分支拆分为两个主题分支，可以像下面这样编辑待办事项列表：

```
label onto

pick afbecd http: add support for TLS v1.3
label tlsv1.3

reset onto
pick 192837 Switch from GNU Makefiles to CMake
pick 918273 Fix detection of OpenSSL in CMake
pick fdbaec Fix detection of cURL in CMake on Windows
pick 5a6c7e Document the switch to CMake
label cmake

reset onto
merge tlsv1.3
merge cmake
```

## BUGS

`--preserve-merges --interactive`提供的待办事项列表不代表修订图的拓扑结构。编辑提交和重写他们的提交消息应该可以正常工作，但尝试重新提交提交往往会产生违反直觉的结果。在这种情况下使用`--rebase-merges`。

例如，尝试重新排列

```
1 --- 2 --- 3 --- 4 --- 5
```

至

```
1 --- 2 --- 4 --- 3 --- 5
```

通过移动“选择 4”线将导致以下历史记录：

```
	3
       /
1 --- 2 --- 4 --- 5
```

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件