# git-add

> 原文： [`git-scm.com/docs/git-add`](https://git-scm.com/docs/git-add)
> 
> 贡献者：[yulezheng](https://github.com/yulezheng)

## 名称

git-add - 将文件内容添加到暂存区（stage，或者叫 index）中

## 概要

```
git add [--verbose | -v] [--dry-run | -n] [--force | -f] [--interactive | -i] [--patch | -p]
	  [--edit | -e] [--[no-]all | --[no-]ignore-removal | [--update | -u]]
	  [--intent-to-add | -N] [--refresh] [--ignore-errors] [--ignore-missing] [--renormalize]
	  [--chmod=(+|-)x] [--] [<pathspec>…​]
```

## 描述

此命令使用工作树中当前的内容更新暂存区，为下一次提交（commit）暂存内容。它通常将现有路径的当前内容作为一个整体添加，但是通过一些选项，它还可以用于添加仅部分对于工作树的修改被应用的内容，或者删除工作树中不再存在的路径。

暂存区保存工作树内容的快照，并将此快照作为下一次提交（commit）的内容。因此，在对工作树进行任何更改之后，在运行 commit 命令之前，必须使用`add`命令将新的文件或有改动的文件添加到暂存区中。

在提交（commit）之前可以多次执行此命令。它只添加指定文件在 add 命令运行时刻的内容;如果您希望下次提交（commit）中包含后续更改，则必须再次运行`git add`以将新内容添加到暂存区中。

`git status`命令可用于列出改动被放入暂存区但还未提交的文件。

默认情况下，`git add`命令不会添加忽略的文件。如果在命令行中显式指定了任何忽略的文件，则`git add`将运行失败并且显示出忽略文件的列表。由 Git 执行的目录递归或文件名通配所覆盖到的忽略文件将被默认忽略。 `git add` 命令可使用`-f`（force）选项添加被忽略的文件。

有关将内容添加到提交（commit）的其他方法，请参阅 [git-commit [1]](https://git-scm.com/docs/git-commit) 。

## 选项

```
 <pathspec>… 
```

要添加内容的文件。可以用文件名通配符（例如`*.c`）来添加所有匹配的文件。还可以给出一个主目录名称（例如用`dir`添加`dir/file1`和`dir/file2`），将整个目录的当前状态作为一个主体来更新暂存区（例如，指定`dir`将不仅记录工作树中修改了文件`dir/file1`，添加了文件`dir/file2`，还会记录删除了文件`dir/file3`）。请注意，旧版本的 Git 默认忽略已删除的文件;如果要添加已修改或新增的文件但忽略已删除的文件，请使用`--no-all`选项。

有关&lt; pathspec&gt;的详细信息，请参阅 [gitglossary [7]](https://git-scm.com/docs/gitglossary) 中的 _pathspec_ 条目。

```
 -n 
```

```
 --dry-run 
```

实际上不添加文件（至暂存区），只显示文件是否存在和/或被忽略。

```
 -v 
```

```
 --verbose 
```

列出详细信息。

```
 -f 
```

```
 --force 
```

允许强制添加忽略的文件。

```
 -i 
```

```
 --interactive 
```

将工作树中被修改的内容以交互方式添加到暂存区中。提供可选的路径参数以将操作限制于工作树的一个子集中。详细信息请参阅“交互模式”。

```
 -p 
```

```
 --patch 
```

以交互方式选择暂存区和工作树之间的修改，并将它们添加到暂存区中。这使用户有机会在将修改后的内容添加到暂存区之前查看差异。

这实际上运行了`add --interactive`，但绕过初始命令菜单并直接跳转到`patch`子命令。详细信息请参阅“交互模式”。

```
 -e 
```

```
 --edit 
```

在编辑器中打开与暂存区不一致的内容，让用户编辑它。编辑器关闭后，调整代码块内容并将修改应用于暂存区。

此选项的目的是选择要应用哪些修改过的行，甚至修改要暂存的行的内容。这比使用交互式代码块选择器更快更灵活。但是，这很容易导致混淆从而产生不需要应用于暂存区的修改。参阅下面的 EDITING PATCHES。

```
 -u 
```

```
 --update 
```

只在已有的匹配&lt; pathspec&gt;的条目中更新暂存区。这将删除或修改暂存区条目以匹配工作树，但不添加新文件。

如果在使用`-u`选项时没有给出&lt; pathspec&gt;，将更新整个工作树中的所有跟踪文件（旧版本的 Git 将更新限定于当前目录及其子目录）。

```
 -A 
```

```
 --all 
```

```
 --no-ignore-removal 
```

将工作树中匹配&lt; pathspec&gt;的文件和暂存区中已有的条目内容更新到暂存区。这将添加，修改和删除暂存区条目以匹配工作树。

如果在使用`-A`选项时没有给出&lt; pathspec&gt;，将更新当前工作树中的所有文件（旧版本的 Git 将更新限定于当前目录及其子目录）。

```
 --no-all 
```

```
 --ignore-removal 
```

通过添加暂存区没有的新文件和工作树中有修改的文件来更新暂存区，但忽略已从工作树中删除的文件。当没有&lt; pathspec&gt;时，此选项不起作用。

此选项主要用于帮助习惯了旧版本 Git 的用户，其“git add&lt; pathspec&gt; ...”是“git add --no-all&lt; pathspec&gt; ...”的同义词，即忽略已删除文件。

```
 -N 
```

```
 --intent-to-add 
```

仅记录稍后将添加路径的事实。路径的条目放在暂存区中，没有内容。除其他外，这对于使用`git diff`显示此类文件的未分级内容并使用`git commit -a`提交它们非常有用。

```
 --refresh 
```

不添加文件，而只刷新它们在暂存区中的 stat（）信息。

```
 --ignore-errors 
```

如果由于索引错误而无法添加某些文件，不中止操作，而是继续添加其他文件。该命令仍将以非零状态退出。配置变量`add.ignoreErrors`可以设置为 true 以使其成为默认行为。

```
 --ignore-missing 
```

此选项只能与--dry-run 一起使用。通过使用此选项，用户可以检查是否将忽略某些给定文件，无论它们是否已存在于工作树中。

```
 --no-warn-embedded-repo 
```

默认情况下，若未使用`git submodule add`在`.gitmodules`中创建条目时就向暂存区中添加嵌入式存储库，`git add`会发出警告。此选项将禁止警告（例如在子模块上手动执行操作）。

```
 --renormalize 
```

对所有被跟踪的文件应用"clean"进程，以强制将它们再次添加到暂存区。在更改`core.autocrlf`配置或`text`属性以更正添加的文件中错误的 CRLF / LF 行结尾方式时，这很有用。该选项与`-u`同义。

```
 --chmod=(+|-)x 
```

覆盖被添加文件的可执行权限。可执行权限仅在暂存区中更改，磁盘上的文件保持不变。

```
 -- 
```

此选项可用于将命令行选项与文件列表分开（当文件名可能被误认为是命令行选项时很有用）。

## 例子

*   添加`Documentation`目录及其子目录下所有`*.txt`文件的内容：

    ```
    $ git add Documentation/\*.txt
    ```

    请注意，在此示例中，使用了星号`*`;这使命令包含来自`Documentation/`子目录的文件。

*   从所有 git - * .sh 脚本添加内容：

    ```
    $ git add git-*.sh
    ```

    因为这个例子除了星号还补充了其他条件（即明确地列出了文件），所以它不包括`subdir/git-foo.sh`。

## 交互模式

当命令进入交互模式时，它显示 _ 状态 _ 子命令的输出，然后进入其交互式命令循环。

命令循环显示可用的子命令列表，并给出提示“What now&gt;”。通常，当提示以单个 _&gt;结束时 ，您只能选择输入给定的一个选项，如下所示：

```
    *** Commands ***
      1: status       2: update       3: revert       4: add untracked
      5: patch        6: diff         7: quit         8: help
    What now> 1
```

如果符合的选项是唯一的，您也可以输入`s`或`sta`或`status`。

主命令循环有 6 个子命令（加上帮助和退出）。

```
 status 
```

运行这条命令将显示每个路径中 HEAD 和暂存区之间的变化（即，如果此时`git commit`将会提交什么），以及暂存区和工作树文件之间的变化（即你在`git commit`之前可以使用`git add`暂存什么）。示例输出如下所示：

```
              staged     unstaged path
     1:       binary      nothing foo.png
     2:     +403/-35        +1/-1 git-add--interactive.perl
```

它表明 foo.png 与 HEAD 有区别（但是文件为二进制因此无法显示行数）并且暂存区副本和工作树版本之间没有区别（如果工作树版本也存在不同，显示的就不是*nothing*而是*binary*）。另一个文件 git-add {litdd} interactive.perl，如果你提交了暂存区中的内容，则添加了 403 行并删除了 35 行，但工作树文件中仍有进一步修改（一次添加和一次删除）。

```
 update 
```

这会显示状态信息并发出“Update&gt;&gt;”提示。当提示以 double &gt;&gt;结束时，您可以进行多个选择，用空格或逗号连接。你也可以选择范围，例如，“2-5 7,9”表示从列表中选择 2,3,4,5,7,9。如果省略范围中的第二个数字，则选中第一个数字之后的所有选项。例如。 “7-”从列表中选择 7,8,9。你可以用 _*_ 来选择所有。

您选中的内容将用 _*_ 标志，如下所示：

```
           staged     unstaged path
  1:       binary      nothing foo.png
* 2:     +403/-35        +1/-1 git-add--interactive.perl
```

要删除选择，在输入的选项前添加`-`，如下所示：

```
Update>> -2
```

完成选择后，输入空行以暂存选中路径在工作树中的内容到暂存区中。

```
 revert 
```

这与 _update_ 具有非常相似的 UI，所选路径的暂存信息被恢复为 HEAD 版本的信息。恢复新路径将使它们不被跟踪。

```
 add untracked 
```

这与 _update_ 和*revert* 具有非常相似的 UI，并允许您向暂存区添加未跟踪的路径。

```
 patch 
```

这使您可以像选择器一样选择 _*status*_ 中的一个路径。选择路径后，它会显示暂存区和工作树文件之间的差异，并询问您是否要暂存各个部分的变动。您可以选择以下选项之一并键入 return：

```
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
```

在对所有部分进行选择之后，如果有任何部分被选中，则将所选的部分更新到暂存区。

通过将配置变量`interactive.singleKey`设置为`true`，即可不必在此处键入 return。

```
 diff 
```

此命令可以查看将要提交的内容（即 HEAD 和暂存区之间）。

## 编辑补丁

调用`git add -e`或从交互式块选择器中选择`e`，将在编辑器中打开补丁;退出编辑器后，结果将应用于暂存区。您可以随意对修补程序进行任意更改，但请注意，某些更改可能会导致令人困惑的结果，甚至会产生无法应用的修补程序。如果要完全中止操作（即，在暂存区中不做任何更新），只需删除修补程序的所有行。以下内容列出了您可能在修补程序中看到的一些常见内容，以及哪些编辑操作对它们有意义。

```
 added content 
```

添加的内容由以“+”开头的行表示。您可以通过删除它们来阻止暂存任何添加行。

```
 removed content 
```

删除的内容由以“ - ”开头的行表示。您可以通过将“ - ”转换为“ ”（空格）来阻止删除它们。

```
 modified content 
```

修改后的内容由“ - ”行（删除旧内容）后跟“+”行（添加替换内容）表示。您可以通过将“ - ”行转换为“ ”并删除“+”行来阻止暂存修改。请注意，仅修改其中的一半可能会引入异常的更改到暂存区。

还可以执行更复杂的操作。但要注意，因为补丁仅应用于暂存区而不是工作树，所以工作树将不执行索引中的更改。例如，向暂存区中引入的 HEAD 和工作树中都不存在的新行将作用于 commit（提交），但该行将在工作树中还原。

需要避免或非常谨慎地使用这些结构。

```
 removing untouched content 
```

索引和工作树之间没有差异的内容可以在上下文行中显示，以“ ”（空格）开头。您可以通过将空格转换为“ - ”来删除上下文行。生成的工作树文件将重新添加这些内容。

```
 modifying existing content 
```

还可以通过删除（将“ ”转换为“ - ”）和添加添加新内容到“+”行来修改上下文行。类似地，可以修改“+”行以用于现有的添加或修改。在所有情况下，新修改将在工作树中还原。

```
 new content 
```

您还可以添加补丁中不存在的新内容;只需添加新行，每行以“+”开头。添加将在工作树中还原。

还有一些操作应该完全避免，因为它们会使补丁无法应用：

*   添加上下文（“ ”）或删除（“ - ”）行

*   删除上下文或删除行

*   修改上下文或删除行的内容

## 也可以看看

[git-status [1]](https://git-scm.com/docs/git-status) [git-rm [1]](https://git-scm.com/docs/git-rm) [git-reset [1]](https://git-scm.com/docs/git-reset) [git-mv [1]](https://git-scm.com/docs/git-mv) [git-commit [1]](https://git-scm.com/docs/git-commit) [git-update-index [1]](https://git-scm.com/docs/git-update-index)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件