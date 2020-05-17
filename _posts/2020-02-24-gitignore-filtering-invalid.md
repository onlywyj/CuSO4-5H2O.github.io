---
layout: post
title: '.gitignore文件过滤规则不生效问题'
date: 2020-02-24
author: Only
tags: Git .gitignore
---

> 毕竟把日志啥的也提交上去是很糟心的。

####  .gitignore文件过滤规则不生效

首先，请检查本地根目录下的`.gitingnore`文件是否缺失或者文件过滤规则是否配置错误。

其次，出现此种情况的原因是因为文件已被track，.gitignore无法忽略已被纳入版本管理的文件，需要做的有三步：
* `$ git rm -r --cached .` 将所有本地缓存删除，将所有文件变为未track状态
* `$ git add .` 添加所有文件
* `$ git commit -m 'update .gitignore'` 重新提交