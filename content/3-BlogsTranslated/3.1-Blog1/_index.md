---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS services scale to new heights for Prime Day 2025: key metrics and milestones

by Channy Yun (윤석찬) | on 26 AUG 2025 | in [Announcements](https://aws.amazon.com/blogs/aws/category/news/announcements/), [Events](https://aws.amazon.com/blogs/aws/category/news/events/), [News](https://aws.amazon.com/blogs/aws/category/news/) | [Permalink](https://aws.amazon.com/blogs/aws/aws-services-scale-to-new-heights-for-prime-day-2025-key-metrics-and-milestones/)

![anh](/images/C46-1.png)

[Amazon Prime Day 2025](https://www.aboutamazon.com) was the biggest Prime Day shopping event so far, setting new records for both total sales and the number of items purchased during the four-day promotion. Prime members saved billions of dollars while browsing and buying from millions of deals.

This year, the Prime Day experience was significantly reshaped by advances in [generative AI from Amazon and AWS](https://www.aboutamazon.com). Customers interacted with [Alexa+](https://www.aboutamazon.com), the next-generation personal assistant available in early access to millions of users, the AI-powered shopping assistant [Rufus](https://www.aboutamazon.com), and [AI Shopping Guides](https://www.aboutamazon.com).

These capabilities are built on more than 15 years of AWS cloud innovation and machine learning expertise, combined with Amazon’s deep experience in retail and consumer services. Together, they helped customers quickly discover relevant deals, get helpful product information, and enjoy the fast, free delivery that Prime members expect all year long.

As in previous years, AWS is sharing a behind-the-scenes look at how our cloud services powered Prime Day at massive scale. Below are some of the services and headline metrics that made this year’s record-breaking event possible.

![anh](/images/C46-2.png)

## Prime Day 2025 – all the numbers

In the weeks leading up to major shopping events such as Prime Day, Amazon fulfillment centers and delivery stations prepare to ensure operations are safe, efficient, and highly available. For example, the [Amazon automated storage and retrieval system (ASRS)](https://www.aboutamazon.com) uses a global fleet of industrial mobile robots to move inventory throughout Amazon facilities.

[AWS Outposts](https://aws.amazon.com/outposts/), a fully managed service that brings AWS infrastructure and services on-premises, runs software that coordinates the command-and-control of the ASRS and helps support same-day and next-day delivery with low-latency processing of critical robotic commands.

During Prime Day 2025, AWS Outposts at one of the largest Amazon fulfillment centers sent more than **524 million** commands to over **7,000** robots. At peak, the system handled **8 million commands per hour**, an increase of about **160%** compared to Prime Day 2024.

![anh](/images/C46-3.png)

Here are additional eye-catching metrics from key AWS services:

- **[Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2/)** – During Prime Day 2025, [AWS Graviton](https://aws.amazon.com/ec2/graviton/) processors powered **over 40%** of the Amazon EC2 compute used by Amazon.com. Amazon also deployed more than **87,000** [AWS Inferentia](https://aws.amazon.com/machine-learning/inferentia/) and [AWS Trainium](https://aws.amazon.com/machine-learning/trainium/) chips to run deep learning and generative AI workloads, including [Amazon Rufus](https://www.aboutamazon.com).

- **[Amazon SageMaker AI](https://aws.amazon.com/sagemaker/)** – Amazon SageMaker AI, a fully managed ML service, processed more than **626 billion** inference requests during Prime Day 2025.

- **[Amazon Elastic Container Service (Amazon ECS)](https://aws.amazon.com/ecs/) and [AWS Fargate](https://aws.amazon.com/fargate/)** – Amazon ECS, working with the serverless compute engine AWS Fargate, launched an average of **18.4 million tasks per day** on Fargate, a **77%** increase compared to the previous Prime Day average.

- **[AWS Fault Injection Service (AWS FIS)](https://aws.amazon.com/fis/)** – AWS teams ran more than **6,800** AWS FIS experiments—over **8×** as many as in 2024—to validate resilience and help keep Amazon.com highly available during the event. This growth was driven in part by new Amazon ECS support for network fault injection on AWS Fargate and deeper integration of FIS into CI/CD pipelines.

- **[AWS Lambda](https://aws.amazon.com/lambda/)** – The serverless compute service AWS Lambda handled more than **1.7 trillion** invocations **per day** during Prime Day 2025.

- **[Amazon API Gateway](https://aws.amazon.com/api-gateway/)** – Amazon API Gateway processed over **1 trillion** internal service requests, representing about a **30%** increase in average daily traffic compared to Prime Day 2024.

- **[Amazon CloudFront](https://aws.amazon.com/cloudfront/)** – The Amazon CloudFront content delivery network served more than **3 trillion** HTTP requests during the global Prime Day week, a **43%** uplift in request volume versus Prime Day 2024.

- **[Amazon Elastic Block Store (Amazon EBS)](https://aws.amazon.com/ebs/)** – Amazon EBS peaked at **20.3 trillion** I/O operations during Prime Day 2025, moving up to approximately **1 exabyte** of data per day.

- **[Amazon Aurora](https://aws.amazon.com/rds/aurora/)** – On Prime Day, Amazon Aurora processed about **500 billion** transactions, stored **4,071 TB** of data, and transferred **999 TB** of data across PostgreSQL, MySQL, and DSQL workloads.

- **[Amazon DynamoDB](https://aws.amazon.com/dynamodb/)** – Amazon DynamoDB, a fully managed serverless NoSQL database, underpins many high-traffic Amazon experiences such as Alexa, Amazon.com, and Amazon fulfillment centers. During Prime Day, these systems generated **tens of trillions** of API calls, with DynamoDB peaking at **151 million requests per second** while maintaining high availability and single-digit millisecond latency.

- **[Amazon ElastiCache](https://aws.amazon.com/elasticache/)** – Amazon ElastiCache, the fully managed in-memory caching service, reached more than **1.5 quadrillion** requests in a single day and over **1.4 trillion** requests in a single minute.

- **[Amazon Kinesis Data Streams](https://aws.amazon.com/kinesis/data-streams/)** – Amazon Kinesis Data Streams processed a peak of **807 million** records per second during Prime Day 2025.

- **[Amazon Simple Queue Service (Amazon SQS)](https://aws.amazon.com/sqs/)** – Amazon SQS, the managed message queuing service for distributed and serverless applications, set a new high-water mark of **166 million messages per second**.

- **[Amazon GuardDuty](https://aws.amazon.com/guardduty/)** – The intelligent threat detection service Amazon GuardDuty monitored an average of **8.9 trillion log events per hour**, an increase of roughly **48.9%** compared to last year’s Prime Day.

- **[AWS CloudTrail](https://aws.amazon.com/cloudtrail/)** – AWS CloudTrail processed more than **2.5 trillion** events over Prime Day 2025, up from **976 billion** events in 2024.

## Prepare to scale

If you are planning for similar business-critical events, major product launches, or large-scale migrations, AWS recommends taking advantage of the newly branded **[AWS Countdown](https://aws.amazon.com/premiumsupport/aws-infrastructure-event-management/)** program (formerly AWS Infrastructure Event Management, or IEM). This comprehensive offering helps you review operational readiness, identify and mitigate risks, and plan capacity using proven playbooks developed by AWS experts.

We have expanded AWS Countdown to include:

- **[Generative AI implementation support](https://aws.amazon.com/generative-ai/)** – Guidance to help you confidently design, launch, and scale generative AI initiatives.
- **Migration and modernization assistance**, including [mainframe modernization](https://aws.amazon.com/mainframe-modernization/) and other large-scale transformation efforts.
- **Infrastructure optimization** for specialized domains such as [election systems](https://aws.amazon.com/solutions/case-studies/elections/), [retail operations](https://aws.amazon.com/retail/), [healthcare services](https://aws.amazon.com/health/), and [sports and gaming events](https://aws.amazon.com/sports/).

![anh](/images/C46-4.png)

We are excited to see what new records our customers will set in the coming years as they build on AWS.

---

![anh](/images/C46-5.png)

### Channy Yun (윤석찬)

Channy is a Lead Blogger for the AWS News Blog and a Principal Developer Advocate for AWS Cloud. A long-time open web enthusiast and blogger, he is passionate about community-driven learning and sharing technology.
