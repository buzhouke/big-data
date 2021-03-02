# Shell

## Shell概述

Shell是一个命令行解释器, 就是我们和内核进行交互的命令

Shell命令解释器将敲入的命令翻译成机器语言

Shell是一个功能相当强大的编程语言,易编写,易调试,灵活性强,Shell是解释执行的脚本语言,在Shell中可以直接调节用Linux系统命令

BShell(Bash)和CShell(语法类似C),两种语法完全不兼容.

Linux使用Bash作为基本Shell

/etc/shells可查看支持哪些Shell

```shell
[root@hadoop3-slave ~]# vi /etc/shells
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
/bin/tcsh
/bin/csh
```

## 脚本执行方式

echo与转义字符

![echo使用](https://buhzou-1300695412.cos.ap-nanjing.myqcloud.com/image-20210210205938378.png)



> 看那茅屋围着新绿，
>
> 被斜阳照得一片鲜明。
>
> 一日已告终，太阳消退