# githooks

> 原文： [`git-scm.com/docs/githooks`](https://git-scm.com/docs/githooks)

## 名称

githooks - Git 使用的钩子

## 概要

$ GIT_DIR / hooks / *（或`git config core.hooksPath` / *）

## 描述

钩子是可以放在钩子目录中的程序，用于在 git 的执行中的某些点触发动作。没有设置可执行位的挂钩将被忽略。

默认情况下，hooks 目录为`$GIT_DIR/hooks`，但可以通过`core.hooksPath`配置变量进行更改（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

在 Git 调用钩子之前，它将其工作目录更改为裸存储库中的$ GIT_DIR 或非裸存储库中工作树的根。推送期间触发的挂钩例外（_ 预接收 _，_ 更新 _，_ 接收后 _，_ 更新后 _， _push-to-checkout_ ）总是在$ GIT_DIR 中执行。

钩子可以通过环境，命令行参数和 stdin 获取它们的参数。有关详细信息，请参阅下面每个挂钩的文档。

`git init`可能会将挂钩复制到新存储库，具体取决于其配置。有关详细信息，请参见 [git-init [1]](https://git-scm.com/docs/git-init) 中的“TEMPLATE DIRECTORY”部分。当本文档的其余部分引用“默认挂钩”时，它正在讨论 Git 附带的默认模板。

目前支持的钩子如下所述。

## 挂钩

### applypatch-MSG

这个钩子由 [git-am [1]](https://git-scm.com/docs/git-am) 调用。它需要一个参数，即包含建议的提交日志消息的文件的名称。退出非零状态会导致`git am`在应用修补程序之前中止。

允许钩子编辑消息文件，并可用于将消息规范化为某种项目标准格式。它还可以用于在检查消息文件后拒绝提交。

默认 _applypatch-msg_ 挂钩，如果启用，则运行 _commit-msg_ 挂钩，如果后者启用的话。

### 预 applypatch

这个钩子由 [git-am [1]](https://git-scm.com/docs/git-am) 调用。它不需要参数，并且在应用补丁之后但在提交之前调用。

如果它以非零状态退出，则在应用补丁后将不会提交工作树。

它可用于检查当前工作树，如果未通过某些测试则拒绝提交。

默认的 _pre-applypatch_ 挂钩启用时会运行 _ 预提交 _ 挂钩，如果后者启用的话。

### 后 applypatch

这个钩子由 [git-am [1]](https://git-scm.com/docs/git-am) 调用。它不需要参数，并在应用补丁并进行提交后调用。

此挂钩主要用于通知，不会影响`git am`的结果。

### 预提交

这个钩子由 [git-commit [1]](https://git-scm.com/docs/git-commit) 调用，可以用`--no-verify`选项旁路。它不需要任何参数，并在获取建议的提交日志消息和进行提交之前调用。退出此脚本的非零状态会导致`git commit`命令在创建提交之前中止。

默认的 _ 预提交 _ 挂钩，在启用时，会捕获带有尾随空格的行的引入，并在找到这样的行时中止提交。

如果命令不会调出编辑器来修改提交消息，则使用环境变量`GIT_EDITOR=:`调用所有`git commit`挂钩。

### 准备提交-MSG

在准备默认日志消息之后，在编辑器启动之前， [git-commit [1]](https://git-scm.com/docs/git-commit) 会调用此挂钩。

它需要一到三个参数。第一个是包含提交日志消息的文件的名称。第二个是提交消息的来源，可以是：`message`（如果给出了`-m`或`-F`选项）; `template`（如果给出了`-t`选项或设置了配置选项`commit.template`）; `merge`（如果提交是合并或`.git/MERGE_MSG`文件存在）; `squash`（如果存在`.git/SQUASH_MSG`文件）;或`commit`，然后是提交 SHA-1（如果给出了`-c`，`-C`或`--amend`选项）。

如果退出状态为非零，则`git commit`将中止。

挂钩的目的是在适当的位置编辑消息文件，并且不会被`--no-verify`选项抑制。非零退出意味着挂钩失败并中止提交。它不应该用作预提交钩子的替代品。

Git 附带的示例`prepare-commit-msg`挂钩删除了在提交模板的注释部分中找到的帮助消息。

### 提交-MSG

这个钩子由 [git-commit [1]](https://git-scm.com/docs/git-commit) 和 [git-merge [1]](https://git-scm.com/docs/git-merge) 调用，可以用`--no-verify`选项绕过。它需要一个参数，即包含建议的提交日志消息的文件的名称。以非零状态退出会导致命令中止。

允许钩子编辑消息文件，并可用于将消息规范化为某种项目标准格式。它还可以用于在检查消息文件后拒绝提交。

默认的 _commit-msg_ 挂钩启用时会检测到重复的“Signed-off-by”行，如果找到，则中止提交。

### 提交后

这个钩子由 [git-commit [1]](https://git-scm.com/docs/git-commit) 调用。它不需要参数，并在提交后调用。

此挂钩主要用于通知，不会影响`git commit`的结果。

### 前底垫

这个钩子由 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 调用，可以用来防止分支被重新绑定。可以用一个或两个参数调用钩子。第一个参数是分支系列的上游。第二个参数是重新分支的分支，在重新定位当前分支时不会设置。

### 后检出

更新工作树后运行 [git-checkout [1]](https://git-scm.com/docs/git-checkout) 时会调用此挂钩。钩子被赋予三个参数：前一个 HEAD 的 ref，新 HEAD 的 ref（可能已经或可能没有改变），以及一个标志，指示检出是否是分支检出（更改分支，标志= 1）或文件签出（从索引中检索文件，标志= 0）。这个钩子不会影响`git checkout`的结果。

它也在 [git-clone [1]](https://git-scm.com/docs/git-clone) 之后运行，除非使用`--no-checkout`（`-n`）选项。给钩子的第一个参数是 null-ref，第二个是新 HEAD 的 ref，而标志总是 1.同样对于`git worktree add`，除非使用`--no-checkout`。

此挂钩可用于执行存储库有效性检查，如果不同则自动显示与先前 HEAD 的差异，或设置工作目录元数据属性。

### 后合并

这个钩子由 [git-merge [1]](https://git-scm.com/docs/git-merge) 调用，当在本地存储库上完成`git pull`时会发生这种情况。钩子接受一个参数，一个状态标志，指定合并是否是一个压缩合并。如果由于冲突导致合并失败，则此挂钩不会影响`git merge`的结果，也不会执行。

该钩子可以与相应的预提交钩子一起使用，以保存和恢复与工作树相关联的任何形式的元数据（例如：权限/所有权，ACLS 等）。有关如何执行此操作的示例，请参阅 contrib / hooks / setgitperms.perl。

### 前推

这个钩子由 [git-push [1]](https://git-scm.com/docs/git-push) 调用，可用于防止发生推动。使用两个参数调用钩子，这两个参数提供目标远程的名称和位置，如果未使用命名远程，则两个值将相同。

有关要推送内容的信息在钩子的标准输入上提供了以下形式的行：

```
<local ref> SP <local sha1> SP <remote ref> SP <remote sha1> LF
```

例如，如果运行命令`git push origin master:foreign`，则挂钩将收到如下所示的行：

```
refs/heads/master 67890 refs/heads/foreign 12345
```

虽然将提供完整的 40 个字符的 SHA-1。如果外来参考不存在，`&lt;remote SHA-1&gt;`将为 40 `0`。如果要删除 ref，`&lt;local ref&gt;`将作为`(delete)`提供，`&lt;local SHA-1&gt;`将为 40 `0`。如果本地提交是由可以扩展的名称以外的其他东西指定的（例如`HEAD~`或 SHA-1），它将按照最初给出的方式提供。

如果此挂钩以非零状态退出，则`git push`将在不推送任何内容的情况下中止。可以通过写入标准错误将关于推送拒绝原因的信息发送给用户。

### 预接收

当 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 对`git push`作出反应并更新其存储库中的引用时，将调用此挂钩。在开始更新远程存储库上的 refs 之前，将调用预接收挂钩。其退出状态决定了更新的成功或失败。

该钩子为接收操作执行一次。它不需要参数，但是对于每个 ref 都要更新它在标准输入上接收格式的一行：

```
<old-value> SP <new-value> SP <ref-name> LF
```

其中`&lt;old-value&gt;`是存储在 ref 中的旧对象名称，`&lt;new-value&gt;`是要存储在 ref 中的新对象名称，`&lt;ref-name&gt;`是 ref 的全名。创建新参考时，`&lt;old-value&gt;`为 40 `0`。

如果钩子以非零状态退出，则不会更新任何引用。如果钩子退出零，则 _ 更新 _ 钩子仍然可以防止更新单个引用。

标准输出和标准错误输出都转发到另一端的`git send-pack`，因此您只需为用户输入`echo`消息即可。

在`git push --push-option=...`的命令行上给出的推送选项的数量可以从环境变量`GIT_PUSH_OPTION_COUNT`中读取，选项本身可以在`GIT_PUSH_OPTION_0`，`GIT_PUSH_OPTION_1`中找到，......如果协商不使用推送选项阶段，不会设置环境变量。如果客户端选择使用推送选项但不传输任何选项，则计数变量将设置为零，`GIT_PUSH_OPTION_COUNT=0`。

有关注意事项，请参阅 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 中的“隔离环境”部分。

### 更新

当 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 对`git push`作出反应并更新其存储库中的引用时，将调用此挂钩。在更新远程存储库上的 ref 之前，将调用更新挂钩。其退出状态决定了 ref 更新的成功或失败。

钩子为每个 ref 更新执行一次，并带有三个参数：

*   要更新的 ref 的名称，

*   存储在 ref 中的旧对象名称，

*   以及要存储在 ref 中的新对象名称。

从更新挂钩零退出允许更新 ref。以非零状态退出会阻止`git receive-pack`更新该 ref。

此挂钩可用于通过确保对象名称是提交对象来防止 _ 强制 _ 更新某些引用，该提交对象是旧对象名称所指定的提交对象的后代。也就是说，执行“仅限快进”政策。

它还可以用于记录 old..new 状态。但是，它并不知道整个分支集合，所以当天真地使用时，它最终会为每个 ref 发送一封电子邮件。 _ 接收后 _ 钩子更适合这种情况。

在限制用户仅通过线路访问 git 命令的环境中，此挂钩可用于实现访问控制，而不依赖于文件系统所有权和组成员身份。请参阅 [git-shell [1]](https://git-scm.com/docs/git-shell) ，了解如何使用登录 shell 限制用户只能访问 git 命令。

标准输出和标准错误输出都转发到另一端的`git send-pack`，因此您只需为用户输入`echo`消息即可。

默认 _ 更新 _ 挂钩，启用时 - `hooks.allowunannotated`配置选项未设置或设置为 false-可防止未注释的标签被推送。

### 后收到

当 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 对`git push`作出反应并更新其存储库中的引用时，将调用此挂钩。在更新所有引用后，它将在远程存储库上执行一次。

该钩子为接收操作执行一次。它不需要参数，但获得的信息与 _ 预接收 _ 钩子在其标准输入上的信息相同。

这个钩子不会影响`git receive-pack`的结果，因为它是在完成实际工作后调用的。

这取代了 _ 更新后 _ 钩子，除了它们的名称之外，它还获得了所有引用的旧值和新值。

标准输出和标准错误输出都转发到另一端的`git send-pack`，因此您只需为用户输入`echo`消息即可。

默认的 _post-receive_ 挂钩是空的，但是在 Git 发行版的`contrib/hooks`目录中提供了一个示例脚本`post-receive-email`，它实现了发送提交电子邮件。

在`git push --push-option=...`的命令行上给出的推送选项的数量可以从环境变量`GIT_PUSH_OPTION_COUNT`中读取，选项本身可以在`GIT_PUSH_OPTION_0`，`GIT_PUSH_OPTION_1`中找到，......如果协商不使用推送选项阶段，不会设置环境变量。如果客户端选择使用推送选项但不传输任何选项，则计数变量将设置为零，`GIT_PUSH_OPTION_COUNT=0`。

### 更新后的

当 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 对`git push`作出反应并更新其存储库中的引用时，将调用此挂钩。在更新所有引用后，它将在远程存储库上执行一次。

它需要可变数量的参数，每个参数都是实际更新的 ref 的名称。

此挂钩主要用于通知，不会影响`git receive-pack`的结果。

_ 更新后 _ 挂钩可以判断推送的磁头是什么，但是它不知道它们的原始值和更新值是什么，因此它是一个糟糕的做旧日志的地方。 _ 接收后 _ 钩子确实获得了 refs 的原始值和更新值。如果你需要它们，你可以考虑它。

启用后，默认 _ 更新后 _ 挂钩运行`git update-server-info`，以使哑传输（例如 HTTP）使用的信息保持最新。如果要发布可通过 HTTP 访问的 Git 存储库，则应该启用此挂钩。

标准输出和标准错误输出都转发到另一端的`git send-pack`，因此您只需为用户输入`echo`消息即可。

### 一键检出

当 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 对`git push`做出反应并更新其存储库中的引用时，以及当 push 尝试更新当前已检出的分支时，将调用此挂钩并且`receive.denyCurrentBranch`配置变量设置为`updateInstead`。如果工作树和远程存储库的索引与当前检出的提交有任何差异，则默认拒绝这样的推送;当工作树和索引都与当前提交匹配时，它们会更新以匹配新推送的分支提示。此挂钩用于覆盖默认行为。

钩子接收提交，当前分支的尖端将被更新。它可以以非零状态退出以拒绝推送（当它这样做时，它不能修改索引或工作树）。或者它可以对工作树和索引进行任何必要的更改，以便在当前分支的提示更新为新提交时将它们置于所需状态，并以零状态退出。

例如，钩子可以简单地运行`git read-tree -u -m HEAD "$1"`以模拟`git push`与`git push`反向运行的`git fetch`，因为`git read-tree -u -m`的双树形式与切换的`git checkout`基本相同分支，同时保持工作树中的本地更改不会干扰分支之间的差异。

### 预自动 GC

该钩子由`git gc --auto`调用（参见 [git-gc [1]](https://git-scm.com/docs/git-gc) ）。它不带参数，从此脚本退出非零状态会导致`git gc --auto`中止。

### 后重写

这个钩子由重写提交的命令调用（ [git-commit [1]](https://git-scm.com/docs/git-commit) 用`--amend`和 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 调用;当前`git filter-branch`执行 _ 不是 _ 打电话给它！）。它的第一个参数表示它被调用的命令：当前`amend`或`rebase`之一。将来可能会传递更多与命令相关的参数。

钩子以格式接收 stdin 上重写提交的列表

```
<old-sha1> SP <new-sha1> [ SP <extra-info> ] LF
```

_extra-info_ 再次依赖于命令。如果为空，则也省略前面的 SP。目前，没有命令传递任何 _extra-info_ 。

自动注释复制后，钩子总是运行（参见 [git-config [1]](https://git-scm.com/docs/git-config) 中的“notes.rewrite。&lt; command&gt;”），因此可以访问这些注释。

以下特定于命令的注释适用：

```
 rebase 
```

对于 _ 壁球 _ 和 _fixup_ 操作，所有被压缩的提交都被列为被重写为压缩的提交。这意味着将有几行共享相同的 _new-sha1_ 。

保证提交按照 rebase 处理它们的顺序列出。

### sendemail-验证

这个钩子由 [git-send-email [1]](https://git-scm.com/docs/git-send-email) 调用。它需要一个参数，即保存要发送的电子邮件的文件的名称。退出非零状态会导致`git send-email`在发送任何电子邮件之前中止。

### fsmonitor 守卫员

当配置选项`core.fsmonitor`设置为`.git/hooks/fsmonitor-watchman`时，将调用此挂钩。它需要两个参数，一个版本（当前为 1）和自 1970 年 1 月 1 日午夜以来经过的纳秒时间。

钩子应输出到 stdout 工作目录中可能自请求的时间以来可能已更改的所有文件的列表。逻辑应该具有包容性，以便它不会错过任何潜在的变化。路径应该相对于工作目录的根目录，并由单个 NUL 分隔。

可以包含实际没有更改的文件。应包括所有更改，包括新创建和删除的文件。重命名文件时，应包括旧名称和新名称。

Git 将限制检查更改的文件以及根据给定的路径名​​检查未跟踪文件的目录。

告诉 git“所有文件都已更改”的优化方法是返回文件名`/`。

退出状态确定 git 是否将使用钩子中的数据来限制其搜索。出错时，它将回退到验证所有文件和文件夹。

### P4-预提交

该钩子由`git-p4 submit`调用。它不需要参数，也不需要标准输入。从此脚本退出非零状态会阻止`git-p4 submit`启动。运行`git-p4 submit --help`了解详细信息。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件