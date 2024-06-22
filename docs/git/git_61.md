# git-archive

> 原文： [`git-scm.com/docs/git-archive`](https://git-scm.com/docs/git-archive)

## 名称

git-archive - 从命名树创建文件存档

## 概要

```
git archive [--format=<fmt>] [--list] [--prefix=<prefix>/] [<extra>]
	      [-o <file> | --output=<file>] [--worktree-attributes]
	      [--remote=<repo> [--exec=<git-upload-archive>]] <tree-ish>
	      [<path>…​]
```

## 描述

创建包含指定树的树结构的指定格式的存档，并将其写入标准输出。如果&lt;前缀&gt;指定它被添加到存档中的文件名前面。

_git archive_ 在给定树 ID 时与给定提交 ID 或标记 ID 时的行为不同。在第一种情况下，当前时间用作存档中每个文件的修改时间。在后一种情况下，使用引用的提交对象中记录的提交时间。另外，如果使用 tar 格式，则提交 ID 存储在全局扩展 pax 头中;它可以使用 _git get-tar-commit-id_ 提取。在 ZIP 文件中，它存储为文件注释。

## OPTIONS

```
 --format=<fmt> 
```

生成的存档的格式： _tar_ 或 _zip_ 。如果未给出此选项，并且指定了输出文件，则尽可能从文件名推断格式（例如，写入“foo.zip”使输出为 zip 格式）。否则输出格式为`tar`。

```
 -l 
```

```
 --list 
```

显示所有可用格式。

```
 -v 
```

```
 --verbose 
```

向 stderr 报告进度。

```
 --prefix=<prefix>/ 
```

将&lt;前缀&gt; /前置到存档中的每个文件名。

```
 -o <file> 
```

```
 --output=<file> 
```

将存档写入&lt; file&gt;而不是 stdout。

```
 --worktree-attributes 
```

在工作树中查找.gitattributes 文件中的属性（参见 ATTRIBUTES ）。

```
 <extra> 
```

这可以是归档后端理解的任何选项。见下一节。

```
 --remote=<repo> 
```

而不是从本地存储库创建 tar 存档，从远程存储库中检索 tar 存档。请注意，远程存储库可能会限制`&lt;tree-ish&gt;`中允许哪些 sha1 表达式。有关详细信息，请参阅 [git-upload-archive [1]](https://git-scm.com/docs/git-upload-archive) 。

```
 --exec=<git-upload-archive> 
```

与--remote 一起使用以指定远程端的 _git-upload-archive_ 的路径。

```
 <tree-ish> 
```

树或承诺为其生成存档。

```
 <path> 
```

如果没有可选的路径参数，则当前工作目录的所有文件和子目录都将包含在存档中。如果指定了一个或多个路径，则仅包括这些路径。

## 备用额外选项

### 压缩

```
 -0 
```

存储文件而不是缩小文件。

```
 -9 
```

最高和最慢的压缩级别。您可以指定 1 到 9 之间的任意数字来调整压缩速度和比率。

## 组态

```
 tar.umask 
```

此变量可用于限制 tar 存档条目的权限位。默认值为 0002，关闭世界写入位。特殊值“user”表示将使用归档用户的 umask。有关详细信息，请参阅 umask（2）。如果使用`--remote`，则只有远程存储库的配置生效。

```
 tar.<format>.command 
```

此变量指定一个 shell 命令，通过该命令管道`git archive`生成的 tar 输出。该命令是使用 shell 在其标准输入上生成的 tar 文件执行的，并应在其标准输出上生成最终输出。任何压缩级选项都将传递给命令（例如，“ - 9”）。如果没有给出其他格式，则与`&lt;format&gt;`具有相同扩展名的输出文件将使用此格式。

“tar.gz”和“tgz”格式是自动定义的，默认为`gzip -cn`。您可以使用自定义命令覆盖它们。

```
 tar.<format>.remote 
```

如果为 true，则启用`&lt;format&gt;`以供远程客户端通过 [git-upload-archive [1]](https://git-scm.com/docs/git-upload-archive) 使用。对于用户定义的格式，默认为 false，但对于“tar.gz”和“tgz”格式，则为 true。

## ATTRIBUTES

```
 export-ignore 
```

具有 export-ignore 属性的文件和目录不会添加到存档文件中。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

```
 export-subst 
```

如果为文件设置了 export-subst 属性，那么在将此文件添加到存档时，Git 将展开多个占位符。有关详细信息，请参阅 [gitattributes [5]](https://git-scm.com/docs/gitattributes) 。

请注意，默认情况下，属性取自正在归档的树中的`.gitattributes`文件。如果您想调整事后生成输出的方式（例如，您在`.gitattributes`中未添加适当的 export-ignore 而提交），请根据需要调整签出的`.gitattributes`文件并使用`--worktree-attributes`选项。或者，您可以在归档`$GIT_DIR/info/attributes`文件中的任何树时保留应该应用的必要属性。

## 例子

```
 git archive --format=tar --prefix=junk/ HEAD | (cd /var/tmp/ && tar xf -) 
```

创建一个 tar 存档，其中包含当前分支上最新提交的内容，并将其解压缩到`/var/tmp/junk`目录中。

```
 git archive --format=tar --prefix=git-1.4.0/ v1.4.0 | gzip >git-1.4.0.tar.gz 
```

为 v1.4.0 版本创建压缩的 tarball。

```
 git archive --format=tar.gz --prefix=git-1.4.0/ v1.4.0 >git-1.4.0.tar.gz 
```

与上面相同，但使用内置的 tar.gz 处理。

```
 git archive --prefix=git-1.4.0/ -o git-1.4.0.tar.gz v1.4.0 
```

与上面相同，但是从输出文件中推断出格式。

```
 git archive --format=tar --prefix=git-1.4.0/ v1.4.0^{tree} | gzip >git-1.4.0.tar.gz 
```

为 v1.4.0 发行版创建压缩的 tarball，但没有全局扩展的 pax 标头。

```
 git archive --format=zip --prefix=git-docs/ HEAD:Documentation/ > git-1.4.0-docs.zip 
```

将当前头文档/目录中的所有内容放入 _git-1.4.0-docs.zip_ ，前缀为 _git-docs /_ 。

```
 git archive -o latest.zip HEAD 
```

创建一个 Zip 存档，其中包含当前分支上最新提交的内容。请注意，输出格式是由输出文件的扩展名推断的。

```
 git config tar.tar.xz.command "xz -c" 
```

配置“tar.xz”格式以生成 LZMA 压缩的 tarfiles。您可以使用它指定`--format=tar.xz`，或创建类似`-o foo.tar.xz`的输出文件。

## 也可以看看

[gitattributes [5]](https://git-scm.com/docs/gitattributes)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件