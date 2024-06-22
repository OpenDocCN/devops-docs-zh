# gitworkflows

> 原文： [`git-scm.com/docs/gitworkflows`](https://git-scm.com/docs/gitworkflows)

## 名称

gitworkflows - 使用 Git 推荐的工作流程概述

## 概要

```
git *
```

## 描述

本文档试图记下并激发`git.git`本身使用的一些工作流程元素。一般而言，许多想法都适用，但涉及较少人员的较小项目很少需要完整的工作流程。

我们制定了一套 _ 规则 _ 供快速参考，而散文则试图激励他们每个人。不要总是从字面上理解它们;你应该重视你的行动的好理由，而不是像这样的联机会。

## 单独更改

作为一般规则，您应该尝试将更改拆分为小的逻辑步骤，并提交每个更改。它们应该是一致的，独立于任何后期提交，通过测试套件等。这使得审查过程更加容易，并且历史对于以后的检查和分析更有用，例如 [git-blame [1 ]](https://git-scm.com/docs/git-blame) 和 [git-bisect [1]](https://git-scm.com/docs/git-bisect) 。

要实现这一目标，请尝试从一开始就将工作分成小步骤。压缩一些提交总是比将一个大提交分成几个更容易。不要害怕沿途做太小或不完美的步骤。您可以随后返回并在发布之前使用`git rebase --interactive`编辑提交。您可以使用`git stash push --keep-index`独立于其他未提交的更改运行测试套件;参见 [git-stash [1]](https://git-scm.com/docs/git-stash) 的实例部分。

## 管理分支

有两个主要工具可用于包括从一个分支到另一个分支的更改： [git-merge [1]](https://git-scm.com/docs/git-merge) 和 [git-cherry-pick [1]](https://git-scm.com/docs/git-cherry-pick) 。

合并有许多优点，因此我们尝试仅使用合并来解决尽可能多的问题。樱桃采摘仍然偶尔有用;请参阅下面的“向上合并”以获取示例。

最重要的是，合并工作在分支级别，而樱桃选择在提交级别工作。这意味着合并可以轻松地从 1 次，10 次或 1000 次提交中继承更改，这反过来意味着工作流程可以更好地扩展到大量贡献者（和贡献）。合并也更容易理解，因为合并提交是“承诺”，现在包括来自其所有父项的所有更改。

当然有一个权衡：合并需要更仔细的分支管理。以下小节讨论了重点。

### 毕业

由于给定的特征从实验到稳定，它也在软件的相应分支之间“毕业”。 `git.git`使用以下 _ 集成分支 _：

*   _maint_ 跟踪应该进入下一个“维护版本”的提交，即更新最后发布的稳定版本;

*   _master_ 跟踪应该进入下一个版本的提交;

*   _next_ 旨在作为测试主要稳定性主题的测试分支。

第四个官方分支的使用方式略有不同：

*   _pu_ （建议更新）是一个集成分支，用于尚未准备好包含的内容（请参阅下面的“集成分支”）。

四个分支中的每一个通常是其上方的分支的直接后代。

从概念上讲，一旦被认为足够稳定，该特征进入不稳定的分支（通常是 _ 下一个 _ 或 _pu_ ），并且“毕业”到 _ 主 _ 用于下一个版本。

### 合并向上

然而，上面讨论的“向下分度”不能通过实际向下合并来完成，因为这会将不稳定分支上的 _ 所有 _ 变化合并到稳定分支中。因此如下：

Rule: Merge upwards

始终将修复程序提交到需要它们的最旧的受支持分支。然后（周期性地）将集成分支向上合并到彼此中。

这提供了非常受控制的修复流程。如果您发现自己已将修复程序应用于例如在 _maint_ 中也需要 _master_ ，你需要向下挑选它（使用 [git-cherry-pick [1]](https://git-scm.com/docs/git-cherry-pick) ）。这种情况会发生几次，除非你经常这样做，否则无需担心。

### 主题分支

任何重要的功能都需要实现几个补丁，并且可能在其生命周期内获得额外的错误修正或改进。

直接在集成分支上提交所有内容会导致许多问题：糟糕的提交无法撤消，因此必须逐个还原，这会在您忘记还原一组更改时创建令人困惑的历史记录和进一步的错误可能性。并行工作混合了变化，造成了进一步的混乱。

使用“主题分支”解决了这些问题。这个名字非常自我解释，但有一个警告来自上面的“合并向上”规则：

Rule: Topic branches

为每个主题（功能，错误修复，...）制作一个侧支。在最旧的集成分支中解决它，您最终希望将其合并到其中。

然后很多事情可以很自然地完成：

*   要将功能/错误修复纳入集成分支，只需将其合并即可。如果主题在此期间进一步发展，请再次合并。 （请注意，您不必首先将其合并到最旧的集成分支。例如，您可以先将错误修复合并到 _ 下一个 _，给它一些测试时间，并合并到 _maint_ 当你知道它是稳定的。）

*   如果您发现需要分支 _ 其他 _ 的新功能继续处理您的主题，请将 _ 其他 _ 合并到 _ 主题 _。 （但是，不要“习惯性地”这样做，见下文。）

*   如果您发现分叉错误的分支并希望“及时”移动它，请使用 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 。

请注意，最后一点与其他两个点发生冲突：已在其他位置合并的主题不应重新绑定。请参阅 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 中关于从上游重新恢复的部分。

我们应该指出，“习惯性地”（通常没有任何实际理由）将整合分支合并到您的主题中 - 并且通过扩展，将任何上游的内容合并到下游的任何内容 - 是不赞成的：

Rule: Merge to downstream only at well-defined points

除非有充分的理由，否则不要合并到下游：上游 API 更改会影响您的分支;你的分支机构不再干净地融入上游;等等

否则，合并到的主题突然包含多个（分离良好的）更改。由此导致的许多小合并将极大地混乱历史。后来调查文件历史记录的任何人都必须查明该合并是否会影响开发中的主题。上游甚至可能无意中被合并为“更稳定”的分支。等等。

### 扔掉一体化

如果您遵循最后一段，您现在将拥有许多小主题分支，偶尔会想知道它们是如何交互的。合并它们的结果可能甚至不起作用？但另一方面，我们希望避免将它们合并到任何“稳定”的位置，因为这样的合并不容易被撤消。

当然，解决方案是进行我们可以撤消的合并：合并到一个扔掉的分支。

Rule: Throw-away integration branches

要测试多个主题的交互，请将它们合并到一个扔掉的分支中。你必须永远不要在这样的分支上做任何工作！

如果你（非常）清楚地知道这个分支将在测试后立即被删除，你甚至可以发布这个分支，例如让测试人员有机会使用它，或者其他开发人员有机会看看他们是否正在进行的工作将是兼容的。 `git.git`有一个名为 _pu_ 的官方一次性集成分支。

### 发布的分支管理

假设您正在使用上面讨论的合并方法，当您发布项目时，您将需要执行一些额外的分支管理工作。

功能发布是从 _ 主 _ 分支创建的，因为 _ 主 _ 跟踪应该进入下一个功能发布的提交。

_ 主 _ 分支应该是 _maint_ 的超集。如果此条件不成立，则 _maint_ 包含一些未包含在 _master_ 中的提交。因此，这些提交所代表的修补程序将不会包含在您的功能发行版中。

要验证 _master_ 确实是 _maint_ 的超集，请使用 git log：

Recipe: Verify _master_ is a superset of _maint_

`git log master..maint`

此命令不应列出任何提交。否则，请检查 _master_ 并将 _maint_ 合并到其中。

现在，您可以继续创建功能发布。将标签应用于 _master_ 的尖端，指示发布版本：

Recipe: Release tagging

`git tag -s -m "Git X.Y.Z" vX.Y.Z master`

您需要将新标记推送到公共 Git 服务器（请参阅下面的“DISTRIBUTED WORKFLOWS”）。这使得其他人可以使用该标签来跟踪您的项目。推送还可以触发更新后挂钩以执行与发布相关的项目，例如构建发布 tar 包和预格式化文档页面。

同样，对于维护版本， _maint_ 正在跟踪要释放的提交。因此，在上述步骤中，只需标记并按 _maint_ 而不是 _master_ 。

### 功能发布后的维护分支管理

功能发布后，您需要管理维护分支。

首先，如果您希望继续发布在最近版本之前发布的功能版本的维护修补程序，那么您必须创建另一个分支来跟踪该先前版本的提交。

为此，将当前维护分支复制到以先前版本号命名的另一个分支（例如，maint-X.Y。（Z-1），其中 X.Y.Z 是当前版本）。

Recipe: Copy maint

`git branch maint-X.Y.(Z-1) maint`

现在应该将 _maint_ 分支快速转发到新发布的代码，以便可以跟踪当前版本的维护修复：

Recipe: Update maint to new release

*   `git checkout maint`

*   `git merge --ff-only master`

如果合并失败，因为它不是快进，那么可能在功能发布中错过了 _maint_ 的一些修复。如果按照上一节中的描述验证了分支的内容，则不会发生这种情况。

### 功能发布后的分支管理 for next 和 pu

功能发布后，集成分支 _next_ 可以选择使用 _next_ 上的幸存主题从 _master_ 的尖端重绕并重建：

Recipe: Rewind and rebuild next

*   `git checkout next`

*   `git reset --hard master`

*   `git merge ai/topic_in_next1`

*   `git merge ai/topic_in_next2`

*   ...

这样做的好处是 _next_ 的历史将是干净的。例如，合并到 _ 中的一些主题下一个 _ 可能最初看起来很有希望，但后来被发现是不合需要的或者是不成熟的。在这种情况下，主题将从 _ 下一个 _ 中恢复出来，但事实上它仍然存在于曾经合并和还原的历史中。通过重新创建 _ 下一个 _，你可以为这些主题提供另一个版本，以便重试，并且功能发布是历史上的一个好点。

如果你这样做，那么你应该做一个公告，表明[​​HTG0]下一个被重绕并重建。

对于 _pu_ ，可以遵循相同的倒带和重建过程。如上所述，由于 _pu_ 是丢弃分支，因此不需要公告。

## 分布式工作流程

在最后一节之后，您应该知道如何管理主题。一般而言，您不会是唯一从事该项目的人，因此您必须分享您的工作。

粗略地说，有两个重要的工作流程：合并和补丁。重要的区别在于合并工作流可以传播完整的历史记录，包括合并，而补丁则不能。两个工作流程都可以并行使用：在`git.git`中，只有子系统维护人员使用合并工作流程，而其他人都发送补丁。

请注意，维护者可能会施加限制，例如“签名”要求，所有提交包含的提交/补丁必须遵守。有关更多信息，请参阅项目文档。

### 合并工作流程

合并工作流程通过在上游和下游之间复制分支来工作。上游可以将贡献合并到官方历史中;下游基地的工作在官方历史上。

有三种主要工具可用于此：

*   [git-push [1]](https://git-scm.com/docs/git-push) 将您的分支复制到远程存储库，通常是一个可供所有相关方读取的存储库;

*   [git-fetch [1]](https://git-scm.com/docs/git-fetch) 将远程分支复制到您的存储库;和

*   [git-pull [1]](https://git-scm.com/docs/git-pull) 一次性获取并合并。

注意最后一点。除非你真的想要合并远程分支，否则 _ 不能 _ 使用 _git pull_ 。

更改很容易：

Recipe: Push/pull: Publishing branches/topics

`git push &lt;remote&gt; &lt;branch&gt;`并告诉每个人他们可以从哪里取。

你仍然需要通过其他方式告诉别人，比如邮件。 （Git 提供 [git-request-pull [1]](https://git-scm.com/docs/git-request-pull) 向上游维护者发送预先格式化的拉取请求，以简化此任务。）

如果您只想获得集成分支的最新副本，那么保持最新也很容易：

Recipe: Push/pull: Staying up to date

使用`git fetch &lt;remote&gt;`或`git remote update`保持最新。

然后简单地从稳定的遥控器中分叉您的主题分支，如前所述。

如果您是维护者并希望将其他人的主题分支合并到集成分支，他们通常会通过邮件发送请求。这样的请求看起来像

```
Please pull from
    <url> <branch>
```

在这种情况下， _git pull_ 可以一次性进行获取和合并，如下所示。

Recipe: Push/pull: Merging remote topics

`git pull &lt;url&gt; &lt;branch&gt;`

有时，维护者在尝试从下游提取更改时可能会发生合并冲突。在这种情况下，他们可以要求下游进行合并并自己解决冲突（也许他们会更好地了解如何解决它们）。这是下游 _ 应该 _ 从上游合并的罕见情况之一。

### 补丁工作流程

如果您是以电子邮件形式向上游发送更改的贡献者，您应该像往常一样使用主题分支（参见上文）。然后使用 [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 生成相应的电子邮件（强烈建议手动格式化它们，因为它使维护者的生活更轻松）。

Recipe: format-patch/am: Publishing branches/topics

*   `git format-patch -M upstream..topic`将它们转换为预先格式化的补丁文件

*   `git send-email --to=&lt;recipient&gt; &lt;patches&gt;`

有关更多使用说明，请参阅 [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 和 [git-send-email [1]](https://git-scm.com/docs/git-send-email) 联机帮助页。

如果维护者告诉您补丁不再适用于当前的上游，则必须重新定义主题（您不能使用合并，因为您无法格式化补丁合并）：

Recipe: format-patch/am: Keeping topics up to date

`git pull --rebase &lt;url&gt; &lt;branch&gt;`

然后，您可以在 rebase 期间修复冲突。大概你没有通过邮件发布你的主题，所以重新定位它不是问题。

如果您收到这样的补丁系列（作为维护者，或者作为发送给它的邮件列表的读者），将邮件保存到文件，创建一个新的主题分支并使用 _git am_ 导入承诺：

Recipe: format-patch/am: Importing patches

`git am &lt; patch`

值得指出的一个特性是三向合并，如果遇到冲突可以提供帮助：`git am -3`将使用补丁中包含的索引信息来确定合并基础。有关其他选项，请参阅 [git-am [1]](https://git-scm.com/docs/git-am) 。

## 也可以看看

[gittutorial [7]](https://git-scm.com/docs/gittutorial) ， [git-push [1]](https://git-scm.com/docs/git-push) ， [git-pull [1]](https://git-scm.com/docs/git-pull) ， [git-merge [1]](https://git-scm.com/docs/git-merge) ， [git-rebase [1]](https://git-scm.com/docs/git-rebase) ， [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) ， [git-send-email [1]](https://git-scm.com/docs/git-send-email) ， [git-am [ 1]](https://git-scm.com/docs/git-am)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件