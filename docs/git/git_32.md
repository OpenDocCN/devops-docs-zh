# git-cherry-pick

> 原文： [`git-scm.com/docs/git-cherry-pick`](https://git-scm.com/docs/git-cherry-pick)

## 名称

git-cherry-pick - 应用某些现有提交引入的更改

## 概要

```
git cherry-pick [--edit] [-n] [-m parent-number] [-s] [-x] [--ff]
		  [-S[<keyid>]] <commit>…​
git cherry-pick --continue
git cherry-pick --quit
git cherry-pick --abort
```

## 描述

给定一个或多个现有提交，应用每个引入的更改，为每个提交记录一个新提交。这需要您的工作树是干净的（没有 HEAD 提交的修改）。

如果不明显如何应用更改，则会发生以下情况：

1.  当前分支和`HEAD`指针保持在最后一次成功提交。

2.  `CHERRY_PICK_HEAD` ref 设置为指向引入难以应用的更改的提交。

3.  干净地应用更改的路径在索引文件和工作树中都会更新。

4.  对于冲突路径，索引文件最多可记录三个版本，如 [git-merge [1]](https://git-scm.com/docs/git-merge) 的“TRUE MERGE”部分所述。工作树文件将包含由通常的冲突标记`&lt;&lt;&lt;&lt;&lt;&lt;&lt;`和`&gt;&gt;&gt;&gt;&gt;&gt;&gt;`括起来的冲突的描述。

5.  没有进行其他修改。

有关解决此类冲突的一些提示，请参阅 [git-merge [1]](https://git-scm.com/docs/git-merge) 。

## OPTIONS

```
 <commit>…​ 
```

承诺挑选樱桃。有关拼写提交方法的更完整列表，请参阅 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 。可以传递提交集，但默认情况下不执行遍历，就像指定了`--no-walk`选项一样，请参阅 [git-rev-list [1]](https://git-scm.com/docs/git-rev-list) 。请注意，指定范围会将所有&lt; commit&gt; ...参数提供给单个修订版步行（请参阅后面使用 _maint master..next_ 的示例）。

```
 -e 
```

```
 --edit 
```

使用此选项， _git cherry-pick_ 将允许您在提交之前编辑提交消息。

```
 -x 
```

记录提交时，在原始提交消息中附加一行“（从提交中挑选出来的樱桃）”，以指示从哪个提交中挑选出这个更改。这只适用于没有冲突的樱桃选择。如果您从私人分支机构挑选，则不要使用此选项，因为该信息对收件人无用。另一方面，如果您在两个公开可见的分支之间进行挑选（例如，从开发分支向旧版本的维护分支向后端移植修复），添加此信息可能很有用。

```
 -r 
```

过去，命令默认执行上述`-x`，`-r`禁用它。现在默认情况下不执行`-x`所以此选项是无操作。

```
 -m parent-number 
```

```
 --mainline parent-number 
```

通常你不能挑选合并因为你不知道合并的哪一边应该被认为是主线。此选项指定主线的父编号（从 1 开始），并允许 cherry-pick 重放相对于指定父级的更改。

```
 -n 
```

```
 --no-commit 
```

通常，该命令会自动创建一系列提交。此标志应用必要的更改来挑选您的工作树和索引的每个命名提交，而不进行任何提交。此外，使用此选项时，索引不必与 HEAD 提交匹配。樱桃选择是针对索引的开始状态完成的。

当在一行中挑选多个提交效果时，这非常有用。

```
 -s 
```

```
 --signoff 
```

在提交消息的末尾添加 Sign-by-by 行。有关详细信息，请参阅 [git-commit [1]](https://git-scm.com/docs/git-commit) 中的签收选项。

```
 -S[<keyid>] 
```

```
 --gpg-sign[=<keyid>] 
```

GPG 签名提交。 `keyid`参数是可选的，默认为提交者标识;如果指定，它必须粘在没有空格的选项上。

```
 --ff 
```

如果当前 HEAD 与 cherry-pick'ed 提交的父级相同，则将执行此提交的快进。

```
 --allow-empty 
```

在默认情况下，挑选空提交将失败，表明需要显式调用`git commit --allow-empty`。此选项会覆盖该行为，允许在提取中自动保留空提交。请注意，当“ - ff”生效时，即使没有此选项，也会保留满足“快进”要求的空提交。另请注意，使用此选项仅保留最初为空的提交（即提交记录与其父项相同的树）。由于先前提交而变为空的提交被删除。要强制包含这些提交，请使用`--keep-redundant-commits`。

```
 --allow-empty-message 
```

默认情况下，使用空消息挑选提交将失败。此选项会覆盖该行为，允许提取空消息的提交。

```
 --keep-redundant-commits 
```

如果提取的提交重复了当前历史记录中的提交，则它将变为空。默认情况下，这些冗余提交会导致`cherry-pick`停止，以便用户可以检查提交。此选项会覆盖该行为并创建一个空提交对象。意味着`--allow-empty`。

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
 git cherry-pick master 
```

应用 master 分支顶端提交引入的更改，并使用此更改创建新提交。

```
 git cherry-pick ..master 
```

```
 git cherry-pick ^HEAD master 
```

应用所有提交引入的更改，这些提交是 master 的祖先但不是 HEAD 的祖先，以生成新的提交。

```
 git cherry-pick maint next ^master 
```

```
 git cherry-pick maint master..next 
```

应用作为 maint 或 next 的祖先的所有提交所引入的更改，但不应包含 master 或其任何祖先。注意，后者并不意味着`maint`和`master`和`next`之间的所有内容;具体而言，如果`master`中包含`maint`，则不会使用`maint`。

```
 git cherry-pick master~4 master~2 
```

应用 master 指向的第五个和第三个最后提交所引入的更改，并使用这些更改创建 2 个新提交。

```
 git cherry-pick -n master~1 next 
```

将工作树和索引应用于 master 指向的第二个最后一次提交所引入的更改以及 next 指向的最后一个提交，但不要使用这些更改创建任何提交。

```
 git cherry-pick --ff ..next 
```

如果历史是线性的并且 HEAD 是 next 的祖先，则更新工作树并使 HEAD 指针前进以匹配 next。否则，将下一个但不是 HEAD 的提交引入的更改应用于当前分支，为每个新更改创建一个新提交。

```
 git rev-list --reverse master -- README | git cherry-pick -n --stdin 
```

将触及 README 的主分支上的所有提交引入的更改应用到工作树和索引，因此可以检查结果并将其作为单个新提交（如果合适）。

以下序列尝试向后移植补丁，因为补丁适用的代码已经发生了太大的变化，然后再次尝试，这次会更加关注匹配上下文行。

```
$ git cherry-pick topic^             (1)
$ git diff                           (2)
$ git reset --merge ORIG_HEAD        (3)
$ git cherry-pick -Xpatience topic^  (4)
```

1.  应用`git show topic^`显示的更改。在此示例中，修补程序不能完全应用，因此有关冲突的信息将写入索引和工作树，而不会产生新的提交结果。

2.  总结要调和的变化

3.  取消樱桃挑选。换句话说，返回 pre-cherry-pick 状态，保留您在工作树中的任何本地修改。

4.  尝试再次应用`topic^`引入的更改，花费额外的时间来避免基于错误匹配的上下文行的错误。

## 也可以看看

[git-revert [1]](https://git-scm.com/docs/git-revert)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件