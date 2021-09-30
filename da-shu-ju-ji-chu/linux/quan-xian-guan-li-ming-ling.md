# 权限管理命令

## chmod 改变文件或目录权限

change the permissions mode of a file

/bin/chmod

所有用户

chmod \[{ugoa}{+-=}\] \[文件或目录\]

​ \[mode = 421\] \[文件或目录\]

​ -R 递归修改

上面的ugoa分别对应所有者，所属组，其他人，所有人，+-=增加/减少/赋予权限

```text
[root@VM_0_14_centos ab]# ls
[root@VM_0_14_centos ab]# vim test.txt
[root@VM_0_14_centos ab]# ls -l
total 4
-rw-r--r-- 1 root root 5 Feb  1 18:39 test.txt
[root@VM_0_14_centos ab]# chmod g+w test.txt
[root@VM_0_14_centos ab]# ls -l
total 4
-rw-rw-r-- 1 root root 5 Feb  1 18:39 test.txt
[root@VM_0_14_centos ab]# chmod g-w,o+w test.txt
[root@VM_0_14_centos ab]# ls -l
total 4
-rw-r--rw- 1 root root 5 Feb  1 18:39 test.txt
[root@VM_0_14_centos ab]# chmod g=rwx test.txt
[root@VM_0_14_centos ab]# ls -l
total 4
-rw-rwxrw- 1 root root 5 Feb  1 18:39 test.txt
```

Linux的权限也会用数字来表示,r---4,w---2,x---1,

rwxrw-r--

7 6 4

```text
[root@VM_0_14_centos ab]# chmod 640 test.txt
[root@VM_0_14_centos ab]# ls -l
total 4
-rw-r----- 1 root root 5 Feb  1 18:39 test.txt
```

加上-R之后可以递归修改:

```text
[root@VM_0_14_centos tmp]# chmod -R 777 ab
[root@VM_0_14_centos tmp]# ls -l ab
total 4
-rwxrwxrwx 1 root root 5 Feb  1 18:39 test.txt
```

接下来,重头戏,真的是不看不知道:

| 简称 | 权限 | 文件 | 目录 |
| :---: | :---: | :---: | :---: |
| r | 读 | 可以查看文件内容 | 可以列出目录中的内容 |
| w | 写 | 可以修改文件内容 | 可以在目录中创建,删除文件 |
| x | 执行 | 可以执行文件 | 可以进入目录 |

这也就意味着,当对文件夹赋予777权限,那么任意一个普通用户,都能够删除该目录下的文件.

\(学到了学到了\)

## chown 改变文件或目录的所有者

change file ownership

/bin/chown

所有用户

chown \[用户\] \[文件或目录\]

## chgrp 改变文件或目录的所属组

change file group ownership

/bin/chgrp

所有用户

chagrp \[用户组\] \[文件或目录\]

## umask 显示新建文件的缺省\(默认\)权限

0特殊权限

022 --- -w- -w-

777 rwx rwx rwx

022 --- -w- -w-

--------------------------异或

755 rwx r-x r-x

也就是默认新建目录是755权限

```text
[root@VM_0_14_centos ~]# umask
0022
[root@VM_0_14_centos ~]# umask -S
u=rwx,g=rx,o=rx
```

