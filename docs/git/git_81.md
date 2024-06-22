# git-update-ref

> 原文： [`git-scm.com/docs/git-update-ref`](https://git-scm.com/docs/git-update-ref)

## 名称

git-update-ref - 安全地更新存储在 ref 中的对象名

## 概要

```
git update-ref [-m <reason>] [--no-deref] (-d <ref> [<oldvalue>] | [--create-reflog] <ref> <newvalue> [<oldvalue>] | --stdin [-z])
```

## 描述

给出两个参数，存储&lt; newvalue&gt;在&lt; ref&gt;中，可能取消引用符号引用。例如。 `git update-ref HEAD &lt;newvalue&gt;`将当前分支头更新为新对象。

给出三个参数，存储&lt; newvalue&gt;在&lt; ref&gt;中，在验证&lt; ref&gt;的当前值之后，可能解除引用符号引用。匹配&lt; oldvalue&gt;。例如。 `git update-ref refs/heads/master &lt;newvalue&gt; &lt;oldvalue&gt;`将主分支头更新为&lt; newvalue&gt;仅当其当前值为&lt; oldvalue&gt;时。您可以将 40“0”或空字符串指定为&lt; oldvalue&gt;确保您创建的引用不存在。

它还允许“ref”文件作为指向另一个 ref 文件的符号指针，方法是从“ref：”的四字节头文件序列开始。

更重要的是，它允许更新 ref 文件以遵循这些符号指针，无论它们是符号链接还是这些“常规文件符号引用”。它只跟随**真实**符号链接，如果它们以“refs /”开头：否则它只会尝试读取它们并将它们更新为常规文件（即它将允许文件系统跟随它们，但会覆盖它们符号链接到其他具有常规文件名的地方）。

如果给出了--no-deref，则&lt; ref&gt;本身被覆盖，而不是遵循符号指针的结果。

一般来说，使用

```
git update-ref HEAD "$head"
```

应该是 _ 很多 _ 比做更安全

```
echo "$head" > "$GIT_DIR/HEAD"
```

从符合条件的符号链接**和**两者都是错误检查的立场。符号链接的“refs /”规则意味着指向树“外部”的符号链接是安全的：它们将被用于读取但不用于写入（因此我们永远不会通过 ref 符号链接写入其他树，如果您已通过创建符号链接树复制了整个存档。

使用`-d`标志，它将删除命名的&lt; ref&gt;验证后仍然包含&lt; oldvalue&gt;。

使用`--stdin`，update-ref 从标准输入读取指令并一起执行所有修改。指定表单的命令：

```
update SP <ref> SP <newvalue> [SP <oldvalue>] LF
create SP <ref> SP <newvalue> LF
delete SP <ref> [SP <oldvalue>] LF
verify SP <ref> [SP <oldvalue>] LF
option SP <opt> LF
```

使用`--create-reflog`，update-ref 将为每个 ref 创建一个 reflog，即使通常不会创建一个。

引用包含空格的字段，就好像它们是 C 源代码中的字符串一样;即，被双引号包围并带有反斜杠逃逸。使用 40“0”字符或空字符串指定零值。要指定缺失值，请完全省略该值及其前面的 SP。

或者，使用`-z`以 NUL 终止格式指定，而不引用：

```
update SP <ref> NUL <newvalue> NUL [<oldvalue>] NUL
create SP <ref> NUL <newvalue> NUL
delete SP <ref> NUL [<oldvalue>] NUL
verify SP <ref> NUL [<oldvalue>] NUL
option SP <opt> NUL
```

在此格式中，使用 40“0”指定零值，并使用空字符串指定缺失值。

无论使用哪种格式，都可以以 Git 识别为对象名称的任何形式指定值。任何其他格式的命令或重复的&lt; ref&gt;产生错误。命令含义是：

```
 update 
```

设置&lt; ref&gt;到&lt; newvalue&gt;在验证&lt; oldvalue&gt;之后，如果给出。指定零&lt; newvalue&gt;确保更新后 ref 不存在和/或零&lt; oldvalue&gt;确保在更新之前 ref 不存在。

```
 create 
```

创建&lt; ref&gt;与&lt; newvalue&gt;在验证它不存在之后。给定的&lt; newvalue&gt;可能不是零。

```
 delete 
```

删除&lt; ref&gt;在验证它与&lt; oldvalue&gt;存在之后，如果给出。如果给出，&lt; oldvalue&gt;可能不是零。

```
 verify 
```

验证&lt; ref&gt;反对&lt; oldvalue&gt;但不要改变它。如果&lt; oldvalue&gt;零或缺少，ref 必须不存在。

```
 option 
```

修改命名&lt; ref&gt;的下一个命令的行为。唯一有效的选项是`no-deref`，以避免取消引用符号引用。

如果可以同时使用匹配的&lt; oldvalue&gt;来锁定所有&lt; ref&gt;，则执行所有修改。否则，不执行任何修改。注意，虽然每个人&lt; ref&gt;以原子方式更新或删除，并发读者仍可以看到修改的子集。

## 记录更新

如果 config 参数“core.logAllRefUpdates”为 true 且 ref 为 1，则为“refs / heads /”，“refs / remotes /”，“refs / notes /”或符号 ref HEAD;或文件“$ GIT_DIR / logs /&lt; ref&gt;”然后存在`git update-ref`将一行添加到日志文件“$ GIT_DIR / logs /&lt; ref&gt;” （在创建日志名称之前取消引用所有符号引用）描述 ref 值的更改。日志行的格式为：

```
oldsha1 SP newsha1 SP committer LF
```

其中“oldsha1”是先前存储在&lt; ref&gt;中的 40 字符十六进制值，“newsha1”是&lt; newvalue&gt;的 40 字符十六进制值。 “committer”是标准 Git committer ident 格式的提交者姓名，电子邮件地址和日期。

可选地使用-m：

```
oldsha1 SP newsha1 SP committer TAB message LF
```

其中所有字段如上所述，“message”是提供给-m 选项的值。

如果当前用户无法创建新日志文件，附加到现有日志文件或没有可用的提交者信息，则更新将失败（不更改&lt; ref&gt;）。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件