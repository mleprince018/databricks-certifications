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
- spark: unified analytics engine for BI data processing
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
>> - driver is not allowed to touch data: can gather results, not processed partitions
  
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
  - LOCAL COUNT: after each slot is done calculating on a particular machine/node/executor it persists the local count so that it can be picked up by the next stage
    - shuffle data: data written to disk so it can be read in later for follow up tasks (shuffle write then read!!!)
    - slots can't just send indv results back to driver and have driver calc total, executors need to do that
  - GLOBAL COUNT: Driver tells a slot to pick up the counts from the remaining slots and sum up the total
    - then take RESULT and return back to driver 
  - stage boundary results when a secondary task cannot begin until ALL the prior task(s) have completed 
    - I.E. a global count cannot occur until all local counts finish
      - exception in spark 3.x that some stages can be run in parallel -  if both inputs will be used to join
    - can become a bottleneck in performance 
    - division of local/global count|distinct|sort|aggregate is an example of a stage boundary
    - only transformations that ultimately require knowledge of the whole dataset require multiple stages
- Shuffle operation is one of the most "expensive" operations in spark -> data movement across network, I/O, etc

- Shuffle operations occur in 2 kinds - wide and narrow 
- during (C) of execution - job is broken up into stages and it needs to be planned:
  - Example Lineage: 1-read, 2-select, 3-filter, 4-groupBy, 5-filter, 6-write
  - **Narrow transformations/dependencies** (select, filter, cast, union, joins when inputs are co-partitioned): when data required to compute records in a single partition that exist in at least one partition of the parent RDD - 
  - **Wide transformations/dependencies** ( distinct, groupBy, sort, join w/ inputs not co-partitioned): come from many partitions of the parent RDD
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


