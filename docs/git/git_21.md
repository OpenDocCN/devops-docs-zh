# git-fetch

> 原文： [`git-scm.com/docs/git-fetch`](https://git-scm.com/docs/git-fetch)

## 名称

git-fetch - 从另一个存储库下载对象和引用

## 概要

```
git fetch [<options>] [<repository> [<refspec>…​]]
git fetch [<options>] <group>
git fetch --multiple [<options>] [(<repository> | <group>)…​]
git fetch --all [<options>]
```

## 描述

从一个或多个其他存储库中获取分支和/或标记（统称为“refs”），以及完成其历史记录所需的对象。远程跟踪分支已更新（有关控制此行为的方法，请参阅下面&lt; refspec&gt;的说明）。

默认情况下，还会获取指向要获取的历史记录的任何标记;效果是获取指向您感兴趣的分支的标记。可以使用--tags 或--no-tags 选项或配置远程来更改此默认行为。&lt; name&gt; .tagOpt。通过使用明确获取标记的 refspec，您可以获取不指向您感兴趣的分支的标记。

_git fetch_ 可以从单个命名的存储库或 URL 获取，或者如果&lt; group&gt;则从一个存储库获取。给出并且有一个遥控器。&lt; group&gt;配置文件中的条目。 （参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

如果未指定远程，则默认情况下将使用`origin`远程，除非为当前分支配置了上游分支。

获取的引用名称及其指向的对象名称将写入`.git/FETCH_HEAD`。脚本或其他 git 命令可以使用此信息，例如 [git-pull [1]](https://git-scm.com/docs/git-pull) 。

## OPTIONS

```
 --all 
```

获取所有遥控器。

```
 -a 
```

```
 --append 
```

将获取的引用的引用名称和对象名称附加到`.git/FETCH_HEAD`的现有内容。如果没有此选项，`.git/FETCH_HEAD`中的旧数据将被覆盖。

```
 --depth=<depth> 
```

从每个远程分支历史记录的提示限制提取到指定的提交数。如果使用`--depth=&lt;depth&gt;`选项（参见 [git-clone [1]](https://git-scm.com/docs/git-clone) ）获取`git clone`创建的 _ 浅 _ 存储库，请将历史记录加深或缩短到指定的提交数。不提取深化提交的标记。

```
 --deepen=<depth> 
```

与--depth 类似，不同之处在于它指定了当前浅边界而不是每个远程分支历史记录的提交数。

```
 --shallow-since=<date> 
```

深化或缩短浅存储库的历史记录，以包括&lt; date&gt;之后的所有可访问提交。

```
 --shallow-exclude=<revision> 
```

深化或缩短浅存储库的历史记录，以排除从指定的远程分支或标记可到达的提交。可以多次指定此选项。

```
 --unshallow 
```

如果源存储库已完成，请将浅存储库转换为完整存储库，从而消除浅存储库所施加的所有限制。

如果源存储库很浅，则尽可能多地获取，以便当前存储库与源存储库具有相同的历史记录。

```
 --update-shallow 
```

默认情况下，从浅存储库中获取时，`git fetch`拒绝需要更新.git / shallow 的引用。此选项更新.git / shallow 并接受此类引用。

```
 --negotiation-tip=<commit|glob> 
```

默认情况下，Git 将向服务器报告可从所有本地引用访问的提交，以查找公共提交以尝试减少要接收的包文件的大小。如果指定，Git 将仅报告从给定提示可到达的提交。当用户知道哪个本地 ref 可能与正在获取的上游引用有共同提交时，这对于加速提取是有用的。

可以多次指定此选项;如果是这样，Git 将报告从任何给定提交可到达的提交。

此选项的参数可以是 ref 的名称，ref 或者提交的（可能缩写的）SHA-1 上的 glob。指定 glob 等效于多次指定此选项，每个匹配的 ref 名称一个。

另请参见 [git-config [1]](https://git-scm.com/docs/git-config) 中记录的`fetch.negotiationAlgorithm`配置变量。

```
 --dry-run 
```

显示将要完成的任务，而不进行任何更改。

```
 -f 
```

```
 --force 
```

当 _git fetch_ 与`&lt;src&gt;:&lt;dst&gt;` refspec 一起使用时，它可能会拒绝更新本地分支，如下面`&lt;refspec&gt;`部分所述。此选项会覆盖该检查。

```
 -k 
```

```
 --keep 
```

保持下载的包。

```
 --multiple 
```

允许多个&lt; repository&gt;和&lt; group&gt;要指定的参数。不能指定&lt; refspec&gt; s。

```
 -p 
```

```
 --prune 
```

在获取之前，删除远程不再存在的任何远程跟踪引用。如果仅由于默认标记自动跟踪或由于--tags 选项而提取标记，则不对其进行修剪。但是，如果由于显式 refspec（在命令行或远程配置中，例如，如果使用--mirror 选项克隆远程），则会提取标记，那么它们也会受到修剪。提供`--prune-tags`是提供标签 refspec 的简写。

有关详细信息，请参阅下面的 PRUNING 部分。

```
 -P 
```

```
 --prune-tags 
```

在获取之前，如果启用了`--prune`，则删除遥控器上不再存在的任何本地标签。应该更仔细地使用此选项，与`--prune`不同，它将删除已创建的任何本地引用（本地标记）。此选项是提供显式标记 refspec 和`--prune`的简写，请参阅其文档中有关该标记的讨论。

有关详细信息，请参阅下面的 PRUNING 部分。

```
 -n 
```

```
 --no-tags 
```

默认情况下，指向从远程存储库下载的对象的标记将被提取并存储在本地。此选项会禁用此自动标记。可以使用远程。&lt; name&gt; .tagOpt 设置指定远程的默认行为。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

```
 --refmap=<refspec> 
```

在获取命令行中列出的引用时，使用指定的 refspec（可以多次给出）将 refs 映射到远程跟踪分支，而不是远程存储库的`remote.*.fetch`配置变量的值。有关详细信息，请参阅“已配置的远程跟踪分支”部分。

```
 -t 
```

```
 --tags 
```

从远程获取所有标记（即，将远程标记`refs/tags/*`提取到具有相同名称的本地标记），以及否则将获取的任何其他标记。即使使用了--prune，单独使用此选项也不会对标记进行修剪（尽管如果它们也是显式 refspec 的目标，则无论如何都可以修剪标记;请参阅`--prune`）。

```
 --recurse-submodules[=yes|on-demand|no] 
```

此选项控制是否以及在何种条件下应该提取已填充的子模块的新提交。当设置为 _no_ 时，它可以用作布尔选项来完全禁用递归，或者当设置为 _yes_ 时无条件地递归到所有填充的子模块，这是使用此选项时的默认值没有任何价值。当超级项目检索到更新子模块对尚未在本地子模块克隆中的提交的引用的提交时，使用 _ 按需 _ 仅递归到填充的子模块。

```
 -j 
```

```
 --jobs=<n> 
```

用于获取子模块的并行子节点数。每个都将从不同的子模块中获取，这样获取许多子模块的速度会更快。默认情况下，将一次提取一个子模块。

```
 --no-recurse-submodules 
```

禁用递归获取子模块（这与使用`--recurse-submodules=no`选项具有相同的效果）。

```
 --submodule-prefix=<path> 
```

前置&lt;路径&gt;在信息性消息中打印的路径，例如“获取子模块 foo”。在子模块上递归时，此选项在内部使用。

```
 --recurse-submodules-default=[yes|on-demand] 
```

此选项在内部用于临时为--recurse-submodules 选项提供非负默认值。所有其他配置 fetch 子模块递归的方法（例如 [gitmodules [5]](https://git-scm.com/docs/gitmodules) 和 [git-config [1]](https://git-scm.com/docs/git-config) 中的设置）都会覆盖此选项，指定 - [no-] recurse-submodules 直接。

```
 -u 
```

```
 --update-head-ok 
```

默认情况下 _git fetch_ 拒绝更新与当前分支对应的头部。此标志禁用检查。这纯粹是为 _git pull_ 内部使用与 _git fetch_ 进行通信，除非你实现自己的瓷器，否则你不应该使用它。

```
 --upload-pack <upload-pack> 
```

当给定，并且要获取的存储库由 _git fetch-pack_ 处理时，`--exec=&lt;upload-pack&gt;`被传递给命令以指定另一端运行的命令的非默认路径。

```
 -q 
```

```
 --quiet 
```

将--quiet 转换为 git-fetch-pack 并使任何其他内部使用的 git 命令静音。未向标准错误流报告进度。

```
 -v 
```

```
 --verbose 
```

要冗长。

```
 --progress 
```

除非指定了-q，否则在将标准错误流附加到终端时，默认情况下会报告进度状态。即使标准错误流未定向到终端，此标志也会强制进度状态。

```
 -o <option> 
```

```
 --server-option=<option> 
```

使用协议版本 2 进行通信时，将给定的字符串传输到服务器。给定的字符串不得包含 NUL 或 LF 字符。当给出多个`--server-option=&lt;option&gt;`时，它们都按照命令行中列出的顺序发送到另一侧。

```
 -4 
```

```
 --ipv4 
```

仅使用 IPv4 地址，忽略 IPv6 地址。

```
 -6 
```

```
 --ipv6 
```

仅使用 IPv6 地址，忽略 IPv4 地址。

```
 <repository> 
```

“远程”存储库，它是获取或拉取操作的源。该参数可以是 URL（参见下面的 GIT URL 部分）或遥控器的名称（参见下面的 REMOTES 部分）。

```
 <group> 
```

一个名称，指的是存储库列表作为遥控器的值。&lt; group&gt;在配置文件中。 （参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

```
 <refspec> 
```

指定要获取的引用和要更新的本地引用。如果命令行中没有&lt; refspec&gt; s，则从`remote.&lt;repository&gt;.fetch`变量中读取要获取的引用（参见下面的配置的远程跟踪分支）。

&lt; refspec&gt;的格式参数是可选加`+`，后跟源&lt; src&gt;，后跟冒号`:`，后跟目标 ref&lt; dst&gt;。当&lt; dst&gt;时，可以省略冒号。是空的。 &lt; SRC&gt;通常是 ref，但它也可以是完全拼写的十六进制对象名称。

`tag &lt;tag&gt;`表示与`refs/tags/&lt;tag&gt;:refs/tags/&lt;tag&gt;`相同;它请求获取给定标记的所有内容。

匹配&lt; src&gt;的远程引用取出，如果&lt; dst&gt;不是空字符串，尝试更新与其匹配的本地引用。

在没有`--force`的情况下是否允许更新取决于它被提取到的 ref 命名空间，被提取的对象的类型，以及更新是否被认为是快进。通常，相同的规则适用于推送时的提取，请参阅 [git-push [1]](https://git-scm.com/docs/git-push) 的`&lt;refspec&gt;...`部分。 _git fetch_ 特有的那些规则的例外情况如下所述。

直到 Git 版本 2.20，并且与使用 [git-push [1]](https://git-scm.com/docs/git-push) 推送时不同，对`refs/tags/*`的任何更新都将在 refspec（或`--force`）中没有`+`的情况下被接受。在获取时，我们会混淆地将远程的所有标记更新视为强制提取。从 Git 版本 2.20 开始，获取更新`refs/tags/*`的方式与推送时相同。即如果没有 refspec（或`--force`）中的`+`，任何更新都将被拒绝。

与使用 [git-push [1]](https://git-scm.com/docs/git-push) 进行推送时不同，[refdpec（或`--force`）中的`+`将接受`refs/{tags,heads}/*`以外的任何更新，无论是否交换，例如一个 blob 的树对象，或另一个提交的提交，它没有先前的提交作为祖先等。

与使用 [git-push [1]](https://git-scm.com/docs/git-push) 进行推送不同，没有任何配置可以修改这些规则，也没有类似于`pre-receive`挂钩的`pre-fetch`挂钩。

与使用 [git-push [1]](https://git-scm.com/docs/git-push) 推送一样，可以通过向 refspec 添加可选的前导`+`（或使用`--force`来覆盖上面描述的关于不允许作为更新的所有规则。 ]命令行选项）。唯一的例外是没有任何强制将使`refs/heads/*`命名空间接受非提交对象。

| 注意 | 当你想要获取的远程分支被认为是经常倒带和重新定位时，预计它的新提示将不会是其上一个提示的后代（如上次提取时存储在远程跟踪分支中）。您可能希望使用`+`符号来指示此类分支将需要非快进更新。无法确定或声明具有此行为的存储库中的分支可用;拉动用户只需知道这是分支的预期使用模式。 |

## GIT 网址

通常，URL 包含有关传输协议，远程服务器的地址以及存储库路径的信息。根据传输协议，可能缺少某些信息。

Git 支持 ssh，git，http 和 https 协议（此外，ftp 和 ftps 可用于获取，但这是低效的并且已弃用;请勿使用它）。

本机传输（即 git：// URL）不进行身份验证，应在不安全的网络上谨慎使用。

可以使用以下语法：

*   SSH：// [用户@] host.xz [：端口] /path/to/repo.git/

*   GIT 中：//host.xz [：端口] /path/to/repo.git/

*   HTTP [S]：//host.xz [：端口] /path/to/repo.git/

*   FTP [S]：//host.xz [：端口] /path/to/repo.git/

另一种类似 scp 的语法也可以与 ssh 协议一起使用：

*   [用户@] host.xz：路径/到/ repo.git /

只有在第一个冒号之前没有斜杠时才会识别此语法。这有助于区分包含冒号的本地路径。例如，本地路径`foo:bar`可以指定为绝对路径或`./foo:bar`，以避免被误解为 ssh url。

ssh 和 git 协议还支持〜用户名扩展：

*   SSH：// [用户@] host.xz [：端口] /〜[用户] /path/to/repo.git/

*   GIT 中：//host.xz [：端口] /〜[用户] /path/to/repo.git/

*   [用户@] host.xz：/〜[用户] /path/to/repo.git/

对于本地也受 Git 支持的本地存储库，可以使用以下语法：

*   /path/to/repo.git/

*   文件：///path/to/repo.git/

这两种语法大多是等价的，除了克隆时，前者暗示--local 选项。有关详细信息，请参阅 [git-clone [1]](https://git-scm.com/docs/git-clone) 。

当 Git 不知道如何处理某种传输协议时，它会尝试使用 _remote-&lt; transport&gt;_ 远程助手，如果存在的话。要显式请求远程帮助程序，可以使用以下语法：

*   &lt;运输&gt; ::&lt;地址&gt;

其中&lt;地址&gt;可以是路径，服务器和路径，或者由被调用的特定远程助手识别的任意类似 URL 的字符串。有关详细信息，请参阅 [gitremote-helpers [1]](https://git-scm.com/docs/gitremote-helpers) 。

如果存在大量具有相似名称的远程存储库，并且您希望为它们使用不同的格式（以便将您使用的 URL 重写为有效的 URL），则可以创建表单的配置部分：

```
	[url "<actual url base>"]
		insteadOf = <other url base>
```

例如，有了这个：

```
	[url "git://git.host.xz/"]
		insteadOf = host.xz:/path/to/
		insteadOf = work:
```

像“work：repo.git”这样的 URL 或类似“host.xz：/path/to/repo.git”的 URL 将在任何带有 URL 的上下文中被重写为“git：//git.host.xz/repo” git 的”。

如果要为仅推送重写 URL，可以创建表单的配置部分：

```
	[url "<actual url base>"]
		pushInsteadOf = <other url base>
```

例如，有了这个：

```
	[url "ssh://example.org/"]
		pushInsteadOf = git://example.org/
```

像“git：//example.org/path/to/repo.git”这样的网址将被重写为“ssh：//example.org/path/to/repo.git”以进行推送，但是 pull 仍会使用原始网址。

## 遥控

可以使用以下某个名称而不是 URL 作为`&lt;repository&gt;`参数：

*   Git 配置文件中的一个遥控器：`$GIT_DIR/config`，

*   `$GIT_DIR/remotes`目录中的文件，或

*   `$GIT_DIR/branches`目录中的文件。

所有这些也允许你从命令行省略 refspec，因为它们每个都包含一个 git 将默认使用的 refspec。

### 在配置文件中命名为 remote

您可以选择使用 [git-remote [1]](https://git-scm.com/docs/git-remote) ， [git-config [1]](https://git-scm.com/docs/git-config) 提供之前配置的遥控器的名称，甚至可以手动编辑`$GIT_DIR/config`文件。此远程的 URL 将用于访问存储库。如果未在命令行上提供 refspec，则默认情况下将使用此远程的 refspec。配置文件中的条目如下所示：

```
	[remote "<name>"]
		url = <url>
		pushurl = <pushurl>
		push = <refspec>
		fetch = <refspec>
```

`&lt;pushurl&gt;`仅用于推送。它是可选的，默认为`&lt;url&gt;`。

### `$GIT_DIR/remotes`中的命名文件

您可以选择在`$GIT_DIR/remotes`中提供文件名。此文件中的 URL 将用于访问存储库。如果未在命令行上提供 refspec，则此文件中的 refspec 将用作默认值。该文件应具有以下格式：

```
	URL: one of the above URL format
	Push: <refspec>
	Pull: <refspec>
```

_git push_ 使用`Push:`行， _git pull_ 和 _git fetch_ 使用`Pull:`系。可以为其他分支映射指定多个`Push:`和`Pull:`行。

### `$GIT_DIR/branches`中的命名文件

您可以选择在`$GIT_DIR/branches`中提供文件名。此文件中的 URL 将用于访问存储库。该文件应具有以下格式：

```
	<url>#<head>
```

`&lt;url&gt;`是必需的; `#&lt;head&gt;`是可选的。

根据操作，如果您没有在命令行上提供一个 refitpec，git 将使用以下 refspec 之一。 `&lt;branch&gt;`是`$GIT_DIR/branches`中此文件的名称，`&lt;head&gt;`默认为`master`。

git fetch 使用：

```
	refs/heads/<head>:refs/heads/<branch>
```

git push 使用：

```
	HEAD:refs/heads/<head>
```

## 配置的远程跟踪分支

您经常通过定期重复从中获取相同的远程存储库。为了跟踪这种远程存储库的进度，`git fetch`允许您配置`remote.&lt;repository&gt;.fetch`配置变量。

通常这样的变量可能如下所示：

```
[remote "origin"]
	fetch = +refs/heads/*:refs/remotes/origin/*
```

此配置以两种方式使用：

*   运行`git fetch`时未指定要在命令行上获取的分支和/或标记，例如， `git fetch origin`或`git fetch`，`remote.&lt;repository&gt;.fetch`值用作 refspecs-它们指定要获取的 refs 和要更新的本地 refs。上面的示例将获取`origin`中存在的所有分支（即，与值的左侧匹配的任何 ref，`refs/heads/*`）并更新`refs/remotes/origin/*`层次结构中的相应远程跟踪分支。

*   当使用显式分支和/或标记运行`git fetch`以在命令行上获取时，例如， `git fetch origin master`，在命令行上给出的&lt; refspec&gt;确定要取出的内容（例如示例中的`master`，这是`master:`的简写，这反过来意味着“获取 _] master_ 分支但是我没有明确说出要从命令行“更新它的远程跟踪分支”，并且示例命令将只获取 _ 主 _ 分支。 `remote.&lt;repository&gt;.fetch`值确定更新哪个远程跟踪分支（如果有）。当以这种方式使用时，`remote.&lt;repository&gt;.fetch`值对决定 _ 获取 _ 的内容没有任何影响（即，当命令行列出 refspecs 时，这些值不用作 refspecs）;它们仅用于决定 _ 其中 _ 通过充当映射来存储所获取的引用。

后一次使用`remote.&lt;repository&gt;.fetch`值可以通过在命令行上给出`--refmap=&lt;refspec&gt;`参数来覆盖。

## 修枝

Git 有一个默认的保存数据，除非它被明确地丢弃;这延伸到持有本地对引用的本地引用，这些引用本身已经删除了那些分支。

如果留下累积，这些过时的引用可能会使具有大量分支流失的大而繁忙的存储库的性能变差，例如使`git branch -a --contains &lt;commit&gt;`等命令的输出不必要地冗长，并影响任何可以使用完整的已知引用集的其他任何东西。

这些远程跟踪引用可以作为一次性删除，其中包括：

```
# While fetching
$ git fetch --prune <name>

# Only prune, don't fetch
$ git remote prune <name>
```

要将引用修剪为正常工作流程的一部分而不需要记住运行它，请在配置中全局设置`fetch.prune`，或者在远程设置`remote.&lt;name&gt;.prune`。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

事情变得棘手和具体。修剪功能实际上并不关心分支，而是将修剪本地&lt;→远程引用作为远程 refspec 的函数（参见上面的`&lt;refspec&gt;`和配置远程跟踪分支] ）。

因此，如果遥控器的 refspec 包括例如`refs/tags/*:refs/tags/*`，或您手动运行，例如`git fetch --prune &lt;name&gt; "refs/tags/*:refs/tags/*"`它不会是被删除的过时远程跟踪分支，而是远程上不存在的任何本地标记。

这可能不是您所期望的，即您想要修剪远程`&lt;name&gt;`，但也要从中明确地获取标记，因此当您从中获取时，您将删除所有本地标记，其中大多数可能不是来自`&lt;name&gt;` ]遥远的第一名。

因此在使用像`refs/tags/*:refs/tags/*`这样的 refspec 或任何其他可能将多个遥控器的引用映射到同一本地命名空间的 refspec 时要小心。

由于在遥控器上保持最新的分支和标签是一个常见的用例，`--prune-tags`选项可以与`--prune`一起提供，以修剪遥控器上不存在的本地标签，并强制 - 更新那些不同的标签。也可以使用配置中的`fetch.pruneTags`或`remote.&lt;name&gt;.pruneTags`启用标签修剪。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

`--prune-tags`选项相当于在遥控器的 refspecs 中声明了`refs/tags/*:refs/tags/*`。这可能会导致一些看似奇怪的互动：

```
# These both fetch tags
$ git fetch --no-tags origin 'refs/tags/*:refs/tags/*'
$ git fetch --no-tags --prune-tags origin
```

在没有`--prune`或其配置版本的情况下提供它时不会出错的原因是为了配置版本的灵活性，并在命令行标志之间以及配置版本之间保持 1 = 1 的映射。

例如，合理的是配置`~/.gitconfig`中的`fetch.pruneTags=true`，以便在`git fetch --prune`运行时修剪标签，而不会在没有`--prune`的情况下每次调用`git fetch`。

使用`--prune-tags`修剪标签在获取 URL 而不是命名远程时也有效。这些将在原点上找不到所有修剪标签：

```
$ git fetch origin --prune --prune-tags
$ git fetch origin --prune 'refs/tags/*:refs/tags/*'
$ git fetch <url of origin> --prune --prune-tags
$ git fetch <url of origin> --prune 'refs/tags/*:refs/tags/*'
```

## OUTPUT

“git fetch”的输出取决于所使用的传输方法;本节介绍通过 Git 协议（本地或通过 ssh）和 Smart HTTP 协议获取时的输出。

获取的状态以表格形式输出，每行代表单个 ref 的状态。每一行的形式如下：

```
 <flag> <summary> <from> -> <to> [<reason>]
```

仅当使用--verbose 选项时，才会显示最新引用的状态。

在使用配置变量 fetch.output 指定的紧凑输出模式中，如果在另一个字符串中找到整个`&lt;from&gt;`或`&lt;to&gt;`，则在另一个字符串中将其替换为`*`。例如，`master -&gt; origin/master`变为`master -&gt; origin/*`。

```
 flag 
```

一个表示 ref 状态的字符：

```
 (space) 
```

成功获得快进;

```
 + 
```

成功的强制更新;

```
 - 
```

成功修剪参考;

```
 t 
```

成功更新标签;

```
 * 
```

成功获取新参考;

```
 ! 
```

对于被拒绝或未能更新的引用;和

```
 = 
```

对于一个最新的 ref，不需要提取。

```
 summary 
```

对于成功获取的 ref，摘要以适合用作`git log`的参数的形式显示 ref 的旧值和新值（在大多数情况下这是`&lt;old&gt;..&lt;new&gt;`，而强制非快速的`&lt;old&gt;...&lt;new&gt;` - 转发更新）。

```
 from 
```

从中获取远程引用的名称，减去其`refs/&lt;type&gt;/`前缀。在删除的情况下，远程 ref 的名称是“（none）”。

```
 to 
```

要更新的本地引用的名称减去其`refs/&lt;type&gt;/`前缀。

```
 reason 
```

一个人类可读的解释。在成功获取 refs 的情况下，不需要解释。对于失败的 ref，描述了失败的原因。

## 例子

*   更新远程跟踪分支：

    ```
    $ git fetch origin
    ```

    上述命令从远程 refs / heads / namespace 复制所有分支，并将它们存储到本地 refs / remotes / origin / namespace，除非分支。&lt; name&gt; .fetch 选项用于指定非默认 refspec。

*   明确使用 refspecs：

    ```
    $ git fetch origin +pu:pu maint:tmp
    ```

    这通过从远程存储库中的分支（分别）`pu`和`maint`获取来更新（或根据需要创建）本地存储库中的分支`pu`和`tmp`。

    `pu`分支即使不快进也会更新，因为它带有加号前缀; `tmp`不会。

*   查看远程分支，无需在本地存储库中配置远程：

    ```
    $ git fetch git://git.kernel.org/pub/scm/git/git.git maint
    $ git log FETCH_HEAD
    ```

    第一个命令从`git://git.kernel.org/pub/scm/git/git.git`的存储库中获取`maint`分支，第二个命令使用`FETCH_HEAD`检查 [git-log [1]](https://git-scm.com/docs/git-log) 的分支。最终将通过 git 的内置内务处理删除获取的对象（参见 [git-gc [1]](https://git-scm.com/docs/git-gc) ）。

## 安全

提取和推送协议的目的不是为了防止一方窃取不打算共享的其他存储库中的数据。如果您需要保护私有数据免受恶意对等方的攻击，那么最佳选择是将其存储在另一个存储库中。这适用于客户端和服务器。特别是，服务器上的命名空间对读访问控制无效;您应该只将命名空间的读访问权授予您信任的客户端，并具有对整个存储库的读访问权限。

已知的攻击向量如下：

1.  受害者发送“有”行，宣传其拥有的对象的 ID，这些对象并未明确地用于共享，但如果对等方也拥有它们，则可用于优化转移。攻击者选择一个对象 ID X 来窃取并向 X 发送一个 ref，但不需要发送 X 的内容，因为受害者已经拥有它。现在，受害者认为攻击者拥有 X，并且稍后会将 X 的内容发送回攻击者。 （这种攻击对于客户端在服务器上执行是最直接的，通过在客户端有权访问的命名空间中创建 ref，然后获取它。服务器在客户端上执行它的最可能方式是“将“X”合并到一个公共分支中，并希望用户在此分支上执行其他工作，并将其推送回服务器，而不会注意到合并。）

2.  与＃1 一样，攻击者选择一个对象 ID X 来窃取。受害者发送攻击者已经拥有的对象 Y，并且攻击者错误地声称拥有 X 而不是 Y，因此受害者将 Y 作为针对 X 的增量发送。该增量显示 X 的区域与攻击者的 Y 类似。

## BUGS

使用--recurse-submodules 只能在已检出的子模块中获取新的提交。例如，上游在超级项目的刚刚提取的提交中添加了一个新的子模块，子模块本身无法获取，因此无法在以后检查该子模块而无需再次进行提取。预计将在未来的 Git 版本中修复。

## 也可以看看

[git-pull [1]](https://git-scm.com/docs/git-pull)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件