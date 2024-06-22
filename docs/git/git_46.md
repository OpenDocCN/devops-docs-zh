# gittutorial

> 原文： [`git-scm.com/docs/gittutorial`](https://git-scm.com/docs/gittutorial)

## 名称

gittutorial - Git 的教程介绍

## 概要

```
git *
```

## 描述

本教程将介绍如何将新项目导入 Git，对其进行更改以及与其他开发人员共享更改。

如果您主要对使用 Git 获取项目感兴趣，例如，测试最新版本，您可能更喜欢从 Git 用户手册的前两章开始。

首先，请注意您可以获取`git log --graph`等命令的文档：

```
$ man git-log
```

要么：

```
$ git help log
```

使用后者，您可以使用您选择的手动查看器;有关详细信息，请参阅 [git-help [1]](https://git-scm.com/docs/git-help) 。

在进行任何操作之前，最好使用您的姓名和公共电子邮件地址向 Git 介绍自己。最简单的方法是：

```
$ git config --global user.name "Your Name Comes Here"
$ git config --global user.email you@yourdomain.example.com
```

## 导入新项目

假设您有初始工作的 tarball project.tar.gz。您可以将它放在 Git 版本控制下，如下所示。

```
$ tar xzf project.tar.gz
$ cd project
$ git init
```

Git 会回复

```
Initialized empty Git repository in .git/
```

您现在已经初始化了工作目录 - 您可能会注意到创建了一个名为“.git”的新目录。

接下来，告诉 Git 使用 _git add_ 拍摄当前目录下所有文件内容的快照（注意 _。_）：

```
$ git add .
```

此快照现在存储在 Git 称为“索引”的临时暂存区域中。您可以使用 _git commit_ 将索引的内容永久存储在存储库中：

```
$ git commit
```

这将提示您提交提交消息。您现在已经在 Git 中存储了项目的第一个版本。

## 做出改变

修改一些文件，然后将更新的内容添加到索引：

```
$ git add file1 file2 file3
```

你现在准备好了。您可以使用带有--cached 选项的 _git diff_ 查看即将提交的内容：

```
$ git diff --cached
```

（没有--cached， _git diff_ 会显示你已经做过但尚未添加到索引中的任何更改。）您还可以通过 _git status [ _git status 获得有关情况的简短摘要 _：_

```
$ git status
On branch master
Changes to be committed:
Your branch is up to date with 'origin/master'.
  (use "git reset HEAD <file>..." to unstage)

	modified:   file1
	modified:   file2
	modified:   file3
```

如果您需要进行任何进一步调整，请立即执行此操作，然后将任何新修改的内容添加到索引中。最后，提交您的更改：

```
$ git commit
```

这将再次提示您输入描述更改的消息，然后记录项目的新版本。

或者，您可以使用，而不是预先运行 _git add_

```
$ git commit -a
```

它会自动注意任何修改过的（但不是新的）文件，将它们添加到索引中，然后一步一步地提交。

关于提交消息的注意事项：虽然不是必需的，但最好以一个简短（小于 50 个字符）的行来概括更改开始提交消息，然后是空白行，然后是更全面的描述。提交消息中第一个空白行的文本被视为提交标题，并且该标题在整个 Git 中使用。例如， [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 将提交转换为电子邮件，它使用主题行上的标题和正文中的其余提交。

## Git 跟踪内容而不是文件

许多修订控制系统提供`add`命令，告诉系统开始跟踪对新文件的更改。 Git 的`add`命令执行更简单和更强大的功能： _git add_ 用于新修改的文​​件和新修改的文​​件，在这两种情况下，它都会获取给定文件的快照，并在索引中显示内容，准备好包含在下一次提交中。

## 查看项目历史记录

您可以随时查看更改的历史记录

```
$ git log
```

如果您还希望在每个步骤中看到完整的差异，请使用

```
$ git log -p
```

通常，变更概述对于了解每个步骤非常有用

```
$ git log --stat --summary
```

## 管理分支机构

单个 Git 存储库可以维护多个开发分支。要创建名为“experimental”的新分支，请使用

```
$ git branch experimental
```

如果你现在跑

```
$ git branch
```

您将获得所有现有分支的列表：

```
  experimental
* master
```

“experimental”分支是您刚创建的分支，“master”分支是自动为您创建的默认分支。星号标记您当前所在的分支;类型

```
$ git checkout experimental
```

切换到实验分支。现在编辑文件，提交更改，然后切换回主分支：

```
(edit file)
$ git commit -a
$ git checkout master
```

检查您所做的更改是否已不再可见，因为它是在实验分支上进行的，您将返回主分支。

您可以在主分支上进行不同的更改：

```
(edit file)
$ git commit -a
```

在这一点上，两个分支已经分歧，每个分支都有不同的变化。要将实验中所做的更改合并到 master 中，请运行

```
$ git merge experimental
```

如果变化没有冲突，那么你就完成了。如果存在冲突，标记将留在显示冲突的有问题的文件中;

```
$ git diff
```

会表明这一点。一旦编辑了文件以解决冲突，

```
$ git commit -a
```

将提交合并的结果。最后，

```
$ gitk
```

将显示生成的历史记录的精美图形表示。

此时，您可以删除实验分支

```
$ git branch -d experimental
```

此命令可确保实验分支中的更改已在当前分支中。

如果你在一个分支疯狂的想法上发展，那么后悔它，你总是可以删除分支

```
$ git branch -D crazy-idea
```

分支机构既便宜又简单，所以这是尝试一些东西的好方法。

## 使用 Git 进行协作

假设 Alice 已经在/ home / alice / project 中启动了一个带有 Git 存储库的新项目，并且在同一台机器上有一个主目录的 Bob 想要贡献。

鲍勃开头：

```
bob$ git clone /home/alice/project myrepo
```

这将创建一个新目录“myrepo”，其中包含 Alice 的存储库的副本。克隆与原始项目处于同等地位，拥有自己原始项目历史的副本。

鲍勃然后进行一些更改并提交它们：

```
(edit files)
bob$ git commit -a
(repeat as necessary)
```

当他准备好时，他告诉 Alice 从/ home / bob / myrepo 的存储库中取出更改。她这样做：

```
alice$ cd /home/alice/project
alice$ git pull /home/bob/myrepo master
```

这将 Bob 的“主”分支的更改合并到 Alice 的当前分支中。如果 Alice 在此期间进行了自己的更改，那么她可能需要手动修复任何冲突。

因此，“pull”命令执行两个操作：它从远程分支获取更改，然后将它们合并到当前分支中。

请注意，一般情况下，Alice 会在启动此“拉动”之前希望提交本地更改。如果 Bob 的工作与 Alice 自其历史分歧后所做的工作冲突，Alice 将使用她的工作树和索引来解决冲突，现有的本地更改将干扰冲突解决过程（Git 仍将执行获取，但将拒绝合并 - - 爱丽丝将不得不以某种方式摆脱她的局部变化，并在发生这种情况时再次拉动）。

Alice 可以使用“fetch”命令查看 Bob 在没有合并的情况下做了什么。这允许 Alice 使用特殊符号“FETCH_HEAD”来检查 Bob 做了什么，以确定他是否有任何值得拉动的东西，如下所示：

```
alice$ git fetch /home/bob/myrepo master
alice$ git log -p HEAD..FETCH_HEAD
```

即使 Alice 有未提交的本地更改，此操作也是安全的。范围符号“HEAD..FETCH_HEAD”表示“显示可从 FETCH_HEAD 到达的所有内容，但排除可从 HEAD 访问的任何内容”。 Alice 已经知道了导致她当前状态（HEAD）的所有内容，并且回顾了 Bob 在他的状态（FETCH_HEAD）中没有看到的这个命令。

如果 Alice 想要想象自从他们的历史分叉后 Bob 做了什么，她可以发出以下命令：

```
$ gitk HEAD..FETCH_HEAD
```

这使用了我们之前在 _git log_ 中看到的相同的双点范围表示法。

Alice 可能想要查看他们分叉后他们做了什么。她可以使用三点形式而不是两点形式：

```
$ gitk HEAD...FETCH_HEAD
```

这意味着“显示从任何一个都可以访问的所有内容，但排除可以从它们中访问的任何内容”。

请注意，这些范围表示法可以与 gitk 和“git log”一起使用。

在检查 Bob 做了什么之后，如果没有紧急情况，Alice 可能会决定继续工作而不会从 Bob 那里撤离。如果 Bob 的历史确实有 Alice 会立即需要的东西，那么 Alice 可以选择先将她的工作藏匿在一起，做一个“拉动”，然后在最终的历史记录之上最终取消正在进行的工作。

当您在一个小型紧密结合的小组中工作时，一遍又一遍地与同一个存储库进行交互并不罕见。通过定义 _ 远程 _ 存储库速记，您可以更轻松：

```
alice$ git remote add bob /home/bob/myrepo
```

有了这个，Alice 可以使用 _git fetch_ 命令单独执行“pull”操作的第一部分，而无需将它们与自己的分支合并，使用：

```
alice$ git fetch bob
```

与简写形式不同，当 Alice 使用 _git remote_ 设置的远程存储库速记从 Bob 获取时，所获取的内容存储在远程跟踪分支中，在本例中为`bob/master`。所以在此之后：

```
alice$ git log -p master..bob/master
```

显示了 Bob 从 Alice 的主分支分支后所做的所有更改的列表。

检查完这些更改后，Alice 可以将更改合并到她的主分支中：

```
alice$ git merge bob/master
```

这`merge`也可以通过 _ 从她自己的远程跟踪分支 _ 拉出来完成，如下所示：

```
alice$ git pull . remotes/bob/master
```

请注意，无论命令行上给出了什么，git pull 总是合并到当前分支中。

之后，Bob 可以使用 Alice 的最新更改来更新他的回购

```
bob$ git pull
```

请注意，他不需要提供 Alice 的存储库的路径;当 Bob 克隆了 Alice 的存储库时，Git 将她的存储库的位置存储在存储库配置中，该位置用于拉取：

```
bob$ git config --get remote.origin.url
/home/alice/project
```

（ _git clone_ 创建的完整配置使用`git config -l`可见， [git-config [1]](https://git-scm.com/docs/git-config) 手册页解释了每个选项的含义。）

Git 还以“origin / master”的名义保留了 Alice 的主分支的原始副本：

```
bob$ git branch -r
  origin/master
```

如果 Bob 后来决定在不同的主机上工作，他仍然可以使用 ssh 协议执行克隆和拉取：

```
bob$ git clone alice.org:/home/alice/project myrepo
```

或者，Git 有一个原生协议，或者可以使用 http;有关详细信息，请参阅 [git-pull [1]](https://git-scm.com/docs/git-pull) 。

Git 也可以用于类似 CVS 的模式，具有各种用户推送更改的中央存储库;见 [git-push [1]](https://git-scm.com/docs/git-push) 和 [gitcvs-migration [7]](https://git-scm.com/docs/gitcvs-migration) 。

## 探索历史

Git 历史表示为一系列相互关联的提交。我们已经看到 _git log_ 命令可以列出这些提交。请注意，每个 git 日志条目的第一行也提供了提交的名称：

```
$ git log
commit c82a22c39cbc32576f64f5c6b3f24b99ea8149c7
Author: Junio C Hamano <junkio@cox.net>
Date:   Tue May 16 17:18:22 2006 -0700

    merge-base: Clarify the comments on post processing.
```

我们可以将此名称赋予 _git show_ 以查看有关此提交的详细信息。

```
$ git show c82a22c39cbc32576f64f5c6b3f24b99ea8149c7
```

但还有其他方法可以引用提交。您可以使用足够长的名称的任何初始部分来唯一标识提交：

```
$ git show c82a22c39c	# the first few characters of the name are
			# usually enough
$ git show HEAD		# the tip of the current branch
$ git show experimental	# the tip of the "experimental" branch
```

每个提交通常都有一个“父”提交，指向项目的先前状态：

```
$ git show HEAD^  # to see the parent of HEAD
$ git show HEAD^^ # to see the grandparent of HEAD
$ git show HEAD~4 # to see the great-great grandparent of HEAD
```

请注意，合并提交可能包含多个父级：

```
$ git show HEAD¹ # show the first parent of HEAD (same as HEAD^)
$ git show HEAD² # show the second parent of HEAD
```

您也可以提交自己的提交名称;跑完之后

```
$ git tag v2.5 1b2e1d63ff
```

您可以通过名称“v2.5”来引用 1b2e1d63ff。如果您打算与其他人共享此名称（例如，识别发布版本），您应该创建一个“标记”对象，并可能签名;有关详细信息，请参阅 [git-tag [1]](https://git-scm.com/docs/git-tag) 。

任何需要知道提交的 Git 命令都可以使用任何这些名称。例如：

```
$ git diff v2.5 HEAD	 # compare the current HEAD to v2.5
$ git branch stable v2.5 # start a new branch named "stable" based
			 # at v2.5
$ git reset --hard HEAD^ # reset your current branch and working
			 # directory to its state at HEAD^
```

小心最后一个命令：除了丢失工作目录中的任何更改外，它还将删除此分支的所有后续提交。如果此分支是包含这些提交的唯一分支，则它们将丢失。此外，不要在其他开发人员从中公开可见的分支上使用 _git reset_ ，因为它会迫使其他开发人员进行不必要的合并以清理历史记录。如果您需要撤消已推送的更改，请改用 _git revert_ 。

_git grep_ 命令可以在项目的任何版本中搜索字符串，所以

```
$ git grep "hello" v2.5
```

在 v2.5 中搜索所有出现的“hello”。

如果省略提交名称， _git grep_ 将搜索它在当前目录中管理的任何文件。所以

```
$ git grep "hello"
```

是一种快速搜索 Git 跟踪的文件的方法。

许多 Git 命令也采用提交集，可以通过多种方式指定。以下是 _git log_ 的一些示例：

```
$ git log v2.5..v2.6            # commits between v2.5 and v2.6
$ git log v2.5..                # commits since v2.5
$ git log --since="2 weeks ago" # commits from the last 2 weeks
$ git log v2.5.. Makefile       # commits since v2.5 which modify
				# Makefile
```

你也可以给 _git log_ 一个“范围”的提交，其中第一个不一定是第二个的祖先;例如，如果分支“稳定”和“主”的提示在一段时间之前偏离了共同的提交，那么

```
$ git log stable..master
```

将列出在主分支中但不在稳定分支中进行的提交

```
$ git log master..stable
```

将显示在稳定分支上但不在主分支上进行的提交列表。

_git log_ 命令有一个缺点：它必须在列表中显示提交。当历史中的发展线分散并然后合并在一起时， _git log_ 呈现这些提交的顺序是没有意义的。

大多数具有多个贡献者的项目（例如 Linux 内核或 Git 本身）经常合并，而 _gitk_ 在可视化其历史方面做得更好。例如，

```
$ gitk --since="2 weeks ago" drivers/
```

允许您浏览在“drivers”目录下修改文件的过去 2 周内提交的任何提交。 （注意：您可以在按下“ - ”或“+”的同时按住控制键来调整 gitk 的字体。）

最后，大多数采用文件名的命令都可以选择允许您通过提交在任何文件名之前，以指定文件的特定版本：

```
$ git diff v2.5:Makefile HEAD:Makefile.in
```

您也可以使用 _git show_ 查看任何此类文件：

```
$ git show v2.5:Makefile
```

## 下一步

本教程应足以为您的项目执行基本的分布式版本控制。但是，要完全理解 Git 的深度和强大功能，您需要了解它所基于的两个简单想法：

*   对象数据库是一个相当优雅的系统，用于存储项目文件，目录和提交的历史记录。

*   索引文件是目录树状态的缓存，用于创建提交，检出工作目录，以及保存合并中涉及的各种树。

本教程的第二部分解释了对象数据库，索引文件以及充分利用 Git 所需的其他一些可能性和结果。你可以在 [gittutorial-2 [7]](https://git-scm.com/docs/gittutorial-2) 找到它。

如果你不想马上继续这样做，那么在这一点上可能有趣的一些其他离题是：

*   [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) ， [git-am [1]](https://git-scm.com/docs/git-am) ：这些将 git 提交系列转换成电子邮件补丁，反之亦然，对 Linux 内核等项目很有用它严重依赖于电子邮件补丁。

*   [git-bisect [1]](https://git-scm.com/docs/git-bisect) ：当项目中存在回归时，追踪错误的一种方法是搜索历史记录以查找应该归咎于的确切提交。 Git bisect 可以帮助您对该提交执行二进制搜索。即使在具有大量合并分支的复杂非线性历史的情况下，它也足够智能地执行接近最优的搜索。

*   [gitworkflows [7]](https://git-scm.com/docs/gitworkflows) ：概述推荐的工作流程。

*   [giteveryday [7]](https://git-scm.com/docs/giteveryday) ：每天 Git 有 20 个命令或者所以。

*   [gitcvs-migration [7]](https://git-scm.com/docs/gitcvs-migration) ：适用于 CVS 用户的 Git。

## 也可以看看

[gittutorial-2 [7]](https://git-scm.com/docs/gittutorial-2) ， [gitcvs-migration [7]](https://git-scm.com/docs/gitcvs-migration) ， [gitcore-tutorial [7]](https://git-scm.com/docs/gitcore-tutorial) ， [gitglossary [7]](https://git-scm.com/docs/gitglossary) ， [git-help [1]](https://git-scm.com/docs/git-help) ， [gitworkflows [7]](https://git-scm.com/docs/gitworkflows) ， [giteveryday [7]](https://git-scm.com/docs/giteveryday) ， Git 用户手册

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件