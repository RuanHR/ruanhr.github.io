---
layout: post
title: Linux根文件系统
date: 2018-01-03
categories: blog
tags: [linux]
description: Linux根文件系统
---

# Linux根文件系统

根文件系统`rootfs`(Root File System)，Linux是一种树形结构组织文件`FHS`（FileSystem Hierarchy Standard）。`“/”是所有文件的跟`，本篇主要介绍一下linux“/”目录下面文件夹的一些作用和用途。

### 详细讲解

#### `/boot`

存放`引导系统启动的相关文件`，如内核，initrd，以及grub(bootloader)。

#### `/dev`

`设备文件`,起一个连接作用，可以看作访问这些外部设备的端口；把你对设备的操作映射到具体的驱动程序代码中去。

```linux
设备文件分为两种：块设备文件(b，随机访问，数据块)和字符设备文件(c，线性访问，按字符为单位)
常见文件目录如下：
/dev/hd[a-t]：IDE设备
/dev/sd[a-z]：SCSI设备
/dev/md[0-31]：软raid设备
/dev/loop[0-7]：本地回环设备
/dev/ram[0-15]：内存
/dev/null：无限数据接收设备,相当于`黑洞`
/dev/tty[0-63]：虚拟终端
/dev/ttyS[0-3]：串口
/dev/console：控制台
/dev/fb[0-31]：framebuffer
```

#### `/etc`

主要存放了`系统配置文件`，`网络配置文件`等等

```linux
/etc/profile：系统环境变量
/etc/passwd：用户数据库，包括用户名、用户目录、加密的口令和用户的其他信息
/etc/group：组的数据库，类似/etc/passwd
/etc/inittab：init配置文件
/etc/login.defs：login 命令的配置文件
/etc/rc|rc.d|rc*.d：指定运行模式及相关初始化工作，各个启动级别的执行程序连接目录
/etc/magic：file的配置文件.包含不同文件格式的说明，文件开头几行指定了文件格式也叫魔术
/etc/motd：Message Of TheDay，录后成功登自动输出.内容由系统管理员确定
/etc/shadow：安装了影子口令软件的影子口令文件.比如将/etc/passwd文件中的加密口令移动到/etc/shadow中
/etc/securetty：确认安全终端，即哪个终端允许root登录
```

#### `/home`

`普通用户`的相关文件

#### `/root`

`系统管理员`（root user）的目录。它是一个特殊的用户，用所最高权限，一般情况加不要使用，以免误操作。

#### `/lib`

`库文件`一般在/lib、/usr/lib；主要存放系统的链接库文件，没有该目录则系统无法正常运行

```linux
有一个特点所有的库以lib开头；
一般库文件分为两种：静态库（以“.a”开头）和动态库|共享库（.dll , .so(shared object))；
GCC在链接时优先使用共享库，只有当共享库不存在时才考虑使用静态库；
/lib：目录中存储着程序运行时使用的共享库,共享库使程序可以重用代码；
/lib/modules：内核模块，运行的时候需要
```

#### `/media`

挂载点目录，移动设备（u盘，光盘）

#### `/mnt`

默认挂在公区或软驱的，额外的临时文件系统，（第二块硬盘）
#### `/misc`

杂项

#### `/opt`

可选目录，早期安装第三方程序的安装目录。安装到/opt目录下的程序，它所有的数据、库文件等等都是放在同个目录下面，如果要删除软件比较方便。

#### `/proc`

伪文件系统，内核映射文件。（以后的相关优化等）

```linux
操作系统运行时，进程信息及内核信息（比如cpu、硬盘分区、内存信息等）存放在这里。
/proc目录伪装的文件系统proc的挂载目录，proc并不是真正的文件系统，它的定义可以参见 /etc/fstab
```

#### `/sys`

伪文件系统，跟硬件设备相关的属性映射（例如修改磁盘调度队列IO）
#### `/tmp`

临时文件系统，有些linux系统会定期自动对这个目录清理,还有一个目录/var/tmp

#### `/var`

存放系统运行时经常变动的文件，

```linux
/var目录中有些内容是在/usr中的，但为了保持/usr目录的相对稳定就把常变得放在/var；
/var/lib：存放系统正常运行时要改变的文件。
/var/local：存放 /usr/local 中安装的程序的可变数据
/var/lock：锁定文件
/var/log：各种程序的日志 (log) 文件
/var/tmp：比 /tmp 允许更大的或需要存在较长时间的临时文件
```

#### `/bin`

可执行文件，用户命令即系统所需要的那些命令位于此目录，eg:ls、cp、mkdir

#### `/sbin`

超级用户专用的命令；

#### `/usr`

用户共享的只读文件(user shared read-only)，那些不适合放在/bin或etc目录下的额外的工具都放在这里。

```linux
/usr/bin：目录用于存放程序;
/usr/share：用于存放一些共享的数据；
/usr/share/fonts字体目录；
/usr/share/doc和/usr/share/man帮助文件；
/usr/local：这里主要存放手动安装的软件,目录结构和/usr类似；
/usr/lib：存放那些不能直接运行的,但却是许多程序运行所必需的一些函数库文件
注意：/bin、/sbin、/lib存着系统启动时依赖的文件，而/usr下面的bin、sbin、lib下面存放着系统启动后功能所需要的文件
```
