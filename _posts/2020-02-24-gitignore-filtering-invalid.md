---
layout: post
title: '.gitignore文件过滤规则不生效问题'
date: 2020-02-24
author: Only
tags: Git .gitignore
---

> 毕竟把日志啥的也提交上去是很糟心的

### 起因

在一次commit后，突然发现提示信息出现了一些新文件。细看了一下，这次提交居然create了几十个文件。仔细检查发现子目录下的.gitignore文件缺失，因为是仓库子目录下的文件夹，如果过滤规则缺失，使用`git add -A name/`，则会将文件夹下的所有文件都提交。

###  解决

首先，请检查相关文件夹目录下的`.gitingnore`文件是否缺失或者文件过滤规则是否配置错误。
其次，如果文件已被track，那么.gitignore是无法忽略已被纳入版本管理的文件，此时需要做的有三步：
* `$ git rm -r --cached .` 将所有本地缓存删除，将所有文件变为未track状态
* `$ git add .` 添加所有文件
* `$ git commit -m 'update .gitignore'` 重新提交

### .gitignore模板

**首先附上.gitignore模板集合：<https://github.com/github/gitignore>**

下面是我使用过的.gitignore文件内容

```bash
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
#*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
*.7z  
*.dmg  
*.gz  
*.iso  
*.tar   
*.via
*.tmp
*.err

# virtual machine crash logs, see 
hs_err_pid*

#自定义忽略文件
/target/
/.metadata/

#built application files
*.apk
*.ap_
 
# files for the dex VM
*.dex

# generated files
bin/
gen/
out/
build/

##ignore this file##
.classpath
.project
.settings   
  
 ##filter databfile、sln file##
*.mdb  
*.ldb  
*.sln   

##class file##
*.com  
*.class  
*.dll  
*.exe  
*.o  
*.so 


# OS generated files #  
.DS_Store  
.DS_Store?  
._*  
.Spotlight-V100  
.Trashes  
Icon?  
ehthumbs.db  
Thumbs.db

# Android Studio
*.iml
.idea/
gradle/
 
# Local IDEA workspace
 
# Gradle cache
.gradle
 
#NDK
obj/
```

