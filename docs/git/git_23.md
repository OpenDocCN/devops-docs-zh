# git-push

> 原文： [`git-scm.com/docs/git-push`](https://git-scm.com/docs/git-push)

## 名称

git-push - 更新远程引用以及相关对象

## 概要

```
git push [--all | --mirror | --tags] [--follow-tags] [--atomic] [-n | --dry-run] [--receive-pack=<git-receive-pack>]
	   [--repo=<repository>] [-f | --force] [-d | --delete] [--prune] [-v | --verbose]
	   [-u | --set-upstream] [-o <string> | --push-option=<string>]
	   [--[no-]signed|--signed=(true|false|if-asked)]
	   [--force-with-lease[=<refname>[:<expect>]]]
	   [--no-verify] [<repository> [<refspec>…​]]
```

## 描述

使用本地引用更新远程引用，同时发送完成给定引用所必需的对象。

通过在那里设置 _ 挂钩 _，当你每次推入存储库时都可以使存储库发生有趣的事情。请参阅 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 的文档。

如果命令行未使用`&lt;repository&gt;`参数指定进行推送的位置，则会查询当前分支的`branch.*.remote`配置以确定推送位置。如果缺少相应配置，则默认为 _ 原点 _。

当命令行没有指定用`&lt;refspec&gt;...`参数或`--all`，`--mirror`，`--tags`选项推送什么时，该命令通过查询`remote.*.push`配置找到默认的`&lt;refspec&gt;`，如果找不到，根据`push.default`配置来决定推送什么（参见 [git-config [1]](https://git-scm.com/docs/git-config) 了解`push.default`的含义）。

当命令行和配置都没有指定要推送的内容时，则使用默认行为，它对应于`push.default`的`simple`值：当前分支被推送到相应的上游分支，但作为安全措施，如果上游分支与本地分支的名称不同，则推送被中止。

## 选项

```
 <repository> 
```

作为推送操作目标的“远程”存储库。该参数可以是一个 URL（参见下面的 GIT URL 部分）或遥控器的名称（参见下面的 REMOTES 部分）。

```
 <refspec>…​ 
```

使用什么源对象指定要更新的目标。 &lt; refspec&gt;参数的格式是可选加`+`，后跟源对象&lt; src&gt;，后跟冒号`:`，后跟目标 ref&lt; dst&gt;。

&lt; src&gt;通常是您想要推送的分支的名称，但它可以是任意“SHA-1 表达式”，例如`master~4`或`HEAD`（参见 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) ）。

&lt; dst&gt;通过此推送告知远程端的哪个 ref 已更新。这里不能使用任意表达式，必须命名实际的 ref。如果没有任何`&lt;refspec&gt;`参数的`git push [&lt;repository&gt;]`设置为使用`&lt;src&gt;`和`remote.&lt;repository&gt;.push`配置变量更新目标的某些 ref，则可以省略`:&lt;dst&gt;`部分 - 这样的推送将更新`&lt;src&gt;`的参考号通常在命令行上没有任何`&lt;refspec&gt;`的情况下更新。否则，缺少`:&lt;dst&gt;`意味着更新与`&lt;src&gt;`相同的参考号。

如果&lt; dst&gt;不以`refs/`开头（例如`refs/heads/master`）我们将尝试推断目的地&lt; repository&gt;上的`refs/*`中的位置它属于&lt; src&gt;的类型被推，是否&lt; dst&gt;很暧昧。

*   如果&lt; dst&gt;明确地引用远程&lt; repository&gt;上的引用。然后推送到那个参考。

*   如果&lt; src&gt;被解析为以 refs / heads /或 refs / tags /开头的 ref，然后将其添加到&lt; dst&gt;。

*   将来可能会添加其他歧义解决方案，但是现在任何其他情况都会出错，并指出我们尝试过的错误，并且取决于`advice.pushUnqualifiedRefname`配置（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）建议你可能想要推荐的 refs / namespace。

&lt; src&gt;引用的对象被用于更新&lt; dst&gt;远程参考。是否允许这取决于`refs/*`中&lt; dst&gt;的位置。参考活动如下面详细描述的那样，在这些部分中，“更新”是指除删除之外的任何修改，如下面几节中所述的不同处理。

`refs/heads/*`命名空间仅接受提交对象，并且只有在可以快速转发时才更新。

`refs/tags/*`命名空间将接受任何类型的对象（因为可以标记提交，树和 blob），并且将拒绝对它们的任何更新。

可以将任何类型的对象推送到`refs/{tags,heads}/*`之外的任何命名空间。对于标记和提交，这些将被视为它们是`refs/heads/*`内的提交，以确定是否允许更新。

即允许在`refs/{tags,heads}/*`之外快速提交提交和标记，即使在快速转发的内容不是提交的情况下，也允许标记对象恰好指向新提交，这是提交的快进它正在替换的最后一个标记（或提交）。如果标记指向相同的提交，并且推送剥离的标记，即推送现有标记对象指向的提交，或者现有提交指向的新标记对象，则也允许使用完全不同的标记替换标记。 。

`refs/{tags,heads}/*`之外的树和 blob 对象的处理方式与它们在`refs/tags/*`中的方式相同，任何对它们的更新都将被拒绝。

通过将可选的前导`+`添加到 refspec（或使用`--force`命令行选项），可以覆盖上面描述的关于不允许作为更新的所有规则。唯一的例外是没有任何强制将使`refs/heads/*`命名空间接受非提交对象。钩子和配置也可以覆盖或修改这些规则，参见例如 [git-config [1]](https://git-scm.com/docs/git-config) 中的`receive.denyNonFastForwards`和 [githooks [5]](https://git-scm.com/docs/githooks) 中的`pre-receive`和`update`。

推空&lt; src&gt;允许您删除&lt; dst&gt;来自远程存储库的 ref。除非配置或挂钩禁止，否则始终在 refspec（或`--force`）中没有前导`+`的情况下接受删除。参见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`receive.denyDeletes`和 [githooks [5]](https://git-scm.com/docs/githooks) 中的`pre-receive`和`update`。

特殊 refspec `:`（或`+:`允许非快进更新）指示 Git 推送“匹配”分支：对于本地端存在的每个分支，如果远程端分支更新，则更新远程端名称已存在于远程端。

`tag &lt;tag&gt;`表示与`refs/tags/&lt;tag&gt;:refs/tags/&lt;tag&gt;`相同。

```
 --all 
```

推送所有分支（即`refs/heads/`下的引用）;不能与其他&lt; refspec&gt;一起使用。

```
 --prune 
```

删除没有本地对应项的远程分支。例如，如果不再存在具有相同名称的本地分支，则将删除远程分支`tmp`。这也尊重 refspecs，例如如果`refs/heads/foo`不存在，`git push --prune remote refs/heads/*:refs/tmp/*`将确保远程`refs/tmp/foo`将被删除。

```
 --mirror 
```

而不是将每个引用命名为 push，指定将`refs/`下的所有引用（包括但不限于`refs/heads/`，`refs/remotes/`和`refs/tags/`）镜像到远程存储库。新创建的本地引用将被推送到远程端，本地更新的引​​用将在远程端强制更新，并且已删除的引用将从远程端删除。如果设置了配置选项`remote.&lt;remote&gt;.mirror`，则这是默认值。

```
 -n 
```

```
 --dry-run 
```

做所有事情，除了实际发送更新。

```
 --porcelain 
```

生成机器可读输出。每个引用的输出状态行将以制表符的形式分隔并发送到 stdout 而不是 stderr。我们将给出参考的完整符号名称。

```
 -d 
```

```
 --delete 
```

所有列出的引用都将从远程存储库中被删除。这与使用冒号为所有引号添加前缀相同。

```
 --tags 
```

除了在命令行中明确列出的 refspec 之外，还会推送`refs/tags`下的所有引用。

```
 --follow-tags 
```

推送没有此选项的将要被推送的所有引用，并且还在`refs/tags`中推送远程数据库中缺少的注释标记，但是指向可以从被推送的引用中访问的 commit-ish。也可以使用配置变量`push.followTags`指定。有关更多信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 中的`push.followTags`。

```
 --[no-]signed 
```

```
 --signed=(true|false|if-asked) 
```

GPG 签署推送请求以更新接收方的 refs，以允许钩子检查和/或记录。如果记录为`false`或`--no-signed`，则不会尝试签名。如果为`true`或`--signed`，如果服务器不支持签名推送，则推送将失败。如果设置为`if-asked`，则当且仅当服务器支持签名推送时才签名。如果对`gpg --sign`的实际调用失败，推送也将失败。有关接收端的详细信息，请参阅 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 。

```
 --[no-]atomic 
```

如果可用，请在远程端使用原子事务。要么更新所有引用，要么在出错时，不更新引用。如果服务器不支持原子推送，则推送将失败。

```
 -o <option> 
```

```
 --push-option=<option> 
```

将给定的字符串传输到服务器，服务器将它们传递给预接收和后接收挂钩。给定的字符串不得包含 NUL 或 LF 字符。当给出多个`--push-option=&lt;option&gt;`时，它们都按照命令行中列出的顺序发送到另一侧。如果没有从命令行给出`--push-option=&lt;option&gt;`，则使用配置变量`push.pushOption`的值。

```
 --receive-pack=<git-receive-pack> 
```

```
 --exec=<git-receive-pack> 
```

远程端 _git-receive-pack_ 程序的路径。当通过 ssh 推送到远程存储库时，有时很有用，并且您没有将程序放在默认$ PATH 上的目录中。

```
 --[no-]force-with-lease 
```

```
 --force-with-lease=<refname> 
```

```
 --force-with-lease=<refname>:<expect> 
```

通常，“git push”拒绝更新远程 ref，该远程 ref 不是用于覆盖它的本地 ref 的祖先。

如果远程 ref 的当前值是预期值，则此选项将覆盖此限制。 否则“git push”会失败。

想象一下，你必须改变你已发表的内容。您必须绕过“必须快进”规则才能将最初发布的历史记录替换为重新定位的历史记录。如果其他人在您重新定位时建立在您的原始历史之上，那么远程分支的提示可能会随着她的提交而提前，并且盲目推动`--force`将失去她的工作。

此选项允许您说您希望更新的历史记录是您重新定义并想要替换的内容。如果远程引用仍然指向您指定的提交，您可以确定没有其他人对引用做过任何事情。这就像在 ref 上接受“租约”而没有明确地锁定它，并且仅当“lease”仍然有效时才会更新远程 ref。

单独`--force-with-lease`，没有指定细节，将通过要求它们的当前值与我们为它们提供的远程跟踪分支相同来保护将要更新的所有远程 ref。

`--force-with-lease=&lt;refname&gt;`，未指定期望值，将保护命名 ref（单独），如果要更新，则要求其当前值与我们为其设置的远程跟踪分支相同。

`--force-with-lease=&lt;refname&gt;:&lt;expect&gt;`将保护命名 ref（单独），如果它将要更新，通过要求其当前值与指定值`&lt;expect&gt;`相同（允许它与远程跟踪分支不同于我们的 refname，或者在使用此表单时我们甚至不必拥有这样的远程跟踪分支）。如果`&lt;expect&gt;`是空字符串，则指定的命名 ref 必须不存在。

请注意，除了`--force-with-lease=&lt;refname&gt;:&lt;expect&gt;`之外的所有明确指定 ref 的当前值的表单仍然是实验性的，并且随着我们获得与此功能相关的经验，它们的语义可能会发生变化。

“--no-force-with-lease”将在命令行中取消之前的所有--force-with-lease。

关于安全性的一般注意事项：提供没有预期值的该选项，即`--force-with-lease`或`--force-with-lease=&lt;refname&gt;`与在遥控器上隐式运行`git fetch`以在后台推送的任何东西非常相互作用，例如，您在 Cronjob 中的存储库中的`git fetch origin`。

它提供的保护优于`--force`，确保您的工作所依据的后续更改不会被破坏，但如果某些后台进程在后台更新引用，则这很容易被忽略。除了远程跟踪信息之外，我们没有任何其他内容作为您希望看到的能作为参考资料的启发式信息。有可能导致破坏。

如果您的编辑器或其他系统在后台运行`git fetch`，那么缓解这种情况的方法就是简单地设置另一个远程：

```
git remote add origin-push $(git config remote.origin.url)
git fetch origin-push
```

现在，当后台进程运行`git fetch origin`时，`origin-push`上的引用将不会更新，因此命令如下：

```
git push --force-with-lease origin-push
```

除非您手动运行`git fetch origin-push`，否则将失败。这种方法当然完全会被运行`git fetch --all`的东西所击败，在这种情况下你需要禁用它或做一些更乏味的事情，如：

```
git fetch              # update 'master' from remote
git tag base master    # mark our base point
git rebase -i master   # rewrite some commits
git push --force-with-lease=master:base master:master
```

即为您已经看到并愿意覆盖的上游代码版本创建`base`标记，然后重写历史记录，如果远程版本仍在`base`，最后强制推送更改为`master`，无论是什么，您的本地`remotes/origin/master`已在后台更新。

```
 -f 
```

```
 --force 
```

通常，该命令拒绝更新远程 ref，该远程 ref 不是用于覆盖它的本地 ref 的祖先。此外，当使用`--force-with-lease`选项时，该命令拒绝更新当前值与预期值不匹配的远程 ref。

此标志禁用这些检查，并可能导致远程存储库丢失提交;小心使用它。

请注意，`--force`适用于所有被推送的引用，因此在`push.default`设置为`matching`或使用`remote.*.push`配置的多个推送目标时使用它可能会覆盖当前分支以外的引用（包括本地引用）严格落后于他们的远程对手）。要强制推送到一个分支，请使用 refspec 前面的`+`进行推送（例如`git push origin +master`强制推送到`master`分支）。有关详细信息，请参阅上面的`&lt;refspec&gt;...`部分。

```
 --repo=<repository> 
```

此选项等同于&lt; repository&gt;论点。如果同时指定了两者，则命令行参数优先。

```
 -u 
```

```
 --set-upstream 
```

对于每个最新或成功推送的分支，添加上游（跟踪）引用，由无参数 [git-pull [1]](https://git-scm.com/docs/git-pull) 和其他命令使用。有关更多信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 中的`branch.&lt;name&gt;.merge`。

```
 --[no-]thin 
```

这些选项传递给 [git-send-pack [1]](https://git-scm.com/docs/git-send-pack) 。当发送方和接收方共享许多相同的对象时，精简传输会显着减少发送的数据量。默认值为`--thin`。

```
 -q 
```

```
 --quiet 
```

除非发生错误，否则禁止所有输出，包括更新的 ref 列表。未向标准错误流报告进度。

```
 -v 
```

```
 --verbose 
```

详细地运行。

```
 --progress 
```

除非-q 已被指定，否则在将标准错误流附加到终端时，默认情况下会报告进度状态。即使标准错误流未定向到终端，此标志也会强制进度状态。

```
 --no-recurse-submodules 
```

```
 --recurse-submodules=check|on-demand|only|no 
```

可用于确保要推送的修订使用的所有子模块提交在远程跟踪分支上可用。如果使用 _ 检查 _，Git 将验证在子模块的至少一个远程处可用的所有要推送的修订中更改的子模块提交。如果缺少任何提交，则将中止推送并以非零状态退出。如果使用 _ 按需 _，则将推送在要推送的修订中更改的所有子模块。如果按需无法推送所有必要的修订，它也将被中止并退出非零状态。如果仅使用，则在超级项目未被按下时递归推送所有子模块。当不需要子模块递归时， _no_ 或`--no-recurse-submodules`的值可用于覆盖 push.recurseSubmodules 配置变量。

```
 --[no-]verify 
```

切换预推钩（参见 [githooks [5]](https://git-scm.com/docs/githooks) ）。默认值为--verify，使钩子有机会阻止推送。使用--no-verify，挂钩完全被绕过。

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

## GIT 网址

通常，URL 包含有关传输协议，远程服务器的地址以及存储库路径的信息。根据传输协议，可能缺少某些信息。

Git 支持 ssh，git，http 和 https 协议（此外，ftp 和 ftps 可用于获取，但这是低效的并且已弃用;请勿使用它）。

本机传输（即 git：// URL）不进行身份验证，在不安全的网络上应当谨慎使用。

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

## OUTPUT

“git push”的输出取决于所使用的传输方法;本节描述了推送 Git 协议时的输出（本地或通过 ssh）。

push 的状态以表格形式输出，每行代表一个 ref 的状态。每一行的形式如下：

```
 <flag> <summary> <from> -> <to> (<reason>)
```

如果使用了--porcelain，那么输出的每一行都是以下形式：

```
 <flag> \t <from>:<to> \t <summary> (<reason>)
```

仅当使用--porcelain 或--verbose 选项时，才会显示最新引用的状态。

```
 flag 
```

一个表示 ref 状态的字符：

```
 (space) 
```

为了成功快进推送;

```
 + 
```

成功的强制更新;

```
 - 
```

成功删除参考;

```
 * 
```

成功推出新的参考;

```
 ! 
```

对于拒绝或未能推送的裁判;和

```
 = 
```

对于一个最新的 ref 并且不需要推送的 ref。

```
 summary 
```

对于成功推送的 ref，摘要以适合用作`git log`的参数的形式显示 ref 的旧值和新值（在大多数情况下这是`&lt;old&gt;..&lt;new&gt;`，而强制非快速的`&lt;old&gt;...&lt;new&gt;` - 转发更新）。

对于失败的更新，提供了更多详细信息：

```
 rejected 
```

Git 根本没有尝试发送引用，通常是因为它不是快进而你没有强制更新。

```
 remote rejected 
```

远程端拒绝更新。通常由远程端的挂钩引起，或者因为远程存储库具有以下安全选项之一：`receive.denyCurrentBranch`（用于推送到已检出的分支），`receive.denyNonFastForwards`（用于强制非快进更新） ），`receive.denyDeletes`或`receive.denyDeleteCurrent`。见 [git-config [1]](https://git-scm.com/docs/git-config) 。

```
 remote failure 
```

远程端没有报告 ref 的成功更新，可能是因为远程端的临时错误，网络连接中断或其他瞬态错误。

```
 from 
```

被推送的本地引用的名称减去其`refs/&lt;type&gt;/`前缀。在删除的情况下，省略本地引用的名称。

```
 to 
```

要更新的远程引用的名称减去其`refs/&lt;type&gt;/`前缀。

```
 reason 
```

一个人类可读的解释。在成功推送 refs 的情况下，不需要解释。对于失败的 ref，描述了失败的原因。

## 关于快速前进的说明

当更新更改一个分支（或更多，一般来说，一个 ref），它曾经指向提交 A，当指向另一个提交 B 时，当且仅当 B 是 A 的后代时，它才被称为快进更新。

在从 A 到 B 的快速更新中，原始提交 A 构建在其上的提交集是新提交 B 构建在其上的提交的子集。因此，它不会失去任何历史。

相反，非快进更新将丢失历史记录。例如，假设您和其他人在同一个提交 X 中启动，并且您构建了一个导致提交 B 的历史记录，而另一个人构建了一个导致提交 A 的历史记录。历史记录如下所示：

```
      B
     /
 ---X---A
```

进一步假设另一个人已经将更改导致 A 返回到原始存储库，您从中获得了原始提交 X.

由另一个人完成的推送更新了用于指向提交 X 的分支以指向提交 A.这是一个快进。

但是如果你试图推送，你将尝试用提交 B 更新分支（现在指向 A）。这样做 _ 而不是 _ 快进。如果你这样做，提交 A 引入的更改将会丢失，因为每个人现在都将开始在 B 之上构建。

默认情况下，该命令不允许更新不是快进以防止此类历史记录丢失。

如果您不想丢失您的工作（从 X 到 B 的历史记录）或其他人的工作（从 X 到 A 的历史记录），您需要先从存储库中获取历史记录，创建包含已完成更改的历史记录由双方共同推动结果。

你可以执行“git pull”，解决潜在的冲突，并“git push”结果。 “git pull”将在提交 A 和 B 之间创建合并提交 C.

```
      B---C
     /   /
 ---X---A
```

使用生成的合并提交更新 A 将快进并且您的推送将被接受。

或者，您可以使用“git pull --rebase”在 A 之上重新定义 X 和 B 之间的更改，然后将结果推回。 rebase 将创建一个新的提交 D，它在 A 之上构建 X 和 B 之间的变化。

```
      B   D
     /   /
 ---X---A
```

同样，使用此提交更新 A 将快进并且您的推送将被接受。

还有一种常见的情况是，当您尝试推送时，您可能会遇到非快进拒绝，甚至当您进入存储库时，也有可能没有其他人推进。在你自己推送提交 A 之后（在本节的第一张图片中），将其替换为“git commit --amend”以生成提交 B，并尝试将其推出，因为忘记已经将 A 推出了。在这种情况下，并且只有当您确定没有人同时获取您之前的提交 A（并开始在其上构建）时，您可以运行“git push --force”来覆盖它。换句话说，“git push --force”是一种保留用于表示失去历史记录的情况的方法。

## 例子

```
 git push 
```

像`git push &lt;remote&gt;`一样工作，其中&lt; remote&gt;是当前分支的远程（或`origin`，如果没有为当前分支配置远程）。

```
 git push origin 
```

如果没有其他配置，则将当前分支推送到已配置的上游（`remote.origin.merge`配置变量），如果它与当前分支具有相同的名称，则错误输出而不推送。

没有&lt; refspec&gt;时此命令的默认行为给定可以通过设置遥控器的`push`选项或`push.default`配置变量来配置。

例如，要默认仅将当前分支推送到`origin`，请使用`git config remote.origin.push HEAD`。任何有效的&lt; refspec&gt; （如下例中的那些）可以配置为`git push origin`的默认值。

```
 git push origin : 
```

将“匹配”分支推送到`origin`。见&lt; refspec&gt;在上面的 OPTIONS 部分中，对“匹配”分支进行了描述。

```
 git push origin master 
```

找到与源存储库中的`master`匹配的引用（很可能，它会找到`refs/heads/master`），并用它更新`origin`存储库中的相同引用（例如`refs/heads/master`）。如果远程不存在`master`，则会创建它。

```
 git push origin HEAD 
```

一种将当前分支推送到远程同名的便捷方法。

```
 git push mothership master:satellite/master dev:satellite/dev 
```

使用与`master`匹配的源 ref（例如`refs/heads/master`）来更新`mothership`存储库中与`satellite/master`（最可能是`refs/remotes/satellite/master`）匹配的 ref;为`dev`和`satellite/dev`做同样的事情。

有关匹配语义的讨论，请参阅上面描述`&lt;refspec&gt;...`的部分。

这是为了模拟在`mothership`上运行的`git fetch`在相反方向上运行`git push`，以便集成在`satellite`上完成的工作，当你只能以一种方式进行连接时，通常需要这样做（即卫星可以进入母舰，但母舰无法启动与卫星的连接，因为后者位于防火墙后面或不运行 sshd）。

在`satellite`机器上运行`git push`后，您将进入`mothership`并在那里运行`git merge`以完成在`mothership`上运行的`git pull`的仿真，以提取在`satellite`上进行的更改。

```
 git push origin HEAD:master 
```

将当前分支推送到`origin`存储库中与`master`匹配的远程 ref。此表单便于在不考虑其本地名称的情况下推送当前分支。

```
 git push origin master:refs/heads/experimental 
```

通过复制当前`master`分支在`origin`存储库中创建分支`experimental`。仅当本地名称和远程名称不同时，才需要此表单在远程存储库中创建新分支或标记;否则，引用名称本身就可以使用。

```
 git push origin :experimental 
```

找到与`origin`存储库中的`experimental`匹配的引用（例如`refs/heads/experimental`），并将其删除。

```
 git push origin +dev:master 
```

使用 dev 分支更新原始存储库的主分支，允许非快进更新。 **这可以在原始存储库中悬挂未引用的提交。** 考虑以下情况：无法进行快进：

```
	    o---o---o---A---B  origin/master
		     \
		      X---Y---Z  dev
```

上面的命令会将原始存储库更改为

```
		      A---B  (unnamed branch)
		     /
	    o---o---o---X---Y---Z  master
```

提交 A 和 B 将不再属于具有符号名称的分支，因此将无法访问。因此，这些提交将通过源存储库上的`git gc`命令删除。

## 安全

提取和推送协议的目的不是为了防止一方窃取不打算共享的其他存储库中的数据。如果您需要保护私有数据免受恶意对等方的攻击，那么最佳选择是将其存储在另一个存储库中。这适用于客户端和服务器。特别是，服务器上的命名空间对读访问控制无效;您应该只将命名空间的读访问权授予您信任的客户端，并具有对整个存储库的读访问权限。

已知的攻击向量如下：

1.  受害者发送“have”行，宣传其拥有的对象的 ID，这些对象并未明确地用于共享，但如果对等方也拥有它们，则可用于优化转移。攻击者选择一个对象 ID X 来窃取并向 X 发送一个 ref，但不需要发送 X 的内容，因为受害者已经拥有它。现在，受害者认为攻击者拥有 X，并且稍后会将 X 的内容发送回攻击者。 （这种攻击对于客户端在服务器上执行是最直接的，通过在客户端有权访问的命名空间中创建 ref，然后获取它。服务器在客户端上执行它的最可能方式是“将“X”合并到一个公共分支中，并希望用户在此分支上执行其他工作，并将其推送回服务器，而不会注意到合并。）

2.  与＃1 一样，攻击者选择一个对象 ID X 来窃取。受害者发送攻击者已经拥有的对象 Y，并且攻击者错误地声称拥有 X 而不是 Y，因此受害者将 Y 作为针对 X 的增量发送。该增量显示 X 的区域与攻击者的 Y 类似。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件
