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
 - Spark Procedure
   SparkContext -> Cluster Manager -> Executor -> DAGScheduler -> TaskScheduler -> reverse
 - RDD Programming
   - 
