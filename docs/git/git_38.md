# git-grep

> 原文： [`git-scm.com/docs/git-grep`](https://git-scm.com/docs/git-grep)

## 名称

git-grep - 打印与图案匹配的线条

## 概要

```
git grep [-a | --text] [-I] [--textconv] [-i | --ignore-case] [-w | --word-regexp]
	   [-v | --invert-match] [-h|-H] [--full-name]
	   [-E | --extended-regexp] [-G | --basic-regexp]
	   [-P | --perl-regexp]
	   [-F | --fixed-strings] [-n | --line-number] [--column]
	   [-l | --files-with-matches] [-L | --files-without-match]
	   [(-O | --open-files-in-pager) [<pager>]]
	   [-z | --null]
	   [ -o | --only-matching ] [-c | --count] [--all-match] [-q | --quiet]
	   [--max-depth <depth>] [--[no-]recursive]
	   [--color[=<when>] | --no-color]
	   [--break] [--heading] [-p | --show-function]
	   [-A <post-context>] [-B <pre-context>] [-C <context>]
	   [-W | --function-context]
	   [--threads <num>]
	   [-f <file>] [-e] <pattern>
	   [--and|--or|--not|(|)|-e <pattern>…​]
	   [--recurse-submodules] [--parent-basename <basename>]
	   [ [--[no-]exclude-standard] [--cached | --no-index | --untracked] | <tree>…​]
	   [--] [<pathspec>…​]
```

## 描述

在工作树中的跟踪文件中查找指定的模式，在索引文件中注册的 blob 或给定树对象中的 blob。模式是由换行符分隔的一个或多个搜索表达式的列表。搜索表达式匹配所有行的空字符串。

## 组态

```
 grep.lineNumber 
```

如果设置为 true，则默认启用`-n`选项。

```
 grep.column 
```

如果设置为 true，则默认启用`--column`选项。

```
 grep.patternType 
```

设置默认匹配行为。使用 _basic_ ，_ 扩展 _，_ 固定 _ 或 _perl_ 的值将启用`--basic-regexp`，`--extended-regexp`，`--fixed-strings` ，或`--perl-regexp`选项相应，而值 _ 默认 _ 将返回默认匹配行为。

```
 grep.extendedRegexp 
```

如果设置为 true，则默认启用`--extended-regexp`选项。当`grep.patternType`选项设置为 _ 默认值 _ 以外的值时，将忽略此选项。

```
 grep.threads 
```

要使用的 grep 工作线程数。如果未设置（或设置为 0），则默认使用 8 个线程（暂时）。

```
 grep.fullName 
```

如果设置为 true，则默认启用`--full-name`选项。

```
 grep.fallbackToNoIndex 
```

如果设置为 true，如果 git grep 在 git 存储库之外执行，则回退到 git grep --no-index。默认为 false。

## OPTIONS

```
 --cached 
```

不是搜索工作树中的跟踪文件，而是搜索索引文件中注册的 blob。

```
 --no-index 
```

搜索当前目录中不由 Git 管理的文件。

```
 --untracked 
```

除了搜索工作树中的跟踪文件外，还可以搜索未跟踪的文件。

```
 --no-exclude-standard 
```

同时通过不遵守`.gitignore`机制来搜索被忽略的文件。仅适用于`--untracked`。

```
 --exclude-standard 
```

不要注意通过`.gitignore`机制指定的忽略文件。仅在使用`--no-index`搜索当前目录中的文件时有用。

```
 --recurse-submodules 
```

递归搜索已在存储库中初始化和检出的每个子模块。当与&lt; tree&gt;组合使用时选项所有子模块输出的前缀将是父项目的&lt; tree&gt;的名称。宾语。

```
 -a 
```

```
 --text 
```

处理二进制文件就像它们是文本一样。

```
 --textconv 
```

尊重 textconv 过滤器设置。

```
 --no-textconv 
```

不要尊重 textconv 过滤器设置。这是默认值。

```
 -i 
```

```
 --ignore-case 
```

忽略模式和文件之间的大小写差异。

```
 -I 
```

与二进制文件中的模式不匹配。

```
 --max-depth <depth> 
```

对于每个&lt; pathspec&gt;在命令行上给出，最多下降&lt; depth&gt;目录级别。值-1 表示没有限制。如果&lt; pathspec＆gt ;,则忽略此选项包含活动的通配符。换句话说，如果“a *”匹配名为“a *”的目录，则“*”字面匹配，因此--max-depth 仍然有效。

```
 -r 
```

```
 --recursive 
```

与`--max-depth=-1`相同;这是默认值。

```
 --no-recursive 
```

与`--max-depth=0`相同。

```
 -w 
```

```
 --word-regexp 
```

仅在单词边界处匹配模式（从行的开头开始，或者以非单词字符开头;在行的末尾结束或后跟非单词字符）。

```
 -v 
```

```
 --invert-match 
```

选择不匹配的行。

```
 -h 
```

```
 -H 
```

默认情况下，该命令显示每个匹配的文件名。 `-h`选项用于抑制此输出。 `-H`是完整性的，除了它覆盖了之前在命令行中给出的`-h`之外什么都不做。

```
 --full-name 
```

从子目录运行时，该命令通常输出相对于当前目录的路径。此选项强制路径相对于项目顶级目录输出。

```
 -E 
```

```
 --extended-regexp 
```

```
 -G 
```

```
 --basic-regexp 
```

使用 POSIX 扩展/基本正则表达式来表示模式。默认是使用基本正则表达式。

```
 -P 
```

```
 --perl-regexp 
```

对模式使用与 Perl 兼容的正则表达式。

对这些类型的正则表达式的支持是可选的编译时依赖性。如果 Git 没有编译并支持它们，那么提供此选项将导致它死亡。

```
 -F 
```

```
 --fixed-strings 
```

对模式使用固定字符串（不要将模式解释为正则表达式）。

```
 -n 
```

```
 --line-number 
```

将行号前缀为匹配行。

```
 --column 
```

从匹配行的开头开始对第一个匹配的 1 索引字节偏移进行前缀。

```
 -l 
```

```
 --files-with-matches 
```

```
 --name-only 
```

```
 -L 
```

```
 --files-without-match 
```

不显示每个匹配的行，而是仅显示包含（或不包含）匹配的文件的名称。为了更好地与 _git diff_ 兼容，`--name-only`是`--files-with-matches`的同义词。

```
 -O[<pager>] 
```

```
 --open-files-in-pager[=<pager>] 
```

打开寻呼机中的匹配文件（不是 _grep_ 的输出）。如果寻呼机恰好是“较少”或“vi”，并且用户仅指定了一个模式，则第一个文件将自动定位在第一个匹配位置。 `pager`参数是可选的;如果指定，它必须粘在没有空格的选项上。如果未指定`pager`，将使用默认寻呼机（参见 [git-config [1]](https://git-scm.com/docs/git-config) 中的`core.pager`）。

```
 -z 
```

```
 --null 
```

输出\ 0 而不是通常在文件名后面的字符。

```
 -o 
```

```
 --only-matching 
```

仅打印匹配行的匹配（非空）部分，每个此类部分位于单独的输出行上。

```
 -c 
```

```
 --count 
```

而不是显示每个匹配的行，而是显示匹配的行数。

```
 --color[=<when>] 
```

显示彩色火柴。该值必须始终为（默认值），never 或 auto。

```
 --no-color 
```

关闭匹配突出显示，即使配置文件提供默认的颜色输出。与`--color=never`相同。

```
 --break 
```

在不同文件的匹配项之间打印空行。

```
 --heading 
```

在该文件中的匹配项上方显示文件名，而不是在每个显示的行的开头。

```
 -p 
```

```
 --show-function 
```

显示包含匹配函数名称的上一行，除非匹配行是函数名称本身。名称的确定方式与 _git diff_ 计算出补丁程序块标题的方式相同（参见 _ 在 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 中定义自定义的 hunk-header_ ）。

```
 -<num> 
```

```
 -C <num> 
```

```
 --context <num> 
```

显示&lt; num&gt;前导和尾随行，并在连续的匹配组之间放置包含`--`的行。

```
 -A <num> 
```

```
 --after-context <num> 
```

显示&lt; num&gt;尾随行，并在连续的匹配组之间放置一行包含`--`。

```
 -B <num> 
```

```
 --before-context <num> 
```

显示&lt; num&gt;引导线，并在连续的匹配组之间放置包含`--`的行。

```
 -W 
```

```
 --function-context 
```

显示上一行中包含函数名称的周围文本，直到下一个函数名称之前的文本，有效地显示了找到匹配项的整个函数。

```
 --threads <num> 
```

要使用的 grep 工作线程数。有关详细信息，请参见 _CONFIGURATION_ 中的`grep.threads`。

```
 -f <file> 
```

从&lt; file&gt;中读取模式，每行一个。

```
 -e 
```

下一个参数是模式。此选项必须用于以`-`开头的模式，并且应该在将用户输入传递给 grep 的脚本中使用。多个模式由 _ 或 _ 组合。

```
 --and 
```

```
 --or 
```

```
 --not 
```

```
 ( …​ ) 
```

指定如何使用布尔表达式组合多个模式。 `--or`是默认运算符。 `--and`的优先级高于`--or`。 `-e`必须用于所有模式。

```
 --all-match 
```

当给出多个模式表达式与`--or`组合时，指定此标志以限制匹配到具有匹配所有这些行的行的文件。

```
 -q 
```

```
 --quiet 
```

不输出匹配的线;相反，当匹配时退出状态 0，当没有匹配时退出非零状态。

```
 <tree>…​ 
```

不是搜索工作树中的跟踪文件，而是搜索给定树中的 blob。

```
 -- 
```

表示选项的结束;其余参数是&lt; pathspec&gt;限制器。

```
 <pathspec>…​ 
```

如果给定，则将搜索限制为与至少一个模式匹配的路径。两个前导路径匹配，并支持 glob（7）模式。

有关&lt; pathspec&gt;的详细信息语法，请参阅 [gitglossary [7]](https://git-scm.com/docs/gitglossary) 中的 _pathspec_ 条目。

## 例子

```
 git grep 'time_t' -- '*.[ch]' 
```

在工作目录及其子目录中的所有跟踪的.c 和.h 文件中查找`time_t`。

```
 git grep -e '#define' --and \( -e MAX_PATH -e PATH_MAX \) 
```

查找具有`#define`和`MAX_PATH`或`PATH_MAX`的行。

```
 git grep --all-match -e NODE -e Unexpected 
```

在具有与两者匹配的行的文件中查找具有`NODE`或`Unexpected`的行。

```
 git grep solution -- :^Documentation 
```

查找`solution`，不包括`Documentation`中的文件。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件