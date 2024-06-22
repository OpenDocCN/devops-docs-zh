# git-merge-base

> 原文： [`git-scm.com/docs/git-merge-base`](https://git-scm.com/docs/git-merge-base)

## 名称

git-merge-base - 为合并找到尽可能好的共同祖先

## 概要

```
git merge-base [-a|--all] <commit> <commit>…​
git merge-base [-a|--all] --octopus <commit>…​
git merge-base --is-ancestor <commit> <commit>
git merge-base --independent <commit>…​
git merge-base --fork-point <ref> [<commit>]
```

## 描述

_git merge-base_ 找到两个提交之间最好的共同祖先，用于三向合并。如果后者是前者的祖先，一个共同的祖先 _ 比另一个共同的祖先更好 _。没有任何更好的共同祖先的共同祖先是 _ 最佳共同祖先 _，即 _ 合并基 _。请注意，一对提交可以有多个合并基础。

## 操作模式

作为最常见的特殊情况，在命令行上仅指定两个提交意味着计算给定两个提交之间的合并基础。

更一般地，在计算合并基数的两个提交中，一个由命令行上的第一个提交参数指定;另一个提交是（可能是假设的）提交，它是命令行上所有剩余提交的合并。

因此，如果指定了两个以上的提交，则 _ 合并库 _ 不一定包含在每个提交参数中。当与`--merge-base`选项一起使用时，这与 [git-show-branch [1]](https://git-scm.com/docs/git-show-branch) 不同。

```
 --octopus 
```

计算所有提供的提交的最佳共同祖先，为 n 路合并做准备。这模仿了 _git show-branch --merge-base_ 的行为。

```
 --independent 
```

不是打印合并库，而是使用相同的祖先打印提供的提交的最小子集。换句话说，在给出的提交中，列出那些不能从任何其他提交的提交。这模仿了 _git show-branch --independent_ 的行为。

```
 --is-ancestor 
```

检查第一个&lt; commit&gt;是第二个&lt; commit&gt;的祖先，如果为 true 则退出，状态为 0，否则退出状态为 1。错误由非零状态发出信号，该状态不为 1。

```
 --fork-point 
```

找到分支（或任何导致&lt; commit&gt;的历史记录）从另一个分支（或任何引用）&lt; ref&gt;分叉的点。这不只是寻找两个提交的共同祖先，而且还考虑了&lt; ref&gt;的 reflog。查看导致&lt; commit&gt;的历史记录分支早期的分支&lt; ref&gt;分叉（见下面关于这种模式的讨论）。

## OPTIONS

```
 -a 
```

```
 --all 
```

输出提交的所有合并基础，而不是仅输出一个。

## 讨论

给定两个提交 _A_ 和 _B_ ，`git merge-base A B`将通过父关系输出可从 _A_ 和 _B_ 到达的提交。

例如，使用此拓扑：

```
         o---o---o---B
        /
---o---1---o---o---o---A
```

_A_ 和 _B_ 之间的合并碱基是 _1_ 。

给定三次提交 _A_ ， _B_ 和 _C_ ，`git merge-base A B C`将计算 _A_ 和假设提交 _ 之间的合并基数 M_ ，是 _B_ 和 _C_ 之间的合并。例如，使用此拓扑：

```
       o---o---o---o---C
      /
     /   o---o---o---B
    /   /
---2---1---o---o---o---A
```

`git merge-base A B C`的结果是 _1_ 。这是因为 _B_ 和 _C_ 之间具有合并提交 _M_ 的等效拓扑是：

```
       o---o---o---o---o
      /                 \
     /   o---o---o---o---M
    /   /
---2---1---o---o---o---A
```

`git merge-base A M`的结果是 _1_ 。提交 _2_ 也是 _A_ 和 _M_ 之间的共同祖先，但 _1_ 是更好的共同祖先，因为 _2_ 是 _1_ 的祖先。因此， _2_ 不是合并基础。

`git merge-base --octopus A B C`的结果是 _2_ ，因为 _2_ 是所有提交的最佳共同祖先。

当历史涉及纵横交错时，两个提交可以有不止一个 _ 最佳 _ 共同祖先。例如，使用此拓扑：

```
---1---o---A
    \ /
     X
    / \
---2---o---o---B
```

_1_ 和 _2_ 都是 A 和 B 的合并碱基。两者都不比另一个好（两者都是 _ 最佳 _ 合并碱基）。如果未给出`--all`选项，则未指定输出哪一个最佳选项。

在两个提交 A 和 B 之间检查“快进”的常用习惯用法是（或者至少用于）计算 A 和 B 之间的合并基础，并检查它是否与 A 相同，在这种情况下，A 是 B 的祖先。你会看到这个习惯用法经常用在较旧的脚本中。

```
A=$(git rev-parse --verify A)
if test "$A" = "$(git merge-base A B)"
then
	... A is an ancestor of B ...
fi
```

在现代 git 中，您可以更直接地说出这一点：

```
if git merge-base --is-ancestor A B
then
	... A is an ancestor of B ...
fi
```

代替。

## 关于叉点模式的讨论

处理使用`git checkout -b topic origin/master`创建的`topic`分支后，远程跟踪分支`origin/master`的历史记录可能已经倒回并重建，从而导致此形状的历史记录：

```
                 o---B2
                /
---o---o---B1--o---o---o---B (origin/master)
        \
         B0
          \
           D0---D1---D (topic)
```

其中`origin/master`用于指向提交 B0，B1，B2，现在它指向 B，当`origin/master`位于 B0 时，您的`topic`分支在它上面启动，并且您构建了三个提交，D0， D1 和 D，在它上面。想象一下，您现在想要在更新的 origin / master 之上重新设置您在该主题上所做的工作。

在这种情况下，`git merge-base origin/master topic`将在上图中返回 B0 的父节点，但是 B0 ^ .. D 是**而不是**你想要在 B 之上重放的提交范围（它包括 B0） ，这不是你写的;它是一个提交，当它从 B0 移动到 B1 时，另一方丢弃了）。

`git merge-base --fork-point origin/master topic`旨在帮助解决此类问题。它不仅需要 B，还需要 B0，B1 和 B2（即存储库的 reflog 知道的远程跟踪分支的旧技巧），以查看构建主题分支的提交并找到 B0，允许您仅重放对你主题的提交，不包括后来丢弃的提交。

于是

```
$ fork_point=$(git merge-base --fork-point origin/master topic)
```

会找到 B0，和

```
$ git rebase --onto origin/master $fork_point topic
```

将重放 B 顶部的 D0，D1 和 D 以创建此形状的新历史记录：

```
		 o---B2
		/
---o---o---B1--o---o---o---B (origin/master)
	\                   \
	 B0                  D0'--D1'--D' (topic - updated)
	  \
	   D0---D1---D (topic - old)
```

需要注意的是，您的存储库中较旧的 reflog 条目可能会被`git gc`过期。如果远程跟踪分支`origin/master`的 reflog 中不再出现 B0，则`--fork-point`模式显然无法找到并失败，从而避免给出随机且无用的结果（例如 B0 的父级，就像同一命令一样没有`--fork-point`选项给出）。

此外，您使用`--fork-point`模式的远程跟踪分支必须是您的主题从其提示分叉的主题。如果你从一个比提示更早的提交分叉，这个模式将找不到叉点（想象在上面的示例历史 B0 不存在，origin / master 从 B1 开始，移到 B2 然后 B，你分叉你的主题 at origin / master ^当 origin / master 为 B1 时;历史的形状与上面相同，没有 B0，B1 的父级是`git merge-base origin/master topic`正确找到的，但`--fork-point`模式不会，因为它不是曾经位于 origin / master 的提交之一。

## 也可以看看

[git-rev-list [1]](https://git-scm.com/docs/git-rev-list) ， [git-show-branch [1]](https://git-scm.com/docs/git-show-branch) ， [git-merge [1]](https://git-scm.com/docs/git-merge)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件