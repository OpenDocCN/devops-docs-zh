# git-write-tree

> 原文： [`git-scm.com/docs/git-write-tree`](https://git-scm.com/docs/git-write-tree)

## 名称

git-write-tree - 从当前索引创建树对象

## 概要

```
git write-tree [--missing-ok] [--prefix=<prefix>/]
```

## 描述

使用当前索引创建树对象。新树对象的名称将打印到标准输出。

索引必须处于完全合并状态。

从概念上讲， _git write-tree_ sync（）将当前索引内容转换为一组树文件。为了使你的目录中实际存在的匹配，你需要在 _git write-tree_ 之前完成 _git update-index_ 阶段。

## OPTIONS

```
 --missing-ok 
```

通常 _git write-tree_ 确保目录引用的对象存在于对象数据库中。此选项禁用此检查。

```
 --prefix=<prefix>/ 
```

写一个表示子目录`&lt;prefix&gt;`的树对象。这可用于为命名子目录中的子项目编写树对象。

## GIT

部分 [git [1]](https://git-scm.com/docs/git) 套件