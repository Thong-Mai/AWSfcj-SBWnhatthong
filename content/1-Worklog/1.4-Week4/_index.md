---
title: "Week 4 Worklog"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Goals for Week 4

- Learn core concepts of **Amazon S3**: bucket, object, access control, storage class, endpoint.
- Understand **backup and archival** mechanisms (Glacier) and **RTO/RPO**.
- Do labs related to backup and S3 static hosting.

### Tasks for this week

| Day | Task                                                                                                                                                                      | Start Date | End Date   | References                                       |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ------------------------------------------------ |
| 1   | Learn S3 basics: **bucket, object, key, prefix, ACL, bucket policy, storage classes, S3 endpoint, static website hosting**.                                               | 29/09/2025 | 29/09/2025 | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)         |
| 2   | Learn about **Glacier, Snowball, Snowmobile** and options for long-term archival and bulk data transfer.                                                                  | 30/09/2025 | 30/09/2025 |           |
| 3   | Learn **Disaster Recovery, RTO, RPO, AWS Backup** and when these concepts are important.                                                                                  | 01/10/2025 | 01/10/2025 | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)             |
| 4   | Backup lab: create an S3 bucket, configure a Backup Plan, test backup/restore, verify data, then clean up all the resources.                                             | 02/10/2025 | 02/10/2025 |                 |
| 5   | Repeat the S3 static website lab to improve confidence; upload more images and tweak access permissions.                                                                  | 03/10/2025 | 03/10/2025 |                |

### Results achieved in Week 4

- Understood that **S3 is object storage**:
  - Each file is an object stored inside a bucket.
  - Ideal for logs, images, static files, and raw data (like clickstream events).
- Learned:
  - Differences between S3 storage classes (Standard, IA, Glacier) and when to use each.
  - How to control access using **ACLs and bucket policies**.
- Understood DR concepts:
  - What RTO and RPO mean and how they affect system design.
- Backup lab results:
  - Now understand the workflow of “create backup plan → test restore → clean up”.
- Repeating static website hosting:
  - Made me more confident when working with S3 and less afraid of misconfiguring permissions.