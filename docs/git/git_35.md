# git-revert

> 原文： [`git-scm.com/docs/git-revert`](https://git-scm.com/docs/git-revert)

## 名称

git-revert - 恢复一些现有的提交

## 概要

```
git revert [--[no-]edit] [-n] [-m parent-number] [-s] [-S[<keyid>]] <commit>…​
git revert --continue
git revert --quit
git revert --abort
```

## 描述

给定一个或多个现有提交，还原相关修补程序引入的更改，并记录一些记录它们的新提交。这需要您的工作树是干净的（没有 HEAD 提交的修改）。

注意： _git revert_ 用于记录一些新的提交以反转某些早期提交的效果（通常只有一个错误的提交）。如果你想丢弃工作目录中所有未提交的更改，你应该看到 [git-reset [1]](https://git-scm.com/docs/git-reset) ，特别是`--hard`选项。如果你想在另一个提交中提取特定文件，你应该看到 [git-checkout [1]](https://git-scm.com/docs/git-checkout) ，特别是`git checkout &lt;commit&gt; -- &lt;filename&gt;`语法。请注意这些替代方案，因为它们都会丢弃工作目录中未提交的更改。

## OPTIONS

```
 <commit>…​ 
```

承诺恢复。有关拼写提交名称的更完整列表，请参阅 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 。也可以给出提交集，但默认情况下不进行遍历，请参阅 [git-rev-list [1]](https://git-scm.com/docs/git-rev-list) 及其`--no-walk`选项。

```
 -e 
```

```
 --edit 
```

使用此选项， _git revert_ 将允许您在提交恢复之前编辑提交消息。如果从终端运行命令，则这是默认设置。

```
 -m parent-number 
```

```
 --mainline parent-number 
```

通常，您无法还原合并，因为您不知道合并的哪一侧应被视为主线。此选项指定主线的父编号（从 1 开始），并允许恢复相对于指定父级的更改。

还原合并提交声明您永远不会希望合并带来的树更改。因此，以后的合并只会带来由不是先前还原的合并的祖先的提交引入的树更改。这可能是也可能不是你想要的。

有关详细信息，请参阅 revert-a-faulty-merge How-To 。

```
 --no-edit 
```

使用此选项， _git revert_ 将不会启动提交消息编辑器。

```
 -n 
```

```
 --no-commit 
```

通常，该命令会自动创建一些提交日志消息，提交哪些提交已被还原。此标志应用将命名提交还原到工作树和索引所需的更改，但不进行提交。此外，使用此选项时，索引不必与 HEAD 提交匹配。恢复是针对索引的开始状态完成的。

在将多个提交效果还原到行中的索引时，这非常有用。

```
 -S[<keyid>] 
```

```
 --gpg-sign[=<keyid>] 
```

GPG 签名提交。 `keyid`参数是可选的，默认为提交者标识;如果指定，它必须粘在没有空格的选项上。

```
 -s 
```

```
 --signoff 
```

在提交消息的末尾添加 Sign-by-by 行。有关详细信息，请参阅 [git-commit [1]](https://git-scm.com/docs/git-commit) 中的签收选项。

```
 --strategy=<strategy> 
```

使用给定的合并策略。应该只使用一次。有关详细信息，请参阅 [git-merge [1]](https://git-scm.com/docs/git-merge) 中的 MERGE STRATEGIES 部分。

```
 -X<option> 
```

```
 --strategy-option=<option> 
```

将合并策略特定选项传递给合并策略。有关详细信息，请参阅 [git-merge [1]](https://git-scm.com/docs/git-merge) 。

## SEQUENCER SUBCOMMANDS

```
 --continue 
```

使用 _.git / sequencer_ 中的信息继续进行中的操作。可以在解决失败的挑选或恢复中的冲突后继续使用。

```
 --quit 
```

忘记当前正在进行的操作。在樱桃挑选或恢复失败后，可用于清除顺序器状态。

```
 --abort 
```

取消操作并返回到预序列状态。

## 例子

```
 git revert HEAD~3 
```

还原 HEAD 中第四个最后一次提交所指定的更改，并使用还原的更改创建一个新提交。

```
 git revert -n master~5..master~2 
```

将提交所做的更改从 master（包含）中的第五个最后一次提交恢复到 master（包含）中的第三个最后一次提交，但不要使用还原的更改创建任何提交。恢复仅修改工作树和索引。

## 也可以看看

[git-cherry-pick [1]](https://git-scm.com/docs/git-cherry-pick)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件