---
description: 仅为个人学习记录
---

# Elasticsearch01

10/17加入SZ学长的开源项目，于是时间基本花在对这个项目（这块业务）的了解与相关技术的学习。

实质上是对大数据刚刚入门的水平，对着很多东西都无从下手，对于具体要做什么也是一个很迷茫的状态，但这里相当感谢学长给我这个机会以及很耐心地带我了解这一块业务。

于是10/19-10/25这一周的时间，基本上是在学习大数据（Hadoop及Spark）以及了解风控这一块的知识点，关于风控是看了几页《风控要略：互联网业务反欺诈之路》、《写给风控师的实操手册》，第2本大概看下来是很虚，因为没有学习过深度学习相关的东西，更没有任何的实操经验。

然后就一直反反复复折腾Hadoop，但远程连接还是没成功，考虑到花了太多时间又好像和项目没有多大关系就放弃了。（这个过程里反正还是让Linux小白再熟悉了一点点Linux命令叭，虚拟机上装一遍再服务器上装一遍hhh

关于大数据技术就看得很散，但是很幸运地看到了下面这张图，对于整个大数据的知识体系有了一定的了解。

![img](https://i.loli.net/2020/10/26/3mJT8MDzVinEpLo.png)

之前刚知道elasticsearch的时候只是从全文索引这个点上去了解，并不是很明白为什么要完成从MySQL到ElasticSearch的转入，但是也很幸运地看到了这个教程 [Elastic 搜索开发实战](https://elastic-search-in-action.medcl.com/)，基本上感受到了ElasticSearch的强大更破除了之前雾里看花的困惑。

同时考虑到对于大数据，接下来都是以一个项目驱动学习的步调在走，因此和SZ学长确认不会有集群后，放弃了对（伪）分布式的学习，原本的学习路线也做出了相应调整（原本是看B站视频，林子雨老师的[大数据技术原理与应用](https://www.bilibili.com/video/BV1BJ411F7u8)\)，再对Spark框架有一个非常浅层的了解（《Spark海量数据处理》微信读书上也可以找到，对函数式编程思想讲解得非常到位）后，（10/26）转到对ElasticSearch的学习上。

以下是琐碎记录，

安装elasticsearch-7.9.3，`E:\software\elastic\elasticsearch-7.9.3\bin>.\elasticsearch.bat`，访问[http://localhost:9200/，](http://localhost:9200/，)

![image-20201026104352224](https://i.loli.net/2020/10/26/kgtEIXSDG7CybQO.png)

curl -XPOST -H 'Content-Type: application/json' [http://localhost:9200/index/doc/1?pretty](http://localhost:9200/index/doc/1?pretty) -d'{"name":"medcl"}'

输入这条语句时反复报错，换成postman同样报错，下载推荐的kibana卡在一堆WARN和ERROR上面。

换成如下语句仍未解决问题，

curl -XPOST -H "Content-Type:application/json" [http://localhost:9200/index/doc/1?pretty](http://localhost:9200/index/doc/1?pretty) -d"{"name":"medcl"}"

~~出去吃了个水果捞冷静冷静，但顺便胃也凉掉了。~~

~~yysy，芋泥+芋圆+椰奶+红豆+花生碎真的绝配~~

冷静思考的话，一般对于一样东西不熟悉，刚入门的时候就是拿着报错去搜，搜完试了一圈无效，那么基本上就是残废状态，骂句劝退就无可奈何。

如上文这个查询语句，pretty，就明显搞不懂是什么东西，百度。

去用curl试了试elastic常用命令（[https://www.jianshu.com/p/c6708ea88710），然后发现，](https://www.jianshu.com/p/c6708ea88710），然后发现，)

![image-20201026154311770](https://i.loli.net/2020/10/26/UD8jHAPR62EfdQT.png)

用yellow笔标记出了这个yellow

\(备份一下没run成功的命令\)

`curl -X POST "localhost:9200/_reindex"-d`

`curl -X POST -H "Content-Type:application/json" "localhost:9200/my_index/_doc/1" -d "{"name":"vivien"}"`

但是POST还是无效，于是这个yellow一时半会难以解决。

```cpp
curl -X POST "localhost:9200/chat_index_alias/_search" -d '{"query": { "match":  "question": "吃饭了吗" } }}'
```

跑了一下午各种WARN、ERROR的kibana最后竟然还是跑出来了，其中给的demo的仪表盘也实在是i了i了。

![image-20201026212639484](https://i.loli.net/2020/10/26/xUIAL6cTStJ2jqX.png)

太好看了太强大了疯狂尖叫。

![image-20201026214019314](https://i.loli.net/2020/10/26/rvjYfmgyZnXUwpT.png)

借助Dev Tools快乐地逃出了curl的魔咒。

隔了一天再又想想，决定先从自己的网站动手，使用DataX将MySQL数据导入ElasticSearch。

实际上看着文档还是有点害怕

[https://github.com/alibaba/DataX/blob/master/elasticsearchwriter/doc/elasticsearchwriter.md](https://github.com/alibaba/DataX/blob/master/elasticsearchwriter/doc/elasticsearchwriter.md)

认真翻了一下GitHub上的issues，还是很害怕。

在Linux上将MySQL数据借助DataX工具导入ElasticSearch，根据官方文档，应选用ElasticSearch5.x。

记录

```javascript
{
    "job": {
        "setting": {
            "speed": {
                "channel": 1,
                "record": -1,
                "byte": -1
            }
        },
        "content": [{
            "reader": {
                "name": "mysqlreader",
                "parameter": {
                    "username": "root",
                    "password": "vihunxi28",
                    "column": [
                        "id",
                        "name"
                    ],
                    "splitPk": "id",
                    "connection": [{
                        "table": [
                            "datax_test"
                        ],
                        "jdbcUrl": [
                            "jdbc:mysql://localhost:3306/test"
                        ]
                    }]
                }
            },
            "writer": {
                "name": "elasticsearchwriter",
                "parameter": {
                    "endpoint": "http://localhost:9200",
                    "accessId": "admin",
                    "accessKey": "123456",
                    "index": "test-datax",
                    "type": "default",
                    "cleanup": true,
                    "settings": {
                        "index": {
                            "number_of_shards": 1,
                            "number_of_replicas": 0
                        }
                    },
                    "discovery": false,
                    "batchSize": 1000,
                    "splitter": ",",
                    "column": [{
                            "name": "id",
                            "type": "id"
                        },
                        {
                            "name": "name",
                            "type": "string"
                        }
                    ]
                }
            }
        }]
    }
}
```

研究了一个晚上，下午再看还是心有戚戚，放弃DataX

改用Logstash，这是logstash.conf

```text
input {
  jdbc {
    jdbc_driver_library => "E:\software\mysql-connector-java-8.0.21\mysql-connector-java-8.0.21.jar"
    jdbc_driver_class => "Java::com.mysql.jdbc.Driver"
    jdbc_connection_string => "jdbc:mysql://127.0.0.1:3306/myblog?serverTimezone=UTC"
    jdbc_user => "root"
    jdbc_password => "vihunxi28"
    statement => "SELECT  `id`,  `appreciation`,  `commentabled`,  LEFT(`content`, 256),  `create_time`,  `description`,  `first_picture`,  `flag`,  `published`,  `recommend`,  `share_statement`,  `title`,  `update_time`,  `views`,  `type_id`,  `user_id`,  `comment_count` FROM `myblog`.`t_blog`
"
    jdbc_paging_enabled => "true"
    jdbc_page_size => "50000"
  }
}

filter {
}

output {
  stdout {
    codec => rubydebug
  }

  elasticsearch {
    index => "blog-mysql"
  }
}
```

```javascript
.\bin\logstash.bat -f logstash.conf
```

然后就是听天由命看运气了，希望各个版本都能顺利运行（不想翻官网了呜呜呜

![image-20201029140238067](https://i.loli.net/2020/10/29/XV6uWzThxOF3KL2.png)

假装它是在正常地跑着的。

虽然怎么看怎么都觉得很诡异

经过一番斗智斗勇，我终于在它的不断跳转中获取到了如下信息，数据库连接错误。

![image-20201029141141849](https://i.loli.net/2020/10/29/zwaCNTGlvb5cgHX.png)

~~想吃芋泥~~

然后再经过一些乱七八糟的操作，分析出来是时区错误，于是赶忙在配置文件中指明具体的时区（上文配置文件已更新）。

最后顺利将MySQL的数据导入到了本地的elastic中。

![image-20201029150433105](https://i.loli.net/2020/10/29/Hbl8cCg51aPDkyX.png)

![image-20201029150828370](https://i.loli.net/2020/10/29/BfyZGQtm2kax8Dn.png)

根据教程应当实现网站的全文搜索功能，但估计了一下时间，花一天肯定做不完，超过一天没做完估计又是各种焦虑，跑去和学长讨论了一下子项目。

~~很迷茫，作业堆着不想做，只想出去玩~~，根据学长的建议，要去看**DataX的文档和设计思路**，了解到项目会分为实验性质的demo与上线的生产环境下构建的项目会有所区别，ELK不能满足大公司的实际需求，应当学习Flink与Spark、、

但我们这个项目仍使用ELK实现思路和离线数据的处理，工程化版本转换Demo的逻辑。

额外讲一句，其实觉得搞学习记录会比看知识点总结更有用，学习记录能够复盘，也就是作为材料来总结归纳整一个学习过程当中的不足。尤其是很多时候，解决问题都是东搜一个西试一个，最后发现改了哪里，怎么改的，怎么改对的都不知道，算是借鉴上周学Hadoop的血泪史叭，本周（10/26-11/01）就这样子记录学习过程。

## ~~其实就是掩饰自己学得实在不行~~

