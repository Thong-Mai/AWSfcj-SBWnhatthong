---
title: "Week 3 Worklog"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Goals for Week 3

- Learn core concepts of **Amazon EC2** and how to choose instance types.
- Practice creating, stopping, and starting EC2 instances; understand **EBS, AMI, key pair**.
- Do labs on **backup/restore** and **S3 static website**, to connect compute & storage knowledge.

### Tasks for this week

| Day | Task                                                                                                                                                 | Start Date | End Date   | References                                            |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------------------- |
| 1   | Learn key EC2 concepts: **instance types, AMI, EBS, instance store, user data, metadata, Auto Scaling**.                                             | 22/09/2025 | 23/09/2025 | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)             |
| 2   | Practice: create an EC2 instance, attach an EBS volume, test start/stop, change instance type to see effect on resources and cost.                  | 24/09/2025 | 24/09/2025 |                    |
| 3   | Backup lab: create backups, run a restore test, then clean up all resources to avoid unnecessary charges.                                           | 25/09/2025 | 25/09/2025 | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)              |
| 4   | Practice **static website hosting on S3**: upload HTML files, configure static hosting, test public access.                                         | 26/09/2025 | 26/09/2025 | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)              |
| 5   | Write notes comparing: using **EC2 + web server** vs **S3 static hosting** for simple websites.                                                      | 27/09/2025 | 27/09/2025 |               |

### Results achieved in Week 3

- Learned the role of **EC2**:
  - Virtual servers with customizable CPU/RAM/disk.
  - Understood EBS volumes vs instance store and their use cases.
- Can:
  - Create, stop, start, and resize an EC2 instance.
  - Use key pairs for SSH and manage access securely.
- Completed backup lab:
  - Understood the importance of **backup & restore** in production systems.
- Built a static website on S3:
  - Realized that for certain scenarios, S3 + static hosting is simpler, cheaper, and easier to manage than EC2.
- Started to imagine that later:
  - EC2 will host **PostgreSQL + R Shiny** in the Clickstream project.