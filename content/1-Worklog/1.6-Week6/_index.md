---
title: "Week 6 Worklog"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Goals for Week 6

- Systematize **database** knowledge: RDBMS, NoSQL, OLTP, OLAP, primary/foreign keys, indexes, partitioning, logs, buffers.
- Explore AWS managed database services: **RDS, Aurora, ElastiCache, Redshift**.
- Start drafting high-level requirements for the Clickstream project (but **not yet coding the project**).
- Get used to cost estimation and thinking about overall architecture.

### Tasks for this week

| Day | Task                                                                                                                                                                                      | Start Date | End Date   | References                                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | ---------- | ------------------------------------------------- |
| 1   | Review database concepts: **tables, sessions, primary key, foreign key, indexes, partitions, execution plans, logs, buffers, OLTP vs OLAP**.                                              | 13/10/2025 | 14/10/2025 | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)          |
| 2   | Learn about AWS database services: **RDS, Aurora, ElastiCache, Redshift** and their main use cases.                                                 | 15/10/2025 | 15/10/2025 |                    |
| 3   | Write initial notes on project requirements: need an e-commerce website, a place to store clickstream logs, and a place to process & analyze the data; list potential AWS services.        | 16/10/2025 | 16/10/2025 |                    |
| 4   | Draft a high-level architecture: CloudFront, S3, Cognito, API Gateway/Lambda, EC2 + PostgreSQL, CloudWatch, SNS… (concept level).                                                         | 17/10/2025 | 17/10/2025 |                 |
| 5   | Try AWS Pricing Calculator to estimate rough costs for a small setup (EC2, RDS, S3, CloudFront, Lambda).                                                      | 18/10/2025 | 18/10/2025 |                           |

### Results achieved in Week 6

- Reinforced **database fundamentals**:
  - Clearer understanding of OLTP vs OLAP.
  - Remembered the roles of primary/foreign keys, indexes, partitions.
- Learned:
  - When to use RDS/Aurora for transactional workloads.
  - When Redshift is suitable for analytic workloads.
  - How ElastiCache can speed up queries.
- Started to visualize the **Clickstream project**:
  - Ingestion layer (web + SDK + API).
  - Storage and processing (S3 + ETL + PostgreSQL).
  - Visualization (Shiny / BI tool).
- Drafting architecture and doing cost estimation helped:
  - Prepare mentally and technically before **officially starting the project in Week 7**.