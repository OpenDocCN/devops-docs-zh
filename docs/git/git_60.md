# git-instaweb

> 原文： [`git-scm.com/docs/git-instaweb`](https://git-scm.com/docs/git-instaweb)

## 名称

git-instaweb - 立即在 gitweb 中浏览您的工作存储库

## 概要

```
git instaweb [--local] [--httpd=<httpd>] [--port=<port>]
               [--browser=<browser>]
git instaweb [--start] [--stop] [--restart]
```

## 描述

设置`gitweb`的简单脚本和用于浏览本地存储库的 Web 服务器。

## OPTIONS

```
 -l 
```

```
 --local 
```

仅将 Web 服务器绑定到本地 IP（127.0.0.1）。

```
 -d 
```

```
 --httpd 
```

将执行的 HTTP 守护程序命令行。这里可以指定命令行选项，配置文件将添加到命令行的末尾。目前支持 apache2，lighttpd，mongoose，plackup，python 和 webrick。 （默认值：lighttpd）

```
 -m 
```

```
 --module-path 
```

模块路径（仅当 httpd 是 Apache 时才需要）。 （默认值：/ usr / lib / apache2 / modules）

```
 -p 
```

```
 --port 
```

要将 httpd 绑定到的端口号。 （默认值：1234）

```
 -b 
```

```
 --browser 
```

应该用于查看 gitweb 页面的 Web 浏览器。这将被传递到 _git web {litdd}浏览 _ 帮助程序脚本以及 gitweb 实例的 URL。有关详细信息，请参阅 [git-web {litdd}浏览[1]](https://git-scm.com/docs/git-web{litdd}browse) 。如果脚本失败，则 URL 将打印到 stdout。

```
 start 
```

```
 --start 
```

启动 httpd 实例并退出。根据需要重新生成配置文件以生成新实例。

```
 stop 
```

```
 --stop 
```

停止 httpd 实例并退出。这不会生成任何用于生成新实例的配置文件，也不会关闭浏览器。

```
 restart 
```

```
 --restart 
```

重新启动 httpd 实例并退出。根据需要重新生成配置文件以生成新实例。

## 组态

您可以在.git / config 中指定配置

```
[instaweb]
	local = true
	httpd = apache2 -f
	port = 4321
	browser = konqueror
	modulePath = /usr/lib/apache2/modules
```

如果未设置配置变量`instaweb.browser`，则在定义时将使用`web.browser`。有关详细信息，请参阅 [git-web {litdd}浏览[1]](https://git-scm.com/docs/git-web{litdd}browse) 。

## 也可以看看

[gitweb [1]](https://git-scm.com/docs/gitweb)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件