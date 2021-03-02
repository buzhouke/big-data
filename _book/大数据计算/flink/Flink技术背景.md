

流处理（streaming）系统则是构建在无界数据集之上的、可提供实时计算的另一类数据处理系统。

经过一段时间的应用实践，MapReduce的缺陷也逐渐暴露，最让人诟病的是Map+Shuffle+Reduce编程模型导致计算作业效率低下。为此，2007年，谷歌发起了Flume项目。起初，Flume只有Java版本，因此也被称为Flume Java（这里所说的Flume和Apache的Flume不同）。Flume将数据处理过程抽象成计算图（有向无环图），数据处理逻辑被编译成Map+Shuffle+Reduce 的组合，并加入物理执行计划优化，而不是简单地将Map+Shuffle+Reduce串联。

Flume引入的管道（Pipeline）、动态负载均衡（谷歌内部称为液态分片）和流语义思想成为大数据处理技术变革的宝贵理论财富。产生于处理推特信息流的流式数据处理框架 Storm 以牺牲强一致性换取实时性，并在一些场景下取得了成功。为了让数据处理程序兼备强一致性和实时性，工程师们将强实时性的 Storm 和强一致性的 Hadoop 批处理系统融合在一起，即Lambda架构。其中，Storm负责实时生成近似结果，Hadoop负责计算最终精准结果。Lambda架构需要部署两套队列集群，数据要持久化存放两份，这会导致数据冗余，增加系统维护成本。

产生于处理推特信息流的流式数据处理框架 Storm 以牺牲强一致性换取实时性，并在一些场景下取得了成功。为了让数据处理程序兼备强一致性和实时性，工程师们将强实时性的 Storm 和强一致性的 Hadoop 批处理系统融合在一起，即Lambda架构。其中，Storm负责实时生成近似结果，Hadoop负责计算最终精准结果。Lambda架构需要部署两套队列集群，数据要持久化存放两份，这会导致数据冗余，增加系统维护成本。Lambda架构示意图，如图1-5所示。



![img](https://res.weread.qq.com/wrepub/epub_25449828_5)![img](https://res.weread.qq.com/wrepub/epub_25449828_6)



![img](https://res.weread.qq.com/wrepub/epub_25449828_7)![img](https://res.weread.qq.com/wrepub/epub_25449828_8)

![img](https://res.weread.qq.com/wrepub/epub_25449828_9)![Spark Streaming流式数据处理任务的架构方案](https://res.weread.qq.com/wrepub/epub_25449828_10)

Flink架构：![Flink架构](https://res.weread.qq.com/wrepub/epub_25449828_11)



最初，流式数据处理是通过批处理系统实现的，如SparkStreaming，其原理是将多个微批处理任务串接起来构建流式数据处理任务。但是这种采用微批重复运行的机制，牺牲了延迟和吞吐，引发了Spark Streaming是不是真正流式数据处理引擎的争议。为此，业界便开始构建用于处理没有时间边界数据（无界数据集，Unbounded Data）的实时数据系统。

因此，从这个角度可以定义流是一种为无界数据集设计的数据处理引擎，这种引擎具备以下特征：

1. 具备强一致性，即支持exactly-once语义。
2. 提供丰富的时间工具，如事件时间、处理时间、窗口。
3. 保证系统具有可弹性、伸缩性。
4. 同时保证高吞吐、低延迟与容错。
5. 支持高层语义，如流式关系型API（SQL）、复杂事件处理（CEP，Complex Event Processing）。

CAP定理指出：一个分布式系统最多能具备一致性（Consistency）、可用性（Availability）及分区容错性（Partition tolerance）中的两个特性，而不可能同时具备这三个特性。

1. 一致性：在分布式系统中同一数据的所有备份，在同一时刻，其值是相同的，或者说，所有客户端读取的值是相同的。
2. 可用性：在集群的部分节点出现故障后，集群还能正常响应客户端的读写请求。
3. 分区容错性：分区是对分布式系统通信时限的要求，即如果不能在有限时间内达成数据一致，则系统发生分区。所谓分区容错性，是指即便发生了分区，分布式系统仍然能正确响应客户端的读写请求。

Flink如何做到一致性？

Flink如何做到可用性？