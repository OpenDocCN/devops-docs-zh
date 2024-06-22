# git-request-pull

> 原文： [`git-scm.com/docs/git-request-pull`](https://git-scm.com/docs/git-request-pull)

## 名称

git-request-pull - 生成挂起更改的摘要

## 概要

```
git request-pull [-p] <start> <url> [<end>]
```

## 描述

生成一个请求，要求您的上游项目将更改提取到其树中。打印到标准输出的请求从分支描述开始，总结了更改并指示它们可以从哪里拉出。

上游项目应该具有`&lt;start&gt;`命名的提交，并且输出要求它通过访问由`&lt;url&gt;`命名的存储库来集成自提交以来所做的更改，直到`&lt;end&gt;`命名的提交。

## OPTIONS

```
 -p 
```

在输出中包含补丁文本。

```
 <start> 
```

承诺开始。这将命名已在上游历史记录中的提交。

```
 <url> 
```

要从中提取的存储库 URL。

```
 <end> 
```

承诺结束于（默认为 HEAD）。这会在您要求提取的历史记录的末尾命名提交。

当`&lt;url&gt;`命名的存储库在 ref 的一端提交与本地的 ref 不同时，可以使用`&lt;local&gt;:&lt;remote&gt;`语法，使其本地名称为冒号`:`，并且远程名称。

## 例子

想象一下，您在`v1.0`版本之上的`master`分支上构建了您的工作，并希望将其集成到项目中。首先，您将该更改推送到您的公共存储库，供其他人查看：

```
git push https://git.ko.xz/project master
```

然后，运行以下命令：

```
git request-pull v1.0 https://git.ko.xz/project master
```

它将向上游发出请求，总结`v1.0`版本和`master`之间的变化，从公共存储库中提取它。

如果您将更改推送到名称与本地名称不同的分支，例如

```
git push https://git.ko.xz/project master:for-linus
```

然后你可以要求被拉

```
git request-pull v1.0 https://git.ko.xz/project master:for-linus
```

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件