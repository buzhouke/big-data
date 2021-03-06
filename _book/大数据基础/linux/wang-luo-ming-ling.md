# 网络命令

## write

write 给在线用户发信息，以Ctrl+D保存结束

write + 用户名

## wall

wall 给所有在线用户发信息

write + 想要发送的信息

## ping

测试网络连通性，如果不终止就需要Ctrl+C终止，也可以指定次数

重点关注 packet loss,丢包率

## ifconfig

查看网络和设置网卡信息

以太网，MAC地址

看计算机IP， 子网掩码

接受的数据包总数

网卡在内存当中的物理地址

## mail

mail 查看发送电子邮件

一般用来看系统给root发的信息，而这种邮件都比较重要

## last

查看之前登陆列表以及系统重启

**非常常用的日志查询命令**

lastlog，用户最后一次登陆的时间

lastlog -u uid 查看对应用户的上次登陆信息

## traceroute

traceroute 显示数据包到主机间的路径

感觉学计网的时候很有用

也可以查看网络故障

## netstat

显示网络相关信息

-t : TCP协议

-u : UDP协议

-l : 监听

-r : 路由

-n : 显示IP地址和端口号

```text
[root@VM_0_14_centos ~]# netstat -tlun
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:888             0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:52333           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:21              0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp6       0      0 ::1:25                  :::*                    LISTEN
tcp6       0      0 :::9092                 :::*                    LISTEN
tcp6       0      0 :::41128                :::*                    LISTEN
tcp6       0      0 :::3306                 :::*                    LISTEN
tcp6       0      0 :::8083                 :::*                    LISTEN
tcp6       0      0 :::21                   :::*                    LISTEN
udp        0      0 0.0.0.0:68              0.0.0.0:*
udp        0      0 172.17.0.14:123         0.0.0.0:*
udp        0      0 127.0.0.1:123           0.0.0.0:*
udp6       0      0 fe80::5054:ff:fec9::123 :::*
udp6       0      0 ::1:123                 :::*
```

可通过查询服务器开启了哪些端口来判断开启了哪些服务

查看本机所有的网络连接：

```text
netstat -an
```

查看本机路由表：

```text
[root@VM_0_14_centos ~]# netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         172.17.0.1      0.0.0.0         UG        0 0          0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U         0 0          0 eth0
172.17.0.0      0.0.0.0         255.255.240.0   U         0 0          0 eth0
```

## setup 配置网络

长这样：

![image-20210205151634257](https://i.loli.net/2021/02/05/8BvmGeXtUTcd7V6.png)

tab键移动光标

## mount 挂载命令

可以当作Windows的盘符来理解，不过这里的挂载点是目录。

~~

