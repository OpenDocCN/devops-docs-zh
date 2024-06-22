# git-submodule

> 原文： [`git-scm.com/docs/git-submodule`](https://git-scm.com/docs/git-submodule)

## 名称

git-submodule - 初始化，更新或检查子模块

## 概要

```
git submodule [--quiet] add [<options>] [--] <repository> [<path>]
git submodule [--quiet] status [--cached] [--recursive] [--] [<path>…​]
git submodule [--quiet] init [--] [<path>…​]
git submodule [--quiet] deinit [-f|--force] (--all|[--] <path>…​)
git submodule [--quiet] update [<options>] [--] [<path>…​]
git submodule [--quiet] summary [<options>] [--] [<path>…​]
git submodule [--quiet] foreach [--recursive] <command>
git submodule [--quiet] sync [--recursive] [--] [<path>…​]
git submodule [--quiet] absorbgitdirs [--] [<path>…​]
```

## 描述

检查，更新和管理子模块。

有关子模块的更多信息，请参阅 [gitsubmodules [7]](https://git-scm.com/docs/gitsubmodules) 。

## COMMANDS

```
 add [-b <branch>] [-f|--force] [--name <name>] [--reference <repository>] [--depth <depth>] [--] <repository> [<path>] 
```

将给定存储库添加为给定路径下的子模块，该路径指向要在当前项目旁边提交的变更集：当前项目称为“超级项目”。

&lt;库&gt;是新子模块的原始存储库的 URL。这可以是绝对 URL，或者（如果它以./或../开头），相对于超级项目的默认远程存储库的位置（请注意，指定存储库 _foo.git_ ，这是位于超级项目 _bar.git_ 旁边，您必须使用 _../foo.git_ 而不是 _./foo.git_ - 作为一个在遵循相对 URL 规则时可能会有所期望 - 因为 Git 中相对 URL 的评估与相对目录的相同。

默认远程是当前分支的远程跟踪分支的远程。如果不存在这样的远程跟踪分支或者 HEAD 被分离，则假定“origin”是默认远程。如果超级项目没有配置默认远程，则超级项目是其自己的权威上游，而是使用当前工作目录。

可选参数&lt; path&gt;是克隆子模块在超级项目中存在的相对位置。如果&lt; path&gt;如果未给出，则使用源存储库的规范部分（“repo”表示“/path/to/repo.git”，“foo”表示“host.xz：foo / .git”）。如果&lt; path&gt;存在并且已经是一个有效的 Git 存储库，然后它将在没有克隆的情况下进行提交。 &lt;路径&gt;除非`--name`用于指定逻辑名称，否则它也会在其配置条目中用作子模块的逻辑名称。

给定的 URL 记录在`.gitmodules`中，供后续用户克隆超级项目使用。如果 URL 是相对于超级项目的存储库给出的，则推测是超级项目，子模块存储库将保存在同一相对位置，并且只需要提供超级项目的 URL。 git-submodule 将使用`.gitmodules`中的相对 URL 正确定位子模块。

```
 status [--cached] [--recursive] [--] [<path>…​] 
```

显示子模块的状态。这将打印每个子模块当前检出的提交的 SHA-1，以及子模块路径和 SHA-1 的 _git describe_ 的输出。如果子模块未初始化，则每个 SHA-1 可能以`-`为前缀，如果当前检出的子模块提交与包含存储库的索引中找到的 SHA-1 不匹配，则`+`和`U`如果子模块有合并冲突。

如果指定了`--recursive`，则此命令将递归到嵌套的子模块中，并显示其状态。

如果您只对当前初始化的子模块相对于索引或 HEAD 中记录的提交的更改感兴趣， [git-status [1]](https://git-scm.com/docs/git-status) 和 [git-diff [1]](https://git-scm.com/docs/git-diff) 也将提供该信息（并且还可以报告对子模块工作树的更改）。

```
 init [--] [<path>…​] 
```

通过在.git / config 中设置`submodule.$name.url`来初始化索引中记录的子模块（已在其他地方添加并提交）。它使用`.gitmodules`中的相同设置作为模板。如果 URL 是相对的，则将使用默认远程解析。如果没有默认远程，则假定当前存储库位于上游。

可选&lt;路径&gt;参数限制将初始化哪些子模块。如果未指定路径且已配置 submodule.active，则将初始化配置为活动的子模块，否则将初始化所有子模块。

如果存在，它还将复制`submodule.$name.update`的值。此命令不会更改.git / config 中的现有信息。然后，您可以在.git / config 中自定义子模块克隆 URL 以进行本地设置，然后继续`git submodule update`;如果您不打算自定义任何子模块位置，也可以在没有显式 _init_ 步骤的情况下使用`git submodule update --init`。

有关默认远程的定义，请参阅 add 子命令。

```
 deinit [-f|--force] (--all|[--] <path>…​) 
```

取消注册给定的子模块，即从.git / config 中删除整个`submodule.$name`部分及其工作树。对`git submodule update`，`git submodule foreach`和`git submodule sync`的进一步调用将跳过任何未注册的子模块，直到它们再次初始化为止，因此如果您不想再在工作树中本地检出子模块，请使用此命令。

当命令在没有 pathspec 的情况下运行时，它会出错，而不是去除所有内容，以防止出错。

如果指定了`--force`，则即使子模块包含本地修改，也将删除该子模块的工作树。

如果你真的想要从存储库中删除子模块并提交使用 [git-rm [1]](https://git-scm.com/docs/git-rm) 。有关删除选项，请参阅 [gitsubmodules [7]](https://git-scm.com/docs/gitsubmodules) 。

```
 update [--init] [--remote] [-N|--no-fetch] [--[no-]recommend-shallow] [-f|--force] [--checkout|--rebase|--merge] [--reference <repository>] [--depth <depth>] [--recursive] [--jobs <n>] [--] [<path>…​] 
```

通过克隆缺失的子模块并更新子模块的工作树，更新已注册的子模块以匹配超级项目所期望的内容。 “更新”可以通过多种方式完成，具体取决于命令行选项和`submodule.&lt;name&gt;.update`配置变量的值。命令行选项优先于配置变量。如果两者都没有给出，则执行 _ 检出 _。从命令行以及通过`submodule.&lt;name&gt;.update`配置支持的 _ 更新 _ 程序是：

```
 checkout 
```

超级项目中记录的提交将在分离的 HEAD 上的子模块中检出。

如果指定了`--force`，则子模块将被检出（使用`git checkout --force`），即使包含存储库的索引中指定的提交已经与子模块中检出的提交匹配。

```
 rebase 
```

子模块的当前分支将重新定位到超级项目中记录的提交。

```
 merge 
```

超级项目中记录的提交将合并到子模块中的当前分支中。

以下 _ 更新 _ 程序仅可通过`submodule.&lt;name&gt;.update`配置变量获得：

```
 custom command 
```

执行带有单个参数的任意 shell 命令（超级项目中记录的提交的 sha1）。当`submodule.&lt;name&gt;.update`设置为 _！命令 _ 时，感叹号后面的余数是自定义命令。

```
 none 
```

子模块未更新。

如果子模块尚未初始化，并且您只想使用`.gitmodules`中存储的设置，则可以使用`--init`选项自动初始化子模块。

如果指定了`--recursive`，则此命令将递归到已注册的子模块中，并更新其中的任何嵌套子模块。

```
 summary [--cached|--files] [(-n|--summary-limit) <n>] [commit] [--] [<path>…​] 
```

显示给定提交（默认为 HEAD）和工作树/索引之间的提交摘要。对于所讨论的子模块，显示了给定超级项目提交与索引或工作树（由`--cached`切换）之间的子模块中的一系列提交。如果给出了选项`--files`，则显示超级项目的索引与子模块的工作树之间的子模块中的一系列提交（此选项不允许使用`--cached`选项或提供显式承诺）。

在 [git-diff [1]](https://git-scm.com/docs/git-diff) 中使用`--submodule=log`选项也可以提供该信息。

```
 foreach [--recursive] <command> 
```

在每个签出的子模块中计算任意 shell 命令。该命令可以访问变量$ name，$ sm_path，$ displaypath，$ sha1 和$ toplevel：$ name 是`.gitmodules`中相关子模块部分的名称，$ sm_path 是直接记录的子模块的路径 superproject，$ displaypath 包含从当前工作目录到子模块根目录的相对路径，$ sha1 是直接超级项目中记录的提交，$ toplevel 是直接超级项目顶级的绝对路径。请注意，为了避免与 Windows 上的 _$ PATH_ 冲突， _$ path_ 变量现在是 _$ sm_path_ 变量的弃用同义词。此命令将忽略超级项目中定义但未检出的任何子模块。除非给出`--quiet`，否则 foreach 会在评估命令之前打印每个子模块的名称。如果给出了`--recursive`，则递归遍历子模块（即，给定的 shell 命令也在嵌套的子模块中进行计算）。任何子模块中命令的非零返回都会导致处理终止。这可以通过添加 _||来覆盖：_ 到命令结束。

例如，下面的命令将显示每个子模块的路径和当前检出的提交：

```
git submodule foreach 'echo $path `git rev-parse HEAD`'
```

```
 sync [--recursive] [--] [<path>…​] 
```

将子模块的远程 URL 配置设置与`.gitmodules`中指定的值同步。它只会影响那些已经在.git / config 中有 URL 条目的子模块（初始化或新添加时就是这种情况）。当子模块 URL 更改上游并且您需要相应地更新本地存储库时，这非常有用。

`git submodule sync`同步所有子模块，而`git submodule sync -- A`仅同步子模块“A”。

如果指定了`--recursive`，则此命令将递归到已注册的子模块中，并同步其中的任何嵌套子模块。

```
 absorbgitdirs 
```

如果子模块的 git 目录在子模块内，则将子模块的 git 目录移动到其 superprojects `$GIT_DIR/modules`路径，然后通过设置`core.worktree`并添加指向的.git 文件来连接 git 目录及其工作目录。嵌入在 superprojects git 目录中的 git 目录。

独立克隆并随后作为子模块或旧设置添加的存储库在子模块内部具有子模块 git 目录，而不是嵌入到 superprojects git 目录中。

默认情况下，此命令是递归的。

## OPTIONS

```
 -q 
```

```
 --quiet 
```

仅打印错误消息。

```
 --progress 
```

此选项仅对添加和更新命令有效。除非指定了-q，否则在将标准错误流附加到终端时，默认情况下会报告进度状态。即使标准错误流未定向到终端，此标志也会强制进度状态。

```
 --all 
```

此选项仅对 deinit 命令有效。取消注册工作树中的所有子模块。

```
 -b 
```

```
 --branch 
```

存储库的分支添加为子模块。分支名称在`update --remote`中记录为`update --remote`中的`submodule.&lt;name&gt;.branch`。 `.`的特殊值用于指示子模块中分支的名称应与当前存储库中的当前分支的名称相同。

```
 -f 
```

```
 --force 
```

此选项仅对 add，deinit 和 update 命令有效。运行 add 时，允许添加否则忽略的子模块路径。当运行 deinit 时，子模块工作树将被删除，即使它们包含本地更改。运行更新时（仅对结帐过程有效），在切换到其他提交时，丢弃子模块中的本地更改;并且始终在子模块中运行 checkout 操作，即使包含存储库的索引中列出的提交与子模块中签出的提交匹配也是如此。

```
 --cached 
```

此选项仅对 status 和 summary 命令有效。这些命令通常使用子模块 HEAD 中的提交，但使用此选项时，将使用存储在索引中的提交。

```
 --files 
```

此选项仅对 summary 命令有效。当使用此选项时，此命令将索引中的提交与子模块 HEAD 中的提交进行比较。

```
 -n 
```

```
 --summary-limit 
```

此选项仅对 summary 命令有效。限制摘要大小（总计显示的提交数）。给 0 将禁用摘要;负数表示无限制（默认值）。此限制仅适用于已修改的子模块。对于添加/删除/ typechanged 子模块，大小始终限制为 1。

```
 --remote 
```

此选项仅对 update 命令有效。不使用超级项目记录的 SHA-1 来更新子模块，而是使用子模块的远程跟踪分支的状态。使用的遥控器是分支的遥控器（`branch.&lt;name&gt;.remote`），默认为`origin`。使用的远程分支默认为`master`，但可以通过在`.gitmodules`或`.git/config`中设置`submodule.&lt;name&gt;.branch`选项来覆盖分支名称（优先使用`.git/config`）。

这适用于任何支持的更新过程（`--checkout`，`--rebase`等）。唯一的变化是目标 SHA-1 的来源。例如，`submodule update --remote --merge`会将上游子模块更改合并到子模块中，而`submodule update --merge`会将超级项目 gitlink 更改合并到子模块中。

为了确保当前跟踪分支状态，`update --remote`在计算 SHA-1 之前获取子模块的远程存储库。如果您不想获取，则应使用`submodule update --remote --no-fetch`。

使用此选项可将上游子项目的更改与子模块的当前 HEAD 集成。或者，您可以从子模块运行`git pull`，除了远程分支名称之外，它是等效的：`update --remote`使用默认上游存储库和`submodule.&lt;name&gt;.branch`，而`git pull`使用子模块的`branch.&lt;name&gt;.merge`。如果您想在超级项目中分配默认上游分支，请选择`submodule.&lt;name&gt;.branch`，如果您希望在子模块本身工作时想要更原始的感觉，请选择`branch.&lt;name&gt;.merge`。

```
 -N 
```

```
 --no-fetch 
```

此选项仅对 update 命令有效。不要从远程站点获取新对象。

```
 --checkout 
```

此选项仅对 update 命令有效。在子模块中的分离 HEAD 上签出超级项目中记录的提交。这是默认行为，此选项的主要用途是在设置为`checkout`以外的值时覆盖`submodule.$name.update`。如果未将键`submodule.$name.update`显式设置或设置为`checkout`，则此选项是隐式的。

```
 --merge 
```

此选项仅对 update 命令有效。将超级项目中记录的提交合并到子模块的当前分支中。如果给出此选项，则不会分离子模块的 HEAD。如果合并失败阻止了此过程，则必须使用通常的冲突解决工具解决子模块中产生的冲突。如果键`submodule.$name.update`设置为`merge`，则此选项是隐式的。

```
 --rebase 
```

此选项仅对 update 命令有效。将当前分支重新引导到超级项目中记录的提交。如果给出此选项，则不会分离子模块的 HEAD。如果合并失败阻止了此过程，则必须使用 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 解决这些故障。如果键`submodule.$name.update`设置为`rebase`，则此选项是隐式的。

```
 --init 
```

此选项仅对 update 命令有效。初始化所有在更新之前尚未调用“git submodule init”的子模块。

```
 --name 
```

此选项仅对 add 命令有效。它将子模块的名称设置为给定的字符串，而不是默认为其路径。该名称必须作为目录名有效，并且不能以 _/_ 结尾。

```
 --reference <repository> 
```

此选项仅对添加和更新命令有效。这些命令有时需要克隆远程存储库。在这种情况下，此选项将传递给 [git-clone [1]](https://git-scm.com/docs/git-clone) 命令。

**注**：**不是**使用此选项除非您已阅读 [git-clone [1]](https://git-scm.com/docs/git-clone) 的`--reference`，`--shared`和`--dissociate`选项仔细。

```
 --dissociate 
```

此选项仅对添加和更新命令有效。这些命令有时需要克隆远程存储库。在这种情况下，此选项将传递给 [git-clone [1]](https://git-scm.com/docs/git-clone) 命令。

**注**：参见`--reference`选项的注意事项。

```
 --recursive 
```

此选项仅对 foreach，update，status 和 sync 命令有效。递归遍历子模块。该操作不仅在当前仓库的子模块中执行，而且还在这些子模块内的任何嵌套子模块中执行（依此类推）。

```
 --depth 
```

此选项对添加和更新命令有效。创建一个 _ 浅 _ 克隆，其历史记录被截断为指定的修订数。见 [git-clone [1]](https://git-scm.com/docs/git-clone)

```
 --[no-]recommend-shallow 
```

此选项仅对 update 命令有效。子模块的初始克隆将使用默认情况下`.gitmodules`文件提供的推荐`submodule.&lt;name&gt;.shallow`。要忽略建议，请使用`--no-recommend-shallow`。

```
 -j <n> 
```

```
 --jobs <n> 
```

此选项仅对 update 命令有效。与多个作业并行克隆新的子模块。默认为`submodule.fetchJobs`选项。

```
 <path>…​ 
```

子模块的路径。指定时，这将限制命令仅对指定路径上找到的子模块进行操作。 （添加时需要此参数）。

## FILES

初始化子模块时，使用包含存储库的顶级目录中的`.gitmodules`文件来查找每个子模块的 URL。该文件的格式应与`$GIT_DIR/config`相同。每个子模块 url 的关键是“submodule。$ name.url”。有关详细信息，请参阅 [gitmodules [5]](https://git-scm.com/docs/gitmodules) 。

## 也可以看看

[gitsubmodules [7]](https://git-scm.com/docs/gitsubmodules) ， [gitmodules [5]](https://git-scm.com/docs/gitmodules) 。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件