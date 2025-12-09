---
title: "Week 2 Worklog"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Goals for Week 2

- Understand basic networking concepts in AWS: **VPC, Subnet, Route Table, Security Group, Internet Gateway, NAT**.
- Learn how to **control traffic flow** in and out of a VPC.
- Practice creating a VPC, Subnets, and an EC2 instance, then test SSH connectivity.
- Get used to reading and drawing AWS network diagrams.

### Tasks for this week

| Day | Task                                                                                                                                       | Start Date | End Date   | References                                              |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ------------------------------------------------------- |
| 1   | Learn AWS networking basics: **VPC, CIDR, Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group, NACL**.                      | 15/09/2025 | 17/09/2025 | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)  |
| 2   | Watch demos of higher-level networking models: **VPC Peering, VPN, Direct Connect, Transit Gateway** (concept level).                      | 18/09/2025 | 18/09/2025 | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)                        |
| 3   | Hands-on lab: create a **custom VPC** with 2 subnets (public/private), attach Internet Gateway, configure routing for each subnet.         | 19/09/2025 | 19/09/2025 |                            |
| 4   | Launch an EC2 instance in the public subnet, attach a Security Group, test SSH from local machine (MobaXterm/Putty).                      | 20/09/2025 | 20/09/2025 |                         |
| 5   | Draw the network diagram (by hand or with an online tool) showing VPC, subnets, EC2, IGW, and route tables that were created.             | 21/09/2025 | 21/09/2025 |                       |

### Results achieved in Week 2

- Understood that **VPC is like a private network** in the cloud:
  - Can differentiate between public and private subnets.
  - Knows the role of Internet Gateway and NAT Gateway for Internet access.
- Practiced:
  - Creating VPCs, subnets, route tables, attaching an Internet Gateway.
  - Launching an EC2 instance in a public subnet and connecting via SSH.
- Started to get used to **drawing network diagrams**:
  - Helps visualize traffic flow and security boundaries.
  - Makes it easier to discuss and design architecture as a team.
- This week laid the foundation for:
  - Later deploying PostgreSQL on EC2 in a private subnet.
  - Designing network architecture for the Clickstream project.