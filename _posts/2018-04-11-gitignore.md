---
layout: post
title: Git中gitignore文件
date: 2018-01-09
categories: blog
tags: [git]
description: java项目,gitignore文件
---

# Git中gitignore文件

### java项目忽略文件.gitignore配置
```yml
target/
!.mvn/wrapper/maven-wrapper.jar

### STS ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans

### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr

### NetBeans ###
nbproject/private/
build/
nbbuild/
dist/
nbdist/
.nb-gradle/
src/main/webapp/WEB-INF/lib/
src/main/webapp/WEB-INF/classes/
src/main/webapp/META-INF/maven/
src/main/webapp/META-INF/

```