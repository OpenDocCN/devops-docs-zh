# git

> 原文： [`git-scm.com/docs/git`](https://git-scm.com/docs/git)

## 名称

git - 愚蠢的内容跟踪器

## 概要

```
git [--version] [--help] [-C <path>] [-c <name>=<value>]
    [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
    [-p|--paginate|-P|--no-pager] [--no-replace-objects] [--bare]
    [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
    [--super-prefix=<path>]
    <command> [<args>]
```

## 描述

Git 是一个快速，可扩展的分布式版本控制系统，具有异常丰富的命令集，可提供高级操作和对内部的完全访问。

参见 [gittutorial [7]](https://git-scm.com/docs/gittutorial) 开始，然后参见 [giteveryday [7]](https://git-scm.com/docs/giteveryday) 获取有用的最小命令集。 Git 用户手册有更深入的介绍。

掌握了基本概念后，您可以回到此页面了解 Git 提供的命令。您可以使用“git help command”了解有关各个 Git 命令的更多信息。 [gitcli [7]](https://git-scm.com/docs/gitcli) 手册页概述了命令行命令语法。

可以在`https://git.github.io/htmldocs/git.html`查看最新 Git 文档的格式化和超链接副本。

## OPTIONS

```
 --version 
```

打印 _git_ 程序来自的 Git 套件版本。

```
 --help 
```

打印概要和最常用命令的列表。如果给出选项`--all`或`-a`，则打印所有可用命令。如果命名了 Git 命令，则此选项将显示该命令的手册页。

其他选项可用于控制手册页的显示方式。有关详细信息，请参阅 [git-help [1]](https://git-scm.com/docs/git-help) ，因为`git --help ...`内部转换为`git help ...`。

```
 -C <path> 
```

如同在 _&lt; path&gt;中启动 git 一样运行 _ 而不是当前的工作目录。当给出多个`-C`选项时，相对于前一个`-C &lt;path&gt;`解释每个后续的非绝对`-C &lt;path&gt;`。

此选项会影响期望路径名称的选项，如`--git-dir`和`--work-tree`，因为它们对路径名的解释将相对于`-C`选项导致的工作目录。例如，以下调用是等效的：

```
git --git-dir=a.git --work-tree=b -C c status
git --git-dir=c/a.git --work-tree=c/b status
```

```
 -c <name>=<value> 
```

将配置参数传递给命令。给定的值将覆盖配置文件中的值。 &lt; name&gt;预期格式与 _git config_ （由点分隔的子键）列出的格式相同。

注意，允许省略`git -c foo.bar ...`中的`=`并将`foo.bar`设置为布尔值 true（就像配置文件中的`[foo]bar`一样）。包括等于但是空值（如`git -c foo.bar= ...`）将`foo.bar`设置为`git config --type=bool`将转换为`false`的空字符串。

```
 --exec-path[=<path>] 
```

安装核心 Git 程序的路径。这也可以通过设置 GIT_EXEC_PATH 环境变量来控制。如果没有给出路径， _git_ 将打印当前设置然后退出。

```
 --html-path 
```

打印路径，不带斜杠，安装 Git 的 HTML 文档并退出。

```
 --man-path 
```

打印 manpath（参见`man(1)`）获取此版本 Git 的手册页并退出。

```
 --info-path 
```

打印安装记录此版本 Git 的 Info 文件的路径并退出。

```
 -p 
```

```
 --paginate 
```

如果标准输出是终端，则将所有输出管道输入 _ 减去 _（或如果设置为$ PAGER）。这将覆盖`pager.&lt;cmd&gt;`配置选项（请参阅下面的“配置机制”部分）。

```
 -P 
```

```
 --no-pager 
```

不要将 Git 输出传输到寻呼机。

```
 --git-dir=<path> 
```

设置存储库的路径。这也可以通过设置`GIT_DIR`环境变量来控制。它可以是当前工作目录的绝对路径或相对路径。

```
 --work-tree=<path> 
```

设置工作树的路径。它可以是绝对路径或相对于当前工作目录的路径。这也可以通过设置 GIT_WORK_TREE 环境变量和 core.worktree 配置变量来控制（有关更详细的讨论，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 中的 core.worktree）。

```
 --namespace=<path> 
```

设置 Git 名称空间。有关详细信息，请参阅 [gitnamespaces [7]](https://git-scm.com/docs/gitnamespaces) 。相当于设置`GIT_NAMESPACE`环境变量。

```
 --super-prefix=<path> 
```

目前仅供内部使用。设置一个前缀，该前缀提供从存储库上方到其根目录的路径。一个用途是给出调用它的超级项目的子模块上下文。

```
 --bare 
```

将存储库视为裸存储库。如果未设置 GIT_DIR 环境，则将其设置为当前工作目录。

```
 --no-replace-objects 
```

不要使用替换引用来替换 Git 对象。有关详细信息，请参阅 [git-replace [1]](https://git-scm.com/docs/git-replace) 。

```
 --literal-pathspecs 
```

按字面意思处理 pathspecs（即没有 globbing，没有 pathspec magic）。这相当于将`GIT_LITERAL_PATHSPECS`环境变量设置为`1`。

```
 --glob-pathspecs 
```

为所有 pathspec 添加“glob”魔法。这相当于将`GIT_GLOB_PATHSPECS`环境变量设置为`1`。可以使用 pathspec magic“:( literal）”在各个 pathspec 上禁用通配符

```
 --noglob-pathspecs 
```

为所有 pathspec 添加“literal”魔法。这相当于将`GIT_NOGLOB_PATHSPECS`环境变量设置为`1`。可以使用 pathspec magic“:( glob）”在各个 pathspec 上启用 globbing

```
 --icase-pathspecs 
```

为所有 pathspec 添加“icase”魔法。这相当于将`GIT_ICASE_PATHSPECS`环境变量设置为`1`。

```
 --no-optional-locks 
```

不要执行需要锁定的可选操作。这相当于将`GIT_OPTIONAL_LOCKS`设置为`0`。

```
 --list-cmds=group[,group…​] 
```

按组列出命令。这是一个内部/实验选项，可能会在将来更改或删除。支持的组包括：builtins，parseopt（使用 parse-options 的内置命令），main（libexec 目录中的所有命令），其他（`$PATH`中具有 git-前缀的所有其他命令），list-&lt; category&gt; （请参阅 command-list.txt 中的类别），nohelpers（排除帮助程序命令），别名和配置（从配置变量 completion.commands 检索命令列表）

## GIT 命令

我们将 Git 分为高级（“瓷器”）命令和低级（“管道”）命令。

## 高级命令（瓷器）

我们将瓷器命令分成主命令和一些辅助用户实用程序。

### 主要瓷器命令

```
 git-add[1] 
```

将文件内容添加到索引

```
 git-am[1] 
```

从邮箱中应用一系列修补程序

```
 git-archive[1] 
```

从命名树创建文件存档

```
 git-bisect[1] 
```

使用二进制搜索来查找引入错误的提交

```
 git-branch[1] 
```

列出，创建或删除分支

```
 git-bundle[1] 
```

通过存档移动对象和引用

```
 git-checkout[1] 
```

切换分支或恢复工作树文件

```
 git-cherry-pick[1] 
```

应用某些现有提交引入的更改

```
 git-citool[1] 
```

git-commit 的图形替代方案

```
 git-clean[1] 
```

从工作树中删除未跟踪的文件

```
 git-clone[1] 
```

将存储库克隆到新目录中

```
 git-commit[1] 
```

记录对存储库的更改

```
 git-describe[1] 
```

根据可用的 ref 给对象一个人类可读的名称

```
 git-diff[1] 
```

显示提交，提交和工作树等之间的更改

```
 git-fetch[1] 
```

从另一个存储库下载对象和引用

```
 git-format-patch[1] 
```

准备电子邮件提交补丁

```
 git-gc[1] 
```

清理不必要的文件并优化本地存储库

```
 git-grep[1] 
```

打印与图案匹配的线条

```
 git-gui[1] 
```

Git 的便携式图形界面

```
 git-init[1] 
```

创建一个空的 Git 存储库或重新初始化现有存储库

```
 gitk[1] 
```

Git 存储库浏览器

```
 git-log[1] 
```

显示提交日志

```
 git-merge[1] 
```

一起加入两个或多个开发历史

```
 git-mv[1] 
```

移动或重命名文件，目录或符号链接

```
 git-notes[1] 
```

添加或检查对象注释

```
 git-pull[1] 
```

从另一个存储库或本地分支获取并与其集成

```
 git-push[1] 
```

更新远程引用以及关联的对象

```
 git-range-diff[1] 
```

比较两个提交范围（例如，分支的两个版本）

```
 git-rebase[1] 
```

在另一个基本提示之上重新应用提交

```
 git-reset[1] 
```

将当前 HEAD 重置为指定状态

```
 git-revert[1] 
```

还原一些现有提交

```
 git-rm[1] 
```

从工作树和索引中删除文件

```
 git-shortlog[1] 
```

总结 _git log_ 输出

```
 git-show[1] 
```

显示各种类型的对象

```
 git-stash[1] 
```

将更改存储在脏工作目录中

```
 git-status[1] 
```

显示工作树状态

```
 git-submodule[1] 
```

初始化，更新或检查子模块

```
 git-tag[1] 
```

创建，列出，删除或验证使用 GPG 签名的标记对象

```
 git-worktree[1] 
```

管理多个工作树

### 辅助命令

机器人：

```
 git-config[1] 
```

获取并设置存储库或全局选项

```
 git-fast-export[1] 
```

Git 数据导出器

```
 git-fast-import[1] 
```

快速 Git 数据导入器的后端

```
 git-filter-branch[1] 
```

重写分支

```
 git-mergetool[1] 
```

运行合并冲突解决工具以解决合并冲突

```
 git-pack-refs[1] 
```

打包头和标签以实现高效的存储库访问

```
 git-prune[1] 
```

从对象数据库中修剪所有无法访问的对象

```
 git-reflog[1] 
```

管理 reflog 信息

```
 git-remote[1] 
```

管理一组跟踪的存储库

```
 git-repack[1] 
```

在存储库中打包解压缩的对象

```
 git-replace[1] 
```

创建，列出，删除引用以替换对象

读写器：

```
 git-annotate[1] 
```

使用提交信息注释文件行

```
 git-blame[1] 
```

显示修订版和作者上次修改文件的每一行

```
 git-count-objects[1] 
```

计算解压缩的对象数量及其磁盘消耗量

```
 git-difftool[1] 
```

使用常见差异工具显示更改

```
 git-fsck[1] 
```

验证数据库中对象的连接性和有效性

```
 git-help[1] 
```

显示有关 Git 的帮助信息

```
 git-instaweb[1] 
```

立即在 gitweb 中浏览您的工作存储库

```
 git-merge-tree[1] 
```

显示三向合并而不触及索引

```
 git-rerere[1] 
```

重用已记录的冲突合并解决方案

```
 git-show-branch[1] 
```

显示分支及其提交

```
 git-verify-commit[1] 
```

检查提交的 GPG 签名

```
 git-verify-tag[1] 
```

检查标签的 GPG 签名

```
 gitweb[1] 
```

Git Web 界面（Web 前端到 Git 存储库）

```
 git-whatchanged[1] 
```

显示每个提交引入的差异日志

### 与他人互动

这些命令通过电子邮件补丁与外部 SCM 和其他人进行交互。

```
 git-archimport[1] 
```

将 GNU Arch 存储库导入 Git

```
 git-cvsexportcommit[1] 
```

将单个提交导出到 CVS 结帐

```
 git-cvsimport[1] 
```

从另一个喜欢讨厌的 SCM 中抢救你的数据

```
 git-cvsserver[1] 
```

Git 的 CVS 服务器模拟器

```
 git-imap-send[1] 
```

将 stdin 的补丁集合发送到 IMAP 文件夹

```
 git-p4[1] 
```

从 Perforce 存储库导入并提交到 Perforce 存储库

```
 git-quiltimport[1] 
```

将 quilt 补丁集应用于当前分支

```
 git-request-pull[1] 
```

生成待处理更改的摘要

```
 git-send-email[1] 
```

将一组补丁作为电子邮件发送

```
 git-svn[1] 
```

Subversion 存储库和 Git 之间的双向操作

## 低级命令（管道）

虽然 Git 包含自己的瓷层，但它的低级命令足以支持替代瓷器的开发。这些瓷器的开发者可能首先阅读 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 和 [git-read-tree [1]](https://git-scm.com/docs/git-read-tree) 。

这些低级命令的接口（输入，输出，选项集和语义）比 Porcelain 级别命令更稳定，因为这些命令主要用于脚本使用。另一方面，Porcelain 命令的界面可能会发生变化，以改善最终用户体验。

以下描述将低级命令划分为操作对象（在存储库，索引和工作树中）的命令，询问和比较对象的命令，以及在存储库之间移动对象和引用的命令。

### 操纵命令

```
 git-apply[1] 
```

将修补程序应用于文件和/或索引

```
 git-checkout-index[1] 
```

将文件从索引复制到工作树

```
 git-commit-graph[1] 
```

编写并验证 Git 提交图文件

```
 git-commit-tree[1] 
```

创建一个新的提交对象

```
 git-hash-object[1] 
```

计算对象 ID 并可选择从文件创建 blob

```
 git-index-pack[1] 
```

构建现有打包存档的包索引文件

```
 git-merge-file[1] 
```

运行三向文件合并

```
 git-merge-index[1] 
```

为需要合并的文件运行合并

```
 git-multi-pack-index[1] 
```

编写并验证多包索引

```
 git-mktag[1] 
```

创建标记对象

```
 git-mktree[1] 
```

从 ls-tree 格式的文本构建树对象

```
 git-pack-objects[1] 
```

创建对象的打包存档

```
 git-prune-packed[1] 
```

删除包文件中已有的额外对象

```
 git-read-tree[1] 
```

将树信息读入索引

```
 git-symbolic-ref[1] 
```

阅读，修改和删除符号引用

```
 git-unpack-objects[1] 
```

从打包存档中解压缩对象

```
 git-update-index[1] 
```

将工作树中的文件内容注册到索引

```
 git-update-ref[1] 
```

安全地更新存储在 ref 中的对象名称

```
 git-write-tree[1] 
```

从当前索引创建树对象

### 询问命令

```
 git-cat-file[1] 
```

提供存储库对象的内容或类型和大小信息

```
 git-cherry[1] 
```

查找尚未应用于上游的提交

```
 git-diff-files[1] 
```

比较工作树和索引中的文件

```
 git-diff-index[1] 
```

将树与工作树或索引进行比较

```
 git-diff-tree[1] 
```

比较通过两个树对象找到的 blob 的内容和模式

```
 git-for-each-ref[1] 
```

每个参考的输出信息

```
 git-get-tar-commit-id[1] 
```

从使用 git-archive 创建的存档中提取提交 ID

```
 git-ls-files[1] 
```

显示有关索引和工作树中文件的信息

```
 git-ls-remote[1] 
```

列出远程存储库中的引用

```
 git-ls-tree[1] 
```

列出树对象的内容

```
 git-merge-base[1] 
```

找到合并的尽可能好的共同祖先

```
 git-name-rev[1] 
```

查找给定转速的符号名称

```
 git-pack-redundant[1] 
```

查找冗余包文件

```
 git-rev-list[1] 
```

列出以反向时间顺序提交对象

```
 git-rev-parse[1] 
```

挑选和按摩参数

```
 git-show-index[1] 
```

显示打包归档索引

```
 git-show-ref[1] 
```

列出本地存储库中的引用

```
 git-unpack-file[1] 
```

创建一个包含 blob 内容的临时文件

```
 git-var[1] 
```

显示 Git 逻辑变量

```
 git-verify-pack[1] 
```

验证打包的 Git 存档文件

通常，询问命令不会触及工作树中的文件。

### 同步存储库

```
 git-daemon[1] 
```

Git 存储库的一个非常简单的服务器

```
 git-fetch-pack[1] 
```

从另一个存储库接收丢失的对象

```
 git-http-backend[1] 
```

服务器端实现 Git over HTTP

```
 git-send-pack[1] 
```

通过 Git 协议将对象推送到另一个存储库

```
 git-update-server-info[1] 
```

更新辅助信息文件以帮助虚拟服务器

以下是上面使用的帮助程序命令;最终用户通常不直接使用它们。

```
 git-http-fetch[1] 
```

通过 HTTP 从远程 Git 存储库下载

```
 git-http-push[1] 
```

通过 HTTP / DAV 将对象推送到另一个存储库

```
 git-parse-remote[1] 
```

有助于解析远程存储库访问参数的例程

```
 git-receive-pack[1] 
```

接收推入存储库的内容

```
 git-shell[1] 
```

受限制的登录 shell 仅用于 Git SSH 访问

```
 git-upload-archive[1] 
```

将存档发送回 git-archive

```
 git-upload-pack[1] 
```

将对象打包回 git-fetch-pack

### 内部帮助器命令

这些是其他命令使用的内部帮助程序命令;最终用户通常不直接使用它们。

```
 git-check-attr[1] 
```

显示 gitattributes 信息

```
 git-check-ignore[1] 
```

调试 gitignore / exclude 文件

```
 git-check-mailmap[1] 
```

显示联系人的规范名称和电子邮件地址

```
 git-check-ref-format[1] 
```

确保参考名称格式正确

```
 git-column[1] 
```

以列显示数据

```
 git-credential[1] 
```

检索并存储用户凭据

```
 git-credential-cache[1] 
```

帮助程序将密码临时存储在内存中

```
 git-credential-store[1] 
```

帮助程序将凭据存储在磁盘上

```
 git-fmt-merge-msg[1] 
```

生成合并提交消息

```
 git-interpret-trailers[1] 
```

在提交消息中添加或解析结构化信息

```
 git-mailinfo[1] 
```

从单个电子邮件中提取补丁和作者身份

```
 git-mailsplit[1] 
```

简单的 UNIX mbox 拆分程序

```
 git-merge-one-file[1] 
```

与 git-merge-index 一起使用的标准帮助程序

```
 git-patch-id[1] 
```

计算修补程序的唯一 ID

```
 git-sh-i18n[1] 
```

Git 的 shell 脚本的 i18n 设置代码

```
 git-sh-setup[1] 
```

常见的 Git shell 脚本设置代码

```
 git-stripspace[1] 
```

删除不必要的空格

## 配置机制

Git 使用简单的文本格式来存储每个存储库和每个用户的自定义项。这样的配置文件可能如下所示：

```
#
# A '#' or ';' character indicates a comment.
#

; core variables
[core]
	; Don't trust file modes
	filemode = false

; user identity
[user]
	name = "Junio C Hamano"
	email = "gitster@pobox.com"
```

各种命令从配置文件中读取并相应地调整其操作。有关列表和有关配置机制的更多详细信息，请参见 [git-config [1]](https://git-scm.com/docs/git-config) 。

## 标识符术语

```
 <object> 
```

指示任何类型对象的对象名称。

```
 <blob> 
```

表示 blob 对象名称。

```
 <tree> 
```

表示树对象名称。

```
 <commit> 
```

表示提交对象名称。

```
 <tree-ish> 
```

表示树，提交或标记对象名称。采用&lt; tree-ish&gt;的命令参数最终想要在&lt;树&gt;上运行。对象但自动解除引用&lt; commit&gt;和&lt; tag&gt;指向&lt; tree&gt;的对象。

```
 <commit-ish> 
```

表示提交或标记对象名称。采用&lt; commit-ish&gt;的命令参数最终想要在&lt; commit&gt;上运行对象但自动解除引用&lt; tag&gt;指向&lt; commit&gt;的对象。

```
 <type> 
```

表示需要对象类型。目前之一：`blob`，`tree`，`commit`或`tag`。

```
 <file> 
```

表示文件名 - 几乎总是相对于树结构`GIT_INDEX_FILE`描述的根。

## 符号标识符

任何接受任何&lt; object&gt;的 Git 命令也可以使用以下符号表示法：

```
 HEAD 
```

表示当前分支的头部。

```
 <tag> 
```

有效标签 _ 名称 _（即`refs/tags/&lt;tag&gt;`参考）。

```
 <head> 
```

有效头 _ 名称 _（即`refs/heads/&lt;head&gt;`参考）。

有关拼写对象名称的更完整列表，请参阅 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 中的“指定修订”部分。

## 文件/目录结构

请参阅 [gitrepository-layout [5]](https://git-scm.com/docs/gitrepository-layout) 文档。

有关每个钩子的更多详细信息，请阅读 [githooks [5]](https://git-scm.com/docs/githooks) 。

更高级别的 SCM 可以在`$GIT_DIR`中提供和管理附加信息。

## 术语

请参阅 [gitglossary [7]](https://git-scm.com/docs/gitglossary) 。

## 环境变量

各种 Git 命令使用以下环境变量：

### Git 存储库

这些环境变量适用于 _ 所有 _ 核心 Git 命令。 Nb：值得注意的是，它们可能被 SCMS 上面的 GMS 使用/覆盖，所以如果使用外部前端则要小心。

```
 GIT_INDEX_FILE 
```

此环境允许指定备用索引文件。如果未指定，则使用`$GIT_DIR/index`的默认值。

```
 GIT_INDEX_VERSION 
```

此环境变量允许为新存储库指定索引版本。它不会影响现有的索引文件。默认情况下，使用索引文件版本 2 或 3。有关详细信息，请参阅 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 。

```
 GIT_OBJECT_DIRECTORY 
```

如果通过此环境变量指定了对象存储目录，则在下面创建 sha1 目录 - 否则使用默认的`$GIT_DIR/objects`目录。

```
 GIT_ALTERNATE_OBJECT_DIRECTORIES 
```

由于 Git 对象的不可变特性，旧对象可以存档到共享的只读目录中。此变量指定一个“：”分隔（在 Windows 上;“分隔”）Git 对象目录列表，可用于搜索 Git 对象。新对象不会写入这些目录。

以`"`（双引号）开头的条目将被解释为 C 风格的引用路径，删除前导和尾随双引号并尊重反斜杠转义。例如，值`"path-with-\"-and-:-in-it":vanilla-path`有两条路径：`path-with-"-and-:-in-it`和`vanilla-path`。

```
 GIT_DIR 
```

如果设置了`GIT_DIR`环境变量，则它指定要使用的路径而不是存储库基础的默认`.git`。 `--git-dir`命令行选项也设置此值。

```
 GIT_WORK_TREE 
```

设置工作树根目录的路径。这也可以通过`--work-tree`命令行选项和 core.worktree 配置变量来控制。

```
 GIT_NAMESPACE 
```

设置 Git 名称空间;有关详细信息，请参阅 [gitnamespaces [7]](https://git-scm.com/docs/gitnamespaces) 。 `--namespace`命令行选项也设置此值。

```
 GIT_CEILING_DIRECTORIES 
```

这应该是以冒号分隔的绝对路径列表。如果设置，它是 Git 在查找存储库目录时不应该进入的目录列表（对于排除缓慢加载的网络目录很有用）。它不会排除当前工作目录或在命令行或环境中设置的 GIT_DIR。通常，Git 必须读取此列表中的条目并解析可能存在的任何符号链接，以便将它们与当前目录进行比较。但是，如果即使这种访问速度很慢，您也可以在列表中添加一个空条目，告诉 Git 后续条目不是符号链接，无需解析;例如，`GIT_CEILING_DIRECTORIES=/maybe/symlink::/very/slow/non/symlink`。

```
 GIT_DISCOVERY_ACROSS_FILESYSTEM 
```

当在没有“.git”存储库目录的目录中运行时，Git 会尝试在父目录中找到这样的目录以查找工作树的顶部，但默认情况下它不会跨越文件系统边界。可以将此环境变量设置为 true，以告知 Git 不要停在文件系统边界。与`GIT_CEILING_DIRECTORIES`类似，这不会影响通过`GIT_DIR`或命令行设置的显式存储库目录。

```
 GIT_COMMON_DIR 
```

如果将此变量设置为路径，则通常在$ GIT_DIR 中的非工作树文件将从此路径中获取。特定于工作树的文件（如 HEAD 或索引）取自$ GIT_DIR。有关详细信息，请参阅 [gitrepository-layout [5]](https://git-scm.com/docs/gitrepository-layout) 和 [git-worktree [1]](https://git-scm.com/docs/git-worktree) 。此变量的优先级低于其他路径变量，例如 GIT_INDEX_FILE，GIT_OBJECT_DIRECTORY ......

### Git 承诺

```
 GIT_AUTHOR_NAME 
```

```
 GIT_AUTHOR_EMAIL 
```

```
 GIT_AUTHOR_DATE 
```

```
 GIT_COMMITTER_NAME 
```

```
 GIT_COMMITTER_EMAIL 
```

```
 GIT_COMMITTER_DATE 
```

```
 EMAIL 
```

见 [git-commit-tree [1]](https://git-scm.com/docs/git-commit-tree)

### Git Diffs

```
 GIT_DIFF_OPTS 
```

只有有效设置为“--unified = ??”或者“-u ??”设置创建统一差异时显示的上下文行数。这优先于 Git diff 命令行上传递的任何“-U”或“--unified”选项值。

```
 GIT_EXTERNAL_DIFF 
```

设置环境变量`GIT_EXTERNAL_DIFF`时，将调用由其命名的程序，而不是上面描述的 diff 调用。对于添加，删除或修改的路径，使用 7 个参数调用`GIT_EXTERNAL_DIFF`：

```
path old-file old-hex old-mode new-file new-hex new-mode
```

哪里：

```
 <old|new>-file 
```

是文件 GIT_EXTERNAL_DIFF 可用于读取&lt; old | new&gt;的内容，

```
 <old|new>-hex 
```

是 40-hexdigit SHA-1 哈希，

```
 <old|new>-mode 
```

是文件模式的八进制表示。

文件参数可以指向用户的工作文件（例如“git-diff-files”中的`new-file`），`/dev/null`（例如添加新文件时的`old-file`）或临时文件（例如`old-file`）在索引中）。 `GIT_EXTERNAL_DIFF`不应该担心取消链接临时文件---当`GIT_EXTERNAL_DIFF`退出时它会被删除。

对于未合并的路径，使用 1 参数&lt; path&gt;调用`GIT_EXTERNAL_DIFF`。

对于每个路径`GIT_EXTERNAL_DIFF`被调用，设置了两个环境变量`GIT_DIFF_PATH_COUNTER`和`GIT_DIFF_PATH_TOTAL`。

```
 GIT_DIFF_PATH_COUNTER 
```

每个路径的基于 1 的计数器递增 1。

```
 GIT_DIFF_PATH_TOTAL 
```

路径总数。

### 其他

```
 GIT_MERGE_VERBOSITY 
```

控制递归合并策略显示的输出量的数字。覆盖 merge.verbosity。见 [git-merge [1]](https://git-scm.com/docs/git-merge)

```
 GIT_PAGER 
```

此环境变量会覆盖`$PAGER`。如果将其设置为空字符串或值“cat”，则 Git 将不会启动寻呼机。另请参见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`core.pager`选项。

```
 GIT_EDITOR 
```

此环境变量会覆盖`$EDITOR`和`$VISUAL`。当在交互模式下启动编辑器时，它由几个 Git 命令使用。另请参阅 [git-var [1]](https://git-scm.com/docs/git-var) 和 [git-config [1]](https://git-scm.com/docs/git-config) 中的`core.editor`选项。

```
 GIT_SSH 
```

```
 GIT_SSH_COMMAND 
```

如果设置了这些环境变量中的任何一个，则当需要连接到远程系统时， _git fetch_ 和 _git push_ 将使用指定的命令而不是 _ssh_ 。传递给配置命令的命令行参数由 ssh 变量确定。有关详细信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 中的`ssh.variant`选项。

`$GIT_SSH_COMMAND`优先于`$GIT_SSH`，由 shell 解释，允许包含其他参数。另一方面，`$GIT_SSH`必须只是程序的路径（如果需要其他参数，则可以是包装器 shell 脚本）。

通常，通过个人`.ssh/config`文件更容易配置任何所需的选项。有关详细信息，请参阅 ssh 文档。

```
 GIT_SSH_VARIANT 
```

如果设置了此环境变量，它将覆盖 Git 的自动检测，无论`GIT_SSH` / `GIT_SSH_COMMAND` / `core.sshCommand`是指 OpenSSH，plink 还是 tortoiseplink。此变量会覆盖用于相同目的的配置设置`ssh.variant`。

```
 GIT_ASKPASS 
```

如果设置了此环境变量，则需要获取密码或密码短语（例如，用于 HTTP 或 IMAP 身份验证）的 Git 命令将使用适当的提示作为命令行参数调用此程序，并从其 STDOUT 读取密码。另请参见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`core.askPass`选项。

```
 GIT_TERMINAL_PROMPT 
```

如果此环境变量设置为`0`，则 git 将不会在终端上提示（例如，在请求 HTTP 身份验证时）。

```
 GIT_CONFIG_NOSYSTEM 
```

是否跳过从系统范围`$(prefix)/etc/gitconfig`文件中读取设置。此环境变量可与`$HOME`和`$XDG_CONFIG_HOME`一起用于为挑剔脚本创建可预测的环境，或者您可以临时设置它以避免在等待具有足够权限的人员使用错误的`/etc/gitconfig`文件时进行修复。

```
 GIT_FLUSH 
```

如果此环境变量设置为“1”，则命令如 _git blame_ （在增量模式下）， _git rev-list_ ， _git log_ ， _git check-attr_ 和 _git check-ignore_ 将在刷新每条记录后强制刷新输出流。如果此变量设置为“0”，则将使用完全缓冲的 I / O 完成这些命令的输出。如果未设置此环境变量，Git 将根据 stdout 是否重定向到文件来选择缓冲或面向记录的刷新。

```
 GIT_TRACE 
```

启用常规跟踪消息，例如别名扩展，内置命令执行和外部命令执行。

如果此变量设置为“1”，“2”或“true”（比较不区分大小写），则跟踪消息将打印到 stderr。

如果变量设置为大于 2 且小于 10（严格）的整数值，则 Git 会将此值解释为打开文件描述符，并尝试将跟踪消息写入此文件描述符。

或者，如果变量设置为绝对路径（以 _/_ 字符开头），Git 会将其解释为文件路径，并尝试将跟踪消息附加到其中。

取消设置变量或将其设置为空，“0”或“false”（不区分大小写）禁用跟踪消息。

```
 GIT_TRACE_FSMONITOR 
```

为文件系统监视器扩展启用跟踪消息。有关可用的跟踪输出选项，请参见`GIT_TRACE`。

```
 GIT_TRACE_PACK_ACCESS 
```

为所有对任何包的访问启用跟踪消息。对于每次访问，都会记录包文件名和包中的偏移量。这可能有助于解决一些与包相关的性能问题。有关可用的跟踪输出选项，请参见`GIT_TRACE`。

```
 GIT_TRACE_PACKET 
```

为进出特定程序的所有数据包启用跟踪消息。这有助于调试对象协商或其他协议问题。在以“PACK”开头的数据包中关闭跟踪（但请参见下面的`GIT_TRACE_PACKFILE`）。有关可用的跟踪输出选项，请参见`GIT_TRACE`。

```
 GIT_TRACE_PACKFILE 
```

允许跟踪给定程序发送或接收的包文件。与其他跟踪输出不同，此跟踪是逐字的：没有标头，也没有二进制数据的引用。您几乎肯定希望直接进入文件（例如，`GIT_TRACE_PACKFILE=/tmp/my.pack`），而不是将其显示在终端上或将其与其他跟踪输出混合。

请注意，这仅适用于克隆和提取的客户端。

```
 GIT_TRACE_PERFORMANCE 
```

启用与性能相关的跟踪消息，例如每个 Git 命令的总执行时间。有关可用的跟踪输出选项，请参见`GIT_TRACE`。

```
 GIT_TRACE_SETUP 
```

在 Git 完成其设置阶段后，启用跟踪消息打印.git，工作树和当前工作目录。有关可用的跟踪输出选项，请参见`GIT_TRACE`。

```
 GIT_TRACE_SHALLOW 
```

启用跟踪消息，以帮助调试获取/克隆浅存储库。有关可用的跟踪输出选项，请参见`GIT_TRACE`。

```
 GIT_TRACE_CURL 
```

启用 git 传输协议的所有传入和传出数据（包括描述性信息）的卷曲完整跟踪转储。这类似于在命令行上执行 curl `--trace-ascii`。此选项会覆盖设置`GIT_CURL_VERBOSE`环境变量。有关可用的跟踪输出选项，请参见`GIT_TRACE`。

```
 GIT_TRACE_CURL_NO_DATA 
```

启用卷曲跟踪时（参见上面的`GIT_TRACE_CURL`），不要转储数据（即只转储信息行和标题）。

```
 GIT_REDACT_COOKIES 
```

这可以设置为以逗号分隔的字符串列表。当启用卷曲跟踪时（参见上面的`GIT_TRACE_CURL`），每当转储客户端发送的“Cookies：”头时，其密钥在该列表中的 cookie（区分大小写）的值将被编辑。

```
 GIT_LITERAL_PATHSPECS 
```

将此变量设置为`1`将导致 Git 按字面处理所有 pathspec，而不是作为 glob 模式。例如，运行`GIT_LITERAL_PATHSPECS=1 git log -- '*.c'`将搜索触及路径`*.c`的提交，而不是 glob `*.c`匹配的任何路径。如果要向 Git 提供文字路径（例如，之前通过`git ls-tree`，`--raw` diff 输出等给你的路径），你可能会想要这个。

```
 GIT_GLOB_PATHSPECS 
```

将此变量设置为`1`将导致 Git 将所有 pathspecs 视为 glob 模式（也称为“glob”魔术）。

```
 GIT_NOGLOB_PATHSPECS 
```

将此变量设置为`1`将导致 Git 将所有 pathspecs 视为文字（也称为“文字”魔法）。

```
 GIT_ICASE_PATHSPECS 
```

将此变量设置为`1`将导致 Git 将所有 pathspec 视为不区分大小写。

```
 GIT_REFLOG_ACTION 
```

更新 ref 时，除了 ref 的旧值和新值之外，还会创建 reflog 条目以跟踪 ref 更新的原因（通常是更新 ref 的高级命令的名称） 。脚本化的 Porcelain 命令可以使用`git-sh-setup`中的 set_reflog_action 辅助函数将其名称设置为此变量，当它被最终用户作为顶级命令调用时，将记录在 reflog 的主体中。

```
 GIT_REF_PARANOIA 
```

如果设置为`1`，则在迭代 refs 列表时包含损坏或命名错误的引用。在正常的，未损坏的存储库中，这没有任何作用。但是，启用它可能有助于 git 在存在损坏的 refs 的情况下检测并中止某些操作。当执行像 [git-prune [1]](https://git-scm.com/docs/git-prune) 这样的破坏性操作时，Git 会自动设置此变量。您不应该自己设置它，除非您想要确保操作已触及每个参考（例如，因为您正在克隆存储库以进行备份）。

```
 GIT_ALLOW_PROTOCOL 
```

如果设置为以冒号分隔的协议列表，则表现为`protocol.allow`设置为`never`，并且每个列出的协议都将`protocol.&lt;name&gt;.allow`设置为`always`（覆盖任何现有配置）。换句话说，将不允许任何未提及的协议（即，这是白名单，而不是黑名单）。有关详细信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 中`protocol.allow`的说明。

```
 GIT_PROTOCOL_FROM_USER 
```

设置为 0 以防止 fetch / push / clone 使用的协议配置为`user`状态。这对于限制来自不受信任的存储库的递归子模块初始化或对于将可能不受信任的 URL 提供给 git 命令的程序非常有用。有关详细信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 。

```
 GIT_PROTOCOL 
```

仅限内部使用。用于握线协议。包含一个冒号 _：_ 分隔的键列表，其中包含可选值 _ 键[= value]_ 。必须忽略未知键和值的存在。

```
 GIT_OPTIONAL_LOCKS 
```

如果设置为`0`，Git 将完成任何请求的操作，而不执行任何需要锁定的可选子操作。例如，这将阻止`git status`刷新索引作为副作用。这对于在后台运行的进程非常有用，这些进程不希望与存储库上的其他操作引起锁争用。默认为`1`。

```
 GIT_REDIRECT_STDIN 
```

```
 GIT_REDIRECT_STDOUT 
```

```
 GIT_REDIRECT_STDERR 
```

仅限 Windows：允许将标准输入/输出/错误句柄重定向到环境变量指定的路径。这在多线程应用程序中特别有用，其中通过`CreateProcess()`传递标准句柄的规范方法不是一个选项，因为它需要将句柄标记为可继承（因此**每个**生成的进程都将继承它们） ，可能阻止常规的 Git 操作）。主要用途是使用命名管道进行通信（例如`\\.\pipe\my-git-stdin-123`）。

支持两个特殊值：`off`将关闭相应的标准句柄，如果`GIT_REDIRECT_STDERR`为`2&gt;&1`，标准错误将重定向到与标准输出相同的句柄。

```
 GIT_PRINT_SHA1_ELLIPSIS (deprecated) 
```

如果设置为`yes`，则在（缩写）SHA-1 值后面打印省略号。这会影响分离的 HEAD（ [git-checkout [1]](https://git-scm.com/docs/git-checkout) ）和原始 diff 输出（ [git-diff [1]](https://git-scm.com/docs/git-diff) ）的指示。在所提到的情况下打印省略号不再被认为是足够的，并且在可预见的将来可能会删除对它的支持（以及变量）。

## 讨论

有关以下内容的更多详细信息，请参见用户手册和 [gitcore-tutorial [7]](https://git-scm.com/docs/gitcore-tutorial) 的 Git 概念章节。

Git 项目通常包含一个工作目录，顶层有一个“.git”子目录。 .git 目录包含一个表示项目完整历史记录的压缩对象数据库，一个将该历史记录链接到工作树的当前内容的“索引”文件，以及指向该历史记录的指针，如标记和分公司负责人

对象数据库包含三种主要类型的对象：blob，用于保存文件数据;树，指向 blob 和其他树来构建目录层次结构;和提交，每个引用一个树和一些父提交。

相当于其他系统称为“变更集”或“版本”的提交代表项目历史中的一个步骤，每个父项代表紧接在前的步骤。具有多个父项的提交代表独立开发线的合并。

所有对象都由其内容的 SHA-1 哈希命名，通常写为 40 个十六进制数字的字符串。这些名称是全球唯一的。通过签署该提交，可以保证导致提交的整个历史记录。为此目的提供第四对象类型，标签。

首次创建时，对象存储在单个文件中，但为了提高效率，以后可以将它们一起压缩为“包文件”。

名为 refs 的命名指针标记历史中的有趣点。 ref 可以包含对象的 SHA-1 名称或另一个 ref 的名称。名称以`ref/head/`开头的引用包含正在开发的分支的最新提交（或“头部”）的 SHA-1 名称。感兴趣的标签的 SHA-1 名称存储在`ref/tags/`下。名为`HEAD`的特殊引用包含当前签出分支的名称。

索引文件使用所有路径的列表进行初始化，并为每个路径初始化 blob 对象和一组属性。 blob 对象表示当前分支头部的文件内容。属性（上次修改时间，大小等）取自工作树中的相应文件。通过比较这些属性可以找到对工作树的后续更改。可以用新内容更新索引，并且可以从存储在索引中的内容创建新提交。

索引还能够存储给定路径名的多个条目（称为“阶段”）。这些阶段用于在合并进行时保存文件的各种未合并版本。

## 进一步的文件

请参阅“说明”部分中的参考资料以开始使用 Git。以下可能是首次使用者所需的详细信息。

用户手册和 [gitcore-tutorial [7]](https://git-scm.com/docs/gitcore-tutorial) 的 Git 概念章节都提供了对底层 Git 架构的介绍。

有关推荐工作流程的概述，请参见 [gitworkflows [7]](https://git-scm.com/docs/gitworkflows) 。

有关一些有用的示例，另请参见 howto 文档。

内部结构记录在 Git API 文档中。

从 CVS 迁移的用户可能还想阅读 [gitcvs-migration [7]](https://git-scm.com/docs/gitcvs-migration) 。

## 作者

Git 由 Linus Torvalds 创办，目前由 Junio C Hamano 维护。许多贡献来自 Git 邮件列表&lt; git@vger.kernel.org &gt;。 [`www.openhub.net/p/git/contributors/summary`](http://www.openhub.net/p/git/contributors/summary) 为您提供了更完整的贡献者列表。

如果你自己克隆了 git.git，那么 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 和 [git-blame [1]](https://git-scm.com/docs/git-blame) 的输出可以向你展示项目特定部分的作者。

## 报告错误

将错误报告给 Git 邮件列表&lt; git@vger.kernel.org &gt;主要完成开发和维护的地方。您无需订阅列表即可在其中发送消息。有关以前的错误报告和其他讨论，请参阅 [`public-inbox.org/git`](https://public-inbox.org/git) 的列表存档。

与安全相关的问题应私下披露给 Git Security 邮件列表&lt; git-security@googlegroups.com &gt;。

## 也可以看看

[gittutorial [7]](https://git-scm.com/docs/gittutorial) ， [gittutorial-2 [7]](https://git-scm.com/docs/gittutorial-2) ， [giteveryday [7]](https://git-scm.com/docs/giteveryday) ， [gitcvs-migration [7]](https://git-scm.com/docs/gitcvs-migration) ， [gitglossary [7]](https://git-scm.com/docs/gitglossary) ， [gitcore-tutorial [7]](https://git-scm.com/docs/gitcore-tutorial) ， [gitcli [7]](https://git-scm.com/docs/gitcli) ， Git 用户手册， [gitworkflows [7 ]](https://git-scm.com/docs/gitworkflows)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件