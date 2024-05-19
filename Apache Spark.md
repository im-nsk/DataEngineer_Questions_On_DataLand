**Q1.** What is Apache Spark, and how does it differ from Hadoop MapReduce? Explain with examples.
**Explanation:** Apache Spark is an open-source, distributed computing system that provide an interface for programming entire clusters with implicit data parallelism & fault tolerance. Unlike Hadoop MapReduce, which write intermediate result to disk, Spark processes data in-memory, which can significantly improve processing speeds for many data processing tasks.
**Example:**- Consider a simple word count program. In Hadoop MapReduce, the process involves multiple read and write operation to disk between map & reduce phases. In contrast, Spark keeps the intermediate data in memory, reducing the I/O operation and thus speeding up the computation.

**Q2.** What are Resilient Distributed Datasets (RDDs) in Spark, and what are their key properties?
**Explanation:** RDDs are the fundamental data structure of Spark. They are fault-tolerant collections of object spread across a cluster, which can be processed in parallel.
**Key Properties:**
**Immutability:** Once created, an RDD can't be altered. Transformation on RDDs result in the creation of new RDDs.
**Fault Tolerance:** RDDs are designed to handle failures gracefully by recomputing lost data using lineage information.
**Lazy Evaluation:**  RDD operations are evaluated lazily. Instead of performing the computation as soon as they are invoked, Spark records the transformation to be executed when an action(like `collect` or `save`) is called.
**Partitioning:** RDDs are divided into logical partitions, which may be computed on different nodes of the cluster.

**Q3.** Explain the concept of transformations and actions in Spark. Provide examples of each.
**Explanation:** In Spark, operations on RDDs are divided into two types: `transformations` and `actions`.
***Transformations*** are operations on RDDs that return a new RDD, such as `map`, `filter`, and `reduceByKey`. They are lazily evaluated, meaning they are only define the computation but do not execute it.
***Actions*** are operations that trigger the execution of the transformations to produce a result. They include operations like `collect`, `count`, and `saveAsTextFile`.
Example:
```
rdd = sc.textFile('hdfs://....')
words = rdd.flatMap(lambda line: line.split(""))
pairs = words.map(lambda word: (word, 1))
wordCounts = pairs.reduceByKey(lambda a, b: a+b)
-- flatMap, map, reduceByKey are transformations.

output = wordCounts.collect()
for (word, count) in output:
	print(f'{word}: {count}')
-- count is action that triggers the execution of the previous transformations and returns the result to the driver program.
```

**Q4.** What is a SparkSession, and why is it important in Spark 2.0 and later versions? How do you create one?
**Explanation:** A `SparkSession` is the entry point to programming with the Spark SQL module. In Spark 2.0 and later, it encapsulates the older `SQLContext` and `HiveContext`, providing a unified entry point for reading data, creating DataFrames, and performing SQL operations.
**Importance**:
* ***Unified Context:*** `SparkSession` replaces `SQLContext` & `HiveContext` with a single entry point.
* ***Easy to Use:*** Simplifies the API for working with structured data.
* ***Configuration:*** Allows for easier and centralized configuration of Spark applications.
**Example:**
```
from pyspark.sql import SparkSession

spark = SparkSession.builder\
	.appName('ExampleApp')\
	.config('spark.some.config.option', 'some-values)\
	.getOrCreate()
```

**Q5.** Discuss the role of SparkContext in a Spark application. How does it interact with SparkCluster?
**Explanation:** `SparkContext` is the entry point for a Spark application. It represents the connections to a Spark cluster and allows the application to interact with the cluster resources.
**Roles of SparkContext:**
* ***Cluster Connection:*** Manages the connection to the Spark cluster.
* ***RDD Creation:*** Provided methods to create RDDs from various data sources.
* ***Configuration Management:*** Handles configuration parameters for the Spark application.
* ***Job Scheduling:*** Submits jobs to the cluster and monitors their execution.
**Interaction with SparkCluster:**
When a Spark application is launched, the `SparkContext` connects to the cluster manager (such as YARN, Mesos, or Spark's standalone cluster manager). The `SparkContext` distributes the application code across the cluster, coordinates the execution of tasks, and manages resources.
**Example:**
```
from pyspark import SparkContext

sc = SparkContext("local", "ExampleApp")
data = [1,2,3,4,5]
distData = sc.parallelize(data)
```


