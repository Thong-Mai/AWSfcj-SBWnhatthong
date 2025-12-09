---
title: "Event 3"
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

## Location & Time

- **Location:** Bitexco Financial Tower  
- **Date:** 29/11/2025  

---

## Event Objectives

- Introduce the **Security Pillar** in the AWS Well-Architected Framework in a structured way.
- Clarify the **Shared Responsibility Model** and common security mistakes on the cloud.
- Present the 5 main areas:
  1. Identity & Access Management  
  2. Detection  
  3. Infrastructure Protection  
  4. Data Protection  
  5. Incident Response
- Show how to apply these principles to systems running on AWS (multi-account, VPC, S3, EC2, RDS, …).

---

## Speakers

- **Huỳnh Hoàng Long**  
- **Đinh Lê Hoàng Anh**  
- **Trần Đức Anh**  
- **Nguyễn Tuấn Thịnh**  
- **Nguyễn Đỗ Thành Đạt**  
- **Kha Van**  
- **Thịnh Lâm**  
- **Việt Nguyễn**  
- **Mendel Grabski (Long)**  
- **Tinh Truong**

---

## Key Highlights

### 1. Big Picture of Security on AWS

- **Role of the Security Pillar:**
  - One of the most important pillars in the Well-Architected Framework.  
  - If security is neglected, other pillars (cost, performance, reliability) will also suffer.
- **Shared Responsibility Model:**
  - AWS protects the physical infrastructure, hypervisor, and some managed service layers.
  - Customers are responsible for configuration, account management, and protecting their own data and applications.
- **Common mistakes:**
  - Unnecessarily public S3 buckets.
  - Security Groups opening 0.0.0.0/0 on sensitive ports.
  - No MFA enabled, long-lived access keys not rotated.

### 2. Identity & Access Management (IAM)

- **Access design:**
  - Use roles instead of installing long-lived access keys on servers.  
  - Apply the **Least Privilege** principle.
- **Supporting tools:**
  - IAM Identity Center, AWS Organizations, Service Control Policies (SCP), Permission Boundaries.  
  - IAM Access Analyzer to review permissions that are shared publicly or cross-account.
- **Best practices:**
  - Enforce MFA for privileged users.  
  - Standardize the process for granting, changing, and revoking access.

### 3. Detection – Monitoring & Threat Detection

- **Core services:**
  - CloudTrail: records API calls.  
  - GuardDuty: detects suspicious or anomalous behavior.  
  - Security Hub: aggregates security findings in one place.
- **Logs & events:**
  - Collect VPC Flow Logs, ALB/NLB logs, S3 access logs, etc.  
  - Use EventBridge to route security events to alert channels or response workflows.
- **Detection-as-code mindset:**
  - Define detection rules as code so they can be versioned, reviewed, and rolled back.

### 4. Infrastructure Protection

- **Network segmentation:**
  - Design clear public/private subnets and avoid direct Internet access to databases.  
  - Use Security Groups and NACLs for the right purpose.
- **Edge protection:**
  - Use AWS WAF, AWS Shield, and AWS Network Firewall for Internet-facing applications.
- **Baseline for workloads:**
  - Standardize OS configuration, patching, and hardening for EC2/ECS/EKS, etc.

### 5. Data Protection

- **Encryption & key management:**
  - Use AWS KMS to manage keys, policies, and rotation.  
  - Encrypt data at-rest for S3, EBS, RDS, DynamoDB, …  
  - Encrypt data in-transit with TLS/HTTPS.
- **Secrets management:**
  - Use AWS Secrets Manager and SSM Parameter Store.  
  - Avoid storing passwords/API keys directly in code.
- **Data classification:**
  - Identify sensitive data and apply appropriate access policies.

### 6. Incident Response

- **Basic process:**
  - Detect → contain → remediate → recover → learn (post-mortem).
- **Playbooks:**  
  Prepare predefined scenarios for:
  - Leaked access keys.  
  - Public S3 buckets with sensitive data.  
  - EC2 instances showing signs of compromise.
- **Automation:**
  - Use Lambda, Step Functions, and Systems Manager to automate part of the response actions and reduce MTTR.

---

## Key Takeaways

### 1. Security by Design

- Security must be designed **from the architecture phase**, not added at the end.
- Defense in depth: multiple small protective layers are safer than relying on a single firewall.

### 2. Planning IAM & Accounts

- A good multi-account structure + SCP helps:
  - Limit blast radius when incidents occur.  
  - Clearly separate dev/stage/prod environments.

### 3. Logging & Detection

- Without logs, you’re almost “blind” when an incident happens.  
- You need to enable logs in the right places and have a way to read/alert on them, not just turn them on and forget.

### 4. Preparing for Incident Response

- If you wait until an incident happens to think about what to do, it is usually too late.  
- Playbooks, runbooks, and periodic drills are crucial.

---

## Applying to Work

- Re-check:
  - S3 buckets, Security Groups, IAM users/roles in personal and project accounts.  
  - Enable MFA and disable unused access keys.
- Set up:
  - Basic CloudTrail, GuardDuty, and Security Hub in your learning account.  
  - A few CloudWatch Alarms related to security.
- Try writing:
  - A small incident response scenario (e.g., leaked key) and simulate how you would handle it.

---

## Event Experience

The **“AWS Well-Architected: Security Pillar”** event helped me:

- See security from the perspective of **overall architecture**, not just a few random checkboxes in the Console.
- Understand how AWS designs its security toolset to:
  - Prevent issues,  
  - Detect issues,  
  - And respond to incidents.  
- Learn that when building systems on AWS (such as my clickstream project), I need to think about security from the beginning, instead of “leaving it for later”.

---

## Event Photos

![anh](/images/event3.png) 