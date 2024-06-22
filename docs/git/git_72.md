# git-hash-object

> 原文： [`git-scm.com/docs/git-hash-object`](https://git-scm.com/docs/git-hash-object)

## 名称

git-hash-object - 计算对象 ID，并可选择从文件创建 blob

## 概要

```
git hash-object [-t <type>] [-w] [--path=<file>|--no-filters] [--stdin [--literally]] [--] <file>…​
git hash-object [-t <type>] [-w] --stdin-paths [--no-filters]
```

## 描述

使用指定文件的内容（可以在工作树之外）计算具有指定类型的对象的对象 ID 值，并可选择将结果对象写入对象数据库。将其对象 ID 报告给其标准输出。 _git cvsimport_ 使用它来更新索引而不修改工作树中的文件。当&lt; type&gt;未指定，默认为“blob”。

## OPTIONS

```
 -t <type> 
```

指定类型（默认值：“blob”）。

```
 -w 
```

实际上将对象写入对象数据库。

```
 --stdin 
```

从标准输入而不是从文件中读取对象。

```
 --stdin-paths 
```

从标准输入读取文件名，每行一个，而不是从命令行读取。

```
 --path 
```

哈希对象，因为它位于给定的路径。文件的位置不会直接影响哈希值，但路径用于确定在将对象放置到对象数据库之前应该将哪些 Git 过滤器应用于对象，并且，作为应用过滤器的结果，实际的 blob 放置进入对象数据库可能与给定文件不同。此选项主要用于散列位于工作目录外部的临时文件或从 stdin 读取的文件。

```
 --no-filters 
```

按原样哈希内容，忽略属性机制选择的任何输入过滤器，包括行尾转换。如果从标准输入读取文件，则始终隐含，除非给出`--path`选项。

```
 --literally 
```

允许`--stdin`将任何垃圾散列到松散的对象中，否则可能无法通过标准对象解析或 git-fsck 检查。用于压力测试 Git 本身或复制野外遇到的腐败或伪造物体的特征。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件