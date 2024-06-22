# git-am

> 原文： [`git-scm.com/docs/git-am`](https://git-scm.com/docs/git-am)

## 名称

git-am - 从邮箱中应用一系列补丁

## 概要

```
git am [--signoff] [--keep] [--[no-]keep-cr] [--[no-]utf8]
	 [--[no-]3way] [--interactive] [--committer-date-is-author-date]
	 [--ignore-date] [--ignore-space-change | --ignore-whitespace]
	 [--whitespace=<option>] [-C<n>] [-p<n>] [--directory=<dir>]
	 [--exclude=<path>] [--include=<path>] [--reject] [-q | --quiet]
	 [--[no-]scissors] [-S[<keyid>]] [--patch-format=<format>]
	 [(<mbox> | <Maildir>)…​]
git am (--continue | --skip | --abort | --quit | --show-current-patch)
```

## 描述

将邮箱中的邮件拆分为提交日志消息，作者信息和修补程序，并将其应用于当前分支。

## OPTIONS

```
 (<mbox>|<Maildir>)…​ 
```

要从中读取修补程序的邮箱文件列表。如果您不提供此参数，则该命令将从标准输入读取。如果您提供目录，它们将被视为 Maildirs。

```
 -s 
```

```
 --signoff 
```

使用您自己的提交者标识在提交消息中添加`Signed-off-by:`行。有关详细信息，请参阅 [git-commit [1]](https://git-scm.com/docs/git-commit) 中的签收选项。

```
 -k 
```

```
 --keep 
```

将`-k`标志传递给 _git mailinfo_ （参见 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) ）。

```
 --keep-non-patch 
```

将`-b`标志传递给 _git mailinfo_ （参见 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) ）。

```
 --[no-]keep-cr 
```

使用`--keep-cr`，使用相同选项调用 _git mailsplit_ （参见 [git-mailsplit [1]](https://git-scm.com/docs/git-mailsplit) ），以防止它在行末端剥离 CR。 `am.keepcr`配置变量可用于指定默认行为。 `--no-keep-cr`可用于覆盖`am.keepcr`。

```
 -c 
```

```
 --scissors 
```

在剪刀线之前移除体内的所有物体（参见 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) ）。默认情况下可以使用`mailinfo.scissors`配置变量激活。

```
 --no-scissors 
```

忽略剪刀线（见 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) ）。

```
 -m 
```

```
 --message-id 
```

将`-m`标志传递给 _git mailinfo_ （参见 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) ），以便将 Message-ID 标头添加到提交消息中。 `am.messageid`配置变量可用于指定默认行为。

```
 --no-message-id 
```

不要将 Message-ID 标头添加到提交消息中。 `no-message-id`用于覆盖`am.messageid`。

```
 -q 
```

```
 --quiet 
```

安静。仅打印错误消息。

```
 -u 
```

```
 --utf8 
```

将`-u`标志传递给 _git mailinfo_ （参见 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) ）。从电子邮件中获取的建议提交日志消息被重新编码为 UTF-8 编码（配置变量`i18n.commitencoding`可用于指定项目的首选编码，如果它不是 UTF-8）。

这在 git 的早期版本中是可选的，但现在它是默认的。您可以使用`--no-utf8`覆盖它。

```
 --no-utf8 
```

将`-n`标志传递给 _git mailinfo_ （参见 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) ）。

```
 -3 
```

```
 --3way 
```

```
 --no-3way 
```

当补丁不能干净地应用时，如果补丁记录了它应该应用的 blob 的身份，则回退到三向合并，并且我们在本地可以使用这些 blob。 `--no-3way`可用于覆盖 am.threeWay 配置变量。有关更多信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 中的 am.threeWay。

```
 --ignore-space-change 
```

```
 --ignore-whitespace 
```

```
 --whitespace=<option> 
```

```
 -C<n> 
```

```
 -p<n> 
```

```
 --directory=<dir> 
```

```
 --exclude=<path> 
```

```
 --include=<path> 
```

```
 --reject 
```

这些标志传递给应用补丁的 _git apply_ （参见 [git-apply [1]](https://git-scm.com/docs/git-apply) ）程序。

```
 --patch-format 
```

默认情况下，该命令将尝试自动检测修补程序格式。此选项允许用户绕过自动检测并指定应将补丁解释为的补丁格式。有效格式为 mbox，mboxrd，stgit，stgit-series 和 hg。

```
 -i 
```

```
 --interactive 
```

以交互方式运行。

```
 --committer-date-is-author-date 
```

默认情况下，该命令将电子邮件中的日期记录为提交作者日期，并使用提交创建时间作为提交者日期。这允许用户使用与作者日期相同的值来说谎提交者日期。

```
 --ignore-date 
```

默认情况下，该命令将电子邮件中的日期记录为提交作者日期，并使用提交创建时间作为提交者日期。这允许用户使用与提交者日期相同的值来欺骗作者日期。

```
 --skip 
```

跳过当前的补丁。这仅在重新启动已中止的修补程序时才有意义。

```
 -S[<keyid>] 
```

```
 --gpg-sign[=<keyid>] 
```

GPG 签名提交。 `keyid`参数是可选的，默认为提交者标识;如果指定，它必须粘在没有空格的选项上。

```
 --continue 
```

```
 -r 
```

```
 --resolved 
```

在修补程序失败（例如，尝试应用冲突的修补程序）之后，用户已手动应用它并且索引文件存储应用程序的结果。使用从电子邮件和当前索引文件中提取的作者和提交日志进行提交，然后继续。

```
 --resolvemsg=<msg> 
```

当发生补丁失败时，&lt; msg&gt;将在退出前打印到屏幕上。这将覆盖标准消息，通知您使用`--continue`或`--skip`来处理故障。这仅供 _git rebase_ 和 _git am_ 之间的内部使用。

```
 --abort 
```

恢复原始分支并中止修补操作。

```
 --quit 
```

中止修补操作但保持 HEAD 和索引不变。

```
 --show-current-patch 
```

显示因“冲突”而停止“git am”时正在应用的补丁。

## 讨论

提交作者姓名取自消息的“发件人：”行，提交作者日期取自消息的“日期：”行。在剥离公共前缀“[PATCH&lt; anything&gt;]”之后，“Subject：”行被用作提交的标题。 “Subject：”行应该在一行文本中简明地描述提交的内容。

“From：”和“Subject：”行开始正文覆盖从标题中获取的相应提交作者姓名和标题值。

提交消息由从“主题：”获取的标题，空白行和消息正文直到补丁开始的位置形成。每行末尾的多余空格会自动删除。

该补丁预计将是内联的，直接跟在消息之后。任何形式的行：

*   三个破折号和行尾，或

*   以“diff - ”开头的行，或

*   一行以“索引：”开头

被视为补丁的开头，并且在第一次出现这样的行之前终止提交日志消息。

最初调用`git am`时，为其指定要处理的邮箱的名称。在看到第一个不适用的补丁时，它会在中间中止。您可以通过以下两种方式之一从中恢复：

1.  通过使用`--skip`选项重新运行命令来跳过当前补丁。

2.  手解决工作目录中的冲突，并更新索引文件，使其进入补丁应生成的状态。然后使用`--continue`选项运行命令。

该命令在当前操作完成之前拒绝处理新邮箱，因此如果您决定从头开始，请在运行带有邮箱名称的命令之前运行`git am --abort`。

在应用任何补丁之前，ORIG_HEAD 设置为当前分支的尖端。如果您遇到多次提交有问题，例如在错误的分支上运行 _git am_ ，或者通过更改邮箱更容易修复提交中的错误（例如“From：”行中的错误），这很有用）。

## 挂钩

该命令可以运行`applypatch-msg`，`pre-applypatch`和`post-applypatch`挂钩。有关详细信息，请参阅 [githooks [5]](https://git-scm.com/docs/githooks) 。

## 也可以看看

[git-apply [1]](https://git-scm.com/docs/git-apply) 。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件