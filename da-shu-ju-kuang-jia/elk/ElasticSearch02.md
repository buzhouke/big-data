# ElasticSearch02

导入[课程](https://time.geekbang.org/course/intro/100030501)中的movie.csv,

结合GitHub上的说明和各种踩坑记录捣鼓出来的配置文件：

```
input {
  file {
    path => "E:/Project/ELK_Learning/geektime-ELK-master/part-1/2.4-Logstash安装与导入数据/movielens/ml-latest-small/movies.csv"
    start_position => "beginning"
    sincedb_path => "null"
  }
}

filter {
}

output {
  stdout {
    codec => rubydebug
  }

elasticsearch {
    index => "movie"
  }
}
```

## 核心概念

以下在《Elasticsearch实战与原理解析》文段的基础上进行补充

Elasticsearch的核心概念有**Node、Cluster、Shards、Replicas、Index、Type、Document、Settings、Mapping和Analyzer**，其含义分别如下所示。
（1）Node：即节点。节点是组成Elasticsearch集群的基本服务单元，集群中的每个运行中的Elasticsearch服务器都可称之为节点。

（2） Cluster：即集群。Elasticsearch的集群是由具有相同cluster.name （默认值为elasticsearch）的一个或多个Elasticsearch节点组成的，各个节点协同工作，共享数据。同一个集群内节点的名字不能重复，但集群名称一定要相同。
在实际使用Elasticsearch集群时，一般需要给集群起一个合适的名字来替代cluster.name的默认值。自定义集群名称的好处是，可以防止一个新启动的节点加入相同网络中的另一个同名的集群中。
在Elasticsearch集群中，节点的状态有Green、Yellow和Red三种，分别如下所述。
① Green：绿色，表示节点运行状态为健康状态。所有的主分片和副本分片都可以正常工作，集群100%健康。
② Yellow：黄色，表示节点的运行状态为预警状态。所有的主分片都可以正常工作，但至少有一个副本分片是不能正常工作的。此时集群依然可以正常工作，但集群的高可用性在某种程度上被弱化。
③ Red：红色，表示集群无法正常使用。此时，集群中至少有一个分片的主分片及它的全部副本分片都不可正常工作。虽然集群的查询操作还可以进行，但是也只能返回部分数据（其他正常分片的数据可以返回），而分配到这个有问题分片上的写入请求将会报错，最终导致数据丢失。

（3）Shards：即分片。当索引的数据量太大时，受限于单个节点的内存、磁盘处理能力等，节点无法足够快地响应客户端的请求，此时需要将一个索引上的数据进行水平拆分。拆分出来的每个数据部分称之为一个分片。一般来说，每个分片都会放到不同的服务器上。
进行分片操作之后，索引在规模上进行扩大，性能上也随之水涨船高的有了提升。
Elasticsearch依赖Lucene，Elasticsearch中的每个分片其实都是Lucene中的一个索引文件，因此每个分片必须有一个主分片和零到多个副本分片。
当软件开发人员在一个设置有多分片的索引中写入数据时，是通过路由来确定具体写入哪个分片中的，因此在创建索引时需要指定分片的数量，并且分片的数量一旦确定就不能更改。
当软件开发人员在查询索引时，需要在索引对应的多个分片上进行查询。Elasticsearch会把查询发送给每个相关的分片，并汇总各个分片的查询结果。对上层的应用程序而言，分片是透明的，即应用程序并不知道分片的存在。
在Elasticsearch中，默认为一个索引创建5个主分片，并分别为每个主分片创建一个副本。
（4）Replicas：即备份，也可称之为副本。副本指的是对主分片的备份，这种备份是精确复制模式。每个主分片可以有零个或多个副本，主分片和备份分片都可以对外提供数据查询服务。当构建索引进行写入操作时，首先在主分片上完成数据的索引，然后数据会从主分片分发到备份分片上进行索引。
当主分片不可用时，Elasticsearch会在备份分片中选举出一个分片作为主分片，从而避免数据丢失。
一方面，备份分片既可以提升Elasticsearch系统的高可用性能，又可以提升搜索时的并发性能；另一方面，备份分片也是一把双刃剑，即如果备份分片数量设置得太多，则在写操作时会增加数据同步的负担。
（5）Index：即索引。在Elasticsearch中，索引由一个和多个分片组成。在使用索引时，需要通过索引名称在集群内进行唯一标识。
（6）Type：即类别。类别指的是索引内部的逻辑分区，通过Type的名字在索引内进行唯一标识。在查询时如果没有该值，则表示需要在整个索引中查询。
（7）Document：即文档。索引中的每一条数据叫作一个文档，与关系数据库的使用方法类似，一条文档数据通过_id在Type内进行唯一标识。
（8）Settings：Settings是对集群中索引的定义信息，比如一个索引默认的分片数、副本数等。
（9）Mapping：Mapping表示中保存了定义索引中字段（Field）的存储类型、分词方式、是否存储等信息，有点类似于关系数据库（如MySQL）中的表结构信息。
在Elasticsearch中，Mapping是可以动态识别的。如果没有特殊需求，则不需要手动创建Mapping，因为Elasticsearch会根据数据格式自动识别它的类型。当需要对某些字段添加特殊属性时，如定义使用其他分词器、是否分词、是否存储等，就需要手动设置Mapping了。一个索引的Mapping一旦创建，若已经存储了数据，就不可修改了。

创建临时index，写入样本数据，获取自动创建的mapping定义，如不合适再自行修改。

（10） Analyzer：Analyzer表示的是字段分词方式的定义。一个Analyzer通常由一个Tokenizer和零到多个Filter组成。在Elasticsearch中，默认的标准Analyzer包含一个标准的

单就上面的几个概念其实还是不能把握搜索全貌的，结合 https://wiki.jikexueyuan.com/project/elasticsearch-definitive-guide-cn 与阮一峰的课程进行学习。

其中跟着电子书学es的增删改查还是蛮循序渐进的，其中要注意将里面的“blog”替换成“_doc”就行，

```
use the /{index}/_doc/{id}
```

会比线上课程清晰很多。

```json
#es的增删改查操作：
PUT /website/_doc/123
{
  "title": "My first blog entry",
  "text":  "Just trying this out...",
  "date":  "2014/01/01"
}

POST /website/_doc/12
{
  "title":"My second blog entry",
  "text":"(/{index}/_doc/{id}, /{index}/_doc, or /{index}/_create/{id})",
  "date":"2020/11/30"
}

POST /website/_doc/
{
  "title": "My second blog entry",
  "text":  "Still trying this out...",
  "date":  "2014/01/01"
}

GET /website/_doc/123?pretty
GET /website/_doc/12

GET /website/_doc/12?_source=date

GET /website/_doc/12/_source

HEAD /website/_doc/123

HEAD /website/blog/123

PUT /website/_doc/123
{
  "title": "My first blog entry",
  "text":  "I am starting to get the hang of this...",
  "date":  "2014/01/02"
}

DELETE /website/blog/123

PUT /website/_doc/1/_create
{
  "title": "My first blog entry",
  "text":  "Just trying this out..."
}

GET /website/_doc/1

PUT /website/_doc/1?version=1
{
  "title": "My first blog entry",
  "text":  "Starting to get the hang of this..."
}

POST /_mget
{
   "docs" : [
      {
         "_index" : "website",
         "_id" :    2,
         "_source": "views"
      },
      {
         "_index" : "website",
         "_id" :    1,
         "_source": "views"
      }
   ]
}

GET /_mget
{
  "docs": [
    {
      "_index": "website",
      "_id": "2"
    },
    {
      "_index": "website",
      "_id": "1"
    }
  ]
}

```

## 坑

再填一下之前curl遇到的各种坑，

将单引号换双引号，删除不必要的空格。

```shell
# For Mac & Windows（课程说明里头的：跑了之后run不动的
curl -H 'Content-Type: application/x-ndjson' -XPOST 'localhost:9200/_bulk?pretty' --data-binary @logs.jsonl

#不会报错的：
curl -H "Content-Type:application/x-ndjson" -XPOST "http://127.0.0.1:9200/_bulk?pretty" --data-binary @logs.jsonl

curl -H "Content-Type:application/x-ndjson" -XPOST “http://127.0.0.1:9200/bank/account/_bulk?pretty" --data-binary @accounts.json
```

## Kibana可视化

在Management搞index pattern，再去Kibana看Discover。