# Fundamentals Certification #


#### Reference notes and markdown
- Main Study Resources:
  - [ACloudGuru - Solutions Architect Associate](https://www.udemy.com/course/aws-certified-solutions-architect-associate/learn/lecture/13885822?start=0#overview)
    - Very good course on covering foundational services and concepts for AWS Solutions Architect
  - [AWS Certified Solutions Architect Associate Practice Exams (5)](https://www.udemy.com/course/aws-certified-solutions-architect-associate-amazon-practice-exams-saa-c02/)
    - cannot recommend this enough, while the ACloudGuru gave me the foundation and core to get about 50%, these exams and their explanations took me the rest of the way
    - EXCELLENT explanations for each question (right or wrong) as to why the correct answer is correct and all others are not as good or incorrect
  - Alternative to ACloudGuru: [OReilly AWS Certified Solutions Architect](https://learning.oreilly.com/videos/aws-certified-solutions/9780136721246/9780136721246-ACS2_00_00_00)
    - Although I never used it, this was going to be the backup course I took. I only used the question reviews at the end of each section. 
    - Much better framed to scope of AWS cert exam than acloudguru


#### AWS Identities used for Authentication ##
- **AWS account root user**
  - created with AWS ACC - has full access to all services/resources within acc
  - sign in with email
  - best practice is to create an IAM user and then let that one do all tasks
  - Instance Lifecycle: 
  
  ![Instance Lifecycle](./images/instance_lifecycle.png)