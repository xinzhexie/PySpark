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
   - Boardcast
   - Accumulator
   - Serializers
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
   - SparkSession: load data from different source and convert to dataframe. 在启动进入pyspark以后，pyspark就默认提供了一个SparkContext 对象（名称为sc）和一个SparkSession对象（名称为spark）
   ```
   from pyspark import SparkContext,SparkConf  
   from pyspark.sql import SparkSession
   spark = SparkSession.builder.config(conf = SparkConf()).getOrCreate()
   spark.read.text("people.txt")
   spark.read.json("people.json")
   spark.read.parquet("people.parquet")
   df.write.text("people.txt")
   df.write.json("people.json“)
   df.write.parquet("people.parquet“) 
    ```    
    - functions
    ```
    DataFrame.printSchema( )
    DataFrame.select( )
    DataFrame.filter( )
    DataFrame.groupBy( )
    DataFrame.sort( )
    ```
    ```
    #random
    from pyspark.sql.functions import rand, randn 
    df = spark.range(0, 10).withColumn('rand1', rand(seed=10)) \
                       .withColumn('rand2', rand(seed=27))
    df.show()
    #round
    from pyspark.sql.functions import round
    df = spark.createDataFrame([(2.5,)], ['a'])
    df.select(round('a', 0).alias('r')).show()
    
    #sample
    sample1 = color_df.sample(
    withReplacement=False, # 无放回抽样
    fraction=0.6,
    seed=1000)
    
    #describe
    spark_df.describe().show()
    spark_df.describe('a').show()
    
    #minmax
    color_df.select(min('uniform'), max('uniform')).show()
    
    #mean variance
    color_df.select(mean('uniform').alias('mean'),
                stddev('uniform').alias('stddev'))\
    .show()
    
    #cov corr
    # 协方差
    df.stat.cov('rand1','rand2')

    # 样本协方差
    from pyspark.sql.functions import covar_pop
    df.agg(covar_samp("rand1", "rand1").alias('new_col')).collect()

    # 相关系数
    df.stat.corr('rand1', 'rand2')
    
    #Create a DataFrame with two columns (name, item)
    names = ["Alice", "Bob", "Mike"]
    items = ["milk", "bread", "butter", "apples", "oranges"]
    df = spark.createDataFrame([(names[i % 3], items[i % 5]) for i in range(100)], ["name", "item"])
    df.show(5)

    df.stat.crosstab("name", "item").show()
    
    #freq
    # 找出现次数最多的元素(频数分布)
    df = spark.createDataFrame([(1, 2, 3) if i % 2 == 0 else (i, 2 * i, i % 4) for i in range(100)],
                               ["a", "b", "c"])
    df.show(10)

    # 下面的代码找到每列出现次数占总的40%以上频繁项目
    df.stat.freqItems(["a", "b", "c"], 0.4).show()
    
    #distinct
    df.agg(func.countDistinct('a')).show()
    
    #grouping
    Aggregate function: indicates whether a specified column in a GROUP BY list is aggregated or not, returns 1 for aggregated or 0 for not aggregated in the result set.
    
    df.cube("name").agg(func.grouping("name"), func.sum("age")).orderBy("name").show()
    
    #grouping_id
    df.cube("name").agg(grouping_id(), sum("age")).orderBy("name").show()
    
    ```
    - RDD <-> dataframe
 - Spark Streaming
 - Spark ML Lib
