# git-send-email

> 原文： [`git-scm.com/docs/git-send-email`](https://git-scm.com/docs/git-send-email)

## 名称

git-send-email - 以电子邮件形式发送补丁集合

## 概要

```
git send-email [<options>] <file|directory|rev-list options>…​
git send-email --dump-aliases
```

## 描述

获取命令行上给出的补丁并通过电子邮件发送出去。可以将修补程序指定为文件，目录（将发送目录中的所有文件），或直接指定为修订列表。在最后一种情况下， [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 接受的任何格式都可以传递给 git send-email。

电子邮件的标题可通过命令行选项进行配置。如果未在命令行中指定，将提示用户启用 ReadLine 接口以提供必要信息。

补丁文件有两种格式：

1.  mbox 格式文件

    这就是 [git-format-patch [1]](https://git-scm.com/docs/git-format-patch) 生成的内容。大多数标头和 MIME 格式都会被忽略。

2.  Greg Kroah-Hartman 的 _send_lots_of_email.pl_ 脚本使用的原始格式

    此格式要求文件的第一行包含“Cc：”值和消息的“Subject：”作为第二行。

## OPTIONS

### 构成

```
 --annotate 
```

查看并编辑您即将发送的每个补丁。默认值是`sendemail.annotate`的值。有关`sendemail.multiEdit`的信息，请参阅配置部分。

```
 --bcc=<address>,…​ 
```

为每封电子邮件指定“密件抄送：”值。默认值是`sendemail.bcc`的值。

可以多次指定此选项。

```
 --cc=<address>,…​ 
```

为每封电子邮件指定起始“抄送：”值。默认值是`sendemail.cc`的值。

可以多次指定此选项。

```
 --compose 
```

调用文本编辑器（参见 [git-var [1]](https://git-scm.com/docs/git-var) 中的 GIT_EDITOR）来编辑补丁系列的介绍性消息。

使用`--compose`时，git send-email 将使用消息中指定的 From，Subject 和 In-Reply-To 标头。如果邮件的正文（您在标题后面键入的内容和空行）仅包含空行（或 Git：前缀）行，则不会发送摘要，但是 From，Subject 和 In-Reply-To 标题将除非他们被删除使用。

将提示缺少 From 或 In-Reply-To 标头。

请参见`sendemail.multiEdit`的 CONFIGURATION 部分。

```
 --from=<address> 
```

指定电子邮件的发件人。如果未在命令行中指定，则使用`sendemail.from`配置选项的值。如果既未设置命令行选项也未设置`sendemail.from`，则将提示用户输入值。提示的默认值将是 GIT_AUTHOR_IDENT 的值，如果未设置，则为 GIT_COMMITTER_IDENT，由“git var -l”返回。

```
 --reply-to=<address> 
```

指定收件人应答的地址。如果对消息的回复应该转到另一个地址而不是--from 参数指定的地址，请使用此选项。

```
 --in-reply-to=<identifier> 
```

使第一封邮件（或所有带有`--no-thread`的邮件）显示为对给定 Message-Id 的回复，这可以避免破坏线程以提供新的补丁系列。第二封及后续电子邮件将根据`--[no-]chain-reply-to`设置作为回复发送。

因此，例如，当指定`--thread`和`--no-chain-reply-to`时，第二个和后续补丁将回复第一个补丁，如下图所示`[PATCH v2 0/3]`回复`[PATCH 0/2]`：

```
[PATCH 0/2] Here is what I did...
  [PATCH 1/2] Clean up and tests
  [PATCH 2/2] Implementation
  [PATCH v2 0/3] Here is a reroll
    [PATCH v2 1/3] Clean up
    [PATCH v2 2/3] New tests
    [PATCH v2 3/3] Implementation
```

仅在设置了--compose 时才需要。如果未设置--compose，则会提示。

```
 --subject=<string> 
```

指定电子邮件线程的初始主题。仅在设置了--compose 时才需要。如果未设置--compose，则会提示。

```
 --to=<address>,…​ 
```

指定生成的电子邮件的主要收件人。通常，这将是所涉及项目的上游维护者。默认值是`sendemail.to`配置值的值;如果未指定，并且未指定--to-cmd，则会提示。

可以多次指定此选项。

```
 --8bit-encoding=<encoding> 
```

遇到非 ASCII 消息或未声明其编码的主题时，添加标题/引用以指示它在&lt; encoding&gt;中编码。默认值是 _sendemail.assume8bitEncoding_ 的值;如果未指定，则会在遇到任何非 ASCII 文件时提示。

请注意，不会尝试验证编码。

```
 --compose-encoding=<encoding> 
```

指定撰写邮件的编码。默认值是 _sendemail.composeencoding_ 的值;如果未指定，则假定为 UTF-8。

```
 --transfer-encoding=(7bit|8bit|quoted-printable|base64|auto) 
```

指定用于通过 SMTP 发送邮件的传输编码。遇到非 ASCII 消息时，7 位将失败。当存储库包含包含回车符的文件时，quoted-printable 可能很有用，但是使原始补丁电子邮件文件（从 MUA 保存）更难以手动检查。 base64 更加傻瓜式，但也更加不透明。 auto 会尽可能使用 8bit，否则引用可打印。

默认值是`sendemail.transferEncoding`配置值的值;如果未指定，则默认为`auto`。

```
 --xmailer 
```

```
 --no-xmailer 
```

添加（或阻止添加）“X-Mailer：”标题。默认情况下，会添加标题，但可以通过将`sendemail.xmailer`配置变量设置为`false`来关闭标题。

### 发出

```
 --envelope-sender=<address> 
```

指定用于发送电子邮件的信封发件人。如果您的默认地址不是订阅列表的地址，这将非常有用。要使用 _From_ 地址，请将值设置为“auto”。如果使用 sendmail 二进制文件，则必须具有-f 参数的适当权限。默认值是`sendemail.envelopeSender`配置变量的值;如果未指定，则选择信封发件人将留给您的 MTA。

```
 --smtp-encryption=<encryption> 
```

指定要使用的加密， _ssl_ 或 _tls_ 。任何其他值都将恢复为纯 SMTP。默认值是`sendemail.smtpEncryption`的值。

```
 --smtp-domain=<FQDN> 
```

指定 HELO / EHLO 命令中用于 SMTP 服务器的完全限定域名（FQDN）。某些服务器要求 FQDN 与您的 IP 地址匹配。如果未设置，git send-email 会尝试自动确定您的 FQDN。默认值是`sendemail.smtpDomain`的值。

```
 --smtp-auth=<mechanisms> 
```

允许的 SMTP-AUTH 机制的以空格分隔的列表。此设置仅强制使用列出的机制。例：

```
$ git send-email --smtp-auth="PLAIN LOGIN GSSAPI" ...
```

如果至少一个指定的机制与 SMTP 服务器通告的机制匹配，并且所使用的 SASL 库支持该机制，则该机制用于身份验证。如果既未指定 _sendemail.smtpAuth_ 也未指定`--smtp-auth`，则可以使用 SASL 库支持的所有机制。特殊值 _none_ 可能被指定为独立于`--smtp-user`完全禁用认证

```
 --smtp-pass[=<password>] 
```

SMTP-AUTH 的密码。参数是可选的：如果未指定参数，则使用空字符串作为密码。默认值为`sendemail.smtpPass`，但`--smtp-pass`始终覆盖此值。

此外，无需在配置文件或命令行中指定密码。如果已指定用户名（使用`--smtp-user`或`sendemail.smtpUser`），但未指定密码（使用`--smtp-pass`或`sendemail.smtpPass`），则使用 _git-credential_ 获取密码。

```
 --no-smtp-auth 
```

禁用 SMTP 身份验证。 `--smtp-auth=none`的简写

```
 --smtp-server=<host> 
```

如果设置，则指定要使用的传出 SMTP 服务器（例如`smtp.example.com`或原始 IP 地址）。或者，它可以指定类似 sendmail 的程序的完整路径名;该程序必须支持`-i`选项。可以通过`sendemail.smtpServer`配置选项指定默认值;内置默认值是在`/usr/sbin`，`/usr/lib`和$ PATH 中搜索`sendmail`（如果此类程序可用），否则返回`localhost`。

```
 --smtp-server-port=<port> 
```

指定与默认端口不同的端口（SMTP 服务器通常侦听 smtp 端口 25，但也可以侦听提交端口 587 或公共 SSL smtp 端口 465）;也接受符号端口名称（例如“提交”而不是 587）。也可以使用`sendemail.smtpServerPort`配置变量设置端口。

```
 --smtp-server-option=<option> 
```

如果设置，则指定要使用的传出 SMTP 服务器选项。可以通过`sendemail.smtpServerOption`配置选项指定默认值。

必须为要传递给服务器的每个选项重复--smtp-server-option 选项。同样，必须为每个选项使用配置文件中的不同行。

```
 --smtp-ssl 
```

_--smtp-encryption ssl_ 的旧版别名。

```
 --smtp-ssl-cert-path 
```

用于 SMTP SSL / TLS 证书验证的可信 CA 证书存储的路径（已由 _c_rehash_ 处理的目录，或包含一个或多个 PEM 格式证书的单个文件连接在一起：请参阅 verify（1 ）-CAfile 和-CApath 有关这些的更多信息）。将其设置为空字符串以禁用证书验证。默认为`sendemail.smtpsslcertpath`配置变量的值（如果已设置），或者支持 SSL 库的默认编译（在大多数平台上应该是最佳选择）。

```
 --smtp-user=<user> 
```

SMTP-AUTH 的用户名。默认值是`sendemail.smtpUser`的值;如果未指定用户名（使用`--smtp-user`或`sendemail.smtpUser`），则不会尝试进行身份验证。

```
 --smtp-debug=0|1 
```

启用（1）或禁用（0）调试输出。如果启用，将打印 SMTP 命令和答复。用于调试 TLS 连接和身份验证问题。

```
 --batch-size=<num> 
```

某些电子邮件服务器（例如 smtp.163.com）限制每个会话（连接）发送的电子邮件数量，这将导致发送许多邮件时出现故障。使用此选项，send-email 将在发送$&lt; num&gt;后断开连接。消息并等待几秒钟（请参阅--relogin-delay）并重新连接，以解决此类限制。您可能希望使用某种形式的凭据帮助程序，以避免每次发生这种情况时都必须重新键入密码。默认为`sendemail.smtpBatchSize`配置变量。

```
 --relogin-delay=<int> 
```

等待$&lt; int&gt;重新连接到 SMTP 服务器之前的秒数。与--batch-size 选项一起使用。默认为`sendemail.smtpReloginDelay`配置变量。

### 自动化

```
 --to-cmd=<command> 
```

指定每个补丁文件执行一次的命令，该文件应生成特定于补丁文件的“收件人：”条目。此命令的输出必须是每行一个电子邮件地址。默认值为 _sendemail.tocmd_ 配置值。

```
 --cc-cmd=<command> 
```

指定每个补丁文件执行一次的命令，该文件应生成特定于补丁文件的“Cc：”条目。此命令的输出必须是每行一个电子邮件地址。默认值是`sendemail.ccCmd`配置值的值。

```
 --[no-]chain-reply-to 
```

如果设置了此项，则每封电子邮件都将作为对上一封电子邮件的回复发送。如果使用“--no-chain-reply-to”禁用，则第一封后的所有电子邮件将作为对第一封电子邮件的回复发送。使用它时，建议给出的第一个文件是整个补丁系列的概述。默认情况下禁用，但`sendemail.chainReplyTo`配置变量可用于启用它。

```
 --identity=<identity> 
```

配置标识。给定时，会导致 _sendemail 中的值。&lt; identity&gt;_ 子部分优先于 _sendemail_ 部分中的值。默认标识是`sendemail.identity`的值。

```
 --[no-]signed-off-by-cc 
```

如果设置了此项，请将在 Signed-off-by：或 Cc：行中找到的电子邮件添加到 cc 列表中。默认值是`sendemail.signedoffbycc`配置值的值;如果未指定，则默认为--signed-off-by-cc。

```
 --[no-]cc-cover 
```

如果设置了此项，则系列的第一个补丁中的 Cc：标题中的电子邮件（通常是求职信）将添加到每个电子邮件集的 cc 列表中。默认值为 _sendemail.cccover_ 配置值;如果未指定，则默认为--no-cc-cover。

```
 --[no-]to-cover 
```

如果设置了此项，则系列的第一个补丁中的 To：标题中找到的电子邮件（通常是求职信）将添加到每个电子邮件集的列表中。默认值为 _sendemail.tocover_ 配置值;如果未指定，则默认为--no-to-cover。

```
 --suppress-cc=<category> 
```

指定其他类别的收件人以禁止 auto-cc：

*   _ 作者 _ 将避免包括补丁作者。

*   _ 自 _ 将避免包含发件人。

*   _cc_ 将避免包括除自我之外的补丁标题中的 Cc 行中提到的任何人（使用 _self_ ）。

*   _bodycc_ 将避免包括在补丁体（提交消息）中的 Cc 行中提到的任何人，除了 self（使用 _self_ ）。

*   _ 呜咽 _ 将避免包括除了自我之外的签名旁线中提到的任何人（使用 _ 自 _）。

*   _misc-by_ 将避免包括在 Acked-by，Review-by，Tested-by 和补丁体中的其他“-by”行中提到的任何人，除了 Signed-off-by（使用 _sob_ 为此）。

*   _cccmd_ 将避免运行--cc-cmd。

*   _ 体 _ 相当于 _sob_ + _bodycc_ + _misc-by_ 。

*   _ 所有 _ 将抑制所有自动 cc 值。

默认值是`sendemail.suppresscc`配置值的值;如果未指定，如果指定了--suppress-from，则默认为 _self_ ，如果指定了--no-signed-off-cc，则默认为 _body_ 。

```
 --[no-]suppress-from 
```

如果设置了此项，请不要将 From：地址添加到 cc：列表。默认值是`sendemail.suppressFrom`配置值的值;如果未指定，则默认为--no-suppress-from。

```
 --[no-]thread 
```

如果设置了此项，则会将 In-Reply-To 和 References 标头添加到发送的每封电子邮件中。每个邮件是否引用前一封电子邮件（每个 _git format-pat_ 措辞的`deep`线程）或第一封电子邮件（`shallow`线程）由“ - [no-]链式回复控制-至”。

如果使用“--no-thread”禁用，则不会添加这些标头（除非使用--in-reply-to 指定）。默认值是`sendemail.thread`配置值的值;如果未指定，则默认为--thread。

当 _git send-email_ 被要求添加它时，由用户确保不存在 In-Reply-To 标头（特别注意 _git format-patch_ 可以是配置为执行线程本身）。如果不这样做，可能无法在收件人的 MUA 中产生预期的结果。

### 管理

```
 --confirm=<mode> 
```

发送前确认：

*   _ 始终 _ 将始终在发送前确认

*   _ 从来没有 _ 在发送之前永远不会确认

*   _cc_ 将在发送之前确认 send-email 自动将补丁中的地址添加到抄送列表

*   _ 撰写 _ 将在使用--compose 发送第一条消息之前确认。

*   _auto_ 相当于 _cc_ + _ 组成 _

默认值是`sendemail.confirm`配置值的值;如果未指定，则默认为 _auto_ ，除非指定了任何抑制选项，在这种情况下默认为 _ 组成 _。

```
 --dry-run 
```

做任何事情，除了实际发送电子邮件。

```
 --[no-]format-patch 
```

当参数可以理解为引用或文件名时，选择将其理解为格式补丁参数（`--format-patch`）或文件名（`--no-format-patch`）。默认情况下，发生此类冲突时，git send-email 将失败。

```
 --quiet 
```

使 git-send-email 更简洁。每封电子邮件一行应该是输出的全部内容。

```
 --[no-]validate 
```

对补丁进行健全性检查。目前，验证意味着以下内容：

*   如果存在，则调用 sendemail-validate 钩子（参见 [githooks [5]](https://git-scm.com/docs/githooks) ）。

*   除非使用合适的传输编码（ _auto_ ， _base64_ 或 _ 引用的可打印 _），否则警告包含超过 998 个字符的行;这是由于 [`www.ietf.org/rfc/rfc5322.txt`](http://www.ietf.org/rfc/rfc5322.txt) 所描述的 SMTP 限制。

默认值是`sendemail.validate`的值;如果未设置，则默认为`--validate`。

```
 --force 
```

即使安全检查会阻止它，也要发送电子邮件。

### 信息

```
 --dump-aliases 
```

而不是正常操作，从已配置的别名文件中转储速记别名，每行按字母顺序排列一个。请注意，这仅包括别名，而不包括其扩展的电子邮件地址。有关别名的更多信息，请参见 _sendemail.aliases 文件 _。

## 组态

```
 sendemail.aliasesFile 
```

要避免键入长电子邮件地址，请将其指向一个或多个电子邮件别名文件。您还必须提供`sendemail.aliasFileType`。

```
 sendemail.aliasFileType 
```

sendemail.aliasesFile 中指定的文件格式。必须是 _mutt_ ， _mailrc_ ， _pine_ ， _elm_ 或 _gnus_ 或 _sendmail 之一 _。

可以在同名电子邮件程序的文档中找到每种格式的别名文件。标准格式的差异和限制如下所述：

```
 sendmail 
```

*   不支持引用的别名和引用地址：忽略包含`"`符号的行。

*   不支持重定向到文件（`/path/name`）或管道（`|command`）。

*   不支持文件包含（`:include: /path/name`）。

*   对于任何明确不受支持的构造以及解析器无法识别的任何其他行，标准错误输出上会显示警告。

```
 sendemail.multiEdit 
```

如果为 true（默认），将生成单个编辑器实例以编辑您必须编辑的文件（使用`--annotate`时的补丁，以及使用`--compose`时的摘要）。如果为 false，将逐个编辑文件，每次生成一个新的编辑器。

```
 sendemail.confirm 
```

设置发送前是否确认的默认值。必须是 _ 始终 _，_ 永远 _， _cc_ ，_ 组成 _ 或 _auto_ 之一。有关这些值的含义，请参见上一节中的`--confirm`。

## 例子

### 使用 gmail 作为 smtp 服务器

要使用 _git send-email_ 通过 GMail SMTP 服务器发送补丁，请编辑〜/ .gitconfig 以指定您的帐户设置：

```
[sendemail]
	smtpEncryption = tls
	smtpServer = smtp.gmail.com
	smtpUser = yourname@gmail.com
	smtpServerPort = 587
```

如果您的 Gmail 帐户上有多重身份验证设置，则需要生成一个特定于应用程序的密码，以便与 _git send-email_ 一起使用。访问 [`security.google.com/settings/security/apppasswords`](https://security.google.com/settings/security/apppasswords) 进行创建。

一旦您的提交准备好发送到邮件列表，请运行以下命令：

```
$ git format-patch --cover-letter -M origin/master -o outgoing/
$ edit outgoing/0000-*
$ git send-email outgoing/*
```

第一次运行它时，系统将提示您输入凭据。根据需要输入特定于应用程序或常规密码。如果您配置了凭证帮助程序（请参阅 [git-credential [1]](https://git-scm.com/docs/git-credential) ），密码将保存在凭证存储中，因此您不必在下次输入密码。

注意：以下 perl 模块需要 Net :: SMTP :: SSL，MIME :: Base64 和 Authen :: SASL

## 也可以看看

[git-format-patch [1]](https://git-scm.com/docs/git-format-patch) ， [git-imap-send [1]](https://git-scm.com/docs/git-imap-send) ，mbox（5）

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件