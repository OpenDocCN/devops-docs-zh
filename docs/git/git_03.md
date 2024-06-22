# git-help

> 原文： [`git-scm.com/docs/git-help`](https://git-scm.com/docs/git-help)
> 
> 贡献者：[honglyua](https://github.com/honglyua)

## 名称

git-help - 显示有关 Git 的帮助信息

## 概要

```
git help [-a|--all [--[no-]verbose]] [-g|--guide]
	   [-i|--info|-m|--man|-w|--web] [COMMAND|GUIDE]
```

## 描述

如果没有选项，也没有给出 COMMAND 或 GUIDE，则 _git_ 命令的概要和最常用的 Git 命令列表将打印在标准输出上。

如果给出选项`--all`或`-a`，则所有可用命令都将打印在标准输出上。

如果给出选项`--guide`或`-g`，则标准输出上也会打印有用的 Git 指南列表。

如果给出了命令或指南，则会显示该命令或指南的手册页。默认情况下， _man_ 程序用于此目的，但这可以被其他选项或配置变量覆盖。

如果给出了别名，git 会在标准输出上显示别名的定义。要获取别名命令的手册页，请使用`git COMMAND --help`。

请注意，`git --help ...`与`git help ...`相同，因为前者在内部转换为后者。

要显示 [git [1]](https://git-scm.com/docs/git) 手册页，请使用`git help git`。

可以使用 _git help help_ 或`git help --help`显示此页面

## 选项

```
 -a 
```

```
 --all 
```

在标准输出上打印所有可用命令。此选项会覆盖任何给定的命令或指南名称。

```
 --verbose 
```

与`--all`打印描述一起使用时，用于所有已识别的命令。这是默认值。

```
 -c 
```

```
 --config 
```

列出所有可用的配置变量。这是 [git-config [1]](https://git-scm.com/docs/git-config) 中列表的简短摘要。

```
 -g 
```

```
 --guides 
```

在标准输出上打印有用指南列表。此选项会覆盖任何给定的命令或指南名称。

```
 -i 
```

```
 --info 
```

以 _info_ 格式显示命令的手册页。 _info_ 程序将用于此目的。

```
 -m 
```

```
 --man 
```

以 _man_ 格式显示命令的手册页。此选项可用于覆盖`help.format`配置变量中设置的值。

默认情况下， _man_ 程序将用于显示手册页，但`man.viewer`配置变量可用于选择其他显示程序（见下文）。

```
 -w 
```

```
 --web 
```

以 _Web_ （HTML）格式显示命令的手册页。 Web 浏览器将用于此目的。

如果未设置前者，可以使用配置变量`help.browser`或`web.browser`指定 Web 浏览器。如果没有设置这些配置变量， _git web{litdd}browse_ 帮助程序脚本（由 _git help_ 调用）将选择合适的默认值。有关详细信息，请参阅 [git-web {litdd}浏览[1]](https://git-scm.com/docs/git-web{litdd}browse) 。

## 配置变量

### help.format

如果未传递命令行选项，则将检查`help.format`配置变量。此变量支持以下值;他们使 _git help_ 的行为与其对应的命令行选项相同：

*   “man”对应于 _-m | --man_ ，

*   “info”对应 _-i | --info_ ，

*   “web”或“html”对应于 _-w | --web_ 。

### help.browser，web.browser 和 browser.<tool>.path

如果选择 _web_ 格式（通过命令行选项或配置变量），也将检查`help.browser`，`web.browser`和`browser.&lt;tool&gt;.path`。参见上面 OPTIONS 部分的 _-w | --web_ 和 [git-web {litdd}浏览[1]](https://git-scm.com/docs/git-web{litdd}browse) 。

### man.viewer

如果选择 _man_ 格式，将检查`man.viewer`配置变量。目前支持以下值：

*   “man”：像往常一样使用 _man_ 程序，

*   “woman”：使用 _emacsclient_ 在 emacs 中启动“woman”模式（这只适用于 emacsclient 版本 22），

*   “konqueror”：使用 _kfmclient_ 在新的 konqueror 标签中打开手册页（参见下面的 _ 关于 konqueror_ 的注释）。

如果存在相应的`man.<tool>.cmd`配置条目（参见下文），则可以使用其他工具的值。

可以为`man.viewer`配置变量赋予多个值。将按配置文件中列出的顺序尝试相应的程序。

例如，这个配置：

```
	[man]
		viewer = konqueror
		viewer = woman
```

将首先尝试使用 konqueror。但这可能会失败（例如，如果没有设置 DISPLAY），那么将尝试 emacs 的 woman 模式。

如果一切都失败，或者没有配置查看器，将尝试在`GIT_MAN_VIEWER`环境变量中指定的查看器。如果那也失败了，无论如何都会尝试 _man_ 程序。

### man.<tool>.path

您可以通过设置配置变量`man.<tool>.path`显式提供首选人员查看器的完整路径。例如，您可以通过设置 _man.konqueror.path_ 来配置 konqueror 的绝对路径。否则， _git help_ 假定该工具在 PATH 中可用。

### man.<tool>.cmd

当`man.viewer`配置变量指定的 man 查看器不在支持的那个中时，将查找相应的`man.<tool>.cmd`配置变量。如果此变量存在，则指定的工具将被视为自定义命令，并且将使用 shell eval 运行命令，并将手册页作为参数传递。

### 关于 konqueror 的注意事项

当在`man.viewer`配置变量中指定 _konqueror_ 时，我们启动 _kfmclient_ 以尝试在新选项卡中打开已打开的 konqueror 的手册页（如果可能）。

为了保持一致性，如果将 _man.konqueror.path_ 设置为 _A_PATH_TO/konqueror_，我们也会尝试这样的技巧。这意味着我们将尝试启动 _A_PATH_TO/kfmclient_ 。

如果你真的想使用 _konqueror_ ，那么你可以使用如下内容：

```
	[man]
		viewer = konq

	[man "konq"]
		cmd = A_PATH_TO/konqueror
```

### 关于 git config --global 的注意事项

请注意，可能应使用`--global`标志设置所​​有这些配置变量，例如：

```
$ git config --global help.format web
$ git config --global web.browser firefox
```

因为它们可能比特定于存储库更具用户特性。有关此内容的详细信息，请参阅 [git-config [1]](https://git-scm.com/docs/git-config) 。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件