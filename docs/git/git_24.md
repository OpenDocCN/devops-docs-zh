# git-remote

> 原文： [`git-scm.com/docs/git-remote`](https://git-scm.com/docs/git-remote)

## 名称

git-remote - 管理一组跟踪的存储库

## 概要

```
git remote [-v | --verbose]
git remote add [-t <branch>] [-m <master>] [-f] [--[no-]tags] [--mirror=<fetch|push>] <name> <url>
git remote rename <old> <new>
git remote remove <name>
git remote set-head <name> (-a | --auto | -d | --delete | <branch>)
git remote set-branches [--add] <name> <branch>…​
git remote get-url [--push] [--all] <name>
git remote set-url [--push] <name> <newurl> [<oldurl>]
git remote set-url --add [--push] <name> <newurl>
git remote set-url --delete [--push] <name> <url>
git remote [-v | --verbose] show [-n] <name>…​
git remote prune [-n | --dry-run] <name>…​
git remote [-v | --verbose] update [-p | --prune] [(<group> | <remote>)…​]
```

## 描述

管理您跟踪其分支的存储库集（“远程”）。

## OPTIONS

```
 -v 
```

```
 --verbose 
```

稍微冗长一点，并在名字后显示远程网址。注意：必须放在`remote`和`subcommand`之间。

## COMMANDS

没有参数，显示现有遥控器的列表。有几个子命令可用于对遥控器执行操作。

```
 add 
```

添加名为&lt; name&gt;的远程名称对于&lt; url&gt;的存储库。然后，命令`git fetch &lt;name&gt;`可用于创建和更新远程跟踪分支&lt; name&gt; /&lt; branch&gt;。

使用`-f`选项，在设置远程信息后立即运行`git fetch &lt;name&gt;`。

使用`--tags`选项，`git fetch &lt;name&gt;`从远程存储库导入每个标记。

使用`--no-tags`选项，`git fetch &lt;name&gt;`不会从远程存储库导入标记。

默认情况下，仅导入已获取分支上的标记（请参阅 [git-fetch [1]](https://git-scm.com/docs/git-fetch) ）。

使用`-t &lt;branch&gt;`选项，而不是默认的 glob refspec 用于远程跟踪`refs/remotes/&lt;name&gt;/`命名空间下的所有分支，而是创建仅跟踪`&lt;branch&gt;`的 refspec。您可以提供多个`-t &lt;branch&gt;`来跟踪多个分支而不占用所有分支。

使用`-m &lt;master&gt;`选项，symbolic-ref `refs/remotes/&lt;name&gt;/HEAD`被设置为指向远程的`&lt;master&gt;`分支。另请参见 set-head 命令。

使用`--mirror=fetch`创建获取镜像时，refs 不会存储在 _refs / remotes /_ 命名空间中，而是遥控器上 _refs /_ 中的所有内容都将被直接镜像进入本地存储库中的 _refs /_ 。此选项仅在裸存储库中有意义，因为获取将覆盖任何本地提交。

使用`--mirror=push`创建推镜时，`git push`将始终表现为`--mirror`通过。

```
 rename 
```

重命名名为&lt; old&gt;的远程名称到&lt; new&gt;。将更新远程的所有远程跟踪分支和配置设置。

如果&lt; old&gt;和&lt; new&gt;是相同的，&lt; old&gt;是`$GIT_DIR/remotes`或`$GIT_DIR/branches`下的文件，远程转换为配置文件格式。

```
 remove 
```

```
 rm 
```

删除名为&lt; name&gt;的远程数据库。将删除远程的所有远程跟踪分支和配置设置。

```
 set-head 
```

设置或删除指定远程的默认分支（即 symbolic-ref `refs/remotes/&lt;name&gt;/HEAD`的目标）。不需要具有远程的默认分支，但允许指定远程的名称来代替特定分支。例如，如果`origin`的默认分支设置为`master`，则可以在通常指定`origin/master`的任何位置指定`origin`。

使用`-d`或`--delete`，删除符号 ref `refs/remotes/&lt;name&gt;/HEAD`。

使用`-a`或`--auto`，查询远程以确定其`HEAD`，然后将 symbolic-ref `refs/remotes/&lt;name&gt;/HEAD`设置为同一分支。例如，如果远程`HEAD`指向`next`，“`git remote set-head origin -a`”将 symbolic-ref `refs/remotes/origin/HEAD`设置为`refs/remotes/origin/next`。这仅在`refs/remotes/origin/next`已存在时才有效;如果不是，它必须先取出。

使用`&lt;branch&gt;`显式设置 symbolic-ref `refs/remotes/&lt;name&gt;/HEAD`。例如，“git remote set-head origin master”将 symbolic-ref `refs/remotes/origin/HEAD`设置为`refs/remotes/origin/master`。这仅在`refs/remotes/origin/master`已存在时才有效;如果不是，它必须先取出。

```
 set-branches 
```

更改命名远程跟踪的分支列表。在初始设置遥控器之后，这可用于跟踪可用远程分支的子集。

命名分支将被解释为使用 _git remote add_ 命令行上的`-t`选项指定。

使用`--add`，而不是替换当前跟踪的分支列表，添加到该列表。

```
 get-url 
```

检索远程的 URL。这里扩展了`insteadOf`和`pushInsteadOf`的配置。默认情况下，仅列出第一个 URL。

使用`--push`，将查询推送 URL 而不是提取 URL。

使用`--all`，将列出远程的所有 URL。

```
 set-url 
```

更改远程的 URL。设置远程&lt; name&gt;的第一个网址匹配正则表达式&lt; oldurl&gt; （如果没有给出&lt; oldurl&gt;则是第一个 URL）到&lt; newurl&gt;。如果&lt; oldurl&gt;与任何 URL 都不匹配，发生错误并且没有任何更改。

使用`--push`，操纵推送 URL 而不是获取 URL。

使用`--add`，不添加现有 URL，而是添加新 URL。

使用`--delete`，而不是更改现有网址，所有匹配正则表达式&lt; url&gt;的网址已删除远程&lt; name&gt;。尝试删除所有非推送 URL 是一个错误。

请注意，推送 URL 和提取 URL 即使可以设置不同，仍必须引用相同的位置。您推送到推送 URL 的内容应该是您从提取 URL 中立即获取的内容。如果您尝试从一个位置（例如您的上游）获取并推送到另一个位置（例如您的发布存储库），请使用两个单独的遥控器。

```
 show 
```

提供有关远程&lt; name&gt;的一些信息。

使用`-n`选项，不会先使用`git ls-remote &lt;name&gt;`查询远程磁头;而是使用缓存的信息。

```
 prune 
```

删除与&lt; name&gt;关联的陈旧引用。默认情况下，&lt; name&gt;下的过时远程跟踪分支被删除，但根据全局配置和远程配置，我们甚至可以修剪那些尚未推送的本地标签。相当于`git fetch --prune &lt;name&gt;`，但不会获取新的引用。

请参阅 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 的 PRUNING 部分，了解它将根据各种配置进行修剪的内容。

使用`--dry-run`选项，报告将修剪哪些分支，但不实际修剪它们。

```
 update 
```

获取由远程数据库定义的存储库中的远程数据库或远程组的更新。&lt; group&gt;。如果在命令行中既未指定 group 也未指定 remote，则将使用配置参数 remotes.default;如果未定义 remotes.default，则所有没有配置参数 remote 的遥控器将被更新。&lt; name&gt; .skipDefaultUpdate 设置为 true。 （参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

使用`--prune`选项，对所有已更新的遥控器运行修剪。

## 讨论

使用`remote.origin.url`和`remote.origin.fetch`配置变量实现远程配置。 （参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

## 例子

*   添加一个新的远程，获取，并从中检出一个分支

    ```
    $ git remote
    origin
    $ git branch -r
      origin/HEAD -&gt; origin/master
      origin/master
    $ git remote add staging git://git.kernel.org/.../gregkh/staging.git
    $ git remote
    origin
    staging
    $ git fetch staging
    ...
    From git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging
     * [new branch]      master     -&gt; staging/master
     * [new branch]      staging-linus -&gt; staging/staging-linus
     * [new branch]      staging-next -&gt; staging/staging-next
    $ git branch -r
      origin/HEAD -&gt; origin/master
      origin/master
      staging/master
      staging/staging-linus
      staging/staging-next
    $ git checkout -b staging staging/master
    ...
    ```

*   模仿 _git clone_ 但仅跟踪选定的分支

    ```
    $ mkdir project.git
    $ cd project.git
    $ git init
    $ git remote add -f -t master -m master origin git://example.com/git.git/
    $ git merge origin
    ```

## 也可以看看

[git-fetch [1]](https://git-scm.com/docs/git-fetch) [git-branch [1]](https://git-scm.com/docs/git-branch) [git-config [1]](https://git-scm.com/docs/git-config)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件