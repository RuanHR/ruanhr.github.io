---
layout: post
title: Linux用户和组管理
date: 2018-01-02
categories: blog
tags: [linux]
description: Linux用户和组管理
---

# Linux用户和组管理

在Linux系统中，用户信息及密码，组信息及其密码，都保存在不同的文件中，用户和组是多对多的关系，即一个用户可以属于多个组，一个组也可以包含多个用户。

> 用户信息（/etc/passwd）
> 用户密码（/etc/shadow）
> 用户组帐号（/etc/group）
> 用户组密码（/etc/gshadow）

### 配置文件

1. /etc/passwd

> /etc/passwd：保存了所有用户信息，密码使用了x表示(不是明文，加密了的)

```linux
cat /etc/passwd
root:x:0:0:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:5:lo:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
```

>- root:x:0:0:root:/root:/bin/bash
>- 表示：用户帐号|用户密码|用户ID|用户组ID|用户名全称|用户主目录|用户所使用的shell。
>- 该文件默认就会初始化很多用户，当shell等于/sbin/nologin时就表示该用户不能登陆。

2. /etc/shadow

```linux
cat /etc/shadow
root:$sdDsdDcCVvAsa55wfAFfv/VASdasdAQ8Sd:16827:0:99999:7:::
bin:*:15980:0:99999:7:::
daemon:*:15920:0:99999:7:::
```

> 第二位为用户密码，使用了MD5加密算法，超级用户才拥有该文件读权限。

3. /etc/group

```linux
cat /etc/group
root:x:0:
bin:x:1:bin,daemon
daemon:x:2:bin,daemon
sys:x:3:bin,adm
```

> 分别表示：用户组名称 x 用户组ID 用户组成员列表

### 用户管理

1. 添加用户 useradd [option] userName

```linux
useradd -p 123456 ruanhr
cat /etc/passwd | grep "ruanhr"
ruanhr:x:506:506::/home/ruanhr:/bin/bash
```

>- -d dir ：#指定目录，默认目录/home/userName
>- -n ： #不为用户创建私有用户组
>- -g groupName : #指定用户组，该用户组必须存在,eg：useradd -g blogger ruanhr
>- -G groupName1,groupName2 ： #指定多个组
>- -p passWord ： #指定用户密码，eg：usreadd -p 123456 ruanhr

2. 修改用户 usermod [option] userName

```linux
usermod -l newUserName userName #修改用户名。
usermod -d 原目录 新目录名称 #修改用户目录，eg：usermod -d /home/ruanhr rhr。
usermod -L userName #锁定用户，锁定后用户密码前会有感叹号，解锁使用-U，可用-S查看状态。
usermod -g 新组 用户 #修改用户组
usermod -G 组 用户 #给用户添加组
```

3. 设置密码

```linux
passwd [userName] #带参数修改某用户密码（一般root用户才有权限），不带参数修改自己的密码。
passwd -l userName #锁定用户密码，解锁使用-u。
passwd -d userName #删除用户密码。
```

4. 删除用户

```linux
userdel userName   #删除用户保留文件夹
userdel -r userName #删除用户不保留文件夹
```

### 用户组管理

1. 添加组

```linux
groupadd [-r] groupName #创建用户组，带-r,会创建系统组GID<500,不带-r,GID>=500。
groupmod -n newGroupName groupName #修改组名称。
groupmod -g newGroupId groupId #修改组GID，不可与已有Id重复。
```

2. 删除组

```linux
groupdel groupName #注：删除组是应先删除用户，被删除的组不能是某个账户的私有用户组。
```

3. 用户和组

```linux
gpasswd -a userName groupName #添加用户到某个组（root用户和改组管理员又该权限）。
gpasswd -d userName groupName #把某个用户从组中删除（root用户和改组管理员又该权限）。
gpasswd -A userName groupName #设置userName为组管理员。
groups userName #查看用户所属组
```

### 权限管理

授权<br/>

> chmod [option] 文件或目录

```linux
chmod 777 文件目录 #授权rwxrwxrwx。
chmod u=rwx,g=rwx,o=rwx #等同上面的授权。
chmod o-x,g+w 文件 #文件去除其他组用户执行的权限，增加组写的权限。
chmod a+w  #给所有用户添加自读权限。
```

修改所属<br/>

1. 改变文件/目录所属者(用户)

```linux
chown ruanhr 文件/目录  #改变文件/目录的所有者为ruanhr
chown ‐R root 文件/目录  #改变文件/目录以及子目录文件的所有者为root
```

2. 改变文件/目录所属者（组）

```linux
chgrp root 文件/目录：改变文件/目录所属的组为root
```
