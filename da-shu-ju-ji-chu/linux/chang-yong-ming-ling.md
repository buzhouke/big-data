# 常用命令

\[TOC\]

## Linux常用命令-文件处理命令

### 命令格式与目录处理命令ls

**命令格式：命令 \[-选项\] \[参数\]**

例： ls -la /etc

说明：

1）个别命令使用不遵循此格式

2）当有多个选项时，可以写在一起

3）简化选项与完整选项

​ -a 等于 -all

### **目录处理命令：**

#### ls

\(list\) 所在路径 /bin/ls

执行权限：所有目录

ls \[-选项\] \[文件或目录\]：

-a 显示所有文件，包括隐藏文件

-l 详细信息显示

-d 查看目录属性

![image-20210124121916346](https://i.loli.net/2021/01/24/u52V9Aw7zFBhdNC.png)

权限中，显示了三段，分别对应以下三种角色：

u：所有者，只能有一个

g：所属组，授权其他用户可以使用

o：其他人，既不是所有者，也不在所属组

其中，r读，w写，r执行。

-l，意思是long，就是显示出详细的信息，这里打印的信息中可以看到，有读写权限，然后开始罗列文件计数，所有者，所属组，字节大小（可以通过-lh显示单位），最后修改时间，文件名。

再是在elasticStack中，发现读写权限这里以d开头，这说明他是一个目录

以l开头，说明他是一个软链接。

#### mkdir

（make directory）

mkdir -p递归创建文件目录

![&#x793A;&#x4F8B;](https://i.loli.net/2021/01/25/kqct9Tumyv3xoRr.png)

rmdir 删除空目录

#### cp

（copy） 复制 文件或目录

命令所在路径：/bin/cp

cp -rp \[原文件或目录\] \[目标目录\]

​ -r 复制目录

​ -p 保留文件属性

测试以上命令：

```text
[root@VM_0_14_centos tmp]# mkdir -p test01/test02 //递归创建目录test01/test02
[root@VM_0_14_centos tmp]# mkdir -p test03/test04 //递归创建目录test03/test04
[root@VM_0_14_centos tmp]# vim test01/test02/test05.txt //创建文件并写入test05.txt
[root@VM_0_14_centos tmp]# cp test01/test02/test05.txt test03/test04 //复制刚刚创建的文件去目录test03/test04
[root@VM_0_14_centos tmp]# cd test03/test04
[root@VM_0_14_centos test04]# ls
test05.txt
[root@VM_0_14_centos test04]# ls -l test05
ls: cannot access test05: No such file or directory
[root@VM_0_14_centos test04]# ls -l test05.txt //显示复制的文件的详细信息
-rw-r--r-- 1 root root 7 Jan 26 12:23 test05.txt //发现时间不太一样，复制过去相当于最后修改的时间
[root@VM_0_14_centos test04]# cd ..
[root@VM_0_14_centos test03]# cd ..
[root@VM_0_14_centos tmp]# ls -l test01/test02
total 4
-rw-r--r-- 1 root root 7 Jan 26 12:22 test05.txt
```

然后俺们看看加上”-p“能做什么：

```text
//依旧是在/tmp路径下：
[root@VM_0_14_centos tmp]# vim test01/test02/test-p.txt //创建test -p文件
[root@VM_0_14_centos tmp]# cp -p test01/test02/test-p.txt test03/test04
[root@VM_0_14_centos tmp]# ls -l test03/test04
total 8
-rw-r--r-- 1 root root 7 Jan 26 12:23 test05.txt
-rw-r--r-- 1 root root 8 Jan 26 12:29 test-p.txt
[root@VM_0_14_centos tmp]# ls -l test01/test02
total 8
-rw-r--r-- 1 root root 7 Jan 26 12:22 test05.txt
-rw-r--r-- 1 root root 8 Jan 26 12:29 test-p.txt
```

可以观察到test-p.txt他的最后修改时间，两个位置的都是一样的。

再观察加上-r：

```text
[root@VM_0_14_centos tmp]# ls -l test03
total 8
drwxr-xr-x 2 root root 4096 Jan 26 12:39 test02
drwxr-xr-x 2 root root 4096 Jan 26 12:29 test04
[root@VM_0_14_centos tmp]# cp -r -p test03/test04 test01
[root@VM_0_14_centos tmp]# ls -l test01
total 8
drwxr-xr-x 2 root root 4096 Jan 26 12:29 test02
drwxr-xr-x 2 root root 4096 Jan 26 12:29 test04

[root@VM_0_14_centos tmp]# cp -r test03/test04 test01/test04
[root@VM_0_14_centos tmp]# ls -l test01/test04
total 12
drwxr-xr-x 2 root root 4096 Jan 26 12:43 test04
-rw-r--r-- 1 root root    7 Jan 26 12:23 test05.txt
-rw-r--r-- 1 root root    8 Jan 26 12:29 test-p.txt
```

很显然，-r是去复制目录，加上-p后，创建的时间也不会再改变。

#### mv

（move） 剪切文件、改名

命令所在路径：/bin/mv

mv \[原文件或目录\] \[目标目录\]

删掉刚刚的两个test01，test03目录，然后剪切：

```text
[root@VM_0_14_centos tmp]# rm -rf test01
[root@VM_0_14_centos tmp]# cd test01
-bash: cd: test01: No such file or directory
[root@VM_0_14_centos tmp]# rm -rf test03
[root@VM_0_14_centos tmp]# mkdir -p test01/test01
[root@VM_0_14_centos tmp]# mkdir -p test02/test03
[root@VM_0_14_centos tmp]# vim test01/test01/test04.txt
[root@VM_0_14_centos tmp]# mv test01/test01/test04.txt test02/test03
[root@VM_0_14_centos tmp]# ls -l test02/test03
total 4
-rw-r--r-- 1 root root 7 Jan 26 12:48 test04.txt
```

看看改名又是咋改的：

```text
[root@VM_0_14_centos tmp]# mv test02/test03/test04.txt test02/test03/test05.txt
[root@VM_0_14_centos tmp]# ls -l test02/test03
total 4
-rw-r--r-- 1 root root 7 Jan 26 12:48 test05.txt
//同一个路径下可不就是改名了嘛嘿嘿
```

#### rm

（remove） 删除 文件

命令所在路径：/bin/rm

rm -rf \[文件或目录\]

​ -r 删除目录

​ -f 强制执行

这个命令太常见了而且上面也有用过了就不再多说啦。

### 目录处理命令

#### touch

（touch） 创建 空文件

命令所在路径：/bin/touch

touch \[文件名\]

#### cat

显示文件内容

命令所在路径：/bin/cat

cat \[文件名\]

​ -n显示行号

```text
[root@VM_0_14_centos ~]# cd /tmp
[root@VM_0_14_centos tmp]# cat test01
cat: test01: Is a directory
[root@VM_0_14_centos tmp]# cat /etc/issue
\S
Kernel \r on an \m

[root@VM_0_14_centos tmp]# cat -n /etc/issue
     1  \S
     2  Kernel \r on an \m
     3
[root@VM_0_14_centos tmp]# tac -n /etc/issue
tac: invalid option -- 'n'
Try 'tac --help' for more information.
[root@VM_0_14_centos tmp]# tac  /etc/issue

Kernel \r on an \m
\S
```

#### more

（more） 分页显示文件内容

命令所在路径：/bin/touch

more \[文件名\]

​ （空格）或f 翻页

​ （Enter） 换行

​ q或Q 退出

对应的还有less，进行向上翻页，在输入/后可以搜索

#### less

\(less\) 分页显示文件内容（可向上翻页）

命令所在路径：/usr/bin/less

#### head 显示文件的前几行

```text
[root@VM_0_14_centos ~]# head -n 20 /ect/services
head: cannot open ‘/ect/services’ for reading: No such file or directory
[root@VM_0_14_centos ~]# head -n 20 /etc/services
# /etc/services:
# $Id: services,v 1.55 2013/04/14 ovasik Exp $
#
# Network services, Internet style
# IANA services version: last updated 2013-04-10
#
# Note that it is presently the policy of IANA to assign a single well-known
# port number for both TCP and UDP; hence, most entries here have two entries
# even if the protocol doesn't support UDP operations.
# Updated from RFC 1700, ``Assigned Numbers'' (October 1994).  Not all ports
# are included, only the more common ones.
#
# The latest IANA port assignments can be gotten from
#       http://www.iana.org/assignments/port-numbers
# The Well Known Ports are those from 0 through 1023.
# The Registered Ports are those from 1024 through 49151
# The Dynamic and/or Private Ports are those from 49152 through 65535
#
# Each line describes one service, and is of the form:
#
```

#### tail 查看文件后几行

可以用于监控日志。

### 链接命令

#### ln

（link） 生成 链接文件

命令所在路径：/bin/ln

ln -s \[原文件\] \[目标文件\]

​ -s 创建软链接

软链接实质上类似Windows的快捷方式

```text
[root@VM_0_14_centos ~]# tail -f /var/ln -s /ect/issue /tmp/issue.soft
tail: /ect/issue: invalid number of seconds
[root@VM_0_14_centos ~]# ln -s /ect/issue /tmp/issue.soft
[root@VM_0_14_centos ~]# ls -l /tmp/issue.soft
lrwxrwxrwx 1 root root 10 Jan 26 15:51 /tmp/issue.soft -> /ect/issue
[root@VM_0_14_centos ~]# ls -l /etc/issue
-rw-r--r--. 1 root root 23 Nov 23  2018 /etc/issue
```

1、软连接的权限为 lrwxrwxrwx

2、文件相当小，只是符号链接

3、会有箭头指向源文件

创建硬链接：

```text
[root@VM_0_14_centos ~]# ln /etc/issue /tmp/issue.hard
[root@VM_0_14_centos ~]# ls -l /tmp/issue.hard
-rw-r--r--. 2 root root 23 Nov 23  2018 /tmp/issue.hard
```

硬链接是拷贝，并会同步更新：

```text
[root@VM_0_14_centos ~]# echo "20200126" >> /etc/issue
[root@VM_0_14_centos ~]# cat /etc/issue
\S
Kernel \r on an \m

20200126
[root@VM_0_14_centos ~]# cat /tmp/issue.hard
\S
Kernel \r on an \m

20200126
```

通过i节点识别，可以做到同步：

```text
[root@VM_0_14_centos ~]# ls -i /tmp/issue.soft
27873 /tmp/issue.soft
[root@VM_0_14_centos ~]# ls -i /tmp/issue.hard
262292 /tmp/issue.hard
[root@VM_0_14_centos ~]# ls -i /etc/issue
262292 /etc/issue
```

软链接可以跨分区，硬链接不行。

硬链接不能针对目录来设置。

