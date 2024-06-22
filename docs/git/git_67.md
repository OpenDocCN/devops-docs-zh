# git-checkout-index

> 原文： [`git-scm.com/docs/git-checkout-index`](https://git-scm.com/docs/git-checkout-index)

## 名称

git-checkout-index - 将文件从索引复制到工作树

## 概要

```
git checkout-index [-u] [-q] [-a] [-f] [-n] [--prefix=<string>]
		   [--stage=<number>|all]
		   [--temp]
		   [-z] [--stdin]
		   [--] [<file>…​]
```

## 描述

将索引中列出的所有文件复制到工作目录（不覆盖现有文件）。

## OPTIONS

```
 -u 
```

```
 --index 
```

更新索引文件中已签出条目的统计信息。

```
 -q 
```

```
 --quiet 
```

如果文件存在或不在索引中，请保持安静

```
 -f 
```

```
 --force 
```

强制覆盖现有文件

```
 -a 
```

```
 --all 
```

检出索引中的所有文件。不能与显式文件名一起使用。

```
 -n 
```

```
 --no-create 
```

不要签出新文件，只刷新已经签出的文件。

```
 --prefix=<string> 
```

创建文件时，请添加&lt; string&gt; （通常是包含尾随的目录/）

```
 --stage=<number>|all 
```

不要检出未合并的条目，而是从命名阶段复制出文件。 &lt;数&gt;必须介于 1 和 3 之间。注意： - stage = all 自动隐含--temp。

```
 --temp 
```

而不是将文件复制到工作目录，而是将内容写入临时文件。临时名称关联将写入 stdout。

```
 --stdin 
```

而不是从命令行获取路径列表，从标准输入中读取路径列表。默认情况下，路径由 LF（即每行一个路径）分隔。

```
 -z 
```

仅对`--stdin`有意义;路径用 NUL 字符而不是 LF 分隔。

```
 -- 
```

不要将任何更多的参数解释为选项。

标志的顺序过去很重要，但现在不再重要。

刚做`git checkout-index`什么也没做。你可能意味着`git checkout-index -a`。如果你想强制它，你想要`git checkout-index -f -a`。

直觉不是这里的目标。重复性是。 “没有参数意味着没有工作”行为的原因是你应该能够做到的脚本：

```
$ find . -name '*.h' -print0 | xargs -0 git checkout-index -f --
```

这将强制所有现有的`*.h`文件替换为其缓存副本。如果一个空命令行暗示“全部”，那么这将强制刷新索引中的所有内容，这不是重点。但是因为 _git checkout-index_ 接受--stdin 它会更快使用：

```
$ find . -name '*.h' -print0 | git checkout-index -f -z --stdin
```

当你知道其余的是文件名时，`--`是个好主意。它可以防止文件名出现问题，例如`-a`。在脚本中使用`--`可能是一个很好的策略。

## 使用--temp 或--stage = all

当使用`--temp`（或`--stage=all`暗示）_ 时，git checkout-index_ 将为每个要检出的索引条目创建一个临时文件。索引不会使用统计信息进行更新。如果调用者需要所有未合并条目的所有阶段，以便外部合并工具可以处理未合并文件，则这些选项非常有用。

列表将写入 stdout，提供临时文件名与跟踪路径名的关联。列表格式有两种变体：

1.  tempname TAB 路径 RS

    第一种格式是省略`--stage`或不是`--stage=all`时使用的格式。字段 tempname 是保存文件内容的临时文件名，path 是索引中的跟踪路径名。仅输出所请求的条目。

2.  stage1temp SP stage2temp SP stage3tmp TAB 路径 RS

    第二种格式是`--stage=all`时使用的格式。如果索引中存在阶段条目，则三阶段临时字段（stage1temp，stage2temp，stage3temp）列出临时文件的名称;如果没有阶段条目，则列出`.`。将始终从输出中省略仅具有阶段 0 条目的路径。

在两种格式中，RS（记录分隔符）默认为换行符，但如果在命令行上传递-z，则为空字节。临时文件名始终是安全字符串;它们永远不会包含目录分隔符或空格字符。 path 字段始终相对于当前目录，临时文件名始终相对于顶级目录。

如果要复制到临时文件的对象是符号链接，则链接的内容将写入普通文件。最终用户或瓷器可以使用这些信息。

## 例子

```
 To update and refresh only the files already checked out 
```

```
$ git checkout-index -n -f -a && git update-index --ignore-missing --refresh
```

```
 Using git checkout-index to "export an entire tree" 
```

前缀能力基本上使得 _git checkout-index_ 用作“导出为树”功能变得微不足道。只需将所需的树读入索引，然后执行：

```
$ git checkout-index --prefix=git-export-dir/ -a
```

`git checkout-index`将索引“导出”到指定目录中。

最后的“/”很重要。导出的名称实际上只是以指定的字符串为前缀。将此与下面的示例进行对比。

```
 Export files with a prefix 
```

```
$ git checkout-index --prefix=.merged- Makefile
```

这将检出当前缓存的`Makefile`副本到文件`.merged-Makefile`中。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件