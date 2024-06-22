# git-rev-parse

> 原文： [`git-scm.com/docs/git-rev-parse`](https://git-scm.com/docs/git-rev-parse)

## 名称

git-rev-parse - 挑选和按摩参数

## 概要

```
git rev-parse [<options>] <args>…​
```

## 描述

许多 Git 瓷器命令采用混合标志（即以破折号 _-_ 开头的参数）和用于内部使用的底层 _git rev-list_ 命令的参数以及用于标记和参数的参数。他们在 _git rev-list_ 下游使用的其他命令。此命令用于区分它们。

## OPTIONS

### 操作模式

这些选项中的每一个都必须首先出现在命令行中。

```
 --parseopt 
```

在选项解析模式下使用 _git rev-parse_ （参见下面的 PARSEOPT 部分）。

```
 --sq-quote 
```

在 shell 引用模式下使用 _git rev-parse_ （参见下面的 SQ-QUOTE 部分）。与下面的`--sq`选项相比，此模式仅引用。命令输入没有其他任何操作。

### --parseopt 的选项

```
 --keep-dashdash 
```

仅在`--parseopt`模式下有意义。告诉选项解析器回显第一个`--`遇到而不是跳过它。

```
 --stop-at-non-option 
```

仅在`--parseopt`模式下有意义。允许选项解析器在第一个非选项参数处停止。这可用于解析自带选项的子命令。

```
 --stuck-long 
```

仅在`--parseopt`模式下有意义。如果可用，以长格式输出选项，并且参数卡住。

### 过滤选项

```
 --revs-only 
```

不要输出不适用于 _git rev-list_ 命令的标志和参数。

```
 --no-revs 
```

不要输出 _git rev-list_ 命令的标志和参数。

```
 --flags 
```

不要输出非标志参数。

```
 --no-flags 
```

不要输出标志参数。

### 输出选项

```
 --default <arg> 
```

如果用户没有给出参数，请改用`&lt;arg&gt;`。

```
 --prefix <arg> 
```

表现为好像从工作树的`&lt;arg&gt;`子目录调用了 _git rev-parse_ 。任何相对文件名都被解析为好像它们以`&lt;arg&gt;`为前缀，并将以该格式打印。

这可用于将参数转换为子目录中运行的命令，以便在移动到存储库的顶级后仍可使用它们。例如：

```
prefix=$(git rev-parse --show-prefix)
cd "$(git rev-parse --show-toplevel)"
# rev-parse provides the -- needed for 'set'
eval "set $(git rev-parse --sq --prefix "$prefix" -- "$@")"
```

```
 --verify 
```

验证是否提供了一个参数，并且可以将其转换为可用于访问对象数据库的原始 20 字节 SHA-1。如果是这样，将其发射到标准输出;否则，错误。

如果要确保输出实际命名对象数据库中的对象和/或可以用作所需的特定对象类型，可以将`^{type}`剥离运算符添加到参数中。例如，`git rev-parse "$VAR^{commit}"`将确保`$VAR`命名为 commit-ish 的现有对象（即提交或指向提交的带注释标记）。为了确保`$VAR`命名任何类型的现有对象，可以使用`git rev-parse "$VAR^{object}"`。

```
 -q 
```

```
 --quiet 
```

仅在`--verify`模式下有意义。如果第一个参数不是有效的对象名，请不要输出错误消息;而是以静默方式退出非零状态。有效对象名称的 SHA-1 在成功时打印到 stdout。

```
 --sq 
```

通常每个标志和参数输出一行。此选项使输出成为一行，正确引用以供 shell 使用。当你希望你的参数包含空格和换行符时很有用（例如当使用 pickaxe `-S`和 _git diff- *_ 时）。与`--sq-quote`选项相反，命令输入仍然像往常一样解释。

```
 --short[=length] 
```

与`--verify`相同，但将对象名称缩短为至少包含`length`字符的唯一前缀。最小长度为 4，默认值为`core.abbrev`配置变量的有效值（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

```
 --not 
```

显示对象名称时，请在 _^_ 前面添加前缀，并从已有的对象名称中删除 _^_ 前缀。

```
 --abbrev-ref[=(strict|loose)] 
```

对象名称的非模糊短名称。选项 core.warnAmbiguousRefs 用于选择严格的缩写模式。

```
 --symbolic 
```

通常，对象名称以 SHA-1 形式输出（可能有 _^_ 前缀）;此选项使它们以尽可能接近原始输入的形式输出。

```
 --symbolic-full-name 
```

这类似于--symbolic，但它省略了不是引用的输入（即分支或标记名称;或者更明确地消除歧义“head / master”形式，当您想要命名“master”分支时，有一个不幸的命名标记“master”），并将它们显示为完整的重新命名（例如“refs / heads / master”）。

### 对象选项

```
 --all 
```

显示`refs/`中找到的所有引用。

```
 --branches[=pattern] 
```

```
 --tags[=pattern] 
```

```
 --remotes[=pattern] 
```

分别显示所有分支，标签或远程跟踪分支（即分别在`refs/heads`，`refs/tags`或`refs/remotes`中找到的 refs）。

如果给出`pattern`，则仅显示与给定 shell glob 匹配的 refs。如果模式不包含通配符（`?`，`*`或`[`），则通过附加`/*`将其转换为前缀匹配。

```
 --glob=pattern 
```

显示与 shell glob 模式`pattern`匹配的所有引用。如果模式不以`refs/`开头，则会自动添加前缀。如果模式不包含通配符（`?`，`*`或`[`），则通过附加`/*`将其转换为前缀匹配。

```
 --exclude=<glob-pattern> 
```

不包括引用匹配 _&lt; glob-pattern&gt;_ 否则会考虑下一个`--all`，`--branches`，`--tags`，`--remotes`或`--glob`。重复此选项会累积排除模式，直至下一个`--all`，`--branches`，`--tags`，`--remotes`或`--glob`选项（其他选项或参数不会清除累积模式）。

当应用于`--branches`，`--tags`或`--remotes`时，给出的模式不应以`refs/heads`，`refs/tags`或`refs/remotes`开始，并且当应用于`--glob`时，它们必须以`refs/`开头]或`--all`。如果打算使用尾随 _/ *_ ，则必须明确给出。

```
 --disambiguate=<prefix> 
```

显示名称以给定前缀开头的每个对象。 &lt;前缀&gt;必须至少有 4 个十六进制数字才能避免错误地列出存储库中的每个对象。

### 文件选项

```
 --local-env-vars 
```

列出存储库本地的 GIT_ *环境变量（例如 GIT_DIR 或 GIT_WORK_TREE，但不是 GIT_EDITOR）。仅列出变量的名称，而不是它们的值，即使它们已设置。

```
 --git-dir 
```

如果已定义，则显示`$GIT_DIR`。否则显示.git 目录的路径。显示的路径，相对时，相对于当前工作目录。

如果未定义`$GIT_DIR`并且未检测到当前目录位于 Git 存储库或工作树中，则将消息打印到 stderr 并以非零状态退出。

```
 --absolute-git-dir 
```

与`--git-dir`类似，但其输出始终是规范化的绝对​​路径。

```
 --git-common-dir 
```

如果已定义则显示`$GIT_COMMON_DIR`，否则显示`$GIT_DIR`。

```
 --is-inside-git-dir 
```

当前工作目录低于存储库目录时打印“true”，否则为“false”。

```
 --is-inside-work-tree 
```

当前工作目录在存储库的工作树内打印“true”，否则为“false”。

```
 --is-bare-repository 
```

当存储库裸露时为“true”，否则为“false”。

```
 --is-shallow-repository 
```

当存储库浅时打印“true”，否则为“false”。

```
 --resolve-git-dir <path> 
```

检查&lt; path&gt;是指向有效存储库的有效存储库或 gitfile，并打印存储库的位置。如果&lt; path&gt;是一个 gitfile 然后打印到真实存储库的已解析路径。

```
 --git-path <path> 
```

解析“$ GIT_DIR /&lt; path&gt;”并考虑其他路径重定位变量，如$ GIT_OBJECT_DIRECTORY，$ GIT_INDEX_FILE ....例如，如果$ GIT_OBJECT_DIRECTORY 设置为/ foo / bar，则“git rev-parse --git-path objects / abc”返回/ foo / bar / abc。

```
 --show-cdup 
```

从子目录调用该命令时，显示相对于当前目录的顶级目录的路径（通常是“../”序列或空字符串）。

```
 --show-prefix 
```

从子目录调用该命令时，显示当前目录相对于顶级目录的路径。

```
 --show-toplevel 
```

显示顶级目录的绝对路径。

```
 --show-superproject-working-tree 
```

显示使用当前存储库作为其子模块的超级项目工作树（如果存在）的根的绝对路径。如果当前存储库未被任何项目用作子模块，则不输出任何内容。

```
 --shared-index-path 
```

在拆分索引模式下显示共享索引文件的路径，如果不在拆分索引模式下则显示为空。

### 其他选择

```
 --since=datestring 
```

```
 --after=datestring 
```

解析日期字符串，并为 _git rev-list_ 输出相应的--max-age =参数。

```
 --until=datestring 
```

```
 --before=datestring 
```

解析日期字符串，并为 _git rev-list_ 输出相应的--min-age =参数。

```
 <args>…​ 
```

要解析的标志和参数。

## 指定修订

修订参数 _&lt; rev&gt;_ 通常（但不一定）命名提交对象。它使用所谓的 _ 扩展 SHA-1_ 语法。以下是拼写对象名称的各种方法。列表末尾附近列出的名称包含提交中包含的树和 blob。

| 注意 | 本文档显示了 git 看到的“原始”语法。 shell 和其他 UI 可能需要额外的引用来保护特殊字符并避免单词拆分。 |

```
 <sha1>, e.g. dae86e1950b1277e545cee180551750029cfe735, dae86e 
```

完整的 SHA-1 对象名称（40 字节十六进制字符串），或存储库中唯一的前导子字符串。例如。如果存储库中没有其他对象以 dae86e 开头的对象，则 dae86e1950b1277e545cee180551750029cfe735 和 dae86e 都命名相同的提交对象。

```
 <describeOutput>, e.g. v1.7.4.2-679-g3bee7fb 
```

`git describe`的输出;即，最接近的标签，可选地后跟破折号和多次提交，然后是破折号， _g_ 和缩写的对象名称。

```
 <refname>, e.g. master, heads/master, refs/heads/master 
```

一个象征性的引用名称。例如。 _master_ 通常表示 _refs / heads / master_ 引用的提交对象。如果您碰巧同时拥有 _ 磁头/主控 _ 和 _ 标签/主控 _，您可以明确地说 _ 磁头/主控 _ 告诉 Git 您的意思。当含糊不清时，_&lt; refname&gt;_ 通过以下规则中的第一场比赛消除歧义：

1.  如果 _$ GIT_DIR /&lt; refname&gt;_ 存在，这就是你的意思（这通常只适用于`HEAD`，`FETCH_HEAD`，`ORIG_HEAD`，`MERGE_HEAD`和`CHERRY_PICK_HEAD`）;

2.  否则， _refs /&lt; refname&gt;_ 如果存在;

3.  否则， _refs / tags /&lt; refname&gt;_ 如果存在;

4.  否则， _refs / heads /&lt; refname&gt;_ 如果存在;

5.  否则， _refs / remotes /&lt; refname&gt;_ 如果存在;

6.  否则， _refs / remotes /&lt; refname&gt; / HEAD_ （如果存在）。

    `HEAD`命名您基于工作树中的更改的提交。 `FETCH_HEAD`记录您使用上次`git fetch`调用从远程存储库中获取的分支。 `ORIG_HEAD`是由以大刀阔斧的方式移动`HEAD`的命令创建的，用于在操作之前记录`HEAD`的位置，以便您可以轻松地将分支的尖端更改回运行它们之前的状态。 `MERGE_HEAD`在运行`git merge`时记录您要合并到分支中的提交。当您运行`git cherry-pick`时，`CHERRY_PICK_HEAD`会记录您正在挑选的提交。

    请注意，上述任何 _refs / *_ 情况可能来自 _$ GIT_DIR / refs_ 目录或来自 _$ GIT_DIR / packed-refs_ 文件。虽然未指定引用名称编码，但首选 UTF-8，因为某些输出处理可能会假定使用 UTF-8 中的引用名称。

```
 @ 
```

单独 _@_ 是`HEAD`的捷径。

```
 <refname>@{<date>}, e.g. master@{yesterday}, HEAD@{5 minutes ago} 
```

引用后跟 _@_ 后缀为日期规格括号对（例如 _{昨天}_ ， _{1 个月 2 周 3 天 1 小时 1 秒前}_ 或 _{1979-02-26 18:30:00}_ ）指定先前时间点的 ref 值。此后缀只能在引用名称后立即使用，并且引用必须具有现有日志（ _$ GIT_DIR / logs /&lt; ref&gt;_ ）。请注意，这会在给定时间查找**本地** ref 的状态;例如，上周本地 _ 主 _ 分支机构的内容。如果要查看在特定时间内提交的提交，请参阅`--since`和`--until`。

```
 <refname>@{<n>}, e.g. master@{1} 
```

后缀为 _@_ 的后缀为括号对中的序数规范（例如 _{1}_ ， _{15}_ ）指定第 n 个先验那个参考的价值。例如 _master @ {1}_ 是 _master_ 的前一个值，而 _master @ {5}_ 是 _master_ 的第 5 个先前值]。此后缀只能在引用名称后立即使用，并且引用必须具有现有日志（ _$ GIT_DIR / logs /&lt; refname&gt;_ ）。

```
 @{<n>}, e.g. @{1} 
```

您可以使用带有空参考部分的 _@_ 构造来获取当前分支的 reflog 条目。例如，如果你在分支 _blabla_ ，那么 _@ {1}_ 意味着与 _blabla @ {1}_ 相同。

```
 @{-<n>}, e.g. @{-1} 
```

构造 _@ { - &lt; n&gt;}_ 表示在当前分支/提交之前检出的第 n 个分支/提交。

```
 <branchname>@{upstream}, e.g. master@{upstream}, @{u} 
```

后缀 _@ {upstream}_ 到分支机构（简称 _&lt; branchname&gt; @ {u}_ ）指的是由 branchname 指定的分支设置为在其上构建的分支（配置为`branch.&lt;name&gt;.remote`和`branch.&lt;name&gt;.merge`）。缺少的 branchname 默认为当前的。当拼写为大写时，这些后缀也被接受，无论如何它们都意味着相同的东西。

```
 <branchname>@{push}, e.g. master@{push}, @{push} 
```

后缀 _@ {push}_ 报告分支“我们将推送到哪里”如果`branchname`在`branchname`被检出时运行（或者当前`HEAD`如果没有指定分支机构）。由于我们的推送目的地位于远程存储库中，当然，我们报告与该分支对应的本地跟踪分支（即 _refs / remotes /_ 中的内容）。

这是一个让它更清晰的例子：

```
$ git config push.default current
$ git config remote.pushdefault myfork
$ git checkout -b mybranch origin/master

$ git rev-parse --symbolic-full-name @{upstream}
refs/remotes/origin/master

$ git rev-parse --symbolic-full-name @{push}
refs/remotes/myfork/mybranch
```

请注意，在我们设置三角形工作流程的示例中，我们从一个位置拉出并推送到另一个位置。在非三角形工作流程中， _@ {push}_ 与 _@ {upstream}_ 相同，并且不需要它。

拼写为大写时也接受此后缀，无论情况如何都是相同的。

```
 <rev>^, e.g. HEAD^, v1.5.1⁰ 
```

修订参数的后缀 _^_ 表示该提交对象的第一个父级。 _^&lt; n&gt;_ 表示第 n 个亲本（即 _&lt; rev&gt; ^_ 等同于 _&lt; rev&gt; ^_ ）。作为特殊规则，_&lt; rev&gt; ^ 0_ 表示提交本身并且在 _&lt; rev&gt;时使用。_ 是引用提交对象的标记对象的对象名称。

```
 <rev>~<n>, e.g. master~3 
```

后缀 _〜&lt; n&gt;_ 到版本参数意味着提交对象是指定提交对象的第 n 代祖先，仅跟随第一个父对象。即 _&lt; rev&gt;〜_ 相当于 _&lt; rev&gt; ^^^_ ，其相当于 _&lt; rev&gt; ^ 1 ^ 1 ^_ 。请参阅下文，了解此表单的用法。

```
 <rev>^{<type>}, e.g. v0.99.8^{commit} 
```

后缀 _^_ 后跟括号对中包含的对象类型名称意味着在 _&lt; rev&gt;处取消引用对象。_ 递归地直到 _&lt; type&gt;类型的对象为止。找到 _ 或者不再解除引用对象（在这种情况下，barf）。例如，如果 _&lt; rev&gt;_ 是 commit-ish，_&lt; rev&gt; ^ {commit}_ 描述了相应的提交对象。类似地，如果 _&lt; rev&gt;_ 是树，_&lt; rev&gt; ^ {tree}_ 描述了相应的树对象。 _&lt; rev&gt; ^ 0_ 是 _&lt; rev&gt; ^ {commit}_ 的简写。

_rev ^ {object}_ 可以用来确保 _rev_ 命名一个存在的对象，而不需要 _rev_ 作为标签，并且不需要解除引用 _rev_ ;因为标签已经是一个对象，所以即使一次到达一个对象也不需要解除引用。

_rev ^ {tag}_ 可用于确保 _rev_ 标识现有标记对象。

```
 <rev>^{}, e.g. v0.99.8^{} 
```

后缀 _^_ 后跟空括号对意味着该对象可以是标记，并递归取消引用标记，直到找到非标记对象。

```
 <rev>^{/<text>}, e.g. HEAD^{/fix nasty bug} 
```

后缀 _^_ 到一个修订参数，后跟一个括号对，其中包含一个由斜杠引导的文本，与下面的 _：/ fix 讨厌错误 _ 语法相同，只是它返回可以从 _&lt; rev&gt;到达的最年轻的匹配提交 _^_ 之前的 _。

```
 :/<text>, e.g. :/fix nasty bug 
```

冒号后跟一个斜杠，后跟一个文本，命名一个提交，其提交消息与指定的正则表达式匹配。此名称返回可从任何 ref 访问的最新匹配提交，包括 HEAD。正则表达式可以匹配提交消息的任何部分。为了匹配以字符串开头的消息，可以使用例如 _：/ ^ foo_ 。特殊序列 _：/！_ 保留用于匹配的修饰符。 _：/！ - foo_ 执行负匹配，而 _：/ !! foo_ 匹配文字 _！_ 字符，然后是 _foo_ 。以 _ 开头的任何其他序列：/！_ 暂时保留。根据给定的文本，shell 的单词拆分规则可能需要额外的引用。

```
 <rev>:<path>, e.g. HEAD:README, :README, master:./README 
```

后缀 _：_ 后跟一个路径，命名由冒号前部分命名的树形对象中给定路径上的 blob 或树。 _：path_ （在冒号前面有空部分）是下面描述的语法的特例：在给定路径的索引中记录的内容。以 _./_ 或 _../_ 开头的路径是相对于当前工作目录的。给定路径将转换为相对于工作树的根目录。这对于从具有与工作树具有相同树结构的提交或树来解决 blob 或树最有用。

```
 :<n>:<path>, e.g. :0:README, :README 
```

冒号，可选地后跟一个阶段号（0 到 3）和一个冒号，后跟一个路径，在给定路径的索引中命名一个 blob 对象。缺少的阶段编号（以及其后面的冒号）命名阶段 0 条目。在合并期间，阶段 1 是共同的祖先，阶段 2 是目标分支的版本（通常是当前分支），阶段 3 是正在合并的分支的版本。

以下是 Jon Loeliger 的插图。提交节点 B 和 C 都是提交节点 A 的父节点。父提交从左到右排序。

```
G   H   I   J
 \ /     \ /
  D   E   F
   \  |  / \
    \ | /   |
     \|/    |
      B     C
       \   /
        \ /
         A
```

```
A =      = A⁰
B = A^   = A¹     = A~1
C = A²  = A²
D = A^^  = A¹¹   = A~2
E = B²  = A^²
F = B³  = A^³
G = A^^^ = A¹¹¹ = A~3
H = D²  = B^²    = A^^²  = A~2²
I = F^   = B³^    = A^³^
J = F²  = B³²   = A^³²
```

## 指定范围

遍历诸如`git log`之类的命令的历史操作在一组提交上，而不仅仅是单个提交。

对于这些命令，使用上一节中描述的表示法指定单个修订版意味着来自给定提交的提交`reachable`集。

提交的可达集是提交本身和其祖先链中的提交。

### 提交排除

```
 ^<rev> (caret) Notation 
```

要排除从提交可到达的提交，使用前缀 _^_ 表示法。例如。 _^ r1 r2_ 表示从 _r2_ 可到达的提交，但排除可从 _r1_ （即 _r1_ 及其祖先）到达的提交。

### 虚线范围符号

```
 The .. (two-dot) Range Notation 
```

_^ r1 r2_ 设置操作经常出现，因此有一个简写。如果你有两个提交 _r1_ 和 _r2_ （根据上面的 SPECIFYING REVISIONS 中解释的语法命名），你可以要求从 r2 可以访问的提交，不包括那些可以从 r1 到达的提交 _^ r1 r2_ 可以写成 _r1..r2_ 。

```
 The …​ (three-dot) Symmetric Difference Notation 
```

类似的符号 _r1 ... r2_ 称为 _r1_ 和 _r2_ 的对称差异，定义为 _r1 r2 - not $（git merge） -base --all r1 r2）_。它是可以从 _r1_ （左侧）或 _r2_ （右侧）中的任何一个到达的提交集，但不是两者都可以。

在这两个简写符号中，您可以省略一端并将其默认为 HEAD。例如，_ 原点.._ 是 _origin..HEAD_ 的简写并询问“自从我从原点分支分叉后我做了什么？”同样， _.origin_ 是 _HEAD..origin_ 的简写，并询问“自从我从它们分叉后，起源做了什么？”注意 _.._ 将意味着 _HEAD..HEAD_ ，这是一个空的范围，既可以从 HEAD 到达又无法到达。

### 其他&lt; rev&gt; ^父简写符号

存在三个其他的缩写，对于合并提交特别有用，用于命名由提交及其父提交形成的集合。

_r1 ^ @_ 符号表示 _r1_ 的所有亲本。

_r1 ^！_ 表示法包含 commit _r1_ 但不包括其所有父母。这个符号本身表示单个提交 _r1_ 。

_&lt; rev&gt; ^ - &lt; n&gt;_ 符号包括 _&lt; rev&gt;_ 但不包括第 n 个亲本（即 _&lt; rev&gt; ^&lt; n&gt; ..&lt; rev&gt;_ 的简写），_&lt; n&gt;如果没有给出，_ = 1。这通常对于合并提交很有用，您可以通过 _&lt; commit&gt; ^ -_ 来获取合并提交中合并的分支中的所有提交 _&lt; commit&gt;_ （包括 _&lt; commit&gt;_ 本身）。

虽然 _&lt; rev&gt; ^&lt; n&gt;_ 是关于指定单个提交父级，这三个符号也考虑其父级。例如你可以说 _HEAD ^ 2 ^ @_ ，但你不能说 _HEAD ^ @ ^ 2_ 。

## 修订范围摘要

```
 <rev> 
```

包括可从&lt; rev&gt;到达的提交（即&lt; rev&gt;及其祖先）。

```
 ^<rev> 
```

排除可从&lt; rev&gt;到达的提交（即&lt; rev&gt;及其祖先）。

```
 <rev1>..<rev2> 
```

包括可从&lt; rev2&gt;到达的提交但不包括那些可以从&lt; rev1&gt;到达的那些。何时&lt; rev1&gt;或者&lt; rev2&gt;省略，默认为`HEAD`。

```
 <rev1>...<rev2> 
```

包括可从&lt; rev1&gt;到达的提交或者&lt; rev2&gt;但排除那两个可以访问的。何时&lt; rev1&gt;或者&lt; rev2&gt;省略，默认为`HEAD`。

```
 <rev>^@, e.g. HEAD^@ 
```

后缀为 _^_ 后跟 at 符号与列出 _&lt; rev&gt;的所有父项相同。_ （意思是，包括从其父母可以访问的任何内容，但不包括提交本身）。

```
 <rev>^!, e.g. HEAD^! 
```

后缀为 _^_ 后跟感叹号与提交 _&lt; rev&gt;相同。_ 然后它的所有父母都以 _^_ 为前缀来排除它们（以及它们的祖先）。

```
 <rev>^-<n>, e.g. HEAD^-, HEAD^-2 
```

等同于 _&lt; rev&gt; ^&lt; n&gt; ..&lt; rev&gt;_ ，_&lt; n&gt;如果没有给出，_ = 1。

以下是使用上面的 Loeliger 插图的一些示例，其中注释的扩展和选择中的每个步骤都经过仔细说明：

```
   Args   Expanded arguments    Selected commits
   D                            G H D
   D F                          G H I J D F
   ^G D                         H D
   ^D B                         E I J F B
   ^D B C                       E I J F B C
   C                            I J F C
   B..C   = ^B C                C
   B...C  = B ^F C              G H D E B C
   B^-    = B^..B
	  = ^B¹ B              E I J F B
   C^@    = C¹
	  = F                   I J F
   B^@    = B¹ B² B³
	  = D E F               D G H E F I J
   C^!    = C ^C^@
	  = C ^C¹
	  = C ^F                C
   B^!    = B ^B^@
	  = B ^B¹ ^B² ^B³
	  = B ^D ^E ^F          B
   F^! D  = F ^I ^J D           G H D F
```

## PARSEOPT

在`--parseopt`模式下， _git rev-parse_ 帮助按摩选项为 C 脚本提供相同的设施。它作为一个选项规范化器（例如拆分单个开关聚合值），有点像`getopt(1)`。

它采用标准输入来解析和理解选项的规范，并在标准输出上回显适合`sh(1)` `eval`的字符串，用规范化的参数替换参数。如果出现错误，它会输出标准错误流的使用情况，并以代码 129 退出。

注意：确保在将结果传递给`eval`时引用结果。请参阅下面的示例。

### 输入格式

_git rev-parse --parseopt_ 输入格式是完全基于文本的。它有两个部分，由仅包含`--`的线分隔。分隔符之前的行（应该是一个或多个）用于使用。分隔符后面的行描述了选项。

每行选项都有这种格式：

```
<opt-spec><flags>*<arg-hint>? SP+ help LF
```

```
 <opt-spec> 
```

它的格式是短选项字符，然后用逗号分隔的长选项名称。两个部件都不是必需的，但至少需要一个部件。不得包含任何`&lt;flags&gt;`字符。 `h,help`，`dry-run`和`f`是正确的`&lt;opt-spec&gt;`的例子。

```
 <flags> 
```

`&lt;flags&gt;`属于`*`，`=`，`?`或`!`。

*   如果选项采用参数，请使用`=`。

*   使用`?`表示该选项采用可选参数。您可能希望使用`--stuck-long`模式能够明确地解析可选参数。

*   使用`*`表示不应在为`-h`参数生成的用法中列出此选项。 [gitcli [7]](https://git-scm.com/docs/gitcli) 中记载的`--help-all`显示了它。

*   使用`!`不会使相应的否定长选项可用。

```
 <arg-hint> 
```

`&lt;arg-hint&gt;`（如果指定）用作帮助输出中参数的名称，用于带参数的选项。 `&lt;arg-hint&gt;`由第一个空格终止。习惯上使用短划线来分隔多字参数提示中的单词。

剥离空格后，该行的其余部分用作与选项关联的帮助。

空行被忽略，与此规范不匹配的行将用作选项组标题（使用空格开始行以有意创建此类行）。

### 例

```
OPTS_SPEC="\
some-command [<options>] <args>...

some-command does foo and bar!
--
h,help    show the help

foo       some nifty option --foo
bar=      some cool option --bar with an argument
baz=arg   another cool option --baz with a named argument
qux?path  qux may take a path argument but has meaning by itself

  An option group Header
C?        option C with an optional argument"

eval "$(echo "$OPTS_SPEC" | git rev-parse --parseopt -- "$@" || echo exit $?)"
```

### 用法文字

在上例中`"$@"`为`-h`或`--help`时，将显示以下用法文本：

```
usage: some-command [<options>] <args>...

    some-command does foo and bar!

    -h, --help            show the help
    --foo                 some nifty option --foo
    --bar ...             some cool option --bar with an argument
    --baz <arg>           another cool option --baz with a named argument
    --qux[=<path>]        qux may take a path argument but has meaning by itself

An option group Header
    -C[...]               option C with an optional argument
```

## SQ-QUOTE

在`--sq-quote`模式下， _git rev-parse_ 在标准输出上回显适合`sh(1)` `eval`的单行。该行是通过规范化`--sq-quote`之后的参数来完成的。除了引用参数之外别无其他。

如果您希望在输出引用 shell 之前 _git rev-parse_ 仍然按常规解释命令输入，请参阅`--sq`选项。

### 例

```
$ cat >your-git-script.sh <<\EOF
#!/bin/sh
args=$(git rev-parse --sq-quote "$@")   # quote user-supplied arguments
command="git frotz -n24 $args"          # and use it inside a handcrafted
					# command line
eval "$command"
EOF

$ sh your-git-script.sh "a b'c"
```

## 例子

*   打印当前提交的对象名称：

    ```
    $ git rev-parse --verify HEAD
    ```

*   从$ REV shell 变量中的修订版打印提交对象名称：

    ```
    $ git rev-parse --verify $REV^{commit}
    ```

    如果$ REV 为空或不是有效修订，则会出错。

*   与上面类似：

    ```
    $ git rev-parse --default master --verify $REV
    ```

    但是如果$ REV 为空，则将打印来自 master 的提交对象名称。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件