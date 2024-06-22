# git-check-ignore

> 原文： [`git-scm.com/docs/git-check-ignore`](https://git-scm.com/docs/git-check-ignore)

## 名称

git-check-ignore - 调试 gitignore / exclude 文件

## 概要

```
git check-ignore [<options>] <pathname>…​
git check-ignore [<options>] --stdin
```

## 描述

对于通过命令行或通过`--stdin`从文件给出的每个路径名，检查文件是否被.gitignore（或排除机制的其他输入文件）排除，并输出路径（如果它被排除）。

默认情况下，跟踪文件根本不显示，因为它们不受排除规则的约束;但请参阅'--no-index'。

## OPTIONS

```
 -q, --quiet 
```

不要输出任何内容，只需设置退出状态即可。这仅对单个路径名有效。

```
 -v, --verbose 
```

还输出有关每个给定路径名的匹配模式（如果有）的详细信息。有关排除源内和排除源之间的优先级规则，请参阅 [gitignore [5]](https://git-scm.com/docs/gitignore) 。

```
 --stdin 
```

从标准输入读取路径名，每行一个，而不是命令行。

```
 -z 
```

输出格式被修改为可机器解析（见下文）。如果还给出`--stdin`，则输入路径用 NUL 字符而不是换行符分隔。

```
 -n, --non-matching 
```

显示与任何模式都不匹配的给定路径。这仅在启用`--verbose`时才有意义，否则将无法区分匹配模式的路径和不匹配模式的路径。

```
 --no-index 
```

在进行检查时不要查看索引。这可以用于调试由例如跟踪路径的原因。 `git add .`并且未被用户预期的规则忽略，或者在开发包含否定的模式以匹配先前使用`git add -f`添加的路径时。

## OUTPUT

默认情况下，将输出与忽略模式匹配的任何给定路径名，每行一个。如果没有模式匹配给定路径，则不会为该路径输出任何内容;这意味着路径不会被忽略。

如果指定了`--verbose`，则输出是以下形式的一系列行：

&lt;信源&gt; &lt;结肠癌和 GT; &lt; LINENUM&gt; &lt;结肠癌和 GT; &lt;模式&gt; &lt; HT&gt; &lt;路径名&gt;

&lt;路径名&gt;是要查询的文件的路径，&lt; pattern&gt;是匹配模式，&lt; source&gt;是模式的源文件，&lt; linenum&gt;是该源中模式的行号。如果模式包含`!`前缀或`/`后缀，则它将保留在输出中。 &lt;信源&gt;在引用`core.excludesFile`配置的文件时，或者在引用`.git/info/exclude`或每个目录的排除文件时相对于存储库根目录时，它将是绝对路径。

如果指定了`-z`，则输出中的路径名由空字符分隔;如果还指定了`--verbose`，则还使用空字符代替冒号和硬标签：

&lt;信源&gt; &lt; NULL&gt; &lt; LINENUM&gt; &lt; NULL&gt; &lt;模式&gt; &lt; NULL&gt; &lt;路径名&gt; &lt; NULL&gt;

如果指定了`-n`或`--non-matching`，则还将输出不匹配的路径名，在这种情况下，每个输出记录中的所有字段除了&lt; pathname&gt;将是空的。这在非交互式运行时非常有用，因此可以将文件递增地流式传输到长时间运行的检查忽略过程的 STDIN，并且对于每个文件，STDOUT 将指示该文件是否与模式匹配。 （如果没有这个选项，就不可能判断给定文件的输出是否缺少意味着它是否与任何模式不匹配，或者输出是否尚未生成。）

缓冲发生在 [git [1]](https://git-scm.com/docs/git) 的`GIT_FLUSH`选项中。调用者负责避免因输入缓冲区过满或从空输出缓冲区读取而导致的死锁。

## 退出状态

```
 0 
```

忽略一个或多个提供的路径。

```
 1 
```

没有提供的路径被忽略。

```
 128 
```

遇到致命错误。

## 也可以看看

[gitignore [5]](https://git-scm.com/docs/gitignore) [git-config [1]](https://git-scm.com/docs/git-config) [git-ls-files [1]](https://git-scm.com/docs/git-ls-files)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件