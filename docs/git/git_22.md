# git-pull

> 原文： [`git-scm.com/docs/git-pull`](https://git-scm.com/docs/git-pull)
> 
> 贡献者：[Mrhuangyi](https://github.com/Mrhuangyi)

## 名称

git-pull - 从另一个存储库或本地分支获取并与其集成

## 概要

```
git pull [<options>] [<repository> [<refspec>…​]]
```

## 描述

将来自远程存储库的更改合并到当前分支中。在默认模式下，`git pull`是`git fetch`的缩写，后跟`git merge FETCH_HEAD`。

更确切地说， _git pull_ 使用给定参数运行 _git fetch_ 并调用 _git merge_ 将检索到的分支头合并到当前分支中。使用`--rebase`，它运行 _git rebase_ 而不是 _git merge_ 。

&lt;库&gt;应该是传递给 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 的一个远程存储库的名称。 &lt;的 Refspec&gt;可以命名任意远程引用（例如，标签的名称），或者甚至是具有相应远程跟踪分支的引用集合（例如，refs / heads / *：refs / remotes / origin / *），但通常它是远程存储库中分支的名称。

&lt; repository&gt;和&lt; branch&gt;的默认值从 [git-branch [1]](https://git-scm.com/docs/git-branch) `--track`设置的当前分支的“远程”和“合并”配置中读取。

假设存在以下历史记录并且当前分支为“`master`”：

```
	  A---B---C master on origin
	 /
    D---E---F---G master
	^
	origin/master in your repository
```

因为它偏离本地`master`（即`E`），所以“`git pull`”将从远程`master`分支获取并重放相应的更改，直到它的当前提交（`C`）在`master`上面并将结果记录在新提交中，同时记录两个父提交的名称以及描述更改的用户的日志消息。

```
	  A---B---C origin/master
	 /         \
    D---E---F---G---H master
```

有关详细信息，请参阅 [git-merge [1]](https://git-scm.com/docs/git-merge) ，包括如何呈现和处理冲突。

在 Git 1.7.0 或更高版本中，要取消一个有冲突的合并，请使用`git reset --merge`。 **警告**：在旧版本的 Git 中，不鼓励使用未提交的更改运行 _git pull_ ：尽管或许可行，但它可能会使您处于难以退出的冲突状态

如果任何远程更改与本地未提交的更改重叠，则将自动取消合并并且不更改工作树。通过 [git-stash [1]](https://git-scm.com/docs/git-stash) 拉动或存放它们之前，通常最好在工作顺序中进行任何局部更改。

## OPTIONS

```
 -q 
```

```
 --quiet 
```

这将传递给基础 git-fetch 以在传输过程中进行静噪报告，并在合并期间将基础 git-merge 传递给静噪输出。

```
 -v 
```

```
 --verbose 
```

传递--verbose 到 git-fetch 和 git-merge。

```
 --[no-]recurse-submodules[=yes|on-demand|no] 
```

此选项控制是否应该获取和更新所有已填充子模块的新提交（请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 和 [gitmodules [5]](https://git-scm.com/docs/gitmodules) ）。

如果通过 rebase 完成检出，则本地子模块提交也会被重新设置。

如果通过合并完成更新，则解析并检出子模块冲突。

### 与合并相关的选项

```
 --commit 
```

```
 --no-commit 
```

执行合并并提交结果。此选项可用于覆盖--no-commit。

使用--no-commit 执行合并但假装合并失败并且不自动提交，以便让用户有机会在提交之前检查并进一步调整合并结果。

```
 --edit 
```

```
 -e 
```

```
 --no-edit 
```

在提交成功的机械合并之前调用编辑器以进一步编辑自动生成的合并消息，以便用户可以解释并证明合并。 `--no-edit`选项可用于接受自动生成的消息（通常不鼓励这样做）。

较旧的脚本可能取决于不允许用户编辑合并日志消息的历史行为。他们将在运行`git merge`时看到编辑器打开。为了便于将此类脚本调整为更新的行为，可以在环境变量`GIT_MERGE_AUTOEDIT`的开头设置为`no`。

```
 --ff 
```

当合并解析为快进时，仅更新分支指针，而不创建合并提交。这是默认行为。

```
 --no-ff 
```

即使合并解析为快进，也要创建合并提交。这是在 _refs / tags /_ 层次结构中合并未存储在其自然位置的带注释（且可能已签名）的标记时的默认行为。

```
 --ff-only 
```

拒绝以非零状态合并和退出，除非当前`HEAD`已经是最新的，或者合并可以解析为快进。

```
 -S[<keyid>] 
```

```
 --gpg-sign[=<keyid>] 
```

GPG 签署生成的合并提交。 `keyid`参数是可选的并且默认为提交者标识;如果具体指定，它必须粘在没有空格的选项上。

```
 --log[=<n>] 
```

```
 --no-log 
```

除了分支名称之外，还使用最多&lt; n&gt;的单行描述填充日志消息。正在合并的实际提交。另见 [git-fmt-merge-msg [1]](https://git-scm.com/docs/git-fmt-merge-msg) 。

使用--no-log 不会列出正在合并的实际提交中的单行描述。

```
 --signoff 
```

```
 --no-signoff 
```

在提交日志消息的末尾由提交者逐行添加签名。签收的含义取决于项目，但它通常证明提交者有权在同一许可下提交此作品并同意开发者原产地证书（参见 [`developercertificate.org/`](http://developercertificate.org/) ] 欲获得更多信息）。

使用--no-signoff 时不要添加类似 Sign-off-by 的短行。

```
 --stat 
```

```
 -n 
```

```
 --no-stat 
```

在合并结束时显示 diffstat。 diffstat 也由配置选项 merge.stat 控制。

使用-n 或--no-stat 时，在合并结束时不显示 diffstat。

```
 --squash 
```

```
 --no-squash 
```

生成工作树和索引状态，就好像发生了真正的合并（合并信息除外），但实际上没有提交，移动`HEAD`或记录`$GIT_DIR/MERGE_HEAD`（导致下一个`git commit`命令到创建合并提交）。这允许您在当前分支之上创建单个提交，其效果与合并另一个分支（或章鱼的情况下更多）相同。

使用--no-squash 执行合并并提交结果。此选项可用于覆盖--squash。

```
 -s <strategy> 
```

```
 --strategy=<strategy> 
```

使用给定的合并策略;可以多次提供，以按照应该尝试的顺序指定它们。如果没有`-s`选项，则使用内置的策略列表（合并单个头时 _git merge-recursive_ ，否则使用 _git merge-octopus_ ）。

```
 -X <option> 
```

```
 --strategy-option=<option> 
```

将合并策略特定选项传递给合并策略。

```
 --verify-signatures 
```

```
 --no-verify-signatures 
```

验证正在合并的侧分支的提示提交是否使用有效密钥签名，即具有有效 uid 的密钥：在默认信任模型中，这意味着签名密钥已由可信密钥签名。如果侧分支的提示提交未使用有效密钥签名，则合并将中止。

```
 --summary 
```

```
 --no-summary 
```

同义词--stat 和--no-stat;这些已被弃用，将来会被删除。

```
 --allow-unrelated-histories 
```

默认情况下，`git merge`命令拒绝合并不共享共同祖先的历史记录。在合并独立开始生命的两个项目的历史时，此选项可用于覆盖此安全性。由于这是一种非常罕见的情况，因此默认情况下不会启用任何配置变量来启用它，也不会添加。

```
 -r 
```

```
 --rebase[=false|true|merges|preserve|interactive] 
```

如果为 true，则在获取后将当前分支重新绑定在上游分支的顶部。如果存在与上游分支对应的远程跟踪分支，并且自上次提取以来上游分支已重新定位，则 rebase 使用该信息来避免重新定位非本地更改。

设置为`merges`时，使用`git rebase --rebase-merges`进行 rebase，以便本地合并提交包含在 rebase 中（有关详细信息，请参阅 [git-rebase [1]](https://git-scm.com/docs/git-rebase) ）。

设置为 preserve 时，将`--preserve-merges`选项的 rebase 传递给`git rebase`，以便本地创建的合并提交不会被展平。

如果为 false，则将当前分支合并到上游分支中。

当为`interactive`时，启用 rebase 的交互模式。

如果要使`git pull`始终使用`--rebase`而不是合并，请参见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`pull.rebase`，`branch.&lt;name&gt;.rebase`和`branch.autoSetupRebase`。

| 注意 | 这是一种潜在的 _ 危险 _ 操作模式。它重写了历史，当你已经发布了这段历史时，它并不是一个好兆头。除非您仔细阅读 [git-rebase [1]](https://git-scm.com/docs/git-rebase) ，否则**不能**使用此选项。 |

```
 --no-rebase 
```

先覆盖--rebase。

```
 --autostash 
```

```
 --no-autostash 
```

在开始 rebase 之前，根据需要隐藏本地修改（参见 [git-stash [1]](https://git-scm.com/docs/git-stash) ），并在完成后应用存储条目。 `--no-autostash`用于覆盖`rebase.autoStash`配置变量（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

此选项仅在使用“--rebase”时有效。

### 与获取相关的选项

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

此选项可以多次被指定;如果是这样，Git 将报告从任何给定的提交中可访问的提交。

此选项的参数可以是 ref 的名称，ref 或者提交的（可能缩写的）SHA-1 上的 glob。指定 glob 等效于多次指定此选项，每个匹配对应的一个 ref 名称。

另请参见 [git-config [1]](https://git-scm.com/docs/git-config) 中记录的`fetch.negotiationAlgorithm`配置变量。

```
 -f 
```

```
 --force 
```

当 _git fetch_ 与`&lt;src&gt;:&lt;dst&gt;` refspec 一起使用时，它可能会拒绝更新本地分支，如 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 文档的`&lt;refspec&gt;`部分所述。此选项会覆盖该检查。

```
 -k 
```

```
 --keep 
```

保持下载的包。

```
 --no-tags 
```

默认情况下，指向从远程存储库下载的对象的标记将被提取并存储在本地。此选项会禁用此自动标记。可以使用远程。&lt; name&gt; .tagOpt 设置指定远程的默认行为。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

```
 -u 
```

```
 --update-head-ok 
```

默认情况下 _git fetch_ 拒绝更新与当前分支对应的头部。此标志禁用检查。这纯粹是为 _git pull_ 与 _git fetch_ 内部使用时进行通信，除非你实现你自己的瓷器，否则你不应该使用它。

```
 --upload-pack <upload-pack> 
```

当给定，并且要获取的存储库由 _git fetch-pack_ 处理时，`--exec=&lt;upload-pack&gt;`被传递给命令以指定另一端运行的命令的非默认路径。

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

“远程”存储库，它是获取或拉取操作的源。该参数可以是 URL（参见下面的 GIT URL 部分）或 remote 的名称（参见下面的 REMOTES 部分）。

```
 <refspec> 
```

指定要获取的引用和要更新的本地引用。如果命令行中没有&lt; refspec&gt; s，则从`remote.&lt;repository&gt;.fetch`变量读取 reff to fetch（参见 [git-fetch [1]](https://git-scm.com/docs/git-fetch) ）。

&lt; refspec&gt;的格式参数是可选加`+`，后跟源&lt; src&gt;，后跟冒号`:`，后跟目标 ref&lt; dst&gt;。当&lt; dst&gt;时，可以省略冒号。是空的。 &lt; SRC&gt;通常是 ref，但它也可以是完全拼写的十六进制对象名称。

`tag &lt;tag&gt;`表示与`refs/tags/&lt;tag&gt;:refs/tags/&lt;tag&gt;`相同;它请求获取给定标记的所有内容。

匹配&lt; src&gt;的远程引用取出，如果&lt; dst&gt;不是空字符串，尝试更新与其匹配的本地引用。

在没有`--force`的情况下是否允许更新取决于它被提取到的 ref 命名空间，被提取的对象的类型，以及更新是否被认为是快进。通常，相同的规则适用于推送时的提取，请参阅 [git-push [1]](https://git-scm.com/docs/git-push) 的`&lt;refspec&gt;...`部分。 _git fetch_ 特有的那些规则的例外情况如下所述。

直到 Git 版本 2.20，并且与使用 [git-push [1]](https://git-scm.com/docs/git-push) 推送时不同，对`refs/tags/*`的任何更新都将在 refspec（或`--force`）中没有`+`的情况下被接受。在获取时，我们会混淆地将远程的所有标记更新视为强制提取。从 Git 版本 2.20 开始，获取更新`refs/tags/*`的方式与推送时相同。即如果没有 refspec（或`--force`）中的`+`，任何更新都将被拒绝。

与使用 [git-push [1]](https://git-scm.com/docs/git-push) 进行推送时不同，[refdpec（或`--force`）中的`+`将接受`refs/{tags,heads}/*`以外的任何更新，无论是否交换，例如一个 blob 的树对象，或另一个提交的提交，它没有先前的提交作为祖先等。

与使用 [git-push [1]](https://git-scm.com/docs/git-push) 进行推送不同，没有任何配置可以修改这些规则，也没有类似于`pre-receive`挂钩的`pre-fetch`挂钩。

与使用 [git-push [1]](https://git-scm.com/docs/git-push) 推送一样，可以通过向 refspec 添加可选的前导`+`（或使用`--force` 命令行选项）来覆盖上面描述的关于不允许作为更新的所有规则。。唯一的例外是没有任何强制将使`refs/heads/*`命名空间接受非提交对象。

| 注意 | 当你想要获取的远程分支被认为是经常倒带和重新定位时，预计它的新提示将不会是其上一个提示的后代（如上次提取时存储在远程跟踪分支中）。您可能希望使用`+`符号来指示此类分支将需要非快进更新。无法确定或声明具有此行为的存储库中的分支可用;拉动用户只需知道这是分支的预期使用模式。 |

| 注意 | 在 _git pull_ 命令行上直接列出多个&lt; refspec&gt 和在您的配置中为&lt; repository&gt;提供多个`remote.&lt;repository&gt;.fetch`条目并运行 _git pull_ 命令，没有任何明确的&lt; refspec&gt;参数之间存在差异。在命令行中明确列出的&lt; refspec&gt;在获取后始终合并到当前分支中。换句话说，如果你列出多个远程引用， _git pull_ 将创建一个 Octopus 合并。另一方面，如果你没有列出任何明确的&lt; refspec&gt;在命令行上的参数， _git pull_ 将获取它在`remote.&lt;repository&gt;.fetch`配置中找到的所有&lt; refspec&gt; s 并仅合并第一个找到的&lt; refspec&gt;到当前的分支。这是因为很少使用远程参考设备制作八达通，而通过提取一个以上来一次跟踪多个远程磁头通常很有用。 |

## GIT 网址

通常，URL 包含有关传输协议，远程服务器的地址以及存储库路径的信息。根据传输协议，可能缺少某些信息。

Git 支持 ssh，git，http 和 https 协议（此外，ftp 和 ftps 可用于获取，但这是低效的并且已被弃用;请勿使用它）。

本机传输（即 git：// URL）不进行身份验证，在不安全的网络上应当谨慎使用。

可以使用以下语法：

*   SSH：// [用户@] host.xz [：端口] /path/to/repo.git/

*   GIT 中：//host.xz [：端口] /path/to/repo.git/

*   HTTP [S]：//host.xz [：端口] /path/to/repo.git/

*   FTP [S]：//host.xz [：端口] /path/to/repo.git/

另一种类似 scp 的语法也可以与 ssh 协议一起使用：

*   [用户@] host.xz：路径/到/ repo.git /

只有在第一个冒号之前没有斜杠时才会识别此语法。这有助于区分包含冒号的本地路径。例如，本地路径`foo:bar`可以指定为绝对路径或`./foo:bar`，以避免被误解为一个 ssh url。

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

您可以选择使用 [git-remote [1]](https://git-scm.com/docs/git-remote) ， [git-config [1]](https://git-scm.com/docs/git-config) 提供之前配置的遥控器的名称，甚至可以手动编辑`$GIT_DIR/config`文件。此远程的 URL 将用于访问存储库。如果你未在命令行上提供 refspec，则默认情况下将使用此远程的 refspec。配置文件中的条目如下所示：

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

根据操作，如果您没有在命令行上提供一个 refitpec，git 将使用以下 refspec 中的一个。 `&lt;branch&gt;`是`$GIT_DIR/branches`中此文件的名称，`&lt;head&gt;`默认为`master`。

git fetch 使用：

```
	refs/heads/<head>:refs/heads/<branch>
```

git push 使用：

```
	HEAD:refs/heads/<head>
```

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

这个选项通过支持 _ 我们的 _ 版本来强制大块的冲突干净地自动解决。来自与我们方不冲突的其他树的更改将反映到合并结果中。对于二进制文件，整个内容都来自我们这边。

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

## 违约行为

通常人们使用`git pull`而不给出任何参数。传统上，这相当于说`git pull origin`。但是，当在分支`&lt;name&gt;`上存在配置`branch.&lt;name&gt;.remote`时，将使用该值代替`origin`。

为了确定用于获取的 URL，请参考配置`remote.&lt;origin&gt;.url`的值，如果没有任何此类变量，则使用`$GIT_DIR/remotes/&lt;origin&gt;`中`URL:`行的值。

为了确定在命令行上没有任何 refspec 参数的情况下运行命令时要获取（并且可选地存储在远程跟踪分支中）的远程分支，将查询配置变量`remote.&lt;origin&gt;.fetch`的值，如果没有 t any，咨询`$GIT_DIR/remotes/&lt;origin&gt;`并使用其`Pull:`线。除了 OPTIONS 部分中描述的 refspec 格式之外，您还可以使用如下所示的 globbing refspec：

```
refs/heads/*:refs/remotes/origin/*
```

globbing refspec 必须具有非空 RHS（即必须存储在远程跟踪分支中获取的内容），并且其 LHS 和 RHS 必须以`/*`结束。以上规定了使用相同名称的`refs/remotes/origin/`层次结构中的远程跟踪分支跟踪所有远程分支。

在获取之后确定要合并哪个远程分支的规则有点涉及，以便不破坏向后兼容性。

如果在`git pull`的命令行上给出了显式 refspec，则它们都被合并。

如果命令行上没有给出 refspec，则`git pull`使用配置中的 refspec 或`$GIT_DIR/remotes/&lt;origin&gt;`。在这种情况下，以下规则适用：

1.  如果当前分支`&lt;name&gt;`的`branch.&lt;name&gt;.merge`配置存在，那么这是合并的远程站点的分支的名称。

2.  如果 refspec 是一个全局的，则不会合并任何内容。

3.  否则，合并第一个 refspec 的远程分支。

## 例子

*   更新你克隆的存储库的远程跟踪分支，然后将其中一个合并到当前分支中：

    ```
    $ git pull
    $ git pull origin
    ```

    通常，合并的分支是远程存储库的 HEAD，但选择由分支确定。&lt; name&gt; .remote 和 branch。&lt; name&gt; .merge options;有关详细信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 。

*   合并到当前分支远程分支`next`：

    ```
    $ git pull origin next
    ```

    这会在 FETCH_HEAD 中暂时保留`next`的副本，但不会更新任何远程跟踪分支。使用远程跟踪分支，可以通过调用 fetch 和 merge 来完成相同的操作：

    ```
    $ git fetch origin
    $ git merge origin/next
    ```

如果您尝试拉取后导致复杂冲突并且想要重新开始，则可以使用 _git reset_ 进行恢复。

## 安全

设计提取和推送协议的目的不是为了防止一方窃取不打算共享的其他存储库中的数据。如果您需要保护私有数据免受恶意对等方的攻击，那么最佳选择是将其存储在另一个存储库中。这适用于客户端和服务器。特别是，服务器上的命名空间对读访问控制无效;您应该只将命名空间的读访问权授予您信任的客户端，并具有对整个存储库的读访问权限。

已知的攻击向量如下：

1.  受害者发送“有”行，宣传其拥有的对象的 ID，这些对象并未明确地用于共享，但如果对等方也拥有它们，则可用于优化转移。攻击者选择一个对象 ID X 来窃取并向 X 发送一个 ref，但不需要发送 X 的内容，因为受害者已经拥有它。现在，受害者认为攻击者拥有 X，并且稍后会将 X 的内容发送回攻击者。 （这种攻击对于客户端在服务器上执行是最直接的，通过在客户端有权访问的命名空间中创建 ref，然后获取它。服务器在客户端上执行它的最可能方式是“将“X”合并到一个公共分支中，并希望用户在此分支上执行其他工作，并将其推送回服务器，而不会注意到合并。）

2.  与＃1 一样，攻击者选择一个对象 ID X 来窃取。受害者发送攻击者已经拥有的对象 Y，并且攻击者错误地声称拥有 X 而不是 Y，因此受害者将 Y 作为针对 X 的增量发送。该增量显示 X 的区域与攻击者的 Y 类似。

## BUGS

使用--recurse-submodules 只能在已检出的子模块中获取新的提交。例如，当上游在超级项目的刚刚提取的提交中添加了一个新的子模块，子模块本身无法获取，因此无法在以后检查该子模块而无需再次进行提取。这预计将在未来的 Git 版本中被修复。

## 也可以看看

[git-fetch [1]](https://git-scm.com/docs/git-fetch) ， [git-merge [1]](https://git-scm.com/docs/git-merge) ， [git-config [1]](https://git-scm.com/docs/git-config)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件
