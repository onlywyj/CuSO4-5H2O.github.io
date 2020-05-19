---
layout: post
title: '修改Git Bash的起始路径'
date: 2020-02-26
author: Only
tags: Git
---

> 不用每次打开Git Bash都需要使用cd命令更换目录了

### 起因

Git Bash打开的默认工作目录一般为`C:/Users/user`，而我们的本地Git仓库一般都不在这个位置，每次打开的时候都需要使用cd命令更换到Git仓库位置，很麻烦。

### 过程

* Git安装完成后一般会在桌面创建一个快捷方式Git Bash，鼠标右键->属性，可以看到目标栏后面为`--cd-to-home`，起始位置为默认的。

	<img src="http://onlywyj.gitee.io/image_bed/blog/2020-02-26-01.png" alt="01" style="zoom: 50%;" />

* 复制此快捷方式，右键->属性，将目标后面的`--cd-to-home`这一段删除，将起始位置改为你本地仓库的路径。为了区分，还可以将这个快捷方式重命名，这里我命名为"CuSO4-5H2O"。

	<img src="http://onlywyj.gitee.io/image_bed/blog/2020-02-26-02.png" alt="02" style="zoom: 50%;" />

* 点击“确定”，双击修改后的快捷方式，可以看到已经自动切换到目标目录了。

	<img src="http://onlywyj.gitee.io/image_bed/blog/2020-02-26-03.png" alt="03" style="zoom:80%;" />

