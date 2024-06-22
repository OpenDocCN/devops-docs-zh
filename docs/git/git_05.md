# git-clone

> 原文： [`git-scm.com/docs/git-clone`](https://git-scm.com/docs/git-clone)
> 
> 贡献者：[honglyua](https://github.com/honglyua)

## 名称

git-clone - 将存储库克隆到新目录中

## 概要

```
git clone [--template=<template_directory>]
	  [-l] [-s] [--no-hardlinks] [-q] [-n] [--bare] [--mirror]
	  [-o <name>] [-b <name>] [-u <upload-pack>] [--reference <repository>]
	  [--dissociate] [--separate-git-dir <git dir>]
	  [--depth <depth>] [--[no-]single-branch] [--no-tags]
	  [--recurse-submodules[=<pathspec>]] [--[no-]shallow-submodules]
	  [--jobs <n>] [--] <repository> [<directory>]
```

## 描述

将存储库克隆到新创建的目录中，为克隆存储库中的每个分支创建远程跟踪分支（使用`git branch -r`可见），并创建并检出从克隆存储库的当前活动分支的初始分支。

在克隆之后，没有参数的普通`git fetch`将更新所有远程跟踪分支，并且没有参数的`git pull`将另外将远程主分支合并到当前主分支中（如果有"--single-branch“的话，见下文）。

通过在`refs/remotes/origin`下创建对远程分支头的引用并初始化`remote.origin.url`和`remote.origin.fetch`配置变量来实现此默认配置。

## 选项

```
 --local 
```

```
 -l 
```

当要克隆的存储库位于本地计算机上时，此标志会绕过正常的“Git 感知”传输机制，并通过制作 HEAD 以及对象和 refs 目录下的所有内容的副本来克隆存储库。 `.git/objects/`目录下的文件是硬链接的，以便在可能的情况下节省空间。

如果将存储库指定为本地路径（例如，`/path/to/repo`），则这是默认值，而--local 本质上是无操作。如果将存储库指定为 URL，则忽略此标志（并且我们从不使用本地优化）。当给出`/path/to/repo`时，指定`--no-local`将覆盖默认值，而是使用常规 Git 传输。

```
 --no-hardlinks 
```

从本地文件系统上的存储库强制克隆进程，以复制`.git/objects`目录下的文件，而不是使用硬链接。如果您尝试备份存储库，则可能需要这样做。

```
 --shared 
```

```
 -s 
```

当要克隆的存储库位于本地计算机上而不是使用硬链接时，会自动设置`.git/objects/info/alternates`以与源存储库共享对象。生成的存储库在没有任何自己的对象的情况下开始。

**注**：这可能是危险的操作;**不要**使用它，除非你明白它的作用。如果使用此选项克隆存储库，然后在源存储库中删除分支（或使用任何其他提交未引用的 Git 命令），则某些对象可能会变为未引用（或悬空）。这些对象可以通过自动调用`git gc --auto`的普通 Git 操作（例如`git commit`）删除。 （参见 [git-gc [1]](https://git-scm.com/docs/git-gc) 。）如果这些对象被删除并被克隆的存储库引用，那么克隆的存储库将会损坏。

请注意，在使用`-s`克隆的存储库中运行没有`-l`选项的`git repack`会将源存储库中的对象复制到克隆存储库中的包中，从而节省`clone -s`的磁盘空间节省。但是，运行`git gc`是安全的，它默认使用`-l`选项。

如果要在其源存储库中中断使用`-s`克隆的存储库的依赖关系，只需运行`git repack -a`即可将源存储库中的所有对象复制到克隆存储库中的包中。

```
 --reference[-if-able] <repository> 
```

如果引用存储库位于本地计算机上，则自动设置`.git/objects/info/alternates`以从引用存储库获取对象。使用现有存储库作为备用存储库，将需要从克隆的存储库中复制更少的对象，从而降低网络和本地存储成本。使用`--reference-if-able`时，将跳过不存在的目录，并显示警告而不是中止克隆。

**注**：参见`--shared`选项的注释，以及`--dissociate`选项。

```
 --dissociate 
```

借用`--reference`选项指定的引用存储库中的对象，仅减少网络传输，并在通过制作必要的借用对象本地副本进行克隆后停止从它们借用。当已经从另一个存储库借用对象的存储库本地克隆时，也可以使用此选项 - 新存储库将从同一存储库中借用对象，并且此选项可用于停止借用。

```
 --quiet 
```

```
 -q 
```

安静地操作。未向标准错误流报告进度。

```
 --verbose 
```

```
 -v 
```

详细地运行。不影响将进度状态报告给标准错误流。

```
 --progress 
```

除非指定了-q，否则在将标准错误流附加到终端时，默认情况下会报告进度状态。即使标准错误流未定向到终端，此标志也会强制进度状态。

```
 --no-checkout 
```

```
 -n 
```

克隆完成后不会检查 HEAD。

```
 --bare 
```

制作一个 _bare_ Git 存储库。也就是说，不是创建`<directory>`并将管理文件放在`<directory>/.git`中，而是将`<directory>`本身设为`$GIT_DIR`。这显然意味着`-n`，因为无处可查看工作树。此外，远端上的分支头直接复制到相应的本地分支头，而不将它们映射到`refs/remotes/origin/`。使用此选项时，既不会创建远程跟踪分支，也不会创建相关的配置变量。

```
 --mirror 
```

设置源存储库的镜像。这类似`--bare`。与`--bare`相比，`--mirror`不仅将源的本地分支映射到目标的本地分支，它还映射所有引用（包括远程跟踪分支，注释等）并设置 refspec 配置，以便所有这些引用被目标存储库中的`git remote update`覆盖。

```
 --origin <name> 
```

```
 -o <name> 
```

不使用远程名称`origin`来跟踪上游存储库，而是使用`<name>`。

```
 --branch <name> 
```

```
 -b <name> 
```

而不是将新创建的 HEAD 指向克隆存储库的 HEAD 所指向的分支，而是指向`<name>`分支。在非裸存储库中，这是将要检出的分支。 `--branch`还可以在生成的存储库中的获取 tags 和分离 HEAD。

```
 --upload-pack <upload-pack> 
```

```
 -u <upload-pack> 
```

给定时，通过 ssh 访问要克隆的存储库，这将指定另一端运行的命令的非默认路径。

```
 --template=<template_directory> 
```

指定将使用模板的目录; （参见 [git-init [1]](https://git-scm.com/docs/git-init) 的“TEMPLATE DIRECTORY”部分。）

```
 --config <key>=<value> 
```

```
 -c <key>=<value> 
```

在新创建的存储库中设置配置变量;这在初始化存储库之后，但在获取远程历史记录或检出任何文件之前立即生效。key 的格式与 [git-config [1]](https://git-scm.com/docs/git-config) （例如`core.eol=true`）的格式相同。如果为同一个键指定了多个值，则每个值都将写入配置文件。例如，这样就可以安全地向源远程添加额外的 fetch refspec。

由于当前实现的限制，一些配置变量在初始提取和检出之后才会生效。已知未生效的配置变量为：`remote.<name>.mirror`和`remote.<name>.tagOpt`。请改用相应的`--mirror`和`--no-tags`选项。

```
 --depth <depth> 
```

创建一个 _ 浅 _ 克隆，其历史记录被截断为指定的提交次数。除非`--no-single-branch`用于获取所有分支的提示附近的历史，否则意味着`--single-branch`。如果要浅层克隆子模块，也要传递`--shallow-submodules`。

```
 --shallow-since=<date> 
```

在指定时间后创建具有历史记录的浅层克隆。

```
 --shallow-exclude=<revision> 
```

创建具有历史记录的浅层克隆，不包括可从指定的远程分支或标记访问的提交。可以多次指定此选项。

```
 --[no-]single-branch 
```

仅克隆导致单个分支尖端的历史记录，由`--branch`选项指定或主分支远程的`HEAD`指向。进一步提取到生成的存储库只会更新分支的远程跟踪分支，此选项用于初始克隆。如果在进行`--single-branch`克隆时远程处的 HEAD 未指向任何分支，则不会创建远程跟踪分支。

```
 --no-tags 
```

不要克隆任何标签，并在配置中设置`remote.<remote>.tagOpt=--no-tags`，确保将来的`git pull`和`git fetch`操作不会跟随任何标签。后续显式标记提取仍然有效（参见 [git-fetch [1]](https://git-scm.com/docs/git-fetch) ）。

可以与`--single-branch`一起使用来克隆和维护一个除了单个克隆分支之外没有引用的分支。这很有用，例如维护某些存储库的默认分支的最小克隆以进行搜索索引。

```
 --recurse-submodules[=<pathspec] 
```

创建克隆后，根据提供的 pathspec 初始化和克隆子模块。如果未提供 pathspec，则初始化并克隆所有子模块。对于包含多个条目的 pathspec，可以多次给出此选项。生成的克隆将`submodule.active`设置为提供的 pathspec，或“.” （如果没有提供 pathspec，则表示所有子模块）。

子模块使用其默认设置进行初始化和克隆。这相当于克隆完成后立即运行`git submodule update --init --recursive <pathspec>`。如果克隆的存储库没有工作树/检出（即，如果给出`--no-checkout` / `-n`，`--bare`或`--mirror`中的任何一个），则忽略此选项

```
 --[no-]shallow-submodules 
```

克隆的所有子模块都是浅的，深度为 1。

```
 --separate-git-dir=<git dir> 
```

不要将克隆的存储库放在应该位于的位置，而是将克隆的存储库放在指定的目录中，然后创建与文件系统无关的 Git 符号链接。结果是 Git 存储库可以与工作树分开。

```
 -j <n> 
```

```
 --jobs <n> 
```

同时获取的子模块数。默认为`submodule.fetchJobs`选项。

```
 <repository> 
```

要从中克隆的（可能是远程的）存储库。有关指定存储库的更多信息，请参见下面的 GIT URL 部分。

```
 <directory> 
```

要克隆到的新目录的名称。如果没有明确给出目录（`/path/to/repo.git`为`/path/to/repo.git`，`host.xz:foo/.git`为`foo`），则使用源存储库的“人性化”部分。仅当目录为空时才允许克隆到现有目录中。

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

*   [用户@] host.xz：path/to/ repo.git /

只有在第一个冒号之前没有斜杠时才会识别此语法。这有助于区分包含冒号的本地路径。例如，本地路径`foo:bar`可以指定为绝对路径或`./foo:bar`，以避免被误解为 ssh url。

ssh 和 git 协议还支持〜用户名扩展：

*   SSH：// [用户@] host.xz [：端口] /〜[用户] /path/to/repo.git/

*   GIT 中：//host.xz [：端口] /〜[用户] /path/to/repo.git/

*   [用户@] host.xz：/〜[用户] /path/to/repo.git/

对于本地也受 Git 支持的本地存储库，可以使用以下语法：

*   /path/to/repo.git/

*   文件：///path/to/repo.git/

这两种语法大多是等价的，除了前者暗示--local 选项。

当 Git 不知道如何处理某种传输协议时，它会尝试使用 _remote-<transport>_ 远程助手，如果存在的话。要显式请求远程帮助程序，可以使用以下语法：

*   <transport>::<address>

其中<address>可以是路径，服务器，或者由被调用的特定远程助手识别的任意类似 URL 的字符串。有关详细信息，请参阅 [gitremote-helpers [1]](https://git-scm.com/docs/gitremote-helpers) 。

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

像“work：repo.git”这样的 URL 或类似“host.xz：/path/to/repo.git”的 URL 将在任何带有 URL 的上下文中被重写为“git：//git.host.xz/repo.git”。

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

## 例子

*   从上游克隆：

    ```
    $ git clone git://git.kernel.org/pub/scm/.../linux.git my-linux
    $ cd my-linux
    $ make
    ```

*   创建一个从当前目录借用的本地克隆，而不检查：

    ```
    $ git clone -l -s -n . ../copy
    $ cd ../copy
    $ git show-branch
    ```

*   从现有本地目录借用时从上游克隆：

    ```
    $ git clone --reference /git/linux.git \
    	git://git.kernel.org/pub/scm/.../linux.git \
    	my-linux
    $ cd my-linux
    ```

*   创建一个裸存储库以将更改发布到公共：

    ```
    $ git clone --bare -l /home/proj/.git /pub/scm/proj.git
    ```

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件