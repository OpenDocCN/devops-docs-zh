# git-for-each-ref

> 原文： [`git-scm.com/docs/git-for-each-ref`](https://git-scm.com/docs/git-for-each-ref)

## 名称

git-for-each-ref - 每个参考的输出信息

## 概要

```
git for-each-ref [--count=<count>] [--shell|--perl|--python|--tcl]
		   [(--sort=<key>)…​] [--format=<format>] [<pattern>…​]
		   [--points-at=<object>]
		   (--merged[=<object>] | --no-merged[=<object>])
		   [--contains[=<object>]] [--no-contains[=<object>]]
```

## 描述

迭代匹配`&lt;pattern&gt;`的所有引用并根据给定的`&lt;format&gt;`显示它们，然后根据给定的`&lt;key&gt;`集对它们进行排序。如果给出`&lt;count&gt;`，则在显示许多参考后停止。 `&lt;format&gt;`中的插值可以选择引用指定主语言中的字符串文字，允许直接用该语言进行评估。

## OPTIONS

```
 <pattern>…​ 
```

如果给出一个或多个模式，则仅显示与至少一个模式匹配的 refs，使用 fnmatch（3）或字面意思，在后一种情况下完全匹配或从开头到斜线。

```
 --count=<count> 
```

默认情况下，该命令显示与`&lt;pattern&gt;`匹配的所有引用。此选项使其在显示许多引用后停止。

```
 --sort=<key> 
```

要排序的字段名称。前缀`-`按值的降序排序。未指定时，使用`refname`。您可以使用--sort =&lt; key&gt;选项多次，在这种情况下，最后一个键成为主键。

```
 --format=<format> 
```

一个字符串，用于插入显示的 ref 及其指向的对象的`%(fieldname)`。如果`fieldname`以星号（`*`为前缀）并且 ref 指向标记对象，请使用标记对象引用的对象中的字段值（而不是标记对象中的字段）。未指定时，`&lt;format&gt;`默认为`%(objectname) SPC %(objecttype) TAB %(refname)`。它还将`%%`插入到`%`，`%xx`其中`xx`是十六进制数字，插入到十六进制代码`xx`的字符中;例如`%00`内插到`\0`（NUL），`%09`到`\t`（TAB）和`%0a`到`\n`（LF）。

```
 --color[=<when>] 
```

尊重`--format`选项中指定的任何颜色。 `&lt;when&gt;`字段必须是`always`，`never`或`auto`之一（如果`&lt;when&gt;`不存在，则表现得好像`always`一样）。

```
 --shell 
```

```
 --perl 
```

```
 --python 
```

```
 --tcl 
```

如果给定，则替换`%(fieldname)`占位符的字符串将被引用为适合指定宿主语言的字符串文字。这是为了生成一个可以直接“评估”的 scriptlet。

```
 --points-at=<object> 
```

仅列出指向给定对象的引用。

```
 --merged[=<object>] 
```

仅列出可从指定提交（HEAD，如果未指定）可访问的引用的 refs，与`--no-merged`不兼容。

```
 --no-merged[=<object>] 
```

仅列出其指针无法从指定的提交（如果未指定，则为 HEAD）可访问的 refs，与`--merged`不兼容。

```
 --contains[=<object>] 
```

仅列出包含指定提交的引用（如果未指定，则为 HEAD）。

```
 --no-contains[=<object>] 
```

仅列出不包含指定提交的引用（如果未指定，则为 HEAD）。

```
 --ignore-case 
```

排序和过滤 refs 不区分大小写。

## 字段名称

来自引用对象中的结构化字段的各种值可用于插入到结果输出中，或作为排序键。

对于所有对象，可以使用以下名称：

```
 refname 
```

ref 的名称（$ GIT_DIR /之后的部分）。对于 ref 附加`:short`的非模糊短名称。选项 core.warnAmbiguousRefs 用于选择严格的缩写模式。如果附加`lstrip=&lt;N&gt;`（`rstrip=&lt;N&gt;`），从参考号的前面（后面）剥离`&lt;N&gt;`斜线分离的路径分量（例如`%(refname:lstrip=2)`将`refs/tags/foo`变为`foo`并且`%(refname:rstrip=2)`变为`refs/tags/foo`进入`refs`）。如果`&lt;N&gt;`为负数，则从指定端剥离尽可能多的路径组件以留下`-&lt;N&gt;`路径组件（例如`%(refname:lstrip=-2)`将`refs/tags/foo`变为`tags/foo`而`%(refname:rstrip=-1)`将`refs/tags/foo`变为`refs/tags/foo` COD17]）。当 ref 没有足够的组件时，如果使用正&lt; N&gt;进行剥离，则结果将变为空字符串，或者如果使用负&lt; N&gt;进行剥离，则结果将变为完整的 refname。两者都不是错误。

`strip`可以用作`lstrip`的同义词。

```
 objecttype 
```

对象的类型（`blob`，`tree`，`commit`，`tag`）。

```
 objectsize 
```

对象的大小（与 _git cat-file -s_ 报告相同）。附加`:disk`以获取对象占用磁盘的大小（以字节为单位）。请参阅下面`CAVEATS`部分中有关磁盘大小的说明。

```
 objectname 
```

对象名称（又名 SHA-1）。对于对象名称的非模糊缩写，附加`:short`。对于具有所需长度的对象名称的缩写，附加`:short=&lt;length&gt;`，其中最小长度为 MINIMUM_ABBREV。可能会超出长度以确保唯一的对象名称。

```
 deltabase 
```

如果将其存储为 delta，则会扩展为给定对象的 delta base 的对象名称。否则它会扩展为空对象名称（全为零）。

```
 upstream 
```

本地引用的名称，可以被视为显示引用的“上游”。以与上述`refname`相同的方式尊重`:short`，`:lstrip`和`:rstrip`。另外尊重`:track`以显示“[前 N，后 M]”和`:trackshort`以显示简洁版本：“&gt;” （提前），“&lt;” （后面），“&lt;&gt;” （前后）或“=”（同步）。每当遇到未知的上游引用时，`:track`也会打印“[gone]”。附加`:track,nobracket`以显示没有括号的跟踪信息（即“在 N 之前，在 M 之后”）。

对于任何远程跟踪分支`%(upstream)`，`%(upstream:remotename)`和`%(upstream:remoteref)`分别指代远程名称和被跟踪远程 ref 的名称。换句话说，远程跟踪分支可以通过使用 refspec `%(upstream:remoteref):%(upstream)`从`%(upstream:remotename)`获取来显式和单独更新。

如果 ref 没有与之关联的跟踪信息，则无效。除`nobracket`之外的所有选项都是互斥的，但如果一起使用，则选择最后一个选项。

```
 push 
```

本地引用的名称，表示显示的引用的`@{push}`位置。与`upstream`一样，`:short`，`:lstrip`，`:rstrip`，`:track`，`:trackshort`，`:remotename`和`:remoteref`选项。如果未配置`@{push}` ref，则生成空字符串。

```
 HEAD 
```

_*_ 如果 HEAD 匹配当前 ref（检出的分支），否则。

```
 color 
```

更改输出颜色。后跟`:&lt;colorname&gt;`，其中颜色名称在 [git-config [1]](https://git-scm.com/docs/git-config) 的“CONFIGURATION FILE”部分的值下描述。例如，`%(color:bold red)`。

```
 align 
```

左，中，或右对齐％（对齐：...）和％（结束）之间的内容。 “对齐：”之后是以逗号分隔的任何顺序的`width=&lt;width&gt;`和`position=&lt;position&gt;`，其中`&lt;position&gt;`是左，右或中间，默认为左，`&lt;width&gt;`是内容的总长度对齐。为简洁起见，可以省略“width =”和/或“position =”前缀，并且&lt; width&gt;和&lt; position&gt;用来代替。例如，`%(align:&lt;width&gt;,&lt;position&gt;)`。如果内容长度大于宽度，则不执行对齐。如果与`--quote`一起使用，则引用％（align：...）和％（end）之间的所有内容，但如果嵌套，则只有最顶层执行引用。

```
 if 
```

用作％（if）...％（然后）...％（结束）或％（如果）...％（然后）...％（否则）...％（结束）。如果在％（if）之后有一个带有值或字符串文字的原子，则打印％（then）之后的所有内容，否则如果使用％（else）原子，则打印％（else）之后的所有内容。我们在％（然后）之前评估字符串时忽略空格，当我们使用打印“*”或“”的％（HEAD）原子并且我们想要仅在条件下应用 _ 时这很有用 _HEAD_ 参考。附加“：equals =&lt; string&gt;”或“：notequals =&lt; string&gt;”比较％（if：...）和％（then）原子与给定字符串之间的值。_

```
 symref 
```

给定符号 ref 引用的引用。如果不是符号引用，则不打印任何内容。以与`refname`相同的方式尊重`:short`，`:lstrip`和`:rstrip`选项。

除上述内容外，对于提交和标记对象，标题字段名称（`tree`，`parent`，`object`，`type`和`tag`）可用于指定标题字段中的值。

对于提交和标记对象，特殊的`creatordate`和`creator`字段将对应于`committer`或`tagger`字段中的相应日期或名称 - 电子邮件 - 日期元组，具体取决于对象类型。这些用于处理带注释和轻量级标签的混合。

将 name-email-date 元组作为其值（`author`，`committer`和`tagger`）的字段可以后缀`name`，`email`和`date`以提取指定的组件。

提交和标记对象中的完整消息是`contents`。它的第一行是`contents:subject`，其中 subject 是提交消息的所有行的连接，直到第一个空行。下一行是`contents:body`，其中 body 是第一个空白行之后的所有行。可选的 GPG 签名是`contents:signature`。使用`contents:lines=N`获得消息的第一`N`行。另外，由 [git-interpre-trailers [1]](https://git-scm.com/docs/git-interpret-trailers) 解释的预告片作为`trailers`（或通过使用历史别名`contents:trailers`）获得。使用`trailers:only`可以省略拖车块中的非拖车线。可以从预告片中删除空格连续，以便每个预告片单独出现在一行上，其全部内容为`trailers:unfold`。两者可以一起用作`trailers:unfold,only`。

出于排序目的，具有数值的字段按数字顺序排序（`objectsize`，`authordate`，`committerdate`，`creatordate`，`taggerdate`）。所有其他字段用于按字节值顺序排序。

还有一个按版本排序的选项，这可以通过使用 fieldname `version:refname`或其别名`v:refname`来完成。

在任何情况下，引用不适用于 ref 引用的对象的字段的字段名称不会导致错误。它返回一个空字符串。

作为日期类型字段的特殊情况，您可以通过添加`:`后跟日期格式名称来指定日期的格式（请参阅 [git-rev-list [1]的`--date`选项的值](https://git-scm.com/docs/git-rev-list)需要）。

像％（对齐）和％（如果）这样的原子总是需要匹配的％（结束）。我们称它们为“开放原子”，有时将它们表示为％（$ open）。

当特定于脚本语言的引用生效时，顶级开放原子与其匹配的％（结束）之间的所有内容都根据开放原子的语义进行评估，并且仅引用顶级开放原子的结果。

## 例子

直接生成格式化文本的示例。显示最近 3 个标记的提交：

```
#!/bin/sh

git for-each-ref --count=3 --sort='-*authordate' \
--format='From: %(*authorname) %(*authoremail)
Subject: %(*subject)
Date: %(*authordate)
Ref: %(*refname)

%(*body)
' 'refs/tags'
```

一个简单的例子，展示了在输出中使用 shell eval，演示了如何使用--shell。列出所有头的前缀：

```
#!/bin/sh

git for-each-ref --shell --format="ref=%(refname)" refs/heads | \
while read entry
do
	eval "$entry"
	echo `dirname $ref`
done
```

关于标签的更详细的报告，证明格式可能是整个脚本：

```
#!/bin/sh

fmt='
	r=%(refname)
	t=%(*objecttype)
	T=${r#refs/tags/}

	o=%(*objectname)
	n=%(*authorname)
	e=%(*authoremail)
	s=%(*subject)
	d=%(*authordate)
	b=%(*body)

	kind=Tag
	if test "z$t" = z
	then
		# could be a lightweight tag
		t=%(objecttype)
		kind="Lightweight tag"
		o=%(objectname)
		n=%(authorname)
		e=%(authoremail)
		s=%(subject)
		d=%(authordate)
		b=%(body)
	fi
	echo "$kind $T points at a $t object $o"
	if test "z$t" = zcommit
	then
		echo "The commit was authored by $n $e
at $d, and titled

    $s

Its message reads as:
"
		echo "$b" | sed -e "s/^/    /"
		echo
	fi
'

eval=`git for-each-ref --shell --format="$fmt" \
	--sort='*objecttype' \
	--sort=-taggerdate \
	refs/tags`
eval "$eval"
```

显示％（if）...％（然后）...％（else）...％（end）的用法的示例。这为当前分支添加星号前缀。

```
git for-each-ref --format="%(if)%(HEAD)%(then)* %(else)  %(end)%(refname:short)" refs/heads/
```

显示％（if）...％（然后）...％（结束）的用法的示例。这将打印 authorname（如果存在）。

```
git for-each-ref --format="%(refname)%(if)%(authorname)%(then) Authored by: %(authorname)%(end)"
```

## CAVEATS

请注意，磁盘上对象的大小是准确报告的，但应该注意得出哪些引用或对象负责磁盘使用的结论。打包的非 delta 对象的大小可能远大于对其增量的对象的大小，但是选择哪个对象是基础并且 delta 是任意的并且在重新打包期间可能会发生变化。

还要注意，对象的多个副本可能存在于对象数据库中;在这种情况下，未定义将报告哪个副本的大小或增量基数。

## 也可以看看

[git-show-ref [1]](https://git-scm.com/docs/git-show-ref)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件