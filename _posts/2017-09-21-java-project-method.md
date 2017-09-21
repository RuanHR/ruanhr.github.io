---
layout: post
title: Java 开发相关实现方法
date: 2017-09-21
categories: blog
tags: [java]
description: Java Development Related Implementation Method
---

### Java Development Related Implementation Method
Author : ToniR<br>
ToniR's qq : 729703544<br>
Email : rhr13591118151@gmail.com

#### M1: 实体类逆向生成数据库

> 在mybaits中，是通过数据库生成mapper，实体，xml，将jpa与mybatis相结合，在实体类添加字段后，通过jpa的映射关系将属性对应到数据库中，在使用通用mapper的情况下不需要再去数据库中添加字段

Github Address : [Spring-Boot-Generater-Reverse](https://github.com/RuanHR/Spring-Boot-Generater-Reverse)

#### M2: mybatis生成实体类，通用mapper，xml文件

> 根据数据库，生成对应配置实体类，通用mapper，xml文件

Github Adderss : [Spring-Boot-Generater](https://github.com/RuanHR/Spring-Boot-Generater)
