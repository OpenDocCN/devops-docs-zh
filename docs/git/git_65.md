# git-cat-file

> 原文： [`git-scm.com/docs/git-cat-file`](https://git-scm.com/docs/git-cat-file)

## 名称

git-cat-file - 提供存储库对象的内容或类型和大小信息

## 概要

```
git cat-file (-t [--allow-unknown-type]| -s [--allow-unknown-type]| -e | -p | <type> | --textconv | --filters ) [--path=<path>] <object>
git cat-file (--batch | --batch-check) [ --textconv | --filters ] [--follow-symlinks]
```

## 描述

在第一种形式中，该命令提供存储库中对象的内容或类型。除非使用`-t`或`-p`查找对象类型，或者使用`-s`查找对象大小，或使用`--textconv`或`--filters`（暗示类型为“blob”），否则类型是必需的。

在第二种形式中，stdin 上提供了一个对象列表（由换行符分隔），每个对象的 SHA-1，类型和大小都打印在 stdout 上。可以使用可选的`&lt;format&gt;`参数覆盖输出格式。如果指定了`--textconv`或`--filters`，则输入应该列出对象名称，后跟路径名称，由单个空格分隔，以便可以确定相应的驱动程序。

## OPTIONS

```
 <object> 
```

要显示的对象的名称。有关拼写对象名称的更完整列表，请参阅 [gitrevisions [7]](https://git-scm.com/docs/gitrevisions) 中的“指定修订”部分。

```
 -t 
```

而不是内容，显示由&lt; object&gt;标识的对象类型。

```
 -s 
```

而不是内容，显示由&lt; object&gt;标识的对象大小。

```
 -e 
```

如果&lt; object&gt;，则退出为零状态存在并且是一个有效的对象。如果&lt; object&gt;是非格式的无效格式退出非零，并在 stderr 上发出错误。

```
 -p 
```

漂亮打印&lt; object&gt;的内容根据其类型。

```
 <type> 
```

通常，这与&lt; object&gt;的实际类型匹配。但是要求一种可以从给定的&lt; object&gt;中解除引用的类型。也是允许的。一个例子是要求一个带有&lt; object&gt;的“树”。是包含它的提交对象，或者要求&lt; object&gt;的“blob”是一个指向它的标记对象。

```
 --textconv 
```

显示由 textconv 过滤器转换的内容。在这种情况下，&lt; object&gt;必须采用&lt; tree-ish&gt;形式：&lt; path&gt;或：&lt; path&gt;为了将过滤器应用于&lt; path&gt;中索引中记录的内容。

```
 --filters 
```

显示由当前工作树中为给定&lt; path&gt;配置的过滤器转换的内容。 （即涂抹过滤器，行尾转换等）。在这种情况下，&lt; object&gt;必须是&lt; tree-ish&gt;：&lt; path&gt;或：&lt; path&gt;的形式。

```
 --path=<path> 
```

与--textconv 或--filters 一起使用时，允许单独指定对象名称和路径，例如当很难弄清楚 blob 来自的修订版。

```
 --batch 
```

```
 --batch=<format> 
```

打印 stdin 上提供的每个对象的对象信息和内容。除了`--textconv`或`--filters`之外，不能与任何其他选项或参数组合使用，在这种情况下，输入行也需要指定路径，用空格分隔。有关详细信息，请参阅下面的`BATCH OUTPUT`部分。

```
 --batch-check 
```

```
 --batch-check=<format> 
```

打印 stdin 上提供的每个对象的对象信息。除了`--textconv`或`--filters`之外，不能与任何其他选项或参数组合使用，在这种情况下，输入行也需要指定路径，用空格分隔。有关详细信息，请参阅下面的`BATCH OUTPUT`部分。

```
 --batch-all-objects 
```

不是在 stdin 上读取对象列表，而是对存储库中的所有对象和任何备用对象存储（不仅仅是可访问的对象）执行请求的批处理操作。需要指定`--batch`或`--batch-check`。请注意，按顺序访问对象按其哈希值排序。

```
 --buffer 
```

通常在输出每个对象后刷新批输出，以便进程可以从`cat-file`以交互方式读写。使用此选项，输出使用正常的 stdio 缓冲;在大量对象上调用`--batch-check`时效率更高。

```
 --unordered 
```

使用`--batch-all-objects`时，按顺序访问对象，这可能比散列顺序更有效地访问对象内容。订单的确切细节未指定，但如果您不需要特定订单，这通常会导致更快的输出，尤其是`--batch`。请注意，`cat-file`仍将仅显示每个对象一次，即使它在存储库中多次存储也是如此。

```
 --allow-unknown-type 
```

允许-s 或-t 查询未知类型的已损坏/损坏对象。

```
 --follow-symlinks 
```

使用--batch 或--batch-check 时，在请求具有树形式为 tree-ish：path-in-tree 的扩展 SHA-1 表达式的对象时，请遵循存储库中的符号链接。不提供有关链接本身的输出，而是提供有关链接对象的输出。如果符号链接指向树之外（例如指向/ foo 的链接或指向../foo 的根级链接），则将打印链接在树之外的部分。

当指定索引中的对象（例如`:link`而不是`HEAD:link`）而不是树中的对象时，此选项（当前）不能正常工作。

除非使用`--batch`或`--batch-check`，否则不能（当前）使用此选项。

例如，考虑一个包含以下内容的 git 存储库：

```
f: a file containing "hello\n"
link: a symlink to f
dir/link: a symlink to ../f
plink: a symlink to ../f
alink: a symlink to /etc/passwd
```

对于常规文件`f`，将打印`echo HEAD:f | git cat-file --batch`

```
ce013625030ba8dba906f756967f9e9ca394464a blob 6
```

并且`echo HEAD:link | git cat-file --batch --follow-symlinks`将打印与`HEAD:dir/link`相同的内容，因为它们都指向`HEAD:f`。

没有`--follow-symlinks`，这些将打印有关符号链接本身的数据。在`HEAD:link`的情况下，你会看到

```
4d1ae35ba2c8ec712fa2a379db44ad639ca277bd blob 1
```

`plink`和`alink`都指向树外，因此它们将分别打印：

```
symlink 4
../f
```

```
symlink 11
/etc/passwd
```

## OUTPUT

如果指定了`-t`，则其中一个&lt; type&gt;。

如果指定了`-s`，则&lt; object&gt;的大小。以字节为单位

如果指定了`-e`，则不输出，除非&lt; object&gt;是畸形的

如果指定了`-p`，则&lt; object&gt;的内容很漂亮。

如果&lt; type&gt;指定了&lt; object&gt;的原始（虽然未压缩）内容。将被退回。

## 批量输出

如果给出`--batch`或`--batch-check`，`cat-file`将从标准输入读取对象，每行一个，并打印有关它们的信息。默认情况下，整行被视为一个对象，就像它被送到 [git-rev-parse [1]](https://git-scm.com/docs/git-rev-parse) 一样。

您可以使用自定义`&lt;format&gt;`指定为每个对象显示的信息。 `&lt;format&gt;`按字面复制到每个对象的 stdout，展开`%(atom)`形式的占位符，后跟换行符。可用的原子是：

```
 objectname 
```

对象的 40 十六进制对象名称。

```
 objecttype 
```

对象的类型（与`cat-file -t`报告相同）。

```
 objectsize 
```

对象的大小（以字节为单位）（与`cat-file -s`报告相同）。

```
 objectsize:disk 
```

对象占用磁盘的大小（以字节为单位）。请参阅下面`CAVEATS`部分中有关磁盘大小的说明。

```
 deltabase 
```

如果对象存储为磁盘上的增量，则会扩展为增量基础对象的 40-hex sha1。否则，展开为空 sha1（40 个零）。见下面的`CAVEATS`。

```
 rest 
```

如果在输出字符串中使用此原子，则输入行将在第一个空白边界处拆分。在该空格之前的所有字符都被认为是对象名称;在第一次运行空白之后的字符（即，行的“其余”）被输出以代替`%(rest)`原子。

如果未指定格式，则默认格式为`%(objectname) %(objecttype) %(objectsize)`。

如果指定了`--batch`，则对象信息后跟对象内容（由`%(objectsize)`字节组成），后跟换行符。

例如，没有自定义格式的`--batch`会产生：

```
<sha1> SP <type> SP <size> LF
<contents> LF
```

而`--batch-check='%(objectname) %(objecttype)'`会产生：

```
<sha1> SP <type> LF
```

如果在 stdin 上指定了无法解析为存储库中对象的名称，则`cat-file`将忽略任何自定义格式并打印：

```
<object> SP missing LF
```

如果指定的名称可能引用多个对象（模糊的短 sha），则`cat-file`将忽略任何自定义格式并打印：

```
<object> SP ambiguous LF
```

如果使用了--follow-symlinks，并且存储库中的符号链接指向存储库外部，则`cat-file`将忽略任何自定义格式并打印：

```
symlink SP <size> LF
<symlink> LF
```

符号链接将是绝对的（以/开头）或相对于树根。例如，如果 dir / link 指向../../foo，那么&lt; symlink&gt;将是../foo。 &lt;大小&gt;符号链接的大小（以字节为单位）。

如果使用--follow-symlinks，将显示以下错误消息：

```
<object> SP missing LF
```

在请求的初始符号链接不存在时打印。

```
dangling SP <size> LF
<object> LF
```

在初始符号链接存在时打印，但它（transitive-of）指向的符号链接不存在。

```
loop SP <size> LF
<object> LF
```

打印用于符号链接循环（或任何需要解析超过 40 个链接分辨率的符号链接）。

```
notdir SP <size> LF
<object> LF
```

在符号链接解析期间，当文件用作目录名时，将打印。

## CAVEATS

请注意，磁盘上对象的大小是准确报告的，但应该注意得出哪些引用或对象负责磁盘使用的结论。打包的非 delta 对象的大小可能远大于对其增量的对象的大小，但是选择哪个对象是基础并且 delta 是任意的并且在重新打包期间可能会发生变化。

还要注意，对象的多个副本可能存在于对象数据库中;在这种情况下，未定义将报告哪个副本的大小或增量基数。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件