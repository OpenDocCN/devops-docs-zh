# git-bundle

> 原文： [`git-scm.com/docs/git-bundle`](https://git-scm.com/docs/git-bundle)

## 名称

git-bundle - 通过归档移动对象和引用

## 概要

```
git bundle create <file> <git-rev-list-args>
git bundle verify <file>
git bundle list-heads <file> [<refname>…​]
git bundle unbundle <file> [<refname>…​]
```

## 描述

某些工作流程要求在一台计算机上的一个或多个开发分支在另一台计算机上复制，但这两台计算机无法直接连接，因此无法使用交互式 Git 协议（git，ssh，http）。此命令支持 _git fetch_ 和 _git pull_ 通过在原始机器的存档中打包对象和引用来操作，然后使用 _git fetch 将它们导入另一个存储库通过某种方式（例如，通过 sneakernet）移动存档后，HTG5]和 _git pull_ 。由于存储库之间不存在直接连接，因此用户必须为目标存储库保存的包指定基础：包假定基础中的所有对象都已存在于目标存储库中。_

## OPTIONS

```
 create <file> 
```

用于创建名为 _ 文件 _ 的包。这需要 _git-rev-list-args_ 参数来定义包内容。

```
 verify <file> 
```

用于检查捆绑包文件是否有效，并将干净地应用于当前存储库。这包括检查 bundle 格式本身以及检查先决条件提交是否存在并在当前存储库中完全链接。 _git bundle_ 打印缺失提交列表（如果有），并以非零状态退出。

```
 list-heads <file> 
```

列出捆绑中定义的引用。如果后跟一个引用列表，则只打印出与给定引用匹配的引用。

```
 unbundle <file> 
```

将包中的对象传递给 _git index-pack_ 以存储在存储库中，然后打印所有已定义引用的名称。如果给出了引用列表，则仅打印与列表中的引用匹配的引用。这个命令实际上是管道，只能由 _git fetch_ 调用。

```
 <git-rev-list-args> 
```

_git rev-parse_ 和 _git rev-list_ （包含命名 ref，参见下面的 SPECIFYING REFERENCES）可接受的参数列表，用于指定特定对象和对传输的引用。例如，`master~10..master`导致当前主引用与自其第 10 个祖先提交以来添加的所有对象一起打包。可以打包的引用和对象的数量没有明确的限制。

```
 [<refname>…​] 
```

用于限制报告为可用的引用的引用列表。这主要用于 _git fetch_ ，它希望只接收那些被请求的引用，而不一定是包中的所有东西（在这种情况下， _git bundle_ 就像 _git fetch-pack_ ）。

## 指定参考

_git bundle_ 只会打包由 _git show-ref_ 显示的引用：这包括头部，标签和远程头部。诸如`master~1`之类的参考文献无法打包，但非常适合定义基础。可以打包多个参考，并且可以指定多个参考。包装的对象是未包含在给定碱基的联合中的对象。每个基础可以明确指定（例如`^master~10`），或隐式指定（例如`master~10..master`，`--since=10.days.ago master`）。

目的地使用的基础非常重要。可以谨慎行事，导致捆绑文件包含目标中已有的对象，因为在目的地解包时会忽略这些对象。

## 例子

假设您要将历史记录从计算机 A 上的存储库 R1 传输到计算机 B 上的另一个存储库 R2。无论出于何种原因，不允许 A 和 B 之间的直接连接，但我们可以通过某种机制将数据从 A 移动到 B. ，电子邮件等）。我们希望通过在 R1 中的分支主机上进行的开发来更新 R2。

要引导该进程，您可以先创建一个没有任何基础的包。您可以使用标记记住上次处理的提交，以便以后使用增量包更新其他存储库：

```
machineA$ cd R1
machineA$ git bundle create file.bundle master
machineA$ git tag -f lastR2bundle master
```

然后将 file.bundle 传输到目标机器 B.由于此捆绑包不需要提取任何现有对象，因此可以通过克隆从机器 B 上创建新的存储库：

```
machineB$ git clone -b master /home/me/tmp/file.bundle R2
```

这将在结果存储库中定义一个名为“origin”的远程，它允许您从包中获取和提取。 R2 中的$ GIT_DIR / config 文件将具有如下条目：

```
[remote "origin"]
    url = /home/me/tmp/file.bundle
    fetch = refs/heads/*:refs/remotes/origin/*
```

要更新生成的 mine.git 存储库，可以在使用增量更新替换存储在/home/me/tmp/file.bundle 中的软件包之后进行提取或提取。

在原始存储库中进行更多工作之后，您可以创建增量包以更新其他存储库：

```
machineA$ cd R1
machineA$ git bundle create file.bundle lastR2bundle..master
machineA$ git tag -f lastR2bundle master
```

然后将捆绑包转移到另一台机器上以替换/home/me/tmp/file.bundle，并从中拉出。

```
machineB$ cd R2
machineB$ git pull
```

如果您知道预期的收件人存储库应该具有必要的对象的提交，您可以使用该知识来指定基础，给出一个截止点来限制生成的包中的修订和对象。前面的示例使用 lastR2bundle 标记用于此目的，但您可以使用您将为 [git-log [1]](https://git-scm.com/docs/git-log) 命令提供的任何其他选项。以下是更多示例：

您可以使用两者中都存在的标记：

```
$ git bundle create mybundle v1.0.0..master
```

您可以根据时间使用基础：

```
$ git bundle create mybundle --since=10.days master
```

您可以使用提交次数：

```
$ git bundle create mybundle -10 master
```

您可以运行`git-bundle verify`以查看是否可以从使用基础创建的包中提取：

```
$ git bundle verify mybundle
```

这将列出您必须具有的提交以从包中提取，如果您没有它们将会出错。

从收件人存储库的角度来看，捆绑包就像它从中取出或取出的常规存储库。例如，您可以在获取时映射引用：

```
$ git fetch mybundle master:localRef
```

您还可以查看它提供的参考资料：

```
$ git ls-remote mybundle
```

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件