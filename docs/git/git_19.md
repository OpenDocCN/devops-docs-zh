# git-tag

> 原文： [`git-scm.com/docs/git-tag`](https://git-scm.com/docs/git-tag)

## 名称

git-tag - 创建，列出，删除或验证使用 GPG 签名的标记对象

## 概要

```
git tag [-a | -s | -u <keyid>] [-f] [-m <msg> | -F <file>] [-e]
	<tagname> [<commit> | <object>]
git tag -d <tagname>…​
git tag [-n[<num>]] -l [--contains <commit>] [--no-contains <commit>]
	[--points-at <object>] [--column[=<options>] | --no-column]
	[--create-reflog] [--sort=<key>] [--format=<format>]
	[--[no-]merged [<commit>]] [<pattern>…​]
git tag -v [--format=<format>] <tagname>…​
```

## 描述

在`refs/tags/`中添加标记引用，除非`-d/-l/-v`用于删除，列出或验证标记。

除非给出`-f`，否则指定的标记必须不存在。

如果传递`-a`，`-s`或`-u &lt;keyid&gt;`之一，该命令将创建 _ 标签 _ 对象，并需要标记消息。除非给出`-m &lt;msg&gt;`或`-F &lt;file&gt;`，否则将启动编辑器以供用户键入标记消息。

如果给出`-m &lt;msg&gt;`或`-F &lt;file&gt;`并且`-a`，`-s`和`-u &lt;keyid&gt;`不存在，则暗示`-a`。

否则，创建直接指向给定对象（即，轻量标签）的标签引用。

使用`-s`或`-u &lt;keyid&gt;`时，将创建 GnuPG 签名标记对象。如果未使用`-u &lt;keyid&gt;`，则使用当前用户的提交者标识来查找用于签名的 GnuPG 密钥。配置变量`gpg.program`用于指定自定义 GnuPG 二进制文件。

标记对象（使用`-a`，`-s`或`-u`创建）称为“带注释”标记;它们包含创建日期，标记器名称和电子邮件，标记消息以及可选的 GnuPG 签名。而“轻量级”标签只是对象的名称（通常是提交对象）。

带注释的标签用于发布，而轻量级标签用于私有或临时对象标签。因此，一些用于命名对象的 git 命令（如`git describe`）将默认忽略轻量级标记。

## OPTIONS

```
 -a 
```

```
 --annotate 
```

创建一个未签名的带注释的标记对象

```
 -s 
```

```
 --sign 
```

使用默认电子邮件地址密钥创建 GPG 签名标记。

```
 -u <keyid> 
```

```
 --local-user=<keyid> 
```

使用给定密钥创建 GPG 签名标记。

```
 -f 
```

```
 --force 
```

用给定名称替换现有标记（而不是失败）

```
 -d 
```

```
 --delete 
```

删除具有给定名称的现有标签。

```
 -v 
```

```
 --verify 
```

验证给定标记名称的 GPG 签名。

```
 -n<num> 
```

&lt; NUM&gt;指定在使用-l 时打印注释的行数（如果有）。意味着`--list`。

默认情况下不打印任何注释行。如果`-n`没有给出编号，则只打印第一行。如果标记未注释，则显示提交消息。

```
 -l 
```

```
 --list 
```

列出标签。使用可选的`&lt;pattern&gt;...`，例如`git tag --list 'v-*'`，仅列出与模式匹配的标记。

不带参数运行“git tag”也会列出所有标签。该模式是 shell 通配符（即，使用 fnmatch（3）匹配）。可以给出多种模式;如果它们中的任何一个匹配，则显示标记。

如果提供了任何其他类似列表的选项（如`--contains`），则会隐式提供此选项。有关详细信息，请参阅每个选项的文档。

```
 --sort=<key> 
```

根据给定的密钥排序。前缀`-`按值的降序排序。您可以使用--sort =&lt; key&gt;选项多次，在这种情况下，最后一个键成为主键。还支持“version：refname”或“v：refname”（标记名称被视为版本）。 “version：refname”排序顺序也可能受“versionsort.suffix”配置变量的影响。支持的键与`git for-each-ref`中的键相同。排序顺序默认为`tag.sort`变量（如果存在）配置的值，否则为字典顺序。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

```
 --color[=<when>] 
```

尊重`--format`选项中指定的任何颜色。 `&lt;when&gt;`字段必须是`always`，`never`或`auto`之一（如果`&lt;when&gt;`不存在，则表现得好像`always`一样）。

```
 -i 
```

```
 --ignore-case 
```

排序和过滤标签不区分大小写。

```
 --column[=<options>] 
```

```
 --no-column 
```

在列中显示标记列表。有关选项语法，请参阅配置变量 column.tag。没有选项的`--column`和`--no-column`分别相当于和 _ 永远不会 _。

此选项仅在列出没有注释行的标签时适用。

```
 --contains [<commit>] 
```

仅列出包含指定提交的标记（如果未指定，则为 HEAD）。意味着`--list`。

```
 --no-contains [<commit>] 
```

仅列出不包含指定提交的标记（如果未指定，则为 HEAD）。意味着`--list`。

```
 --merged [<commit>] 
```

仅列出其提交可从指定提交到达的标记（如果未指定，则为`HEAD`），与`--no-merged`不兼容。

```
 --no-merged [<commit>] 
```

仅列出其提交无法从指定提交到达的标记（如果未指定，则为`HEAD`），与`--merged`不兼容。

```
 --points-at <object> 
```

仅列出给定对象的标签（如果未指定，则为 HEAD）。意味着`--list`。

```
 -m <msg> 
```

```
 --message=<msg> 
```

使用给定的标记消息（而不是提示）。如果给出了多个`-m`选项，则它们的值将作为单独的段落连接在一起。如果没有给出`-a`，`-s`或`-u &lt;keyid&gt;`，则表示`-a`。

```
 -F <file> 
```

```
 --file=<file> 
```

从给定文件中获取标记消息。使用 _-_ 从标准输入读取信息。如果没有给出`-a`，`-s`或`-u &lt;keyid&gt;`，则表示`-a`。

```
 -e 
```

```
 --edit 
```

从带有`-F`的文件和带有`-m`的命令行获取的消息通常用作未修改的标记消息。此选项允许您进一步编辑从这些来源获取的消息。

```
 --cleanup=<mode> 
```

此选项设置清除标记消息的方式。 _&lt; mode&gt;_ 可以是 _ 逐字 _，_ 空白 _ 和 _ 条带 _ 之一。 _ 条 _ 模式是默认模式。 _ 逐字 _ 模式根本不改变消息，_ 空格 _ 只删除前导/尾随空白行，_ 条 _ 删除空白和评论。

```
 --create-reflog 
```

为标记创建 reflog。要全局启用标签的 reflog，请参见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`core.logAllRefUpdates`。否定形式`--no-create-reflog`仅覆盖较早的`--create-reflog`，但目前不会否定`core.logAllRefUpdates`的设置。

```
 --format=<format> 
```

一个字符串，用于插入显示的标记 ref 中的`%(fieldname)`及其指向的对象。格式与 [git-for-each-ref [1]](https://git-scm.com/docs/git-for-each-ref) 的格式相同。未指定时，默认为`%(refname:strip=2)`。

```
 <tagname> 
```

要创建，删除或描述的标记的名称。新标签名称必须通过 [git-check-ref-format [1]](https://git-scm.com/docs/git-check-ref-format) 定义的所有检查。其中一些检查可能会限制标记名称中允许的字符。

```
 <commit> 
```

```
 <object> 
```

新标记将引用的对象，通常是提交。默认为 HEAD。

## 组态

默认情况下， _git 标签 _ 处于默认签名模式（-s）将使用您的提交者标识（`Your Name &lt;your@email.address&gt;`形式）来查找密钥。如果要使用其他默认密钥，可以在存储库配置中指定它，如下所示：

```
[user]
    signingKey = <gpg-keyid>
```

仅在列出标签时，即使用或暗示`-l`时，才会遵守`pager.tag`。默认是使用寻呼机。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

## 讨论

### 关于重新标记

当您标记错误的提交并且您想要重新标记时，您应该怎么做？

如果你从未推过任何东西，只需重新标记即可。使用“-f”替换旧的。而且你已经完成了。

但是如果你把事情搞砸了（或者其他人可以直接读取你的存储库），那么其他人就已经看到了旧的标签了。在这种情况下，您可以执行以下两项操作之一：

1.  理智的事情。只是承认你搞砸了，并使用不同的名字。其他人已经看过一个标签名称，如果你保持相同的名字，你可能会遇到两个人都有“版本 X”的情况，但他们实际上有 _ 不同的 _“X”。所以只需称它为“X.1”并完成它。

2.  疯狂的事情。你真的想把新版本称为“X”，_ 即使 _ 其他人已经看过旧版本。所以再次使用 _git tag -f_ ，好像你还没有发布旧版本一样。

但是，Git 确实**没有**（它不应该）改变用户背后的标签。因此，如果有人已经得到了旧标签，那么在树上执行 _git pull_ 不应该只是让它们覆盖旧标签。

如果有人从你那里得到了一个发布标签，你就不能通过更新自己的标签来改变标签。这是一个很大的安全问题，因为人们必须能够信任他们的标签名称。如果你真的想做疯狂的事情，你需要了解它，告诉别人你搞砸了。你可以通过发布一个非常公开的声明来做到这一点：

```
Ok, I messed up, and I pushed out an earlier version tagged as X. I
then fixed something, and retagged the *fixed* tree as X again.

If you got the wrong tag, and want the new one, please delete
the old one and fetch the new one by doing:

	git tag -d X
	git fetch origin tag X

to get my updated tag.

You can test which tag you have by doing

	git rev-parse X

which should return 0123456789abcdef.. if you have the new version.

Sorry for the inconvenience.
```

这看起来有点复杂吗？它**应该是**。没有办法自动“修复”它是正确的。人们需要知道他们的标签可能已被更改。

### 关于自动跟随

如果您正在关注其他人的树，则您很可能使用远程跟踪分支（例如`refs/remotes/origin/master`）。您通常需要来自另一端的标签。

另一方面，如果你想要从其他人那里获取一次性合并，那么你通常不希望从那里获得标签。这种情况更常发生在靠近顶层但不限于它们的人身上。当彼此拉扯时，凡人都不一定想要从另一个人那里自动获得私人锚点标签。

通常，邮件列表上的“请拉”消息只提供两条信息：一个 repo URL 和一个分支名称;这是为了在 _git fetch_ 命令行结束时轻松剪切和粘贴：

```
Linus, please pull from

	git://git..../proj.git master

to get the following updates...
```

变为：

```
$ git pull git://git..../proj.git master
```

在这种情况下，您不希望自动关注其他人的标签。

Git 的一个重要方面是它的分布式特性，这在很大程度上意味着系统中没有固有的“上游”或“下游”。从表面上看，上面的例子似乎表明标签命名空间由人的上层所有，而且标签只向下流动，但事实并非如此。它仅显示使用模式确定谁对其标签感兴趣。

一次性拉动表示提交历史现在正越过一个人圈之间的边界（例如“主要对内核的网络部分感兴趣的人”），他们可能拥有自己的一组标签（例如“这是来自网络组的第三个候选版本被提议用于 2.6.21 版本的一般消费“）到另一个人群（例如”整合各种子系统改进的人“）。后者通常对前一组内部使用的详细标签不感兴趣（这就是“内部”的含义）。这就是为什么在这种情况下不希望自动跟踪标签的原因。

很可能在网络人员中，他们可能想要在他们的组内部交换标签，但在该工作流程中，他们很可能通过具有远程跟踪分支来跟踪彼此的进度。同样，自动跟随这些标签的启发式是一件好事。

### 关于回溯标签

如果您从另一个 VCS 导入了一些更改，并且想为工作的主要版本添加标记，那么能够指定嵌入标记对象内部的日期是很有用的。例如，标签对象中的这种数据会影响 gitweb 界面中标签的排序。

要设置将来标记对象中使用的日期，请设置环境变量 GIT_COMMITTER_DATE（请参阅后面对可能值的讨论;最常见的形式是“YYYY-MM-DD HH：MM”）。

例如：

```
$ GIT_COMMITTER_DATE="2006-10-02 10:31" git tag -s v1.0.1
```

## 日期格式

`GIT_AUTHOR_DATE`，`GIT_COMMITTER_DATE`环境变量支持以下日期格式：

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

## 也可以看看

[git-check-ref-format [1]](https://git-scm.com/docs/git-check-ref-format) 。 [git-config [1]](https://git-scm.com/docs/git-config) 。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件