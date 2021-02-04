# Flink安装与WordCount

大数据的框架几乎都不支持Windows环境，生活不易，猫猫叹气。

一想到在服务器上捣鼓东西就开始怂，不过这次装Flink框架简直顺利得不行。

打开宝塔面板，在/usr/local/路径下上传从官网下载的[压缩包](https://mirrors.tuna.tsinghua.edu.cn/apache/flink/flink-1.12.1/flink-1.12.1-bin-scala_2.11.tgz),下载的是2\_11，最新版本。

然后解压，这个注意没必要指定文件夹，解压后它会以"flink-1.12.1"命名一个文件夹的（脑抽指定了发现要多打开一次 不开心）

![image-20210124183726222](https://i.loli.net/2021/01/24/YImaCwWd1HSlVZ3.png)

要记得检查安全组是否放行：

![&#x5B89;&#x5168;&#x7EC4;&#x653E;&#x884C;](https://i.loli.net/2021/01/24/IQRGcXtUiWFn48m.png)

访问8081端口，可以看到Flink的仪表盘：

![Flink&#x9762;&#x677F;](https://i.loli.net/2021/01/24/xBYHlMuQ1Z3CGWz.png)

整套流程跟着官方教程一步步走下来其实很顺利，但是写博客记录时找不到了。

## WordCount

然后还是WordCount，本地开发环境，我这里就是idea+maven，maven导入依赖：

```markup
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-java</artifactId>
  <version>1.12.1</version>
</dependency>
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-streaming-java_2.11</artifactId>
  <version>1.12.1</version>
</dependency>
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-clients_2.11</artifactId>
  <version>1.12.1</version>
</dependency>
```

代码：

```java
package site.buzhou;

import org.apache.flink.api.common.functions.FlatMapFunction;
import org.apache.flink.api.java.DataSet;
import org.apache.flink.api.java.ExecutionEnvironment;
import org.apache.flink.api.java.tuple.Tuple2;
import org.apache.flink.util.Collector;


/**
 * @program: FlinkTutorialDemo01
 * @description:
 * @author: 不周
 * @create: 2021-01-24 18:03
 **/
public class WordCount {
    public static void main(String[] args) throws Exception {
        //创建执行环境
        ExecutionEnvironment env = ExecutionEnvironment.getExecutionEnvironment();
        String inputPath = "E:\\Project\\Hadoop\\FlinkTutorialDemo01\\src\\main\\resources\\words.txt";
        DataSet<String> stringDataSource = env.readTextFile(inputPath);

        //对数据集进行处理
        DataSet<Tuple2<String,Integer>> dataSet = stringDataSource.flatMap(new MyFlatMapper()).groupBy(0).sum(1);
        dataSet.print();
    }

    public static class MyFlatMapper implements FlatMapFunction<String, Tuple2<String,Integer>>{


        public void flatMap(String s, Collector<Tuple2<String, Integer>> collector) throws Exception {
            String[] words = s.split(" ");
            for(String word:words){
                collector.collect(new Tuple2<String, Integer>(word,1));
            }
        }
    }
}
```

编译结果：

![image-20210124183951491](https://i.loli.net/2021/01/24/6Fu8dw7GUbaTxIZ.png)

看得出来就是复制官网的文段嘛hh：

```text
We recommend packaging the application code and all its required dependencies into one jar-with-dependencies which we refer to as the application jar. The application jar can be submitted to an already running Flink cluster, or added to a Flink application container image.

Projects created from the Java Project Template or Scala Project Template are configured to automatically include the application dependencies into the application jar when running mvn clean package. For projects that are not set up from those templates, we recommend adding the Maven Shade Plugin (as listed in the Appendix below) to build the application jar with all required dependencies.

Important: For Maven (and other build tools) to correctly package the dependencies into the application jar, these application dependencies must be specified in scope compile (unlike the core dependencies, which must be specified in scope provided).

Scala Versions
Scala versions (2.11, 2.12, etc.) are not binary compatible with one another. For that reason, Flink for Scala 2.11 cannot be used with an application that uses Scala 2.12.

All Flink dependencies that (transitively) depend on Scala are suffixed with the Scala version that they are built for, for example flink-streaming-scala_2.11.

Developers that only use Java can pick any Scala version, Scala developers need to pick the Scala version that matches their application’s Scala version.

Please refer to the build guide for details on how to build Flink for a specific Scala version.
```

[https://flink.apache.org/downloads.html\#apache-flink-1121](https://flink.apache.org/downloads.html#apache-flink-1121)

