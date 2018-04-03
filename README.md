1、Apache Hive

Hive是原始的SQL-on-Hadoop解决方案。它是一个开源的Java项目，能够将SQL转换成一系列可以在标准的Hadoop TaskTrackers上运行的MapReduce任务。Hive通过一个metastore（本身就是一个数据库）存储表模式、分区和位置以期提供像MySQL一样的功能。它支持大部分MySQL语法，同时使用相似的 database/table/view约定组织数据集。Hive提供了以下功能：

Hive-QL，一个类似于SQL的查询接口
一个命令行客户端
通过中央服务支持元数据共享
JDBC 驱动
多语言 Apache Thrift 驱动
一个用于创建自定义函数和转换的Java API
何时使用它？

Hive是一个几乎所有的Hadoop机器都安装了的实用工具。Hive环境很容易建立，不需要很多基础设施。鉴于它的使用成本很低，我们几乎没有理由将其拒之门外。

但是需要注意的是，Hive的查询性能通常很低，这是因为它会把SQL转换为运行得较慢的MapReduce任务。

Hive的未来

Hortonworks 目前正在推进Apache Tez 的开发以便于将其作为新的Hive后端解决现在因为使用MapReduce而导致的响应时间慢的问题。

2、Cloudera Impala

Impala是一个针对Hadoop的开源的“交互式”SQL查询引擎。它由Cloudera构建，后者是目前市场上最大的Hadoop供应商之一。和Hive一样，Impala也提供了一种可以针对已有的Hadoop数据编写SQL查询的方法。与Hive不同的是它并没有使用MapReduce执行查询，而是使用了自己的执行守护进程集合，这些进程需要与Hadoop数据节点安装在一起。Impala提供了以下功能：

ANSI-92 SQL语法支持
HIVE-QL支持
一个命令行客户端
ODBC 驱动
与Hive metastore互操作以实现跨平台的模式共享
一个用于创建函数和转换的C++ API
何时使用它？

Impala的设计目标是作为Apache Hive的一个补充，因此如果你需要比Hive更快的数据访问那么它可能是一个比较好的选择，特别是当你部署了一个Cloudera、MapR或者Amazon Hadoop集群的时候。但是，为了最大限度地发挥Impala的优势你需要将自己的数据存储为特定的文件格式（Parquet），这个转变可能会比较痛苦。另外，你还需要在集群上安装Impala守护进程，这意味着它会占用一部分TaskTrackers的资源。Impala目前并不支持YARN。

Impala的未来

Cloudera 已经开始尝试将Impala与YARN集成，这让我们在下一代Hadoop集群上做Impala开发的时候不再那么痛苦。

3、Presto

Presto是一个用Java语言开发的、开源的“交互式”SQL查询引擎。它由Facebook构建，即Hive最初的创建者。Presto采用的方法类似于Impala，即提供交互式体验的同时依然使用已有的存储在Hadoop上的数据集。它也需要安装在许多“节点”上，类似于Impala。Presto提供了以下功能：

ANSI-SQL语法支持 (可能是ANSI-92)
JDBC 驱动
一个用于从已有数据源中读取数据的“连接器”集合。连接器包括：HDFS、Hive和Cassandra
与Hive metastore交互以实现模式共享
何时使用它？

Presto的目标和Cloudera Impala一样。但是与Impala不同的是它并没有被一个主要的供应商支持，所以很不幸你在使用Presto的时候无法获得企业支持。但是有一些知名的、令人尊敬的技术公司已经在产品环境中使用它了，它大概是有社区的支持。与Impala相似的是，它的性能也依赖于特定的数据存储格式（RCFile）。老实地说，在部署Presto之前你需要仔细考虑自己是否有能力支持并调试Presto，如果你对它的这些方面满意并且相信Facebook并不会遗弃开源版本的Presto，那么使用它。

4、Shark

Shark是由UC Berkeley大学使用Scala语言开发的一个开源SQL查询引擎。与Impala和Presto相似的是，它的设计目标是作为Hive的一个补充，同时在它自己的工作节点集合上执行查询而不是使用MapReduce。与Impala和Presto不同的是Shark构建在已有的 Apache Spark数据处理引擎之上。Spark现在非常流行，它的社区也在发展壮大。可以将Spark看作是一个比MapReduce更快的可选方案。Shark提供了以下功能：

类似于SQL的查询语言支持，支持大部分Hive-QL
一个命令行客户端（基本上是Hive客户端）
与Hive metastore交互以实现模式共享
支持已有的Hive 扩展，例如UDFs和SerDes
何时使用它？

Shark非常有趣，因为它既想支持Hive功能又想极力地改善性能。现在有很多组织正在使用Spark，但是不确定有多少在用Shark。我并不认为它的性能能够赶上Presto和Impala，但是如果你已经打算使用Spark那么可以尝试使用一下Shark，特别是Spark正在被越来越多的主要供应商所支持。

5、Apache Drill

Apache Drill是一个针对Hadoop的、开源的“交互式”SQL查询引擎。Drill现在由MapR推动，尽管他们现在也支持Impala。Apache Drill的目标与Impala和Presto相似——对大数据集进行快速的交互式查询，同时它也需要安装工作节点（drillbits）。不同的是Drill旨在支持多种后端存储（HDFS、HBase、MongoDB），同时它的一个重点是复杂的嵌套数据集（例如JSON）。不幸的是drill现在仅在Alpha阶段，因此应用还不是很广泛。Drill提供了以下功能：

ANSI SQL兼容
能够与一些后端存储和元数据存储交互（Hive、HBase、MongoDB）
UDFs扩展框架、存储插件
何时使用它？

最好别用。该项目依然在Alpha阶段，因此不要在生产环境中使用它。

6、HAWQ

Hawq是EMC Pivotal 公司的一个非开源产品，作为该公司专有Hadoop版本“Pivotal HD”的一部分提供。Pivotal宣称Hawq是“世界上最快的Hadoop SQL引擎”，已经发展了10年。然而这种观点难以得到证实。很难知道Hawq到底提供了哪些特性，但是可以收集到下面这些：

完整的SQL语法支持
能够通过Pivotal Xtension框架（PXF）与Hive和HBase互操作
能够与Pivotal GemFire XD（内存实时数据库）互操作
何时使用它？

如果你使用由Pivotal公司提供的Hadoop版本那么就使用它，否则不使用。

7、BigSQL

Big Blue 有它自己的Hadoop版本，称为Big Insights。BigSQL作为该版本的一部分提供。BigSQL用于使用MapReduce和其他能够提供低延迟结果的方法（不详）查询存储在HDFS中的数据。从BigSQL的文档中可以了解到它大概提供以下功能：

JDBC和ODBC 驱动
广泛的SQL支持
可能有一个命令行客户端
何时使用它？

如果你是IBM的客户那么就使用它，否则不使用。

8、Apache Phoenix

Apache Phoenix是一个用于Apache HBase的开源SQL引擎。它的目标是通过一个嵌入的JDBC驱动对存储在HBase中的数据提供低延迟查询。与之前介绍的其他引擎不同的是，Phoenix提供了HBase数据的读、写操作。它的功能有：

一个JDBC驱动
一个命令行客户端
批量加载数据的机制
能够创建新表，或者映射到已有的HBase数据
何时使用它？

如果你使用HBase那么就使用它。尽管Hive能够从HBase中读取数据，但是Phoenix还提供了写入功能。不清楚它是否适合产品环境和事务，但是作为一个分析工具它的功能无疑足够强大。

Apache Tajo

Apache Tajo项目的目的是在HDFS之上构建一个先进的数据仓库系统。Tajo将自己标榜为一个“大数据仓库”，但是它好像和之前介绍的那些低延迟查询引擎类似。虽然它支持外部表和Hive数据集（通过HCatalog），但是它的重点是数据管理，提供低延迟的数据访问，以及为更传统的ETL提供工具。它也需要在数据节点上部署Tajo特定的工作进程。Tajo的功能包括：

ANSI SQL兼容
JDBC 驱动
集成Hive metastore能够访问Hive数据集
一个命令行客户端
一个自定义函数API
何时使用它？

虽然Tajo的一些基准测试结果非常漂亮，但是基准测试可能会有一些偏见，不能对其完全信任 。Tajo社区现在也不够繁荣，在北美也没有主要的Hadoop供应商支持它。但是如果你在南韩，Gruter 是主要的项目赞助者，如果你使用他们的平台那么可能会得到他们良好的支持，否则的话最好还是使用Impala或者Presto这些引擎。