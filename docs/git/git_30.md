# git-describe

> 原文： [`git-scm.com/docs/git-describe`](https://git-scm.com/docs/git-describe)

## 名称

git-describe - 根据可用的 ref 给对象一个人类可读的名称

## 概要

```
git describe [--all] [--tags] [--contains] [--abbrev=<n>] [<commit-ish>…​]
git describe [--all] [--tags] [--contains] [--abbrev=<n>] --dirty[=<mark>]
git describe <blob>
```

## 描述

该命令查找可从提交访问的最新标记。如果标记指向提交，则仅显示标记。否则，它将标记名称后缀为标记对象顶部的附加提交数和最近提交的缩写对象名称。结果是一个“人类可读”的对象名称，它也可用于标识对其他 git 命令的提交。

默认情况下（不带--all 或--tags）`git describe`仅显示带注释的标签。有关创建带注释标签的更多信息，请参阅 [git-tag [1]](https://git-scm.com/docs/git-tag) 的-a 和-s 选项。

如果给定对象引用 blob，则将其描述为`&lt;commit-ish&gt;:&lt;path&gt;`，以便可以在`&lt;commit-ish&gt;`中的`&lt;path&gt;`处找到 blob，它本身描述了在反向修订中出现此 blob 的第一次提交从 HEAD 步行。

## OPTIONS

```
 <commit-ish>…​ 
```

要描述的 Commit-ish 对象名称。如果省略，则默认为 HEAD。

```
 --dirty[=<mark>] 
```

```
 --broken[=<mark>] 
```

描述工作树的状态。当工作树与 HEAD 匹配时，输出与“git describe HEAD”相同。如果工作树具有本地修改，则附加“-dirty”。如果存储库已损坏并且 Git 无法确定是否存在本地修改，则 Git 将出错，除非给出“--broken”，而后缀为“-broken”。

```
 --all 
```

不要只使用带注释的标签，而是使用`refs/`命名空间中的任何引用。此选项可以匹配任何已知分支，远程跟踪分支或轻量级标记。

```
 --tags 
```

不使用带注释的标签，而是使用`refs/tags`命名空间中的任何标签。此选项可以匹配轻量级（非注释）标记。

```
 --contains 
```

而不是找到提交之前的标记，找到提交后出现的标记，从而包含它。自动暗示--tags。

```
 --abbrev=<n> 
```

不使用默认的 7 个十六进制数字作为缩写对象名称，而是使用&lt; n&gt;数字，或形成唯一对象名称所需的数字。 &lt; n&gt; 0 将禁止长格式，仅显示最接近的标记。

```
 --candidates=<n> 
```

而不是仅仅考虑 10 个最近的标签作为描述输入提交的候选者，而是考虑到&lt; n&gt;。候选人。增加&lt; n&gt;超过 10 将需要稍长但可能产生更准确的结果。 &lt; n&gt;为 0 将导致仅输出完全匹配。

```
 --exact-match 
```

仅输出完全匹配（标记直接引用提供的提交）。这是--candidates = 0 的同义词。

```
 --debug 
```

详细显示有关用于标准错误的搜索策略的信息。标签名称仍将打印到标准输出。

```
 --long 
```

即使与标记匹配，也始终输出长格式（标记，提交数和缩写提交名称）。当您想要在“describe”输出中查看提交对象名称的某些部分时，这很有用，即使有问题的提交恰好是标记版本。它不会仅仅发出标记名称，而是将这样的提交描述为 v1.2-0-gdeadbee（自标记 v1.2 以来第 0 次提交指向对象 deadbee ...）。

```
 --match <pattern> 
```

仅考虑与给定`glob(7)`模式匹配的标记，不包括“refs / tags /”前缀。如果与`--all`一起使用，它还会考虑与模式匹配的本地分支和远程跟踪引用，分别排除“refs / heads /”和“refs / remotes /”前缀;从不考虑其他类型的参考。如果多次给出，将累积模式列表，并且将考虑匹配任何模式的标签。使用`--no-match`清除和重置模式列表。

```
 --exclude <pattern> 
```

不要考虑与给定`glob(7)`模式匹配的标记，不包括“refs / tags /”前缀。如果与`--all`一起使用，它也不会考虑与模式匹配的本地分支和远程跟踪引用，分别排除“refs / heads /”和“refs / remotes /”前缀;从不考虑其他类型的参考。如果多次给出，则将累积模式列表，并且将排除匹配任何模式的标签。与--match 结合使用时，如果标记与至少一个匹配模式匹配且与任何--exclude 模式不匹配，则会考虑使用该标记。使用`--no-exclude`清除和重置模式列表。

```
 --always 
```

将唯一缩写的提交对象显示为后备。

```
 --first-parent 
```

在看到合并提交时，仅遵循第一个父提交。当您希望不匹配目标提交历史记录中合并的分支上的标记时，这非常有用。

## 例子

有了类似 git.git 当前树的东西，我得到：

```
[torvalds@g5 git]$ git describe parent
v1.0.4-14-g2414721
```

即我的“父”分支的当前头部基于 v1.0.4，但由于它上面有一些提交，因此 describe 添加了额外提交的数量（“14”）和提交的缩写对象名称本身（“2414721”）在最后。

附加提交的数量是“git log v1.0.4..parent”显示的提交数。哈希后缀是父项提示提交的“-g”+ 7-char 缩写（即`2414721b194453f058079d897d13c4e377f92dc6`）。 “g”前缀代表“git”，用于根据管理软件的 SCM 描述软件版本。这在人们可能使用不同 SCM 的环境中很有用。

在标签名称上执行 _git describe_ 只会显示标签名称：

```
[torvalds@g5 git]$ git describe v1.0.4
v1.0.4
```

使用--all，该命令可以使用分支头作为引用，因此输出也显示引用路径：

```
[torvalds@g5 git]$ git describe --all --abbrev=4 v1.0.5²
tags/v1.0.0-21-g975b
```

```
[torvalds@g5 git]$ git describe --all --abbrev=4 HEAD^
heads/lt/describe-7-g975b
```

将--abbrev 设置为 0，该命令可用于查找最接近的标记名，不带任何后缀：

```
[torvalds@g5 git]$ git describe --abbrev=0 v1.0.5²
tags/v1.0.0
```

请注意，如果今天键入这些命令，您获得的后缀可能比 Linus 在运行这些命令时看到的更长，因为您的 Git 存储库可能有新的提交，其对象名称以 975b 开头，当时不存在，并且“ - g975b“单独的后缀可能不足以消除这些提交的歧义。

## 搜索策略

对于每个提交的提交， _git describe_ 将首先查找标记该提交的标记。带注释的标签将始终优先于轻量级标签，具有较新日期的标签将始终优先于具有较旧日期的标签。如果找到完全匹配，将输出其名称并停止搜索。

如果未找到完全匹配， _git describe_ 将返回提交历史记录以找到已标记的祖先提交。祖先的标记将与输入 commit-ish 的 SHA-1 的缩写一起输出。如果指定了`--first-parent`，则 walk 将仅考虑每个提交的第一个父级。

如果在步行期间发现多个标签，则将选择并输出具有与输入 commit-ish 不同的最少提交的标签。这里提交的最小不同定义为`git log tag..input`显示的提交数量将是可能的最小提交数量。

## BUGS

无法描述树对象以及不指向提交的标记对象。在描述 blob 时，忽略指向 blob 的轻量级标记，但 blob 仍被描述为&lt; committ-ish&gt;：&lt; path&gt;尽管轻量级标签是有利的。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件