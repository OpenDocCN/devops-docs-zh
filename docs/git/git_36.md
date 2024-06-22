# git-bisect

> 原文： [`git-scm.com/docs/git-bisect`](https://git-scm.com/docs/git-bisect)

## 名称

git-bisect - 使用二进制搜索来查找引入错误的提交

## 概要

```
git bisect <subcommand> <options>
```

## 描述

该命令采用各种子命令，并根据子命令使用不同的选项：

```
git bisect start [--term-{old,good}=<term> --term-{new,bad}=<term>]
	  [--no-checkout] [<bad> [<good>...]] [--] [<paths>...]
git bisect (bad|new|<term-new>) [<rev>]
git bisect (good|old|<term-old>) [<rev>...]
git bisect terms [--term-good | --term-bad]
git bisect skip [(<rev>|<range>)...]
git bisect reset [<commit>]
git bisect (visualize|view)
git bisect replay <logfile>
git bisect log
git bisect run <cmd>...
git bisect help
```

此命令使用二进制搜索算法来查找项目历史记录中的哪个提交引入了错误。您可以通过首先告诉它已知包含该错误的“错误”提交以及在引入错误之前已知的“良好”提交来使用它。然后`git bisect`在这两个端点之间选择一个提交，并询问您所选的提交是“好”还是“坏”。它继续缩小范围，直到找到引入更改的确切提交。

实际上，`git bisect`可用于查找更改**项目任何**属性的提交;例如，修复错误的提交，或导致基准测试性能提高的提交。为了支持这种更普遍的用法，可以使用术语“旧”和“新”代替“好”和“坏”，或者您可以选择自己的术语。有关详细信息，请参阅下面的“替代术语”部分。

### 基本的 bisect 命令：start，bad，good

例如，假设您正在尝试查找破坏已知在项目的版本`v2.6.13-rc2`中工作的功能的提交。您按如下方式启动 bisect 会话：

```
$ git bisect start
$ git bisect bad                 # Current version is bad
$ git bisect good v2.6.13-rc2    # v2.6.13-rc2 is known to be good
```

一旦指定了至少一个错误提交和一个良好提交，`git bisect`会在该历史记录范围的中间选择一个提交，将其检出并输出类似于以下内容的内容：

```
Bisecting: 675 revisions left to test after this (roughly 10 steps)
```

您现在应该编译已签出的版本并对其进行测试。如果该版本正常工作，请键入

```
$ git bisect good
```

如果该版本被破坏，请键入

```
$ git bisect bad
```

那么`git bisect`会回复类似的东西

```
Bisecting: 337 revisions left to test after this (roughly 9 steps)
```

继续重复这个过程：编译树，测试它，并根据它是好是运行`git bisect good`或`git bisect bad`来请求下一个需要测试的提交。

最终将不再需要检查修改，命令将打印出第一个错误提交的描述。引用`refs/bisect/bad`将指向该提交。

### Bisect 重置

在 bisect 会话之后，要清除二分状态并返回到原始 HEAD，请发出以下命令：

```
$ git bisect reset
```

默认情况下，这会将您的树返回到`git bisect start`之前签出的提交。 （一个新的`git bisect start`也会这样做，因为它清理旧的二分状态。）

使用可选参数，您可以返回另一个提交：

```
$ git bisect reset <commit>
```

例如，`git bisect reset bisect/bad`将检出第一个错误修订，而`git bisect reset HEAD`将使您进入当前二等分提交并避免切换提交。

### 替代术语

有时你不是在寻找引入破坏的提交，而是寻找导致其他“旧”状态和“新”状态之间发生变化的提交。例如，您可能正在寻找引入特定修复的提交。或者您可能正在寻找第一个提交，其中源代码文件名最终都转换为您公司的命名标准。管他呢。

在这种情况下，使用“好”和“坏”这两个词来表示“改变前的状态”和“改变后的状态”可能会非常混乱。因此，您可以分别使用术语“旧”和“新”来代替“好”和“坏”。 （但请注意，您不能在单个会话中将“好”和“坏”与“旧”和“新”混合在一起。）

在这个更一般的用法中，您为`git bisect`提供了一个“新”提交，它具有一些属性和一个没有该属性的“旧”提交。每次`git bisect`签出提交时，您都会测试该提交是否具有该属性。如果是，请将提交标记为“新”;否则，将其标记为“旧”。完成二分时，`git bisect`将报告引入该属性的提交。

要使用“旧”和“新”而不是“好”和坏，您必须运行`git bisect start`而不提交参数，然后运行以下命令来添加提交：

```
git bisect old [<rev>]
```

表示提交是在寻求更改之前，或

```
git bisect new [<rev>...]
```

表明它是在之后。

要获取当前使用的术语的提醒，请使用

```
git bisect terms
```

您可以使用`git bisect terms --term-old`或`git bisect terms --term-good`获得旧的（分别为新的）术语。

如果您想使用自己的术语而不是“坏”/“好”或“新”/“旧”，您可以选择任何您喜欢的名称（现有的 bisect 子命令除外，如`reset`，`start`，... ）通过使用开始二分

```
git bisect start --term-old <term-old> --term-new <term-new>
```

例如，如果您正在寻找引入性能回归的提交，则可以使用

```
git bisect start --term-old fast --term-new slow
```

或者，如果您正在寻找修复错误的提交，您可以使用

```
git bisect start --term-new fixed --term-old broken
```

然后，使用`git bisect &lt;term-old&gt;`和`git bisect &lt;term-new&gt;`代替`git bisect good`和`git bisect bad`来标记提交。

### Bisect 可视化/查看

要查看 _gitk_ 中当前剩余的嫌疑人，请在二分过程中发出以下命令（子命令`view`可用作`visualize`的替代）：

```
$ git bisect visualize
```

如果未设置`DISPLAY`环境变量，则使用 _git log_ 。您还可以提供命令行选项，如`-p`和`--stat`。

```
$ git bisect visualize --stat
```

### Bisect 日志和平分重播

将标记的修订标记为好或坏后，发出以下命令以显示到目前为止已完成的操作：

```
$ git bisect log
```

如果发现在指定修订的状态时出错，可以将此命令的输出保存到文件，编辑它以删除不正确的条目，然后发出以下命令以返回到已更正的状态：

```
$ git bisect reset
$ git bisect replay that-file
```

### 避免测试提交

如果在 bisect 会话的中间，你知道建议的修订版不是一个好的测试版（例如它无法构建，你知道失败与你正在追逐的 bug 没有任何关系），你可以手动选择附近的提交并测试该提交。

例如：

```
$ git bisect good/bad			# previous round was good or bad.
Bisecting: 337 revisions left to test after this (roughly 9 steps)
$ git bisect visualize			# oops, that is uninteresting.
$ git reset --hard HEAD~3		# try 3 revisions before what
					# was suggested
```

然后编译并测试所选的修订版，然后以通常的方式将修订版标记为好或坏。

### Bisect 跳过

您可以通过发出命令让 Git 为您执行此操作，而不是自己选择附近的提交：

```
$ git bisect skip                 # Current version cannot be tested
```

但是，如果你跳过与你正在寻找的提交相邻的提交，Git 将无法准确地确定哪些提交是第一个提交。

您还可以使用范围表示法跳过一系列提交，而不是一次提交。例如：

```
$ git bisect skip v2.5..v2.6
```

这告诉 bisect 进程在`v2.5`之后，直到并包括`v2.6`，都不应该进行提交。

请注意，如果您还想跳过范围的第一次提交，您将发出命令：

```
$ git bisect skip v2.5 v2.5..v2.6
```

这告诉 bisect 进程应该跳过`v2.5`和`v2.6`（包括）之间的提交。

### 通过提供更多参数来平分开始来减少二分

通过在发出`bisect start`命令时指定路径参数，如果您知道要跟踪的问题涉及树的哪一部分，则可以进一步减少试验次数：

```
$ git bisect start -- arch/i386 include/asm-i386
```

如果您事先知道多个好的提交，则可以通过在发出`bisect start`命令时在错误提交后立即指定所有良好提交来缩小平分空间：

```
$ git bisect start v2.6.20-rc6 v2.6.20-rc4 v2.6.20-rc1 --
                   # v2.6.20-rc6 is bad
                   # v2.6.20-rc4 and v2.6.20-rc1 are good
```

### Bisect 运行

如果您有一个可以判断当前源代码是好还是坏的脚本，您可以通过发出命令来平分：

```
$ git bisect run my_script arguments
```

请注意，如果当前源代码是好/旧，则脚本（上例中的`my_script`）应该以代码 0 退出，如果当前源代码是当前源代码，则退出时使用 1 到 127（含）之间的代码（125 除外）。坏/新。

任何其他退出代码都将中止 bisect 进程。应该注意的是，通过`exit(-1)`终止的程序会留下$？ = 255，（参见 exit（3）手册页），因为该值被`& 0377`切断。

当无法测试当前源代码时，应使用特殊退出代码 125。如果脚本以此代码退出，则将跳过当前修订（请参阅上面的`git bisect skip`）。选择 125 作为用于此目的的最高敏感值，因为 POSIX shell 使用 126 和 127 来指示特定的错误状态（127 表示未找到命令，126 表示找到命令但不可执行 - 这些详细信息不问题，因为它们是脚本中的正常错误，就`bisect run`而言。

您可能经常发现在二等分会话期间您希望进行临时修改（例如，s / #define DEBUG 0 / #define DEBUG 1 /在头文件中，或者“没有此提交的修订版需要将此修补程序应用于解决方法另一个问题是这个二分法对于应用于被测试的修订版不感兴趣。

为了应对这种情况，在内部 _git bisect_ 找到要测试的下一个修订版之后，脚本可以在编译之前应用补丁，运行真实测试，然后决定是否修改（可能需要修改） patch）通过了测试，然后将树倒回到原始状态。最后，脚本应该以实际测试的状态退出，让`git bisect run`命令循环确定 bisect 会话的最终结果。

## OPTIONS

```
 --no-checkout 
```

不要在二分过程的每次迭代中签出新的工作树。相反，只需更新名为`BISECT_HEAD`的特殊引用，使其指向应测试的提交。

当您在每个步骤中执行的测试不需要签出树时，此选项可能很有用。

如果存储库是裸的，则假定为`--no-checkout`。

## 例子

*   在 v1.2 和 HEAD 之间自动平分破坏的构建：

    ```
    $ git bisect start HEAD v1.2 --      # HEAD is bad, v1.2 is good
    $ git bisect run make                # "make" builds the app
    $ git bisect reset                   # quit the bisect session
    ```

*   自动平分原点和 HEAD 之间的测试失败：

    ```
    $ git bisect start HEAD origin --    # HEAD is bad, origin is good
    $ git bisect run make test           # "make test" builds and tests
    $ git bisect reset                   # quit the bisect session
    ```

*   自动将破坏的测试用例一分为二：

    ```
    $ cat ~/test.sh
    #!/bin/sh
    make || exit 125                     # this skips broken builds
    ~/check_test_case.sh                 # does the test case pass?
    $ git bisect start HEAD HEAD~10 --   # culprit is among the last 10
    $ git bisect run ~/test.sh
    $ git bisect reset                   # quit the bisect session
    ```

    这里我们使用`test.sh`自定义脚本。在此脚本中，如果`make`失败，我们将跳过当前提交。如果测试用例通过，`check_test_case.sh`应为`exit 0`，否则为`exit 1`。

    如果`test.sh`和`check_test_case.sh`都在存储库之外，以防止 bisect，make 和测试进程与脚本之间的交互，则更安全。

*   通过临时修改自动平分（热修复）：

    ```
    $ cat ~/test.sh
    #!/bin/sh

    # tweak the working tree by merging the hot-fix branch
    # and then attempt a build
    if	git merge --no-commit hot-fix &&
    	make
    then
    	# run project specific test and report its status
    	~/check_test_case.sh
    	status=$?
    else
    	# tell the caller this is untestable
    	status=125
    fi

    # undo the tweak to allow clean flipping to the next commit
    git reset --hard

    # return control
    exit $status
    ```

    这在每次测试运行之前应用来自热修复分支的修改，例如，如果你的构建或测试环境发生了变化，那么旧版本可能需要修复已经更新的版本。 （确保热修复分支基于您正在二等分的所有修订中包含的提交，以便合并不会过多，或使用`git cherry-pick`而不是`git merge`。）

*   自动将破坏的测试用例一分为二：

    ```
    $ git bisect start HEAD HEAD~10 --   # culprit is among the last 10
    $ git bisect run sh -c "make || exit 125; ~/check_test_case.sh"
    $ git bisect reset                   # quit the bisect session
    ```

    这表明如果在单行上编写测试，则可以不使用运行脚本。

*   在损坏的存储库中找到对象图的良好区域

    ```
    $ git bisect start HEAD &lt;known-good-commit&gt; [ &lt;boundary-commit&gt; ... ] --no-checkout
    $ git bisect run sh -c '
    	GOOD=$(git for-each-ref "--format=%(objectname)" refs/bisect/good-*) &&
    	git rev-list --objects BISECT_HEAD --not $GOOD &gt;tmp.$$ &&
    	git pack-objects --stdout &gt;/dev/null &lt;tmp.$$
    	rc=$?
    	rm -f tmp.$$
    	test $rc = 0'

    $ git bisect reset                   # quit the bisect session
    ```

    在这种情况下，当 _git bisect run_ 结束时，bisect / bad 将引用具有至少一个父级的提交，其父级的可访问图形在 _git pack 对象 _ 所需的意义上是完全可遍历的。

*   在代码中寻找修复而不是回归

    ```
    $ git bisect start
    $ git bisect new HEAD    # current commit is marked as new
    $ git bisect old HEAD~10 # the tenth commit from now is marked as old
    ```

    要么：

```
$ git bisect start --term-old broken --term-new fixed
$ git bisect fixed
$ git bisect broken HEAD~10
```

### 获得帮助

使用`git bisect`获取简短的使用说明，使用`git bisect help`或`git bisect -h`获取长使用说明。

## 也可以看看

用 git bisect ， [git-blame [1]](https://git-scm.com/docs/git-blame) 对抗回归。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件