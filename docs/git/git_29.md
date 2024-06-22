# git-shortlog

> 原文： [`git-scm.com/docs/git-shortlog`](https://git-scm.com/docs/git-shortlog)

## 名称

git-shortlog - 汇总 _git log_ 输出

## 概要

```
git shortlog [<options>] [<revision range>] [[--] <path>…​]
git log --pretty=short | git shortlog [<options>]
```

## 描述

以适合包含在发布公告中的格式汇总 _git log_ 输出。每个提交将按作者和标题分组。

此外，“[PATCH]”将从提交说明中删除。

如果命令行上没有传递任何修订，并且标准输入不是终端或者没有当前分支， _git shortlog_ 将输出从标准输入读取的日志摘要，而不引用当前存储库。

## OPTIONS

```
 -n 
```

```
 --numbered 
```

根据每位作者的提交次数而不是作者字母顺序对输出进行排序。

```
 -s 
```

```
 --summary 
```

禁止提交描述并仅提供提交计数摘要。

```
 -e 
```

```
 --email 
```

显示每位作者的电子邮件地址。

```
 --format[=<format>] 
```

而不是提交主题，使用一些其他信息来描述每个提交。 _&lt; format&gt;_ 可以是 _git log_ 的`--format`选项接受的任何字符串，例如 _* [％h]％s_ 。 （参见 [git-log [1]](https://git-scm.com/docs/git-log) 的“PRETTY FORMATS”部分。）

```
Each pretty-printed commit will be rewrapped before it is shown.
```

```
 -c 
```

```
 --committer 
```

收集并显示提交者身份而不是作者。

```
 -w[<width>[,<indent1>[,<indent2>]]] 
```

通过在`width`处包裹每一行来包装输出。每个条目的第一行由`indent1`空格缩进，第二行和后续行由`indent2`空格缩进。 `width`，`indent1`和`indent2`分别默认为 76,6 和 9。

如果 width 是`0`（零），则缩进输出行而不包装它们。

```
 <revision range> 
```

仅显示指定修订范围内的提交。当没有&lt;修订范围&gt;如果指定，则默认为`HEAD`（即导致当前提交的整个历史记录）。 `origin..HEAD`指定从当前提交可以访问的所有提交（即`HEAD`），但不是`origin`。有关拼写&lt; revision range&gt;的完整列表，请参阅 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 的“指定范围”部分。

```
 [--] <path>…​ 
```

只考虑足以解释与指定路径匹配的文件的提交。

当出现混淆时，路径可能需要以`--`作为前缀，以将它们与选项或修订范围分开。

## 映射作者

`.mailmap`功能用于将短名中的同一个人合并到一起，其中他们的姓名和/或电子邮件地址拼写不同。

如果文件`.mailmap`存在于存储库的顶层，或者位于 mailmap.file 或 mailmap.blob 配置选项所指向的位置，则它用于将作者和提交者名称以及电子邮件地址映射到规范的真实姓名和电子邮件地址。

在简单形式中，文件中的每一行都包含作者的规范实名，空格和提交中使用的电子邮件地址（由 _&lt;_ 和 _&gt;_ 括起来）映射到名称。例如：

```
Proper Name <commit@email.xx>
```

更复杂的形式是：

```
<proper@email.xx> <commit@email.xx>
```

允许 mailmap 仅替换提交的电子邮件部分，并且：

```
Proper Name <proper@email.xx> <commit@email.xx>
```

它允许 mailmap 替换与指定的提交电子邮件地址匹配的提交的名称和电子邮件，并且：

```
Proper Name <proper@email.xx> Commit Name <commit@email.xx>
```

它允许 mailmap 替换与指定的提交名称和电子邮件地址匹配的提交的名称和电子邮件。

示例 1：您的历史记录包含两位作者 Jane 和 Joe 的提交，其名称以多种形式出现在存储库中：

```
Joe Developer <joe@example.com>
Joe R. Developer <joe@example.com>
Jane Doe <jane@example.com>
Jane Doe <jane@laptop.(none)>
Jane D. <jane@desktop.(none)>
```

现在假设 Joe 希望他的中间名最初使用，而 Jane 更喜欢她的姓氏完全拼写出来。一个合适的`.mailmap`文件看起来像：

```
Jane Doe         <jane@desktop.(none)>
Joe R. Developer <joe@example.com>
```

注意如何不需要`&lt;jane@laptop.(none)&gt;`的条目，因为该作者的真实姓名已经正确。

示例 2：您的存储库包含以下作者的提交：

```
nick1 <bugs@company.xx>
nick2 <bugs@company.xx>
nick2 <nick2@company.xx>
santa <me@company.xx>
claus <me@company.xx>
CTO <cto@coompany.xx>
```

然后你可能想要一个看起来像这样的`.mailmap`文件：

```
<cto@company.xx>                       <cto@coompany.xx>
Some Dude <some@dude.xx>         nick1 <bugs@company.xx>
Other Author <other@author.xx>   nick2 <bugs@company.xx>
Other Author <other@author.xx>         <nick2@company.xx>
Santa Claus <santa.claus@northpole.xx> <me@company.xx>
```

将哈希 _＃_ 用于自己的行或电子邮件地址之后的注释。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件