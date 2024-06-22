# git-merge

> 原文： [`git-scm.com/docs/git-merge`](https://git-scm.com/docs/git-merge)

## 名称

git-merge - 将两个或多个开发历史记录连接在一起

## 概要

```
git merge [-n] [--stat] [--no-commit] [--squash] [--[no-]edit]
	[-s <strategy>] [-X <strategy-option>] [-S[<keyid>]]
	[--[no-]allow-unrelated-histories]
	[--[no-]rerere-autoupdate] [-m <msg>] [-F <file>] [<commit>…​]
git merge --abort
git merge --continue
```

## 描述

将来自命名提交的更改（自其历史记录与当前分支分开时）合并到当前分支中。 _git pull_ 使用此命令来合并来自另一个存储库的更改，并且可以手动使用此命令将更改从一个分支合并到另一个分支。

假设存在以下历史记录，当前分支为“`master`”：

```
	  A---B---C topic
	 /
    D---E---F---G master
```

然后“`git merge topic`”将重放`topic`分支上的更改，因为它从`master`（即`E`）转移到`master`之上的当前提交（`C`），并记录导致新提交以及两个父提交的名称和来自用户的描述更改的日志消息。

```
	  A---B---C topic
	 /         \
    D---E---F---G---H master
```

第二种语法（“`git merge --abort`”）只能在合并导致冲突后运行。 _git merge --abort_ 将中止合并过程并尝试重建合并前状态。但是，如果在合并开始时有未提交的更改（特别是如果在合并开始后进一步修改了这些更改）， _git merge --abort_ 在某些情况下将无法重建原始（之前） - 改变。因此：

**警告**：不鼓励运行 _git merge_ 并进行非平凡的未提交更改：尽管可能，但如果发生冲突，可能会使您处于难以退出的状态。

第三种语法（“`git merge --continue`”）只能在合并导致冲突后运行。

## OPTIONS

```
 --commit 
```

```
 --no-commit 
```

执行合并并提交结果。此选项可用于覆盖--no-commit。

使用--no-commit 执行合并但假装合并失败并且不自动提交，以便让用户有机会在提交之前检查并进一步调整合并结果。

```
 --edit 
```

```
 -e 
```

```
 --no-edit 
```

在提交成功的机械合并之前调用编辑器以进一步编辑自动生成的合并消息，以便用户可以解释并证明合并。 `--no-edit`选项可用于接受自动生成的消息（通常不鼓励这样做）。如果从命令行给出带有`-m`选项的草稿消息并想在编辑器中编辑它，`--edit`（或`-e`）选项仍然有用。

较旧的脚本可能取决于不允许用户编辑合并日志消息的历史行为。他们将在运行`git merge`时看到编辑器打开。为了便于将此类脚本调整为更新的行为，可以在环境变量`GIT_MERGE_AUTOEDIT`的开头设置为`no`。

```
 --ff 
```

当合并解析为快进时，仅更新分支指针，而不创建合并提交。这是默认行为。

```
 --no-ff 
```

即使合并解析为快进，也要创建合并提交。这是在 _refs / tags /_ 层次结构中合并未存储在其自然位置的带注释（且可能已签名）的标记时的默认行为。

```
 --ff-only 
```

拒绝以非零状态合并和退出，除非当前`HEAD`已经是最新的，或者合并可以解析为快进。

```
 -S[<keyid>] 
```

```
 --gpg-sign[=<keyid>] 
```

GPG 签署生成的合并提交。 `keyid`参数是可选的，默认为提交者标识;如果指定，它必须粘在没有空格的选项上。

```
 --log[=<n>] 
```

```
 --no-log 
```

除了分支名称之外，还使用最多&lt; n&gt;的单行描述填充日志消息。正在合并的实际提交。另见 [git-fmt-merge-msg [1]](https://git-scm.com/docs/git-fmt-merge-msg) 。

使用--no-log 不会列出正在合并的实际提交中的单行描述。

```
 --signoff 
```

```
 --no-signoff 
```

在提交日志消息的末尾由提交者添加逐行签名。签收的含义取决于项目，但它通常证明提交者有权在同一许可下提交此作品并同意开发者原产地证书（参见 [`developercertificate.org/`](http://developercertificate.org/) ] 欲获得更多信息）。

使用--no-signoff 时不要添加 Sign-off-by 行。

```
 --stat 
```

```
 -n 
```

```
 --no-stat 
```

在合并结束时显示 diffstat。 diffstat 也由配置选项 merge.stat 控制。

使用-n 或--no-stat 时，在合并结束时不显示 diffstat。

```
 --squash 
```

```
 --no-squash 
```

生成工作树和索引状态，就好像发生了真正的合并（合并信息除外），但实际上没有提交，移动`HEAD`或记录`$GIT_DIR/MERGE_HEAD`（导致下一个`git commit`命令到创建合并提交）。这允许您在当前分支之上创建单个提交，其效果与合并另一个分支（或章鱼的情况下更多）相同。

使用--no-squash 执行合并并提交结果。此选项可用于覆盖--squash。

```
 -s <strategy> 
```

```
 --strategy=<strategy> 
```

使用给定的合并策略;可以多次提供，以按照应该尝试的顺序指定它们。如果没有`-s`选项，则使用内置的策略列表（合并单个头时 _git merge-recursive_ ，否则使用 _git merge-octopus_ ）。

```
 -X <option> 
```

```
 --strategy-option=<option> 
```

将合并策略特定选项传递给合并策略。

```
 --verify-signatures 
```

```
 --no-verify-signatures 
```

验证正在合并的侧分支的提示提交是否使用有效密钥签名，即具有有效 uid 的密钥：在默认信任模型中，这意味着签名密钥已由可信密钥签名。如果侧分支的提示提交未使用有效密钥签名，则合并将中止。

```
 --summary 
```

```
 --no-summary 
```

同义词--stat 和--no-stat;这些已被弃用，将来会被删除。

```
 -q 
```

```
 --quiet 
```

安静地操作。意味着 - 没有进步。

```
 -v 
```

```
 --verbose 
```

要冗长。

```
 --progress 
```

```
 --no-progress 
```

明确打开/关闭进度。如果两者都未指定，则在标准错误连接到终端时显示进度。请注意，并非所有合并策略都可以支持进度报告。

```
 --allow-unrelated-histories 
```

默认情况下，`git merge`命令拒绝合并不共享共同祖先的历史记录。在合并独立开始生命的两个项目的历史时，此选项可用于覆盖此安全性。由于这是一种非常罕见的情况，因此默认情况下不会启用任何配置变量来启用它，也不会添加。

```
 -m <msg> 
```

设置要用于合并提交的提交消息（如果创建了一个）。

如果指定了`--log`，则正在合并的提交的短消息将附加到指定的消息。

_git fmt-merge-msg_ 命令可用于为自动 _git merge_ 调用提供良好的默认值。自动消息可以包括分支描述。

```
 -F <file> 
```

```
 --file=<file> 
```

读取要用于合并提交的提交消息（如果创建了一个）。

如果指定了`--log`，则正在合并的提交的短消息将附加到指定的消息。

```
 --[no-]rerere-autoupdate 
```

如果可能，允许 rerere 机制使用自动冲突解决的结果更新索引。

```
 --abort 
```

中止当前的冲突解决过程，并尝试重建合并前的状态。

如果在合并开始时存在未提交的工作树更改，则 _git merge --abort_ 在某些情况下将无法重建这些更改。因此，建议在运行 _git merge_ 之前始终提交或存储您的更改。

当`MERGE_HEAD`存在时， _git merge --abort_ 相当于 _git reset --merge_ 。

```
 --continue 
```

在 _git merge_ 因冲突而停止后，您可以通过运行 _git merge --continue_ 来结束合并（请参阅下面的“如何解决冲突”部分）。

```
 <commit>…​ 
```

提交，通常是其他分支机构，合并到我们的分支机构。指定多个提交将创建一个包含两个以上父项的合并（被亲切地称为八达通合并）。

如果未从命令行提供任何提交，请合并当前分支配置为用作其上游的远程跟踪分支。另请参见本手册页的配置部分。

当指定`FETCH_HEAD`（并且没有其他提交）时，通过先前调用`git fetch`进行合并而在`.git/FETCH_HEAD`文件中记录的分支被合并到当前分支。

## 预先检查

在应用外部变更之前，您应该使自己的工作处于良好状态并在本地进行，因此如果存在冲突，则不会被破坏。另见 [git-stash [1]](https://git-scm.com/docs/git-stash) 。 _git pull_ 和 _git merge_ 将停止而不做任何事情当本地未提交的更改与 _git pull_ / _git merge_ 可能需要的文件重叠时更新。

为了避免在合并提交中记录不相关的更改，如果索引中相对于`HEAD`提交注册了任何更改， _git pull_ 和 _git merge_ 也将中止。 （根据正在使用的合并策略，可能存在此规则的特殊狭义异常，但通常，索引必须与 HEAD 匹配。）

如果所有已命名的提交都已经是`HEAD`的祖先，则 _git merge_ 将提前退出并显示“已经是最新的”消息。

## 快速前进的合并

通常，当前分支头是命名提交的祖先。这是最常见的情况，尤其是从 _git pull_ 调用时：您正在跟踪上游存储库，您没有提交本地更改，现在您想要更新到更新的上游修订版。在这种情况下，不需要新的提交来存储组合的历史记录;相反，`HEAD`（以及索引）更新为指向命名提交，而不创建额外的合并提交。

使用`--no-ff`选项可以抑制此行为。

## 真正的合并

除了快速合并（见上文）之外，要合并的分支必须通过合并提交绑定在一起，合并提交将它们都作为父项。

将提交一个合并版本，以协调要合并的所有分支的更改，并将`HEAD`，索引和工作树更新到它。只要它们不重叠，就可以在工作树中进行修改;更新将保留它们。

如果不明显如何协调更改，则会发生以下情况：

1.  `HEAD`指针保持不变。

2.  `MERGE_HEAD` ref 设置为指向另一个分支头。

3.  干净地合并的路径在索引文件和工作树中都会更新。

4.  对于冲突路径，索引文件最多可记录三个版本：阶段 1 存储来自共同祖先的版本，阶段 2 来自`HEAD`，阶段 3 来自`MERGE_HEAD`（您可以使用`git ls-files -u`检查阶段）。工作树文件包含“合并”程序的结果;即 3 路合并结果与熟悉的冲突标记`&lt;&lt;&lt;` `===` `&gt;&gt;&gt;`。

5.  没有进行其他更改。特别是，在开始合并之前进行的本地修改将保持不变，并且它们的索引条目保持不变，即匹配`HEAD`。

如果您尝试合并导致复杂冲突并想重新开始，则可以使用`git merge --abort`进行恢复。

## 合并标签

合并带注释（可能已签名）的标记时，即使可以进行快进合并，Git 也会始终创建合并提交，并且使用标记消息准备提交消息模板。此外，如果标记已签名，则签名检查将在消息模板中报告为注释。另见 [git-tag [1]](https://git-scm.com/docs/git-tag) 。

当您想要与导致恰好被标记的提交的工作集成时，例如，与上游发布点同步，您可能不希望进行不必要的合并提交。

在这种情况下，您可以在将标签送入`git merge`之前自行“打开”标签，或者在您自己没有任何工作时通过`--ff-only`。例如

```
git fetch origin
git merge v1.2.3⁰
git merge --ff-only v1.2.3
```

## 如何提出冲突

在合并期间，将更新工作树文件以反映合并的结果。在对共同祖先版本所做的更改中，非重叠版本（即，您更改文件的某个区域而另一侧保留该区域完整，或反之亦然）将逐字汇入最终结果中。然而，当双方对同一区域进行更改时，Git 不能随意选择一侧而是另一侧，并要求您通过将双方所做的事情留在该区域来解决它。

默认情况下，Git 使用与 RCS 套件中“merge”程序使用的样式相同的样式来呈现这样一个冲突的 hunk，如下所示：

```
Here are lines that are either unchanged from the common
ancestor, or cleanly resolved because only one side changed.
<<<<<<< yours:sample.txt
Conflict resolution is hard;
let's go shopping.
=======
Git makes conflict resolution easy.
>>>>>>> theirs:sample.txt
And here is another line that is cleanly resolved or unmodified.
```

发生一对冲突变化的区域标有标记`&lt;&lt;&lt;&lt;&lt;&lt;&lt;`，`=======`和`&gt;&gt;&gt;&gt;&gt;&gt;&gt;`。 `=======`之前的部分通常是你的一面，之后的部分通常是它们的一面。

默认格式不显示原始在冲突区域中所说的内容。你无法分辨出删除了多少行，取而代之的是 Barbie 的评论。你能说的唯一一件事就是你的一方想说它很难而且你更喜欢去购物，而另一方则想要说这很容易。

通过将“merge.conflictStyle”配置变量设置为“diff3”，可以使用替代样式。在“diff3”样式中，上述冲突可能如下所示：

```
Here are lines that are either unchanged from the common
ancestor, or cleanly resolved because only one side changed.
<<<<<<< yours:sample.txt
Conflict resolution is hard;
let's go shopping.
|||||||
Conflict resolution is hard.
=======
Git makes conflict resolution easy.
>>>>>>> theirs:sample.txt
And here is another line that is cleanly resolved or unmodified.
```

除`&lt;&lt;&lt;&lt;&lt;&lt;&lt;`，`=======`和`&gt;&gt;&gt;&gt;&gt;&gt;&gt;`标记外，它还使用另一个`|||||||`标记，后跟原始文本。你可以说原来只是陈述了一个事实，而你的一方只是屈服于那个陈述并放弃了，而另一方则试图采取更积极的态度。您有时可以通过查看原始分辨率来获得更好的分辨率。

## 如何解决冲突

看到冲突后，你可以做两件事：

*   决定不合并。您需要的唯一清理是将索引文件重置为`HEAD`提交以反转 2.并清除由 2.和 3 进行的工作树更改。 `git merge --abort`可用于此目的。

*   解决冲突。 Git 将标记工作树中的冲突。将文件编辑成形状， _git 将 _ 添加到索引中。使用 _git commit_ 或 _git merge - 继续 _ 来达成交易。后一个命令在调用 _git commit_ 之前检查是否存在正在进行的（中断）合并。

您可以使用许多工具解决冲突：

*   使用 mergetool。 `git mergetool`启动图形合并工具，它将帮助您完成合并。

*   看看差异。 `git diff`将显示三向差异，突出显示`HEAD`和`MERGE_HEAD`版本的变化。

*   查看每个分支的差异。 `git log --merge -p &lt;path&gt;`将首先显示`HEAD`版本的差异，然后显示`MERGE_HEAD`版本。

*   看看原件。 `git show :1:filename`显示共同的祖先，`git show :2:filename`显示`HEAD`版本，`git show :3:filename`显示`MERGE_HEAD`版本。

## 例子

*   在当前分支的顶部合并分支`fixes`和`enhancements`，进行章鱼合并：

    ```
    $ git merge fixes enhancements
    ```

*   使用`ours`合并策略将分支`obsolete`合并到当前分支中：

    ```
    $ git merge -s ours obsolete
    ```

*   将分支`maint`合并到当前分支中，但不要自动进行新的提交：

    ```
    $ git merge --no-commit maint
    ```

    当您想要包含对合并的进一步更改，或者想要编写自己的合并提交消息时，可以使用此方法。

    您应该避免滥用此选项以将重大更改隐藏到合并提交中。像碰撞版本/版本名称这样的小修正是可以接受的。

## 合并战略

合并机制（`git merge`和`git pull`命令）允许使用`-s`选项选择后端 _ 合并策略 _。一些策略也可以采用自己的选项，可以通过向`git merge`和/或`git pull`提供`-X&lt;option&gt;`参数来传递。

```
 resolve 
```

这只能使用 3 向合并算法解析两个头（即当前分支和您从中拉出的另一个分支）。它试图仔细检测纵横交错的合并模糊，并且通常被认为是安全和快速的。

```
 recursive 
```

这只能使用 3 向合并算法解析两个磁头。当有多个可用于 3 向合并的共同祖先时，它会创建共同祖先的合并树，并将其用作 3 向合并的参考树。据报道，这会导致更少的合并冲突，而不会因为从 Linux 2.6 内核开发历史记录中进行的实际合并提交所做的测试而导致错误。此外，这可以检测和处理涉及重命名的合并，但目前无法使用检测到的副本。这是拉动或合并一个分支时的默认合并策略。

_ 递归 _ 策略可以采用以下选项：

```
 ours 
```

这个选项通过支持 _ 我们的 _ 版本来强制冲突的帅哥干净地自动解决。来自与我们方不冲突的其他树的更改将反映到合并结果中。对于二进制文件，整个内容都来自我们这边。

这不应该与 _ 我们的 _ 合并策略混淆，后者甚至不会查看其他树包含的内容。它丢弃了另一棵树所做的一切，声明 _ 我们的 _ 历史记录中包含了所有发生的事情。

```
 theirs 
```

这与 _ 我们的 _ 相反;请注意，与 _ 我们的 _ 不同，没有 _ 他们的 _ 合并策略来混淆这个合并选项。

```
 patience 
```

使用此选项， _merge-recursive_ 花费一点额外的时间来避免由于不重要的匹配行（例如，来自不同函数的大括号）而有时发生的错误。当要合并的分支发生疯狂分歧时使用此选项。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--patience`。

```
 diff-algorithm=[patience|minimal|histogram|myers] 
```

告诉 _merge-recursive_ 使用不同的 diff 算法，这有助于避免由于不重要的匹配行（例如来自不同函数的大括号）而发生的错误。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--diff-algorithm`。

```
 ignore-space-change 
```

```
 ignore-all-space 
```

```
 ignore-space-at-eol 
```

```
 ignore-cr-at-eol 
```

为了进行三向合并，将具有指示类型的空白的行更改为未更改。与空行的其他更改混合的空白更改不会被忽略。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `-b`，`-w`，`--ignore-space-at-eol`和`--ignore-cr-at-eol`。

*   如果 _ 他们的 _ 版本只将空格更改引入一行，_ 我们的 _ 版本被使用;

*   如果 _ 我们的 _ 版本引入了空格更改，但 _ 他们的 _ 版本包含了实质性更改，_ 使用了他们的 _ 版本;

*   否则，合并以通常的方式进行。

```
 renormalize 
```

在解析三向合并时，这将运行虚拟签出并检入文件的所有三个阶段。此选项适用于将分支与不同的清除过滤器或行尾规范化规则合并时使用。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 中的“合并具有不同签入/签出属性的分支”。

```
 no-renormalize 
```

禁用`renormalize`选项。这会覆盖`merge.renormalize`配置变量。

```
 no-renames 
```

关闭重命名检测。这会覆盖`merge.renames`配置变量。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--no-renames`。

```
 find-renames[=<n>] 
```

打开重命名检测，可选择设置相似性阈值。这是默认值。这会覆盖 _merge.renames_ 配置变量。另见 [git-diff [1]](https://git-scm.com/docs/git-diff) `--find-renames`。

```
 rename-threshold=<n> 
```

已弃用`find-renames=&lt;n&gt;`的同义词。

```
 subtree[=<path>] 
```

此选项是 _ 子树 _ 策略的更高级形式，其中策略猜测两个树在合并时必须如何移位以相互匹配。相反，指定的路径是前缀（或从头开始剥离），以使两个树的形状匹配。

```
 octopus 
```

这解决了具有两个以上磁头的情况，但拒绝执行需要手动解决的复杂合并。它主要用于将主题分支头捆绑在一起。这是拉动或合并多个分支时的默认合并策略。

```
 ours 
```

这会解析任意数量的头，但合并的结果树始终是当前分支头的树，实际上忽略了所有其他分支的所有更改。它旨在用于取代侧枝的旧发展历史。请注意，这与 _ 递归 _ 合并策略的-Xours 选项不同。

```
 subtree 
```

这是一种修改后的递归策略。当合并树 A 和 B 时，如果 B 对应于 A 的子树，则首先调整 B 以匹配 A 的树结构，而不是读取相同级别的树。这种调整也是对共同的祖先树进行的。

使用三向合并的策略（包括默认的 _ 递归 _），如果在两个分支上进行了更改，但稍后在其中一个分支上进行了更改，则该更改将出现在合并结果中;有些人发现这种行为令人困惑。之所以会发生这种情况，是因为在执行合并时只考虑头和合并基础，而不是单个提交。因此，合并算法将恢复的更改视为完全没有更改，而是替换更改的版本。

## 组态

```
 merge.conflictStyle 
```

指定在合并时将冲突的帅哥写入工作树文件的样式。默认为“合并”，显示`&lt;&lt;&lt;&lt;&lt;&lt;&lt;`冲突标记，一侧更改，`=======`标记，另一侧更改，然后是`&gt;&gt;&gt;&gt;&gt;&gt;&gt;`标记。另一种样式“diff3”在`=======`标记之前添加`|||||||`标记和原始文本。

```
 merge.defaultToUpstream 
```

如果在没有任何提交参数的情况下调用 merge，则通过使用存储在其远程跟踪分支中的最后观察值来合并为当前分支配置的上游分支。查询`branch.&lt;current branch&gt;.remote`命名的远程分支的`branch.&lt;current branch&gt;.merge`值，然后通过`remote.&lt;remote&gt;.fetch`将它们映射到相应的远程跟踪分支，并合并这些跟踪分支的提示。

```
 merge.ff 
```

默认情况下，Git 在合并作为当前提交的后代的提交时不会创建额外的合并提交。相反，当前分支的提示是快进的。当设置为`false`时，此变量告诉 Git 在这种情况下创建额外的合并提交（相当于从命令行提供`--no-ff`选项）。设置为`only`时，仅允许此类快进合并（相当于从命令行提供`--ff-only`选项）。

```
 merge.verifySignatures 
```

如果为 true，则等效于--verify-signatures 命令行选项。有关详细信息，请参阅 [git-merge [1]](https://git-scm.com/docs/git-merge) 。

```
 merge.branchdesc 
```

除分支名称外，还使用与其关联的分支描述文本填充日志消息。默认为 false。

```
 merge.log 
```

除了分支名称之外，还要从正在合并的实际提交中填充最多具有指定数量的单行描述的日志消息。默认为 false，true 是 20 的同义词。

```
 merge.renameLimit 
```

合并期间执行重命名检测时要考虑的文件数;如果未指定，则默认为 diff.renameLimit 的值。如果关闭重命名检测，此设置无效。

```
 merge.renames 
```

Git 是否以及如何检测重命名。如果设置为“false”，则禁用重命名检测。如果设置为“true”，则启用基本重命名检测。默认为 diff.renames 的值。

```
 merge.renormalize 
```

告诉 Git，存储库中文件的规范表示随着时间的推移而发生了变化（例如，早期的提交记录了带有 CRLF 行结尾的文本文件，但最近提交了使用 LF 行结尾的文本文件）。在这样的存储库中，Git 可以在执行合并之前将提交中记录的数据转换为规范形式，以减少不必要的冲突。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 中的“合并具有不同签入/签出属性的分支”部分。

```
 merge.stat 
```

是否在合并结束时打印 ORIG_HEAD 和合并结果之间的 diffstat。默认为 True。

```
 merge.tool 
```

控制 [git-mergetool [1]](https://git-scm.com/docs/git-mergetool) 使用的合并工具。下面的列表显示了有效的内置值。任何其他值都被视为自定义合并工具，并且需要定义相应的 mergetool。&lt; tool&gt; .cmd 变量。

```
 merge.guitool 
```

当指定-g / - gui 标志时，控制 [git-mergetool [1]](https://git-scm.com/docs/git-mergetool) 使用哪个合并工具。下面的列表显示了有效的内置值。任何其他值都被视为自定义合并工具，并且需要定义相应的 mergetool。&lt; guitool&gt; .cmd 变量。

*   araxis

*   公元前

*   BC3

*   codecompare

*   deltawalker

*   diffmerge

*   扩散

*   ecmerge

*   出现

*   examdiff

*   guiffy

*   gvimdiff

*   gvimdiff2

*   gvimdiff3

*   kdiff3

*   合并

*   了 opendiff

*   p4merge

*   tkdiff

*   如果 TortoiseMerge

*   vimdiff 同时

*   vimdiff2

*   vimdiff3

*   的 WinMerge

*   xxdiff

```
 merge.verbosity 
```

控制递归合并策略显示的输出量。如果检测到冲突，则 0 级除了最终错误消息外不输出任何内容。级别 1 仅输出冲突，输出 2 个冲突和文件更改。 5 级及以上输出调试信息。默认值为 2 级。可以被`GIT_MERGE_VERBOSITY`环境变量覆盖。

```
 merge.<driver>.name 
```

为自定义低级合并驱动程序定义一个人类可读的名称。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 merge.<driver>.driver 
```

定义实现自定义低级合并驱动程序的命令。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 merge.<driver>.recursive 
```

命名在共同祖先之间执行内部合并时使用的低级合并驱动程序。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 branch.<name>.mergeOptions 
```

设置合并到分支&lt; name&gt;的默认选项。语法和支持的选项与 _git merge_ 的相同，但目前不支持包含空格字符的选项值。

## 也可以看看

[git-fmt-merge-msg [1]](https://git-scm.com/docs/git-fmt-merge-msg) ， [git-pull [1]](https://git-scm.com/docs/git-pull) ， [gitattributes [5]](https://git-scm.com/docs/gitattributes) ， [git-reset [1]](https://git-scm.com/docs/git-reset) ， [git-diff [1]](https://git-scm.com/docs/git-diff) ， [git-ls-files [1]](https://git-scm.com/docs/git-ls-files) ， [git-add [1]](https://git-scm.com/docs/git-add) ， [git- rm [1]](https://git-scm.com/docs/git-rm) ， [git-mergetool [1]](https://git-scm.com/docs/git-mergetool)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件