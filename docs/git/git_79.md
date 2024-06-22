# git-symbolic-ref

> 原文： [`git-scm.com/docs/git-symbolic-ref`](https://git-scm.com/docs/git-symbolic-ref)

## 名称

git-symbolic-ref - 读取，修改和删除符号引用

## 概要

```
git symbolic-ref [-m <reason>] <name> <ref>
git symbolic-ref [-q] [--short] <name>
git symbolic-ref --delete [-q] <name>
```

## 描述

给定一个参数，读取给定符号 ref 的哪个分支头指向并输出其相对于`.git/`目录的路径。通常，您会将`HEAD`作为&lt; name&gt;查看工作树所在分支的参数。

给定两个参数，创建或更新符号引用&lt; name&gt;指向给定分支&lt; ref&gt;。

给定`--delete`和另一个参数，删除给定的符号引用。

符号引用是一个常规文件，用于存储以`ref: refs/`开头的字符串。例如，您的`.git/HEAD`是一个常规文件，其内容为`ref: refs/heads/master`。

## OPTIONS

```
 -d 
```

```
 --delete 
```

删除符号 ref&lt; name&gt;。

```
 -q 
```

```
 --quiet 
```

如果&lt; name＆gt ;,请不要发出错误消息不是一个象征性的参考，而是一个独立的 HEAD;而是以静默方式退出非零状态。

```
 --short 
```

显示&lt; name&gt;的值时作为一个象征性的参考，试图缩短价值，例如从`refs/heads/master`到`master`。

```
 -m 
```

更新&lt; name&gt;的 reflog 与&lt; reason&gt;。这仅在创建或更新符号引用时有效。

## 笔记

在过去，`.git/HEAD`是指向`refs/heads/master`的符号链接。当我们想切换到另一个分支时，我们做了`ln -sf refs/heads/newbranch .git/HEAD`，当我们想知道我们在哪个分支时，我们做了`readlink .git/HEAD`。但是符号链接不是完全可移植的，因此它们现在已被弃用，并且默认情况下使用符号引用（如上所述）。

如果符号引用的内容被正确打印，则 _git symbolic-ref_ 将以状态 0 退出，如果请求的名称不是符号引用，则状态为 1;如果发生另一个错误，则为 128。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件