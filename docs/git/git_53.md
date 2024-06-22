# git-svn

> 原文： [`git-scm.com/docs/git-svn`](https://git-scm.com/docs/git-svn)

## 名称

git-svn - Subversion 存储库和 Git 之间的双向操作

## 概要

```
git svn <command> [<options>] [<arguments>]
```

## 描述

_git svn_ 是 Subversion 和 Git 之间变更集的简单管道。它提供 Subversion 和 Git 存储库之间的双向变化流。

_git svn_ 可以使用--stdlayout 选项跟踪常见的“trunk / branches / tags”布局的标准 Subversion 存储库。它还可以使用-T / -t / -b 选项跟随任何布局中的分支和标签（请参阅下面的 _init_ 选项，以及 _clone_ 命令）。

一旦跟踪 Subversion 存储库（使用上述任何方法），就可以通过 _fetch_ 命令从 Subversion 更新 Git 存储库，并通过 _dcommit_ 命令从 Git 更新 Subversion。

## COMMANDS

```
 init 
```

使用 _git svn_ 的其他元数据目录初始化一个空的 Git 存储库。 Subversion URL 可以指定为命令行参数，也可以指定为-T / -t / -b 的完整 URL 参数。可选地，可以将要操作的目标目录指定为第二个参数。通常，此命令初始化当前目录。

```
 -T<trunk_subdir> 
```

```
 --trunk=<trunk_subdir> 
```

```
 -t<tags_subdir> 
```

```
 --tags=<tags_subdir> 
```

```
 -b<branches_subdir> 
```

```
 --branches=<branches_subdir> 
```

```
 -s 
```

```
 --stdlayout 
```

这些是 init 的可选命令行选项。这些标志中的每一个都可以指向相对存储库路径（--tags = project / tags）或完整 URL（--tags = https：//foo.org/project/tags）。如果您的 Subversion 存储库在多个路径下放置标记或分支，您可以指定多个--tags 和/或--branches 选项。选项--stdlayout 是将 trunk，tags，branches 设置为相对路径的简便方法，这是 Subversion 的默认设置。如果同时给出任何其他选项，则它们优先。

```
 --no-metadata 
```

在[svn-remote]配置中设置 _noMetadata_ 选项。建议不要使用此选项，请在使用此选项之前阅读本联机帮助页的 _svn.noMetadata_ 部分。

```
 --use-svm-props 
```

在[svn-remote]配置中设置 _useSvmProps_ 选项。

```
 --use-svnsync-props 
```

在[svn-remote]配置中设置 _useSvnsyncProps_ 选项。

```
 --rewrite-root=<URL> 
```

在[svn-remote]配置中设置 _rewriteRoot_ 选项。

```
 --rewrite-uuid=<UUID> 
```

在[svn-remote]配置中设置 _rewriteUUID_ 选项。

```
 --username=<user> 
```

对于 SVN 处理（http，https 和 plain svn）身份验证的传输，请指定用户名。对于其他传输（例如`svn+ssh://`），您必须在 URL 中包含用户名，例如`svn+ssh://foo@svn.bar.com/project`

```
 --prefix=<prefix> 
```

如果指定了 trunk / branches / tags，则允许指定前缀为遥控器名称的前缀。前缀不会自动包含尾部斜杠，因此请确保在参数中包含一个前缀，如果这是您想要的。如果指定了--branches / -b，则前缀必须包含尾部斜杠。在任何情况下都强烈建议设置前缀（带斜杠），因为你的 SVN 跟踪引用将位于“refs / remotes / $ prefix / **”，这与 Git 自己的远程跟踪引用兼容 layout（refs / remotes / $ remote /** ）。如果要跟踪共享公共存储库的多个项目，则设置前缀也很有用。默认情况下，前缀设置为 _origin /_ 。

| 注意 | 在 Git v2.0 之前，默认前缀是“”（没有前缀）。这意味着 SVN 跟踪引用被放在“refs / remotes / *”，这与 Git 自己的远程跟踪引用的组织方式不兼容。如果您仍然需要旧的默认值，可以通过在命令行上传递`--prefix ""`来获取它（如果你的 Perl 的 Getopt :: Long 是&lt; v2.37，`--prefix=""`可能不起作用）。 |

```
 --ignore-refs=<regex> 
```

传递给 _init_ 或 _clone_ 时，此正则表达式将作为配置键保留。有关`--ignore-refs`的说明，请参见 _fetch_ 。

```
 --ignore-paths=<regex> 
```

传递给 _init_ 或 _clone_ 时，此正则表达式将作为配置键保留。有关`--ignore-paths`的说明，请参见 _fetch_ 。

```
 --include-paths=<regex> 
```

传递给 _init_ 或 _clone_ 时，此正则表达式将作为配置键保留。有关`--include-paths`的说明，请参见 _fetch_ 。

```
 --no-minimize-url 
```

当跟踪多个目录（使用--stdlayout， - blank 或--tags 选项）时，git svn 将尝试连接到 Subversion 存储库的根目录（或允许的最高级别）。如果整个项目在存储库中移动，则此默认设置允许更好地跟踪历史记录，但可能会导致读取访问限制到位的存储库出现问题。传递`--no-minimize-url`将允许 git svn 按原样接受 URL，而不尝试连接到更高级别的目录。默认情况下，当仅跟踪一个 URL /分支时，此选项处于关闭状态（这样做不太好）。

```
 fetch 
```

从我们正在跟踪的 Subversion 远程获取未经修改的修订。 $ GIT_DIR / config 文件中[svn-remote“...”]部分的名称可以指定为可选的命令行参数。

如果需要，这会自动更新 rev_map（有关详细信息，请参阅下面的 FILES 部分中的 _$ GIT_DIR / svn / * \ * /。rev_map。*_ ）。

```
 --localtime 
```

将 Git 提交时间存储在本地时区而不是 UTC 中。这使 _git log_ （即使没有--date = local）显示与`svn log`在本地时区相同的时间。

这不会干扰与您克隆的 Subversion 存储库的互操作，但如果您希望本地 Git 存储库能够与其他人的本地 Git 存储库进行互操作，请不要使用此选项，或者您应该同时使用它同一个当地时区。

```
 --parent 
```

仅从当前 HEAD 的 SVN 父级获取。

```
 --ignore-refs=<regex> 
```

忽略与 Perl 正则表达式匹配的分支或标记的引用。像`^refs/remotes/origin/(?!tags/wanted-tag|wanted-branch).*$`这样的“负前瞻断言”可用于仅允许某些参考。

```
config key: svn-remote.<name>.ignore-refs
```

如果设置了 ignore-refs 配置键，并且还给出了命令行选项，则将使用两个正则表达式。

```
 --ignore-paths=<regex> 
```

这允许指定 Perl 正则表达式，该表达式将导致从 SVN 的 checkout 跳过所有匹配路径。 `--ignore-paths`选项应匹配每个 _fetch_ （包括 _ 克隆 _， _dcommit_ ， _rebase_ 等）的自动提取）给定存储库。

```
config key: svn-remote.<name>.ignore-paths
```

如果设置了 ignore-paths 配置密钥，并且还给出了命令行选项，则将使用两个正则表达式。

例子：

```
 Skip "doc*" directory for every fetch 
```

```
--ignore-paths="^doc"
```

```
 Skip "branches" and "tags" of first level directories 
```

```
--ignore-paths="^[^/]+/(?:branches|tags)"
```

```
 --include-paths=<regex> 
```

这允许指定一个 Perl 正则表达式，该表达式将仅包含来自 SVN 的 checkout 的匹配路径。 `--include-paths`选项应匹配每个 _fetch_ （包括 _ 克隆 _， _dcommit_ ， _rebase_ 等）的自动提取）给定存储库。 `--ignore-paths`优先于`--include-paths`。

```
config key: svn-remote.<name>.include-paths
```

```
 --log-window-size=<n> 
```

获取&lt; n&gt;扫描 Subversion 历史记录时每个请求的日志条目。默认值为 100.对于非常大的 Subversion 存储库， _clone_ / _fetch_ 可能需要更大的值才能在合理的时间内完成。但过大的值可能会导致更高的内存使用量和请求超时。

```
 clone 
```

运行 _init_ 和 _ 获取 _。它将根据传递给它的 URL 的基名自动创建一个目录;或者如果第二个论点通过;它将创建一个目录并在其中工作。它接受 _init_ 和 _fetch_ 命令接受的所有参数; `--fetch-all`和`--parent`除外。克隆存储库后， _fetch_ 命令将能够在不影响工作树的情况下更新修订版;并且 _rebase_ 命令将能够使用最新更改更新工作树。

```
 --preserve-empty-dirs 
```

在本地 Git 存储库中为从 Subversion 获取的每个空目录创建一个占位符文件。这包括通过删除 Subversion 存储库中的所有条目（但不是目录本身）而变为空的目录。不再需要时，也会跟踪和删除占位符文件。

```
 --placeholder-filename=<filename> 
```

设置--preserve-empty-dirs 创建的占位符文件的名称。默认值：“。gitignore”

```
 rebase 
```

这将从当前 HEAD 的 SVN 父级获取修订，并针对它重新定义当前（未提交到 SVN）的工作。

这与`svn update`或 _git pull_ 类似，只是它保留 _git rebase_ 而不是 _git merge_ 的线性历史，以便于 _git 的同意 svn_ 。

这接受 _git svn fetch_ 和 _git rebase_ 接受的所有选项。但是，`--fetch-all`仅从当前[svn-remote]获取，而不是所有[svn-remote]定义。

像 _git rebase_ ;这要求工作树清洁并且没有未提交的更改。

如果需要，这会自动更新 rev_map（有关详细信息，请参阅下面的 FILES 部分中的 _$ GIT_DIR / svn / * \ * /。rev_map。*_ ）。

```
 -l 
```

```
 --local 
```

不要远程取;仅针对上游 SVN 上次提取的提交运行 _git rebase_ 。

```
 dcommit 
```

将每个 diff 从当前分支直接提交到 SVN 存储库，然后 rebase 或 reset（取决于 SVN 和 head 之间是否存在差异）。这将在 SVN 中为 Git 中的每个提交创建一个修订版本。

当可选的 Git 分支名称（或 Git 提交对象名称）被指定为参数时，子命令在指定的分支上工作，而不在当前分支上工作。

使用 _dcommit_ 优于 _set-tree_ （下文）。

```
 --no-rebase 
```

提交后，不要改变或重置。

```
 --commit-url <URL> 
```

提交此 SVN URL（完整路径）。这旨在允许使用一种传输方法创建的现有 _git svn_ 存储库（例如，用于匿名读取的`svn://`或`http://`）如果用户稍后被授权访问备用传输方法（例如，用于提交的`svn+ssh://`或`https://`）。

```
config key: svn-remote.<name>.commiturl
config key: svn.commiturl (overwrites all svn-remote.<name>.commiturl options)
```

请注意，commiturl 配置密钥的 SVN URL 包括 SVN 分支。如果您想要为整个 SVN 存储库设置提交 URL，请使用 svn-remote。&lt; name&gt; .pushurl。

强烈建议不要将此选项用于任何其他目的（不要问）。

```
 --mergeinfo=<mergeinfo> 
```

在 dcommit 期间添加给定的合并信息（例如`--mergeinfo="/branches/foo:1-10"`）。所有 svn 服务器版本都可以存储此信息（作为属性），从 1.5 版开始的 svn 客户端可以使用它。要指定来自多个分支的合并信息，请在分支之间使用单个空格字符（`--mergeinfo="/branches/foo:1-10 /branches/bar:3,5-6,8"`）

```
config key: svn.pushmergeinfo
```

此选项将导致 git-svn 尝试在可能的情况下自动填充 SVN 存储库中的 svn：mergeinfo 属性。目前，这只能在提交非快进合并时才能完成，其中除第一个之外的所有父级已经被推入 SVN。

```
 --interactive 
```

要求用户确认应该将补丁集实际发送到 SVN。对于每个补丁，可以回答“是”（接受此补丁），“否”（丢弃此补丁），“全部”（接受所有补丁）或“退出”。

_git svn dcommit_ 如果回答为“no”或“quit”则立即返回，而不向 SVN 提交任何内容。

```
 branch 
```

在 SVN 存储库中创建分支。

```
 -m 
```

```
 --message 
```

允许指定提交消息。

```
 -t 
```

```
 --tag 
```

使用 tags_subdir 而不是 git svn init 期间指定的 branches_subdir 创建标记。

```
 -d<path> 
```

```
 --destination=<path> 
```

如果为 _init_ 或 _clone_ 命令提供了多个 - 分支（或--tags）选项，则必须提供您希望的分支（或标记）的位置在 SVN 存储库中创建。 &lt;路径&gt;指定用于创建分支或标记的路径，并且应该与其中一个已配置的分支或标记 refspecs 的左侧模式匹配。您可以使用命令查看这些 refspecs

```
git config --get-all svn-remote.<name>.branches
git config --get-all svn-remote.<name>.tags
```

其中&lt; name&gt;是 _init_ 的-R 选项（默认情况下为“svn”）指定的 SVN 存储库的名称。

```
 --username 
```

指定要执行提交的 SVN 用户名。此选项会覆盖 _ 用户名 _ 配置属性。

```
 --commit-url 
```

使用指定的 URL 连接到目标 Subversion 存储库。这在源 SVN 存储库是只读的情况下很有用。此选项会覆盖配置属性 _commiturl_ 。

```
git config --get-all svn-remote.<name>.commiturl
```

```
 --parents 
```

创建父文件夹。此参数等效于参数--parents on svn cp 命令，对非标准存储库布局很有用。

```
 tag 
```

在 SVN 存储库中创建标记。这是 _branch -t_ 的简写。

```
 log 
```

当 svn 用户引用-r / - 版本号时，这应该可以很容易地查找 svn 日志消息。

支持'svn log'的以下功能：

```
 -r <n>[:<n>] 
```

```
 --revision=<n>[:<n>] 
```

支持，非数字 args 不是：HEAD，NEXT，BASE，PREV 等...

```
 -v 
```

```
 --verbose 
```

它与 svn log 中的--verbose 输出不完全兼容，但相当接近。

```
 --limit=<n> 
```

与--max-count 不同，不计算合并/排除的提交

```
 --incremental 
```

支持的

新功能：

```
 --show-commit 
```

还显示了 Git commit sha1

```
 --oneline 
```

我们的版本--pretty = oneline

| 注意 | SVN 本身只存储 UTC 时间，没有别的。常规 svn 客户端将 UTC 时间转换为本地时间（或基于 TZ =环境）。此命令具有相同的行为。 |

任何其他参数直接传递给 _git log_

```
 blame 
```

显示修订版和作者上次修改文件的每一行。默认情况下，此模式的输出与'svn blame'的输出格式兼容。与 SVN blame 命令一样，忽略工作树中的本地未提交更改; HEAD 修订版中的文件版本已注释。未知参数直接传递给 _git blame_ 。

```
 --git-format 
```

以与 _git blame_ 相同的格式生成输出，但使用 SVN 修订号而不是 Git 提交哈希值。在此模式下，尚未提交到 SVN 的更改（包括本地工作副本编辑）显示为修订版 0。

```
 find-rev 
```

当给定形式为 _rN_ 的 SVN 修订号时，返回相应的 Git 提交哈希（这可以选择后跟树，以指定应搜索哪个分支）。给定 tree-ish 时，返回相应的 SVN 修订号。

```
 -B 
```

```
 --before 
```

如果给出 SVN 修订版，则不需要完全匹配，而是在指定的修订版中找到与 SVN 存储库（在当前分支上）的状态相对应的提交。

```
 -A 
```

```
 --after 
```

如果给出 SVN 修订版，则不要求完全匹配;如果没有完全匹配，则返回在历史记录中向前搜索的最接近的匹配。

```
 set-tree 
```

您应该考虑使用 _dcommit_ 而不是此命令。将指定的提交或树对象提交给 SVN。这取决于您导入的获取数据是最新的。这使得在提交 SVN 时绝对不会尝试进行修补，它只是用树中指定的文件或提交覆盖文件。假设所有合并都独立于 _git svn_ 功能发生。

```
 create-ignore 
```

递归地在目录上找到 svn：ignore 属性并创建匹配的.gitignore 文件。生成的文件将暂存，但未提交。使用-r / - revision 来引用特定的修订版。

```
 show-ignore 
```

递归查找并列出目录上的 svn：ignore 属性。输出适合附加到$ GIT_DIR / info / exclude 文件。

```
 mkdirs 
```

尝试根据$ GIT_DIR / svn /&lt; refname&gt; /unhandled.log 文件中的信息重新创建核心 Git 无法跟踪的空目录。使用“git svn clone”和“git svn rebase”时会自动重新创建空目录，因此“mkdirs”适用于“git checkout”或“git reset”之类的命令。 （有关详细信息，请参阅 svn-remote。&lt; name&gt; .automkdirs 配置文件选项。）

```
 commit-diff 
```

从命令行提交两个 tree-ish 参数的 diff。此命令不依赖于`git svn init` -ed 存储库。该命令有三个参数，（a）要反对的原始树，（b）新的树结果，（c）目标 Subversion 存储库的 URL。如果您使用的是 _git svn_ -aware 存储库（已经`init`与 _git svn_ 一起使用），则可以省略最终参数（URL）。 -r&lt; revision&gt;此选项是必需的。

提交消息直接使用`-m`或`-F`选项提供，或者在第二个 tree-ish 表示此类对象时间接提供标记或提交，或者通过调用编辑器请求提交消息（请参阅`--edit`选项）下面）。

```
 -m <msg> 
```

```
 --message=<msg> 
```

使用给定的`msg`作为提交消息。此选项禁用`--edit`选项。

```
 -F <filename> 
```

```
 --file=<filename> 
```

从给定文件中获取提交消息。此选项禁用`--edit`选项。

```
 info 
```

显示与“svn info”提供的文件或目录类似的信息。目前不支持-r / - revision 参数。使用--url 选项仅输出 _URL：_ 字段的值。

```
 proplist 
```

列出存储在 Subversion 存储库中的有关给定文件或目录的属性。使用-r / - revision 来引用特定的 Subversion 修订版。

```
 propget 
```

获取作为文件的第一个参数给出的 Subversion 属性。可以使用-r / - revision 指定特定修订。

```
 propset 
```

将作为第一个参数给出的 Subversion 属性设置为作为第三个参数给出的文件的第二个参数给出的值。

例：

```
git svn propset svn:keywords "FreeBSD=%H" devel/py-tipper/Makefile
```

这将为文件 _devel / py-tipper / Makefile_ 设置属性 _svn：keywords_ 到 _FreeBSD =％H_ 。

```
 show-externals 
```

显示 Subversion 外部。使用-r / - revision 指定特定修订。

```
 gc 
```

压缩$ GIT_DIR / svn /&lt; refname&gt; /unhandled.log 文件并删除$ GIT_DIR / svn /&lt; refname&gt; / index 文件。

```
 reset 
```

将 _fetch_ 的效果撤消回指定的修订版。这允许您重新 _ 获取 _ SVN 修订版。通常，SVN 修订版的内容永远不会改变，并且 _ 重置 _ 不应该是必需的。但是，如果 SVN 权限发生更改，或者您更改了--ignore-paths 选项，则 _fetch_ 可能会失败，并且“未在提交中找到”（文件以前未显示）或“校验和不匹配”（错过了修改）。如果问题文件永远不能被忽略（使用--ignore-paths）修复 repo 的唯一方法是使用 _reset_ 。

仅更改了 rev_map 和 refs / remotes / git-svn（有关详细信息，请参阅下面 FILES 部分中的 _$ GIT_DIR / svn / * \ * /。rev_map。*_ ）。使用 _fetch_ 然后 _git reset_ 或 _git rebase_ 关注 _ 重置 _，将本地分支移动到新树上。

```
 -r <n> 
```

```
 --revision=<n> 
```

指定要保留的最新修订。所有后来的修订都被丢弃了。

```
 -p 
```

```
 --parent 
```

同时丢弃指定的修订版，保留最近的父版本。

```
 Example: 
```

假设您在“master”中有本地更改，但您需要重新获取“r2”。

```
    r1---r2---r3 remotes/git-svn
                \
                 A---B master
```

修复了忽略路径或 SVN 权限问题，导致“r2”首先不完整。然后：

```
git svn reset -r2 -p
git svn fetch
```

```
    r1---r2'--r3' remotes/git-svn
      \
       r2---r3---A---B master
```

然后使用 _git rebase_ 修复“master”。不要使用 _git merge_ 或者您的历史记录将与以后的 _dcommit_ 不兼容！

```
git rebase --onto remotes/git-svn A^ master
```

```
    r1---r2'--r3' remotes/git-svn
                \
                 A'--B' master
```

## OPTIONS

```
 --shared[=(false|true|umask|group|all|world|everybody)] 
```

```
 --template=<template_directory> 
```

仅用于 _init_ 命令。这些直接传递给 _git init_ 。

```
 -r <arg> 
```

```
 --revision <arg> 
```

与 _fetch_ 命令一起使用。

这允许支持部分/烧灼历史的修订范围。 $ NUMBER，$ NUMBER1：$ NUMBER2（数字范围），$ NUMBER：HEAD 和 BASE：$ NUMBER 均受支持。

这可以让你在运行 fetch 时制作部分镜像;但通常不推荐，因为历史将被跳过和丢失。

```
 - 
```

```
 --stdin 
```

仅用于 _set-tree_ 命令。

从 stdin 读取提交列表并以相反的顺序提交它们。只从每一行读取前导 sha1，因此可以使用 _git rev-list --pretty = oneline_ 输出。

```
 --rmdir 
```

仅用于 _dcommit_ ， _set-tree_ 和 _commit-diff_ 命令。

如果没有留下文件，则从 SVN 树中删除目录。 SVN 可以对空目录进行编辑，如果没有文件，则默认情况下不会删除它们。 Git 无法对空目录进行版本控制。启用此标志将使提交到 SVN 的行为与 Git 相同。

```
config key: svn.rmdir
```

```
 -e 
```

```
 --edit 
```

仅用于 _dcommit_ ， _set-tree_ 和 _commit-diff_ 命令。

在提交 SVN 之前编辑提交消息。对于提交的对象，默认情况下处于关闭状态，并且在提交树对象时强制关闭。

```
config key: svn.edit
```

```
 -l<num> 
```

```
 --find-copies-harder 
```

仅用于 _dcommit_ ， _set-tree_ 和 _commit-diff_ 命令。

它们都直接传递给 _git diff-tree_ ;有关详细信息，请参阅 [git-diff-tree [1]](https://git-scm.com/docs/git-diff-tree) 。

```
config key: svn.l
config key: svn.findcopiesharder
```

```
 -A<filename> 
```

```
 --authors-file=<filename> 
```

语法与 _git cvsimport_ 使用的文件兼容，但 _&lt;&gt;可以提供空的电子邮件地址。_ ：

```
	loginname = Joe User <user@example.com>
```

如果指定了此选项并且 _git svn_ 遇到作者文件中不存在的 SVN 提交者名称， _git svn_ 将中止操作。然后，用户必须添加适当的条目。修改 authors 文件后重新运行以前的 _git svn_ 命令应继续操作。

```
config key: svn.authorsfile
```

```
 --authors-prog=<filename> 
```

如果指定了此选项，则对于 authors 文件中不存在的每个 SVN 提交者名称，将使用提交者名称作为第一个参数执行给定文件。该程序预计将返回“Name&lt; email&gt;”形式的单行或“名称&lt;&gt;”，将被视为包含在作者文件中。

由于历史原因，首先相对于 _init_ 和 _clone_ 的当前目录搜索相对 _ 文件名 _ 并且相对于 _ 的工作树的根目录获取 _。如果找不到 _ 文件名 _，则会像 _$ PATH_ 中的任何其他命令一样进行搜索。

```
config key: svn.authorsProg
```

```
 -q 
```

```
 --quiet 
```

使 _git svn_ 不那么冗长。再次指定使其更简洁。

```
 -m 
```

```
 --merge 
```

```
 -s<strategy> 
```

```
 --strategy=<strategy> 
```

```
 -p 
```

```
 --preserve-merges 
```

这些仅用于 _dcommit_ 和 _rebase_ 命令。

当使用 _dcommit_ 时，如果不能使用 _git reset_ ，则直接传递给 _git rebase_ （参见 _dcommit_ ）。

```
 -n 
```

```
 --dry-run 
```

这可以与 _dcommit_ ， _rebase_ ，_ 分支 _ 和 _ 标签 _ 命令一起使用。

对于 _dcommit_ ，打印出一系列 Git 参数，这些参数将显示哪些差异将被提交给 SVN。

对于 _rebase_ ，显示与当前分支关联的上游 svn 存储库关联的本地分支以及将从中获取的 svn 存储库的 URL。

对于 _ 分支 _ 和 _ 标签 _，显示创建分支或标记时将用于复制的 URL。

```
 --use-log-author 
```

当检索 svn 提交到 Git 中时（作为 _fetch_ ， _rebase_ 或 _dcommit_ 操作）的一部分，查找第一个`From:`或`Signed-off-by:`行日志消息并将其用作作者字符串。

```
config key: svn.useLogAuthor
```

```
 --add-author-from 
```

当从 Git 提交 svn 时（作为 _set-tree_ 或 _dcommit_ 操作的一部分），如果现有的日志消息还没有`From:`或`Signed-off-by:`行，根据 Git 提交的作者字符串附加`From:`行。如果您使用此，则`--use-log-author`将为所有提交检索有效的作者字符串。

```
config key: svn.addAuthorFrom
```

## 高级选项

```
 -i<GIT_SVN_ID> 
```

```
 --id <GIT_SVN_ID> 
```

这设置了 GIT_SVN_ID（而不是使用环境）。这允许用户在跟踪单个 URL 时覆盖默认的 refname 以从中获取。 _log_ 和 _dcommit_ 命令不再需要此开关作为参数。

```
 -R<remote name> 
```

```
 --svn-remote <remote name> 
```

指定要使用的[svn-remote“&lt; remote name&gt;”]部分，这允许跟踪 SVN 多个存储库。默认值：“svn”

```
 --follow-parent 
```

此选项仅在我们跟踪分支时使用（使用其中一个存储库布局选项--trunk， - label， - blank， - stdlayout）。对于每个跟踪的分支，尝试找出其修订版本的位置，并在分支的第一个 Git 提交中设置合适的父代。当我们跟踪已在存储库中移动的目录时，这尤其有用。如果禁用此功能， _git svn_ 创建的分支将全部为线性且不共享任何历史记录，这意味着没有关于分支分支或合并的信息。但是，长时间/错综复杂的历史记录可能需要很长时间，因此禁用此功能可能会加快克隆过程。默认情况下启用此功能，使用--no-follow-parent 禁用它。

```
config key: svn.followparent
```

## 配置文件选项

```
 svn.noMetadata 
```

```
 svn-remote.<name>.noMetadata 
```

这会在每次提交结束时删除 _git-svn-id：_ 行。

此选项只能用于一次性导入，因为 _git svn_ 无法在没有元数据的情况下再次获取。另外，如果你丢失 _$ GIT_DIR / svn / * \ * /。rev_map。*_ 文件， _git svn_ 将无法重建它们。

_git svn log_ 命令也不能在使用它的存储库上工作。使用这与 _useSvmProps_ 选项冲突（希望）显而易见的原因。

建议不要使用此选项，因为这样很难在现有文档，错误报告和存档中跟踪对 SVN 修订号的旧引用。如果您计划最终从 SVN 迁移到 Git 并确定要删除 SVN 历史记录，请考虑 [git-filter-branch [1]](https://git-scm.com/docs/git-filter-branch) 。 filter-branch 还允许重新格式化元数据，以便于阅读和重写非“svn.authorsFile”用户的作者信息。

```
 svn.useSvmProps 
```

```
 svn-remote.<name>.useSvmProps 
```

这允许 _git svn_ 从使用 SVN :: Mirror（或 svk）为元数据创建的镜像重新映射存储库 URL 和 UUID。

如果 SVN 修订版具有属性“svm：headrev”，则修订版很可能是由 SVN :: Mirror 创建的（也是 SVK 使用的）。该属性包含存储库 UUID 和修订版。我们希望看起来像是在镜像原始 URL，因此引入一个辅助函数，它返回原始标识 URL 和 UUID，并在提交消息中生成元数据时使用它。

```
 svn.useSvnsyncProps 
```

```
 svn-remote.<name>.useSvnsyncprops 
```

与 useSvmProps 选项类似;这适用于随 SVN 1.4.x 及更高版本一起发布的 svnsync（1）命令的用户。

```
 svn-remote.<name>.rewriteRoot 
```

这允许用户从备用 URL 创建存储库。例如，管理员可以在本地服务器上运行 _git svn_ （通过 file：// 访问）但希望使用公共 http：//或 svn：/分发存储库/元数据中的 URL，以便用户看到公共 URL。

```
 svn-remote.<name>.rewriteUUID 
```

与 useSvmProps 选项类似;这适用于需要手动重新映射 UUID 的用户。在通过 useSvmProps 或 useSvnsyncProps 无法使用原始 UUID 的情况下，这可能很有用。

```
 svn-remote.<name>.pushurl 
```

与 Git 的`remote.&lt;name&gt;.pushurl`类似，此密钥设计用于 _url_ 通过只读传输指向 SVN 存储库的情况，以提供备用读/写传输。假设两个键都指向同一个存储库。与 _commiturl_ 不同， _pushurl_ 是基本路径。如果可以使用 _commiturl_ 或 _pushurl_ ， _commiturl_ 优先。

```
 svn.brokenSymlinkWorkaround 
```

这会禁用可能昂贵的检查，以解决由损坏的客户端检入 SVN 的损坏的符号链接。如果跟踪具有许多非符号链接的空 blob 的 SVN 存储库，请将此选项设置为“false”。当 _git svn_ 正在运行时，此选项可能会更改，并在下一个修订版本生效时生效。如果未设置， _git svn_ 假定此选项为“true”。

```
 svn.pathnameencoding 
```

这指示 git svn 将路径名重新编码为给定的编码。它可以被 Windows 用户和非 utf8 语言环境中的用户使用，以避免使用非 ASCII 字符损坏文件名。有效编码是 Perl 的 Encode 模块支持的编码。

```
 svn-remote.<name>.automkdirs 
```

通常，“git svn clone”和“git svn rebase”命令会尝试重新创建 Subversion 存储库中的空目录。如果此选项设置为“false”，则只有在显式运行“git svn mkdirs”命令时才会创建空目录。如果未设置， _git svn_ 假定此选项为“true”。

由于 noMetadata，rewriteRoot，rewriteUUID，useSvnsyncProps 和 useSvmProps 选项都会影响 _git svn_ 生成和使用的元数据;在导入任何历史记录之前，它们**必须在配置文件中设置**，并且一旦设置这些设置就不应该更改。

此外，每个 svn-remote 部分只能使用其中一个选项，因为它们会影响 _git-svn-id：_ 元数据行，但可以一起使用的 rewriteRoot 和 rewriteUUID 除外。

## 基本实例

跟踪并贡献 Subversion 管理项目的主干（忽略标签和分支）：

```
# Clone a repo (like git clone):
	git svn clone http://svn.example.com/project/trunk
# Enter the newly cloned directory:
	cd trunk
# You should be on master branch, double-check with 'git branch'
	git branch
# Do some work and commit locally to Git:
	git commit ...
# Something is committed to SVN, rebase your local changes against the
# latest changes in SVN:
	git svn rebase
# Now commit your changes (that were committed previously using Git) to SVN,
# as well as automatically updating your working HEAD:
	git svn dcommit
# Append svn:ignore settings to the default Git exclude file:
	git svn show-ignore >> .git/info/exclude
```

跟踪并贡献整个 Subversion 管理的项目（包括主干，标签和分支）：

```
# Clone a repo with standard SVN directory layout (like git clone):
	git svn clone http://svn.example.com/project --stdlayout --prefix svn/
# Or, if the repo uses a non-standard directory layout:
	git svn clone http://svn.example.com/project -T tr -b branch -t tag --prefix svn/
# View all branches and tags you have cloned:
	git branch -r
# Create a new branch in SVN
	git svn branch waldo
# Reset your master to trunk (or any other branch, replacing 'trunk'
# with the appropriate name):
	git reset --hard svn/trunk
# You may only dcommit to one branch/tag/trunk at a time.  The usage
# of dcommit/rebase/show-ignore should be the same as above.
```

最初的 _git svn clone_ 可能非常耗时（特别是对于大型 Subversion 存储库）。如果多个人（或一个拥有多台机器的人）想要使用 _git svn_ 与同一个 Subversion 存储库进行交互，您可以将初始 _git svn clone_ 作为服务器上的存储库让每个人用 _git clone_ 克隆该存储库：

```
# Do the initial import on a server
	ssh server "cd /pub && git svn clone http://svn.example.com/project [options...]"
# Clone locally - make sure the refs/remotes/ space matches the server
	mkdir project
	cd project
	git init
	git remote add origin server:/pub/project
	git config --replace-all remote.origin.fetch '+refs/remotes/*:refs/remotes/*'
	git fetch
# Prevent fetch/pull from remote Git server in the future,
# we only want to use git svn for future updates
	git config --remove-section remote.origin
# Create a local branch from one of the branches just fetched
	git checkout -b master FETCH_HEAD
# Initialize 'git svn' locally (be sure to use the same URL and
# --stdlayout/-T/-b/-t/--prefix options as were used on server)
	git svn init http://svn.example.com/project [options...]
# Pull the latest changes from Subversion
	git svn rebase
```

## REBASE VS. PULL / MERGE

更喜欢使用 _git svn rebase_ 或 _git rebase_ ，而不是 _git pull_ 或 _git merge_ 来同步未整合的提交与 _git svn_ 分支。这样做将使未集成提交的历史相对于上游 SVN 存储库保持线性，并允许使用首选 _git svn dcommit_ 子命令将未集成的提交推送回 SVN。

最初， _git svn_ 建议开发人员从 _git svn_ 分支中撤出或合并。这是因为作者赞成`git svn set-tree B`提交单个头而不是`git svn set-tree A..B`符号来提交多个提交。使用 _git pull_ 或 _git merge_ 和`git svn set-tree A..B`将导致非线性历史记录在提交到 SVN 时被平铺，这可能导致合并提交意外地反转 SVN 中的先前提交。

## 合并跟踪

虽然 _git svn_ 可以跟踪采用标准布局的存储库的复制历史记录（包括分支和标记），但它还不能代表 git 内部发生在 SVN 用户上游的合并历史记录。因此，建议用户在 Git 内尽可能保持历史记录，以便于与 SVN 兼容（参见下面的 CAVEATS 部分）。

## 处理 SVN 分支机构

如果 _git svn_ 配置为获取分支（并且--follow-branches 有效），它有时会为一个 SVN 分支创建多个 Git 分支，其中附加分支的名称为 _branchname @nnn_ （nnn 是 SVN 版本号）。如果 _git svn_ 无法在 SVN 分支中找到第一次提交的父提交，则将分支连接到其他分支的历史记录，从而创建这些附加分支。

通常，SVN 分支中的第一次提交包括复制操作。 _git svn_ 将读取此提交以获取创建分支的 SVN 修订版。然后，它将尝试查找与此 SVN 修订版对应的 Git 提交，并将其用作分支的父级。但是，可能没有合适的 Git 提交作为父级。除其他原因外，如果 SVN 分支是 _git svn_ 未提取的修订版本的副本（例如因为它是`--revision`跳过的旧版本），或者如果在 SVN 中，复制了一个未被 _git svn_ 跟踪的目录（例如根本没有跟踪的分支，或被跟踪分支的子目录）。在这些情况下， _git svn_ 仍然会创建一个 Git 分支，但它不会使用现有的 Git 提交作为分支的父级，而是会读取分支从中复制的目录的 SVN 历史记录并创建适当的 Git 提交。这由消息“初始化父：&lt; branchname&gt;”表示。

此外，它将创建一个名为 _&lt; branchname&gt; @&lt; SVN-Revision&gt;的特殊分支。_ ，其中&lt; SVN-Revision&gt;是从中复制分支的 SVN 修订版号。该分支将指向新创建的分支的父提交。如果在 SVN 中分支被删除并且稍后从不同版本重新创建，则将存在多个具有 _@_ 的分支。

请注意，这可能意味着为单个 SVN 修订创建了多个 Git 提交。

例如：在具有标准中继/标签/分支布局的 SVN 存储库中，在 r.100 中创建目录中继/子。在 r.200 中，trunk / sub 通过将其复制到 branches /来分支。 _git svn clone -s_ 然后会创建一个分支 _sub_ 。它还将为 r.100 到 r.199 创建新的 Git 提交，并将它们用作分支 _sub_ 的历史。因此，从 r.100 到 r.199 的每个修订版将有两个 Git 提交（一个包含 trunk /，一个包含 trunk / sub /）。最后，它将创建一个分支 _sub @ 200_ ，指向分支 _sub_ 的新父提交（即 r.200 和 trunk / sub /的提交）。

## CAVEATS

为了简单和与 Subversion 互操作，建议所有 _git svn_ 用户直接从 SVN 服务器克隆，获取和提交，并避免所有 _git clone_ / _ 拉 _ / _ 合并 _ / _ 推送 _ Git 存储库和分支之间的操作。在 Git 分支和用户之间交换代码的推荐方法是 _git format-patch_ 和 _git am_ ，或者只是'dcommit'ing 到 SVN 存储库。

在您计划 _dcommit_ 的分支上不建议运行 _git merge_ 或 _git pull_ ，因为 Subversion 用户无法看到您所做的任何合并。此外，如果您从作为 SVN 分支镜像的 Git 分支合并或拉出， _dcommit_ 可能会提交错误的分支。

如果你合并，请注意以下规则： _git svn dcommit_ 将尝试在名为的 SVN 提交之上提交

```
git log --grep=^git-svn-id: --first-parent -1
```

你 _ 必须 _ 因此确保你想要提交的分支的最新提交是合并的 _ 第一 _ 父。否则会发生混乱，特别是如果第一个父级是同一 SVN 分支上的较旧提交。

_git clone_ 不会在 refs / remotes / hierarchy 或任何 _git svn_ 元数据或 config 下克隆分支。所以使用 _git svn_ 创建和管理的存储库应该使用 _rsync_ 进行克隆，如果要完成克隆的话。

由于 _dcommit_ 内部使用 rebase，任何 Git 分支你 _git push_ 到 _dcommit_ 之前将需要强制覆盖远程存储库上的现有 ref。这通常被认为是不好的做法，详见 [git-push [1]](https://git-scm.com/docs/git-push) 文档。

对于您已经提交的更改，请勿使用 [git-commit [1]](https://git-scm.com/docs/git-commit) 的--amend 选项。将 - 已经推送到其他用户的远程存储库提交的提交视为不好的做法，并且与 SVN 的命令类似于此。

克隆 SVN 存储库时，如果没有使用描述存储库布局的选项（--trunk， - targs， - .branches， - stdlayout）， _git svn clone_ 将创建一个 Git 存储库具有完全线性历史记录，其中分支和标记在工作副本中显示为单独的目录。虽然这是获取完整存储库副本的最简单方法，但对于具有多个分支的项目，它将导致工作副本比主干大许多倍。因此，对于使用标准目录结构（主干/分支/标签）的项目，建议使用选项`--stdlayout`进行克隆。如果项目使用非标准结构，和/或不需要分支和标记，则最简单的方法是仅克隆一个目录（通常是主干），而不提供任何存储库布局选项。如果需要带分支和标签的完整历史记录，则必须使用选项`--trunk` / `--branches` / `--tags`。

当使用多个 - 分支或--tags 时， _git svn_ 不会自动处理名称冲突（例如，如果来自不同路径的两个分支具有相同的名称，或者分支和标记具有相同的名称冲突名称）。在这些情况下，使用 _init_ 设置你的 Git 存储库然后，在你的第一个 _fetch_ 之前，编辑$ GIT_DIR / config 文件，以便分支和标签与不同的名称空间相关联。例如：

```
branches = stable/*:refs/remotes/svn/stable/*
branches = debug/*:refs/remotes/svn/debug/*
```

## BUGS

我们忽略除 svn：executable 之外的所有 SVN 属性。任何未处理的属性都会记录到$ GIT_DIR / svn /&lt; refname&gt; /unhandled.log

Git 未检测到重命名和复制的目录，因此在提交 SVN 时不会进行跟踪。我不打算为此添加支持，因为为所有可能的极端情况工作是非常困难和耗时的（Git 也没有这样做）。如果它们足够相似，Git 可以检测它们，则完全支持提交重命名和复制的文件。

在 SVN 中，可以（虽然不鼓励）提交对标记的更改（因为标记只是目录副本，因此在技术上与分支相同）。克隆 SVN 存储库时， _git svn_ 无法知道将来是否会发生对标记的提交。因此它保守地运作并将所有 SVN 标签作为分支导入，在标签名称前加上 _ 标签/_ 。

## 组态

_git svn_ 将[svn-remote]配置信息存储在存储库$ GIT_DIR / config 文件中。它类似于核心 Git [remote]部分，除了 _fetch_ 键不接受 glob 参数;但它们由 _ 分支 _ 和 _ 标签 _ 键处理。由于某些 SVN 存储库奇怪地配置了多个项目，因此允许使用以下列出的扩展：

```
[svn-remote "project-a"]
	url = http://server.org/svn
	fetch = trunk/project-a:refs/remotes/project-a/trunk
	branches = branches/*/project-a:refs/remotes/project-a/branches/*
	branches = branches/release_*:refs/remotes/project-a/branches/release_*
	branches = branches/re*se:refs/remotes/project-a/branches/*
	tags = tags/*/project-a:refs/remotes/project-a/tags/*
```

请记住，本地参考的 _*_ （星号）通配符（_：_ 的右侧）*必须*是最右边的路径组件;但是远程通配符可以是任何地方，只要它是一个独立的路径组件（由 _/_ 或 EOL 包围）。这种类型的配置不是由 _init_ 自动创建的，应该使用文本编辑器或使用 _git config_ 手动输入。

另请注意，每个单词只允许使用一个星号。例如：

```
branches = branches/re*se:refs/remotes/project-a/branches/*
```

将匹配分支 _ 释放 _，_ 研究 _， _re123se_ ，但

```
branches = branches/re*s*e:refs/remotes/project-a/branches/*
```

会产生错误。

也可以通过在大括号内使用逗号分隔的名称列表来获取分支或标记的子集。例如：

```
[svn-remote "huge-project"]
	url = http://server.org/svn
	fetch = trunk/src:refs/remotes/trunk
	branches = branches/{red,green}/src:refs/remotes/project-a/branches/*
	tags = tags/{1.0,2.0}/src:refs/remotes/project-a/tags/*
```

支持多个提取，分支和标记键：

```
[svn-remote "messy-repo"]
	url = http://server.org/svn
	fetch = trunk/project-a:refs/remotes/project-a/trunk
	fetch = branches/demos/june-project-a-demo:refs/remotes/project-a/demos/june-demo
	branches = branches/server/*:refs/remotes/project-a/branches/*
	branches = branches/demos/2011/*:refs/remotes/project-a/2011-demos/*
	tags = tags/server/*:refs/remotes/project-a/tags/*
```

在这样的配置中创建分支需要使用-d 或--destination 标志消除使用哪个位置的歧义：

```
$ git svn branch -d branches/server release-2-3-0
```

请注意，git-svn 会跟踪分支或标记出现的最高版本。如果在获取后更改了分支或标记的子集，则必须手动编辑$ GIT_DIR / svn / .metadata 以根据需要删除（或重置）branches-maxRev 和/或 tags-maxRev。

## FILES

```
 $GIT_DIR/svn/*\*/.rev_map.* 
```

Subversion 修订号和 Git 提交名之间的映射。在未设置 noMetadata 选项的存储库中，可以从每次提交结束时的 git-svn-id：行重建（有关详细信息，请参阅上面的 _svn.noMetadata_ 部分）。

_git svn fetch_ 和 _git svn rebase_ 会自动更新 rev_map，如果它丢失或不是最新的。 _git svn reset_ 自动倒带。

## 也可以看看

[git-rebase [1]](https://git-scm.com/docs/git-rebase)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件