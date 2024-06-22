# git-fsck

> 原文： [`git-scm.com/docs/git-fsck`](https://git-scm.com/docs/git-fsck)

## 名称

git-fsck - 验证数据库中对象的连通性和有效性

## 概要

```
git fsck [--tags] [--root] [--unreachable] [--cache] [--no-reflogs]
	 [--[no-]full] [--strict] [--verbose] [--lost-found]
	 [--[no-]dangling] [--[no-]progress] [--connectivity-only]
	 [--[no-]name-objects] [<object>*]
```

## 描述

验证数据库中对象的连接性和有效性。

## OPTIONS

```
 <object> 
```

作为不可达性痕迹的头部对象的对象。

如果没有给出对象， _git fsck_ 默认使用索引文件，`refs`命名空间中的所有 SHA-1 引用，以及所有 reflog（除非给出--no-reflogs）作为头。

```
 --unreachable 
```

打印出存在但无法从任何引用节点访问的对象。

```
 --[no-]dangling 
```

打印存在但永远不会 _ 直接 _ 使用的对象（默认）。 `--no-dangling`可用于从输出中省略此信息。

```
 --root 
```

报告根节点。

```
 --tags 
```

报告标签。

```
 --cache 
```

将索引中记录的任何对象也视为不可达性跟踪的头节点。

```
 --no-reflogs 
```

不要认为仅由 reflog 中的条目引用的提交是可访问的。此选项仅用于搜索曾经在 ref 中的提交，但现在不是，但仍在相应的 reflog 中。

```
 --full 
```

不仅检查 GIT_OBJECT_DIRECTORY（$ GIT_DIR / objects）中的对象，还检查在 GIT_ALTERNATE_OBJECT_DIRECTORIES 或$ GIT_DIR / objects / info / alternates 中列出的备用对象池中找到的对象，以及在$ GIT_DIR / objects / pack 中找到的打包 Git 存档中找到的对象打包备用对象池中的子目录。这是默认值;你可以用--no-full 把它关掉。

```
 --connectivity-only 
```

仅检查标记，提交和树对象的连接。通过避免解压缩 blob，这会加速操作，但代价是丢失损坏的对象或其他有问题的问题。

```
 --strict 
```

启用更严格的检查，即捕获由 g + w 位集记录的文件模式，该模式由旧版本的 Git 创建。现有存储库（包括 Linux 内核，Git 本身和稀疏存储库）具有触发此检查的旧对象，但建议使用此标志检查新项目。

```
 --verbose 
```

说实话。

```
 --lost-found 
```

将悬空对象写入.git / lost-found / commit /或.git / lost-found / other /，具体取决于类型。如果对象是 blob，则将内容写入文件，而不是其对象名称。

```
 --name-objects 
```

当显示可到达对象的名称时，除了 SHA-1 之外还显示描述**如何**可以到达的名称，与 [git-rev-parse [1]](https://git-scm.com/docs/git-rev-parse) 兼容，例如`HEAD@{1234567890}~25²:src/`。

```
 --[no-]progress 
```

除非指定了--no-progress 或--verbose，否则默认情况下，当标准错误流附加到终端时，会报告进度状态。 - 即使标准错误流未定向到终端， - progress 也会强制进度状态。

## 讨论

git-fsck 测试 SHA-1 和一般对象的健全性，它完全跟踪产生的可达性和其他所有内容。它会打印出它找到的任何损坏（丢失或坏的对象），如果使用`--unreachable`标志，它还会打印出存在但无法从任何指定的头节点（或默认集）访问的对象，正如刚才提到的）。

您必须在备份或其他存档中找到任何损坏的对象（即，您可以删除它们并与其他某个站点一起执行 _rsync_ ，希望其他人拥有您已损坏的对象）。

如果 core.commitGraph 为 true，则还将使用 _git commit-graph verify_ 检查提交图文件。参见 [git-commit-graph [1]](https://git-scm.com/docs/git-commit-graph) 。

## 提取的诊断

```
 expect dangling commits - potential heads - due to lack of head information 
```

您没有指定任何节点作为头，因此无法区分非父级提交和根节点。

```
 missing sha1 directory <dir> 
```

缺少包含 sha1 对象的目录。

```
 unreachable <type> <object> 
```

&lt; type&gt;对象&lt; object&gt;，实际上并未在任何树或提交的提交中直接或间接引用。这可能意味着您没有指定另一个根节点或树已损坏。如果您没有错过根节点，那么您也可以删除无法访问的节点，因为它们无法使用。

```
 missing <type> <object> 
```

&lt; type&gt;对象&lt; object&gt;，被引用但不存在于数据库中。

```
 dangling <type> <object> 
```

&lt; type&gt;对象&lt; object&gt;，存在于数据库中但从不直接使用。悬挂提交可以是根节点。

```
 hash mismatch <object> 
```

数据库有一个对象，其哈希值与对象数据库值不匹配。这表明存在严重的数据完整性问题。

## 环境变量

```
 GIT_OBJECT_DIRECTORY 
```

用于指定对象数据库的根目录（通常为$ GIT_DIR / objects）

```
 GIT_INDEX_FILE 
```

用于指定索引的索引文件

```
 GIT_ALTERNATE_OBJECT_DIRECTORIES 
```

用于指定其他对象数据库根（通常未设置）

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件