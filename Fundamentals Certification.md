# Fundamentals Certification #
#### Reference notes and markdown
- Main Study Resources:
  - [Apache Spark for Databricks](https://partner-academy.databricks.com/learn/course/63/play/1567:253/welcome-to-the-course;lp=123)
  - [ACloudGuru - Solutions Architect Associate](https://www.udemy.com/course/aws-certified-solutions-architect-associate/learn/lecture/13885822?start=0#overview)
    - Very good course on covering foundational services and concepts for AWS Solutions Architect
  - [AWS Certified Solutions Architect Associate Practice Exams (5)](https://www.udemy.com/course/aws-certified-solutions-architect-associate-amazon-practice-exams-saa-c02/)
    - cannot recommend this enough, while the ACloudGuru gave me the foundation and core to get about 50%, these exams and their explanations took me the rest of the way
    - EXCELLENT explanations for each question (right or wrong) as to why the correct answer is correct and all others are not as good or incorrect
  - Alternative to ACloudGuru: [OReilly AWS Certified Solutions Architect](https://learning.oreilly.com/videos/aws-certified-solutions/9780136721246/9780136721246-ACS2_00_00_00)
    - Although I never used it, this was going to be the backup course I took. I only used the question reviews at the end of each section. 
    - Much better framed to scope of AWS cert exam than acloudguru

# Apache Spark Programming with DataBricks #

# DataBricks Overview #
- DB is a unified data analytics platform bringing together DS projects & workflows - ML/analytics/DS in one cloud platform 
  - Addressing these users: data engineers, DS, ML engineers & Data analysts 
  - __ML/Data Science Workspace__ using MLFlow/PyTorch/TensorFlow 
    - this is where notepad exists 
  - __Unified Data Service__ using (optimized) ApacheSpark and DeltaLake 
    - DB offers optimized version of Apache Spark - 3-5x faster than OSS Spark
    - benefits of Data Lake and 
    - has RT data integrations
  - __Enterprise Cloud Service__ Delivered on Cloud platform as a managed service - AWS & Azure
    - variety of security controls, compliance options, track billing/usage 
  - __BI Integrations__ plug-ins to PowerBI or Tableau 

## Spark Overview ##  
- spark: unified analytics engine for Bid data processing
  -  makes it possible to process big-data and do big-data workflows through distr computing 
  - SQL, graph processing 
  - largest OSS data processing and considered a standard 
  - VERY FAST for large scale data processing, easy to use & unified engine and can plug into many things
  - unified API and engine for SQL queries, streaming data and ML

- Spark API 
  - Spark SQL + DataFrames 
    - Interactive SQL execution: allows use of spark SQL and programming abstraction called dataframes and can act as distr sql query 
    - mucho fast & integrated with spark ecosystem 
  - Spark Streaming
    - analytics needs ability to process streaming data 
    - HDFS, Kafka, Flume, Twitter ... 
  - ML lib 
    - scalable ML library with HQ algos to increase accuracy and speed (100x faster than mapReduce - take that Hadoop!)
    - available in Java, Scala & Python to be used in your workflows 
  - Spark Core 
    - R, SQL, Python, Scala, Java - can all be used for development
    - Generalized execution model/engine 

# Spark Execution #
  - managing infra is annoying so DB offers managed solution 
  - spark uses clusters of machines to break big task into smaller pieces and distr workload across multiple machines
  - Parallelism (parallel threading) 
  ![Spark Application Job-Stage-Task Parallelization](./images/SparkApplication_JobStageTask.png)
  - **Spark application** execution will create 
    - Multiple **Jobs** that execute in || 
    - Each job is broken up into **Stages** : set of ordered steps that together accomplish a job
    - **Tasks** are created by driver for each stage and assigned a partition of data to process 
      - they are the smallest units of work 

## Spark Architecture Basics From Analytics Vidhya ##
- [Understanding Internal Working of Apache Spark](https://www.analyticsvidhya.com/blog/2021/08/understand-the-internal-working-of-apache-spark/)
- [Data Engineering Basics - Spark Architecture](https://www.analyticsvidhya.com/blog/2020/11/data-engineering-for-beginners-get-acquainted-with-the-spark-architecture/)
  - OSS distributed big data processing engine for streaming and batch data with || execution & fault tolerance 
  - APIs in Java, Scala, Python & R 
    - also integrates with Hadoop ecosystem & able to run in hadoop clusters on hadoop data sources including Cassandra
  - developed to address drawbacks of Hadoop MapReduce - meaning Apache Spark uses in-memory computation making it MUCH faster 
  ![Spark Tech Stack](./images/SparkTechStack.png) 
- **Spark core**: basic functionality of spark - components for task scheduling, memory management, fault recovery, interacting with storage systems... 
- home to the API that defines resilient distributed datasets (RDD) sparks main programming abstraction 
- Runtime Libraries
  - **Spark SQL** spark's package for structured data - allows SQL queries, Hive Query languages and various data sources like Hive tables, JSON & Parquet 
  - **Spark Streaming** spark component enables processing of live streams of data (logs, queues of messages, etc...)
  - **MLlib** common ML library with ML algos (Classification, regression, clustering, filtering & model evaluation, etc... ) all these are designed to scale across cluster 
  - **GraphX** library for manipulating graphs (think social networks...) and performing || graph computation, searching, pathfinding, traversal...
  - **Cluster Managers** spark designed to efficiently scale from 1 - 1000s of nodes running over variety of cluster managers: 
    - standalone scheduler: default for simple stuff - reliable and easy to manage resources on app
    - Hdoop YARN - provided in Hadoop 2: all-purpose cluster manager 
    - Apache Mesos - general cluster manager that can also run Hadoop MapReduce with Spark & others... 
    - ?K8s?
- **Spark RDD** - resilient distributed dataset: abstraction of distr data 
  - RDD is immutable - cannot be changed - but evolves by layering on new computations
  - backbone of Apache Spark does 2 main things on RDD: 
    - (1) Transformations : create a new RDD (lazy evaluation)
    - (2) Actions: collect(), count() to return results (execute a "view" of the RDDs)

## Run-Time Execution Flow - Combined with DB Training ##
![Spark Application Executor-Core Parallelization](./images/SparkApplication_ExecutorCore.jpg)

![Spark Execution](./images/SparkApplication_ExecutionFlow.jfif) 

- Basic flow: A driver program communicates with cluster manager to schedule tasks on worker nodes, once complete they send results back to driver program. 

- (A) Spark submit will create a driver program (JVM): **Spark Driver** spark driver first action is to call main method of the program (appears different from "driver" as defined by DB training)
  - (B1) when executing in spark, you give the main class which is the entry point to your program that is executed by the spark driver 
  - (C) creates a *spark context/session* that converts a job query ?or RDD lineage? into a DAG/lineage graph (directed acyclic graph) which splits the job into stages and eventually tasks that get sent to the cluster to send to executors
  - process on a machine responsible for maintainng state of app running on the cluster
  - runs user code which creates RDD dataframes & data units abstractions 
    1. converts user program into tasks
    2. scheduels tasks on executors with the help of the cluster manager
    3. Maintains state & tasks of executors 
      - must be network accessible from worker nodes
- **Spark Cluster Manager** spark execution is agnostic to cluster manager - can use any of available cluster managers, doesn't change behavior - so long as cluster manager can provide executor processes and they communicate - spark doesn't care
  - (B2) Spark Driver will ask Cluster for worker nodes/executors  
  - Spark relies on cluster manager to 
    1. (D) *SCHEDULE & LAUNCH* executors 
    2. (D) Cluster Manager allocates resources for execution of tasks 
    - can dynamically increase/decrease executors based on workload data processing to be done 
    - these often have their own "master/worker" setup - but their terminology is primarily tied to machines rather than processes that spark works on 

>> - DB puts spark driver on cluster manager - so all the above stuff happens in DB's Driver/Instructor
>>    - runs spark application 
>>    - assigns tasks to slots in an executor 
>>    - coordinates work between tasks 
>>    - receives results (if any)
>> - **Driver AKA Teacher/Orchestrator** : for DB a JVM on a machine where application runs & responsible for 3 things:
>>  1. Maintaining info about spark application
>>  2. Responding to user's program
>>  3. Analyzing/Distributing/Scheduling Work across Executors
>>    - In a single DB Cluster only ONE Driver - regardless of number of executors 
>> - driver also looks at data and partitions it across cluster - once partitions are set, never subdivided 
  
- (E) **Spark Executor** a JVM process running on worker node that runs actual programming logic of data processing in the form of tasks (YARN could run multiple executors on a single node - DB does 1 executor PER node)
  - they will register themselves with driver program before beginning execution - so they can 

>> - **Worker Node** ~ VM/Machine that hosts executor processes - DB holds 1:1 ratio of worker node:executor
>>  - fixed # of executors allocated at any given time 
>>    - executors share machine level resources
>>  - Each **Executor** : is process running on the worker node that performs 2 kinds of work assigned by driver 
>>      1. Execute code assigned by driver
>>      2. Report state of computation back to driver 
>>        - driver can ask slot their status, and then decide to reassign task to another slot and pick the quickest result from the 2 slots 

  - a copy of program & config put on all worker nodes so that can be locally read
  - launched at beginning of spark app & when you submit jobs, they run for lifetime of spark app (holds state)
    - this can allow isolation on scheduling side (each driver schedules its own tasks) & execution side (tasks from different apps run in different JVMs)
      - allows for data separation since data cannot be shared unless written to external storage 
  1. run tasks 
  2. return results/info on state to driver 
  2. provides in-memory storage for RDD dataset & dataframes cached by user

>>  - **Spark Partition**: chunk of the data to be processed ~ 128MB
>>    - a collection of rows that sits on 1 physical machine in the cluster
>>    - Spark partition != NOT the same as hard disk partition - not related to storage space 
>>    - size/record splits for partition decided by driver 
>>    - each task processes ONE and ONLY ONE partition
>>  - **Cores AKA slots or threads** - when running on DB with 1 to 1 executor to node - can assume these are the same 
>>    - "slot" most accurate term
>>    - Executors have a # of cores/slots & each slot/core can be assigned a task and performs it on provided partition
>>    - driver needs to submit a job to that slot 
>>    - as tasks run, they share executor resources and they share same JVM heap space as others
>>      - meaning a rogue task can bring down an executor 

- Parallelization:
  - *Spark || at 2 levels - a) splitting work at executor & core*
  - Spark can scale parallelization vertically by growing size of executor/node OR horizontally by adding executor/nodes
- each node shares resources, meaning that if one pesky thread goes haywire, it can ruin other threads running on executor 
- allows isolation, because each executor/node is isolated from the others

- **Jobs, Stages & Tasks**
  - one spark action could result in one or more jobs 
  - number of stages depends on ops submitted with application
  - tasks are smallest unit of work, they share executor JVM resources 
  - in general - better performance is found through better efficient code rather than tuning deployment


![Detailed Spark Execution](./images/SparkApplication_ExecutionFlowDetailed.jfif) 

- *Execution Modes*
  - Cluster Mode (this is DB config - since DB driver schedules tasks across executors)
    - User submits pre-compiled JAR, Py script or R script to cluster manager
    - cluster manager launches driver process on worker node in addition to executor processes
    - cluster manager manages all spark app related stuff 
    - seems to be most scalable since you can have multi clients submit to the cluster
  - Client Mode
    - spark driver remains on client machine that submitted job 
    - spark driver exists on client machine and cluster manager maintains executor processes
    - client machines AKA gateway machines or edge nodes 
  - Local Mode (dev work)
    - runs spark app on single machine 

## Stages, Shuffles & Caching ## 
![Shuffling Transformations - Wide & Narrow](./images/Shuffling_WideVSNarrow.png) 

**Augmented with external articles on Wide & Narrow Dependencies** [Dave Canton](https://medium.com/@dvcanton/wide-and-narrow-dependencies-in-apache-spark-21acf2faf031) -- [Saurav Omar](https://sauravomar01.medium.com/wide-vs-narrow-dependencies-in-apache-spark-2cd33bf7ed7d)  

- **Counting Analogy** 
  - after each slot is done calculating on a particular machine/node/executor it persists the local count so that it can be picked up by the next stage
    - shuffle data: data written to disk so it can be read in later for follow up tasks (shuffle write then read!!!)
  

- Shuffle operations occur in 2 kinds - wide and narrow 
- during (C) of execution - job is broken up into stages and it needs to be planned:
  - Example Lineage: 1-read, 2-select, 3-filter, 4-groupBy, 5-filter, 6-write
  - **Narrow transformations/dependencies** (select, filter, cast, union, joins when inputs are co-partitioned): when data required to compute records in a single partition that exist in at least one partition of the parent RDD - 
  - **Wide transformations/dependencies** (distinct, groupBy, sort, join w/ inputs not co-partitioned): come from many partitions of the parent RDD
  - Marc's definition: when a function execution occurs on an input partition (parent RDD), the partition results (child RDD) can be pieces/subsets of the input partition (parent RDD) **narrow** OR pieces/subsets of MULTIPLE partitions **wide** 
    - said another way - when the child RDD partition comes from only 1 parent RDD partition, it is a narrow transformation, if it comes from multiple parent RDD partitions - then it is a wide transformation
    - because pieces/subsets of multiple partitions requires data sharing between nodes/partitions, this requires data "shuffling"
    - *to optimize performance, you will want to limit data shuffling*
- Shuffle - introduces stage boundaries, so wide transformations (by default) create stage boundaries 
  - Example Lineage turned into stages: 
    - Stage 1: "1-read, 2-select, 3-filter, 4A-groupBy, *shuffle-write*"
    - Stage 2: "*shuffle-read*, 4B-groupBy, 5-filter, 6-write" 
  - after a shuffle file has been written, it can be reused so you don't have to execute Stage 1 anymore 
- Can create additional stage breaks using cache
  - Example Lineage turned into stages WITH CACHE: 
    - Stage 1: "1-read, 2-select, 3-filter, 4A-groupBy, shuffle-write, shuffle-read, 4B-groupBy"
    - Stage 2: "**cache-read**, 5-filter, 6-write" 

### Caching ###
  - Picking up from a cache can be good/bad - spark now optimizes to the **cache step** rather than all the way back (so if you duplicate work, or decide to filter table further - these optimizations won't take place)
- by default, data of a dataframe is present on a spark cluster only while it is being processed during a query: it is not automatically persisted on the cluster afterwards
  - ??? is this why the .explain(True) for the DFs all point back to the physical parquet file ???
  - Why? because Spark is a processing engine - not data storage system that's why
- if you do cache a dataframe - you should always explicitly evict it from cache using "unpersist" after usage so it doesn't clog memory that could be used for other task executions & can prevent from performing query optimizations
- Caching Usages
  - exploratory data analysis & ML model training

# DB Component Concepts

- **DB Workspace**: grouping of envm to access DB objects such as clusters, Notebooks, Jobs, Data 
  - *Interfaces*: accessible through UI, CLI & REST API
    - REST API has 2 versions: 1.2 and 2.0 - 2.0 is preferred
    - CLI is OSS on github and built on REST API 2.0 
- Workspace assets:
  - **Clusters**: set of computational resources and configs where you run your ML/data/adhoc workloads
    - run through set of comands through notebook or automated job (interactive vs batch)
    - runs DB runtime which includes Apache Spark, optimizers, Delta Lake, Java, Py, R, Scala libraries and more 
    - come in 2 kinds of clusters
      - All-purpose clusters: analyze data collaboratively using interactive notebooks, can be created via UI, manually created, and shared among many users
      - Job Clusters: run fast automated jobs - created at runtime and dynamically terminated at the end of the job (cannot restart job cluster)
  - **Notebook**: web-based UI with group of "cells" that allow you to execute commands (reformatted program)
    - DB notebooks allow execution in Scala, Python, SQL, R 
    - can be exported as DBC files that can be migrated/imported etc
    - Notebooks are the "sql client" and can be executed interactively 
    - also provides DBUtils which is only accessible through notebooks 
      - DBUtils allow you to admin obj storage & chain/parameterize notebooks & manage secrets 
  - **Job**: construct to house indv execution of a notebook run - either in ad-hoc submission (interactive) OR batch scheduled run 
    - can be created/called through UI, CLI & REST API 
    - job status can be monitored via UI, CLI, REST & email
  - Data **DB Filesystem DBFS**: distr f/s mounted into each DB workspace and is a layer over cloud object store 
    - contains directories, data, files, etc
    - auto-populated with sample files 
    - Files in DBFS are persisted in object store 
    - through DBFS can configure access controls etc to access data you need without data duplication/migration 
  - Data **Metastore** : Manages tables and permissions and enables granting of rights/sharing data 
    - defaults to databricks central hive metastore - but can be externalized

### Demo
- %fs is a dbutils shortcut to the dbutils.fs function
- Widgets are like a STP parameter you define and can edit the value and reference throughout the notebook 
    ```
    %sql 
    CREATE WIDGET TEXT state DEFAULT "CA"
    SELECT
      *
    FROM
      events
    WHERE
      geo.state = getArgument("state")
    %python
    dbutils.widgets.multiselect("colors", "orange", ["red", "orange", "black", "blue"], "Traffic Sources")
    colors = dbutils.widgets.get("colors").split(",")
      ```

# Spark SQL & DataFrames

## Spark SQL & DataFrame API Intro
![Spark SQL Feeding Query Plan](./images/SparkSQL_QueryPlan.png)
- **Spark SQL** is a module for structured data processing with multiple interfaces: SQL OR Dataframe API: allowing use of Python, Scala, Java, R 
- **Query Engine**: converts execution of all queries so they can run on the same engine, therefore
  - you can have SQL, python, Scala all coding and able to share/reuse code 
  - **Query Plans**: ?query program? 
    - the translated query 
    - With databricks & Spark, it can optimize the query plan automatically before execution 
  - **RDDs**: Resilient Distr dataset (low level representation of datasets) 
    - spark converts our commands into RDD so we don't have to code in it

- **DataFrames**: distributed collection of data grouped into named columns 
- **Schema**: table metadata of dataframe (col names & types) 
  - Dataframe **transformations** are methods that return a new dataframe and are *lazily* evaluated (not actually run until later - ?view?)  
    - {select, where, orderBy, groupBy...} - creates a plan (~ to a view)
  - Dataframe **actions** are methods that trigger computation {count, collect, take, describe, summary, first/head, show...} are *eagerly* evaluated

## Stages of Query Optimization
![Spark Query Optimizer](./images/SparkExecution_CatalystOptimizer.png)
- When you submit a query to Spark it goes through various stages before it is executed
  - doesn't matter whether query from SQL, scala or python - if you go through the API it goes through this
  1. Unresolved Logical Plan: Query is parsed - tablenames, etc are not resolved 
  2. Analysis through Metadata Catalog: tablenames, columns are validated against catalog
  3. Logical plan: Valid query 
  4. Logical Optimization through Catalyst Catalog: First set of optimizations, rewrite/reorder, dedup...
  5. Optimized Logical Plan: generated from catalyst catalog optimization
  6. Physical Plans: Catalyst optimizer determines how/when to optimize  and generates 1+ physical plan(s) 
    - different from optimized, because optimization has been applied 
  7. Each optimization applied leads to a different cost-based plan which is put through a cost model
  8. Most optimal physical plan is selected and compiled to RDDs for execution 
- Adaptive Query Execution (AQE) in Spark 3.0+
  - disabled by default, recommended to enable - provides runtime stats plugged into logical plan 
  - can dynamically re-optimize queries like: switching join strategies, coalesce shuffle partitions

```
df = spark.read.parquet(eventsPath)
limitEventsDF = (df.filter(col("event_name") != "reviews"))
EventsDF2 = limitEventsDF.withColumn("event_timestamp", (col("event_timestamp") / 1e6).cast("timestamp"))

Physical plan:
Project - convert to event timestamp
Filter where event_name != reviews
Pull from parquet file using filter
```
- doesn't work oin csv files - Note the presence of a Filter and PushedFilters in the FileScan csv 
  - Again, we see PushedFilters because Spark is trying to push down to the CSV file.
  - However, this does not work here, and thus we see, like in the last example, we have a Filter after the FileScan, actually an InMemoryFileIndex.
 ```
 == Physical Plan ==
*(1) Filter (isnotnull(site#1206) AND (site#1206 = desktop))
+- FileScan csv [timestamp#1205,site#1206,requests#1207] Batched: false, DataFilters: [isnotnull(site#1206), (site#1206 = desktop)], Format: CSV, Location: InMemoryFileIndex[dbfs:/mnt/training/wikipedia/pageviews/pageviews_by_second.tsv], PartitionFilters: [], PushedFilters: [IsNotNull(site), EqualTo(site,desktop)], ReadSchema: struct<timestamp:timestamp,site:string,requests:int>
```

## Partitioning ## 
- Spark by default partitions data across # of threads available for execution slots/cores may not be the same thing 
- `spark.sparkContext.defaultParallelism` will return # of cores in cluster 
  - local mode has cores on local machine - Mesos etc, has min of 2 or cores on machine...
- Num of partitions of data `df.rdd.getNumPartitions()`
- **coalesce()** - narrow transformation: no shuffling, doesn't increase partitions
  - returns new DF with exactly N partitions (decrease number of partitions from current #, to N)
    - cannot coalesce to # greater than current # of partitions
    - cannot guarantee equal partition sizes
- **repartition()** - wide transformation: reshuffles ALL data, *evenly* balances partition sizes
  - returns new DF with exactly N partitions uniformly distributed - used to repartition to MORE partitions or EVEN out
- General rule - when partitioning, # of partitions to = # of cores - or at least a MULTIPLE of the # of cores
  - each partition, when cached is ~ 200 MB is considered optimal based on real-world exp 
- If i read data in with 10 partitions - do I decrease to 8 partitions? or increase to 16? 
  - depends on size of partitions, if they are small - can decrease to 8 so its  <200 MB, or spread to 16 to get <200 MB
  - Goal is to reduce num of partitions while keeping 1x # of slots
- Wide Transformations: once data is shuffled, it has to be repartitioned
  - `spark.conf.get("spark.sql.shuffle.partitions" [, "8"])` default value - 200 MB size, new partition size assigned 
    - can be tweaked using the above command to tweak the value
- **Partition Guidelines**
  - too many smaller partitions > too few large partitions 
  - Partition sizes <200MB per 8 GB of core total memory: 1/40 ratio  
    - small data - 3 partitions per core??? 
  - size default partition by dividing largest shuffle stage in put by target partition size: 4GB / 200MB = 20 shuffle partitions 
- Dynamically coalesce shuffle partitions (only in AQE Spark 3.0)
  - Best # of partitions changes based on data, the query, etc... so its best if program does it for us
  - can set large # of partitions at the beginning, then let query optimize and group into larger, more uniform partitions

```
groupdf.rdd.getNumPartitions() = returns # of partitions
coalesceDF = groupdf.coalesce(6)
repartDF = groupdf.repartition(8)
repartDF.rdd.getNumPartitions()  ==>> returns 8 
```

## SparkSession 
![Spark SQL vs DataFrame API](./images/SparkSQLvsDataFrameAPI.png)
- **SparkSession**: First step of spark application is creating a session 
  - single entry point to all dataframe API f(x)s
  - automatically created in DB notebook as var = 'spark'
  - pyspark.sql in python docs, & is useful to review both scala & python docs because they have different useful info 
- SparkSessions have the following Methods to return a dataframe:
  - sql: return dataframe result of query 
  - table: return table as a dataframe 
  - read: return dataframe reader that can be used to read data in as a dataframe 
  - range: create dataframe with column containing elements in a range from start to end with step value & # of partitions 
  - createDataFrame: creates a dataframe from list of tuples - used for testing 

# Programming in PySpark #
- **SQL CANNOT USE DATAFRAMES** 
  - To convert a dataframe that can be used by SQL - you convert it into a view using `orig_dataframe_name.createOrReplaceTempView("new_view_name")` 
    - accessible only locally and goes away after your session
  - can also use `orig_df_name.write.mode("overwrite").saveAsTable("new_table_name")`
    - a globally available table that can persist after your session
- ? inability to use upcase in where clause in spark sql or dataframe API 
- ? inability to use spark.sql on a dataframe - needs to pull from a recognized table ? 

## DataSources - simple r/w
- CSV can be read in 
- Apache **Parquet**, columnar storage format that provides efficient stored data - available to all Hadoop ecosystem
  - allows you to load only cols you need so you don't load EVERYTHING & metadata is written in the footer of the file (sounds like easy corruption? 
  - doesn't waste space storing misisng values 
  - predicate filters - pushes filters to the source 
  - data skipping - stores min/max of each segment so you can skip entire files 
  - Tamper-resistant: tough to tamper with particular rows because of its storage format 
  - if working with streaming data, you must define schema first 
- **Delta Lake**: OSS new tech to be used with spark to build robust data lakes 
  - runs on top of existing data lake to provide 
    - stores data in parquet formats
    - ACID t(x)
    - scalable metadata
    - unified streaming/batch processing 
    - data versioning
    - schema enforcement & evolution 
    - Ingestion tables (bronze) -> refined tables (silver) -> feature/agg data store (gold)
- **DataFrameReader**: can read in data from external storage from a variety of file formats 

    ```
    newDF = (spark.read.csv("/filesystempath/table.csv", sep="\t", header=True, inferSchema=True))
    # Notice .schema is before the .<filetype parquet> - if you put it after, it throws an error. Needs to "load" schema before picking up parquet file 
    newDF = spark.read.schema(predefinedSchema).parquet("/file-system/path/table.parquet")
    ```

- **DataFrameWriter**: accessible through df method write

    ```
    (df.write 
        .option("compression","snappy")
        .mode("overwrite")
        .parquet(outPath)
    )
    ```
- can use this *%scala* command to autogenerate schema info from *parquet* files that you can then use as metadata structure ```spark.read.parquet("/file-system/path/table.parquet").schema.toDDL``` 
- by pre-defining the metadata structure, it allows you to read in files ~10x faster on community DB

## DataFrames & Columns 
- entire section is basically last-mile data prep: basic table manipulations to create new calculated variables and subset tables
- **column**: logical construct that will be computed based on data in dataframe (per record basis) using an expr AKA a *calculated field*
  - can select/manipulate/remove cols from dataframes, HOWEVER their values cannot be edited as they are *derived*
  - columns simply represent a value computed on a per-record basis by means of an expression 
    - can only be transformed within context of a dataframe
- different ways to reference column depending on language
  - different ways to create column calculations

# Transformations 

### Transformations & Actions
![Common DataFrame Transformation Methods](./images/DF_TransformationMethods.png)
- column operators include comparison, basic math, changing type `cast | astype` , checking null `isNull | isNotNull | isNaN`
- these often have a corollary to SQL commands that can be used (orderBy, groupBy, limit, select, distinct, drop...)
- Actions will execute (show, count, describe, first/head, collect, take...)

### Rows 
- Methods to work with rows - length, get value of a particular position, field index...
  - get(i) is primary in scala 
- can help sort/subset rows...
  - `newDF = DF.limit(###)` works like obs in SAS - limiting # of rows read in and save to new DF
  - `purchasesDF.select("event_name","second_col").distinct()` the easiest way to make certain items
- `where | filter` are the same in pySpark 
`revenueDF = eventsDF.filter(col("ecommerce.purchase_revenue_in_usd").isNotNull())`
![Common DataFrame Row Action Methods](./images/DF_RowActionMethods.png)



## Aggregation 
- Groupby / Group Data Methods / Agg F(x)s / Math F(x)s
- **groupBy**: dataframe method that groups dataframe by specified cols so you can run aggregations on them (SQL Group By) 
  - returns *GroupedData* obj in python 
    - Grouped data object in scala is called "RelationalGroupedDataset" 
  - takes columns: avg, count, max, mean, min, pivot, sum
- Built-in Functions:
  - approx_count_distinct, avg, collect_list, corr, max, mean, stddev_samp, sumDistinct, var_pop, ceil, log, round, sqrt 
![Common DataFrame Row Action Methods](./images/GroupBy_Count.png)
- I like SQL better - BUT somehow you use dot operators for indv functions - Order of these matters
  - df.groupBy("col-name").sum("numeric") 
  - you use the agg() to allow you to perform additional dot operator functions 
  - AAAND Don't forget the IMPORT - if you don't import sum - it won't work properly! LIKE WTF
  - and you also have to do nesting of function attributes so these dot operators work: look at the sort 
    - you use the column function to house the attribute so you can use the desc option with it inside of the sort AND then apply the limit. VERY convoluted if you ask me. SQL >>>
  - and have to use funky withColumns to remake a col and round it. 

### Example Code with multi-group by, new columns rounded and sorted with a limit
```
from pyspark.sql.functions import avg, approx_count_distinct, sum, round, col
print(eventsPath) # /mnt/training/ecommerce/events/events.parquet
df = spark.read.parquet(eventsPath)
### If you want to add a where/filter - must occur BEFORE .groupBy ###
chainDF = (df.groupBy("traffic_source", "device").agg(
      sum("revenue").alias("total_rev"),
      avg("revenue").alias("avg_rev")
      )
      .withColumn("avg_rev", round("avg_rev", 2))
      .withColumn("total_rev", round("total_rev", 2))
      .sort(col("total_rev").desc())
      .limit(3)
)

# sort by 2 columns - user timestamp desc and event timestamp
eventsDF.sort(col("user_first_touch_timestamp").desc(), col("event_timestamp"))
```
- another option found through stackoverflow: You can try to use 'from pyspark.sql.functions import *' This method may lead to namespace coverage, such as pyspark 'sum' function covering python built-in 'sum' function.
- Another insurance method: `import pyspark.sql.functions as F`, use method: `F.sum`.


## Date Manipulation
- [Datetime patterns](https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html) 
- Datetime type
  - TimestampType: Represents values comprising values of fields year, month, day, hour, minute, and second, with the session local time-zone. The timestamp value represents an absolute point in time.
  - DateType: Represents values comprising values of fields year, month and day, without a time-zone.
- **Type Change:** cast, to_date
    ```
    columnName.cast("timestamp") 

    ### Need to import the TimestampType function ###
    from pyspark.sql.types import TimestampType
    columnName.cast(TimestampType())    
    ```
  - to_date converts a column to datetype() `to_date(col("columnName")) == .cast(DateType())`
    - only works with timestamp values - does not work with bigint|long|double types to date values
- **Format:** date_format
  - date_format("columnName", "MMMM dd, yyyy [date-string in pattern url above]") - RETURNS a string
- **Pulling Date attributes from a date value:**  year, month, dayofweek, minute, second, hour
  - all extract the numerical datetime item from the datetime value - `.withColumn("year", year(col("columnName")))`
- **date manipulation** with `date_add` allows you to add a numerical # of days to a date 

```
timestamp = 1593878946592107 

from pyspark.sql.types import TimestampType, DateType
from pyspark.sql.functions import date_format, hour

timestampDF = (df
        .withColumn("timestamp_orig", col("timestamp")) # long or double type that cannot be converted to DateType
        .withColumn("timestamp", (col("timestamp") / 1e6).cast("timestamp")) # division by 1MM converts to 2020 - otherwise its year 2MM or something
        .withColumn("date_type", col("timestamp").cast(DateType())) # order matters - need to pull from a timestamp value, not from a double 
        .withColumn("date string", date_format("timestamp", "MMMM dd, yyyy"))
        .withColumn("timestamp_timestring", date_format("timestamp", "HH:mm:ss.SSSSSS"))

        .withColumn("date_timestring", date_format("date_type", "HH:mm:ss.SSSSSS")) # returns a string of zeros: '00:00:00.000000'
        .withColumn("HMS_dt", hour(col("date_type")))  # returns 0 because date_type lacks specificity for these values 
              )
timestampDF.display()
timestampDF.printSchema()

timestampDF.printSchema()
#  root
#  |-- user_id: string (nullable = true)
#  |-- timestamp: timestamp (nullable = true)
#  |-- timestamp_orig: long (nullable = true)
#  |-- date_type: date (nullable = true)
#  |-- date string: string (nullable = true)   
#  |-- timestamp_timestring: string (nullable = true)
#  |-- date_timestring: string (nullable = true)
#  |-- HMS_dt: integer (nullable = true)
```
  ![Example code listing results for Datetime manipulation](./images/ExampleCode_DateTimeManipulation.png)

from pyspark.sql.functions import avg
activeDowDF = (activeUsersDF
               .withColumn("day", date_format("date", "EEE"))
               .groupBy("day").agg(
                   avg("active_users").alias("avg_users")
               )

## String & Collection Functions
![Common String Functions](./images/DF_StringFunctions.png)

![Common Collection Functions](./images/DF_CollectionFunctions.png)

![Common Aggregation Functions](./images/DF_AggregateFunctions.png)

```
 |-- items: array (nullable = true)
 |    |-- element: struct (containsNull = true)
 |    |    |-- coupon: string (nullable = true)
 |    |    |-- item_id: string (nullable = true)

detailsDF = (df.withColumn("items", explode("items")) # example selected by class is lame, but you get the idea...
# input to function explode() should be array or map type, not struct 

 |-- items: struct (nullable = true)
 |    |-- coupon: string (nullable = true)
 |    |-- item_id: string (nullable = true)
```
![Example code listing results for Explode - creates new row for each element in array/map](./images/ExampleCode_ArrayColManipulation.png)


```
details = ["Premium", "King", "Mattress"]

mattressDF = (detailsDF.filter(array_contains(col("details"), "Mattress")) # use array_contains to do filters on values
  .withColumn("size", element_at(col("details"), 2)) # explode will create new rows - withColumn for each element in an array to create columns from array elements - note that it is 1,2,3 position and NOT 0,1,2
  .withColumn("quality", element_at(col("details"), 1))
)

### Use groupBy AND agg(collect_set(...)) to group string values into arrays for each unique row
    # collect_set will de-dup and present unique set - whereas collect_list will collect all elements
optionsDF = (unionDF.groupBy("email")
  .agg(collect_set("size").alias("size options"),
       collect_set("quality").alias("quality options"))
)
email | size    ->  email | size_collectset | size_collectlist
 a@b  | S       ->  a@b   | ["S", "L"]      | ["S", "L", "L"]
 b@c  | M       ->  b@c   | ["M"]           | ["M"]
 a@b  | L       ->  c@d   | []              | []
 a@b  | L       ->
 c@d  | (null)  ->
```
## Other functions
- **lit()**: creates a column of literal value
- **isnull()**: returns boolean of true if column is null 
- **rand()**: generates random col with indp & identically distributed samples uniformly distributed in [0.0,1.0)
- **dropna()**: omitting rows with any/all or specified # of null values for all/subset of cols
- **fill()** replace null values with specified value for all/subset of cols 
  - invoked by `.na.fill('value to put in column',"colName")` and can chain together to do 1 col by 1 col
  - OR JSON array `.na.fill({"city-colname1": "unknown", "type-colname2": ""})`
- **replace()**: returns new df replacing a value with another value

## in this course they TOTALLY skip how to do joins - are you kidding me??? 
https://sparkbyexamples.com/pyspark/pyspark-join-explained-with-examples/ 

```
conversionsDF = (usersDF.join(convertedUsersDF, usersDF.email == convertedUsersDF.email,"outer"))
    will create 2 columns with identical names - but from different sources:
    root
    |-- user_id: string (nullable = true)
    |-- user_first_touch_timestamp: long (nullable = true)
    |-- email: string (nullable = true)
    |-- email: string (nullable = true)
    |-- converted: string (nullable = true)

conversionsDF = (usersDF.join(convertedUsersDF,"email","outer")
    does an automatic coalesce? - seems only way to coalesce otherwise is prior rename of cols


    .na.fill('False',"converted")    # Is == to more specific .na.fill(value='False',subset=["converted"] )
                  
Filters also don't make much sense:

abandonedCartsDF = (emailCartsDF.filter(col("converted") == 'False')
                    .filter(col("cart").isNotNull())   )
===
abandonedCartsDF2 = (emailCartsDF.filter( "converted = 'False' and cart IS NOT NULL" ) )
```

## User Defined Functions (UDFs)
- custom transformation functions applied to spark dataframes
- CONS of UDF tldr; it's slow
  - cannot be optimized by catalyst optimizeer
  - Function must be serialized and then sent to executors 
    - row data is deserialized from spark's native binary format to pass to UDF, then results serialized back into spark's native binary format
  - overhead from python interpreter & on executors running the UDF
- pandas/vectorized UDFs use apache arrow to speed up exec - in test envnm - run about the same

# Stream Processing
## Stream Processing 
- Reading through the [online guide](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) might be better than online video
- **stream processing**: act of continuously incorporating new data to a query result
  - input data is unbounded: no predetermined beginning/end
  - data forms series of events from a processing system (IoT sensory data, CC T(x), etc)
  - calc into hourly results, etc and keep up to date 
- **Batch processing**: runs on fixed input dataset - and will only compute ONCE 
- Need ability to get consistent results when querying in batch or streaming, therefore structured streaming 
  - Continuous Applications: streaming, batch, interactive all working on same data to deliver same product 
- Stream processing use-cases:
  - Notification/Alerting
  - RT Reporting
  - Incremental ETL: reduce latency for ETL for efficient queries, can get new data much faster 
  - RT data updates
  - RT decision Making - new business inputs lead to business logic decisions 
  - Online ML - most complex because requires joins, ML lib, etc
- Batch vs Streaming 
  - batch is easier to understand, troubleshoot, etc & higher throughput
  - streaming allows lower latency for fast responsetimes, efficient updates & automatic bookkeeping on new data 
    - streaming system can remember state from prior calc and adjust metrics on arrival of new data 
- **Streaming Methodologies**
  - Continuous Processing 
    - each node in the system, it is listening to updates from other nodes and passing results to child nodes 
    - Advantages: lowest possible latency with each node responding to message
    - Cons: low throughput - because of high overhead per record (contacting OS to send a packet)
      - generally have static deployments that cannot be updated without restarts -> load balancing
  - Micro Batch Processing
    - wait a certain trigger interval to accumulate data before executing batch in parallel, on a cluster behind the scenes
    - can get high throughput through batch processing (sending all at once, rather than one at a time) 
      - means they have comparatively less overhead, and in general require fewer nodes compared to Cont' Processing 
      - can use dynamic LB techniques to update/lower ?tasks? 
    - Cons: higher base latency because of trigger interval
    - when data coming too fast to be consumed continuously 
- Spark has done micro-batch processing because throughput was required 
  - in structured streaming, continuous processing is under dev
- when deciding cont vs micro-batch:
  - Look at latency requirements - micro batch latency ~ 100ms - 1s
  - TCO: Total Cost of Operation - lower # of nodes and admin overhead on cluster mgt etc
- Processing Micro-batch
  - Goal is to process ALL data arrived for each interval in under the amount of time the following interval processing begins 
  - Losing Data:
    - if you process data from a TCP/IP stream every 10 seconds and it takes 15 seconds to process, you will lose 5 seconds of the following interval of data 
    - depending on what you are analyzing, the data loss could be negligible or a big deal 
    - if you process from a pub/sub system, you simply fall behind in processing data. 
      - if delayed long enouhg, pub/sub will run into limits of holding data 
      - should add cluster nodes to stay current 
- Spark treats micro batches as continuous updates new rows appended to an unbound table 
  - dev generates a query on the table AS-IF it were static 
  - computation on input table is then pushed to results table 
    - results table is written to an output sink 
- Structured Streaming: treat a live datastream as a table that is cont' appended ~ batch processing
  - batch queries are simply run as micro queries on appended rows to your stream AKA unbounded table 
    - queries remain the same whether running in batch or streaming 
  - data items that stream in are ~ to new rows in a table 
    - the streaming query job will process new data incrementally, and update state as needed and update results 
    - run in fault tolerant fashion... 
  - afterwards results are updated into a result table which go into a sink

![Structured Streaming Query Standard](./images/StrucStreaming_PyQuery.png)

![Structured Streaming Example](./images/structured-streaming-example-model.png)

- Streaming data issues that batch doesn't deal with: Event time, out of order data
  - therefore stuctured streaming does have certain query limitations & new concepts 
- Structured Streaming + Apache Spark Batch, etc = Continuous Application
  - end-to-end app that reacts to data in RT combining all these tools
- **Supported input sources for streaming**
  - Apache Kafka, Files from distr system - HDFS/S3, socket source for testing, stable source APIs for streaming connections 
  - file source, kafka, socket source & rate source
```
Starting Streaming Queries
Once you have defined the final result DataFrame/Dataset, all that is left is for you to start the streaming computation. To do that, you have to use the DataStreamWriter (Scala/Java/Python docs) returned through Dataset.writeStream(). You will have to specify one or more of the following in this interface.

Details of the output sink: Data format, location, etc.

Output mode: Specify what gets written to the output sink.

Query name: Optionally, specify a unique name of the query for identification.

Trigger interval: Optionally, specify the trigger interval. If it is not specified, the system will check for availability of new data as soon as the previous processing has been completed. If a trigger time is missed because the previous processing has not been completed, then the system will trigger processing immediately.

Checkpoint location: For some output sinks where the end-to-end fault-tolerance can be guaranteed, specify the location where the system will write all the checkpoint information. This should be a directory in an HDFS-compatible fault-tolerant file system. The semantics of checkpointing is discussed in more detail in the next section.
```
- **Supported output sinks for streaming**: define *what* data to write out
  - file format - dumps to parquet, csv, JSON...
  - Kafka - writes to 1+ topics in Kafka
  - console - prints to stdout (generally for debugging)
  - memory - updates in-mem table that can be queried through SQL or dataframe API
  - Foreach - escape hatch - write your own kind of sink
  - Delta - DB proprietary sink
- What are sinks? specify destination for result set of a stream, 
  - sinks + execution engine, help track process of data processing 
  - **Output modes**: define *how* data is output  
    - *certain queries/sinks may only support certain output modes*
      - I.E. gathering all data - don't use complete, use append. If you are groupby agg, complete/update work, but not append
    - *complete*: entire updated output result table is written to the sink (sink implementation decides how to write entire table)
    - *append*: only new records appended to result table since last trigger are written to sink
    - *update*: only new records updated since last trigger will be output to sink
- **Trigger Types**: *when* data is output - when structured streaming should check for new input data and update results
  - *Default*: process each micro-batch as soon as previous one has been processed - in general the fasted
    - if you have a file sink - can mean you write tons of small files on fs
  - *Fixed Interval*: time-based where micro-batch is kicked off at pre-defined intervals
  - *One-Time*: process all available data as a single micro-batch and then stop query
  - *Continuous Processing*: Long-running tasks that continuously read, process and write data as soon as events available (experimental)
- Fault Tolerance:
  - if a failure occurs, streaming engine attempts to restart and/or reprocess data - can only do that if streaming source is replayable
    - assumes data source has kinesis sequence numbers, kafka offsets... 
  - End to end fault tolerance which is guaranteed by: 
    - checkpointing & write-ahead logs: to record offset range of data being processed during each interval
      - appears to store state in case data is lost
    - idempotent sinks: generate same written data for a given input, whether 1 or multiple writes are performed - even if node fails during write & can overwrite based on offsets
    - replayable data sources
  - these guarantee end-to-end exactly once semantics under any failure condition 
- Streaming Query Operations:
  - stop stream
  - await termination
  - status, isActive
  - recent progress,
  - Metadata: name, ID, runID...
![Structured Streaming Query Example](./images/StrucStreaming_PyQuery2.png) 

## Streming Processing Lab ##
```
  .withColumn("mobile", col("device").isin(["iOS", "Android"]))
```
# Other Notes #
- Various blog articles from [Analytics Vidhya](https://www.analyticsvidhya.com/blog/2021/08/understanding-the-basics-of-apache-spark-rdd/) 

## Apache Spark RDD ## 
- Various blog articles from [Analytics Vidhya](https://www.analyticsvidhya.com/blog/2021/08/understanding-the-basics-of-apache-spark-rdd/) 

- [Jacek Laskowski](https://jaceklaskowski.github.io/spark-workshop/slides/spark-sql-internals-of-structured-query-execution.html#/home)
  - Structured Queries: query over data described by a schema (data has structure - think SQL)
    - Schema is a tuple of 3 elements: Name, Type, Nullability
    - Structure matters because you can begin to optimize storage 
  - Query Languages in Spark SQL: SQL, DataFrame (untyped row-based), Dataset (typed)  
    - can use: Scala, Java, Python & R
    - Dataframes & Schemas
      - DF: a collection of rows with a schema := `Dataset[Row]`
      - Row & Row Encoder 
  - Project Catalyst - Tree manipulation framework
    - TreeNode with child nodes, where children are created as Tree Nodes and it has a recursive structure
    - rules are executed sequentially and loops are supported
  ![Project Catalyst creates treenodes out of query plans, spark plans, logical plans...](./images/sparksql-catalyst-treenodes.png)
    - *Query Plan* - base node for relational operators
    - *Logical plan* - base logical query plan
    - *Spark plan* - base physical query plan
  ![Spark Plan](./images/treenode-sparkplan.png)  ![Physical Plan](./images/treenode-physical-plan-explain.png) 
  - Query Execution: heart of any structured query
    - Structured query execution pipeline - has various phases (?stages?) 
    - DF.explain(): learn plans & DF.queryExecution(): to access phases 
    - *Spark Analyzer* to validate logical query plans
    ![Spark Plan](./images/sparksql-queryexecution-pipeline.png) 
    - *Catalyst (base) Optimizer*
      - base of query optimizers, that optimizes logical plans & expressions. Allows for cost based optimization
    - *Spark Logical Optimizer*
      - custom catalyst query optimizer with new optimization rules & added extension points 
    - *Spark Planner*
      - plans optimized logical plans to physical plans - providing at LEAST 1 physical plan for each logical 
    - *Spark Physical Optimizer* for preparations rules 
      - physical query optimizations for whole-stage code generations, subqueries... rules take a spark plan and produce a sparkplan
  - Spark core's RDD
    - A job of stages that execute in distributed fashion 
    - RDD's exist in SparkContext on the driver 
    - RDD composed of partitions: parts of data, compute methods: code 
    - partitions become tasks at runtime, where a task:= your code executed on a part of data
      - tasks are executed on executors 
    - RDD lineage shows RDD with dependencies - a graph of computation steps 
    - `DF.queryExecution.toRDD()` very last phase in a query execution, where spark SQL generates RDD to execute structured query
      - RDD is like assembler or JVM bytecode vs Dataset API is scala/python