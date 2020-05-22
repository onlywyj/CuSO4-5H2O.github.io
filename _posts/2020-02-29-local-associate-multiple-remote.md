---
layout: post
title: 'Git本地仓库关联多个远程仓库'
date: 2020-02-29
author: Only
tags: Git 
---

> 一个当主仓库，另一个做副仓，用来调试备份

### 背景

GitHub又打不开了，就算是正常情况下，访问的速度也是一言难尽，没办法，毕竟是外网。于是我想着做一个仓库的备份，以备不时之需。这里我选择了Gitee，是国内的，访问速度也够快。Gitee其实自带有导入GitHub仓库的功能，只不过对于还在更新的仓库，显然是不太适合。

### 方法一

首先查看下自己本地仓库所对应的远程仓库：

```bash
$ git remote -v
github  git@github.com:onlywyj/onlywyj.github.io.git (fetch)
github  git@github.com:onlywyj/onlywyj.github.io.git (push)
```

然后使用`git remote add <name> <url>`的方式添加新的远程仓库，如下：

```bash
git remote add gitee gitee git@gitee.com:onlywyj/onlywyj.gitee.io.git
```

再次查看：

```bash
$ git remote -v
gitee   git@gitee.com:onlywyj/onlywyj.gitee.io.git (fetch)
gitee   git@gitee.com:onlywyj/onlywyj.gitee.io.git (push)
github  git@github.com:onlywyj/onlywyj.github.io.git (fetch)
github  git@github.com:onlywyj/onlywyj.github.io.git (push)
```

此种方法**push**和**pull**需要分别指定远程仓库：

```bash
$ git push github master
$ git push gitee master
$ git pull github master
$ git pull gitee master
```

### 方法二

使用`git remote set-url -add <name> <url>`，给一个**已有**的**远程仓库**添加额外用于**push**的链接，

`$ git remote set-url --add github git@gitee.com:onlywyj/onlywyj.gitee.io.git`

查看效果：

```bash
$ git remote -v
gitee   git@gitee.com:onlywyj/onlywyj.gitee.io.git (fetch)
gitee   git@gitee.com:onlywyj/onlywyj.gitee.io.git (push)
github  git@github.com:onlywyj/onlywyj.github.io.git (fetch)
github  git@github.com:onlywyj/onlywyj.github.io.git (push)
github  git@gitee.com:onlywyj/onlywyj.gitee.io.git (push)
```

可以发现，github下多了一个用于**push**的链接，也就是说当我使用`git push github master`时，会向这两个链接一起推送，不用再分开操作了。

### 结语

两种方法各有优劣，实际使用中选择适合自己的。

