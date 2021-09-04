## Spark
- Distributed Storage& Distributed Computing
  - Disbuted Storage: HDFS, NoSQL
  - Distributed Computing: MapReduce(Hadoop), Spark(substitution for MapReduce but not Hadoop)
- Two cores for distributed computing
  - MapReduce: Map&Reduce. Input -> Split -> Map -> Shuffle -> Reduce -> Output
  - YARN(Yet Another Resource Negotiator): Cluster Resource Management. 
- Why Spark?
Hadoop is consuming for I/O. Spark in not restraint with Map&Reduce. Can save to mamory. 
- Spark Basics
  - RDD: Resillient Distributed Dataset. Read only. 
    - Action: Map, Filter. Not execute immediately. Return RDD
    - Transformaiton: Count, Collect. return not RDD
  - Application: User-written spark program
  - Task: unit for execution
  - Job: operations on RDDs
  - Stage: Unit for Job
  - Shuffle: Spark generates DAG based on the relationships among RDDs, which leads to wide/narow dependency(Shuffle or not). 
  - Lazy evaluation
 - Spark Procedure
   SparkContext -> Cluster Manager -> Executor -> DAGScheduler -> TaskScheduler -> reverse
 - RDD Programming. https://spark.apache.org/docs/3.1.1/api/python/index.html
   - foreach: Applies the f function to all Row of this DataFrame.
   - filter
   - map
   - groupByKey
   - reduceByKey
   - count, collect, first, take(n), reduce
   - persist, unpersist, cache -> persist(MEMORY_ONLY)
   - RDD Partition. close to numebr of cores. parallelize, reparititon
 - Paired RDD
 a distributed collection of data with the key-value pair. Created by Map or from list. 
   - reduceByKey用于对每个key对应的多个value进行merge 操作，最重要的是它能够在本地先进行merge操作，并且 merge操作可以通过函数自定义 reduceByKey和groupByKey的区别；
groupByKey也是对每个key进行操作，但只生成一个 sequence，groupByKey本身不能自定义函数，需要先用 groupByKey生成RDD，然后才能对此RDD通过map进行 自定义函数操作。
   - keys
   - values
   - sortByKey
   - sortBy
   - mapValues
   - join
 - Spark SQL
   - Hive: SQL on Hadoop. Convert SQL to Hadoop.
   - Dataframe: distributed dataset based on RDD
   - SparkSession: load data from different source and convert to dataframe. 
   
   `from pyspark import SparkContext,SparkConf
from pyspark.sql import SparkSession
spark = SparkSession.builder.config(conf = SparkConf()).getOrCreate()`
