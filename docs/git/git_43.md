# gitignore

> 原文： [`git-scm.com/docs/gitignore`](https://git-scm.com/docs/gitignore)

## 名称

gitignore - 指定要忽略的故意未跟踪文件

## 概要

$ XDG_CONFIG_HOME / git / ignore，$ GIT_DIR / info / exclude，.gitignore

## 描述

`gitignore`文件指定 Git 应忽略的故意未跟踪文件。 Git 已经跟踪的文件不受影响;有关详细信息，请参阅下面的注释。

`gitignore`文件中的每一行都指定一个模式。在决定是否忽略路径时，Git 通常会检查来自多个源的`gitignore`模式，具有以下优先顺序，从最高到最低（在一个优先级内，最后一个匹配模式决定结果）：

*   从命令行读取的模式用于支持它们的那些命令。

*   从与路径相同的目录中的`.gitignore`文件读取的模式，或在任何父目录中读取的模式，其中较高级别文件中的模式（直到工作树的顶层）被较低级别文件中的模式覆盖到包含该文件的目录。这些模式相对于`.gitignore`文件的位置匹配。项目通常在其存储库中包含此类`.gitignore`文件，其中包含作为项目构建的一部分生成的文件的模式。

*   从`$GIT_DIR/info/exclude`读取的模式。

*   模式从配置变量`core.excludesFile`指定的文件中读取。

放置模式的文件取决于模式的使用方式。

*   应该通过克隆进行版本控制并分发到其他存储库的模式（即所有开发人员都希望忽略的文件）应该进入`.gitignore`文件。

*   特定于特定存储库但不需要与其他相关存储库共享的模式（例如，存储在存储库中但特定于一个用户工作流的辅助文件）应该进入`$GIT_DIR/info/exclude`文件。

*   用户希望 Git 在所有情况下忽略的模式（例如，由用户选择的编辑器生成的备份或临时文件）通常会进入用户`~/.gitconfig`中`core.excludesFile`指定的文件。其默认值为$ XDG_CONFIG_HOME / git / ignore。如果$ XDG_CONFIG_HOME 未设置或为空，则使用$ HOME / .config / git / ignore。

底层的 Git 管道工具，如 _git ls-files_ 和 _git read-tree_ ，读取命令行选项指定的`gitignore`模式，或者命令行指定的文件选项。更高级别的 Git 工具，例如 _git status_ 和 _git add_ ，使用上面指定的来源的模式。

## 模式格式

*   空行不匹配任何文件，因此它可以作为可读性的分隔符。

*   以＃开头的行作为注释。对于以哈希开头的模式，在第一个哈希值前加一个反斜杠（“`\`”）。

*   除非用反斜杠（“`\`”）引用尾随空格，否则将忽略尾随空格。

*   可选的前缀“`!`”否定模式;之前模式排除的任何匹配文件将再次包含在内。如果排除该文件的父目录，则无法重新包含文件。出于性能原因，Git 不会列出排除的目录，因此无论在何处定义，所包含文件的任何模式都不起作用。对于以文字“`!`”开头的模式，在第一个“`!`”前放置一个反斜杠（“`\`”），例如“`\!important!.txt`”。

*   如果模式以斜杠结尾，则为了以下描述的目的将其删除，但它只会找到与目录的匹配项。换句话说，`foo/`将匹配目录`foo`和它下面的路径，但不匹配常规文件或符号链接`foo`（这与在 Git 中一般使用 pathspec 的方式一致）。

*   如果模式不包含斜杠 _/_ ，Git 会将其视为 shell glob 模式，并检查相对于`.gitignore`文件位置的路径名匹配（相对于工作的顶层）树，如果不是来自`.gitignore`文件）。

*   否则，Git 将模式视为 shell glob：“`*`”匹配除“`/`”之外的任何内容，“`?`”匹配除“`/`”之外的任何一个字符，并且“`[]`”匹配一个字符选定的范围。有关更详细的说明，请参阅 fnmatch（3）和 FNM_PATHNAME 标志。

*   前导斜杠与路径名的开头匹配。例如，“/ *。c”匹配“cat-file.c”但不匹配“mozilla-sha1 / sha1.c”。

与完整路径名匹配的两个连续星号（“`**`”）可能具有特殊含义：

*   前导“`**`”后跟斜杠表示在所有目录中匹配。例如，“`**/foo`”在任何地方匹配文件或目录“`foo`”，与模式“`foo`”相同。 “`**/foo/bar`”将文件或目录“`bar`”匹配在“`foo`”目录下的任何位置。

*   尾随“`/**`”匹配内部的所有内容。例如，“`abc/**`”匹配目录“`abc`”内的所有文件，相对于`.gitignore`文件的位置，具有无限深度。

*   斜杠后跟两个连续的星号，然后斜杠匹配零个或多个目录。例如，“`a/**/b`”匹配“`a/b`”，“`a/x/b`”，“`a/x/y/b`”等。

*   其他连续的星号被认为是常规星号，并且将根据先前的规则匹配。

## 笔记

gitignore 文件的目的是确保 Git 未跟踪的某些文件保持未跟踪。

要停止跟踪当前跟踪的文件，请使用 _git rm --cached_ 。

## 例子

```
    $ git status
    [...]
    # Untracked files:
    [...]
    #       Documentation/foo.html
    #       Documentation/gitignore.html
    #       file.o
    #       lib.a
    #       src/internal.o
    [...]
    $ cat .git/info/exclude
    # ignore objects and archives, anywhere in the tree.
    *.[oa]
    $ cat Documentation/.gitignore
    # ignore generated html files,
    *.html
    # except foo.html which is maintained by hand
    !foo.html
    $ git status
    [...]
    # Untracked files:
    [...]
    #       Documentation/foo.html
    [...]
```

另一个例子：

```
    $ cat .gitignore
    vmlinux*
    $ ls arch/foo/kernel/vm*
    arch/foo/kernel/vmlinux.lds.S
    $ echo '!/vmlinux*' >arch/foo/kernel/.gitignore
```

第二个.gitignore 阻止 Git 忽略`arch/foo/kernel/vmlinux.lds.S`。

排除除特定目录`foo/bar`以外的所有内容的示例（注意`/*` - 没有斜杠，通配符也会排除`foo/bar`中的所有内容）：

```
    $ cat .gitignore
    # exclude everything except directory foo/bar
    /*
    !/foo
    /foo/*
    !/foo/bar
```

## 也可以看看

[git-rm [1]](https://git-scm.com/docs/git-rm) ， [gitrepository-layout [5]](https://git-scm.com/docs/gitrepository-layout) ， [git-check-ignore [1]](https://git-scm.com/docs/git-check-ignore)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件