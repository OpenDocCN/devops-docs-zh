# gitattributes

> 原文： [`git-scm.com/docs/gitattributes`](https://git-scm.com/docs/gitattributes)

## 名称

gitattributes - 定义每个路径的属性

## 概要

$ GIT_DIR / info / attributes，.gitattributes

## 描述

`gitattributes`文件是一个简单的文本文件，它为路径名提供`attributes`。

`gitattributes`文件中的每一行都是以下形式：

```
pattern	attr1 attr2 ...
```

也就是说，一个模式后跟一个属性列表，用空格分隔。前导空格和尾随空格被忽略。以 _＃_ 开头的行将被忽略。以双引号开头的模式以 C 风格引用。当模式匹配相关路径时，该行上列出的属性将被赋予路径。

对于给定路径，每个属性可以处于以下状态之一：

```
 Set 
```

该路径具有特殊值“true”的属性;这是通过仅列出属性列表中属性的名称来指定的。

```
 Unset 
```

该路径具有特殊值“false”的属性;这是通过在属性列表中列出前缀为短划线`-`的属性的名称来指定的。

```
 Set to a value 
```

该路径具有指定字符串值的属性;这是通过列出属性的名称，后跟等号`=`及其在属性列表中的值来指定的。

```
 Unspecified 
```

没有模式匹配路径，没有任何说明路径是否具有属性，路径的属性被称为未指定。

当多个模式与路径匹配时，后一行会覆盖较早的行。这个覆盖是按属性完成的。

模式匹配路径的规则与`.gitignore`文件中的规则相同（参见 [gitignore [5]](https://git-scm.com/docs/gitignore) ），但有一些例外：

*   消极的模式被禁止

*   与目录匹配的模式不会递归地匹配该目录中的路径（因此在属性文件中使用尾部斜杠`path/`语法是没有意义的;使用`path/**`代替）

在确定为路径分配了哪些属性时，Git 会查询`$GIT_DIR/info/attributes`文件（具有最高优先级），`.gitattributes`文件与相关路径位于同一目录中，其父目录最多为工作树的顶层（包含`.gitattributes`的目录越远离有问题的路径，其优先级越低）。最后考虑全局和系统范围的文件（它们具有最低优先级）。

当工作树中缺少`.gitattributes`文件时，索引中的路径将用作后退。在检出过程中，使用索引中的`.gitattributes`，然后将工作树中的文件用作后备。

如果您希望仅影响单个存储库（即，将属性分配给特定于该存储库的一个用户工作流的文件），则应将属性放在`$GIT_DIR/info/attributes`文件中。应该由版本控制并分发到其他存储库的属性（即，所有用户感兴趣的属性）应该进入`.gitattributes`文件。应该影响单个用户的所有存储库的属性应该放在由`core.attributesFile`配置选项指定的文件中（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。其默认值为$ XDG_CONFIG_HOME / git / attributes。如果$ XDG_CONFIG_HOME 未设置或为空，则使用$ HOME / .config / git / attributes。系统中所有用户的属性应放在`$(prefix)/etc/gitattributes`文件中。

有时您需要覆盖`Unspecified`状态路径的属性设置。这可以通过列出前缀为感叹号`!`的属性的名称来完成。

## 影响

通过为路径分配特定属性可以影响 Git 的某些操作。目前，以下操作是属性感知的。

### 退房和登记入住

当 _git checkout_ 和 _git merge_ 等命令运行时，这些属性会影响存储库中存储的内容如何复制到工作树文件。它们还会影响 Git 如何在 _git add_ 和 _git commit_ 中存储您在存储库中的工作树中准备的内容。

#### `text`

此属性启用并控制行尾标准化。对文本文件进行规范化后，其行结尾将在存储库中转换为 LF。要控制工作目录中使用的行结束样式，请对单个文件使用`eol`属性，对所有文本文件使用`core.eol`配置变量。请注意，将`core.autocrlf`设置为`true`或`input`会覆盖`core.eol`（请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 中这些选项的定义）。

```
 Set 
```

在路径上设置`text`属性可启用行尾标准化，并将路径标记为文本文件。在不猜测内容类型的情况下进行行尾转换。

```
 Unset 
```

取消设置路径上的`text`属性会告诉 Git 在签入或结帐时不要尝试任何行尾转换。

```
 Set to string value "auto" 
```

当`text`设置为“auto”时，路径将标记为自动行结束转换。如果 Git 决定内容是文本，则其行结尾将在签入时转换为 LF。使用 CRLF 提交文件时，不会进行任何转换。

```
 Unspecified 
```

如果未指定`text`属性，Git 使用`core.autocrlf`配置变量来确定是否应转换文件。

任何其他值都会导致 Git 表现为好像没有指定`text`。

#### `eol`

此属性设置要在工作目录中使用的特定行结束样式。它可以在没有任何内容检查的情况下实现行尾转换，从而有效地设置`text`属性。请注意，在具有 CRLF 行结尾的索引中的路径上设置此属性可能会使路径被视为脏。再次将索引添加到索引将规范化索引中的行结尾。

```
 Set to string value "crlf" 
```

此设置强制 Git 在签入时规范化此文件的行结尾，并在签出文件时将它们转换为 CRLF。

```
 Set to string value "lf" 
```

此设置强制 Git 在签入时将行结尾标准化为 LF，并在签出文件时阻止转换为 CRLF。

#### 向后兼容`crlf`属性

为了向后兼容，`crlf`属性解释如下：

```
crlf		text
-crlf		-text
crlf=input	eol=lf
```

#### 行尾转换

虽然 Git 通常单独保留文件内容，但可以将其配置为将行结尾标准化为存储库中的 LF，并且可选地，在检出文件时将它们转换为 CRLF。

如果您只想在工作目录中使用 CRLF 行结尾，而不管您正在使用哪个存储库，则可以设置配置变量“core.autocrlf”而不使用任何属性。

```
[core]
	autocrlf = true
```

这不会强制文本文件的规范化，但确保引入存储库的文本文件在添加时将其行结尾标准化为 LF，并且已在存储库中标准化的文件保持规范化。

如果要确保任何贡献者引入存储库的文本文件的行结束标准化，可以将 _ 所有 _ 文件的`text`属性设置为“auto”。

```
*	text=auto
```

这些属性允许细粒度控制，如何转换行结尾。下面是一个示例，它将使 Git 规范化.txt，.vcproj 和.sh 文件，确保.vcproj 文件的 CRLF 和.sh 文件在工作目录中具有 LF，并防止.jpg 文件无论其内容如何都被规范化。

```
*               text=auto
*.txt		text
*.vcproj	text eol=crlf
*.sh		text eol=lf
*.jpg		-text
```

| 注意 | 使用推送和拉到中央存储库的跨平台项目中启用`text=auto`转换时，应对包含 CRLF 的文本文件进行规范化。 |

从干净的工作目录：

```
$ echo "* text=auto" >.gitattributes
$ git add --renormalize .
$ git status        # Show files that will be normalized
$ git commit -m "Introduce end-of-line normalization"
```

如果任何不应归一化的文件出现在 _git status_ 中，请在运行 _git add -u_ 之前取消设置`text`属性。

```
manual.pdf	-text
```

相反，Git 未检测到的文本文件可以手动启用规范化。

```
weirdchars.txt	text
```

如果`core.safecrlf`设置为“true”或“warn”，Git 将验证当前设置`core.autocrlf`的转换是否可逆。对于“真实”，Git 拒绝不可逆转的转换;对于“警告”，Git 仅打印警告但接受不可逆转的转换。安全触发器可以防止对工作树中的文件进行此类转换，但也有一些例外情况。即使......

*   _git add_ 本身不会触及工作树中的文件，下次检出就会，所以安全触发器;

*   _git apply_ 用补丁更新文本文件确实触摸了工作树中的文件，但操作是关于文本文件而 CRLF 转换是关于修复行结尾不一致的，所以安全性不会触发;

*   _git diff_ 本身不会触及工作树中的文件，它经常运行以检查你打算下一步的更改 _git add_ 。为了及早发现潜在问题，安全触发。

#### `working-tree-encoding`

Git 将以 ASCII 或其中一个超集（例如 UTF-8，ISO-8859-1，...）编码的文件识别为文本文件。以某些其他编码（例如 UTF-16）编码的文件被解释为二进制文件，因此内置的 Git 文本处理工具（例如 _git diff_ ）以及大多数 Git web 前端都无法显示内容默认情况下这些文件。

在这些情况下，您可以使用`working-tree-encoding`属性告诉 Git 工作目录中文件的编码。如果将具有此属性的文件添加到 Git，则 Git 会将指定编码的内容重新编码为 UTF-8。最后，Git 将 UTF-8 编码内容存储在其内部数据结构中（称为“索引”）。在结帐时，内容将重新编码回指定的编码。

请注意，使用`working-tree-encoding`属性可能会有一些陷阱：

*   替代 Git 实现（例如 JGit 或 libgit2）和较旧的 Git 版本（截至 2018 年 3 月）不支持`working-tree-encoding`属性。如果决定使用存储库中的`working-tree-encoding`属性，则强烈建议确保使用存储库的所有客户端都支持它。

    例如，Microsoft Visual Studio 资源文件（`*.rc`）或 PowerShell 脚本文件（`*.ps1`）有时以 UTF-16 编码。如果将`*.ps1`声明为 UTF-16 文件并添加`foo.ps1`并启用`working-tree-encoding` Git 客户端，则`foo.ps1`将在内部存储为 UTF-8。没有`working-tree-encoding`支持的客户端将`foo.ps1`签出为 UTF-8 编码文件。这通常会给该文件的用户带来麻烦。

    如果不支持`working-tree-encoding`属性的 Git 客户端添加新文件`bar.ps1`，则`bar.ps1`将在内部“按原样”存储（在此示例中可能为 UTF-16）。具有`working-tree-encoding`支持的客户端将内部内容解释为 UTF-8 并尝试在检出时将其转换为 UTF-16。该操作将失败并导致错误。

*   将内容重新编码为非 UTF 编码可能会导致错误，因为转换可能不是 UTF-8 往返安全。如果您怀疑您的编码不是往返安全，那么将其添加到`core.checkRoundtripEncoding`以使 Git 检查往返编码（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。已知 SHIFT-JIS（日语字符集）具有 UTF-8 的往返问题，默认情况下会进行检查。

*   重新编码内容需要可能减慢某些 Git 操作的资源（例如 _git checkout_ 或 _git add_ ）。

仅当您无法以 UTF-8 编码存储文件并且希望 Git 能够将内容作为文本处理时，才使用`working-tree-encoding`属性。

例如，如果您的 _* .ps1_ 文件是带字节顺序标记（BOM）的 UTF-16 编码，并且您希望 Git 根据您的平台执行自动行结束转换，请使用以下属性。

```
*.ps1		text working-tree-encoding=UTF-16
```

如果您的 _* .ps1_ 文件是 UTF-16 小端编码而没有 BOM，并且您希望 Git 在工作目录中使用 Windows 行结尾，请使用以下属性（如果您使用`UTF-16-LE-BOM`而不是`UTF-16LE`想要带有 BOM 的 UTF-16 小端。请注意，如果使用`working-tree-encoding`属性来避免歧义，强烈建议使用`eol`明确定义行结尾。

```
*.ps1		text working-tree-encoding=UTF-16LE eol=CRLF
```

您可以使用以下命令获取平台上所有可用编码的列表：

```
iconv --list
```

如果您不知道文件的编码，则可以使用`file`命令猜测编码：

```
file foo.ps1
```

#### `ident`

当为路径设置属性`ident`时，Git 用`$Id:`替换 blob 对象中的`$Id$`，后跟 40 个字符的十六进制 blob 对象名称，然后在结帐时使用美元符号`$`。任何以`$Id:`开头并以 worktree 文件中的`$`结尾的字节序列在签入时将替换为`$Id$`。

#### `filter`

可以将`filter`属性设置为字符串值，该值指定配置中指定的过滤器驱动程序。

过滤器驱动程序由`clean`命令和`smudge`命令组成，其中任何一个都可以不指定。签出时，当指定`smudge`命令时，命令从其标准输入中提供 blob 对象，其标准输出用于更新工作树文件。同样，`clean`命令用于在签入时转换 worktree 文件的内容。默认情况下，这些命令仅处理单个 blob 并终止。如果使用长时间运行的`process`过滤器代替`clean`和/或`smudge`过滤器，那么 Git 可以在单个 Git 命令的整个生命周期内使用单个过滤器命令调用处理所有 blob，例如`git add --all`。如果配置了长时间运行的`process`过滤器，则它始终优先于配置的单个 blob 过滤器。有关用于与`process`过滤器通信的协议的说明，请参阅以下部分。

内容过滤的一个用途是将内容按摩成对于平台，文件系统和用户更方便使用的形状。对于这种操作模式，这里的关键短语是“更方便”而不是“将某些东西变为无法使用”。换句话说，目的是如果有人取消设置过滤器驱动程序定义，或者没有适当的过滤程序，则该项目仍应可用。

内容过滤的另一个用途是存储无法直接在存储库中使用的内容（例如，引用存储在 Git 外部的真实内容的 UUID，或加密内容），并在检出时将其转换为可用形式（例如，下载外部内容，或解密加密内容）。

这两个过滤器的行为不同，默认情况下，过滤器被视为前者，将内容按摩为更方便的形状。配置中缺少过滤器驱动程序定义，或者以非零状态退出的过滤器驱动程序不是错误，而是使过滤器成为无操作通路。

您可以通过将过滤器&lt; driver&gt; .required 配置变量设置为`true`来声明过滤器将自身无法使用的内容转换为可用内容。

注意：每当更改清理过滤器时，repo 应重新规范化：$ git add --renormalize。

例如，在.gitattributes 中，您将为路径分配`filter`属性。

```
*.c	filter=indent
```

然后，您将在.git / config 中定义“filter.indent.clean”和“filter.indent.smudge”配置，以指定一对命令，以便在签入源文件时修改 C 程序的内容（“clean” “运行”并检出（因为命令是“cat”而没有进行任何更改）。

```
[filter "indent"]
	clean = indent
	smudge = cat
```

为了获得最佳效果，`clean`如果运行两次（“清洁→清洁”应相当于“清洁”），则不应进一步改变其输出，并且多个`smudge`命令不应改变`clean`的输出（“涂抹→涂抹→清洁“应相当于”清洁“）。请参阅下面的合并部分。

“indent”过滤器在这方面表现良好：它不会修改已经正确缩进的输入。在这种情况下，没有污迹过滤器意味着清洁过滤器 _ 必须 _ 接受自己的输出而不修改它。

如果过滤器 _ 必须 _ 成功才能使存储的内容可用，您可以在配置中声明过滤器为`required`：

```
[filter "crypt"]
	clean = openssl enc ...
	smudge = openssl enc -d ...
	required
```

过滤器命令行上的序列“％f”将替换为过滤器正在处理的文件的名称。过滤器可能会在关键字替换中使用它。例如：

```
[filter "p4"]
	clean = git-p4-filter --clean %f
	smudge = git-p4-filter --smudge %f
```

请注意，“％f”是正在处理的路径的名称。根据要过滤的版本，磁盘上的相应文件可能不存在，或者可能具有不同的内容。因此，涂抹和清除命令不应该尝试访问磁盘上的文件，而只是作为标准输入上提供给它们的内容的过滤器。

#### 长时间运行的过滤器

如果通过`filter.&lt;driver&gt;.process`定义过滤器命令（字符串值），则 Git 可以在单个 Git 命令的整个生命周期内使用单个过滤器调用处理所有 blob。这是通过使用长时间运行的进程协议（在 technical / long-running-process-protocol.txt 中描述）实现的。

当 Git 遇到需要清理或污迹的第一个文件时，它会启动过滤器并执行握手。在握手中，Git 发送的欢迎消息是“git-filter-client”，只支持版本 2，支持的功能是“干净”，“涂抹”和“延迟”。

然后 Git 发送一个以 flush 数据包终止的“key = value”对列表。该列表至少包含 filter 命令（基于支持的功能）以及要相对于存储库根目录进行筛选的文件的路径名。在刷新数据包之后，Git 发送内容分为零个或多个 pkt-line 数据包和一个刷新数据包以终止内容。请注意，过滤器在收到内容和最终刷新数据包之前不得发送任何响应。另请注意，“key = value”对的“值”可以包含“=”字符，而键永远不会包含该字符。

```
packet:          git> command=smudge
packet:          git> pathname=path/testfile.dat
packet:          git> 0000
packet:          git> CONTENT
packet:          git> 0000
```

期望过滤器响应一个以 flush 数据包终止的“key = value”对的列表。如果过滤器没有遇到问题，则列表必须包含“成功”状态。在这些数据包之后，预期过滤器将在最后发送零个或多个 pkt-line 数据包和一个 flush 数据包的内容。最后，期望用刷新数据包终止的第二个“key = value”对列表。过滤器可以更改第二个列表中的状态，或者将状态保持为空列表。请注意，无论如何，必须使用 flush 数据包终止空列表。

```
packet:          git< status=success
packet:          git< 0000
packet:          git< SMUDGED_CONTENT
packet:          git< 0000
packet:          git< 0000  # empty list, keep "status=success" unchanged!
```

如果结果内容为空，则期望过滤器以“成功”状态和刷新分组响应以发信号通知空内容。

```
packet:          git< status=success
packet:          git< 0000
packet:          git< 0000  # empty content!
packet:          git< 0000  # empty list, keep "status=success" unchanged!
```

如果过滤器不能或不想处理内容，则应该以“错误”状态响应。

```
packet:          git< status=error
packet:          git< 0000
```

如果过滤器在处理过程中遇到错误，则可以在（部分或完全）发送内容后发送状态“错误”。

```
packet:          git< status=success
packet:          git< 0000
packet:          git< HALF_WRITTEN_ERRONEOUS_CONTENT
packet:          git< 0000
packet:          git< status=error
packet:          git< 0000
```

如果过滤器不能或不想处理内容以及 Git 过程生命周期内的任何未来内容，则期望在协议中的任何点以“中止”状态响应。

```
packet:          git< status=abort
packet:          git< 0000
```

如果设置了“错误”/“中止”状态，Git 既不会停止也不会重新启动过滤器进程。但是，Git 根据`filter.&lt;driver&gt;.required`标志设置其退出代码，模仿`filter.&lt;driver&gt;.clean` / `filter.&lt;driver&gt;.smudge`机制的行为。

如果过滤器在通信期间死亡或者不遵守协议，那么 Git 将停止过滤器进程并使用下一个需要处理的文件重新启动它。根据`filter.&lt;driver&gt;.required`标志，Git 会将其解释为错误。

#### 延迟

如果过滤器支持“延迟”功能，那么 Git 可以在过滤器命令和路径名之后发送标志“can-delay”。该标志表示过滤器可以通过响应没有内容但具有状态“延迟”和刷新分组来延迟过滤当前 blob（例如，以补偿​​网络延迟）。

```
packet:          git> command=smudge
packet:          git> pathname=path/testfile.dat
packet:          git> can-delay=1
packet:          git> 0000
packet:          git> CONTENT
packet:          git> 0000
packet:          git< status=delayed
packet:          git< 0000
```

如果过滤器支持“延迟”功能，则它必须支持“list_available_blobs”命令。如果 Git 发送此命令，那么过滤器应该返回一个路径名列表，这些路径名表示先前已经延迟并且现在可用的 blob。该列表必须以刷新数据包结束，然后是“成功”状态，该状态也以刷新数据包终止。如果延迟路径的 blob 尚未可用，则过滤器应该阻止响应，直到至少有一个 blob 可用。过滤器可以通过发送空列表告诉 Git 它没有更多延迟的 blob。一旦过滤器响应空列表，Git 就会停止询问。 Git 此时未收到的所有 blob 都被视为缺失，并将导致错误。

```
packet:          git> command=list_available_blobs
packet:          git> 0000
packet:          git< pathname=path/testfile.dat
packet:          git< pathname=path/otherfile.dat
packet:          git< 0000
packet:          git< status=success
packet:          git< 0000
```

Git 收到路径名后，会再次请求相应的 blob。这些请求包含路径名和空内容部分。如上所述，期望过滤器以通常的方式响应污迹内容。

```
packet:          git> command=smudge
packet:          git> pathname=path/testfile.dat
packet:          git> 0000
packet:          git> 0000  # empty content!
packet:          git< status=success
packet:          git< 0000
packet:          git< SMUDGED_CONTENT
packet:          git< 0000
packet:          git< 0000  # empty list, keep "status=success" unchanged!
```

#### 例

可以在位于 Git 核心存储库中的`contrib/long-running-filter/example.pl`中找到长时间运行的过滤器演示实现。如果您开发自己的长时间运行过滤器过程，那么`GIT_TRACE_PACKET`环境变量对调试非常有用（参见 [git [1]](https://git-scm.com/docs/git) ）。

请注意，您不能将现有的`filter.&lt;driver&gt;.clean`或`filter.&lt;driver&gt;.smudge`命令与`filter.&lt;driver&gt;.process`一起使用，因为前者使用的是与后者不同的进程间通信协议。

#### 签入/签出属性之间的交互

在签入代码路径中，首先使用`filter`驱动程序转换 worktree 文件（如果指定并定义相应的驱动程序），然后使用`ident`（如果指定）处理结果，最后使用`text`处理（再次） ，如果指定和适用）。

在签出代码路径中，首先使用`text`转换 blob 内容，然后使用`ident`转换为`filter`。

#### 合并具有不同签入/签出属性的分支

如果您为文件添加了导致该文件的规范存储库格式更改的属性，例如添加 clean / smudge 过滤器或 text / eol / ident 属性，那么合并属性不存在的任何内容通常会导致合并冲突。

为了防止这些不必要的合并冲突，可以告诉 Git 在通过设置`merge.renormalize`配置变量解析三向合并时运行文件的所有三个阶段的虚拟签出和签入。当转换后的文件与未转换的文件合并时，这可以防止由签入转换引起的更改导致虚假合并冲突。

只要“涂抹→清洁”产生与“干净”相同的输出，即使对于已经弄脏的文件，此策略也会自动解决所有与过滤器相关的冲突。不以这种方式操作的过滤器可能会导致必须手动解决的其他合并冲突。

### 生成差异文本

#### `diff`

属性`diff`影响 Git 如何为特定文件生成差异。它可以告诉 Git 是为路径生成文本补丁还是将路径视为二进制文件。它还可以影响在 hunk header `@@ -k,l +n,m @@`行上显示的行，告诉 Git 使用外部命令生成 diff，或者在生成 diff 之前让 Git 将二进制文件转换为文本格式。

```
 Set 
```

设置`diff`属性的路径被视为文本，即使它们包含通常永远不会出现在文本文件中的字节值，例如 NUL。

```
 Unset 
```

未设置`diff`属性的路径将生成`Binary files differ`（如果启用了二进制补丁，则生成二进制补丁）。

```
 Unspecified 
```

首先指定`diff`属性未指定的路径检查其内容，如果它看起来像文本并且小于 core.bigFileThreshold，则将其视为文本。否则会产生`Binary files differ`。

```
 String 
```

使用指定的 diff 驱动程序显示 Diff。每个驱动程序可以指定一个或多个选项，如以下部分所述。 diff 驱动程序“foo”的选项由 Git 配置文件的“diff.foo”部分中的配置变量定义。

#### 定义外部差异驱动程序

diff 驱动程序的定义是在`gitconfig`中完成的，而不是`gitattributes`文件，所以严格来说这个手册页是一个错误的地方来讨论它。然而...

要定义外部差异驱动程序`jcdiff`，请在`$GIT_DIR/config`文件（或`$HOME/.gitconfig`文件）中添加一个部分，如下所示：

```
[diff "jcdiff"]
	command = j-c-diff
```

当 Git 需要显示`diff`属性设置为`jcdiff`的路径的 diff 时，它会调用您使用上述配置指定的命令，即`j-c-diff`，带有 7 个参数，就像调用`GIT_EXTERNAL_DIFF`程序一样。有关详细信息，请参阅 [git [1]](https://git-scm.com/docs/git) 。

#### 定义自定义的 hunk-header

文本差异输出中的每组更改（称为“hunk”）都以以下形式的行为前缀：

```
@@ -k,l +n,m @@ TEXT
```

这称为 _ 块头 _。默认情况下，“TEXT”部分是以字母，下划线或美元符号开头的行;这匹配 GNU _diff -p_ 输出使用的内容。但是，此默认选择不适用于某些内容，您可以使用自定义模式进行选择。

首先，在.gitattributes 中，您将为路径分配`diff`属性。

```
*.tex	diff=tex
```

然后，您将定义一个“diff.tex.xfuncname”配置来指定一个正则表达式，该表达式与您希望显示为 Hunk 标题“TEXT”的行匹配。在`$GIT_DIR/config`文件（或`$HOME/.gitconfig`文件）中添加一个部分，如下所示：

```
[diff "tex"]
	xfuncname = "^(\\\\(sub)*section\\{.*)$"
```

注意。配置文件解析器会使用单级反斜杠，因此您需要将反斜杠加倍;上面的模式选择一个以反斜杠开头的行，然后是`sub`后跟`section`后跟开放式大括号的零次或多次出现，直到行的末尾。

有一些内置模式可以使这更容易，`tex`就是其中之一，因此您不必在配置文件中编写上述内容（您仍然需要通过属性机制通过`.gitattributes`启用它]）。可以使用以下内置模式：

*   `ada`适用于 Ada 语言的源代码。

*   `bibtex`适用于带有 BibTeX 编码参考的文件。

*   `cpp`适用于 C 和 C ++语言的源代码。

*   `csharp`适用于 C＃语言的源代码。

*   `css`适用于级联样式表。

*   `fortran`适用于 Fortran 语言的源代码。

*   `fountain`适用于 Fountain 文档。

*   `golang`适用于 Go 语言的源代码。

*   `html`适用于 HTML / XHTML 文档。

*   `java`适用于 Java 语言的源代码。

*   `matlab`适用于 MATLAB 语言的源代码。

*   `objc`适用于 Objective-C 语言的源代码。

*   `pascal`适用于 Pascal / Delphi 语言的源代码。

*   `perl`适用于 Perl 语言的源代码。

*   `php`适用于 PHP 语言的源代码。

*   `python`适用于 Python 语言的源代码。

*   `ruby`适用于 Ruby 语言的源代码。

*   `tex`适用于 LaTeX 文档的源代码。

#### 自定义单词差异

您可以通过在“diff。*。wordRegex”配置变量中指定适当的正则表达式来自定义`git diff --word-diff`用于分割行中单词的规则。例如，在 TeX 中，反斜杠后跟一系列字母形成一个命令，但是几个这样的命令可以一起运行而不会插入空格。要分隔它们，请在`$GIT_DIR/config`文件（或`$HOME/.gitconfig`文件）中使用正则表达式，如下所示：

```
[diff "tex"]
	wordRegex = "\\\\[a-zA-Z]+|[{}]|\\\\.|[^\\{}[:space:]]+"
```

为上一节中列出的所有语言提供了内置模式。

#### 执行二进制文件的文本差异

有时需要查看某些二进制文件的文本转换版本的差异。例如，可以将文字处理器文档转换为 ASCII 文本表示，并显示文本的差异。即使这种转换失去了一些信息，生成的差异对人类观看也很有用（但不能直接应用）。

`textconv`配置选项用于定义执行此类转换的程序。程序应该采用单个参数，要转换的文件的名称，并在 stdout 上生成结果文本。

例如，要显示文件的 exif 信息的差异而不是二进制信息（假设您已安装 exif 工具），请将以下部分添加到`$GIT_DIR/config`文件（或`$HOME/.gitconfig`文件）：

```
[diff "jpg"]
	textconv = exif
```

| 注意 | 文本转换通常是单向转换;在这个例子中，我们丢失了实际的图像内容，只关注文本数据。这意味着 textconv 生成的差异是 _ 而不是 _ 适合应用。因此，只有`git diff`和`git log`系列命令（即 log，whatchanged，show）才能执行文本转换。 `git format-patch`永远不会生成此输出。如果你想向某人发送二进制文件的文本转换差异（例如，因为它会快速传达你所做的更改），你应该单独生成它并将其作为注释发送 _ 除了 _ 您可能发送的常用二进制差异。 |

因为文本转换速度很慢，特别是在使用`git log -p`进行大量转换时，Git 提供了一种缓存输出并在将来的差异中使用它的机制。要启用缓存，请在 diff 驱动程序的配置中设置“cachetextconv”变量。例如：

```
[diff "jpg"]
	textconv = exif
	cachetextconv = true
```

这将缓存每个 blob 上无限期运行“exif”的结果。如果更改 diff 驱动程序的 textconv 配置变量，Git 将自动使缓存条目无效并重新运行 textconv 过滤器。如果你想手动使缓存失效（例如，因为你的“exif”版本已更新并且现在产生更好的输出），你可以用`git update-ref -d refs/notes/textconv/jpg`手动删除缓存（其中“jpg”是差异驱动程序的名称，如上例所示）。

#### 选择 textconv 与外部差异

如果要在存储库中显示二进制或特殊格式的 blob 之间的差异，可以选择使用外部 diff 命令，或使用 textconv 将它们转换为可扩展的文本格式。您选择哪种方法取决于您的具体情况。

使用外部 diff 命令的优点是灵活性。您不一定要找到面向行的更改，输出也不一定要像统一的 diff 那样。您可以以最合适的方式为数据格式定位和报告更改。

相比之下，textconv 更具限制性。您提供了将数据转换为面向行的文本格式，Git 使用其常规 diff 工具生成输出。选择此方法有几个好处：

1.  便于使用。编写二进制文本转换通常要比执行自己的 diff 更简单。在许多情况下，现有程序可以用作 textconv 过滤器（例如，exif，odt2txt）。

2.  Git diff 功能。通过自己仅执行转换步骤，您仍然可以利用 Git 的许多差异功能，包括着色，字差异和合并差异进行合并。

3.  缓存。 Textconv 缓存可以加速重复的差异，例如您可能通过运行`git log -p`触发的差异。

#### 将文件标记为二进制文件

Git 通常通过检查内容的开头来正确猜测 blob 是否包含文本或二进制数据。但是，有时您可能希望覆盖其决定，因为 blob 在文件中稍后包含二进制数据，或者因为内容虽然在技术上由文本字符组成，但对于人类读者来说是不透明的。例如，许多 postscript 文件仅包含 ASCII 字符，但会产生嘈杂且无意义的差异。

将文件标记为二进制文件的最简单方法是取消设置`.gitattributes`文件中的 diff 属性：

```
*.ps -diff
```

这将导致 Git 生成`Binary files differ`（或二进制补丁，如果启用了二进制补丁）而不是常规差异。

但是，也可能需要指定其他 diff 驱动程序属性。例如，您可能希望使用`textconv`将 postscript 文件转换为 ASCII 表示形式以供人工查看，但另外将其视为二进制文件。您不能同时指定`-diff`和`diff=ps`属性。解决方案是使用`diff.*.binary`配置选项：

```
[diff "ps"]
  textconv = ps2ascii
  binary = true
```

### 执行三向合并

#### `merge`

当`git merge`期间需要文件级合并以及`git revert`和`git cherry-pick`等其他命令时，属性`merge`会影响文件的三个版本的合并方式。

```
 Set 
```

内置的 3 路合并驱动程序用于以类似于`RCS`套件的 _merge_ 命令的方式合并内容。这适用于普通文本文件。

```
 Unset 
```

将当前分支中的版本作为暂定合并结果，并声明合并存在冲突。这适用于没有明确定义的合并语义的二进制文件。

```
 Unspecified 
```

默认情况下，它使用与设置`merge`属性时相同的内置 3 向合并驱动程序。但是，`merge.default`配置变量可以将不同的合并驱动程序命名为与未指定`merge`属性的路径一起使用。

```
 String 
```

使用指定的自定义合并驱动程序执行 3 向合并。可以通过询问“text”驱动程序明确指定内置的 3 路合并驱动程序;可以使用“二进制”来请求内置的“取当前分支”驱动程序。

#### 内置合并驱动程序

定义了一些内置的低级合并驱动程序，可以通过`merge`属性询问。

```
 text 
```

通常的 3 向文件级合并文本文件。冲突区域标有冲突标记`&lt;&lt;&lt;&lt;&lt;&lt;&lt;`，`=======`和`&gt;&gt;&gt;&gt;&gt;&gt;&gt;`。分支的版本显示在`=======`标记之前，合并分支的版本显示在`=======`标记之后。

```
 binary 
```

保留工作树中的分支版本，但保留路径处于冲突状态以供用户进行整理。

```
 union 
```

对文本文件运行 3 向文件级别合并，但从两个版本中获取行，而不是留下冲突标记。这往往会以随机顺序在结果文件中保留添加的行，用户应验证结果。如果您不理解其含义，请不要使用此功能。

#### 定义自定义合并驱动程序

合并驱动程序的定义在`.git/config`文件中完成，而不是在`gitattributes`文件中完成，因此严格来说，这个手册页是一个错误的地方来讨论它。然而...

要定义自定义合并驱动程序`filfre`，请在`$GIT_DIR/config`文件（或`$HOME/.gitconfig`文件）中添加一个部分，如下所示：

```
[merge "filfre"]
	name = feel-free merge driver
	driver = filfre %O %A %B %L %P
	recursive = binary
```

`merge.*.name`变量为驱动程序提供了一个人类可读的名称。

`merge.*.driver`变量的值用于构造运行以合并祖先版本（`%O`），当前版本（`%A`）和其他分支版本（`%B`）的命令。在构建命令行时，这三个标记将替换为保存这些版本内容的临时文件的名称。此外，％L 将替换为冲突标记大小（见下文）。

合并驱动程序应该通过覆盖它来将合并的结果留在以`%A`命名的文件中，如果它设法干净地合并它们，则退出为零状态，如果存在冲突则为非零。

`merge.*.recursive`变量指定当共同驱动器被调用以在共同祖先之间进行内部合并时，要使用的其他合并驱动程序。如果未指定，则驱动程序本身用于内部合并和最终合并。

合并驱动程序可以通过占位符`%P`了解合并结果的路径名。

#### `conflict-marker-size`

此属性控制冲突合并期间留在工作树文件中的冲突标记的长度。仅将值设置为正整数具有任何有意义的效果。

例如，`.gitattributes`中的这一行可用于告诉合并机器在合并文件`Documentation/git-merge.txt`导致冲突时留下更长的时间（而不是通常的 7 个字符长）冲突标记。

```
Documentation/git-merge.txt	conflict-marker-size=32
```

### 检查空白错误

#### `whitespace`

`core.whitespace`配置变量允许您定义 _diff_ 和 _ 适用 _ 应该考虑项目中所有路径的空白错误（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。此属性为每个路径提供更精细的控制。

```
 Set 
```

注意 Git 已知的所有类型的潜在空白错误。标签宽度取自`core.whitespace`配置变量的值。

```
 Unset 
```

不要注意任何错误。

```
 Unspecified 
```

使用`core.whitespace`配置变量的值来确定要注意的错误。

```
 String 
```

以与`core.whitespace`配置变量相同的格式指定逗号单独的常见空白问题列表。

### 创建档案

#### `export-ignore`

具有`export-ignore`属性的文件和目录不会添加到存档文件中。

#### `export-subst`

如果为文件设置了属性`export-subst`，那么在将此文件添加到存档时，Git 将展开多个占位符。扩展取决于提交 ID 的可用性，即，如果 [git-archive [1]](https://git-scm.com/docs/git-archive) 已被赋予树而不是提交或标记，则不会进行替换。占位符与 [git-log [1]](https://git-scm.com/docs/git-log) 的选项`--pretty=format:`的占位符相同，只是它们需要像这样包装：文件中的`$Format:PLACEHOLDERS$`。例如。字符串`$Format:%H$`将被提交哈希替换。

### 包装物

#### `delta`

对于属性`delta`设置为 false 的路径，不会尝试对 blob 进行增量压缩。

### 在 GUI 工具中查看文件

#### `encoding`

此属性的值指定 GUI 工具应使用的字符编码（例如 [gitk [1]](https://git-scm.com/docs/gitk) 和 [git-gui [1]](https://git-scm.com/docs/git-gui) ）以显示相关文件的内容。请注意，由于性能方面的考虑， [gitk [1]](https://git-scm.com/docs/gitk) 不使用此属性，除非您在其选项中手动启用每个文件编码。

如果未设置此属性或具有无效值，则使用`gui.encoding`配置变量的值（参见 [git-config [1]](https://git-scm.com/docs/git-config) ）。

## 使用宏观属性

您不希望应用任何行尾转换，也不希望为您跟踪的任何二进制文件生成文本差异。您需要指定例如

```
*.jpg -text -diff
```

但是当你有很多属性时，这可能会变得很麻烦。使用宏属性，您可以定义一个属性，该属性在设置时还可以同时设置或取消设置许多其他属性。系统知道内置的宏属性`binary`：

```
*.jpg binary
```

如上所述，设置“binary”属性也会取消设置“text”和“diff”属性。请注意，宏属性只能是“设置”，但设置一个可能具有设置或取消设置其他属性或甚至将其他属性返回到“未指定”状态的效果。

## 定义宏观属性

自定义宏属性只能在顶级 gitattributes 文件（`$GIT_DIR/info/attributes`，工作树顶层的`.gitattributes`文件或全局或系统范围的 gitattributes 文件）中定义，而不能在`.gitattributes`文件中定义。工作树子目录。内置的宏属性“binary”相当于：

```
[attr]binary -diff -merge -text
```

## 例子

如果您有这三个`gitattributes`文件：

```
(in $GIT_DIR/info/attributes)

a*	foo !bar -baz

(in .gitattributes)
abc	foo bar baz

(in t/.gitattributes)
ab*	merge=filfre
abc	-foo -bar
*.c	frotz
```

给路径`t/abc`的属性计算如下：

1.  通过检查`t/.gitattributes`（与相关路径在同一目录中），Git 发现第一行匹配。 `merge`属性已设置。它还发现第二行匹配，并且未设置属性`foo`和`bar`。

2.  然后它检查`.gitattributes`（在父目录中），并发现第一行匹配，但`t/.gitattributes`文件已经决定了如何将`merge`，`foo`和`bar`属性赋予此路径，所以它使`foo`和`bar`未设置。属性`baz`已设置。

3.  最后它检查了`$GIT_DIR/info/attributes`。此文件用于覆盖树内设置。第一行是匹配，`foo`设置，`bar`恢复为未指定状态，`baz`未设置。

结果，分配给`t/abc`的属性变为：

```
foo	set to true
bar	unspecified
baz	set to false
merge	set to string value "filfre"
frotz	unspecified
```

## 也可以看看

[git-check-attr [1]](https://git-scm.com/docs/git-check-attr) 。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件