# git-config

> 原文： [`git-scm.com/docs/git-config`](https://git-scm.com/docs/git-config)
> 
> 贡献者：[honglyua](https://github.com/honglyua)

## 名称

git-config - 获取并设置存储库或全局选项

## 概要

```
git config [<file-option>] [--type=<type>] [--show-origin] [-z|--null] name [value [value_regex]]
git config [<file-option>] [--type=<type>] --add name value
git config [<file-option>] [--type=<type>] --replace-all name value [value_regex]
git config [<file-option>] [--type=<type>] [--show-origin] [-z|--null] --get name [value_regex]
git config [<file-option>] [--type=<type>] [--show-origin] [-z|--null] --get-all name [value_regex]
git config [<file-option>] [--type=<type>] [--show-origin] [-z|--null] [--name-only] --get-regexp name_regex [value_regex]
git config [<file-option>] [--type=<type>] [-z|--null] --get-urlmatch name URL
git config [<file-option>] --unset name [value_regex]
git config [<file-option>] --unset-all name [value_regex]
git config [<file-option>] --rename-section old_name new_name
git config [<file-option>] --remove-section name
git config [<file-option>] [--show-origin] [-z|--null] [--name-only] -l | --list
git config [<file-option>] --get-color name [default]
git config [<file-option>] --get-colorbool name [stdout-is-tty]
git config [<file-option>] -e | --edit
```

## 描述

您可以使用此命令查询/设置/替换/取消设置选项。该名称实际上是由一个点分隔的配置项名和键名组成，即`name 相当于<section>.<key>`，执行`git config <section>.<key>`后，该配置项设置的值将被输出。

可以使用`--add`为某个配置项添加多个值。如果要更新或取消某个配置项，而这个配置项存在多个值时，你需要给出值后缀的正则表达式`value_regex`。它只更新或取消与正则表达式匹配的设置值。如果你想处理那些与正则表达式不匹配的设置行，只需在前面添加一个感叹号（另见示例）。

`--type=<type>`选项是为了保证以给定的 type 去规范化输入和输出 _git config_ 中配置项的值。如果没有给出`--type=<type>`，则不执行规范化。调用者可以使用`--no-type`取消设置里已有的`--type`说明符。

读取时，默认情况下会从系统，全局和存储库本地配置文件中读取值，并且可以使用选项`--system`，`--global`，`--local`，`--worktree`和`--file <filename>`来告诉命令读取哪里的配置文件（参见 FILES ）。

写入时，默认情况下会将新值写入存储库本地配置文件，并且可以使用选项`--system`，`--global`，`--worktree`，`--file <filename>`来告诉命令写入该位置（可以使用`--local`但这是默认选项）。

出错时，此命令将失败，并以非零状态码退出。一些退出状态码有：

*   部分或键无效（ret = 1），

*   没有提供配置选项或名称（ret = 2），

*   配置文件无效（ret = 3），

*   配置文件无法写入（ret = 4），

*   你试图取消一个不存在的选项（ret = 5），

*   您在尝试取消设置/设置配置选项时，该配置项拥有多行配置（ret = 5），或

*   您在尝试使用无效的正则表达式（ret = 6）。

成功时，该命令返回退出状态码 0。

## 选项

```
 --replace-all 
```

默认行为是最多替换一行。它将会替换与键匹配的所有行（以及有可选的 value_regex）。

```
 --add 
```

在不更改任何现有值的情况下向选项添加新行。这与在`--replace-all`中以 _^ $_ 作为 value_regex 的值相同。

```
 --get 
```

获取给定键的值（可选择通过与值匹配的正则表达式进行过滤）。如果未找到对应键值，则返回错误状态码 1；如果找到多个键值对，则返回最后一个值。

```
 --get-all 
```

与 get 类似，但返回所有键值对的值。
```
 --get-regexp 
```

与--get-all 类似，输出所有与正则表达式匹配的键值对，并输出键名和对应值。正则表达式匹配目前区分大小写，并且针对键的规范化版本完成，其中段和变量名称是小写的，但子段名称不是。

```
 --get-urlmatch name URL 
```

如果给出了一个由两部分组成的配置项名称 section.key，则返回与<url>最匹配的值（如果不存在此配置名，则 section.key 的值用作回退）。如果仅将输入 section，则返回所有与 section 匹配的键值对。如果未找到值，则返回错误代码 1。

```
 --global 
```

对于写入选项：写入全局配置文件`~/.gitconfig`，而不是存储库配置文件`.git/config`。如果全局配置文件不存在而`$XDG_CONFIG_HOME/git/config`文件存在，则写入`$XDG_CONFIG_HOME/git/config`文件。

对于读取选项：只读取`~/.gitconfig`和`$XDG_CONFIG_HOME/git/config`文件中的配置，而不是所有可用文件中的。

另见 FILES 。

```
 --system 
```

对于写入选项：写入系统路径下的配置文件`$(prefix)/etc/gitconfig`，而不是存储库配置文件`.git/config`。

对于读取选项：只读取系统路径下的配置文件`$(prefix)/etc/gitconfig`中的配置，而不是所有可用文件中的。

另见 FILES 。

```
 --local 
```

对于写入选项：写入存储库配置文件`.git/config`中。这是默认行为。

对于读取选项：只读取存储库配置文件`.git/config`中的配置，而不是所有可用文件中的。

另见 FILES 。

```
 --worktree 
```

与`--local`类似，但如果`extensions.worktreeConfig`存在，则读取或写入`.git/config.worktree`中。如果不存在，它与`--local`效果相同。

```
 -f config-file 
```

```
 --file config-file 
```

使用给定的配置文件，而不是 GIT_CONFIG 指定的配置文件。

```
 --blob blob 
```

与`--file`类似，但给定是 blob 路径而不是文件路径。例如。您可以使用 _master:.gitmodules_，从 master 分支中的文件`.gitmodules`中读取配置值。有关拼写 blob 名称的更完整列表，请参阅 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 中的“指定修订”部分。

```
 --remove-section 
```

从配置文件中删除给定的配置项。

```
 --rename-section 
```

将给定 section 重命名为新名称。

```
 --unset 
```

从配置文件中删除与键匹配的行。

```
 --unset-all 
```

从配置文件中删除与键匹配的所有行。

```
 -l
 ```

 ```
 --list 
```

列出配置文件中设置的所有配置项及其值。

```
 --type <type> 
```

_git config_ 在给定的类型约束下，将确保任何输入或输出有效，并将以`<type>`的规范形式规范化输出值。

有效`<type>`包括：

*   _bool_ ：将值规范化为“true”或“false”。

*   _int_ ：将值规范化为简单的十进制数。 _k_，_m_ 或 _g_ 的可选后缀将使该值在输入时乘以 1024，1048576 或 1073741824。

*   _bool-or-int_ ：根据 _bool_ 或 _int_ 进行标准化，如上所述。

*   _path_ ：通过以规范化的形式，在值前添加前缀`~`来指向用户的根目录`$HOME`和`~user`。设置值时，此说明符无效（但您可以使用命令行中的`git config section.variable ~/`让 shell 执行扩展。）

*   _expiry-date_：通过以规范化的形式，将固定或相对固定的日期字符串转换为时间戳。设置值时，此说明符无效。

*   _color_ ：获取值时，通过转换为 ANSI 颜色转义序列进行规范化。设置值时，将执行完整性检查以确保给定值可以作为 ANSI 颜色进行规范化，但它按原样编写。

```
 --bool
```

```
 --int 
```

```
 --bool-or-int 
 ```

```
--path 
```

```
 --expiry-date 
```

选择类型说明符的历史选项。而不是`--type`（见上文）。

```
 --no-type 
```

取消设置先前设置的类型说明符（如果先前已设置）。此选项请求 _git config_ 不规范化检索到的变量。没有`--type=<type>`或`--<type>`，`--no-type`也不会受影响。

```
 -z 
```

```
 --null 
```

对于输出值或键名时，始终使用空字符（而不是换行符）作为结束字符串。使用换行符作为键和值之间的分隔符。这允许准确地解析输出而不会混淆，例如包含换行符的值。

```
 --name-only 
```

仅输出`--list`或`--get-regexp`的配置变量名称。

```
 --show-origin 
```

增加输出内容：origin 的类型（文件，标准输入，blob，命令行）和 origin 的指向（配置文件路径，ref 或 blob id，如果适用）

```
 --get-colorbool name [stdout-is-tty] 
```

找到`name`的颜色设置（例如`color.diff`）并输出“true”或“false”。 `stdout-is-tty`可以为“true”或“false”，并在配置显示为“auto”时予以考虑。如果缺少`stdout-is-tty`，则检查命令本身的标准输出，如果要使用颜色则退出状态 0，否则退出状态 1。当`name`的颜色未设置时，该命令使用`color.ui`作为后备。

```
 --get-color name [default] 
```

找到为`name`配置的颜色（例如`color.diff.new`）并将其作为 ANSI 颜色转义序列输出到标准输出。如果没有`name`未配置颜色，则会使用可选的`default`参数。

`--type=color [--default=<default>]`优于`--get-color`。

```
 -e 
```

```
 --edit 
```

打开编辑器，修改指定的配置文件; `--system`，`--global`或存储库（默认）。

```
 --[no-]includes 
```

在配置文件中查找值时，会考虑使用`include.*`指令。如果给出特定文件（例如，使用`--file`，`--global`等），则功能默认是`off`。如果在搜索所有配置文件时，功能是`on`

```
 --default <value> 
```

使用`--get`时，找不到请求的变量时，其行为就像<value>是赋给该变量的值。

## 配置

当使用`--list`或可能有多个返回结果的`--get-*`时，会遵守`pager.config`列出配置。默认是使用分页的。

## 文件

如果未使用`--file`选项，则`git config`会从以下四个配置文件中，搜索配置选项：

```
 $(prefix)/etc/gitconfig 
```

系统范围的配置文件。

```
 $XDG_CONFIG_HOME/git/config 
```

特定用户的第二个配置文件。如果`$XDG_CONFIG_HOME`未设置或为空，将使用`$HOME/.config/git/config`。此文件中设置的任何单值变量都将被`~/.gitconfig`中的任何内容覆盖。如果您有时使用旧版本的 Git，最好不要创建此文件，因为最近添加了对此文件的支持。

```
 ~/.gitconfig 
```

特定用户的配置文件。也称为“全局”配置文件。

```
 $GIT_DIR/config 
```

特定存储库的配置文件。

```
 $GIT_DIR/config.worktree 
```

这是可选的，仅在`$GIT_DIR/config`中存在`extensions.worktreeConfig`时才会被搜索到。

如果没有给出进一步的选项，在读取所有配置选项时，将从以上所有可用的文件中读取。如果全局或系统范围的配置文件不可用，则它们将会被忽略。如果存储库配置文件不可用或不可读，`git config`将以非零错误状态退出。但是，在任何情况下都不会发出错误消息。

按上面给出的顺序读取配置文件中的配置，新读到的配置值将会覆盖之前读到的。当获取多个值时，将使用来自所有文件的键的所有值。

使用`-c`选项运行任何 git 命令时，可以覆盖单个配置参数。有关详细信息，请参阅 [git[1]](https://git-scm.com/docs/git)。

所有配置选项写入时，都将默认写入到当前存储库的配置文件中。请注意，这也会影响`--replace-all`和`--unset`等选项。 **_git config_ 一次只能更改一个配置文件中配置**。

您可以通过命令行选项或环境变量覆盖这些规则。 `--global`，`--system`和`--worktree`选项将分别限制用于全局，系统范围和每个工作树的配置文件。 `GIT_CONFIG`环境变量具有类似的效果，但您可以指定所需的任何文件名。

## 环境变量

```
 GIT_CONFIG 
```

从给定文件而不是.git/config 中获取配置。使用“--global”选项将强制使用~/.gitconfig。使用“--system”选项强制使用$(前缀)/etc/gitconfig。

```
 GIT_CONFIG_NOSYSTEM 
```

是否跳过从系统配置文件$(前缀)/etc/gitconfig 中读取设置。有关详细信息，请参阅[git[1]](https://git-scm.com/docs/git)。

另见 FILES。

## 例子

给出像这样的.git / config：

```
#
# 给出像这样的, 
# '#' 或者 ';' 是表示注释
#
```

```
; core variables
[core]
	; Don't trust file modes
	filemode = false
```

```
; Our diff algorithm
[diff]
	external = /usr/local/bin/diff-wrapper
	renames = true
```

```
; Proxy settings
[core]
	gitproxy=proxy-command for kernel.org
	gitproxy=default-proxy ; for all the rest
```

```
; HTTP
[http]
	sslVerify
[http "https://weak.example.com"]
	sslVerify = false
	cookieFile = /tmp/cookie.txt
```

你可以将 filemode 设置为 true

```
% git config core.filemode true
```

假设的代理命令条目实际上有一个后缀来识别它们适用的 URL。以下是如何将 kernel.org 的条目更改为“ssh”。

```
% git config core.gitproxy '"ssh" for kernel.org' 'for kernel.org$'
```

这确保仅替换 kernel.org 的键/值对。

要删除 diff 的重命名配置，请执行

```
% git config --unset diff.renames
```

如果要删除多个配置值中的其中一个配置（如上面的 core.gitproxy），则必须提供与该配置值匹配的正则表达式。

要查询给定键的值，请执行

```
% git config --get core.filemode
```

要么

```
% git config core.filemode
```

或者，查询多配置值中的某个配置值：

```
% git config --get core.gitproxy "for kernel.org$"
```

如果您想知道多配置值的所有值，请执行以下操作：

```
% git config --get-all core.gitproxy
```

如果你想冒点险，你可以用新的值取代所有的 core.gitproxy

```
% git config --replace-all core.gitproxy ssh
```

但是，如果您真的只想替换默认代理的行配置，即没有“for ...”后缀的行，请执行以下操作：

```
% git config core.gitproxy ssh '! for '
```

要实际只匹配带有感叹号的值，您必须这样做

```
% git config section.key value '[!]'
```

要添加新代理，而不更改任何现有代理，请使用

```
% git config --add core.gitproxy '"proxy-command" for example.com'
```

在脚本中使用配置中的自定义颜色的示例：

```
#!/bin/sh
WS=$(git config --get-color color.diff.whitespace "blue reverse")
RESET=$(git config --get-color "" "reset")
echo "${WS}your whitespace color or blue reverse${RESET}"
```

对于`https://weak.example.com`的 URL，`http.sslVerify`设置为 false，而其他所有 URL 设置为`true`：

```
% git config --type=bool --get-urlmatch http.sslverify https://good.example.com
true
% git config --type=bool --get-urlmatch http.sslverify https://weak.example.com
false
% git config --get-urlmatch http https://weak.example.com
http.cookieFile /tmp/cookie.txt
http.sslverify false
```

## 配置文件

Git 配置文件包含许多影响 Git 命令行为的变量。每个存储库中的文件`.git/config`和可选`config.worktree`（参见下面的`extensions.worktreeConfig`）用于存储该存储库的配置，`$HOME/.gitconfig`用于存储每用户配置，会回退`.git/config`文件中的配置。文件`/etc/gitconfig`可用于存储系统范围的默认配置。

配置变量由 Git 管道和瓷器使用。变量拆分为 sections，其中变量名称是最后一个点分隔的后面段，而 section 名称是最后一个点之前的所有内容。变量名称不区分大小写，仅允许使用字母数字字符和`-`，并且必须以字母字符开头。一些变量可能会出现多次；我们说那个变量是多值的。

### 句法

语法相当灵活和宽容；空白大多被忽略了。_＃_ 号和 _;_ 分号表示该字符开始到行尾都是注释，空行被忽略。

配置文件由 sections 和变量组成。一个 section 以方括号中的 section 名称开头，并一直持续到下一 section 开始。section 名称不区分大小写。只允许使用字母数字字符、`-`和`.`。每个变量必须属于某个 section，这意味着在第一次设置变量之前必须有一个 section。

section 可以进一步划分为小的 section。要开始小的 section，请将其名称放在双引号中，并用空格与 section 隔开，如下例所示：

```
	[section "subsection"]
```

子节名称区分大小写，可以包含除换行符和空字节之外的任何字符。可以通过`\"`和`\\`转义，包含双引号和反斜杠。读取时会删除其他字符前面的反斜杠；例如，`\t`读取为`t`，`\0`读取为`0`，section 不能跨越多行。变量可以直接属于某个 section 或给定的子 section。你可以有`[section]`，但是可以不必有`[section "subsection"]`。

还有一个不推荐使用的`[section.subsection]`语法。使用此语法，子 section 名称将转换为小写，并且还区分大小写。这些子 section 名称遵循与 section 名称相同的限制。

所有其他行（以及节标题后面的行的其余部分）被识别为设置变量，形式为 _name = value_（或者只是`name`，通过一个短划线用来说明变量是布尔值“true”）。变量名称不区分大小写，仅允许使用字母、数字字符和`-`，并且必须以字母开头。

定义值的行可以通过以`\`结束来继续到下一行；反引号和行尾被剥离。_name =_ 之后的空格，第一个注释字符`＃`或`;`之后的行的剩余部分，和该行尾部的空格都会被被丢弃，除非它们用双引号括起来。值中的内部空格逐字保留。

在双引号内，使用双引号`"`和反斜杠`\`字符必须转义：对`"`使用`\"`，而对`\`使用`\\`。

识别以下转义序列（在`\"`和`\\`旁边）：换行符（NL）的`\n`，水平制表的`\t`（HT，TAB）和退格（BS）的`\b`。其他 char 转义序列（包括八进制转义序列）无效。

### 包括

`include`和`includeIf`部分允许您包含来自其他来源的配置指令。这些 section 的行为相同，但`includeIf`的 section 如果其条件未评估为真则可以忽略；请参阅下面的“条件包含”。

您可以通过设置`include.path`（或`includeIf.*.path`）的值为另一个配置文件的文件名，来包含这个配置文件的配置。该变量将路径名作为其值，`并受波形扩展的影响`。这些变量可以多次给出。

文件的内容会被立刻载入配，就好像它们已在 include 伪指令的位置找到一样。如果变量的值是相对路径，则该路径被认为是相对于包含 include 伪指令的配置文件路径。请参阅下面的示例。

### 有条件的包括

您可以通过将`includeIf.<condition>.path`变量设置为要包含的文件的名称来有条件地包含另一个配置文件。

条件以关键字后跟冒号开头，一些数据的格式和含义取决于关键字。支持的关键字是：

```
 gitdir 
```

关键字`gitdir:`后面的数据用作 glob 模式。如果.git 目录的位置与模式匹配，则满足包含条件。

.git 位置可以自动发现，也可以来自`$GIT_DIR`环境变量。如果通过.git 文件（例如，从子模块或链接的工作树）自动发现存储库，则.git 位置将是.git 目录所在的最终位置，而不是.git 文件所在的位置。

该模式可以包含标准的通配符和另外两个可以匹配多个路径组件的`**/`和`/**`。有关详细信息，请参阅 [gitignore[5]](https://git-scm.com/docs/gitignore) 。为了方便：

*   如果模式以`~/`开头，则`~`将替换为环境变量`HOME`的内容。

*   如果模式以`./`开头，则将其替换为包含当前配置文件的目录。

*   如果模式不以`~/`，`./`或`/`开始，则`**/`将自动添加前置。例如，模式`foo/bar`变为`**/foo/bar`并与`/any/path/to/foo/bar`匹配。

*   如果模式以`/`结束，则会自动添加`**`。例如，模式`foo/`变为`foo/**`。换句话说，它以递归方式匹配“foo”和内部的所有内容。

```
 gitdir/i 
```

这与`gitdir`相同，只是匹配是不区分大小写的（例如，在不区分大小写的文件系统上）

关于通过`gitdir`和`gitdir/i`进行匹配的更多注意事项：

*   `$GIT_DIR`中的符号链接在匹配之前未解析。

*   符号链接和路径的 realpath 版本将在`$GIT_DIR`之外匹配。例如，如果`~/git`是`/mnt/storage/git`的符号链接，那么`gitdir:~/git`和`gitdir:/mnt/storage/git`都会匹配。

    在 v2.13.0 中此功能的初始版本中并非如此，该版本仅匹配 realpath 版本。想要与此功能的初始版本兼容的配置需要仅指定 realpath 版本或两个版本。

*   请注意，“../”并不特殊，并且会按字面意思匹配，这不太可能是您想要的。

### 示例

```
# Core variables
[core]
	; Don't trust file modes
	filemode = false
```

```
# Our diff algorithm
[diff]
	external = /usr/local/bin/diff-wrapper
	renames = true
```

```
[branch "devel"]
	remote = origin
	merge = refs/heads/devel
```

```
# Proxy settings
[core]
	gitProxy="ssh" for "kernel.org"
	gitProxy=default-proxy ; for the rest
```

```
[include]
	path = /path/to/foo.inc ; include by absolute path
	path = foo.inc ; find "foo.inc" relative to the current file
	path = ~/foo.inc ; find "foo.inc" in your `$HOME` directory
```

```
; include if $GIT_DIR is /path/to/foo/.git
[includeIf "gitdir:/path/to/foo/.git"]
	path = /path/to/foo.inc
```

```
; include for all repositories inside /path/to/group
[includeIf "gitdir:/path/to/group/"]
	path = /path/to/foo.inc
```

```
; include for all repositories inside $HOME/to/group
[includeIf "gitdir:~/to/group/"]
	path = /path/to/foo.inc
```

```
; relative paths are always relative to the including
; file (if the condition is true); their location is not
; affected by the condition
[includeIf "gitdir:/path/to/group/"]
	path = foo.inc
```

### 值

许多变量的值被视为一个简单的字符串，但是有些变量采用特定类型的值，并且有关于如何拼写它们的规则。

```
 boolean 
```

当一个变量被认为是一个布尔值时，_true_ 和 _false_ 接受许多同义词；这些都是不区分大小写的。

```
 true 
```

字面意思是`yes`，`on`，`true`和`1`。此外，变量没有定义`= <value>`的话，默认值是 true。

```
 false 
```

字面意思是`no`，`off`，`false`，`0`和空字符串。

使用`--type=bool`类型说明符将值转换为规范形式时，_git config_ 将确保输出为“true”或“false”（拼写为小写）。

```
 integer 
```

指定各种大小的许多变量的值可以后缀为`k`，`M`，表示“将数字扩大 1024 倍”，“1024x1024 倍”等。

```
 color 
```

采用颜色的变量的值是一个颜色列表（最多两个，一个用于前景，一个用于背景）和属性（多个您想要的），用空格分隔。

接受的基本颜色是`normal`，`black`，`red`，`green`，`yellow`，`blue`，`magenta`，`cyan`和`white`。给出的第一种颜色是前景，第二是背景。

颜色也可以作为 0 到 255 之间的数字给出，这些使用 ANSI 256 色模式（但请注意，并非所有终端都支持此功能）。如果您的终端支持它，您也可以将 24 位 RGB 值指定为十六进制，如`#ff0ab3`。

接受的属性有`bold`，`dim`，`ul`，`blink`，`reverse`，`italic`和`strike`（用于划掉或“删除”字母）。任何属性相对于颜色（之前，之后或之间）的位置都无关紧要。可以通过在前缀`no`或`no-`（例如，`noreverse`，`no-ul`等）来关闭特定属性。

空颜色字符串根本不产生颜色效果。这可用于避免在不完全禁用颜色的情况下着色特定元素。

对于 git 的预定义颜色槽，属性应在彩色输出中每个项目的开头重置。因此，将`color.decorate.branch`设置为`black`将在普通`black`中绘制该分支名称，即使同一输出行上的前一项（例如`log --decorate`输出中的分支名称列表前的左括号）设置为被涂上`bold`或其他一些属性。但是，自定义日志格式可能会执行更复杂和分层的着色，而否定的格式在那里可能会有用。

```
 pathname 
```

获取路径名值的变量可以被赋予以“`~/`”或“`~user/`”开头的字符串，并且通常的波浪号扩展发生在这样的字符串中：`~/`扩展为`$HOME`的值，`~user/`为指定用户的根目录。

### 变量

请注意，此列表不全面，不一定完整。对于特定于命令的变量，您可以在相应的手册页中找到更详细的说明。

其他与 git 相关的工具可能并且确实使用它们自己的变量。在发明用于您自己的工具的新变量时，请确保它们的名称与 Git 本身和其他常用工具使用的名称不冲突，并在文档中对其进行描述。

```
 advice.* 
```

这些变量控制旨在帮助新用户的各种可选帮助消息。所有 _advice.*_ 变量默认为 _true_，你可以通过将这些设置为`false`告诉 Git 你不需要帮助：

```
 pushUpdateRejected 
```

如果要同时禁用 _pushNonFFCurrent_，_pushNonFFMatching_ ， _pushAlreadyExists_ ，和 _pushFetchFirst_，和 _pushNeedsForce_ 变量，请将此变量设置为 _false_。

```
 pushNonFFCurrent 
```

当 [git-push [1]](https://git-scm.com/docs/git-push) 由于对当前分支的非快进更新而失败时显示的建议。

```
 pushNonFFMatching 
```

当您运行 [git-push [1]](https://git-scm.com/docs/git-push) 并显式推送 _ 匹配 refs_ 时显示的建议（即您使用 _：_，或指定了不是您当前的 refspe 分支）并导致非快进错误。

```
 pushAlreadyExists 
```

当 [git-push [1]](https://git-scm.com/docs/git-push) 拒绝不符合快进条件的更新（例如，标签）时显示。

```
 pushFetchFirst 
```

当 [git-push [1]](https://git-scm.com/docs/git-push) 拒绝尝试覆盖指向我们没有的对象的远程引用的更新时显示。

```
 pushNeedsForce 
```

当 [git-push [1]](https://git-scm.com/docs/git-push) 拒绝尝试覆盖指向不是 commit-ish 的对象的远程 ref 的更新时，或者在不是 commit-ish 的对象上进行远程 ref 操作。

```
 pushUnqualifiedRefname 
```

当 [git-push [1]](https://git-scm.com/docs/git-push) 放弃尝试根据源和目标进行猜测时显示源所属的远程 ref 命名空间，但我们仍然可以建议用户推送到 refs/heads/* 或 refs/tags/*基于源对象的类型。

```
 statusHints 
```

在 [git-commit [1]](https://git-scm.com/docs/git-commit) 中写入提交消息时显示的模板中显示如何从 [git-status [1]](https://git-scm.com/docs/git-status) 的输出中的当前状态开始的指示，以及切换分支时，[git-checkout [1]](https://git-scm.com/docs/git-checkout) 显示的帮助信息。

```
 statusUoption 
```

建议考虑在 [git-status [1]](https://git-scm.com/docs/git-status) 中使用`-u`选项，当命令需要超过 2 秒来枚举未跟踪文件时。

```
 commitBeforeMerge 
```

当 [git-merge [1]](https://git-scm.com/docs/git-merge) 拒绝合并以避免覆盖本地更改时显示的建议。

```
 resetQuiet 
```

建议考虑在 [git-reset [1]](https://git-scm.com/docs/git-reset) 中使用`--quiet`选项，当命令需要 2 秒以上的时间来枚举复位后的非分段更改时。

```
 resolveConflict 
```

当冲突阻止执行操作时，各种命令显示的建议。

```
 implicitIdentity 
```

在从系统用户名和域名中猜出您的信息时，如何设置身份配置的建议。

```
 detachedHead 
```

使用 [git-checkout [1]](https://git-scm.com/docs/git-checkout) 移动到分离 HEAD 状态时显示的建议，以指示如何在事后创建本地分支。

```
 checkoutAmbiguousRemoteBranchName 
```

当 [git-checkout [1]](https://git-scm.com/docs/git-checkout) 的参数，在有多个远端的场景下，模糊地解析为远程跟踪分支时，如果明确的参数会导致远程跟踪分支被检出，则显示建议。请参阅`checkout.defaultRemote`配置变量，如何在给定远端的某些场景下使用。

```
 amWorkDir 
```

当 [git-am [1]](https://git-scm.com/docs/git-am) 无法应用时，显示补丁文件位置的建议。

```
 rmHints 
```

如果 [git-rm [1]](https://git-scm.com/docs/git-rm) 的输出失败，请显示如何从当前状态开始的指示。

```
 addEmbeddedRepo 
```

当你意外地在另一个内部添加一个 git repo 时该怎么做的建议。

```
 ignoredHook 
```

如果钩子被忽略，则显示建议，因为钩子未设置为可执行文件。

```
 waitingForEditor 
```

只要 Git 正在等待用户的编辑输入，就将消息打印到终端。

```
 core.fileMode 
```

告诉 Git 是否要遵守工作树中文件的可执行权限。

当检出标记为可执行的文件，或者以可执行权限检出非可执行文件时，在某些文件系统中会丢失可执行权限。 [git-clone [1]](https://git-scm.com/docs/git-clone) 或 [git-init [1]](https://git-scm.com/docs/git-init) 会探测当前文件系统，看它是否能正确处理文件权限，并根据需要自动设置此变量。

但是，存储库可能位于正确处理文件模式的文件系统上，并且此变量在开始配置时设置为 _true_ ，但稍后从其他环境访问可能会失去文件模式的设置（例如，通过导出 CIFS 挂载的 ext4 ，使用 Git for Windows 或 Eclipse 访问 Cygwin 创建的存储库）。在这种情况下，可能需要将此变量设置为 _false_ 。参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 。

默认值为 true（在配置文件中未指定 core.filemode 时）。

```
 core.hideDotFiles 
```

（仅限 Windows）如果为 true，将标记新创建的、以点开头的命名的目录和文件为隐藏。如果 _dotGitOnly_ ，则只隐藏`.git/`目录，但没有其他以点开头的文件。默认模式为 _dotGitOnly_ 。

```
 core.ignoreCase 
```

内部变量，支持各种变通方法，使 Git 能够更好地处理不区分大小写的文件系统，如 APFS，HFS +，FAT，NTFS 等。例如，如果要在某个目录列表下找出“makefile”文件，Git 会将“Makefile”输出，Git 认为它们是同一个文件，并继续记住它为“Makefile”。

默认值为 false，除了 [git-clone [1]](https://git-scm.com/docs/git-clone) 或 [git-init [1]](https://git-scm.com/docs/git-init) 将在创建存储库时预测并设置 core.ignoreCase 为 true。

在您的操作系统和文件系统上，Git 依赖于正确配置此变量的值。修改此值可能会导致意外后果。

```
 core.precomposeUnicode 
```

此选项仅供 Mac OS 实现 Git 使用。当 core.precomposeUnicode = true 时，Git 会恢复 Mac OS 上以 unicode 分解的文件名。在 Mac OS、Linux 或 Windows 之间共享存储库时，这非常有用。（需要适用于 Windows 1.7.10 或更高版本的 Git，或者在 cygwin 1.7 下使用 Git）。如果为 false，则 Git 会完全透明文件名，后者与旧版本的 Git 向后兼容。

```
 core.protectHFS 
```

如果设置为 true，在 HFS+文件系统上，则不允许检出文件路径，会被视为等同于`.git`的路径。在 Mac OS 上默认为`true`，在其他地方默认为`false`。

```
 core.protectNTFS 
```

如果设置为 true，则不允许签出可能导致 NTFS 文件系统出现问题的路径，例如：与 8.3“short”冲突的名称。在 Windows 上默认为`true`，在其他地方默认为`false`。

```
 core.fsmonitor
```

如果设置，则此变量的值将用作命令，该命令将标识自请求的日期/时间以来可能已更改的所有文件。此信息用于通过避免对未更改的文件进行不必要的处理来加速 git 操作。请参阅 [githooks [5]](https://git-scm.com/docs/githooks) 的“fsmonitor-watchman”部分。

```
 core.trustctime 
```

如果为 false，则忽略索引与工作树之间的 ctime 差异；当 inode 更改时间被 Git 之外的某些东西（文件系统爬虫和一些备份系统）定期修改时，将非常有用。参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 。默认为 True。

```
 core.splitIndex 
```

如果为 true，则将使用索引的拆分索引功能。参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 。默认为 False。

```
 core.untrackedCache 
```

确定如何处理索引的未跟踪缓存功能。如果未设置此变量或将其设置为`keep`，则将保留该值。如果设置为`true`，将自动添加。如果设置为`false`，它将自动删除。在将其设置为`true`之前，您应该检查 mtime 是否在您的系统上正常工作。参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 。 `keep`默认情况下。

```
 core.checkStat 
```

当缺失或设置为`default`时，将检查 stat 结构中的许多字段以检测文件是否已被修改，因为 Git 查看了它。当此配置变量设置为`minimal`时，mtime 和 ctime 的亚秒部分，文件所有者的 uid 和 gid，inode 编号（以及设备编号，如果 Git 编译为使用它），从这些字段中的检查中排除，只留下 mtime 的整个第二部分（和 ctime，如果设置了`core.trustCtime`）和要检查的文件大小。

Git 的实现不会在某些字段中留下可用的值（例如 JGit）;通过从比较中排除这些字段，当同个仓库在被其他系统使用时，`minimal`模式可以帮助实现互操作性。

```
 core.quotePath 
```

输出路径的命令（例如 _ls-files_ ， _diff_ ）将在路径名中引用“异常”字符，方法是将路径名括在双引号中并使用反斜杠转义那些字符。和在 C 语言中转义控制字符（例如 TAB 为`\t`，LF 为`\n`，反斜杠为`\\`）或值大于 0x80 的字节（例如，UTF-8 中为“micro”的八进制`\302\265`）使用的同样的方法。如果此变量设置为 false，则高于 0x80 的字节不再被视为“异常”。无论此变量的设置如何，双引号，反斜杠和控制字符始终都会被转义。简单的空格字符不被视为“不寻常”。许多命令可以使用`-z`选项完全逐字输出路径名。默认值是 true。

```
 core.eol 
```

设置要在工作目录中用于标记为文本的文件的行结束类型（通过设置`text`属性，或者使`text=auto`和 Git 自动检测内容为文本）。替代品是 _lf_ ， _crlf_ 和 _native_，它使用平台的原生的行结束符。默认值为`native`。有关行尾转换的更多信息，请参见 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。请注意，如果`core.autocrlf`设置为`true`或`input`，则忽略该值。

```
 core.safecrlf 
```

如果为 true，则当行结束转换处于活动状态时，使 Git 检查转换`CRLF`是否可逆。 Git 将验证命令是直接还是间接修改工作树中的文件。例如，提交文件后检出同一文件应该会在工作树中生成原始文件。如果`core.autocrlf`的当前设置不是这种情况，Git 将拒绝该文件。该变量可以设置为“警告”，在这种情况下，Git 只会警告不可逆转换，但继续操作。

CRLF 转换有可能破坏数据。启用时，Git 会在提交期间将 CRLF 转换为 LF，在检出时将 LF 转换为 CRLF。 Git 无法重新创建提交之前包含 LF 和 CRLF 混合的文件。对于文本文件，正确的做法是：它校正行结尾符，这样我们在存储库中只有 LF 行结尾。但对于意外归类为文本的二进制文件，转换可能会破坏数据。

如果您提前识别出此类损坏，则可以通过在.gitattributes 中明确设置转换类型来轻松解决此问题。提交后，您仍然在工作树中保留原始文件，此文件尚未损坏。你可以明确告诉 Git 这个文件是二进制文件，Git 会适当地处理文件。

不幸的是，无法区分清除具有混合行结尾的文本文件和破坏二进制文件的不良影响的期望效果。在这两种情况下，CRLF 都以不可逆转的方式被移除。对于文本文件，这是正确的做法，因为 CRLF 是行结尾，而对于二进制文件，转换 CRLF 会破坏数据。

请注意，此安全检查并不意味着在坚持文件时，将为`core.eol`和`core.autocrlf`的不同设置生成与原始文件相同的文件，但仅适用于当前的文件。例如，带有`LF`的文本文件将被`core.eol=lf`接受，之后可以使用`core.eol=crlf`检出，在这种情况下，生成的文件将包含`CRLF`，尽管原始文件包含`LF`。但是，在两个工作树中，行结束符将是一致的，即所有`LF`或全部`CRLF`，但从不混合。 `core.safecrlf`机制将报告具有混合行结尾的文件。

```
 core.autocrlf 
```

将此变量设置为“true”，与在所有文件上将`text`属性设置为“auto”，并将 core.eol 设置为“crlf”相同。如果要在工作目录中包含`CRLF`行结尾且存储库具有 LF 行结尾，则设置为 true。该变量可以设置为 _input_，在这种情况下不执行输出转换。

```
 core.checkRoundtripEncoding 
```

逗号和/或空格分隔的编码列表，Git 执行 UTF-8 往返检查它们是否在`working-tree-encoding`属性中使用（参见 [gitattributes [5]](https://git-scm.com/docs/gitattributes) ）。默认值为`SHIFT-JIS`。

```
 core.symlinks 
```

如果为 false，则将符号链接检出为包含链接文本的小型纯文本。 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 和 [git-add [1]](https://git-scm.com/docs/git-add) 不会将记录类型更改为常规文件。在 FAT 等不支持符号链接的文件系统上很有用。

默认值为 true，除了 [git-clone [1]](https://git-scm.com/docs/git-clone) 或 [git-init [1]](https://git-scm.com/docs/git-init) 将在创建存储库时预测并设置 core.symlinks 为 false。

```
 core.gitProxy 
```

执行（作为 _ 命令主机端口 _）的“代理命令”，在使用 Git 协议进行更新时，将建立替代与远程服务器的直接连接方式。如果变量值在“COMMAND for DOMAIN”格式中，则该命令仅应用于以指定域字符串结尾的主机名。该变量可以多次设置并按给定顺序匹配；第一次匹配上就不继续往下匹配了。

可以被`GIT_PROXY_COMMAND`环境变量覆盖（它总是普遍适用，没有特殊的“for”处理）。

特殊字符串`none`可用作代理命令，以指定不为给定的域模式使用代理。这对于从代理使用中排除防火墙内的服务器非常有用，同时默认为外部域的公共代理。

```
 core.sshCommand 
```

如果设置了此变量，`git fetch`和`git push`将在需要连接到远程系统时使用指定的命令而不是`ssh`。该命令与`GIT_SSH_COMMAND`环境变量的格式相同，并在设置环境变量时被覆盖。

```
 core.ignoreStat 
```

如果为 true，Git 将避免使用 lstat（）调用来检测文件是否已更改，方法是为索引和工作树中相同更新的跟踪文件设置“假定未更改”位。

当在 Git 之外修改文件时，用户将需要明确地分阶段修改文件（例如，参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 中的 _ 示例 _ 部分）。 Git 通常不会检测这些文件的更改。

这在 lstat（）调用非常慢的系统（例如 CIFS / Microsoft Windows）上很有用。

默认为 False。

```
 core.preferSymlinkRefs 
```

将替代 HEAD 和其他符号引用文件的默认“symref”格式，并使用符号链接。这有时需要使用期望 HEAD 成为符号链接的旧脚本。

```
 core.alternateRefsCommand 
```

当显示可用历史记录的提示时，将使用 shell 执行指定的命令，而不是 [git-for-each-ref [1]](https://git-scm.com/docs/git-for-each-ref) 。第一个参数是备用的绝对路径。输出必须每行包含一个十六进制对象 id（即，与`git for-each-ref --format='%(objectname)'`产生的相同）。

请注意，您通常不能将`git for-each-ref`直接放入配置值，因为它不会将存储库路径作为参数（但您可以将上面的命令包装在 shell 脚本中）。

```
 core.alternateRefsPrefixes 
```

列出备用引用时，仅列出以给定前缀开头的引用。前缀匹配，好像它们作为 [git-for-each-ref [1]](https://git-scm.com/docs/git-for-each-ref) 的参数给出。要列出多个前缀，请用空格分隔它们。如果设置了`core.alternateRefsCommand`，则设置`core.alternateRefsPrefixes`无效。

```
 core.bare 
```

如果为 true，则假定此存储库为 _bare_ 并且没有与之关联的工作目录。如果是这种情况，将禁用许多与工作目录相关的命令，例如 [git-add [1]](https://git-scm.com/docs/git-add) 或 [git-merge [1]](https://git-scm.com/docs/git-merge) 。

创建存储库时， [git-clone [1]](https://git-scm.com/docs/git-clone) 或 [git-init [1]](https://git-scm.com/docs/git-init) 会自动预测此设置。默认情况下，假定以“/.git”结尾的存储库不是 bare（bare=false），而假定所有其他存储库都是 bare（bare=ture）。

```
 core.worktree 
```

设置工作树根目录的路径。如果设置了`GIT_COMMON_DIR`环境变量，则会忽略 core.worktree，而不会用于确定工作树的根。这可以被`GIT_WORK_TREE`环境变量和`--work-tree`命令行选项覆盖。该值可以是绝对路径或相对于.git 目录的路径，该目录由--git-dir 或 GIT_DIR 指定，或自动发现。如果指定了--git-dir 或 GIT_DIR 但未指定--work-tree，GIT_WORK_TREE 和 core.worktree，则当前工作目录将被视为工作树的顶级。

请注意，即使在目录的“.git”子目录中的配置文件中设置此变量，并且其值与后一目录不同（例如“/path/to/.git/config”已将 core.worktree 设置为“/different/path”），这很可能是一个配置错误。在“/path/ to”目录中运行 Git 命令仍将使用“/different/path”作为工作树的根目录，除非您知道自己在做什么，否则可能会造成混淆（例如，您正在创建一个只读快照与存储库的通常工作树不同的位置的相同索引）。

```
 core.logAllRefUpdates 
```

启用 reflog。对 ref<ref>的更新记录到文件“`$GIT_DIR/logs/<ref>`”，方法是附加新旧 SHA-1，日期/时间和更新原因，但仅限于文件存在时。如果此配置变量设置为`true`，则会自动为分支头创建缺省的“`$GIT_DIR/logs/<ref>`”文件（即在`refs/heads/`下），远程 ref（即在`refs/remotes/`下），评论 ref（即在`refs/notes/`下） ）和符号 ref `HEAD`。如果设置为`always`，则会自动为`refs/`下的任何参考创建缺少的 reflog。

此信息可用于确定“2 天前”分支的提示是什么。

默认情况下，此值在具有与之关联的工作目录的存储库中为 true，默认情况下在空存储库中为 false。

```
 core.repositoryFormatVersion 
```

标识存储库格式和布局版本的内部变量。

```
 core.sharedRepository 
```

当 _ 组 _（或 _true_ ）时，存储库可在组中的多个用户之间共享（确保所有文件和对象都是组内可写的）。当 _ 所有 _（或 _ 世界 _ 或 _ 所有人 _）时，除了可分组之外，所有用户都可以读取存储库。当 _umask_ （或 _false_ ）时，Git 将使用 umask（2）报告的权限。当 _0xxx_ ，其中 _0xxx_ 是八进制数时，存储库中的文件将具有此模式值。 _0xxx_ 将覆盖用户的 umask 值（而其他选项只会覆盖用户的 umask 值的请求部分）。示例： _0660_ 将对所有者和组进行可读/可写，但其他人无法访问（相当于 _ 组 _，除非 umask 是例如 _0022_ ） 。 _0640_ 是一个可读取组但不可写入组的存储库。参见 [git-init [1]](https://git-scm.com/docs/git-init) 。默认为 False。

```
 core.warnAmbiguousRefs 
```

如果为 true，Git 会警告您，如果您传递的引用名称不明确并且可能与存储库中的多个引用相匹配。默认为 True。

```
 core.compression 
```

整数-1..9，表示默认压缩级别。 -1 是 zlib 的默认值。 0 表示没有压缩，1..9 是各种速度/大小权衡，9 表示最慢。如果设置，则为其他压缩变量提供默认值，例如`core.looseCompression`和`pack.compression`。

```
 core.looseCompression 
```

整数-1..9，表示不在包文件中的对象的压缩级别。 -1 是 zlib 的默认值。 0 表示没有压缩，1..9 是各种速度/大小权衡，9 表示最慢。如果未设置，则默认为 core.compression。如果未设置，则默认为 1（最佳速度）。

```
 core.packedGitWindowSize 
```

在单个映射操作中映射到内存的 pack 文件的字节数。设置大的值时，可以允许您的系统更快地处理较少数量的大包文件。设置较小值时，会因调用操作系统内存管理器频繁，而导致对性能产生负面影响，但在访问大量大型文件包时可能会提高性能。

如果在编译时设置 NO_MMAP，则默认值为 1 MiB，否则 32 位平台上为 32 MiB，64 位平台上为 1 GiB。这应该适用于所有用户/操作系统。您可能不需要调整此值。

支持 _k_ ， _m_ 或 _g_ 的通用单位后缀。

```
 core.packedGitLimit 
```

从包文件同时映射到内存的最大字节数。如果 Git 需要一次访问多个字节以完成操作，它将取消映射现有区域以回收进程中的虚拟地址空间。

在 32 位平台上默认为 256 MiB，在 64 位平台上默认为 32 TiB（实际上无限制）。对于所有用户/操作系统，这应该是合理的，除了最大的项目。您可能不需要调整此值。

支持 _k_ ， _m_ 或 _g_ 的通用单位后缀。

```
 core.deltaBaseCacheLimit 
```

保留用于缓存可能由多个已分层对象引用的基础对象的最大字节数。通过将整个解压缩的基础对象存储在高速缓存中，Git 能够避免多次解包和解压缩经常使用的基础对象。

所有平台上的默认值为 96 MiB。对于所有用户/操作系统，这应该是合理的，除了最大的项目。您可能不需要调整此值。

支持 _k_ ， _m_ 或 _g_ 的通用单位后缀。

```
 core.bigFileThreshold 
```

大于此大小的文件将直接存储，而不会尝试增量压缩。在没有增量压缩的情况下存储大型文件可以避免过多的内存使用，但会增加磁盘使用量。此外，大于此大小的文件始终被视为二进制文件。

所有平台上的默认值为 512 MiB。对于大多数项目来说这应该是合理的，因为源代码和其他文本文件仍然可以进行增量压缩，但是更大的二进制媒体文件不会。

支持 _k_ ， _m_ 或 _g_ 的通用单位后缀。

```
 core.excludesFile 
```

除了 _.gitignore_ （每个目录）和 _.git/info/exclude_ 之外，还可指定包含不打算跟踪的文件路径名。默认为`$XDG_CONFIG_HOME/git/ignore`。如果`$XDG_CONFIG_HOME`未设置或为空，则使用`$HOME/.config/git/ignore`。见 [gitignore [5]](https://git-scm.com/docs/gitignore) 。

```
 core.askPass 
```

交互式地请求密码的一些命令（例如，svn 和 http 接口）可以被告知使用通过该变量的值给出的外部程序。可以被`GIT_ASKPASS`环境变量覆盖。如果未设置，则回退到`SSH_ASKPASS`环境变量的值，或者，如果失败，则返回一个简单的密码提示。外部程序应作为命令行参数给出合适的提示，并在其 STDOUT 上写入密码。

```
 core.attributesFile 
```

除了 _.gitattributes_ （每个目录）和 _.git/info/attributes_ 之外，Git 还会查看此文件中的属性（参见 [gitattributes [5]](https://git-scm.com/docs/gitattributes) ） 。路径扩展的方式与`core.excludesFile`相同。其默认值为`$XDG_CONFIG_HOME/git/attributes`。如果`$XDG_CONFIG_HOME`未设置或为空，则使用`$HOME/.config/git/attributes`。

```
 core.hooksPath 
```

默认情况下，Git 会在 `_$GIT_DIR/hooks_` 目录中查找你的钩子。将此设置为不同的路径时，例如 _/etc/git/hooks_ ，Git 会尝试在该目录中找到你的钩子，例如 _/etc/git/hooks/pre-receive_ 而不是 `_$GIT_DIR/hooks/pre-receive_`。

路径可以是绝对路径也可以是相对路径。相对路径被视为相对于运行钩子的目录（参见 [githooks [5]](https://git-scm.com/docs/githooks) 的“DESCRIPTION”部分）。

如果您希望集中配置 Git 钩子而不是基于每个存储库配置它们，或者作为一个更加灵活和集中的替代方案来使用`init.templateDir`来更改默认挂钩，则此配置变量很有用。

```
 core.editor 
```

当此值被设置时，并且未设置环境变量`GIT_EDITOR`，在执行类似`commit`和`tag`等命令时，允许您通过启动编辑器编辑消息。见 [git-var [1]](https://git-scm.com/docs/git-var) 。

```
 core.commentChar 
```

在执行类似`commit`和`tag`等命令时，会在编辑消息的开头的添加设置的字符，并在编辑器退出后删除它们（默认 _＃_）。

如果设置为“auto”，`git-commit`将选择一个字符，该字符不是现有提交消息中任何行的开头字符。

```
 core.filesRefLockTimeout 
```

尝试锁定单个引用时重试的时间长度（以毫秒为单位）。值 0 表示根本不重试; -1 意味着无限期地尝试。默认值为 100（即重试 100 毫秒）。

```
 core.packedRefsTimeout 
```

尝试锁定`packed-refs`文件时重试的时间长度（以毫秒为单位）。值 0 表示根本不重试; -1 意味着无限期地尝试。默认值为 1000（即重试 1 秒）。

```
 core.pager 
```

文本查看器供 Git 命令使用（例如， _less_ ）。该值应由 shell 解释。首选顺序是`$GIT_PAGER`环境变量，然后是`core.pager`配置，然后是`$PAGER`，然后是在编译时选择的默认值（通常是 _less_）。

当未设置`LESS`环境变量时，Git 将其设置为`FRX`（如果设置了`LESS`环境变量，Git 根本不会更改它）。如果您想有选择地覆盖`LESS`的 Git 默认设置，您可以将`core.pager`设置为例如`less -S`。这将由 Git 传递给 shell，它将最终命令转换为`LESS=FRX less -S`。环境不设置`S`选项，但命令行会设置，指示较少截断长行。同样，将`core.pager`设置为`less -+F`将从命令行取消激活环境指定的`F`选项，取消激活`less`的“退出，如果一个屏幕”行为。可以专门为特定命令激活一些标志：例如，将`pager.blame`设置为`less -S`只能为`git blame`启用行截断。

同样，当未设置`LV`环境变量时，Git 将其设置为`-c`。您可以通过将`LV`导出为其他值或将`core.pager`设置为`lv +c`来覆盖此设置。

```
 core.whitespace 
```

要注意一系列以逗号分隔的常见空格问题。 _git diff_ 将使用`color.diff.whitespace`突出显示它们， _git apply --whitespace = error_ 会将它们视为错误。您可以为`-`添加前缀以禁用其中任何一个（例如`-trailing-space`）：

*   `blank-at-eol`将行末尾的尾随空格视为错误（默认情况下启用）。

*   `space-before-tab`将在行的初始缩进部分中的制表符之前出现的空格字符视为错误（默认情况下启用）。

*   `indent-with-non-tab`将带有空格字符而不是等效选项卡缩进的行视为错误（默认情况下不启用）。

*   `tab-in-indent`将行的初始缩进部分中的制表符视为错误（默认情况下不启用）。

*   `blank-at-eof`将在文件末尾添加的空行视为错误（默认情况下启用）。

*   `trailing-space`是涵盖`blank-at-eol`和`blank-at-eof`的简写。

*   `cr-at-eol`将行尾处的回车处理作为行终止符的一部分，即使用它，如果此回车符之前的字符不是空格（默认情况下未启用），则`trailing-space`不会触发。

*   `tabwidth=<n>`告诉标签占用多少个字符位置;这与`indent-with-non-tab`和 Git 修复`tab-in-indent`错误有关。默认选项卡宽度为 8.允许的值为 1 到 63。

```
 core.fsyncObjectFiles 
```

在编写目标文件时，此布尔值将启用 _fsync（）_。

这对于正确排序数据的文件系统来说是浪费时间和精力的，但对于不使用日志（传统 UNIX 文件系统）或仅使用日志元数据而不是文件内容（OS X 的 HFS+或 Linux）的文件系统非常有用。 ext3 带有“data = writeback”）。

```
 core.preloadIndex 
```

为 _git diff_ 等操作启用并行索引预加载

这可以加速像 _git diff_ 和 _git status_ 这样的操作，特别是在像 NFS 这样具有弱缓存语义和相对较高的 IO 延迟的文件系统上。启用后，Git 将并行执行与文件系统数据的索引比较，从而允许重叠 IO。默认为 true。

```
 core.unsetenvvars 
```

仅限 Windows：以逗号分隔的环境变量名称列表，需要在生成任何其他进程之前取消设置。默认为`PERL5LIB`，以说明 Git for Windows 坚持使用自己的 Perl 解释器。

```
 core.createObject 
```

您可以将其设置为 _link_ ，在这种情况下，使用硬链接后删除源来确保对象创建不会覆盖现有对象。

在某些文件系统/操作系统组合上，这是不可靠的。将此配置设置为 _rename_；但是，这将删除检查，以确保不会覆盖现有的目标文件。

```
 core.notesRef 
```

显示提交消息时，还会显示存储在给定引用中的注释。ref 必须完全合格。如果给定的 ref 不存在，则不是错误，而是表示不应打印​​任何注释。

此设置默认为“refs/notes/commits”，它可以被`GIT_NOTES_REF`环境变量覆盖。见 [git-notes [1]](https://git-scm.com/docs/git-notes) 。

```
 core.commitGraph 
```

如果为 true，则 git 将读取 commit-graph 文件（如果存在）以解析提交的图形结构。默认为 false。有关详细信息，请参阅 [git-commit-graph [1]](https://git-scm.com/docs/git-commit-graph) 。

```
 core.useReplaceRefs 
```

如果设置为`false`，则表现为在命令行上给出了`--no-replace-objects`选项。有关详细信息，请参阅 [git [1]](https://git-scm.com/docs/git) 和 [git-replace [1]](https://git-scm.com/docs/git-replace) 。

```
 core.multiPackIndex 
```

使用 multi-pack-index 文件使用单个索引跟踪多个 packfiles。请参见多包装索引设计文档。

```
 core.sparseCheckout 
```

启用“稀疏检出”功能。有关详细信息，请参阅 [git-read-tree [1]](https://git-scm.com/docs/git-read-tree) 中的“稀疏检出”部分。

```
 core.abbrev 
```

设置长度对象名称为缩写。如果未指定或设置为“auto”，则根据存储库中打包对象的近似数量计算适当的值，这有望使缩写对象名称在一段时间内保持唯一。最小长度为 4。

```
 add.ignoreErrors 
 add.ignore-errors (deprecated) 
```

告诉 _git add_ 在继续添加文件时，由于索引错误而无法添加某些文件。相当于 [git-add [1]](https://git-scm.com/docs/git-add) 的`--ignore-errors`选项。不推荐使用`add.ignore-errors`，因为它不遵循配置变量的通常命名约定。

```
 alias.* 
```

[git [1]](https://git-scm.com/docs/git) 命令包装器的命令别名，例如在定义“alias.last = cat-file commit HEAD”之后，调用“git last”等同于“git cat-file commit HEAD”。为避免使用脚本时出现混淆和麻烦，将忽略隐藏现有 Git 命令的别名。参数由空格分隔，支持通常的 shell 引用和转义。一对引号或反斜杠可用于引用它们。

如果别名扩展以感叹号为前缀，则将其视为 shell 命令。例如，定义“alias.new =！gitk --all --not ORIG_HEAD”，调用“git new”等同于运行 shell 命令“gitk --all --not ORIG_HEAD”。请注意，shell 命令将从存储库的顶级目录执行，该目录可能不一定是当前目录。通过从原始当前目录运行 _git rev-parse --show-prefix_ 来设置`GIT_PREFIX`。参见 [git-rev-parse [1]](https://git-scm.com/docs/git-rev-parse) 。

```
 am.keepcr 
```

如果为 true，git-am 将使用参数`--keep-cr`调用 mbox 格式的补丁 git-mailsplit。在这种情况下，git-mailsplit 不会从以`\r\n`结尾的行中删除`\r`。可以通过从命令行提供`--no-keep-cr`来覆盖。参见 [git-am [1]](https://git-scm.com/docs/git-am) ， [git-mailsplit [1]](https://git-scm.com/docs/git-mailsplit) 。

```
 am.threeWay 
```

默认情况下，如果补丁不能完全应用，`git am`将失败。当设置为 true 时，如果补丁记录了应该应用的 blob 的身份，则此设置告诉`git am`回退到三向合并，并且我们在本地可以获得这些 blob（相当于提供`--3way`选项命令行）。默认为`false`。见 [git-am [1]](https://git-scm.com/docs/git-am) 。

```
 apply.ignoreWhitespace 
```

当设置为 _chang_ 时，告诉 _git apply_ 忽略空白的变化，与`--ignore-space-change`选项相同。设置为以下之一：no，none，never，false 告诉 _git apply_ 尊重所有空格差异。参见 [git-apply [1]](https://git-scm.com/docs/git-apply) 。

```
 apply.whitespace 
```

告诉 _git apply_ 如何处理空格，方法与`--whitespace`选项相同。参见 [git-apply [1]](https://git-scm.com/docs/git-apply) 。

```
 blame.blankBoundary 
```

在 [git-blame [1]](https://git-scm.com/docs/git-blame) 中显示边界提交的空白提交对象名称。此选项默认为 false。

```
 blame.coloring 
```

这确定了应用于非 blame 输出的着色方案。它可以是 _repeatedLines_ ， _highlightRecent_ 或 _none_ 这是默认值。

```
 blame.date 
```

指定用于在 [git-blame [1]](https://git-scm.com/docs/git-blame) 中输出日期的格式。如果未设置，则使用 iso 格式。有关支持的值，请参阅 [git-log [1]](https://git-scm.com/docs/git-log) 中`--date`选项的讨论。

```
 blame.showEmail 
```

在 [git-blame [1]](https://git-scm.com/docs/git-blame) 中显示作者电子邮件而不是作者姓名。此选项默认为 false。

```
 blame.showRoot 
```

不要将 root 提交视为 [git-blame [1]](https://git-scm.com/docs/git-blame) 中的边界。此选项默认为 false。

```
 branch.autoSetupMerge 
```

告诉 _git branch_ 和 _git checkout_ 设置新的分支，以便 [git-pull [1]](https://git-scm.com/docs/git-pull) 将从起点分支适当地合并。请注意，即使未设置此选项，也可以使用`--track`和`--no-track`选项按分支选择此行为。有效设置为：`false` - 未进行自动设置; `true` - 当起点是远程跟踪分支时，自动设置完成; `always` - 当起始点是本地分支或远程跟踪分支时，自动设置完成。此选项默认为 true。

```
 branch.autoSetupRebase 
```

当使用跟踪另一个分支的 _git branch_ 或 _git checkout_ 创建一个新分支时，此变量告诉 Git 设置 pull to rebase 而不是 merge（请参阅“branch.<name>.rebase“）。当`never`时，rebase 永远不会自动设置为 true。当`local`时，对于其他本地分支的跟踪分支，rebase 设置为 true。当`remote`时，对于跟踪的远程跟踪分支分支，rebase 设置为 true。当`always`时，对于所有跟踪分支，rebase 将设置为 true。有关如何设置分支以跟踪另一个分支的详细信息，请参阅“branch.autoSetupMerge”。此选项默认为 never。

```
 branch.sort 
```

当 [git-branch [1]](https://git-scm.com/docs/git-branch) 显示时，此变量控制分支的排序顺序。没有“--sort =<value>”提供的选项，此变量的值将用作默认值。有关有效值，请参阅 [git-for-each-ref [1]](https://git-scm.com/docs/git-for-each-ref) 字段名称。

```
 branch.<name>.remote 
```

当在分支<name>上时，它告诉 _git fetch_ 和 _git push_ 哪个远程提取/推送到。可以使用`remote.pushDefault`（对于所有分支）覆盖要推送到的远程。对于当前分支，推送到的远程可以被`branch.<name>.pushRemote`进一步覆盖。如果未配置远程，或者您不在任何分支上，则默认为`origin`进行提取，`remote.pushDefault`进行推送。另外，`.`（一个句点）是当前的本地存储库（一个点存储库），请参阅下面的`branch.<name>.merge`的最后一个注释。

```
 branch.<name>.pushRemote 
```

当在分支<name>上时，它会覆盖`branch.<name>.remote`以进行推送。它还会覆盖从分支<name>推送的`remote.pushDefault`。当您从一个地方（例如您的上游）拉出并推送到另一个地方（例如您自己的发布存储库）时，您可能希望设置`remote.pushDefault`以指定要推送到所有分支的远程，并使用此选项覆盖它对于特定的分支。

```
 branch.<name>.merge 
```

与 branch<name>.remote 一起定义给定分支的上游分支。它告诉 _git fetch_ / _git pull_ / _git rebase_ 哪个分支合并也会影响 _git push_ （参见 push.default）。当在分支<name>时，它告诉 _git fetch_ 默认的 refspec 被标记为在 FETCH_HEAD 中合并。该值的处理类似于 refspec 的远程部分，并且必须匹配从“branch.<name>.remote”给出的远程提取的 ref。合并信息由 _git pull_ （首先调用 _git fetch_ ）来查找默认分支以进行合并。如果没有此选项， _git pull_ 默认合并第一个引用的 refspec。指定多个值以获得章鱼合并。如果你想设置 _git pull_ 以便它合并到<name>从本地存储库中的另一个分支，您可以将分支.<name>.merge 指向所需的分支，并使用相对路径设置`.`（句点）进行 branch.<name>.remote。

```
 branch.<name>.mergeOptions 
```

设置合并到分支<name>的默认选项。语法和支持的选项与 [git-merge [1]](https://git-scm.com/docs/git-merge) 的选项相同，但目前不支持包含空格字符的选项值。

```
 branch.<name>.rebase 
```

如果为 true，在“git pull”运行时，rebase 分支<name>到已获取的分支之上，而不是从默认远程合并到默认分支。请参阅“pull.rebase”以非特定于分支的方式执行此操作。

当`merges`时，将`--rebase-merges`选项传递给 _git rebase_ ，以便本地合并提交包含在 rebase 中（有关详细信息，请参阅 [git-rebase [1]](https://git-scm.com/docs/git-rebase) ）。

当 perserve 时，也将`--preserve-merges`传递给 _git rebase_ ，以便通过运行 _git pull_ 不会使本地提交的合并提交变平。

当值为`interactive`时，rebase 以交互模式运行。

**注**：这可能是危险的操作;**不要**使用它，除非你理解其含义（详见 [git-rebase [1]](https://git-scm.com/docs/git-rebase) ）。

```
 branch.<name>.description 
```

分支描述，可以使用`git branch --edit-description`进行编辑。分支描述会自动添加到格式补丁封面信函或请求摘要中。

```
 browser.<tool>.cmd 
```

指定用于调用指定浏览器的命令。在 shell 中使用作为参数传递的 URL 计算指定的命令。 （参见 [git-web {litdd}浏览[1]](https://git-scm.com/docs/git-web{litdd}browse) 。）

```
 browser.<tool>.path 
```

覆盖可用于浏览 HTML 帮助的给定工具的路径（参见 [git-help [1]](https://git-scm.com/docs/git-help) 中的`-w`选项）或 gitweb 中的工作存储库（参见 [git-instaweb [ 1]](https://git-scm.com/docs/git-instaweb) ）。

```
 checkout.defaultRemote 
```

当你运行 _git checkout <something>_ 并且只有一个远端时，它可能隐含地退回检查和跟踪，例如 _origin/<something>_ 。一旦你有超过一个带有<something>的远程引用时，这就会停止工作。此设置允许设置首选远程的名称，该名称在消除歧义时应始终获胜。典型的用例是将其设置为`origin`。

目前，它被[git-checkout [1]](https://git-scm.com/docs/git-checkout) 使用，当 _git checkout <something>_ 时，将在另一个远端上检出 _<something>_ 分支，而 [git-worktree [1]](https://git-scm.com/docs/git-worktree) 当 _git worktree add_ 指的是一个远程分支。此设置可能会在将来用于其他类似 checkout 的命令或功能。

```
 checkout.optimizeNewBranch 
```

当使用稀疏检出时，用于优化“git checkout -b <new_branch>”的性能。设置为 true 时，git 不会根据当前的稀疏检出设置更新 repo。这意味着它不会更新索引中的 skip-worktree 位，也不会在工作目录中添加/删除文件以反映当前的稀疏检出设置，也不会显示本地更改。

```
 clean.requireForce 
```

一个布尔值会使 git-clean 无效，除非给出-f，-i 或-n，默认为 true。

```
 color.advice 
```

一个布尔值用于启用/禁用提示中颜色（例如，当推送失败时，请参阅`advice.*`以获取列表）。可以设置为`always`，`false`（或`never`）或`auto`（或`true`），在这种情况下，只有在错误输出到达终端时才使用颜色。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.advice.hint 
```

使用自定义颜色提示。

```
 color.blame.highlightRecent 
```

这可用于根据每行的更改日期为 blame 的元数据着色。

此设置应设置为以逗号分隔的颜色和日期设置列表，以颜色开始和结束，日期应设置为从最旧到最新。如果在给定时间戳之前引入该行，则元数据将根据颜色着色，覆盖较旧的带时间戳的颜色。

而不是绝对时间戳相对时间戳也起作用，例如 2.weeks.ago 适用于 2 周以上的任何事情。

默认为 _ 蓝色，12 个月前，白色，1 个月前，红色 _，颜色为一年以上的所有颜色为蓝色，最近一个月和一年之间的变化保持白色，上个月是红色的。

```
 color.blame.repeatedLines 
```

使用自定义颜色作为 git-blame 输出的部分，每行重复元信息（例如提交 ID，作者姓名，日期和时区）。默认为青色。

```
 color.branch 
```

一个布尔值在 [git-branch [1]](https://git-scm.com/docs/git-branch) 的输出中启用/禁用颜色。可以设置为`always`，`false`（或`never`）或`auto`（或`true`），在这种情况下，颜色仅在输出到终端时使用。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.branch.<slot> 
```

使用自定义颜色进行分支着色。 `<slot>`是`current`（当前分支），`local`（本地分支），`remote`（refs/remotes/中的远程跟踪分支），`upstream`（上游跟踪分支），`plain`（其他参考文献）。

```
 color.diff 
```

是否使用 ANSI 转义序列为补丁添加颜色。如果设置为`always`， [git-diff [1]](https://git-scm.com/docs/git-diff) ， [git-log [1]](https://git-scm.com/docs/git-log) 和 [git-show [1]](https://git-scm.com/docs/git-show) 将使用所有补丁的颜色。如果设置为`true`或`auto`，则这些命令仅在输出到终端时使用颜色。如果未设置，则使用`color.ui`的值（默认为`auto`）。

这不会影响 [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 或 _git-diff- *_ 管道命令。可以使用`--color[=<when>]`选项在命令行上覆盖。

```
 color.diff.<slot> 
```

使用自定义颜色进行差异着色。 `<slot>`指定修补程序的哪个部分使用指定的颜色，并且是`context`之一（上下文文本 - `plain`是历史同义词），`meta`（元信息），`frag`（Hunk header）， _func_ （hunk 头中的函数），`old`（删除行），`new`（添加行），`commit`（提交头），`whitespace`（突出显示空格错误），`oldMoved` （删除行），`newMoved`（添加行），`oldMovedDimmed`，`oldMovedAlternative`，`oldMovedAlternativeDimmed`，`newMovedDimmed`，`newMovedAlternative` `newMovedAlternativeDimmed`（参见 _<mode>_ 设置 _- [git-diff [1]](https://git-scm.com/docs/git-diff) 中的颜色移动 _ 详情），`contextDimmed`，`oldDimmed`，`newDimmed`，`contextBold`，`oldBold`和`newBold`（详见 [git-range-diff [1]](https://git-scm.com/docs/git-range-diff) ）。

```
 color.decorate.<slot> 
```

使用 _git log --decorate_ 输出的自定义颜色。 `<slot>`分别是本地分支，远程跟踪分支，标签，存储和 HEAD 的`branch`，`remoteBranch`，`tag`，`stash`或`HEAD`之一，以及用于移植提交的`grafted`。

```
 color.grep 
```

设置为`always`时，始终突出显示匹配项。当`false`（或`never`）时，永远不会。设置为`true`或`auto`时，仅在将输出写入终端时使用颜色。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.grep.<slot> 
```

使用自定义颜色进行 grep 着色。 `<slot>`指定行的哪一部分使用指定的颜色，并且是其中之一

```
 context 
```

上下文行中不匹配的文本（使用`-A`，`-B`或`-C`时）

```
 filename 
```

文件名前缀（不使用`-h`时）

```
 function 
```

功能名称行（使用`-p`时）

```
 lineNumber 
```

行号前缀（使用`-n`时）

```
 column 
```

列号前缀（使用`--column`时）

```
 match 
```

匹配文本（与设置`matchContext`和`matchSelected`相同）

```
 matchContext 
```

匹配上下文行中的文本

```
 matchSelected 
```

匹配所选行中的文本

```
 selected 
```

选定行中不匹配的文本

```
 separator 
```

一行（`:`，`-`和`=`）之间以及之间的字段之间的分隔符（`--`）

```
 color.interactive 
```

设置为`always`时，始终使用颜色进行交互式提示和显示（例如“git-add --interactive”和“git-clean --interactive”使用的颜色）。如果为假（或`never`），则永远不会。设置为`true`或`auto`时，仅在输出到终端时使用颜色。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.interactive.<slot> 
```

使用自定义颜色 _git add --interactive_ 和 _git clean --interactive_ 输出。 `<slot>`可以是`prompt`，`header`，`help`或`error`，用于交互式命令的四种不同类型的正常输出。

```
 color.pager 
```

在寻呼机正在使用时启用/禁用彩色输出的布尔值（默认为 true）。

```
 color.push 
```

用于启用/禁用推送错误颜色的布尔值。可以设置为`always`，`false`（或`never`）或`auto`（或`true`），在这种情况下，颜色仅在错误输出到达终端时使用。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.push.error 
```

使用自定义颜色进行推送错误。

```
 color.remote 
```

如果设置，则突出显示行开头的关键字。关键字是“错误”，“警告”，“提示”和“成功”，并且不区分大小写。可以设置为`always`，`false`（或`never`）或`auto`（或`true`）。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.remote.<slot> 
```

为每个远程关键字使用自定义颜色。 `<slot>`可以是与相应关键字匹配的`hint`，`warning`，`success`或`error`。

```
 color.showBranch 
```

在 [git-show-branch [1]](https://git-scm.com/docs/git-show-branch) 的输出中启用/禁用颜色的布尔值。可以设置为`always`，`false`（或`never`）或`auto`（或`true`），在这种情况下，颜色仅在输出到终端时使用。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.status 
```

在 [git-status [1]](https://git-scm.com/docs/git-status) 的输出中启用/禁用颜色的布尔值。可以设置为`always`，`false`（或`never`）或`auto`（或`true`），在这种情况下，颜色仅在输出到终端时使用。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.status.<slot> 
```

使用自定义颜色进行状态着色。 `<slot>`是`header`（状态消息的标题文本），`added`或`updated`（已添加但未提交的文件）之一，`changed`（已更改但未添加到索引中的文件） ），`untracked`（未被 Git 跟踪的文件），`branch`（当前分支），`nobranch`（显示 _ 无分支 _ 警告的颜色，默认为红色），`localBranch`或`remoteBranch`（分支和跟踪信息以状态短格式显示时的本地和远程分支名称）或`unmerged`（具有未更改的更改的文件）。

```
 color.transport 
```

拒绝推送时启用/禁用颜色的布尔值。可以设置为`always`，`false`（或`never`）或`auto`（或`true`），在这种情况下，颜色仅在错误输出到达终端时使用。如果未设置，则使用`color.ui`的值（默认为`auto`）。

```
 color.transport.rejected 
```

推送被拒绝时使用自定义颜色。

```
 color.ui 
```

此变量确定控制每个命令族颜色使用的变量（如`color.diff`和`color.grep`）的默认值。随着更多命令学习配置以设置`--color`选项的默认值，其范围将扩展。如果您希望 Git 命令不使用颜色，则将其设置为`false`或`never`，除非使用其他配置或`--color`选项明确启用。如果您希望所有不是用于机器消耗的输出使用颜色，将其设置为`always`，如果您希望此类输出在写入时使用颜色，则将其设置为`true`或`auto`（这是 Git 1.8.4 以来的默认设置）终点站。

```
 column.ui 
```

指定是否应在列中输出支持的命令。此变量由以空格或逗号分隔的标记列表组成：

这些选项控制何时启用该功能（默认为 _never_ ）：

```
 always 
```

总是显示在列中

```
 never 
```

从不在列中显示

```
 auto 
```

如果输出到终端，则显示在列中

这些选项控制布局（默认为 _ 列 _）。如果 _always_，_never_ 或 _auto_，则设置任何这些意味着 _always_。

```
 column 
```

在行之前填充列

```
 row 
```

在列之前填充行

```
 plain 
```

显示在一列中

最后，这些选项可以与布局选项结合使用（默认为 _nodense_ ）：

```
 dense 
```

使不等大小的列使用更多空间

```
 nodense 
```

制作相同大小的列

```
 column.branch 
```

指定是否在列中的`git branch`中输出分支列表。有关详细信息，请参阅`column.ui`。

```
 column.clean 
```

在`git clean -i`中列出项目时指定布局，它始终以列显示文件和目录。有关详细信息，请参阅`column.ui`。

```
 column.status 
```

指定是否在列中的`git status`中输出未跟踪的文件。有关详细信息，请参阅`column.ui`。

```
 column.tag 
```

指定是否在列中的`git tag`中输出标签列表。有关详细信息，请参阅`column.ui`。

```
 commit.cleanup 
```

此设置将覆盖`git commit`中`--cleanup`选项的默认值。有关详细信息，请参阅 [git-commit [1]](https://git-scm.com/docs/git-commit) 。当您总是希望在日志消息中保留以注释字符`#`开头的行时，更改默认值会很有用，在这种情况下您将执行`git config commit.cleanup whitespace`（请注意，您必须删除在提交日志模板中以`#`开头的帮助行，如果你这样做）。

```
 commit.gpgSign 
```

一个布尔值，用于指定是否所有提交都应进行 GPG 签名。在执行诸如 rebase 之类的操作时使用此选项可能会导致大量提交被签名。使用代理可能很方便避免多次输入 GPG 密码。

```
 commit.status 
```

一个布尔值，用于在使用编辑器准备提交消息时启用/禁用提交消息模板中的状态信息。默认为 true。

```
 commit.template 
```

指定要用作新提交消息模板的文件的路径名。

```
 commit.verbose 
```

boolean 或 int，用`git commit`指定详细级别。参见 [git-commit [1]](https://git-scm.com/docs/git-commit) 。

```
 credential.helper 
```

指定在需要用户名或密码凭据时要调用的外部帮助程序;帮助程序可以咨询外部存储，以避免提示用户输入凭据。请注意，可以定义多个帮助程序。有关详细信息，请参阅 [gitcredentials [7]](https://git-scm.com/docs/gitcredentials) 。

```
 credential.useHttpPath 
```

获取凭据时，请考虑 http 或 https URL 的“路径”组件。默认为 false。有关详细信息，请参阅 [gitcredentials [7]](https://git-scm.com/docs/gitcredentials) 。

```
 credential.username 
```

如果没有为网络身份验证设置用户名，则默认使用此用户名。请参阅凭证。<context>.*以及 [gitcredentials [7]](https://git-scm.com/docs/gitcredentials) 。

```
 credential.<url>.* 
```

上面的任何凭证。*选项都可以有选择地应用于某些凭据。例如，“credential.https：//example.com.username”将仅为 https 与 example.com 的连接设置默认用户名。有关如何匹配 URL 的详细信息，请参阅 [gitcredentials [7]](https://git-scm.com/docs/gitcredentials) 。

```
 credentialCache.ignoreSIGHUP 
```

告诉 git-credential-cache-daemon 忽略 SIGHUP，而不是退出。

```
 completion.commands 
```

这仅由 git-completion.bash 用于在已完成命令列表中添加或删除命令。通常只完成瓷器命令和一些选择其他命令。您可以在此变量中添加更多以空格分隔的命令。使用 _-_ 对命令进行前缀将从现有列表中删除它。

```
 diff.autoRefreshIndex 
```

使用 _git diff_ 与工作树文件进行比较时，不要将仅限统计更改视为已更改。而是静默运行`git update-index --refresh`以更新工作树中的内容与索引中的内容匹配的路径的缓存统计信息。此选项默认为 true。请注意，这仅影响 _git diff_ Porcelain，而不影响 _git diff-files_ 等低级 _diff_ 命令。

```
 diff.dirstat 
```

逗号分隔的`--dirstat`参数列表，指定 [git-diff [1]](https://git-scm.com/docs/git-diff) 和朋友的`--dirstat`选项的默认行为。可以在命令行上覆盖默认值（使用`--dirstat=<param1,param2,...>`）。回退默认值（当`diff.dirstat`未更改时）为`changes,noncumulative,3`。可以使用以下参数：

```
 changes 
```

通过计算已从源中删除或添加到目标的行来计算 dirstat 数。这忽略了文件中纯代码移动的数量。换句话说，重新排列文件中的行不会像其他更改那样计算。这是没有给出参数时的默认行为。

```
 lines 
```

通过执行常规的基于行的差异分析来计算 dirstat 数字，并对移除/添加的行数进行求和。（对于二进制文件，计算 64 字节块，因为二进制文件没有自然的线条概念）。这是比`changes`行为更昂贵的`--dirstat`行为，但它确实计算文件中重新排列的行与其他更改一样多。结果输出与您从其他`--*stat`选项获得的输出一致。

```
 files 
```

通过计算更改的文件数来计算 dirstat 数。在 dirstat 分析中，每个更改的文件都相同。这是计算上最便宜的`--dirstat`行为，因为它根本不需要查看文件内容。

```
 cumulative 
```

计算父目录的子目录中的更改。请注意，使用`cumulative`时，报告的百分比总和可能超过 100％。可以使用`noncumulative`参数指定默认（非累积）行为。

```
 <limit> 
```

整数参数指定截止百分比（默认为 3％）。贡献低于此百分比变化的目录不会显示在输出中。

示例：以下将计算已更改的文件，同时忽略少于已更改文件总量的 10％的目录，并在父目录中累计子目录计数：`files,10,cumulative`。

```
 diff.statGraphWidth 
```

在--stat 输出中限制图形部分的宽度。如果设置，则适用于除 format-patch 之外的所有生成--stat 输出的命令。

```
 diff.context 
```

用<n>生成差异。上下文行而不是默认值 3。此值可由-U 选项覆盖。

```
 diff.interHunkContext 
```

显示差异之间的上下文，直到指定的行数，从而融合彼此接近的行。此值用作`--inter-hunk-context`命令行选项的默认值。

```
 diff.external 
```

如果设置了此配置变量，则不使用内部 diff 机器执行 diff 生成，而是使用给定命令。可以使用'GIT_EXTERNAL_DIFF'环境变量覆盖。使用 [git [1]](https://git-scm.com/docs/git) 中“git Diffs”下所述的参数调用该命令。注意：如果您只想在文件的子集上使用外部差异程序，则可能需要使用 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 diff.ignoreSubmodules 
```

设置--ignore-submodules 的默认值。请注意，这仅影响 _git diff_ Porcelain，而不影响 _git diff-files_ 等低级 _diff_ 命令。 _git checkout_ 在报告未提交的更改时也会尊重此设置。设置为 _ 所有 _ 禁用 _git commit_ 和 _git status_ 通常显示的子模块摘要，当设置`status.submoduleSummary`时除非使用--ignore 覆盖它-submodules 命令行选项。 _git 子模块 _ 命令不受此设置的影响。

```
 diff.mnemonicPrefix 
```

如果设置， _git diff_ 使用的前缀对与标准“a/”和“b/”不同，具体取决于所比较的内容。当此配置生效时，反向差异输出也会交换前缀的顺序：

```
 git diff 
```

比较索引和工作树;

```
 git diff HEAD 
```

比较提交和工作树;

```
 git diff --cached 
```

比较 commit 和 index;

```
 git diff HEAD:file1 file2 
```

比较对象和工作树实体;

```
 git diff --no-index a b 
```

比较两个非 git 的东西（1）和（2）。

```
 diff.noprefix 
```

如果设置， _git diff_ 不显示任何源或目标前缀。

```
 diff.orderFile 
```

指示如何在差异中订购文件的文件。有关详细信息，请参阅 [-](https://git-scm.com/docs/git-diff) [git-diff [1]](https://git-scm.com/docs/git-diff) 的 _-O_ 选项。如果`diff.orderFile`是相对路径名，则将其视为相对于工作树顶部的相对路径名。

```
 diff.renameLimit 
```

执行复制/重命名检测时要考虑的文件数;相当于 _git diff_ 选项`-l`。如果关闭重命名检测，此设置无效。

```
 diff.renames 
```

Git 是否以及如何检测重命名。如果设置为“false”，则禁用重命名检测。如果设置为“true”，则启用基本重命名检测。如果设置为“copies”或“copy”，Git 也会检测副本。默认为 true。请注意，这仅影响 _git diff_ Porcelain，如 [git-diff [1]](https://git-scm.com/docs/git-diff) 和 [git-log [1]](https://git-scm.com/docs/git-log) ，而不是[等低级命令] git-diff-files [1]](https://git-scm.com/docs/git-diff-files) 。

```
 diff.suppressBlankEmpty 
```

一个布尔值，用于禁止在每个空输出行之前打印空格的标准行为。默认为 false。

```
 diff.submodule 
```

指定显示子模块差异的格式。 “short”格式只显示范围开头和结尾的提交名称。 “log”格式列出 [git-submodule [1]](https://git-scm.com/docs/git-submodule) `summary`范围内的提交。 “diff”格式显示子模块更改内容的内联差异。默认为“short”。

```
 diff.wordRegex 
```

POSIX 扩展正则表达式用于在执行逐字差异计算时确定什么是“单词”。与正则表达式匹配的字符序列是“单词”，所有其他字符都是**可忽略的**空格。

```
 diff.<driver>.command 
```

自定义 diff 驱动程序命令。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 diff.<driver>.xfuncname 
```

diff 驱动程序应该用来识别 hunk 标头的正则表达式。也可以使用内置模式。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 diff.<driver>.binary 
```

将此选项设置为 true 可使 diff 驱动程序将文件视为二进制文件。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 diff.<driver>.textconv 
```

diff 驱动程序应调用的命令，以生成文本转换后的文件版本。转换的结果用于生成人类可读的差异。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 diff.<driver>.wordRegex 
```

diff 驱动程序用于拆分行中单词的正则表达式。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 diff.<driver>.cachetextconv 
```

将此选项设置为 true 可使 diff 驱动程序缓存文本转换输出。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 diff.tool 
```

控制 [git-difftool [1]](https://git-scm.com/docs/git-difftool) 使用哪种 diff 工具。此变量将覆盖`merge.tool`中配置的值。下面的列表显示了有效的内置值。任何其他值都被视为自定义差异工具，并且需要定义相应的 difftool.<tool>.cmd 变量。

```
 diff.guitool 
```

当指定-g/-gui 标志时，控制 [git-difftool [1]](https://git-scm.com/docs/git-difftool) 使用哪个 diff 工具。此变量将覆盖`merge.guitool`中配置的值。下面的列表显示了有效的内置值。任何其他值都被视为自定义差异工具，并且需要定义相应的 difftool.<guitool>.cmd 变量。

*   araxis

*   bc

*   bc3

*   codecompare

*   deltawalker

*   diffmerge

*   diffuse

*   ecmerge

*   emerge

*   examdiff

*   guiffy

*   gvimdiff

*   gvimdiff2

*   gvimdiff3

*   kdiff3

*   kompare

*   meld

*   了 opendiff

*   p4merge

*   tkdiff

*   vimdiff

*   vimdiff2

*   vimdiff3

*   winmerge

*   xxdiff

```
 diff.indentHeuristic 
```

将此选项设置为`true`以启用实验启发式算法，可以移动差异块边界以使补丁更易于阅读。

```
 diff.algorithm 
```

选择差异算法。变体如下：

```
 default, myers 
```

基本的贪心差异算法。目前，这是默认值。

```
 minimal 
```

花些额外的时间来确保产生尽可能小的差异。

```
 patience 
```

生成补丁时使用“耐心差异”算法。

```
 histogram 
```

该算法将耐心算法扩展为“支持低发生的共同元素”。

```
 diff.wsErrorHighlight 
```

突出显示差异的`context`，`old`或`new`行中的空白错误。多个值用逗号分隔，`none`重置先前的值，`default`将列表重置为`new`，`all`是`old,new,context`的简写。空白错误用`color.diff.whitespace`着色。命令行选项`--ws-error-highlight=<kind>`会覆盖此设置。

```
 diff.colorMoved 
```

如果设置为有效的`<mode>`或真值，则差异中的移动线的颜色会有所不同，有效模式的详细信息请参见 _- [git-diff [1]中的[颜色移动](https://git-scm.com/docs/git-diff)_ 。如果只是设置为 true，将使用默认颜色模式。设置为 false 时，移动的线条不会着色。

```
 diff.colorMovedWS 
```

当使用例如移动的线条着色时`diff.colorMoved`设置，此选项控制`<mode>`如何处理有效模式的详细信息，请参阅 [git-diff [1]](https://git-scm.com/docs/git-diff) 中的 _--color-moved-ws_ 。

```
 difftool.<tool>.path 
```

覆盖给定工具的路径。如果您的工具不在 PATH 中，这非常有用。

```
 difftool.<tool>.cmd 
```

指定用于调用指定 diff 工具的命令。在 shell 中使用以下变量计算指定的命令： _LOCAL_ 设置为包含 diff 前映像内容的临时文件的名称， _REMOTE_ 设置为包含 diff 后映像内容的临时文件的名称。

```
 difftool.prompt 
```

在每次调用 diff 工具之前提示。

```
 fastimport.unpackLimit 
```

如果 [git-fast-import [1]](https://git-scm.com/docs/git-fast-import) 导入的对象数低于此限制，则对象将被解压缩为松散的目标文件。但是，如果导入的对象数等于或超过此限制，则包将作为包存储。从快速导入存储包可以使导入操作更快完成，尤其是在慢速文件系统上。如果未设置，则使用`transfer.unpackLimit`的值。

```
 fetch.recurseSubmodules 
```

此选项可以设置为布尔值，也可以设置为 _on-demand_。将其设置为布尔值会将 fetch 和 pull 的行为更改为无条件地在设置为 true 时递归到子模块或在设置为 false 时完全不递归。当设置为 _on-demand_（默认值）时，fetch 和 pull 将仅在其超级项目检索更新子模块引用的提交时递归到填充的子模块中。

```
 fetch.fsckObjects 
```

如果设置为 true，git-fetch-pack 将检查所有获取的对象。有关已检查的内容，请参阅`transfer.fsckObjects`。默认为 false。如果未设置，则使用`transfer.fsckObjects`的值。

```
 fetch.fsck.<msg-id> 
```

行为类似`fsck.<msg-id>`，但由 [git-fetch-pack [1]](https://git-scm.com/docs/git-fetch-pack) 代替 [git-fsck [1]](https://git-scm.com/docs/git-fsck) 使用。有关详细信息，请参阅`fsck.<msg-id>`文档。

```
 fetch.fsck.skipList 
```

行为类似`fsck.skipList`，但由 [git-fetch-pack [1]](https://git-scm.com/docs/git-fetch-pack) 代替 [git-fsck [1]](https://git-scm.com/docs/git-fsck) 使用。有关详细信息，请参阅`fsck.skipList`文档。

```
 fetch.unpackLimit 
```

如果通过 Git 本机传输获取的对象数低于此限制，则对象将解压缩为松散的对象文件。但是，如果接收到的对象的数量等于或超过此限制，则在添加任何丢失的 delta 基础之后，接收的包将作为包存储。从推送中存储包可以使推送操作更快完成，尤其是在慢速文件系统上。如果未设置，则使用`transfer.unpackLimit`的值。

```
 fetch.prune 
```

如果为 true，则 fetch 将自动表现为在命令行上给出`--prune`选项。另请参阅`remote.<name>.prune`和 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 的 PRUNING 部分。

```
 fetch.pruneTags 
```

如果为 true，则 fetch 将自动表现为修剪时提供`refs/tags/*:refs/tags/*` refspec，如果尚未设置的话。这允许设置此选项和`fetch.prune`以保持与上游引用的 1 = 1 映射。另请参见[HTD0] git-fetch [1] 的`remote.<name>.pruneTags`和 PRUNING 部分。

```
 fetch.output 
```

控制如何打印 ref 更新状态。有效值为`full`和`compact`。默认值为`full`。有关详细信息，请参见 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 中的 OUTPUT 部分。

```
 fetch.negotiationAlgorithm 
```

控制在协商服务器发送的包文件的内容时如何发送有关本地存储库中的提交的信息。设置为“skipping”以使用跳过提交的算法以便更快地收敛，但可能导致大于必要的 packfile;默认值为“default”，它指示 Git 使用从不跳过提交的默认算法（除非服务器已确认它或其后代之一）。未知值将导致 _git fetch_ 错误输出。

另请参见 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 的`--negotiation-tip`选项。

```
 format.attach 
```

启用多部分/混合附件作为 _format-patch_ 的默认设置。该值也可以是双引号字符串，它将启用附件作为默认值，并将值设置为边界。请参阅 [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 中的--attach 选项。

```
 format.from 
```

提供格式化补丁的`--from`选项的默认值。接受布尔值，或名称和电子邮件地址。如果为 false，则 format-patch 默认为`--no-from`，直接在修补程序邮件的“发件人：”字段中使用提交作者。如果为 true，则 format-patch 默认为`--from`，在补丁邮件的“From：”字段中使用您的提交者标识，如果不同，则在补丁邮件正文中包含“From：”字段。如果设置为非布尔值，则 format-patch 使用该值而不是您的提交者标识。默认为 false。

```
 format.numbered 
```

一个布尔值，可以启用或禁用补丁主题中的序列号。它默认为“auto”，仅当有多个补丁时启用它。可以通过将所有消息设置为“true”或“false”来启用或禁用它。请参见 [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 中的--numbered 选项。

```
 format.headers 
```

要通过邮件提交的修补程序中包含的其他电子邮件标头。见 [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 。

```
 format.to 
 format.cc 
```

其他收件人包含在通过邮件提交的补丁中。请参阅 [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 中的--to 和--cc 选项。

```
 format.subjectPrefix 
```

format-patch 的默认设置是输出带有 _[PATCH]_ 主题前缀的文件。使用此变量更改该前缀。

```
 format.signature 
```

format-patch 的默认设置是输出包含 Git 版本号的签名。使用此变量可更改该默认值。将此变量设置为空字符串（“”）以抑制签名生成。

```
 format.signatureFile 
```

与 format.signature 类似，但此变量指定的文件内容将用作签名。

```
 format.suffix 
```

format-patch 的默认设置是输出后缀为`.patch`的文件。使用此变量更改该后缀（如果需要，请确保包含点）。

```
 format.pretty 
```

log/show/whatchanged 命令的默认漂亮格式，参见 [git-log [1]](https://git-scm.com/docs/git-log) ， [git-show [1]](https://git-scm.com/docs/git-show) ， [git -whatchanged [1]](https://git-scm.com/docs/git-whatchanged) 。

```
 format.thread 
```

_git format-patch_ 的默认线程样式。可以是布尔值，也可以是`shallow`或`deep`。 `shallow`线程使每个邮件都回复到系列的头部，其中头部是从求职信，`--in-reply-to`和第一个补丁邮件中按顺序选择的。 `deep`线程使每封邮件都回复上一封邮件。 true 布尔值与`shallow`相同，false 值禁用线程。

```
 format.signOff 
```

一个布尔值，允许您默认启用 format-patch 的`-s/--signoff`选项。 **注意：**将 Signed-off-by：行添加到补丁应该是一种有意识的行为，这意味着您证明您有权在相同的开源许可下提交此作品。有关进一步的讨论，请参阅 _SubmittingPatches_ 文档。

```
 format.coverLetter 
```

一个布尔值，控制在调用 format-patch 时是否生成封面字母，但另外可以设置为“auto”，仅在有多个补丁时生成封面字母。

```
 format.outputDirectory 
```

设置自定义目录以存储生成的文件而不是当前工作目录。

```
 format.useAutoBase 
```

一个布尔值，允许您默认启用 format-patch 的`--base=auto`选项。

```
 filter.<driver>.clean 
```

用于在签入时将工作树文件的内容转换为 blob 的命令。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 filter.<driver>.smudge 
```

该命令用于在结帐时将 blob 对象的内容转换为 worktree 文件。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 fsck.<msg-id> 
```

在 fsck 期间，git 可能会发现遗留数据的问题，这些问题不会被当前版本的 git 生成，如果设置了`transfer.fsckObjects`，则不会通过网络发送。此功能旨在支持使用包含此类数据的旧存储库。

设置`fsck.<msg-id> `将由 [git-fsck [1]](https://git-scm.com/docs/git-fsck) 选取，但要接受推送此类数据集`receive.fsck.<msg-id> `，或者克隆或获取它设置`fetch.fsck.<msg-id> `。

文档的其余部分为了简洁起见讨论了`fsck.*`，但同样适用于相应的`receive.fsck.*`和`fetch.<msg-id> .*`。变量。

与`color.ui`和`core.editor`等变量不同，`receive.fsck.<msg-id> `和`fetch.fsck.<msg-id> `变量如果未设置则不会回退到`fsck.<msg-id> `配置。要在不同情况下统一配置相同的 fsck 设置，所有这三个设置都必须设置为相同的值。

设置`fsck.<msg-id> `时，可以通过配置`fsck.<msg-id> `设置将错误切换为警告，反之亦然，其中`<msg-id> `是 fsck 消息 ID，值为`error`，`warn`或`ignore`之一。为方便起见，fsck 使用消息 ID 作为错误/警告的前缀，例如： “missingEmail：无效的作者/提交者行 - 缺少电子邮件”意味着设置`fsck.missingEmail = ignore`将隐藏该问题。

+ 一般来说，最好枚举`fsck.skipList`存在问题的现有对象，而不是列出这些有问题的对象共享被忽略的破坏类型，因为后者将允许忽略相同破坏的新实例。

+ 设置未知`fsck.<msg-id> `值将导致 fsck 死亡，但对`receive.fsck.<msg-id> `和`fetch.fsck.<msg-id> `执行相同操作只会导致 git 发出警告。

```
 fsck.skipList 
```

指向已知以非致命方式破坏的对象名称列表（即每行一个未缩写的 SHA-1）的路径，应该被忽略。在 Git 2.20 和更高版本的注释（_＃_）的版本中，空行以及任何前导和尾随空格都将被忽略。除了每行 SHA-1 之外的所有内容都会在旧版本上出错。

尽管早期提交包含可以安全忽略的错误（例如无效的提交者电子邮件地址），但应该接受已建立的项目时此功能非常有用。注意：使用此设置无法跳过损坏的对象。

与`fsck.<msg-id> `类似，此变量具有相应的`receive.fsck.skipList`和`fetch.fsck.skipList`变体。

与`color.ui`和`core.editor`等变量不同，`receive.fsck.skipList`和`fetch.fsck.skipList`变量如果未设置则不会回退到`fsck.skipList`配置。要在不同情况下统一配置相同的 fsck 设置，所有这三个设置都必须设置为相同的值。

旧版本的 Git（2.20 之前）记录了对象名称列表应该排序。这绝不是必需的，对象名称可以按任何顺序出现，但是在读取列表时，我们跟踪列表是否为了内部二进制搜索实现的目的而排序，这可以使用已排序的列表保存自己的一些工作。除非你有一个庞大的列表，否则你没有理由不去预先对列表进行排序。在 Git 版本 2.20 之后使用哈希实现，因此现在没有理由对列表进行预排序。

```
 gc.aggressiveDepth 
```

_git gc -aggressive_ 使用的 delta 压缩算法中使用的深度参数。默认为 50。

```
 gc.aggressiveWindow 
```

_git gc -aggressive_ 使用的 delta 压缩算法中使用的窗口大小参数。默认为 250。

```
 gc.auto 
```

当存储库中存在大约多个松散对象时，`git gc --auto`将打包它们。一些 Porcelain 命令使用此命令不时执行轻量级垃圾收集。默认值为 6700。将此值设置为 0 将禁用它。

```
 gc.autoPackLimit 
```

当存储库中没有标记`*.keep`文件的多个包时，`git gc --auto`会将它们合并为一个更大的包。默认值为 50。将此值设置为 0 将禁用它。

```
 gc.autoDetach 
```

如果系统支持，则立即返回`git gc --auto`并在后台运行。默认为 true。

```
 gc.bigPackThreshold 
```

如果非零，则在运行`git gc`时保留所有大于此限制的包。这与`--keep-base-pack`非常相似，只是保留了满足阈值的所有包，而不仅仅是基本包。默认为零。支持 _k_ ， _m_ 或 _g_ 的通用单位后缀。

请注意，如果保留包的数量大于 gc.autoPackLimit，则忽略此配置变量，将重新打包除基本包之外的所有包。在此之后，包的数量应该低于 gc.autoPackLimit，并且应该再次遵守 gc.bigPackThreshold。

```
 gc.writeCommitGraph 
```

如果为 true，那么当 [git-gc [1]](https://git-scm.com/docs/git-gc) 运行时，gc 将重写提交图文件。当使用 [git-gc [1]](https://git-scm.com/docs/git-gc) _--auto_ 时，如果需要内务处理，则会更新提交图。默认值为 false。有关详细信息，请参阅 [git-commit-graph [1]](https://git-scm.com/docs/git-commit-graph)。

```
 gc.logExpiry 
```

如果文件 gc.log 存在，那么`git gc --auto`将打印其内容并退出状态为零而不是运行，除非该文件超过 _gc.logExpiry_ old。默认为“1.day”。有关指定其值的更多方法，请参见`gc.pruneExpire`。

```
 gc.packRefs 
```

在存储库中运行`git pack-refs`会使其在 1.5.1.2 之前的 Git 版本上通过 HTTP 等哑传输不可克隆。此变量确定 _git gc_ 是否运行`git pack-refs`。这可以设置为`notbare`以在所有非裸存储库中启用它，或者可以将其设置为布尔值。默认值为`true`。

```
 gc.pruneExpire 
```

当 _git gc_ 运行时，它将调用 _prune --expire 2.weeks.ago_ 。使用此配置变量覆盖宽限期。值“now”可用于禁用此宽限期并始终立即修剪不可到达的对象，或者“never”可用于抑制修剪。当 _git gc_ 与写入存储库的另一个进程同时运行时，此功能有助于防止损坏;请参阅 [git-gc [1]](https://git-scm.com/docs/git-gc) 的“注意”部分。

```
 gc.worktreePruneExpire 
```

当 _git gc_ 运行时，它调用 _git worktree prune --expire 3.months.ago_ 。此配置变量可用于设置不同的宽限期。值“now”可以用于禁用宽限期并立即修剪`$GIT_DIR/worktrees`，或者“never”可以用于抑制修剪。

```
 gc.reflogExpire 
```

```
 gc.<pattern>.reflogExpire 
```

_git reflog expire_ 删除比此时更早的 reflog 条目;默认为 90 天。值“now”立即使所有条目到期，并且“never”完全抑制到期。使用“<pattern>” （例如“refs/stash”）设置中间仅适用于与<pattern>匹配的引用。

```
 gc.reflogExpireUnreachable 
```

```
 gc.<pattern>.reflogExpireUnreachable 
```

_git reflog expire_ 删除比此时更早的 reflog 条目，并且无法从当前提示访问;默认为 30 天。值“now”立即使所有条目到期，并且“never”完全抑制到期。使用“<pattern>” （例如，“refs/stash”）在中间，该设置仅适用于与<pattern>匹配的引用。

```
 gc.rerereResolved 
```

当 _git rerere gc_ 运行时，您之前解决的冲突合并的记录将保留这么多天。您还可以使用更易读的“1.month.ago”等。默认值为 60 天。见 [git-rerere [1]](https://git-scm.com/docs/git-rerere) 。

```
 gc.rerereUnresolved 
```

当 _git rerere gc_ 运行时，您未解决的冲突合并的记录将保留这么多天。您还可以使用更易读的“1.month.ago”等。默认值为 15 天。见 [git-rerere [1]](https://git-scm.com/docs/git-rerere) 。

```
 gitcvs.commitMsgAnnotation 
```

将此字符串附加到每个提交消息。设置为空字符串以禁用此功能。默认为“via git-CVS emulator”。

```
 gitcvs.enabled 
```

是否为此存储库启用了 CVS 服务器接口。参见 [git-cvsserver [1]](https://git-scm.com/docs/git-cvsserver) 。

```
 gitcvs.logFile 
```

CVS 服务器接口良好的日志文件的路径...记录各种东西。参见 [git-cvsserver [1]](https://git-scm.com/docs/git-cvsserver) 。

```
 gitcvs.usecrlfattr 
```

如果为 true，服务器将查找文件的行尾转换属性以确定要使用的`-k`模式。如果属性强制 Git 将文件视为文本，则`-k`模式将保留为空，因此 CVS 客户端会将其视为文本。如果它们禁止文本转换，则将使用 _-kb_ 模式设置该文件，该模式将抑制客户端可能执行的任何换行。如果属性不允许确定文件类型，则使用`gitcvs.allBinary`。参见 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 gitcvs.allBinary 
```

如果`gitcvs.usecrlfattr`无法解析要使用的正确 _-kb_ 模式，则使用此选项。如果为 true，则所有未解析的文件将以模式 _-kb_ 发送到客户端。这会导致客户端将它们视为二进制文件，这会抑制任何换行，否则可能会执行此操作。或者，如果将其设置为“guess”，则检查文件的内容以确定它是否为二进制，类似于`core.autocrlf`。

```
 gitcvs.dbName 
```

git-cvsserver 用于缓存从 Git 存储库派生的修订信息的数据库。确切的含义取决于使用的数据库驱动程序，对于 SQLite（这是默认驱动程序），这是一个文件名。支持变量替换（详见 [git-cvsserver [1]](https://git-scm.com/docs/git-cvsserver) ）。不得包含分号（`;`）。默认值：_％Ggitcvs。％m.sqlite_

```
 gitcvs.dbDriver 
```

使用 Perl DBI 驱动程序。您可以在此处为此指定任何可用的驱动程序，但它可能不起作用。 git-cvsserver 使用 _DBD :: SQLite_ 进行测试，报告与 _DBD :: Pg_ 一起使用，报告**不是**与 _DBD 一起使用:: mysql_ 。实验功能。可能不包含双冒号（`:`）。默认值： _SQLite_ 。参见 [git-cvsserver [1]](https://git-scm.com/docs/git-cvsserver) 。

```
 gitcvs.dbUser, gitcvs.dbPass 
```

数据库用户和密码。仅在设置`gitcvs.dbDriver`时有用，因为 SQLite 没有数据库用户和/或密码的概念。 _gitcvs.dbUser_ 支持变量替换（详见 [git-cvsserver [1]](https://git-scm.com/docs/git-cvsserver) ）。

```
 gitcvs.dbTableNamePrefix 
```

数据库表名称前缀。在所使用的任何数据库表的名称前面，允许将单个数据库用于多个存储库。支持变量替换（详见 [git-cvsserver [1]](https://git-scm.com/docs/git-cvsserver) ）。任何非字母字符都将替换为下划线。

除`gitcvs.usecrlfattr`和`gitcvs.allBinary`之外的所有 gitcvs 变量也可以指定为 _gitcvs。<access_method>。<varname>_ （其中 _access_method_ 是“ext”和“pserver”之一）使它们仅适用于给定的访问方法。

```
 gitweb.category 
```

```
 gitweb.description 
```

```
 gitweb.owner 
```

```
 gitweb.url 
```

有关说明，请参见 [gitweb [1]](https://git-scm.com/docs/gitweb) 。

```
 gitweb.avatar 
```

```
 gitweb.blame 
```

```
 gitweb.grep 
```

```
 gitweb.highlight 
```

```
 gitweb.patches 
```

```
 gitweb.pickaxe 
```

```
 gitweb.remote_heads 
```

```
 gitweb.showSizes 
```

```
 gitweb.snapshot 
```

有关说明，请参见 [gitweb.conf [5]](https://git-scm.com/docs/gitweb.conf) 。

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

设置默认匹配行为。使用 _basic_ ，_extended_，_fixed_ 或 _perl_ 的值将启用`--basic-regexp`，`--extended-regexp`，`--fixed-strings` ，或`--perl-regexp`选项相应，而值 _default_ 将返回默认匹配行为。

```
 grep.extendedRegexp 
```

如果设置为 true，则默认启用`--extended-regexp`选项。当`grep.patternType`选项设置为 _default_ 以外的值时，将忽略此选项。

```
 grep.threads 
```

要使用的 grep 工作线程数。有关详细信息，请参阅 [git-grep [1]](https://git-scm.com/docs/git-grep) 中的`grep.threads`。

```
 grep.fallbackToNoIndex 
```

如果设置为 true，如果 git grep 在 git 存储库之外执行，则回退到 git grep --no-index。默认为 false。

```
 gpg.program 
```

在制作或验证 PGP 签名时，使用此自定义程序而不是`$PATH`上的“`gpg`”。程序必须支持与 GPG 相同的命令行界面，即验证分离的签名，运行“`gpg --verify $file - <$signature`”，并且程序预期通过退出代码 0 发出良好的签名，并生成 ASCII -armored 分离签名，“`gpg -bsau $key`”的标准输入被输入要签名的内容，并且程序应该将结果发送到其标准输出。

```
 gpg.format 
```

指定使用`--gpg-sign`进行签名时要使用的密钥格式。默认值为“openpgp”，另一个可能的值为“x509”。

```
 gpg.<format>.program 
```

使用此选项可自定义用于所选签名格式的程序。（参见`gpg.program`和`gpg.format`）`gpg.program`仍可用作`gpg.openpgp.program`的遗留同义词。`gpg.x509.program`的默认值为“gpgsm”。

```
 gui.commitMsgWidth 
```

定义 [git-gui [1]](https://git-scm.com/docs/git-gui) 中提交消息窗口的宽度。 “75”是默认值。

```
 gui.diffContext 
```

指定 [git-gui [1]](https://git-scm.com/docs/git-gui) 对 diff 进行调用时应使用的上下文行数。默认值为“5”。

```
 gui.displayUntracked 
```

确定 [git-gui [1]](https://git-scm.com/docs/git-gui) 是否显示文件列表中未跟踪的文件。默认值为“true”。

```
 gui.encoding 
```

指定用于在 [git-gui [1]](https://git-scm.com/docs/git-gui) 和 [gitk [1]](https://git-scm.com/docs/gitk) 中显示文件内容的默认编码。可以通过为相关文件设置 _encoding_ 属性来覆盖它（参见 [gitattributes [5]](https://git-scm.com/docs/gitattributes) ）。如果未设置此选项，则工具默认为区域设置编码。

```
 gui.matchTrackingBranch 
```

确定使用 [git-gui [1]](https://git-scm.com/docs/git-gui) 创建的新分支是否应默认跟踪具有匹配名称的远程分支。默认值：“false”。

```
 gui.newBranchTemplate 
```

在使用 [git-gui [1]](https://git-scm.com/docs/git-gui) 创建新分支时用作建议名称。

```
 gui.pruneDuringFetch 
```

如果 [git-gui [1]](https://git-scm.com/docs/git-gui) 在执行提取时应修剪远程跟踪分支，则为“true”。默认值为“false”。

```
 gui.trustmtime 
```

确定 [git-gui [1]](https://git-scm.com/docs/git-gui) 是否应该信任文件修改时间戳。默认情况下，时间戳不受信任。

```
 gui.spellingDictionary 
```

指定 [git-gui [1]](https://git-scm.com/docs/git-gui) 中用于拼写检查提交消息的字典。设置为“none”时，拼写检查将关闭。

```
 gui.fastCopyBlame 
```

如果为真， _git gui blame_ 使用`-C`而不是`-C -C`进行原始位置检测。它使大型存储库的责任明显加快，但代价是不太彻底的复制检测。

```
 gui.copyBlameThreshold 
```

指定在 _git gui blame_ 原始位置检测中使用的阈值，以字母数字字符测量。有关复制检测的更多信息，请参阅 [git-blame [1]](https://git-scm.com/docs/git-blame) 手册。

```
 gui.blamehistoryctx 
```

当从 _git gui blame_ 调用`Show History Context`菜单项时，指定在 [gitk [1]](https://git-scm.com/docs/gitk) 中为所选提交显示的历史上下文的半径。如果此变量设置为零，则显示整个历史记录。

```
 guitool.<name>.cmd 
```

指定在调用 [git-gui [1]](https://git-scm.com/docs/git-gui) `Tools`菜单的相应项时要执行的 shell 命令行。每个工具都必须使用此选项。该命令从工作目录的根目录执行，并在环境中接收工具名称`GIT_GUITOOL`，当前所选文件的名称为 _FILENAME_ ，以及当前名称分支为 _CUR_BRANCH_ （如果头被分离， _CUR_BRANCH_ 为空）。

```
 guitool.<name>.needsFile 
```

仅当在 GUI 中选择了差异时才运行该工具。它保证 _FILENAME_ 不为空。

```
 guitool.<name>.noConsole 
```

以静默方式运行命令，而不创建窗口以显示其输出。

```
 guitool.<name>.noRescan 
```

工具完成执行后，请勿重新扫描工作目录以进行更改。

```
 guitool.<name>.confirm 
```

在实际运行该工具之前显示确认对话框。

```
 guitool.<name>.argPrompt 
```

从用户请求字符串参数，并通过`ARGS`环境变量将其传递给工具。由于请求参数意味着确认，如果启用此选项，_ 确认 _ 选项无效。如果该选项设置为 _true_ ， _yes_ 或 _1_ ，则对话框使用内置的通用提示;否则使用变量的确切值。

```
 guitool.<name>.revPrompt 
```

从用户请求单个有效修订，并设置`REVISION`环境变量。在其他方面，该选项类似于 _argPrompt_ ，可以与它一起使用。

```
 guitool.<name>.revUnmerged 
```

在 _revPrompt_ 子菜单中仅显示未合并的分支。这对于类似于 merge 或 rebase 的工具很有用，但对于 checkout 或 reset 之类的东西却没有用。

```
 guitool.<name>.title 
```

指定用于提示对话框的标题。默认值为工具名称。

```
 guitool.<name>.prompt 
```

指定在 _argPrompt_ 和 _revPrompt_ 的子部分之前显示在对话框顶部的常规提示字符串。默认值包括实际命令。

```
 help.browser 
```

指定将用于以 _web_ 格式显示帮助的浏览器。参见 [git-help [1]](https://git-scm.com/docs/git-help) 。

```
 help.format 
```

覆盖 [git-help [1]](https://git-scm.com/docs/git-help) 使用的默认帮助格式。支持值 _man_ ， _info_ ， _web_ 和 _html_ 。 _man_ 是默认值。 _web_ 和 _html_ 是一样的。

```
 help.autoCorrect 
```

等待给定的十分之一秒（0.1 秒）后自动纠正并执行错误的命令。如果可以从输入的文本中推导出多个命令，则不会执行任何操作。如果此选项的值为负，则将立即执行更正的命令。如果值为 0 - 将仅显示命令但不执行。这是默认值。

```
 help.htmlPath 
```

指定 HTML 文档所在的路径。支持文件系统路径和 URL。当帮助以 _web_ 格式显示时，HTML 页面将以此路径为前缀。这默认为 Git 安装的文档路径。

```
 http.proxy 
```

覆盖 HTTP 代理，通常使用 _http_proxy_ ， _https_proxy_ 和 _all_proxy_ 环境变量配置（参见`curl(1)`）。除了 curl 理解的语法之外，还可以指定具有用户名但没有密码的代理字符串，在这种情况下，git 将尝试以与其他凭据相同的方式获取一个代理字符串。有关详细信息，请参阅 [gitcredentials [7]](https://git-scm.com/docs/gitcredentials) 。因此语法是 _[protocol：//] [user [：password] @] proxyhost [：port]_ 。这可以在每个远程基础上被覆盖;见远程。<name>.proxy

```
 http.proxyAuthMethod 
```

设置用于针对 HTTP 代理进行身份验证的方法。仅当配置的代理字符串包含用户名部分（即 _user @ host_ 或 _user @ host：port_ ）时才会生效。这可以在每个远程基础上被覆盖;见`remote.<name>.proxyAuthMethod`。两者都可以被`GIT_HTTP_PROXY_AUTHMETHOD`环境变量覆盖。可能的值是：

*   `anyauth` - 自动选择合适的身份验证方法。假设代理使用 407 状态代码和一个或多个具有支持的身份验证方法的代理身份验证头来应答未经身份验证的请求。这是默认值。

*   `basic` - HTTP 基本身份验证

*   `digest` - HTTP 摘要认证;这可以防止密码以明文形式传输到代理

*   `negotiate` - GSS-协商认证（比较`curl(1)`的--negotiate 选项）

*   `ntlm` - NTLM 身份验证（比较`curl(1)`的--ntlm 选项）

```
 http.emptyAuth 
```

在不寻求用户名或密码的情况下尝试身份验证。这可以用于尝试 GSS-Negotiate 身份验证而无需在 URL 中指定用户名，因为 libcurl 通常需要用户名进行身份验证。

```
 http.delegation 
```

控制 GSSAPI 凭证委派。默认情况下，自版本 7.21.7 以来，libcurl 中的委派被禁用。设置参数以告知服务器在用户凭据时允许委派的内容。与 GSS/kerberos 一起使用。可能的值是：

*   `none` - 不允许任何授权。

*   `policy` - 当且仅当在 Kerberos 服务票证中设置了 OK-AS-DELEGATE 标志时才委派，这是领域政策的问题。

*   `always` - 无条件地允许服务器委派。

```
 http.extraHeader 
```

与服务器通信时传递其他 HTTP 标头。如果存在多个此类条目，则将所有条目添加为额外标头。要允许覆盖从系统配置继承的设置，空值会将额外标头重置为空列表。

```
 http.cookieFile 
```

包含先前存储的 cookie 行的文件的路径名，如果它们与服务器匹配，则应在 Git http 会话中使用。从中读取 cookie 的文件的文件格式应该是纯 HTTP 标头或 Netscape/Mozilla cookie 文件格式（参见`curl(1)`）。请注意，除非设置了 http.saveCookies，否则使用 http.cookieFile 指定的文件仅用作输入。

```
 http.saveCookies 
```

如果设置，请将请求期间收到的 cookie 存储到 http.cookieFile 指定的文件中。如果未设置 http.cookieFile，则无效。

```
 http.version 
```

与服务器通信时使用指定的 HTTP 协议版本。如果要强制使用默认值。可用和默认版本取决于 libcurl。实际上，此选项的可能值为：

*   HTTP / 2

*   HTTP / 1.1

```
 http.sslVersion 
```

如果要强制使用默认值，则在协商 SSL 连接时使用的 SSL 版本。可用和默认版本取决于 libcurl 是针对 NSS 还是 OpenSSL 构建的，以及正在使用的加密库的特定配置。在内部设置 _CURLOPT_SSL_VERSION_ 选项;有关此选项的格式以及支持的 ssl 版本的更多详细信息，请参阅 libcurl 文档。实际上，此选项的可能值为：

*   SSLV2

*   SSLV3

*   使用 TLSv1

*   tlsv1.0

*   tlsv1.1

*   TLSv1.2 工作

*   tlsv1.3

可以被`GIT_SSL_VERSION`环境变量覆盖。要强制 git 使用 libcurl 的默认 ssl 版本并忽略任何显式的 http.sslversion 选项，请将`GIT_SSL_VERSION`设置为空字符串。

```
 http.sslCipherList 
```

协商 SSL 连接时使用的 SSL 密码列表。可用的密码取决于 libcurl 是针对 NSS 还是 OpenSSL 构建的，以及正在使用的加密库的特定配置。在内部设置 _CURLOPT_SSL_CIPHER_LIST_ 选项;有关此列表格式的更多详细信息，请参阅 libcurl 文档。

可以被`GIT_SSL_CIPHER_LIST`环境变量覆盖。要强制 git 使用 libcurl 的默认密码列表并忽略任何显式的 http.sslCipherList 选项，请将`GIT_SSL_CIPHER_LIST`设置为空字符串。

```
 http.sslVerify 
```

获取或推送 HTTPS 时是否验证 SSL 证书。默认为 true。可以被`GIT_SSL_NO_VERIFY`环境变量覆盖。

```
 http.sslCert 
```

获取或推送 HTTPS 时包含 SSL 证书的文件。可以被`GIT_SSL_CERT`环境变量覆盖。

```
 http.sslKey 
```

获取或推送 HTTPS 时包含 SSL 私钥的文件。可以被`GIT_SSL_KEY`环境变量覆盖。

```
 http.sslCertPasswordProtected 
```

为 SSL 证书启用 Git 密码提示。否则，如果证书或私钥被加密，OpenSSL 可能会多次提示用户。可以被`GIT_SSL_CERT_PASSWORD_PROTECTED`环境变量覆盖。

```
 http.sslCAInfo 
```

包含证书的文件，用于在获取或推送 HTTPS 时验证对等方。可以被`GIT_SSL_CAINFO`环境变量覆盖。

```
 http.sslCAPath 
```

包含带有 CA 证书的文件的路径，用于在获取或推送 HTTPS 时验证对等方。可以被`GIT_SSL_CAPATH`环境变量覆盖。

```
 http.sslBackend 
```

要使用的 SSL 后端的名称（例如“openssl”或“schannel”）。如果 cURL 缺乏在运行时选择 SSL 后端的支持，则忽略此选项。

```
 http.schannelCheckRevoke 
```

用于在 http.sslBackend 设置为“schannel”时强制执行或禁用 cURL 中的证书吊销检查。如果未设置，则默认为`true`。只有在 Git 始终出错并且消息是关于检查证书的撤销状态时才需要禁用此功能。如果 cURL 缺少在运行时设置相关 SSL 选项的支持，则忽略此选项。

```
 http.schannelUseSSLCAInfo 
```

从 cURL v7.60.0 开始，安全通道后端可以使用通过`http.sslCAInfo`提供的证书包，但这会覆盖 Windows 证书存储区。由于默认情况下不需要这样做，因此当`schannel`后端通过`http.sslBackend`配置时，Git 会告诉 cURL 默认不使用该捆绑包，除非`http.schannelUseSSLCAInfo`覆盖此行为。

```
 http.pinnedpubkey 
```

https 服务的公钥。它可以是 PEM 或 DER 编码的公钥文件的文件名，也可以是以 _sha256 //_ 开头的字符串，后跟公钥的 base64 编码 sha256 哈希。另请参见 libcurl _CURLOPT_PINNEDPUBLICKEY_ 。如果设置了此选项但 cURL 不支持，则 git 将退出并显示错误。

```
 http.sslTry 
```

通过常规 FTP 协议连接时，尝试使用 AUTH SSL / TLS 和加密数据传输。如果 FTP 服务器出于安全原因需要它，或者您希望在远程 FTP 服务器支持时安全地连接，则可能需要这样做。默认值为 false，因为它可能会在配置错误的服务器上触发证书验证错误。

```
 http.maxRequests 
```

并行启动多少 HTTP 请求。可以被`GIT_HTTP_MAX_REQUESTS`环境变量覆盖。默认值为 5。

```
 http.minSessions 
```

在请求之间保留的卷曲会话数（跨插槽计数）。在调用 http_cleanup（）之前，它们不会以 curl_easy_cleanup（）结束。如果未定义 USE_CURL_MULTI，则此值的上限为 1.默认为 1。

```
 http.postBuffer 
```

将数据发送到远程系统时，智能 HTTP 传输使用的缓冲区的最大大小（以字节为单位）。对于大于此缓冲区大小的请求，使用 HTTP / 1.1 和 Transfer-Encoding：chunked 来避免在本地创建大量包文件。默认值为 1 MiB，足以满足大多数请求。

```
 http.lowSpeedLimit, http.lowSpeedTime 
```

如果 HTTP 传输速度小于 _http.lowSpeedLimit_ 的时间超过 _http.lowSpeedTime_ 秒，则传输中止。可以被`GIT_HTTP_LOW_SPEED_LIMIT`和`GIT_HTTP_LOW_SPEED_TIME`环境变量覆盖。

```
 http.noEPSV 
```

一个布尔值，禁用 curl 使用 EPSV ftp 命令。这对于一些不支持 EPSV 模式的“差”ftp 服务器很有帮助。可以被`GIT_CURL_FTP_NO_EPSV`环境变量覆盖。默认值为 false（curl 将使用 EPSV）。

```
 http.userAgent 
```

HTTP USER_AGENT 字符串呈现给 HTTP 服务器。默认值表示客户端 Git 的版本，例如 git / 1.7.1。此选项允许您将此值覆盖为更常见的值，例如 Mozilla / 4.0。例如，如果通过防火墙连接将 HTTP 连接限制为一组常见的 USER_AGENT 字符串（但不包括 git / 1.7.1 之类的字符串），则可能需要这样做。可以被`GIT_HTTP_USER_AGENT`环境变量覆盖。

```
 http.followRedirects 
```

git 是否应该遵循 HTTP 重定向。如果设置为`true`，git 将透明地遵循它遇到的服务器发出的任何重定向。如果设置为`false`，git 会将所有重定向视为错误。如果设置为`initial`，git 将仅针对对远程的初始请求进行重定向，但不会对后续的后续 HTTP 请求进行重定向。由于 git 使用重定向的 URL 作为后续请求的基础，因此这通常就足够了。默认值为`initial`。

```
 http.<url>.* 
```

上面的任何 http。*选项都可以有选择地应用于某些 URL。对于匹配 URL 的配置键，将按以下顺序将配置键的每个元素与 URL 的元素进行比较：

1.  方案（例如`https://example.com/`中的`https`）。该字段必须与配置密钥和 URL 完全匹配。

2.  主机/域名（例如`https://example.com/`中的`example.com`）。该字段必须在配置密钥和 URL 之间匹配。可以将`*`指定为主机名的一部分，以匹配此级别的所有子域。 `https://*.example.com/`例如匹配`https://foo.example.com/`，但不匹配`https://foo.bar.example.com/`。

3.  端口号（例如`http://example.com:8080/`中的`8080`）。该字段必须与配置密钥和 URL 完全匹配。在匹配之前，省略的端口号会自动转换为方案的正确默认值。

4.  路径（例如，`https://example.com/repo.git`中的`repo.git`）。配置密钥的路径字段必须与 URL 的路径字段完全匹配，或者与斜杠分隔的路径元素的前缀匹配。这意味着路径`foo/`的配置密钥与 URL 路径`foo/bar`匹配。前缀只能在斜杠（`/`）边界上匹配。较长的匹配优先（因此具有路径`foo/bar`的配置键与 URL 路径`foo/bar`比仅具有路径`foo/`的配置键更好地匹配）。

5.  用户名（例如`https://user@example.com/repo.git`中的`user`）。如果配置密钥具有用户名，则它必须与 URL 中的用户名完全匹配。如果配置密钥没有用户名，则该配置密钥将匹配具有任何用户名（包括无）的 URL，但优先级低于具有用户名的配置密钥。

上面的列表按优先顺序排序;与配置密钥的路径匹配的 URL 优先于与其用户名匹配的 URL。例如，如果 URL 为`https://user@example.com/foo/bar`，则`https://example.com/foo`的配置密钥匹配将优先于`https://user@example.com`的配置密钥匹配。

在尝试任何匹配之前，所有 URL 都会进行规范化（密码部分，如果嵌入在 URL 中，为了匹配目的总是被忽略），以便简单拼写的等效 URL 将正确匹配。环境变量设置始终覆盖任何匹配项。匹配的 URL 是直接给予 Git 命令的 URL。这意味着由于重定向而访问的任何 URL 都不参与匹配。

```
 i18n.commitEncoding 
```

编码提交消息的字符存储在; Git 本身并不关心，但这些信息是必要的，例如从电子邮件或 gitk 图形历史浏览器（以及可能在将来的其他地方或其他瓷器中）导入提交时。参见例如 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) 。默认为 _utf-8_ 。

```
 i18n.logOutputEncoding 
```

运行 _git log_ 和朋友时，编码提交消息的字符将转换为。

```
 imap.folder 
```

要删除邮件的文件夹，通常是“草稿”文件夹。例如：“INBOX.Drafts”，“INBOX / Drafts”或“[Gmail] / Drafts”。需要。

```
 imap.tunnel 
```

用于设置到 IMAP 服务器的隧道的命令，通过该隧道将管道命令而不是使用到服务器的直接网络连接。未设置 imap.host 时需要。

```
 imap.host 
```

标识服务器的 URL。使用`imap://`前缀进行非安全连接，使用`imaps://`前缀进行安全连接。设置 imap.tunnel 时忽略，否则需要。

```
 imap.user 
```

登录服务器时使用的用户名。

```
 imap.pass 
```

登录服务器时使用的密码。

```
 imap.port 
```

要连接到服务器的整数端口号。对于 imap：// hosts，默认值为 143;对于 imaps：// hosts，默认值为 993。设置 imap.tunnel 时忽略。

```
 imap.sslverify 
```

一个布尔值，用于启用/禁用 SSL / TLS 连接使用的服务器证书的验证。默认值为`true`。设置 imap.tunnel 时忽略。

```
 imap.preformattedHTML 
```

发送补丁时启用/禁用 html 编码的布尔值。 html 编码的补丁将用<pre>括起来。并具有 text / html 的内容类型。具有讽刺意味的是，启用此选项会导致 Thunderbird 将补丁作为普通/文本格式=固定电子邮件发送。默认值为`false`。

```
 imap.authMethod 
```

使用 IMAP 服务器指定身份验证的身份验证方法。如果 Git 是使用 NO_CURL 选项构建的，或者如果你的 curl 版本早于 7.34.0，或者你正在使用`--no-curl`选项运行 git-imap-send，那么唯一支持的方法是 _CRAM-MD5_ 。如果未设置，则 _git imap-send_ 使用基本的 IMAP 明文 LOGIN 命令。

```
 index.recordEndOfIndexEntries 
```

指定索引文件是否应包含“结束索引条目”部分。这减少了多处理器计算机上的索引加载时间，但在使用 2.20 之前的 Git 版本读取索引时会产生“忽略 EOIE 扩展”的消息。如果已明确启用 index.threads，则默认为 _true_ ，否则为 _false_ 。

```
 index.recordOffsetTable 
```

指定索引文件是否应包含“索引条目偏移表”部分。这减少了多处理器计算机上的索引加载时间，但在使用 2.20 之前的 Git 版本读取索引时会产生消息“忽略 IEOT 扩展”。如果已明确启用 index.threads，则默认为 _true_ ，否则为 _false_ 。

```
 index.threads 
```

指定加载索引时要生成的线程数。这意味着减少多处理器计算机上的索引加载时间。指定 0 或 _true_ 将导致 Git 自动检测 CPU 的数量并相应地设置线程数。指定 1 或 _false_ 将禁用多线程。默认为 _true_ 。

```
 index.version 
```

指定应初始化新索引文件的版本。这不会影响现有存储库。

```
 init.templateDir 
```

指定将从中复制模板的目录。 （参见 [git-init [1]](https://git-scm.com/docs/git-init) 的“TEMPLATE DIRECTORY”部分。）

```
 instaweb.browser 
```

指定将用于在 gitweb 中浏览工作存储库的程序。见 [git-instaweb [1]](https://git-scm.com/docs/git-instaweb) 。

```
 instaweb.httpd 
```

用于在工作存储库上启动 gitweb 的 HTTP 守护程序命令行。见 [git-instaweb [1]](https://git-scm.com/docs/git-instaweb) 。

```
 instaweb.local 
```

如果为[true]，由 [git-instaweb [1]](https://git-scm.com/docs/git-instaweb) 启动的 Web 服务器将绑定到本地 IP（127.0.0.1）。

```
 instaweb.modulePath 
```

[git-instaweb [1]](https://git-scm.com/docs/git-instaweb) 的默认模块路径，而不是/usr/lib /apache2/modules。仅在 httpd 为 Apache 时使用。

```
 instaweb.port 
```

用于绑定 gitweb httpd 的端口号。见 [git-instaweb [1]](https://git-scm.com/docs/git-instaweb) 。

```
 interactive.singleKey 
```

在交互式命令中，允许用户使用单个键提供单字母输入（即，不按 Enter 键）。目前这被 [git-add [1]](https://git-scm.com/docs/git-add) ， [git-checkout [1]](https://git-scm.com/docs/git-checkout) ， [git-commit [1]](https://git-scm.com/docs/git-commit) 的`--patch`模式使用， [git-reset [1]](https://git-scm.com/docs/git-reset) 和 [git-stash [1]](https://git-scm.com/docs/git-stash) 。请注意，如果便携式击键输入不可用，则会自动忽略此设置;需要 Perl 模块 Term :: ReadKey。

```
 interactive.diffFilter 
```

当交互式命令（例如`git add --patch`）显示彩色差异时，git 将通过此配置变量定义的 shell 命令管道 diff。该命令可以进一步标记差异用于人类消费，只要它保持与原始差异中的线一一对应。默认为禁用（无过滤）。

```
 log.abbrevCommit 
```

如果为真，则 [git-log [1]](https://git-scm.com/docs/git-log) ， [git-show [1]](https://git-scm.com/docs/git-show) 和 [git -whatchanged [1]](https://git-scm.com/docs/git-whatchanged) 假定`--abbrev-commit`。您可以使用`--no-abbrev-commit`覆盖此选项。

```
 log.date 
```

设置 _log_ 命令的默认日期时间模式。设置 log.date 的值类似于使用 _git log_ 的`--date`选项。有关详细信息，请参阅 [git-log [1]](https://git-scm.com/docs/git-log) 。

```
 log.decorate 
```

打印出 log 命令显示的任何提交的引用名称。如果指定 _ 短 _，则引用名称前缀 _refs / heads /_ ， _refs / tags /_ 和 _refs / remotes /_ 将不会打印。如果指定了 _full_ ，将打印完整的引用名称（包括前缀）。如果指定了 _auto_ ，那么如果输出到达终端，则 ref 名称显示为 _short_ ，否则不显示 ref 名称。这与`git log`的`--decorate`选项相同。

```
 log.follow 
```

如果`true`，`git log`将像单个<path>一样使用`--follow`选项。给出。这与`--follow`具有相同的限制，即它不能用于跟踪多个文件，并且在非线性历史记录上不能很好地工作。

```
 log.graphColors 
```

由逗号分隔的颜色列表，可用于在`git log --graph`中绘制历史记录行。

```
 log.showRoot 
```

如果为 true，则初始提交将显示为大型创建事件。这相当于对空树的差异。像 [git-log [1]](https://git-scm.com/docs/git-log) 或 [git -whatchanged [1]](https://git-scm.com/docs/git-whatchanged) 这样的工具，通常会隐藏根提交，现在将显示它。默认为 True。

```
 log.showSignature 
```

如果为真，则 [git-log [1]](https://git-scm.com/docs/git-log) ， [git-show [1]](https://git-scm.com/docs/git-show) 和 [git -whatchanged [1]](https://git-scm.com/docs/git-whatchanged) 假定`--show-signature`。

```
 log.mailmap 
```

如果为真，则 [git-log [1]](https://git-scm.com/docs/git-log) ， [git-show [1]](https://git-scm.com/docs/git-show) 和 [git -whatchanged [1]](https://git-scm.com/docs/git-whatchanged) 假定`--use-mailmap`。

```
 mailinfo.scissors 
```

如果为 true，则默认情况下 [git-mailinfo [1]](https://git-scm.com/docs/git-mailinfo) （因此 [git-am [1]](https://git-scm.com/docs/git-am) ）会起作用，就像在命令行中提供了--scissors 选项一样。当激活时，该特征在剪刀线之前从消息体中移除所有内容（即主要由“> 8”，“8<”和“ - ”组成）。

```
 mailmap.file 
```

扩充邮件地图文件的位置。首先加载位于存储库根目录中的默认邮件映射，然后加载此变量指向的邮件映射文件。 mailmap 文件的位置可以位于存储库子目录中，也可以位于存储库本身之外的某个位置。参见 [git-shortlog [1]](https://git-scm.com/docs/git-shortlog) 和 [git-blame [1]](https://git-scm.com/docs/git-blame) 。

```
 mailmap.blob 
```

与`mailmap.file`类似，但请将该值视为存储库中 blob 的引用。如果同时给出`mailmap.file`和`mailmap.blob`，则两者都被解析，来自`mailmap.file`的条目优先。在裸存储库中，默认为`HEAD:.mailmap`。在非裸存储库中，它默认为空。

```
 man.viewer 
```

指定可用于以 _man_ 格式显示帮助的程序。参见 [git-help [1]](https://git-scm.com/docs/git-help) 。

```
 man.<tool>.cmd 
```

指定用于调用指定 man 查看器的命令。在 shell 中评估指定的命令，并将 man 页面作为参数传递。 （参见 [git-help [1]](https://git-scm.com/docs/git-help) 。）

```
 man.<tool>.path 
```

覆盖可用于以 _man_ 格式显示帮助的给定工具的路径。参见 [git-help [1]](https://git-scm.com/docs/git-help) 。

```
 merge.conflictStyle 
```

指定在合并时将冲突的行写入工作树文件的样式。默认为“合并”，显示`<<<<<<<`冲突标记，一侧更改，`=======`标记，另一侧更改，然后是`>>>>>>>`标记。另一种样式“diff3”在`=======`标记之前添加`|||||||`标记和原始文本。

```
 merge.defaultToUpstream 
```

如果在没有任何提交参数的情况下调用 merge，则通过使用存储在其远程跟踪分支中的最后观察值来合并为当前分支配置的上游分支。查询`branch.<current branch>.remote`命名的远程分支的`branch.<current branch>.merge`值，然后通过`remote.<remote>.fetch`将它们映射到相应的远程跟踪分支，并合并这些跟踪分支的提示。

```
 merge.ff 
```

默认情况下，Git 在合并作为当前提交的后代的提交时不会创建额外的合并提交。相反，当前分支的提示是快进的。当设置为`false`时，此变量告诉 Git 在这种情况下创建额外的合并提交（相当于从命令行提供`--no-ff`选项）。设置为`only`时，仅允许此类快进合并（相当于从命令行提供`--ff-only`选项）。

```
 merge.verifySignatures 
```

如果为 true，则等效于--verify-signatures 命令行选项。有关详细信息，请参阅 [git-merge [1]](https://git-scm.com/docs/git-merge) 。

```
 merge.branchdesc 
```

除分支名称外，还使用与其关联的分支描述文本填充日志消息。默认为 false。

```
 merge.log 
```

除了分支名称之外，还要从正在合并的实际提交中填充最多具有指定数量的单行描述的日志消息。默认为 false，true 是 20 的同义词。

```
 merge.renameLimit 
```

合并期间执行重命名检测时要考虑的文件数;如果未指定，则默认为 diff.renameLimit 的值。如果关闭重命名检测，此设置无效。

```
 merge.renames 
```

Git 是否以及如何检测重命名。如果设置为“false”，则禁用重命名检测。如果设置为“true”，则启用基本重命名检测。默认为 diff.renames 的值。

```
 merge.renormalize 
```

告诉 Git，存储库中文件的规范表示随着时间的推移而发生了变化（例如，早期的提交记录了带有 CRLF 行结尾的文本文件，但最近提交了使用 LF 行结尾的文本文件）。在这样的存储库中，Git 可以在执行合并之前将提交中记录的数据转换为规范形式，以减少不必要的冲突。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 中的“合并具有不同签入/签出属性的分支”部分。

```
 merge.stat 
```

是否在合并结束时打印 ORIG_HEAD 和合并结果之间的 diffstat。默认为 True。

```
 merge.tool 
```

控制 [git-mergetool [1]](https://git-scm.com/docs/git-mergetool) 使用的合并工具。下面的列表显示了有效的内置值。任何其他值都被视为自定义合并工具，并且需要定义相应的 mergetool.<tool>.cmd 变量。

```
 merge.guitool 
```

当指定-g / - gui 标志时，控制 [git-mergetool [1]](https://git-scm.com/docs/git-mergetool) 使用哪个合并工具。下面的列表显示了有效的内置值。任何其他值都被视为自定义合并工具，并且需要定义相应的 mergetool.<guitool>.cmd 变量。

*   araxis

*   bc

*   BC3

*   codecompare

*   deltawalker

*   diffmerge

*   diffuse

*   ecmerge

*   emerge

*   examdiff

*   guiffy

*   gvimdiff

*   gvimdiff2

*   gvimdiff3

*   kdiff3

*   meld

*   opendiff

*   p4merge

*   tkdiff

*   tortoisemerge

*   vimdiff

*   vimdiff2

*   vimdiff3

*   winmerge

*   xxdiff

```
 merge.verbosity 
```

控制递归合并策略显示的输出量。如果检测到冲突，则 0 级除了最终错误消息外不输出任何内容。级别 1 仅输出冲突，输出 2 个冲突和文件更改。 5 级及以上输出调试信息。默认值为 2 级。可以被`GIT_MERGE_VERBOSITY`环境变量覆盖。

```
 merge.<driver>.name 
```

为自定义低级合并驱动程序定义一个人类可读的名称。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 merge.<driver>.driver 
```

定义实现自定义低级合并驱动程序的命令。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 merge.<driver>.recursive 
```

命名在共同祖先之间执行内部合并时使用的低级合并驱动程序。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 mergetool.<tool>.path 
```

覆盖给定工具的路径。如果您的工具不在 PATH 中，这非常有用。

```
 mergetool.<tool>.cmd 
```

指定用于调用指定合并工具的命令。在 shell 中使用以下变量评估指定的命令： _BASE_ 是包含要合并的文件的公共基础的临时文件的名称（如果可用）; _LOCAL_ 是包含当前分支上文件内容的临时文件的名称; _REMOTE_ 是一个临时文件的名称，包含正在合并的分支的文件内容; _MERGED_ 包含合并工具应写入成功合并结果的文件的名称。

```
 mergetool.<tool>.trustExitCode 
```

对于自定义合并命令，请指定是否可以使用合并命令的退出代码来确定合并是否成功。如果未设置为 true，则检查合并目标文件时间戳，如果文件已更新，则假定合并已成功，否则将提示用户指示合并成功。

```
 mergetool.meld.hasOutput 
```

较旧版本的`meld`不支持`--output`选项。 Git 将通过检查`meld --help`的输出来尝试检测`meld`是否支持`--output`。配置`mergetool.meld.hasOutput`将使 Git 跳过这些检查并改为使用配置的值。将`mergetool.meld.hasOutput`设置为`true`告诉 Git 无条件使用`--output`选项，`false`避免使用`--output`。

```
 mergetool.keepBackup 
```

执行合并后，带有冲突标记的原始文件可以保存为具有`.orig`扩展名的文件。如果此变量设置为`false`，则不保留此文件。默认为`true`（即保留备份文件）。

```
 mergetool.keepTemporaries 
```

在调用自定义合并工具时，Git 使用一组临时文件传递给该工具。如果工具返回错误并且此变量设置为`true`，则将保留这些临时文件，否则在工具退出后将删除它们。默认为`false`。

```
 mergetool.writeToTemp 
```

Git 默认在工作树中写入临时 _BASE_ ， _LOCAL_ 和 _REMOTE_ 版本的冲突文件。设置`true`时，Git 将尝试为这些文件使用临时目录。默认为`false`。

```
 mergetool.prompt 
```

在每次调用合并解决方案之前提示。

```
 notes.mergeStrategy 
```

在解决笔记冲突时默认选择哪种合并策略。必须是`manual`，`ours`，`theirs`，`union`或`cat_sort_uniq`中的一个。默认为`manual`。有关每种策略的更多信息，请参阅 [git-notes [1]](https://git-scm.com/docs/git-notes) 的“NOTES MERGE STRATEGIES”部分。

```
 notes.<name>.mergeStrategy 
```

在将注释合并到 refs / notes /< name>中时选择哪种合并策略。这会覆盖更一般的“notes.mergeStrategy”。有关可用策略的更多信息，请参阅 [git-notes [1]](https://git-scm.com/docs/git-notes) 中的“NOTES MERGE STRATEGIES”部分。

```
 notes.displayRef 
```

（完全限定的）refname，用于在显示提交消息时显示注释。此变量的值可以设置为 glob，在这种情况下，将显示来自所有匹配引用的注释。您也可以多次指定此配置变量。将为不存在的引用发出警告，但是会自动忽略与任何引用不匹配的 glob。

可以使用`GIT_NOTES_DISPLAY_REF`环境变量覆盖此设置，该变量必须是以冒号分隔的 ref 或 glob 列表。

“core.notesRef”的有效值（可能被 GIT_NOTES_REF 覆盖）也隐式添加到要显示的引用列表中。

```
 notes.rewrite.<command> 
```

使用<command>重写提交时（当前`amend`或`rebase`）并且此变量设置为`true`，Git 会自动将您的笔记从原始文件复制到重写的提交。默认为`true`，但请参阅下面的“notes.rewriteRef”。

```
 notes.rewriteMode 
```

在重写期间复制备注时（请参阅“notes.rewrite.<command>”选项），确定目标提交已有备注时要执行的操作。必须是`overwrite`，`concatenate`，`cat_sort_uniq`或`ignore`中的一个。默认为`concatenate`。

可以使用`GIT_NOTES_REWRITE_MODE`环境变量覆盖此设置。

```
 notes.rewriteRef 
```

在重写期间复制注释时，指定应复制其注释的（完全限定的）引用。ref 可以是 glob，在这种情况下，将复制所有匹配引用中的注释。您也可以多次指定此配置。

没有默认值;您必须配置此变量以启用注释重写。将其设置为`refs/notes/commits`以启用默认提交注释的重写。

可以使用`GIT_NOTES_REWRITE_REF`环境变量覆盖此设置，该变量必须是以冒号分隔的 ref 或 glob 列表。

```
 pack.window 
```

在命令行上没有给出窗口大小时， [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) 使用的窗口大小。默认为 10。

```
 pack.depth 
```

当命令行没有给出最大深度时， [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) 使用的最大增量深度。默认为 50.最大值为 4095。

```
 pack.windowMemory 
```

当命令行没有给出限制时， [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) 中每个线程为包窗口内存消耗的最大内存大小。该值可以后缀“k”，“m”或“g”。如果未配置（或明确设置为 0），则没有限制。

```
 pack.compression 
```

整数-1..9，表示包文件中对象的压缩级别。 -1 是 zlib 的默认值。 0 表示没有压缩，1..9 是各种速度/大小权衡，9 表示最慢。如果未设置，则默认为 core.compression。如果未设置，则默认为-1，即 zlib 默认值，即“速度和压缩之间的默认折衷（当前等效于级别 6）”。

请注意，更改压缩级别不会自动重新压缩所有现有对象。您可以通过将-F 选项传递给 [git-repack [1]](https://git-scm.com/docs/git-repack) 来强制重新压缩。

```
 pack.island 
```

扩展正则表达式，用于配置一组 delta 岛。有关详细信息，请参见 [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) 中的“DELTA ISLANDS”。

```
 pack.islandCore 
```

指定要首先打包其对象的岛名称。这会在一个包的前面创建一种伪包，这样来自指定岛的对象有望更快地复制到应该提供给请求这些对象的用户的任何包中。在实践中，这意味着指定的岛屿应该可能对应于回购中最常克隆的岛屿。另请参见 [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) 中的“DELTA ISLANDS”。

```
 pack.deltaCacheSize 
```

在将它们写入包之前，用于缓存 [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) 中的增量的最大内存字节数。此缓存用于加速写入对象阶段，一旦找到所有对象的最佳匹配，就不必重新计算最终的增量结果。尽管如此，在内存紧张的计算机上重新打包大型存储库可能会受到严重影响，特别是如果此缓存将系统推送到交换中。值为 0 表示没有限制。可以使用 1 字节的最小大小来虚拟地禁用该高速缓存。默认为 256 MiB。

```
 pack.deltaCacheLimit 
```

在 [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) 中缓存的 delta 的最大大小。此缓存用于加速写入对象阶段，一旦找到所有对象的最佳匹配，就不必重新计算最终的增量结果。默认为 1000.最大值为 65535。

```
 pack.threads 
```

指定搜索最佳增量匹配时要生成的线程数。这要求使用 pthreads 编译 [git-pack-objects [1]](https://git-scm.com/docs/git-pack-objects) ，否则将忽略此选项并显示警告。这意味着减少多处理器机器的包装时间。但是，增量搜索窗口所需的内存量乘以线程数。指定 0 将导致 Git 自动检测 CPU 的数量并相应地设置线程数。

```
 pack.indexVersion 
```

指定默认包索引版本。对于 1.5.2 之前的 Git 版本使用的旧版程序包索引，有效值为 1;对于具有大于 4 GB 的程序包的功能的新打包索引，有效值为 2，以及对重新打包已损坏的程序包的适当保护。版本 2 是默认值。请注意，强制执行版本 2，并且只要相应的包大于 2 GB，就会忽略此配置选项。

如果您有一个不懂版本 2 `*.idx`文件的旧 Git，则克隆或获取非本机协议（例如“http”），该协议将从另一端复制`*.pack`文件和相应的`*.idx`文件可能会为您提供一个无法使用旧版本的 Git 访问的存储库。但是，如果`*.pack`文件小于 2 GB，则可以在* .pack 文件中使用 [git-index-pack [1]](https://git-scm.com/docs/git-index-pack) 来重新生成`*.idx`文件。

```
 pack.packSizeLimit 
```

包的最大尺寸。此设置仅影响重新打包时对文件的打包，即 git：//协议不受影响。它可以被 [git-repack [1]](https://git-scm.com/docs/git-repack) 的`--max-pack-size`选项覆盖。达到此限制会导致创建多个 packfiles;这反过来又会阻止创建位图。允许的最小尺寸限制为 1 MiB。默认值是无限制的。支持 _k_ ， _m_ 或 _g_ 的共同单位后缀。

```
 pack.useBitmaps 
```

如果为 true，git 将在打包到 stdout 时使用 pack 位图（如果可用）（例如，在获取的服务器端期间）。默认为 true。除非您正在调试包位图，否则通常不需要关闭它。

```
 pack.useSparse 
```

如果为 true，当存在 _--revs_ 选项时，git 将默认使用 _git pack-objects_ 中的 _--sparse_ 选项。此算法仅处理出现在引入新对象的路径中的树。在计算包发送一个小变化时，这可以带来显着的性能优势。但是，如果包含的提交包含某些类型的直接重命名，则可能会将额外的对象添加到包文件中。

```
 pack.writeBitmaps (deprecated) 
```

这是`repack.writeBitmaps`的弃用同义词。

```
 pack.writeBitmapHashCache 
```

如果为 true，git 将在位图索引中包含“哈希缓存”部分（如果有的话）。此缓存可用于提供 git 的 delta 启发式，可能导致位图和非位图对象之间更好的增量（例如，在较旧的位图包和自上一个 gc 以来已推送的对象之间提取时）。缺点是每个磁盘空间对象消耗 4 个字节，并且 JGit 的位图实现不理解它，如果在同一个存储库中使用 Git 和 JGit，则会导致它抱怨。默认为 false。

```
 pager.<cmd> 
```

如果值为 boolean，则在写入 tty 时打开或关闭特定 Git 子命令输出的分页。否则，使用`pager.<cmd>`值指定的寻呼机打开子命令的分页。如果在命令行中指定了`--paginate`或`--no-pager`，则它优先于此选项。要禁用所有命令的分页，请将`core.pager`或`GIT_PAGER`设置为`cat`。

```
 pretty.<name> 
```

- [git-log [1]](https://git-scm.com/docs/git-log) 中指定的--pretty = format 字符串的别名。这里定义的任何别名都可以像内置的漂亮格式一样使用。例如，运行`git config pretty.changelog "format:* %H %s"`会导致调用`git log --pretty=changelog`等同于运行`git log "--pretty=format:* %H %s"`。请注意，将以静默方式忽略与内置格式同名的别名。

```
 protocol.allow 
```

如果设置，请为未明确拥有策略的所有协议（`protocol.<name>.allow`）提供用户定义的默认策略。默认情况下，如果未设置，已知安全协议（http，https，git，ssh，file）的默认策略为`always`，则已知危险协议（ext）的默认策略为`never`，并且所有其他协议默认策略为`user`。支持的政策：

*   `always` - 始终可以使用协议。

*   `never` - 永远不能使用协议。

*   `user` - 仅当`GIT_PROTOCOL_FROM_USER`未设置或值为 1 时才能使用该协议。当您希望协议可由用户直接使用但不希望由此使用时，应使用此策略。在没有用户输入的情况下执行 clone / fetch / push 命令的命令，例如递归子模块初始化。

```
 protocol.<name>.allow 
```

使用 clone / fetch / push 命令设置协议`<name>`使用的策略。有关可用的政策，请参阅上面的`protocol.allow`。

git 当前使用的协议名称是：

*   `file`：任何基于文件的本地路径（包括`file://` URL 或本地路径）

*   `git`：直接 TCP 连接上的匿名 git 协议（或代理，如果已配置）

*   `ssh`：git over ssh（包括`host:path`语法，`ssh://`等）。

*   `http`：git over http，“smart http”和“dumb http”。注意，这 _ 不是 _ 包括`https`;如果要同时配置两者，则必须单独执行此操作。

*   任何外部助手都按其协议命名（例如，使用`hg`来允许`git-remote-hg`助手）

```
 protocol.version 
```

实验。如果设置，客户端将尝试使用指定的协议版本与服务器通信。如果未设置，客户端将不会尝试使用特定协议版本进行通信，这会导致使用协议版本 0。支持的版本：

*   `0` - 原始线路协议。

*   `1` - 原始有线协议，在服务器的初始响应中添加了一个版本字符串。

*   `2` - 有线协议版本 2 。

```
 pull.ff 
```

默认情况下，Git 在合并作为当前提交的后代的提交时不会创建额外的合并提交。相反，当前分支的提示是快进的。当设置为`false`时，此变量告诉 Git 在这种情况下创建额外的合并提交（相当于从命令行提供`--no-ff`选项）。设置为`only`时，仅允许此类快进合并（相当于从命令行提供`--ff-only`选项）。拉动时，此设置会覆盖`merge.ff`。

```
 pull.rebase 
```

如果为 true，则 rebase 在获取的分支顶部分支，而不是在运行“git pull”时合并默认远程的默认分支。请参阅“branch。< name> .rebase”，以便在每个分支的基础上进行设置。

当`merges`时，将`--rebase-merges`选项传递给 _git rebase_ ，以便本地合并提交包含在 rebase 中（有关详细信息，请参阅 [git-rebase [1]](https://git-scm.com/docs/git-rebase) ）。

保留时，也将`--preserve-merges`传递给 _git rebase_ ，以便通过运行 _git pull_ 不会使本地提交的合并提交变平。

当值为`interactive`时，rebase 以交互模式运行。

**注**：这可能是危险的操作;做**而不是**使用它，除非你理解其含义（详见 [git-rebase [1]](https://git-scm.com/docs/git-rebase) ）。

```
 pull.octopus 
```

一次拉出多个分支时使用的默认合并策略。

```
 pull.twohead 
```

拉动单个分支时使用的默认合并策略。

```
 push.default 
```

如果未明确给出 refspec，则定义`git push`应采取的操作。不同的值非常适合特定的工作流程;例如，在纯粹的中央工作流程中（即获取源等于推送目的地），`upstream`可能就是你想要的。可能的值是：

*   `nothing` - 除非明确给出 refspec，否则不要推送任何内容（错误输出）。这主要是针对那些希望通过始终明确避免错误的人。

*   `current` - 推送当前分支以更新接收端具有相同名称的分支。适用于中央和非中央工作流程。

*   `upstream` - 将当前分支推回到分支，该分支的更改通常集成到当前分支（称为`@{upstream}`）。如果您要推送到通常从中拉出的相同存储库（即中央工作流），则此模式才有意义。

*   `tracking` - 这是`upstream`的已弃用的同义词。

*   `simple` - 在集中式工作流程中，像`upstream`一样工作，如果上游分支的名称与本地分支不同，则可以更加安全地拒绝推送。

    当推送到与通常拉出的遥控器不同的遥控器时，请作为`current`。这是最安全的选择，适合初学者。

    此模式已成为 Git 2.0 中的默认模式。

*   `matching` - 推送两端具有相同名称的所有分支。这使你正在推动的存储库记住将被推出的分支集合（例如，如果你总是在那里推动 _maint_ 和 _master_ 而没有其他分支，你推送到的存储库将有这两个分支，你的本地 _maint_ 和 _ 主 _ 将被推到那里。

    要有效地使用这种模式，你必须确保 _ 所有 _ 你要推出的分支都准备好在运行 _git push_ 之前被推出，因为这个模式的重点是允许你一次推动所有分支。如果您通常只在一个分支上完成工作并推出结果，而其他分支未完成，则此模式不适合您。此模式也不适合推入​​共享中央存储库，因为其他人可能会在那里添加新分支，或者更新控制之外的现有分支的提示。

    这曾经是默认值，但不是因为 Git 2.0（`simple`是新的默认值）。

```
 push.followTags 
```

如果设置为 true，则默认启用`--follow-tags`选项。您可以通过指定`--no-follow-tags`在推送时覆盖此配置。

```
 push.gpgSign 
```

可以设置为布尔值，或者字符串 _if-ask_ 。真值会导致所有推送都被 GPG 签名，就像`--signed`传递给 [git-push [1]](https://git-scm.com/docs/git-push) 一样。字符串 _if-ask_ 会在服务器支持的情况下导致推送被签名，就像`--signed=if-asked`被传递给 _git push_ 一样。 false 值可能会覆盖优先级较低的配置文件中的值。显式命令行标志始终会覆盖此配置选项。

```
 push.pushOption 
```

当没有从命令行给出`--push-option=<option>`参数时，`git push`表现得好像每个< value>该变量的值为`--push-option=<value>`。

这是一个多值变量，可以在更高优先级的配置文件（例如存储库中的`.git/config`）中使用空值来清除从较低优先级配置文件（例如`$HOME/.gitconfig`）继承的值。

例：

/ etc / gitconfig push.pushoption =一个 push.pushoption = b

〜/ .gitconfig push.pushoption = c

repo / .git / config push.pushoption = push.pushoption = b

这将导致仅 b（a 和 c 被清除）。

```
 push.recurseSubmodules 
```

确保要推送的修订使用的所有子模块提交都可在远程跟踪分支上使用。如果值为 _check_，然后 Git 将验证在要推送的修订版本中更改的所有子模块提交在子模块的至少一个远程处可用。如果缺少任何提交，则推送将中止并以非零状态退出。如果值为 _on-demand_，那么将推送在要推送的修订中更改的所有子模块。如果按需无法推送所有必要的修订，它也将被中止并退出非零状态。如果值为 _ 否 _，则保留推送时忽略子模块的默认行为。您可以通过指定 _--recurse-submodules = check | on-demand | no_ 在推送时覆盖此配置。

```
 rebase.useBuiltin 
```

设置为`false`以使用 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 的旧版 shellcript 实现。默认情况下是`true`，这意味着在 C 中使用内置的重写。

C 重写首先包含在 Git 版本 2.20 中。如果在重写中发现任何错误，此选项可用于重新启用旧版本。此选项和 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 的 shellscript 版本将在以后的某个版本中删除。

如果您发现某些理由将此选项设置为`false`而非一次性测试，则应将行为差异报告为 git 中的错误。

```
 rebase.stat 
```

是否显示自上次 rebase 以来上游改变的差异。默认为 False。

```
 rebase.autoSquash 
```

如果设置为 true，则默认启用`--autosquash`选项。

```
 rebase.autoStash 
```

设置为 true 时，在操作开始之前自动创建临时存储条目，并在操作结束后应用它。这意味着您可以在脏工作树上运行 rebase。但是，谨慎使用：成功重组后的最终存储应用程序可能会导致非平凡的冲突。 [git-rebase [1]](https://git-scm.com/docs/git-rebase) 的`--no-autostash`和`--autostash`选项可以覆盖此选项。默认为 false。

```
 rebase.missingCommitsCheck 
```

如果设置为“warn”，git rebase -i 将在删除某些提交时打印警告（例如删除了一行），但是 rebase 仍将继续。如果设置为“error”，它将打印上一个警告并停止 rebase，然后可以使用 _git rebase --edit-todo_ 来纠正错误。如果设置为“忽略”，则不进行检查。要在没有警告或错误的情况下删除提交，请使用待办事项列表中的`drop`命令。默认为“ignore”。

```
 rebase.instructionFormat 
```

[git-log [1]](https://git-scm.com/docs/git-log) 中指定的格式字符串，用于交互式 rebase 期间的待办事项列表。格式将自动在格式之前添加长提交哈希。

```
 rebase.abbreviateCommands 
```

如果设置为 true，`git rebase`将在待办事项列表中使用缩写的命令名称，结果如下：

```
	p deadbee The oneline of the commit
	p fa1afe1 The oneline of the next commit
	...
```

代替：

```
	pick deadbee The oneline of the commit
	pick fa1afe1 The oneline of the next commit
	...
```

默认为 false。

```
 rebase.rescheduleFailedExec 
```

自动重新安排失败的`exec`命令。这仅在交互模式下（或提供`--exec`选项时）才有意义。这与指定`--reschedule-failed-exec`选项相同。

```
 receive.advertiseAtomic 
```

默认情况下，git-receive-pack 将向其客户端通告原子推送功能。如果您不想宣传此功能，请将此变量设置为 false。

```
 receive.advertisePushOptions 
```

设置为 true 时，git-receive-pack 将向其客户端通告推送选项功能。默认为 False。

```
 receive.autogc 
```

默认情况下，git-receive-pack 在从 git-push 接收数据并更新 refs 后将运行“git-gc --auto”。您可以通过将此变量设置为 false 来停止它。

```
 receive.certNonceSeed 
```

通过将此变量设置为字符串，`git receive-pack`将接受`git push --signed`并使用此字符串作为密钥使用 HMAC 保护的“nonce”进行验证。

```
 receive.certNonceSlop 
```

当`git push --signed`在这么多秒内发送一个带有“nonce”的推送证书时，将由证书中的“nonce”导出到`GIT_PUSH_CERT_NONCE`到挂钩（而不是接收包要求发送方包括什么）。这可以使`pre-receive`和`post-receive`中的检查更容易一些。而不是检查`GIT_PUSH_CERT_NONCE_SLOP`环境变量，记录 nonce 过时的秒数，以决定是否要接受证书，他们只能检查`GIT_PUSH_CERT_NONCE_STATUS`是`OK`。

```
 receive.fsckObjects 
```

如果设置为 true，git-receive-pack 将检查所有收到的对象。有关已检查的内容，请参阅`transfer.fsckObjects`。默认为 false。如果未设置，则使用`transfer.fsckObjects`的值。

```
 receive.fsck.<msg-id> 
```

像`fsck.<msg-id> `这样的行为，但被 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 代替 [git-fsck [1]](https://git-scm.com/docs/git-fsck) 使用。有关详细信息，请参阅`fsck.<msg-id> `文档。

```
 receive.fsck.skipList 
```

像`fsck.skipList`这样的行为，但被 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 代替 [git-fsck [1]](https://git-scm.com/docs/git-fsck) 使用。有关详细信息，请参阅`fsck.skipList`文档。

```
 receive.keepAlive 
```

从客户端收到包后，`receive-pack`在处理包时可能不会产生任何输出（如果指定了`--quiet`），导致某些网络丢弃 TCP 连接。设置此选项后，如果`receive-pack`在`receive.keepAlive`秒内没有在此阶段传输任何数据，它将发送一个短的 keepalive 数据包。默认值为 5 秒;设置为 0 以完全禁用 Keepalive。

```
 receive.unpackLimit 
```

如果推送中接收的对象数低于此限制，则对象将解压缩为松散的对象文件。但是，如果接收到的对象的数量等于或超过此限制，则在添加任何丢失的 delta 基础之后，接收的包将作为包存储。从推送中存储包可以使推送操作更快完成，尤其是在慢速文件系统上。如果未设置，则使用`transfer.unpackLimit`的值。

```
 receive.maxInputSize 
```

如果传入包流的大小大于此限制，则 git-receive-pack 将错误输出，而不是接受包文件。如果未设置或设置为 0，则大小不受限制。

```
 receive.denyDeletes 
```

如果设置为 true，git-receive-pack 将拒绝删除 ref 的 ref 更新。使用它来防止通过推送删除这样的引用。

```
 receive.denyDeleteCurrent 
```

如果设置为 true，git-receive-pack 将拒绝 ref 更新，该更新将删除非裸存储库的当前已检出分支。

```
 receive.denyCurrentBranch 
```

如果设置为 true 或“refuse”，git-receive-pack 将拒绝对非裸存储库的当前已检出分支的 ref 更新。这样的推送有潜在危险，因为它使 HEAD 与索引和工作树不同步。如果设置为“警告”，则向 stderr 打印此类推送的警告，但允许推进继续。如果设置为 false 或“ignore”，则允许此类推送而不显示任何消息。默认为“refuse”。

另一个选项是“updateInstead”，如果进入当前分支，它将更新工作树。此选项用于在通过交互式 ssh 无法轻松访问一侧时同步工作目录（例如，实时网站，因此要求工作目录清洁）。在 VM 内部开发以在不同的操作系统上测试和修复代码时，此模式也很方便。

默认情况下，如果工作树或索引与 HEAD 有任何差异，“updateInstead”将拒绝推送，但`push-to-checkout`挂钩可用于自定义此操作。见 [githooks [5]](https://git-scm.com/docs/githooks) 。

```
 receive.denyNonFastForwards 
```

如果设置为 true，git-receive-pack 将拒绝不是快进的 ref 更新。使用此选项可以防止通过推送进行此类更新，即使强制推送也是如此。初始化共享存储库时设置此配置变量。

```
 receive.hideRefs 
```

此变量与`transfer.hideRefs`相同，但仅适用于`receive-pack`（因此影响推送，但不影响提取）。尝试通过`git push`更新或删除隐藏的 ref 被拒绝。

```
 receive.updateServerInfo 
```

如果设置为 true，git-receive-pack 将在从 git-push 接收数据并更新 refs 后运行 git-update-server-info。

```
 receive.shallowUpdate 
```

如果设置为 true，则当新引用需要新的浅根时，可以更新.git / shallow。否则这些裁判被拒绝。

```
 remote.pushDefault 
```

默认情况下推送到的遥控器。覆盖所有分支的`branch.<name>.remote`，并由特定分支的`branch.<name>.pushRemote`覆盖。

```
 remote.<name>.url 
```

远程存储库的 URL。参见 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 或 [git-push [1]](https://git-scm.com/docs/git-push) 。

```
 remote.<name>.pushurl 
```

远程存储库的推送 URL。见 [git-push [1]](https://git-scm.com/docs/git-push) 。

```
 remote.<name>.proxy 
```

对于需要 curl（http，https 和 ftp）的远程控制，用于该远程的代理的 URL。设置为空字符串以禁用该远程代理。

```
 remote.<name>.proxyAuthMethod 
```

对于需要 curl（http，https 和 ftp）的远程控制，用于对正在使用的代理进行身份验证的方法（可能在`remote.<name>.proxy`中设置）。见`http.proxyAuthMethod`。

```
 remote.<name>.fetch 
```

[git-fetch [1]](https://git-scm.com/docs/git-fetch) 的默认“refspec”集。参见 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 。

```
 remote.<name>.push 
```

[git-push [1]](https://git-scm.com/docs/git-push) 的默认“refspec”集。见 [git-push [1]](https://git-scm.com/docs/git-push) 。

```
 remote.<name>.mirror 
```

如果为 true，则推送到此遥控器将自动表现为在命令行上给出`--mirror`选项。

```
 remote.<name>.skipDefaultUpdate 
```

如果为 true，则在使用 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 或 [git-remote [1]](https://git-scm.com/docs/git-remote) 的`update`子命令进行更新时，默认情况下将跳过此遥控器。

```
 remote.<name>.skipFetchAll 
```

如果为 true，则在使用 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 或 [git-remote [1]](https://git-scm.com/docs/git-remote) 的`update`子命令进行更新时，默认情况下将跳过此遥控器。

```
 remote.<name>.receivepack 
```

推送时在远程端执行的默认程序。请参阅 [git-push [1]](https://git-scm.com/docs/git-push) 的选项--receive-pack。

```
 remote.<name>.uploadpack 
```

获取时在远程端执行的默认程序。请参阅 [git-fetch-pack [1]](https://git-scm.com/docs/git-fetch-pack) 的选项--upload-pack。

```
 remote.<name>.tagOpt 
```

将此值设置为--no-tags 会在从远程< name>提取时禁用自动标记。将其设置为--tags 将从远程< name>获取每个标记，即使它们无法从远程分支头部访问。将这些标志直接传递给 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 可以覆盖此设置。请参阅 [git-fetch [1]](https://git-scm.com/docs/git-fetch) 的选项--tags 和--no-tags。

```
 remote.<name>.vcs 
```

将其设置为值<vcs>将导致 Git 与 git-remote-<vcs>进行远程交互帮手。

```
 remote.<name>.prune 
```

设置为 true 时，默认情况下从此远程获取也将删除远程不再存在的任何远程跟踪引用（就像在命令行上给出`--prune`选项一样）。覆盖`fetch.prune`设置（如果有）。

```
 remote.<name>.pruneTags 
```

设置为 true 时，如果通常通过`remote.<name>.prune`，`fetch.prune`或`--prune`激活修剪，默认情况下从此遥控器获取也将删除遥控器上不再存在的任何本地标签。覆盖`fetch.pruneTags`设置（如果有）。

另请参见[HTD0] git-fetch [1] 的`remote.<name>.prune`和 PRUNING 部分。

```
 remotes.<group> 
```

由“git remote update <group>”提取的遥控器列表。见 [git-remote [1]](https://git-scm.com/docs/git-remote) 。

```
 repack.useDeltaBaseOffset 
```

默认情况下， [git-repack [1]](https://git-scm.com/docs/git-repack) 会创建使用 delta-base offset 的包。如果您需要直接或通过诸如 http 之类的哑协议与版本低于 1.4.4 的 Git 共享您的存储库，那么您需要将此选项设置为“false”并重新打包。通过本机协议从旧 Git 版本访问不受此选项的影响。

```
 repack.packKeptObjects 
```

如果设置为 true，则使`git repack`表现为`--pack-kept-objects`已通过。有关详细信息，请参阅 [git-repack [1]](https://git-scm.com/docs/git-repack) 。默认为`false`，但如果正在写入位图索引，则为`true`（通过`--write-bitmap-index`或`repack.writeBitmaps`）。

```
 repack.useDeltaIslands 
```

如果设置为 true，则使`git repack`表现为`--delta-islands`已通过。默认为`false`。

```
 repack.writeBitmaps 
```

如果为 true，git 会在将所有对象打包到磁盘时写入位图索引（例如，运行`git repack -a`时）。该索引可以加速为克隆和提取创建的后续包的“计数对象”阶段，代价是一些磁盘空间和在初始重新打包上花费的额外时间。如果创建了多个 packfiles，则无效。默认为 false。

```
 rerere.autoUpdate 
```

设置为 true 时，`git-rerere`在使用先前记录的分辨率干净地解决冲突后，使用结果内容更新索引。默认为 false。

```
 rerere.enabled 
```

激活已解决冲突的记录，以便在再次遇到冲突时，可以自动解决相同的冲突问题。默认情况下，如果`$GIT_DIR`下有`rr-cache`目录，则启用 [git-rerere [1]](https://git-scm.com/docs/git-rerere) 。如果之前在存储库中使用了“rerere”。

```
 reset.quiet 
```

设置为 true 时， _git reset_ 将默认为 _--quiet_ 选项。

```
 sendemail.identity 
```

配置标识。给定时，会导致 _sendemail 中的值。< identity>_ 子部分优先于 _sendemail_ 部分中的值。默认标识是`sendemail.identity`的值。

```
 sendemail.smtpEncryption 
```

有关说明，请参阅 [git-send-email [1]](https://git-scm.com/docs/git-send-email) 。请注意，此设置不受 _ 标识 _ 机制的约束。

```
 sendemail.smtpssl (deprecated) 
```

_sendemail.smtpEncryption = ssl_ 的弃用别名。

```
 sendemail.smtpsslcertpath 
```

ca 证书的路径（目录或单个文件）。将其设置为空字符串以禁用证书验证。

```
 sendemail.<identity>.* 
```

下面找到的 _sendemail。*_ 参数的特定于身份的版本，优先于通过命令行或`sendemail.identity`选择此身份时的参数。

```
 sendemail.aliasesFile 
```

```
 sendemail.aliasFileType 
```

```
 sendemail.annotate 
```

```
 sendemail.bcc 
```

```
 sendemail.cc 
```

```
 sendemail.ccCmd 
```

```
 sendemail.chainReplyTo 
```

```
 sendemail.confirm 
```

```
 sendemail.envelopeSender 
```

```
 sendemail.from 
```

```
 sendemail.multiEdit 
```

```
 sendemail.signedoffbycc 
```

```
 sendemail.smtpPass 
```

```
 sendemail.suppresscc 
```

```
 sendemail.suppressFrom 
```

```
 sendemail.to 
```

```
 sendemail.tocmd 
```

```
 sendemail.smtpDomain 
```

```
 sendemail.smtpServer 
```

```
 sendemail.smtpServerPort 
```

```
 sendemail.smtpServerOption 
```

```
 sendemail.smtpUser 
```

```
 sendemail.thread 
```

```
 sendemail.transferEncoding 
```

```
 sendemail.validate 
```

```
 sendemail.xmailer 
```

有关说明，请参阅 [git-send-email [1]](https://git-scm.com/docs/git-send-email) 。

```
 sendemail.signedoffcc (deprecated) 
```

`sendemail.signedoffbycc`的弃用别名。

```
 sendemail.smtpBatchSize 
```

每次连接要发送的消息数，之后将发生重新登录。如果值为 0 或未定义，则在一个连接中发送所有消息。另请参阅 [git-send-email [1]](https://git-scm.com/docs/git-send-email) 的`--batch-size`选项。

```
 sendemail.smtpReloginDelay 
```

秒重新连接到 smtp 服务器之前等待。另请参阅 [git-send-email [1]](https://git-scm.com/docs/git-send-email) 的`--relogin-delay`选项。

```
 sequence.editor 
```

`git rebase -i`用于编辑 rebase 指令文件的文本编辑器。该值在使用时由 shell 解释。它可以被`GIT_SEQUENCE_EDITOR`环境变量覆盖。未配置时，将使用默认提交消息编辑器。

```
 showBranch.default 
```

[git-show-branch [1]](https://git-scm.com/docs/git-show-branch) 的默认分支集。参见 [git-show-branch [1]](https://git-scm.com/docs/git-show-branch) 。

```
 splitIndex.maxPercentChange 
```

使用拆分索引功能时，它指定拆分索引可以包含的条目百分比与写入新共享索引之前拆分索引和共享索引中的条目总数的比较。该值应介于 0 和 100 之间。如果值为 0，则始终写入新的共享索引，如果为 100，则永远不会写入新的共享索引。默认情况下，该值为 20，因此如果拆分索引中的条目数大于条目总数的 20％，则会写入新的共享索引。参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 。

```
 splitIndex.sharedIndexExpire 
```

使用拆分索引功能时，在创建新的共享索引文件时，将删除自此变量指定以来未修改的共享索引文件。值“now”立即使所有条目到期，并且“never”完全抑制到期。默认值为“2.weeks.ago”。请注意，每次基于它创建新的拆分索引文件或从中读取新的拆分索引文件时，都会将共享索引文件视为已修改（为了过期）。参见 [git-update-index [1]](https://git-scm.com/docs/git-update-index) 。

```
 ssh.variant 
```

默认情况下，Git 根据配置的 SSH 命令的基本名称（使用环境变量`GIT_SSH`或`GIT_SSH_COMMAND`或配置设置`core.sshCommand`配置）确定要使用的命令行参数。如果无法识别 basename，Git 将尝试通过首先使用`-G`（打印配置）选项调用已配置的 SSH 命令来检测对 OpenSSH 选项的支持，然后使用 OpenSSH 选项（如果成功）或除主机之外没有其他选项和远程命令（如果失败）。

可以设置配置变量`ssh.variant`以覆盖此检测。有效值为`ssh`（使用 OpenSSH 选项），`plink`，`putty`，`tortoiseplink`，`simple`（除主机和远程命令外没有其他选项）。可以使用值`auto`显式请求默认自动检测。任何其他值都被视为`ssh`。也可以通过环境变量`GIT_SSH_VARIANT`覆盖此设置。

每个变体使用的当前命令行参数如下：

*   `ssh` - [-p port] [-4] [-6] [-o option] [username @] host 命令

*   `simple` - [用户名@]主机命令

*   `plink`或`putty` - [-P port] [-4] [-6] [username @] host 命令

*   `tortoiseplink` - [-P port] [-4] [-6] -batch [username @] host 命令

除`simple`变体外，命令行参数可能会随着 git 获得新功能而改变。

```
 status.relativePaths 
```

默认情况下， [git-status [1]](https://git-scm.com/docs/git-status) 显示相对于当前目录的路径。将此变量设置为`false`会显示相对于存储库根目录的路径（这是 v1.5.4 之前的 Git 的默认值）。

```
 status.short 
```

设置为 true 以在 [git-status [1]](https://git-scm.com/docs/git-status) 中默认启用--short。选项--no-short 优先于此变量。

```
 status.branch 
```

设置为 true 以在 [git-status [1]](https://git-scm.com/docs/git-status) 中默认启用--branch。选项--no-branch 优先于此变量。

```
 status.displayCommentPrefix 
```

如果设置为 true， [git-status [1]](https://git-scm.com/docs/git-status) 将在每个输出行之前插入一个注释前缀（以`core.commentChar`开头，默认为`#`）。这是 [git-status [1]](https://git-scm.com/docs/git-status) 在 Git 1.8.4 和之前的行为。默认为 false。

```
 status.renameLimit 
```

在 [git-status [1]](https://git-scm.com/docs/git-status) 和 [git-commit [1]](https://git-scm.com/docs/git-commit) 中执行重命名检测时要考虑的文件数。默认为 diff.renameLimit 的值。

```
 status.renames 
```

Git 是否以及如何在 [git-status [1]](https://git-scm.com/docs/git-status) 和 [git-commit [1]](https://git-scm.com/docs/git-commit) 中检测重命名。如果设置为“false”，则禁用重命名检测。如果设置为“true”，则启用基本重命名检测。如果设置为“复制”或“复制”，Git 也会检测副本。默认为 diff.renames 的值。

```
 status.showStash 
```

如果设置为 true， [git-status [1]](https://git-scm.com/docs/git-status) 将显示当前隐藏的条目数。默认为 false。

```
 status.showUntrackedFiles 
```

默认情况下， [git-status [1]](https://git-scm.com/docs/git-status) 和 [git-commit [1]](https://git-scm.com/docs/git-commit) 显示当前未被 Git 跟踪的文件。仅包含未跟踪文件的目录仅显示目录名称。显示未跟踪的文件意味着 Git 需要 lstat（）整个存储库中的所有文件，这在某些系统上可能很慢。因此，此变量控制命令如何显示未跟踪的文件。可能的值是：

*   `no` - 不显示未跟踪的文件。

*   `normal` - 显示未跟踪的文件和目录。

*   `all` - 显示未跟踪目录中的单个文件。

如果未指定此变量，则默认为 _normal_ 。可以使用 [git-status [1]](https://git-scm.com/docs/git-status) 和 [git-commit [1]](https://git-scm.com/docs/git-commit) 的-u | --ntracked-files 选项覆盖此变量。

```
 status.submoduleSummary 
```

默认为 false。如果将此值设置为非零数字或为真（与-1 或无限数字相同），则将启用子模块摘要，并显示已修改子模块的提交摘要（请参阅[HTG0 的--summary-limit 选项] ] git-submodule [1] ）。请注意，当`diff.ignoreSubmodules`设置为 _all_ 时，或者仅对于那些`submodule.<name>.ignore=all`的子模块，将禁止所有子模块的摘要输出命令。该规则的唯一例外是状态和提交将显示分阶段子模块更改。要查看忽略的子模块的摘要，您可以使用--ignore-submodules = dirty 命令行选项或 _git 子模块摘要 _ 命令，该命令显示类似的输出但不遵循这些设置。

```
 stash.showPatch 
```

如果将其设置为 true，则没有选项的`git stash show`命令将以补丁形式显示存储条目。默认为 false。请参阅 [git-stash [1]](https://git-scm.com/docs/git-stash) 中 _show_ 命令的说明。

```
 stash.showStat 
```

如果将此值设置为 true，则没有选项的`git stash show`命令将显示存储条目的 diffstat。默认为 true。请参阅 [git-stash [1]](https://git-scm.com/docs/git-stash) 中 _show_ 命令的说明。

```
 submodule.<name>.url 
```

子模块的 URL。该变量通过 _git submodule init_ 从.gitmodules 文件复制到 git config。用户可以在通过 _git 子模块更新 _ 获取子模块之前更改配置的 URL。如果既未设置 submodule.<name>.active 或 submodule.active，则此变量的存在将用作回退以指示子模块是否对 git 命令感兴趣。有关详细信息，请参阅 [git-submodule [1]](https://git-scm.com/docs/git-submodule) 和 [gitmodules [5]](https://git-scm.com/docs/gitmodules) 。

```
 submodule.<name>.update 
```

通过 _git submodule update_ 更新子模块的方法，这是唯一受影响的命令，其他如 _git checkout --recurse-submodules_ 不受影响。它存在是出于历史原因，当 _git 子模块 _ 是与子模块交互的唯一命令时; `submodule.active`和`pull.rebase`等设置更具体。它由 [gitmodules [5]](https://git-scm.com/docs/gitmodules) 文件中的`git submodule init`填充。请参阅 [git-submodule [1]](https://git-scm.com/docs/git-submodule) 中 _update_ 命令的说明。

```
 submodule.<name>.branch 
```

子模块的远程分支名称，由`git submodule update --remote`使用。设置此选项可覆盖`.gitmodules`文件中的值。有关详细信息，请参阅 [git-submodule [1]](https://git-scm.com/docs/git-submodule) 和 [gitmodules [5]](https://git-scm.com/docs/gitmodules) 。

```
 submodule.<name>.fetchRecurseSubmodules 
```

此选项可用于控制此子模块的递归获取。可以使用 - [no-] recurse-submodules 命令行选项“git fetch”和“git pull”来覆盖它。此设置将覆盖 [gitmodules [5]](https://git-scm.com/docs/gitmodules) 文件中的设置。

```
 submodule.<name>.ignore 
```

定义在什么情况下“git status”和 diff 系列将子模块显示为已修改。当设置为“all”时，它将永远不会被视为已修改（但它仍将显示在状态输出中并在提交时提交），“脏”将忽略对子模块工作树的所有更改并仅采用差异在子模块的 HEAD 和超级项目中记录的提交之间考虑。 “未跟踪”还将显示其工作树中具有已修改跟踪文件的子模块。使用“none”（未设置此选项时的默认值）还会显示在其工作树中具有未跟踪文件的子模块已更改。此设置将覆盖此子模块的.gitmodules 中的任何设置，可以使用“--ignore-submodules”选项在命令行上覆盖这两个设置。 _git submodule_ 命令不受此设置的影响。

```
 submodule.<name>.active 
```

布尔值，指示子模块是否对 git 命令感兴趣。此配置选项优先于 submodule.active 配置选项。有关详细信息，请参阅 [gitsubmodules [7]](https://git-scm.com/docs/gitsubmodules) 。

```
 submodule.active 
```

一个重复字段，其中包含用于匹配子模块路径的 pathspec，以确定子模块是否对 git 命令感兴趣。有关详细信息，请参阅 [gitsubmodules [7]](https://git-scm.com/docs/gitsubmodules) 。

```
 submodule.recurse 
```

指定默认情况下命令是否递归到子模块。这适用于具有`--recurse-submodules`选项的所有命令，`clone`除外。默认为 false。

```
 submodule.fetchJobs 
```

指定同时获取/克隆的子模块数。正整数允许最多并行获取的子模块数。值为 0 将给出一些合理的默认值。如果未设置，则默认为 1。

```
 submodule.alternateLocation 
```

指定在克隆子模块时子模块如何获取备用模块。可能的值为`no`，`superproject`。默认情况下，假定`no`，不添加引用。当该值设置为`superproject`时，要克隆的子模块将计算其相对于超级项目替代的交替位置。

```
 submodule.alternateErrorStrategy 
```

指定如何使用`submodule.alternateLocation`计算的子模块的替代项处理错误。可能的值为`ignore`，`info`，`die`。默认值为`die`。

```
 tag.forceSignAnnotated 
```

一个布尔值，用于指定创建的带注释标签是否应该进行 GPG 签名。如果在命令行中指定了`--annotate`，则它优先于此选项。

```
 tag.sort 
```

当 [git-tag [1]](https://git-scm.com/docs/git-tag) 显示时，此变量控制标签的排序顺序。没有“--sort =<value>”提供的选项，此变量的值将用作默认值。

```
 tar.umask 
```

此变量可用于限制 tar 存档条目的权限位。默认值为 0002，关闭世界写入位。特殊值“user”表示将使用归档用户的 umask。请参阅 umask（2）和 [git-archive [1]](https://git-scm.com/docs/git-archive) 。

```
 transfer.fsckObjects 
```

如果未设置`fetch.fsckObjects`或`receive.fsckObjects`，则使用此变量的值。默认为 false。

设置后，如果格式错误的对象或指向不存在的对象的链接，则提取或接收将中止。此外，还会检查各种其他问题，包括遗留问题（请参阅`fsck.<msg-id> `），以及潜在的安全问题，如`.GIT`目录或恶意`.gitmodules`文件的存在（请参阅 v2.2.1 的发行说明和 v2.17.1 了解详情）。在将来的版本中可能会添加其他健全性和安全性检查。

在接收方，失败的 fsckObjects 会使这些对象无法访问，请参阅 [git-receive-pack [1]](https://git-scm.com/docs/git-receive-pack) 中的“QUARANTINE ENVIRONMENT”。在获取方面，格式错误的对象将在存储库中保持未引用状态。

由于`fetch.fsckObjects`实现的非隔离性质，不能依赖于像`receive.fsckObjects`那样保持对象存储区清洁。

当对象被解包时，它们被写入对象存储库，因此可能会出现恶意对象被引入的情况，即使“获取”失败，只有后续的“获取”成功，因为只检查新的传入对象，而不是已经写入对象库的。不应该依赖这种行为上的差异。将来，这些对象也可能被隔离以用于“获取”。

就目前而言，偏执狂需要找到一些方法来模仿隔离环境，如果他们喜欢与“推”相同的保护。例如。在内部镜像的情况下，分两步执行镜像，一个用于获取不受信任的对象，然后执行第二次“推送”（将使用隔离区）到另一个内部存储库，并让内部客户端使用此推送到存储库，或禁止内部提取，只有在完整的“fsck”运行时才允许它们（并且在此期间没有发生新的提取）。

```
 transfer.hideRefs 
```

字符串`receive-pack`和`upload-pack`用于决定从其初始广告中省略哪些引用。使用多个定义指定多个前缀字符串。排除了此变量值中列出的层次结构下的引用，并在响应`git push`或`git fetch`时隐藏。有关此配置的程序特定版本，请参阅`receive.hideRefs`和`uploadpack.hideRefs`。

您还可以在引用名称前包含`!`以取消该条目，显式公开它，即使之前的条目将其标记为隐藏。如果您有多个 hideRefs 值，则后面的条目会覆盖之前的条目（并且更具体的配置文件中的条目会覆盖不太具体的条目）。

如果正在使用名称空间，则在与`transfer.hiderefs`模式匹配之前，会从每个引用中删除名称空间前缀。例如，如果在`transfer.hideRefs`中指定`refs/heads/master`并且当前名称空间是`foo`，则广告中省略`refs/namespaces/foo/refs/heads/master`，但`refs/heads/master`和`refs/namespaces/bar/refs/heads/master`仍然被宣传为所谓的“有”行。为了在剥离之前匹配 refs，在 ref 名称前面添加`^`。如果组合`!`和`^`，则必须首先指定`!`。

即使你隐藏引用，客户端仍然可以通过 [gitnamespaces [7]](https://git-scm.com/docs/gitnamespaces) 手册页的“安全”部分中描述的技术窃取目标对象;最好将私有数据保存在单独的存储库中。

```
 transfer.unpackLimit 
```

如果未设置`fetch.unpackLimit`或`receive.unpackLimit`，则使用此变量的值。默认值为 100。

```
 uploadarchive.allowUnreachable 
```

如果为 true，则允许客户端使用`git archive --remote`请求任何树，无论是否可以从 ref 提示中访问。有关详细信息，请参阅 [git-upload-archive [1]](https://git-scm.com/docs/git-upload-archive) 的“安全”部分中的讨论。默认为`false`。

```
 uploadpack.hideRefs 
```

此变量与`transfer.hideRefs`相同，但仅适用于`upload-pack`（因此仅影响提取，而不影响推送）。尝试通过`git fetch`获取隐藏的引用将失败。另见`uploadpack.allowTipSHA1InWant`。

```
 uploadpack.allowTipSHA1InWant 
```

当`uploadpack.hideRefs`生效时，允许`upload-pack`接受一个获取请求，该请求在隐藏引用的尖端请求一个对象（默认情况下，这样的请求被拒绝）。另见`uploadpack.hideRefs`。即使这是错误的，客户端也可以通过 [gitnamespaces [7]](https://git-scm.com/docs/gitnamespaces) 手册页的“SECURITY”部分中描述的技术窃取对象;最好将私有数据保存在单独的存储库中。

```
 uploadpack.allowReachableSHA1InWant 
```

允许`upload-pack`接受一个获取请求，该请求要求从任何 ref tip 可以访问的对象。但是，请注意，计算对象可达性在计算上是昂贵的。默认为`false`。即使这是错误的，客户端也可以通过 [gitnamespaces [7]](https://git-scm.com/docs/gitnamespaces) 手册页的“SECURITY”部分中描述的技术窃取对象;最好将私有数据保存在单独的存储库中。

```
 uploadpack.allowAnySHA1InWant 
```

允许`upload-pack`接受一个请求任何对象的获取请求。默认为`false`。

```
 uploadpack.keepAlive 
```

当`upload-pack`启动`pack-objects`时，`pack-objects`准备包时可能会有静音期。通常它会输出进度信息，但如果`--quiet`用于获取，`pack-objects`将一直输出，直到数据包数据开始。一些客户端和网络可能会认为服务器挂起并放弃。设置此选项会指示`upload-pack`每`uploadpack.keepAlive`秒发送一个空的 keepalive 数据包。将此选项设置为 0 会完全禁用 keepalive 数据包。默认值为 5 秒。

```
 uploadpack.packObjectsHook 
```

如果设置了此选项，当`upload-pack`运行`git pack-objects`为客户端创建 packfile 时，它将运行此 shell 命令。它 _ 将 _ 运行的`pack-objects`命令和参数（包括开头的`git pack-objects`）附加到 shell 命令。挂钩的 stdin 和 stdout 被视为`pack-objects`本身运行。即，`upload-pack`将为`pack-objects`提供输入到钩子，并期望在 stdout 上完成一个 packfile。

请注意，如果在存储库级配置中看到此配置变量，则会忽略此配置变量（这是针对从不受信任的存储库获取的安全措施）。

```
 uploadpack.allowFilter 
```

如果设置了此选项，`upload-pack`将支持部分克隆和部分提取对象过滤。

```
 uploadpack.allowRefInWant 
```

如果设置此选项，`upload-pack`将支持协议版本 2 `fetch`命令的`ref-in-want`功能。此功能旨在使负载平衡的服务器受益，因为复制延迟可能无法查看其 ref 引用的 OID 的相同视图。

```
 url.<base>.insteadOf 
```

任何以此值开头的网址都会被重写，而不是以< base>开头。如果某些站点提供大量存储库，并使用多种访问方法为其提供服务，并且某些用户需要使用不同的访问方法，则此功能允许人们指定任何等效的 URL 并让 Git 自动将 URL 重写为特定用户的最佳替代方案，即使对于网站上前所未见的存储库也是如此。当多个 insteadOf 字符串与给定的 URL 匹配时，使用最长匹配。

请注意，任何协议限制都将应用于重写的 URL。如果重写更改 URL 以使用自定义协议或远程帮助程序，则可能需要调整`protocol.*.allow`配置以允许请求。特别是，您希望用于子模块的协议必须设置为`always`而不是默认的`user`。参见上面`protocol.allow`的描述。

```
 url.<base>.pushInsteadOf 
```

任何以此值开头的 URL 都不会被推送到;相反，它将被重写为以< base>开头，并且结果 URL 将被推送到。如果某些站点提供大量存储库，并使用多种访问方法为其提供服务，其中一些方法不允许推送，则此功能允许人们指定只读 URL 并让 Git 自动使用适当的 URL 进行推送，即使对于网站上前所未见的存储库也是如此。当多个 pushInsteadOf 字符串与给定的 URL 匹配时，将使用最长匹配。如果一个遥控器有一个明确的 pushurl，Git 将忽略该遥控器的这个设置。

```
 user.email 
```

您的电子邮件地址将记录在任何新创建的提交中。可以被`GIT_AUTHOR_EMAIL`，`GIT_COMMITTER_EMAIL`和`EMAIL`环境变量覆盖。参见 [git-commit-tree [1]](https://git-scm.com/docs/git-commit-tree) 。

```
 user.name 
```

您的全名将记录在任何新创建的提交中。可以被`GIT_AUTHOR_NAME`和`GIT_COMMITTER_NAME`环境变量覆盖。参见 [git-commit-tree [1]](https://git-scm.com/docs/git-commit-tree) 。

```
 user.useConfigOnly 
```

指示 Git 避免尝试猜测`user.email`和`user.name`的默认值，而只是从配置中检索值。例如，如果您有多个电子邮件地址并希望为每个存储库使用不同的电子邮件地址，那么将此配置选项设置为全局配置中的`true`以及名称，Git 将提示您之前设置电子邮件在新克隆的存储库中进行新提交。默认为`false`。

```
 user.signingKey 
```

如果 [git-tag [1]](https://git-scm.com/docs/git-tag) 或 [git-commit [1]](https://git-scm.com/docs/git-commit) 在创建签名标签或提交时未选择您想要的密钥，则可以覆盖默认选择用这个变量。此选项未更改地传递给 gpg 的--local-user 参数，因此您可以使用 gpg 支持的任何方法指定密钥。

```
 versionsort.prereleaseSuffix (deprecated) 
```

`versionsort.suffix`的弃用别名。如果设置了`versionsort.suffix`，则忽略。

```
 versionsort.suffix 
```

即使在 [git-tag [1]](https://git-scm.com/docs/git-tag) 中使用版本排序，具有相同基本版本但不同后缀的标记名仍然按字典顺序排序，例如，在主要版本之后出现的预发布标签中（例如“1.0”之后的“1.0-rc1”）。可以指定此变量以确定具有不同后缀的标签的排序顺序。

通过在此变量中指定单个后缀，包含该后缀的任何标记名将出现在相应的主要版本之前。例如。如果变量设置为“-rc”，那么所有“1.0-rcX”标签将出现在“1.0”之前。如果指定多次，每个后缀一次，则配置中的后缀顺序将确定具有这些后缀的标记名的排序顺序。例如。如果“-pre”出现在配置中的“-rc”之前，那么所有“1.0-preX”标签将在任何“1.0-rcX”标签之前列出。主释放标签相对于具有各种后缀的标签的放置可以通过在这些其他后缀中指定空后缀来确定。例如。如果后缀“-rc”，“”，“ - hck”和“-bfs”按此顺序出现在配置中，则首先列出所有“v4.8-rcX”标签，然后列出“v4.8”，然后是“v4.8-ckX”，最后是“v4.8-bfsX”。

如果多个后缀匹配相同的标记名，则该标记名将根据从标记名中最早的位置开始的后缀进行排序。如果多个不同的匹配后缀从最早的位置开始，则该标记名将根据这些后缀中最长的一个进行排序。如果它们在多个配置文件中，则不确定不同后缀之间的排序顺序。

```
 web.browser 
```

指定某些命令可能使用的 Web 浏览器。目前只有 [git-instaweb [1]](https://git-scm.com/docs/git-instaweb) 和 [git-help [1]](https://git-scm.com/docs/git-help) 可以使用它。

```
 worktree.guessRemote 
```

如果未指定分支且既不使用`-b`也不使用`-B`和`--detach`，则`git worktree add`默认为从 HEAD 创建新分支。如果`worktree.guessRemote`设置为 true，`worktree add`会尝试查找名称与新分支名称唯一匹配的远程跟踪分支。如果存在这样的分支，则将其签出并设置为新分支的“上游”。如果找不到这样的匹配，则回退到从当前 HEAD 创建新分支。

## BUGS

使用不推荐使用的`[section.subsection]`语法时，如果子节中至少有一个大写字符，则更改值将导致添加多行键而不是更改。例如，当配置看起来像

```
  [section.subsection]
    key = value1
```

并运行`git config section.Subsection.key value2`

```
  [section.subsection]
    key = value1
    key = value2
```

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件