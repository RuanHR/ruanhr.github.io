---
layout: post
title: Linux基本操作
date: 2018-01-02
categories: blog
tags: [linux]
description: Linux基本操作
---

# Linux基本操作

主要介绍操作系统上一些常见命令，比如创建文件目录mkdir，查看目录下文件ls，系统环境变量，系统时间，帮助文档等。

### 目录/文件简介

> 路径：从指定起始点到目的地的所经过的位置。
> 命令规则：

```xml
长度不能操作255个字符；
不能使用“/”当文件名；
严格区分大小写
```

### 目录/文件查看

> 查看目录文件 ls：

```xml
ls -l #长格式，可以简化为，ll；
ls -h #做单位换算，human can readable；
ls -a #以点开头的文件（包括隐藏文件、.表示当前目录、..表示上级目录）；
ls -A #显示全部文件（不包括.和..）；
ls -d #显示自身属性；
ls -i #index node，inode(显示文件唯一序号)；
ls -r #逆序显示；
ls -R #递归显示（recursive）；
```

> eg：ls -l

>- drwxr-xr-x 4 root 4096 1月 3 20:13 hbase-root
>- -rw-r--r-- 1 root    5 1月 3 20:16 hbase.pid

> 1.第一位，表示文件类型

```xml
-：普通文件（f），如上图第二个文件；
d：目录文件，如上图第一为文件目录；
b：块设备文件（block）
c：字节设备文件（character）
l：符号链接文件（sysbolic link ）
p：管道命令（pipe）
s：套接字文件（socket）
```

> 文件权限：紧接着后面9位，每三位一组，每一组（rwx）r：可读，w：可写：x:可执行
> 文件硬链接次数
> 文件的属主（owner）
> 文件的所属组（group）
> 文件的大小（size）,单位字节
> 时间戳（timestamp）:最近一次被修改的时间（文件内容）

其他基本命令

```xml
pwd　　#显示目录路径
cd　　#切换目录，当前目录
tree　　#查看目录树
stat　　#查看状态
```

### 目标/文件操作

> mkdir：创建空目录

```xml
-p： #递归创建，eg：mkdir -p test/test01/test02
-m： #创建文件夹时，指定权限。eg：mkdir -m 777 test03
-v： #创建文件夹时，展示详细信息。eg：mkdir -v test04
{}： #多级创建,eg：#mkdir -pv test05/a{x/m,y}
```

> rmdir：删除空目录

```xml
-p： #删除多级非空目录
```

> touch：创建文件

```xml
-a： #修改访问你时间（默认修改为当前时间）atime=access time
-m： #只更改变动时间，mtime=modify time
-t： #修改某个时间
```

rm：删除文件/目录

```xml
-i： #--interactive，进行交互式删除
-f： #--force，忽略不存在的文件，不给出提示
-r： #--recursive递归删除多级目录，eg:rm -rf /tmp #删除tem目录及其子目录全部文件
-v： #--verbose ，详细显示进行的步骤
```

### 环境变量

命令的内存空间，env<br/>
变量赋值  eg：NAME=Tom(在内存中着一段空间，起名叫NAME,空间放的数据叫Tom),申请变量的过程，就是申请内存使用的过程。

> path：以冒号隔开的一对路径。
> hash：查看缓存（缓存，catch is king.保存的是hash列表。在key,value中的查找速度是O(1).O(1):表示查找时间，不会随着，数据的增加而递增。）

### 时间

硬件主板的震荡器，rtc<br/>
硬件时钟：clock<br/>
系统时钟：date

hwclock #(写入时间)
> -w：读取系统时间到硬件中
> -s ：读取硬件时间到系统中

cal：日历

### 帮助命令

> 在线文档

```xml
info COMMAND(主要讲历史等)
文档：/usr/share/doc
内部命令：help COMMAND
外部命令：COMMAND –help
命令手册：manual
man COMMAND
whatis COMMAND
分章节
1、用户命令（/bin , /usr/bin , /usr/local）
2、系统调用
3、库用户
4、特殊文件（设备文件）
5、文件格式（配置文件的语法）
6、游戏
7、杂项（Miscellaneous）
8、管理命令（/sbin, /usr/sbin , /usr/local/sbin），管理员，会修改硬件相关的信息
```

> man命令

```xml
[] ：可选
<>：必须有
… ：可以出现多次
| ：多选一
{} ：分组
NAME：命令名称及功能简要说明
SYNAPOSIS：用户说明，包括可用的选项
DESCREPTION：命令功能的详细说明， 可能包括没一个选项的意义
OPTIONS：选项
FILES：此命令的相关配置
BUGS：
Examples：使用示例
SEE ALSO：另外参考
```

### 修改网卡

网络接口（网卡）的脚本文件ifcfg-eth0是默认的第一个网络接口，在/ect/sysconfig/network-script/目录下。如果有多个会以ifcfg-ethn命名n为（0，1，2，3，4……）

```linux
$ vim /ect/sysconfig/network-script/ifcfg-eth0
# 参数说明
DEVICE　　#网卡名,在`/etc/udev/rules.d/70-persistent-net.rules`有中有记录。
HWADDR　　#MAC地址,和网卡相对应。
USERCTL　　#[yes|no]（非root用户是否可以控制该设备）
BOOTPROTO　　#IP的配置方法[none|static|bootp|dhcp]（|静态分配IP|BOOTP协议|DHCP协议）
ONBOOT　　#系统启动的时候网络接口是否有效（yes/no）
TYPE　　#网络类型（通常是Ethemet）
NETMASK　　#网络掩码
IPADDR　　#IP地址
IPV6INIT　　#IPV6是否有效[yes/no]
GATEWAY　　#默认网关IP地址
BROADCAST　　#广播地址
NETWORK　　#网络地址
```

### 修改主机名

```linux
$ vi /etc/sysconfig/network   #修改这个文件，系统生效

# 另一种方法
$ hostname　　　　//查看机器名
$ hostname -i  //查看本机器名对应的ip地址
$ hostname 新名称　　//修改主机名(临时有效)
```

### 修改hosts文件

```linux
$ vi /etc/hosts　　#修改ip和域名/主机名映射
```

### 修改默认端口

1.修改配置文件：/etc/ssh/sshd_config ，找到#port 22
2.先将Port 22 前面的 # 号去掉，并另起一行。如定义SSH端口号为21117，则输入（建议在万位的端口）
3.修改完毕后，重启SSH服务，并退出当前连接的SSH端口
4.退出后，重新连接一下，链接成功后可以删除22那行记录。

### 禁用root本地或远程登录

1.禁止root本地登录

> 修改/etc/pam.d/login文件增加下面一行<br/>
> auth required pam_succeed_if.so user != root quiet

2.禁止root远程ssh登录

> 修改/etc/ssh/sshd_config文件，将PermitRootLogin yes 改为no

3.重新启动sshd服务

### 关闭防火墙

```linux
$ service iptables XX
$ service iptables status
$ service iptables stop
```

防火墙是否开机自启动

```linux
$ chkconfig iptables --list
$ vi /etc/inittab
$ chkconfig iptables off
```
