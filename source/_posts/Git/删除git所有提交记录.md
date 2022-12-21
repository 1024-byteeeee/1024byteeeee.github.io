---
title: 删除git所有提交记录
date: 2022-12-17 04:10:12
tags: Git
categories: Git
typora-root-url: ..
---

## 让我们删除历史厚重的主分支提交记录

1、切换一个新的分支

```bash
git checkout --orphan new_branch;
```

2、添加所有到new_branch

```bash
git add -A
```

3、提交新建分支

```bash
git commit -am "commit a new branch"
```

4、删除旧分支

```bash
git branch -D master
```

5、重命名新分支为master

```bash
git checkout new_branch
git branch -m master
```

8、强制推送到仓库中

```bash
git push -f orgin master
```