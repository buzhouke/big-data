## 软件包管理

在Linux系统下，软件包管理分为两种类型：二进制包，源码包。

当时想当然地用一台Ubuntu，两台Centos系统的服务器搭建Hadoop集群，结果当然是，很惨烈。跟着教程一步步折腾来折腾去，结果发现jdk路径就是没法弄得一样。想在Ubuntu系统上尝试yum安装但一直提示找不到相应的包，原因如下：

> 目前主要有两个系列的二进制包管理系统：一个是Red Hat上的RPM包管理系统；另一个是Debian和Ubuntu上的DPKG包管理系统。

像我们安装Hadoop，Flink等等，用的都是源码包，将其下载后解压到指定目录

> 源码包打包压缩之后发布，而Linux中最常用的打包压缩格式是“*.tar.gz”

## 配置文件

安装的时候一直都很迷茫，一会在/etc/profile上操作，一会又去~/.bashrc, 尤其是对以往做出的修改都已经脑子不好使不记得改了哪里哪里：其中一个坑就是最初搞伪分布式Hadoop的时候在Hadoop用户下配置了一些环境路径，找了好一会才找了出来。

> /etc/profile 中设定的变量(全局)的可以作用于任何用户，而 ~/.bashrc 中设定的变量(局部)只能继承 /etc/profile 中的变量，他们是“父子”关系。

## Reference

[linux下的/etc/profile、/etc/bashrc、~/.bash_profile、 ~/.bashrc文件](https://www.jianshu.com/p/6d32b166f47d)

[细说Linux基础知识](https://weread.qq.com/web/reader/f51321b0716ce822f5111a6)

## 搭建Hadoop集群参照的教程

主要是照搬这个：[Hadoop集群（双节点）安装配置](https://weread.qq.com/web/reader/f51321b0716ce822f5111a6)

一开始是看的这个，照着弄好了配置文件之前的：https://segmentfault.com/a/1190000023041375

感觉还不错可参考的踩坑记录：https://blog.csdn.net/gakki_smile/article/details/77198146

https://hadoop.apache.org/docs/r2.8.5/hadoop-project-dist/hadoop-common/ClusterSetup.html

## 一些反思

- 走一步记一步对于俺这样的手残/半途而废人士很友好。
- Linux基础欠缺，翻日志翻出问题但是完全不明白意思也不会解决。一些常用的快捷操作不熟悉浪费很多时间（每次都翻看Vim快捷键实在是太废了）
- 常识欠缺如直接拿Ubuntu和Centos系统来搭建集群，急需掌握docker，k8s等容器化部署技术