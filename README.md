# Databricks Certified Associate Developer for Apache Spark 3.0 - Python
- Explorations in various studies, online blogs/articles and the partners courses through Databricks
- Read the "Fundamentals Certification.md" for details! 
- My exam is set for March 26th - successful or not, I will continue to update and organize these markdown notes so that others may follow in my footsteps!

## Resources Used ##
- [Partner Resources](https://partner-academy.databricks.com/)
- [Test Overview](https://www.testpreptraining.com/tutorial/databricks-certified-associate-developer-for-apache-spark-3-0-faqs/) [Florian Roscheck Overview](https://www.youtube.com/watch?v=d9Mt67UKSio) [Barkha Saxena Medium Article](https://medium.com/@blackhat1729/beginners-guide-to-crack-databricks-certified-associate-developer-for-apache-spark-3-0-7c1aad2a578b)
    - 60 questions total - 120 minutes to take the exam
- ~30% architecture related - SparkConcepts-Architecture.md
    - Types of Qs:
        - definitions
        - how components relate to one another
        - predict spark behavior
        - must understand the THEORY
    - Covered Topics: 
        - context & hierarchy of spark components (job, stage, task, executor, slot, dataframe, RDD, partition...)
        - How spark achieves certain things (fault tolerance, lazy eval, AQE, narrow vs wide, shuffling, broadcast & accumulator vars, storage levels)
        - spark deployment modes (location of spark driver vs executors, deployment components, spark config options...)
- ~70% Spark DataFrame API - PySpark-SparkSQL-DF-Notes.md
        - Types of Qs:
        - select appropriate code block that makes spark do certain action 
        - find the error in a code block 
        - code block with blanks, and fill in the blanks
        - or code block - and you need to fill in the right order
    - Covered Topics:
        - select, rename, manipulate cols
        - filter, drop, sort and agg rows
        - join, partitioning dataframes
        - read/write dataframes to/from disk
        - work with UDFs and SparkSQL functions

## Personal notes
- areas to restudy:
    - DF API
        - read functions & options
        - write functions & options
        - joins (left, inner, right), unions (union, unionAll) 
        - cache / persist 
        - explode
        - register UDF 
        - broadcast, coalesce, col, lit 
        - sort asc, desc
        - collect, count, first, head, take, show
    - Arch
        - AQE 
        - broadcast & accumulator vars, storage levels
