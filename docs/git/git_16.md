# git-mergetool

> 原文： [`git-scm.com/docs/git-mergetool`](https://git-scm.com/docs/git-mergetool)

## 名称

git-mergetool - 运行合并冲突解决工具来解决合并冲突

## 概要

```
git mergetool [--tool=<tool>] [-y | --[no-]prompt] [<file>…​]
```

## 描述

使用`git mergetool`运行多个合并实用程序之一来解决合并冲突。它通常在 _git merge_ 之后运行。

如果一个或多个&lt; file&gt;给出参数，将运行合并工具程序以解决每个文件的差异（跳过那些没有冲突的文件）。指定目录将包括该路径中的所有未解析文件。如果没有&lt; file&gt;如果指定了名称， _git mergetool_ 将在每个具有合并冲突的文件上运行合并工具程序。

## OPTIONS

```
 -t <tool> 
```

```
 --tool=<tool> 
```

使用&lt; tool&gt;指定的合并解析程序。有效值包括 emerge，gvimdiff，kdiff3，meld，vimdiff 和 tortoisemerge。运行`git mergetool --tool-help`以获取有效&lt; tool&gt;的列表设置。

如果未指定合并解析程序， _git mergetool_ 将使用配置变量`merge.tool`。如果未设置配置变量`merge.tool`， _git mergetool_ 将选择合适的默认值。

您可以通过设置配置变量`mergetool.&lt;tool&gt;.path`显式提供工具的完整路径。例如，您可以通过设置`mergetool.kdiff3.path`配置 kdiff3 的绝对路径。否则， _git mergetool_ 假定该工具在 PATH 中可用。

可以通过指定要在配置变量`mergetool.&lt;tool&gt;.cmd`中调用的命令行来自定义 _git mergetool_ 来运行备用程序，而不是运行其中一个已知的合并工具程序。

当使用此工具调用 _git mergetool_ 时（通过`-t`或`--tool`选项或`merge.tool`配置变量），将在`$BASE`设置为名称的情况下调用配置的命令行包含合并公共基础的临时文件（如果有）; `$LOCAL`设置为包含当前分支上文件内容的临时文件的名称; `$REMOTE`设置为包含要合并的文件内容的临时文件的名称，`$MERGED`设置为合并工具应写入合并解析结果的文件的名称。

如果自定义合并工具正确指示合并解析及其退出代码成功，则配置变量`mergetool.&lt;tool&gt;.trustExitCode`可以设置为`true`。否则， _git mergetool_ 将提示用户在自定义工具退出后指示分辨率是否成功。

```
 --tool-help 
```

打印可能与`--tool`一起使用的合并工具列表。

```
 -y 
```

```
 --no-prompt 
```

在每次调用合并解析程序之前不要提示。如果使用`--tool`选项或`merge.tool`配置变量显式指定了合并解析程序，则这是默认值。

```
 --prompt 
```

在每次调用合并解析程序之前提示，以便为用户提供跳过路径的机会。

```
 -g 
```

```
 --gui 
```

当使用`-g`或`--gui`选项调用 _git-mergetool_ 时，将从配置的`merge.guitool`变量而不是`merge.tool`中读取默认合并工具。

```
 --no-gui 
```

这将覆盖先前的`-g`或`--gui`设置，并且将从配置的`merge.tool`变量中读取默认合并工具。

```
 -O<orderfile> 
```

按照&lt; orderfile&gt;中指定的顺序处理文件，每行有一个 shell glob 模式。这会覆盖`diff.orderFile`配置变量（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。要取消`diff.orderFile`，请使用`-O/dev/null`。

## 临时文件

`git mergetool`在解析合并时创建`*.orig`备份文件。一旦文件合并并且其`git mergetool`会话已完成，可以安全地删除它们。

将`mergetool.keepBackup`配置变量设置为`false`会导致`git mergetool`在文件成功合并时自动删除备份。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件