# giteveryday 

> 原文： [`git-scm.com/docs/giteveryday`](https://git-scm.com/docs/giteveryday)

## 名称

giteveryday - Everyday Git 的一组有用的最小命令

## 概要

每日 Git 有 20 个命令或者所以

## 描述

为了在这里描述日常 Git 的一小组有用命令，Git 用户可以大致分为四类。

*   个人开发者（独立）命令对任何提交者都是必不可少的，即使对于单独工作的人也是如此。

*   如果您与其他人一起工作，您还需要个人开发者（参与者）部分中列出的命令。

*   扮演 Integrator 角色的人除了上述之外还需要学习更多命令。

*   存储库管理命令适用于负责 Git 存储库的维护和提供的系统管理员。

## 个人开发者（独立）

独立的开发人员不会与其他人交换补丁，而是使用以下命令单独在单个存储库中工作。

*   [git-init [1]](https://git-scm.com/docs/git-init) 创建一个新的存储库。

*   [git-log [1]](https://git-scm.com/docs/git-log) 看看发生了什么。

*   [git-checkout [1]](https://git-scm.com/docs/git-checkout) 和 [git-branch [1]](https://git-scm.com/docs/git-branch) 切换分支。

*   [git-add [1]](https://git-scm.com/docs/git-add) 来管理索引文件。

*   [git-diff [1]](https://git-scm.com/docs/git-diff) 和 [git-status [1]](https://git-scm.com/docs/git-status) 看你正在做什么。

*   [git-commit [1]](https://git-scm.com/docs/git-commit) 推进当前分支。

*   [git-reset [1]](https://git-scm.com/docs/git-reset) 和 [git-checkout [1]](https://git-scm.com/docs/git-checkout) （带路径名参数）撤消更改。

*   [git-merge [1]](https://git-scm.com/docs/git-merge) 在本地分支之间合并。

*   [git-rebase [1]](https://git-scm.com/docs/git-rebase) 维护主题分支。

*   [git-tag [1]](https://git-scm.com/docs/git-tag) 标记已知点。

### 例子

```
 Use a tarball as a starting point for a new repository. 
```

```
$ tar zxf frotz.tar.gz
$ cd frotz
$ git init
$ git add . (1)
$ git commit -m "import of frotz source tree."
$ git tag v2.43 (2)
```

1.  添加当前目录下的所有内容。

2.  制作一个轻量级，未注释的标签。

```
 Create a topic branch and develop. 
```

```
$ git checkout -b alsa-audio (1)
$ edit/compile/test
$ git checkout -- curses/ux_audio_oss.c (2)
$ git add curses/ux_audio_alsa.c (3)
$ edit/compile/test
$ git diff HEAD (4)
$ git commit -a -s (5)
$ edit/compile/test
$ git diff HEAD^ (6)
$ git commit -a --amend (7)
$ git checkout master (8)
$ git merge alsa-audio (9)
$ git log --since='3 days ago' (10)
$ git log v2.43.. curses/ (11)
```

1.  创建一个新的主题分支。

2.  恢复`curses/ux_audio_oss.c`中的拙劣变化。

3.  你需要告诉 Git 你是否添加了一个新文件;如果您稍后执行`git commit -a`，将会删除和修改。

4.  看看你提出了哪些改变。

5.  如您所测试的那样，通过您的签名来承诺所有内容。

6.  查看所有更改，包括之前的提交。

7.  修改先前的提交，使用原始邮件添加所有新更改。

8.  切换到主分支。

9.  将主题分支合并到主分支中。

10.  审核提交日志;可以组合限制输出的其他形式，包括`-10`（最多显示 10 次提交），`--until=2005-12-10`等。

11.  仅查看触及`curses/`目录中的内容的更改，因为`v2.43`标记。

## 个人开发者（参与者）

作为组项目参与者的开发人员需要学习如何与他人通信，并且除了独立开发人员所需的命令之外还使用这些命令。

*   [git-clone [1]](https://git-scm.com/docs/git-clone) 从上游到本地存储库。

*   来自“origin”的 [git-pull [1]](https://git-scm.com/docs/git-pull) 和 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 与上游保持同步。

*   [git-push [1]](https://git-scm.com/docs/git-push) 到共享存储库，如果采用 CVS 样式的共享存储库工作流程。

*   [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 准备电子邮件提交，如果采用 Linux 内核式公共论坛工作流程。

*   [git-send-email [1]](https://git-scm.com/docs/git-send-email) 发送您的电子邮件提交而不会被您的 MUA 损坏。

*   [git-request-pull [1]](https://git-scm.com/docs/git-request-pull) 创建上游拉动变化的摘要。

### 例子

```
 Clone the upstream and work on it.  Feed changes to upstream. 
```

```
$ git clone git://git.kernel.org/pub/scm/.../torvalds/linux-2.6 my2.6
$ cd my2.6
$ git checkout -b mine master (1)
$ edit/compile/test; git commit -a -s (2)
$ git format-patch master (3)
$ git send-email --to="person <email@example.com>" 00*.patch (4)
$ git checkout master (5)
$ git pull (6)
$ git log -p ORIG_HEAD.. arch/i386 include/asm-i386 (7)
$ git ls-remote --heads http://git.kernel.org/.../jgarzik/libata-dev.git (8)
$ git pull git://git.kernel.org/pub/.../jgarzik/libata-dev.git ALL (9)
$ git reset --hard ORIG_HEAD (10)
$ git gc (11)
```

1.  从主人那里结帐一个新的分支`mine`。

2.  根据需要重复。

3.  从你的分支中提取补丁，相对于主人，

4.  并发送电子邮件

5.  返回`master`，准备好看看有什么新东西

6.  `git pull`默认从`origin`获取并合并到当前分支。

7.  拉动后立即查看自上次检查以来上游所做的更改，仅在我们感兴趣的区域内。

8.  检查外部存储库中的分支名称（如果未知）。

9.  从特定存储库中获取特定分支`ALL`并合并它。

10.  恢复拉力。

11.  垃圾从恢复的拉动中收集剩余的物体。

```
 Push into another repository. 
```

```
satellite$ git clone mothership:frotz frotz (1)
satellite$ cd frotz
satellite$ git config --get-regexp '^(remote|branch)\.' (2)
remote.origin.url mothership:frotz
remote.origin.fetch refs/heads/*:refs/remotes/origin/*
branch.master.remote origin
branch.master.merge refs/heads/master
satellite$ git config remote.origin.push \
	   +refs/heads/*:refs/remotes/satellite/* (3)
satellite$ edit/compile/test/commit
satellite$ git push origin (4)

mothership$ cd frotz
mothership$ git checkout master
mothership$ git merge satellite/master (5)
```

1.  motherhip 机器在你的主目录下有一个 frotz 存储库;从中克隆以在卫星机器上启动存储库。

2.  clone 默认设置这些配置变量。它安排`git pull`来获取和存储母舰机器的分支到本地`remotes/origin/*`远程跟踪分支机构。

3.  安排`git push`将所有本地分支机构推送到母舰机器的相应分支机构。

4.  push 将把我们所有的工作都藏在母舰机器上的`remotes/satellite/*`远程跟踪分支上。您可以将其用作备份方法。同样地，你可以假装母舰从你那里“取出”（当访问是单方面时很有用）。

5.  在母舰机器上，将卫星机器上完成的工作合并到主分支机构中。

```
 Branch off of a specific tag. 
```

```
$ git checkout -b private2.6.14 v2.6.14 (1)
$ edit/compile/test; git commit -a
$ git checkout master
$ git cherry-pick v2.6.14..private2.6.14 (2)
```

1.  基于众所周知（但略微落后）的标签创建一个私有分支。

2.  将`private2.6.14`分支中的所有更改转发到`master`分支，而不进行正式的“合并”。或者手写
    `git format-patch -k -m --stdout v2.6.14..private2.6.14 | git am -3 -k`

备用参与者提交机制使用`git request-pull`或拉取请求机制（例如，在 GitHub（www.github.com）上使用）向您的上游通知您的贡献。

## 积分

作为集体项目中的集成商的相当核心的人接收他人所做的更改，审查和集成他们并发布结果供其他人使用，除了参与者需要的命令之外还使用这些命令。

在 GitHub（www.github.com）上回复`git request-pull`或 pull-request 以将其他人的工作整合到他们的历史中的人也可以使用此部分。存储库的子区域中尉既可以作为参与者也可以作为集成者。

*   [git-am [1]](https://git-scm.com/docs/git-am) 应用您的贡献者通过电子邮件发送的补丁。

*   [git-pull [1]](https://git-scm.com/docs/git-pull) 从您信任的副手中合并。

*   [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 准备并发送建议的替代贡献者。

*   [git-revert [1]](https://git-scm.com/docs/git-revert) 撤消拙劣的提交。

*   [git-push [1]](https://git-scm.com/docs/git-push) 发布前沿。

### 例子

```
 A typical integrator’s Git day. 
```

```
$ git status (1)
$ git branch --no-merged master (2)
$ mailx (3)
& s 2 3 4 5 ./+to-apply
& s 7 8 ./+hold-linus
& q
$ git checkout -b topic/one master
$ git am -3 -i -s ./+to-apply (4)
$ compile/test
$ git checkout -b hold/linus && git am -3 -i -s ./+hold-linus (5)
$ git checkout topic/one && git rebase master (6)
$ git checkout pu && git reset --hard next (7)
$ git merge topic/one topic/two && git merge hold/linus (8)
$ git checkout maint
$ git cherry-pick master~4 (9)
$ compile/test
$ git tag -s -m "GIT 0.99.9x" v0.99.9x (10)
$ git fetch ko && for branch in master maint next pu (11)
    do
	git show-branch ko/$branch $branch (12)
    done
$ git push --follow-tags ko (13)
```

1.  看看你在做什么，如果有的话。

2.  看哪些分支还没有合并到`master`中。对于任何其他集成分支，例如， `maint`，`next`和`pu`（潜在更新）。

3.  阅读邮件，保存适用的邮件，并保存其他尚未准备好的邮件（其他邮件阅读器可用）。

4.  以交互方式应用它们与您的签字。

5.  根据需要创建主题分支并再次应用签名。

6.  rebase 内部主题分支，尚未合并到主服务器或作为稳定分支的一部分公开。

7.  每次从下一次重启`pu`。

8.  捆绑主题分支仍在烹饪。

9.  backport 一个关键修复。

10.  创建签名标记。

11.  确保主人不会被意外地重绕，而不是已经被推出。

12.  在`git show-branch`的输出中，`master`应该包含`ko/master`的所有内容，`next`应该具有`ko/next`所具有的所有内容等。

13.  推出最前沿，以及指向推动历史的新标签。

在这个例子中，`ko`简写点在 kernel.org 的 Git 维护者的存储库中，看起来像这样：

```
(in .git/config)
[remote "ko"]
	url = kernel.org:/pub/scm/git/git.git
	fetch = refs/heads/*:refs/remotes/ko/*
	push = refs/heads/master
	push = refs/heads/next
	push = +refs/heads/pu
	push = refs/heads/maint
```

## 存储库管理

存储库管理员使用以下工具来设置和维护开发人员对存储库的访问权限。

*   [git-daemon [1]](https://git-scm.com/docs/git-daemon) 允许从存储库匿名下载。

*   [git-shell [1]](https://git-scm.com/docs/git-shell) 可以用作共享中央存储库用户的 _ 受限登录 shell_ 。

*   [git-http-backend [1]](https://git-scm.com/docs/git-http-backend) 提供了 Git-over-HTTP（“Smart http”）的服务器端实现，允许提取和推送服务。

*   [gitweb [1]](https://git-scm.com/docs/gitweb) 为 Git 存储库提供了一个 Web 前端，可以使用 [git-instaweb [1]](https://git-scm.com/docs/git-instaweb) 脚本进行设置。

update hook howto 有一个管理共享中央存储库的好例子。

此外，还有许多其他广泛部署的托管，浏览和审查解决方案，例如：

*   gitolite，gerrit code review，cgit 等。

### 例子

```
 We assume the following in /etc/services 
```

```
$ grep 9418 /etc/services
git		9418/tcp		# Git Version Control System
```

```
 Run git-daemon to serve /pub/scm from inetd. 
```

```
$ grep git /etc/inetd.conf
git	stream	tcp	nowait	nobody \
  /usr/bin/git-daemon git-daemon --inetd --export-all /pub/scm
```

实际配置行应该在一行上。

```
 Run git-daemon to serve /pub/scm from xinetd. 
```

```
$ cat /etc/xinetd.d/git-daemon
# default: off
# description: The Git server offers access to Git repositories
service git
{
	disable = no
	type            = UNLISTED
	port            = 9418
	socket_type     = stream
	wait            = no
	user            = nobody
	server          = /usr/bin/git-daemon
	server_args     = --inetd --export-all --base-path=/pub/scm
	log_on_failure  += USERID
}
```

检查你的 xinetd（8）文档和设置，这是来自 Fedora 系统。其他人可能会有所不同

```
 Give push/pull only access to developers using git-over-ssh. 
```

例如那些使用：`$ git push/pull ssh://host.xz/pub/scm/project`

```
$ grep git /etc/passwd (1)
alice:x:1000:1000::/home/alice:/usr/bin/git-shell
bob:x:1001:1001::/home/bob:/usr/bin/git-shell
cindy:x:1002:1002::/home/cindy:/usr/bin/git-shell
david:x:1003:1003::/home/david:/usr/bin/git-shell
$ grep git /etc/shells (2)
/usr/bin/git-shell
```

1.  登录 shell 设置为/ usr / bin / git-shell，除了`git push`和`git pull`之外不允许任何其他内容。用户需要 ssh 访问机器。

2.  在许多发行版/ etc / shells 中需要列出用作登录 shell 的内容。

```
 CVS-style shared repository. 
```

```
$ grep git /etc/group (1)
git:x:9418:alice,bob,cindy,david
$ cd /home/devo.git
$ ls -l (2)
  lrwxrwxrwx   1 david git    17 Dec  4 22:40 HEAD -> refs/heads/master
  drwxrwsr-x   2 david git  4096 Dec  4 22:40 branches
  -rw-rw-r--   1 david git    84 Dec  4 22:40 config
  -rw-rw-r--   1 david git    58 Dec  4 22:40 description
  drwxrwsr-x   2 david git  4096 Dec  4 22:40 hooks
  -rw-rw-r--   1 david git 37504 Dec  4 22:40 index
  drwxrwsr-x   2 david git  4096 Dec  4 22:40 info
  drwxrwsr-x   4 david git  4096 Dec  4 22:40 objects
  drwxrwsr-x   4 david git  4096 Nov  7 14:58 refs
  drwxrwsr-x   2 david git  4096 Dec  4 22:40 remotes
$ ls -l hooks/update (3)
  -r-xr-xr-x   1 david git  3536 Dec  4 22:40 update
$ cat info/allowed-users (4)
refs/heads/master	alice\|cindy
refs/heads/doc-update	bob
refs/tags/v[0-9]*	david
```

1.  将开发人员放在同一个 git 组中。

2.  并使该组可写的共享存储库。

3.  使用来自 Documentation / howto /的 Carl 的 update-hook 示例进行分支策略控制。

4.  alice 和 cindy 可以推入主人，只有 bob 可以推进 doc-update。 david 是发布经理，是唯一可以创建和推送版本标签的人。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件