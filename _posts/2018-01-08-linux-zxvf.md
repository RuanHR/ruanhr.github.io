---
layout: post
title: Linux压缩及归档
date: 2018-01-08
categories: blog
tags: [linux]
description: Linux压缩及归档
---

# Linux压缩及归档

对于压缩和解压缩，在Micrsoft windows下的zip格式，我想大家都很熟悉，可以很好的用来管理我们的文件及文件夹。在Linux中压缩工具和命令也是是我们必须掌握的，常用于解压我们下载的文件，下面来看看常见的压缩格式和命令。

### 压缩

常见压缩格式

`bz2`，`gz`，`zip`，`Z`，`xz`

>- bz2：bzip2工具，采用Burrows-Wheeler块排序文本压缩算法和霍夫曼编码。
>- gz ：gzip工具，GNU压缩工具，用Lempel-Ziv编码。
>- zip：zip工具，Windows上PKZIP工具的Unix实现。
>- Z ：compress工具，原始的Unix文件压缩工具，现在已不常用。

压缩工具

#### bzip2工具

> 该工具逐渐普及，是相对来说是较新的压缩包，在压缩大型二进制文件比较流行。该软件包有以下压缩工具。

>- bzip2 ：用来压缩文件。压缩文件自动替换之前文件，并添加后缀名。
>- bzcat ：用来查看压缩文件，可以直接查看到内容。
>- bunzip2 ：用来解压缩后的.bz2文件。
>- bzip2recover：用来尝试恢复已损文件。

#### gzip 工具

> 是目前Linux最流行的文件压缩工具，是用来代替compress工具.z的，该软件包有以下压缩工具。使用方法类似上面bzip2。

>- gzip：用来压缩文件。
>- gzcat：用来查看压缩文件的内容。
>- gunzip：用来压缩文件。

#### zip工具

> 该工具和MS-DOS、Windwos的PKZIP是兼容的。该压缩文件可以压缩文件和目录。

>- zip ：创建一个压缩文件。包含指定的文件和目录。
>- zipcloak：一个加密的压缩文件。
>- zipnore：从压缩文件中提取批准。
>- zipsplit：将一个现有的zip压缩文件分割成更小的固定大小的文件。
>- unzip：解压缩

### 归档

虽然zip命令能够压缩文件和文件夹到单个文件，但是还不是最标准的归档工具，目前标准的归档工具是tar命令。

#### 压缩命令：

```linux
tar -zcvf 压缩文件名.tar.gz 被压缩文件名 #可先切换到当前目录下。压缩文件名和被压缩文件名都可加入路径。
```

#### 解压缩命令：

```linux
tar -zxvf 压缩文件名.tar.gz #解压缩后的文件只能放在当前的目录。
```
