# git-show-ref

> 原文： [`git-scm.com/docs/git-show-ref`](https://git-scm.com/docs/git-show-ref)

## 名称

git-show-ref - 列出本地存储库中的引用

## 概要

```
git show-ref [-q|--quiet] [--verify] [--head] [-d|--dereference]
	     [-s|--hash[=<n>]] [--abbrev[=<n>]] [--tags]
	     [--heads] [--] [<pattern>…​]
git show-ref --exclude-existing[=<pattern>]
```

## 描述

显示本地存储库中可用的引用以及关联的提交 ID。可以使用模式过滤结果，并且可以将标记解除引用到对象 ID 中。此外，它还可用于测试特定引用是否存在。

默认情况下，显示标签，磁头和远程参考。

--exclude-existing 表单是一个反向的过滤器。它从 stdin 读取 refs，每行一个 ref，并显示本地存储库中不存在的那些。

鼓励使用此实用程序，以便直接访问`.git`目录下的文件。

## OPTIONS

```
 --head 
```

显示 HEAD 引用，即使它通常会被过滤掉。

```
 --heads 
```

```
 --tags 
```

分别限于“refs / heads”和“refs / tags”。这些选择并不相互排斥;当给出两者时，显示存储在“refs / heads”和“refs / tags”中的引用。

```
 -d 
```

```
 --dereference 
```

取消引用标记到对象 ID 中。它们将显示为附加“^ {}”。

```
 -s 
```

```
 --hash[=<n>] 
```

仅显示 SHA-1 哈希值，而不是引用名称。与--dereference 结合使用时，仍会在 SHA-1 之后显示解除引用的标记。

```
 --verify 
```

通过要求精确的 ref 路径来启用更严格的引用检查。除了返回错误代码 1 之外，如果未指定`--quiet`，它还将打印错误消息。

```
 --abbrev[=<n>] 
```

缩写对象名称。使用`--hash`时，您不必说`--hash --abbrev`; `--hash=n`会这样做。

```
 -q 
```

```
 --quiet 
```

不要将任何结果打印到 stdout。与`--verify`结合使用时，可以用于静默检查是否存在引用。

```
 --exclude-existing[=<pattern>] 
```

Make _git show-ref_ 充当从“`^(?:&lt;anything&gt;\s)?&lt;refname&gt;(?:\^{})?$`”形式的 stdin 读取 refs 的过滤器，并对每个执行以下操作：（1）在行尾添加“^ {}”如果有的话（2）忽略是否提供了模式并且不匹配 refname; （3）警告 refname 不是格式良好的 refname 并跳过; （4）忽略 refname 是否是本地存储库中存在的 ref; （5）否则输出该行。

```
 <pattern>…​ 
```

显示与一个或多个模式匹配的引用。模式从全名的末尾匹配，并且仅匹配完整的部分，例如， _master_ 匹配 _refs / heads / master_ ， _refs / remotes / origin / master_ ， _refs / tags / jedi / master_ 但不 _refs / heads / mymaster_ 或 _refs / remotes / master / jedi_ 。

## OUTPUT

输出格式为：_&lt; SHA-1 ID&gt;_ _&lt; space&gt;_ _&lt;参考名称&gt;_ 。

```
$ git show-ref --head --dereference
832e76a9899f560a90ffd62ae2ce83bbeff58f54 HEAD
832e76a9899f560a90ffd62ae2ce83bbeff58f54 refs/heads/master
832e76a9899f560a90ffd62ae2ce83bbeff58f54 refs/heads/origin
3521017556c5de4159da4615a39fa4d5d2c279b5 refs/tags/v0.99.9c
6ddc0964034342519a87fe013781abf31c6db6ad refs/tags/v0.99.9c^{}
055e4ae3ae6eb344cbabf2a5256a49ea66040131 refs/tags/v1.0rc4
423325a2d24638ddcc82ce47be5e40be550f4507 refs/tags/v1.0rc4^{}
...
```

当使用--hash（而不是--dereference）时，输出格式为：_&lt; SHA-1 ID&gt;_

```
$ git show-ref --heads --hash
2e3ba0114a1f52b47df29743d6915d056be13278
185008ae97960c8d551adcd9e23565194651b5d1
03adf42c988195b50e1a1935ba5fcbc39b2b029b
...
```

## 例子

要显示所有称为“master”的引用，无论是标记还是标题或其他任何内容，并且无论它们的引用命名层次结构有多深，请使用：

```
	git show-ref master
```

如果存在这样的引用，这将显示“refs / heads / master”以及“refs / remote / other-repo / master”。

使用`--verify`标志时，该命令需要一个确切的路径：

```
	git show-ref --verify refs/heads/master
```

只会匹配名为“master”的确切分支。

如果没有匹配， _git show-ref_ 将返回错误代码 1，并且在验证的情况下，它将显示错误消息。

对于脚本，你可以要求它安静地使用“--quiet”标志，它可以让你做类似的事情

```
	git show-ref --quiet --verify -- "refs/heads/$headname" ||
		echo "$headname is not a valid branch"
```

检查特定分支是否存在（请注意我们实际上不想显示任何结果，并且我们希望使用完整的 refname 以便不会触发模糊部分匹配的问题）。

要仅显示标记或仅显示正确的分支头，请分别使用“--tags”和/或“--heads”（使用两者表示它显示标记和头部，但不显示 refs /子目录下的其他随机引用）。

要进行自动标记对象解除引用，请使用“-d”或“--dereference”标志，这样就可以了

```
	git show-ref --tags --dereference
```

获取所有标签的列表以及它们取消引用的内容。

## FILES

`.git/refs/*`，`.git/packed-refs`

## 也可以看看

[git-for-each-ref [1]](https://git-scm.com/docs/git-for-each-ref) ， [git-ls-remote [1]](https://git-scm.com/docs/git-ls-remote) ， [git-update-ref [1]](https://git-scm.com/docs/git-update-ref) ， [gitrepository-layout [5]](https://git-scm.com/docs/gitrepository-layout)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件