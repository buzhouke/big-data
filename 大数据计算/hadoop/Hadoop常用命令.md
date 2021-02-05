因为东学一个西学一个总是忘记，所以专门弄个备份：

```
[hadoop@VM_0_14_centos hadoop]$ ./sbin/start-dfs.sh
Starting namenodes on [localhost]
localhost: starting namenode, logging to /usr/local/hadoop/logs/hadoop-hadoop-namenode-VM_0_14_centos.out
localhost: starting datanode, logging to /usr/local/hadoop/logs/hadoop-hadoop-datanode-VM_0_14_centos.out
Starting secondary namenodes [0.0.0.0]
0.0.0.0: starting secondarynamenode, logging to /usr/local/hadoop/logs/hadoop-hadoop-secondarynamenode-VM_0_14_centos.out
[hadoop@VM_0_14_centos hadoop]$ jps
19973 DataNode
19847 NameNode
23479 Jps
20154 SecondaryNameNode

```

服务器安装的是 2.8.5版本

启动后可访问http://localhost:9870

![](https://i.bmp.ovh/imgs/2020/10/37e92708e02f6379.png)



启动和停止命令

```
sbin/start-all.sh 启动所有的Hadoop守护进程。包括NameNode、 Secondary NameNode、DataNode、ResourceManager、NodeManager

sbin/stop-all.sh 停止所有的Hadoop守护进程。包括NameNode、 Secondary NameNode、DataNode、ResourceManager、NodeManager

sbin/start-dfs.sh 启动Hadoop HDFS守护进程NameNode、SecondaryNameNode、DataNode

sbin/stop-dfs.sh 停止Hadoop HDFS守护进程NameNode、SecondaryNameNode和DataNode

sbin/hadoop-daemons.sh start namenode 单独启动NameNode守护进程

sbin/hadoop-daemons.sh stop namenode 单独停止NameNode守护进程

sbin/hadoop-daemons.sh start datanode 单独启动DataNode守护进程

sbin/hadoop-daemons.sh stop datanode 单独停止DataNode守护进程

sbin/hadoop-daemons.sh start secondarynamenode 单独启动SecondaryNameNode守护进程

sbin/hadoop-daemons.sh stop secondarynamenode 单独停止SecondaryNameNode守护进程

sbin/start-yarn.sh 启动ResourceManager、NodeManager

sbin/stop-yarn.sh 停止ResourceManager、NodeManager

sbin/yarn-daemon.sh start resourcemanager 单独启动ResourceManager

sbin/yarn-daemons.sh start nodemanager  单独启动NodeManager

sbin/yarn-daemon.sh stop resourcemanager 单独停止ResourceManager

sbin/yarn-daemons.sh stopnodemanager  单独停止NodeManager

sbin/mr-jobhistory-daemon.sh start historyserver 手动启动jobhistory

sbin/mr-jobhistory-daemon.sh stop historyserver 手动停止jobhistory
```

