# Databricks Certified Associate Developer for Apache Spark 3.0 - Python
- Welcome to my journey to passing the Databricks Certified Associate Developer for Apache Spark 3.0 - Python exam
- I [passed](https://credentials.databricks.com/224539c6-c71b-4897-965d-77448b9497a1) the exam on March 26th 2022 on the first try!!! 
- Please read along below to review my notes and see what I would have done differently.

## Exam Breakdown & Overview ##
- [Florian Roscheck Overview](https://www.youtube.com/watch?v=d9Mt67UKSio) || [Test Overview](https://www.testpreptraining.com/tutorial/databricks-certified-associate-developer-for-apache-spark-3-0-faqs/) || [Barkha Saxena Medium Article](https://medium.com/@blackhat1729/beginners-guide-to-crack-databricks-certified-associate-developer-for-apache-spark-3-0-7c1aad2a578b)
    - Before reading further - or even booking your exam - review the prep notes & links above
    - **Time Commitment:**
        - it took me about 4 months to practice and cover all the material while holding a full-time job
        - Putting in about 5h+ per week ~ 100h+ would be a LOW estimate of time/effort I put into studying for this. I would plan on 125-150h of study for this exam if you have no experience with an analytics tech stack or python. 
        - To avoid cramming so much at the end, I would have gotten more serious about studying the right topics from the start, and these exam overviews can help you plan your study and approach 

- ~30% architecture related - [SparkConcepts-Architecture.md](https://github.com/mleprince018/databricks-certifications/blob/main/SparkConcepts-Architecture.md)
    - Types of Qs:
        - definitions
        - how components relate to one another
        - predict spark behavior
        - must understand the THEORY
    - Covered Topics: 
        - context & hierarchy of spark components (job, stage, task, executor, slot, dataframe, RDD, partition...)
        - How spark achieves certain things (fault tolerance, lazy eval, AQE, narrow vs wide, shuffling, broadcast & accumulator vars, Memory management & storage levels)
        - spark deployment modes (location of spark driver vs executors, deployment components, spark config options...)
            - What's important to understand when testing in this section is there is a difference between DATABRICKS Spark and generic Apache Spark 
            - make sure when you are learning the architecture, you understand the databricks deployment and architecture because that will be what is tested 
    - Additional Resources: 
        - First stop needs to be this course: Introduction to Apache Spark Architecture - ID: E-Z1G4ZV
            - from either the client academy or partner academy, this provides the foundational concepts and key ideas that you can carry with you through the rest of any architecture lesson. The teacher/student/desk analogy will help you better understand nodes, slots, executors, jobs, stages, etc... 
        - Next, because the concepts were different and hard to learn, I learned through online articles and blogs listed in my markdown as well as Florian's Q&A explanations 
- ~70% Spark DataFrame API - [PySpark-SparkSQL-DF-Notes.md](https://github.com/mleprince018/databricks-certifications/blob/main/PySpark-SparkSQL-DF-Notes.md)
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
    - Additional Resources:
        - Practicing pySpark: Sign-up for [Databricks Community Edition](https://community.cloud.databricks.com/login.html)
        - [Florian's Certified Developer for Spark 3.0 Practice Exams](https://www.udemy.com/course/databricks-certified-developer-for-apache-spark-30-practice-exams/)
            - this is an absolute gold-mine, worth every penny. It has questions that conceptually match the exam, with detailed explanations to help you understand and learn. 
            - the third exam is more of a brain-teaser than something that will help you pass, but his exams are by far the most comprehensive. 

## 'If I had to do it again' Timeline Review ## 
- Learning Phase ~4 months
    - Learning Spark Dataframe API
        - Whether studying the course work on the databricks website or other Udemy courses, you need to practice and CODE these multiple times over until you can visualize the syntax for the exam. 
        - These were helpful in foundational knowledge, but these honestly help you with about 25% of the test. 
        - From understanding key concepts and basics, to basic syntax these can help with the questions you will face during the exam. 
        - I personally used the Apache Spark™ Programming with Databricks ID: E-P0W7ZV course, but going through the exercises, it definitely is not enough to pass the exam. 
    - Learning Architecture 
        - As this is about learning concepts - the best way I learned was through youtube explanations (databricks and spark conferences) and reading the plethora of blog articles 
        - the Intro to Spark Architecture is also a phenomenal course to review as well. 
        - To me, the mark of understanding architecture is when you can explain it so a family member understands it. 
- Testing & Refinement ~ 1 month 
    - This is where I would take 1 exam of Florian's every week and study all the questions you got right/wrong to build understanding 
    - Practice on your own and read articles/documentation to complete your knowledge so that you know this information front-to-back
- Final cram ~ 1 week
    - take the [official practice exam](https://files.training.databricks.com/assessments/practice-exams/PracticeExam-DCADAS3-Python.pdf) for a confidence boost
    - practice and test and review anything lingering on the topics list and that you don't fully understand
    - there's a trick question or two on the use of some unique functions that are worth brushing up on before the exam

> After that - it's best of luck! You've got the knowledge and know-how to pass this certification with flying colors! 



