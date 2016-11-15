---
title: Git 删除远程分支
date: 2016-11-15 15:22:32
categories:
- 学习
tags:
- git
---
应该，其实也真的很简单：`git push <remote> :<target-branch-name>`
<!-- more -->

比如你要删除远程仓库 origin 上的 feature/001 分支：

```bash
git push origin :feature/001
```

原来我都是通过 sourceTree 删除的。

参考：[《git-scm.com/3.5 Git 分支 - 远程分支》](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF#删除远程分支)