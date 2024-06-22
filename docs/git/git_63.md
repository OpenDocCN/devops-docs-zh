# git-daemon

> 原文： [`git-scm.com/docs/git-daemon`](https://git-scm.com/docs/git-daemon)

## 名称

git-daemon - Git 存储库的一个非常简单的服务器

## 概要

```
git daemon [--verbose] [--syslog] [--export-all]
	     [--timeout=<n>] [--init-timeout=<n>] [--max-connections=<n>]
	     [--strict-paths] [--base-path=<path>] [--base-path-relaxed]
	     [--user-path | --user-path=<path>]
	     [--interpolated-path=<pathtemplate>]
	     [--reuseaddr] [--detach] [--pid-file=<file>]
	     [--enable=<service>] [--disable=<service>]
	     [--allow-override=<service>] [--forbid-override=<service>]
	     [--access-hook=<path>] [--[no-]informative-errors]
	     [--inetd |
	      [--listen=<host_or_ipaddr>] [--port=<n>]
	      [--user=<user> [--group=<group>]]]
	     [--log-destination=(stderr|syslog|none)]
	     [<directory>…​]
```

## 描述

一个非常简单的 TCP Git 守护程序，它通常侦听端口“DEFAULT_GIT_PORT”，即 9418.它等待连接请求服务，并且如果启用该服务将服务该服务。

它验证该目录是否具有魔术文件“git-daemon-export-ok”，并且它将拒绝导出任何未明确标记为以这种方式导出的 Git 目录（除非指定了`--export-all`参数）。如果您将某些目录路径作为 _git 守护程序 _ 参数传递，则可以进一步将要约限制为包含这些路径的白名单。

默认情况下，仅启用`upload-pack`服务，该服务为 _git fetch-pack_ 和 _git ls-remote_ 客户端提供服务，这些客户端是从 _git fetch_ 调用的， _git pull_ 和 _git clone_ 。

这非常适合只读更新，即从 Git 存储库中提取。

还存在`upload-archive`以服务 _git 存档 _。

## OPTIONS

```
 --strict-paths 
```

完全匹配路径（即当真实路径是“/foo/repo.git”或“/foo/repo/.git”时不允许“/ foo / repo”）并且不做用户相对路径。启用此选项且未指定白名单时， _git 守护程序 _ 将拒绝启动。

```
 --base-path=<path> 
```

将所有路径请求重新映射为相对于给定路径。这有点像“Git root” - 如果你在 example.com 上用 _--base-path = / srv / git_ 运行 _git 守护进程 _，那么如果你以后尝试拉 _git：//example.com/hello.git_ ， _git 守护程序 _ 会将路径解释为 _/srv/git/hello.git_ 。

```
 --base-path-relaxed 
```

如果启用了--base-path 并且 repo lookup 失败，则使用此选项 _git 守护程序 _ 将尝试在不添加基本路径前缀的情况下进行查找。这对于切换到--base-path 用法很有用，同时仍允许旧路径。

```
 --interpolated-path=<pathtemplate> 
```

为了支持虚拟主机，可以使用内插路径模板来动态构建备用路径。模板支持客户端提供的目标主机名％H，但转换为全部小写，％CH 为规范主机名，％IP 为服务器 IP 地址，％P 为端口号，％D 为绝对路径命名的存储库。插值后，路径将根据目录白名单进行验证。

```
 --export-all 
```

允许从所有看起来像 Git 存储库的目录（具有 _ 对象 _ 和 _refs_ 子目录）中拉出，即使它们没有 _git-daemon-export-ok_ ]文件。

```
 --inetd 
```

让服务器作为 inetd 服务运行。意味着--sloglog（可以用`--log-destination=`覆盖）。与--detach， - port， - liste， - user 和--group 选项不兼容。

```
 --listen=<host_or_ipaddr> 
```

收听特定的 IP 地址或主机名。如果支持，IP 地址可以是 IPv4 地址或 IPv6 地址。如果不支持 IPv6，则也不支持--listen = hostname，并且必须为--listen 提供 IPv4 地址。可以不止一次。与`--inetd`选项不兼容。

```
 --port=<n> 
```

听另一个端口。与`--inetd`选项不兼容。

```
 --init-timeout=<n> 
```

建立连接的时刻与收到客户端请求之间的超时（以秒为单位）（通常是一个相当低的值，因为它应该基本上是立即的）。

```
 --timeout=<n> 
```

特定客户端子请求的超时（以秒为单位）。这包括服务器处理子请求所花费的时间以及等待下一个客户端请求所花费的时间。

```
 --max-connections=<n> 
```

最大并发客户端数，默认为 32.将其设置为零，无限制。

```
 --syslog 
```

`--log-destination=syslog`的缩写。

```
 --log-destination=<destination> 
```

将日志消息发送到指定目标。请注意，此选项并不意味着--verbose，因此默认情况下仅记录错误条件。 &lt; destination&gt;必须是以下之一：

```
 stderr 
```

写入标准错误。请注意，如果指定了`--detach`，则进程将脱离实际标准错误，使此目标有效地等效于`none`。

```
 syslog 
```

使用`git-daemon`标识符写入 syslog。

```
 none 
```

禁用所有日志记录

如果指定了`--inetd`或`--detach`，则默认目标为`syslog`，否则为`stderr`。

```
 --user-path 
```

```
 --user-path=<path> 
```

允许〜用户表示法用于请求。当没有参数指定时，对 git：// host / ~alice / foo 的请求被视为访问用户`alice`主目录中的 _foo_ 存储库的请求。如果指定了`--user-path=path`，则将相同的请求作为访问用户`alice`的主目录中的`path/foo`存储库的请求。

```
 --verbose 
```

记录有关传入连接和请求文件的详细信息。

```
 --reuseaddr 
```

绑定侦听套接字时使用 SO_REUSEADDR。这允许服务器重新启动而无需等待旧连接超时。

```
 --detach 
```

脱离外壳。意味着--sloglog。

```
 --pid-file=<file> 
```

将进程 ID 保存在 _ 文件 _ 中。守护程序在`--inetd`下运行时忽略。

```
 --user=<user> 
```

```
 --group=<group> 
```

在进入服务循环之前更改守护进程的 uid 和 gid。如果仅在没有`--group`的情况下给出`--user`，则使用用户的主要组 ID。选项的值赋予`getpwnam(3)`和`getgrnam(3)`，不支持数字 ID。

与`--inetd`一起使用时，给出这些选项是错误的;如果需要，在产生 _git 守护程序 _ 之前使用 inet 守护程序的功能来实现相同的功能。

与许多切换用户 ID 的程序一样，守护程序在运行 git 程序时不会重置诸如`$HOME`之类的环境变量，例如`upload-pack`和`receive-pack`。使用此选项时，您可能还需要在启动守护程序之前将`HOME`设置并导出到`&lt;user&gt;`的主目录，并确保`&lt;user&gt;`可读取该目录中的任何 Git 配置文件。

```
 --enable=<service> 
```

```
 --disable=<service> 
```

默认情况下在站点范围内启用/禁用服务。请注意，如果某个服务器标记为可覆盖，并且存储库通过配置项启用该服务，则仍可以为每个存储库启用站点范围内禁用的服务。

```
 --allow-override=<service> 
```

```
 --forbid-override=<service> 
```

允许/禁止使用每个存储库配置覆盖站点范围的默认值。默认情况下，可以覆盖所有服务。

```
 --[no-]informative-errors 
```

当打开信息性错误时，git-daemon 将向客户端报告更详细的错误，将“no such repository”等条件与“未导出的存储库”区分开来。这对客户来说更方便，但可能会泄漏有关未导出存储库存在的信息。如果未启用信息性错误，则所有错误都会向客户端报告“拒绝访问”。默认值为--no-informative-errors。

```
 --access-hook=<path> 
```

每次客户端连接时，首先运行由&lt; path&gt;指定的外部命令。具有服务名称（例如“upload-pack”），存储库的路径，主机名（％H），规范主机名（％CH），IP 地址（％IP）和 TCP 端口（％P）作为其命令行参数。外部命令可以通过退出非零状态（或通过以零状态退出来允许它）来决定拒绝服务。在做出此决定时，它还可以查看$ REMOTE_ADDR 和`$REMOTE_PORT`环境变量以了解请求者。

外部命令可以选择将单行写入其标准输出，以便在拒绝服务时将其作为错误消息发送给请求者。

```
 <directory> 
```

要添加到允许目录的白名单的目录。除非指定了--strict-paths，否则这还将包括每个命名目录的子目录。

## 服务

可以使用此命令的命令行选项全局启用/禁用这些服务。如果需要更细粒度的控制（例如，允许 _git archive_ 仅在守护程序所服务的几个选定的存储库中运行），则每个存储库配置文件可用于启用或禁用它们。

```
 upload-pack 
```

这服务于 _git fetch-pack_ 和 _git ls-remote_ 客户端。它默认启用，但存储库可以通过将`daemon.uploadpack`配置项设置为`false`来禁用它。

```
 upload-archive 
```

这服务 _git archive --remote_ 。默认情况下禁用它，但存储库可以通过将`daemon.uploadarch`配置项设置为`true`来启用它。

```
 receive-pack 
```

这为 _git send-pack_ 客户端提供服务，允许匿名推送。它默认是禁用的，因为协议中有 _ 没有 _ 身份验证（换句话说，任何人都可以将任何内容推送到存储库中，包括删除引用）。这仅适用于每个人都很友好的封闭式 LAN 环境。可以通过将`daemon.receivepack`配置项设置为`true`来启用此服务。

## 例子

```
 We assume the following in /etc/services 
```

```
$ grep 9418 /etc/services
git		9418/tcp		# Git Version Control System
```

```
 git daemon as inetd server 
```

要将 _git 守护程序 _ 设置为处理列入白名单的目录集/ pub / foo 和/ pub / bar 下的任何存储库的 ine​​td 服务，请将以下条目放入/ etc / inetd all in one 线：

```
	git stream tcp nowait nobody  /usr/bin/git
		git daemon --inetd --verbose --export-all
		/pub/foo /pub/bar
```

```
 git daemon as inetd server for virtual hosts 
```

要将 _git 守护程序 _ 设置为处理不同虚拟主机`www.example.com`和`www.example.org`的存储库的 ine​​td 服务，请在`/etc/inetd`中将以下条目全部放在一行：

```
	git stream tcp nowait nobody /usr/bin/git
		git daemon --inetd --verbose --export-all
		--interpolated-path=/pub/%H%D
		/pub/www.example.org/software
		/pub/www.example.com/software
		/software
```

在此示例中，根级目录`/pub`将包含支持的每个虚拟主机名的子目录。此外，两个主机都将存储库简单地称为`git://www.example.com/software/repo.git`。对于 1.4.0 之前的客户端，也可以将`/software`中的符号链接添加到相应的默认存储库中。

```
 git daemon as regular daemon for virtual hosts 
```

要将 _git 守护程序 _ 设置为常规的非 inetd 服务，根据其 IP 地址处理多个虚拟主机的存储库，请启动守护程序，如下所示：

```
	git daemon --verbose --export-all
		--interpolated-path=/pub/%IP/%D
		/pub/192.168.1.200/software
		/pub/10.10.220.23/software
```

在此示例中，根级目录`/pub`将包含支持的每个虚拟主机 IP 地址的子目录。但是，主机名仍然可以访问存储库，假设它们对应于这些 IP 地址。

```
 selectively enable/disable services per repository 
```

要启用 _git archive --remote_ 并禁用存储库中的 _git fetch_ ，请在存储库的配置文件中保存以下内容（即文件 _config_ next 到`HEAD`， _refs_ 和 _ 对象 _）。

```
	[daemon]
		uploadpack = false
		uploadarch = true
```

## 环境

如果 IP 地址可用， _git 守护程序 _ 会将 REMOTE_ADDR 设置为与其连接的客户端的 IP 地址。 REMOTE_ADDR 将在执行服务时调用的挂钩环境中可用。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件