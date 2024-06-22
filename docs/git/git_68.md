# git-commit-tree 

> 原文： [`git-scm.com/docs/git-commit-tree`](https://git-scm.com/docs/git-commit-tree)

## 名称

git-commit-tree - 创建一个新的提交对象

## 概要

```
git commit-tree <tree> [(-p <parent>)…​]
git commit-tree [(-p <parent>)…​] [-S[<keyid>]] [(-m <message>)…​]
		  [(-F <file>)…​] <tree>
```

## 描述

这通常不是最终用户想要直接运行的。参见 [git-commit [1]](https://git-scm.com/docs/git-commit) 。

基于提供的树对象创建新的提交对象，并在 stdout 上发出新的提交对象 ID。除非给出`-m`或`-F`选项，否则将从标准输入读取日志消息。

提交对象可以包含任意数量的父项。只有一个父，它是一个普通的提交。拥有多个父级会使提交在多行历史记录之间合并。初始（root）提交没有父母。

虽然树表示工作目录的特定目录状态，但提交在“时间”中表示该状态，并说明如何到达那里。

通常一个提交会识别一个新的“HEAD”状态，而 Git 并不关心你在哪里保存关于该状态的注释，实际上我们倾向于只将结果写入`.git/HEAD`所指向的文件，所以我们总能看到最后承诺的状态是什么。

## OPTIONS

```
 <tree> 
```

现有的树对象

```
 -p <parent> 
```

每个`-p`表示父提交对象的 id。

```
 -m <message> 
```

提交日志消息中的段落。这可以被给予不止一次并且每个&lt;消息&gt;成为自己的段落。

```
 -F <file> 
```

从给定文件中读取提交日志消息。使用`-`从标准输入读取。

```
 -S[<keyid>] 
```

```
 --gpg-sign[=<keyid>] 
```

GPG 签名提交。 `keyid`参数是可选的，默认为提交者标识;如果指定，它必须粘在没有空格的选项上。

```
 --no-gpg-sign 
```

不要 GPG 签名提交，以反击命令行先前给出的`--gpg-sign`选项。

## 提交信息

提交封装：

*   所有父对象 id

*   作者姓名，电子邮件和日期

*   提交者姓名和电子邮件以及提交时间。

在命令行上提供父对象 ID 时，作者和提交者信息取自以下环境变量，如果设置：

```
GIT_AUTHOR_NAME
GIT_AUTHOR_EMAIL
GIT_AUTHOR_DATE
GIT_COMMITTER_NAME
GIT_COMMITTER_EMAIL
GIT_COMMITTER_DATE
```

（nb“&lt;”，“&gt;”和“\ n”s 被剥离）

如果未设置（某些）这些环境变量，则从配置项 user.name 和 user.email 获取信息，如果不存在，则获取环境变量 EMAIL，或者，如果未设置，则系统用户用于发送邮件的名称和主机名（取自`/etc/mailname`并在该文件不存在时回退到完全限定的主机名）。

从 stdin 读取提交注释。如果未通过“&lt;”提供更改日志条目重定向， _git commit-tree_ 将等待一个输入并终止于^ D.

## 日期格式

`GIT_AUTHOR_DATE`，`GIT_COMMITTER_DATE`环境变量支持以下日期格式：

```
 Git internal format 
```

它是`&lt;unix timestamp&gt; &lt;time zone offset&gt;`，其中`&lt;unix timestamp&gt;`是自 UNIX 纪元以来的秒数。 `&lt;time zone offset&gt;`是 UTC 的正偏移或负偏移。例如，CET（比 UTC 早 1 小时）是`+0100`。

```
 RFC 2822 
```

RFC 2822 描述的标准电子邮件格式，例如`Thu, 07 Apr 2005 22:13:13 +0200`。

```
 ISO 8601 
```

ISO 8601 标准规定的时间和日期，例如`2005-04-07T22:13:13`。解析器也接受空格而不是`T`字符。

| 注意 | 此外，日期部分以下列格式接受：`YYYY.MM.DD`，`MM/DD/YYYY`和`DD.MM.YYYY`。 |

## 讨论

Git 在某种程度上是字符编码不可知的。

*   blob 对象的内容是未解释的字节序列。核心级别没有编码转换。

*   路径名以 UTF-8 规范化形式 C 编码。这适用于树对象，索引文件，ref 名称，以及命令行参数，环境变量和配置文件中的路径名（`.git/config`（参见 [git） -config [1]](https://git-scm.com/docs/git-config) ）， [gitignore [5]](https://git-scm.com/docs/gitignore) ， [gitattributes [5]](https://git-scm.com/docs/gitattributes) 和 [gitmodules [5]](https://git-scm.com/docs/gitmodules) ）。

    请注意，核心级别的 Git 仅将路径名称视为非 NUL 字节序列，没有路径名称编码转换（Mac 和 Windows 除外）。因此，即使在使用传统扩展 ASCII 编码的平台和文件系统上，使用非 ASCII 路径名也会起作用。但是，在此类系统上创建的存储库将无法在基于 UTF-8 的系统（例如 Linux，Mac，Windows）上正常工作，反之亦然。此外，许多基于 Git 的工具只是假设路径名为 UTF-8，并且无法正确显示其他编码。

*   提交日志消息通常以 UTF-8 编码，但也支持其他扩展 ASCII 编码。这包括 ISO-8859-x，CP125x 和许多其他，但 _ 不是 _ UTF-16/32，EBCDIC 和 CJK 多字节编码（GBK，Shift-JIS，Big5，EUC-x，CP9xx 等。 ）。

虽然我们鼓励提交日志消息以 UTF-8 编码，但核心和 Git 瓷器都不是为了强制项目使用 UTF-8。如果特定项目的所有参与者发现使用遗留编码更方便，Git 不会禁止它。但是，有一些事情需要牢记。

1.  _git commit_ 和 _git commit-tree_ 发出警告，如果提供给它的提交日志消息看起来不像有效的 UTF-8 字符串，除非你明确说你的项目使用了遗产编码。说这个的方法是在`.git/config`文件中使用 i18n.commitencoding，如下所示：

    ```
    [i18n]
    	commitEncoding = ISO-8859-1
    ```

    使用上述设置创建的提交对象在其`encoding`标题中记录`i18n.commitEncoding`的值。这是为了帮助其他人以后再看。缺少此标头意味着提交日志消息以 UTF-8 编码。

2.  _git log_ ， _git show_ ， _git blame_ 和朋友们查看提交对象的`encoding`头，并尝试将日志消息重新编码为除非另有说明，否则为 UTF-8。您可以使用`.git/config`文件中的`i18n.logOutputEncoding`指定所需的输出编码，如下所示：

    ```
    [i18n]
    	logOutputEncoding = ISO-8859-1
    ```

    如果您没有此配置变量，则使用`i18n.commitEncoding`的值。

请注意，我们故意选择在提交以在提交对象级别强制使用 UTF-8 时不重新编写提交日志消息，因为重新编码为 UTF-8 不一定是可逆操作。

## FILES

在/ etc /邮件名

## 也可以看看

[git-write-tree [1]](https://git-scm.com/docs/git-write-tree)

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件