---
title: "Event 2"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

## Location & Time

- **Location:** Bitexco Financial Tower  
- **Date:** 17/11/2025  

---

## Event Objectives

- Clarify the **DevOps mindset** – it’s not just tools, but a way of working between Dev and Ops.
- Introduce the **DevOps services on AWS**: CodeCommit, CodeBuild, CodeDeploy, CodePipeline, CloudFormation/CDK, ECS/EKS, CloudWatch, X-Ray, …
- Show how to **design a CI/CD pipeline** on AWS with different deployment strategies (blue/green, canary, rolling).
- Explain the role of **Infrastructure as Code (IaC)** in managing infrastructure.
- Summarize best practices for **monitoring, logging, and tracing** to operate systems reliably.

---

## Speakers
 
- **Thịnh Nguyễn**  
- **Bảo Huỳnh**  
- **Hoàng Long**  
- **Trần Vĩ**

---

## Key Highlights

### 1. Kick-off & DevOps Context

- **Check-in & networking:**  
  Participants connect and share about their current tech stack (on-prem, AWS, hybrid).
- **High-level introduction:**  
  Speakers recap the history of DevOps and explain why a strict separation between Dev and Ops often leads to communication gaps and delays.
- **Problem statement:**  
  How to increase release frequency, reduce deployment errors, and shorten recovery time when incidents happen.

### 2. DevOps Mindset & Metrics

- **Mindset:**  
  - Dev and Ops share responsibility for product quality and stability.  
  - Favor automated pipelines and small, frequent releases.
- **Metrics:**  
  - DORA metrics (Deployment Frequency, Lead Time, Change Failure Rate, Time to Restore).  
  - MTTR – viewed from a business impact perspective, not just technically.
- **Meaning:**  
  Use numbers to evaluate the process instead of debating based on gut feeling.

### 3. CI/CD on AWS

- **Core tools:**
  - **CodeCommit**: Git repository.  
  - **CodeBuild**: automated build/test.  
  - **CodeDeploy**: deployment to EC2/ECS/Lambda with different strategies.  
  - **CodePipeline**: the “backbone” that connects all CI/CD stages.
- **Deployment strategies:**
  - **Blue/Green:** run 2 “colors” in parallel and switch traffic after validating the new version.  
  - **Canary:** send a small fraction of traffic to the new version first.  
  - **Rolling:** update groups of servers gradually so the system doesn’t go fully down.
- **Pipeline demo:**  
  A simple pipeline from commit → build → test → deploy is demonstrated end-to-end.

### 4. Infrastructure as Code & Containers

- **CloudFormation & CDK:**
  - CloudFormation: define infrastructure using YAML/JSON templates.  
  - CDK: define infrastructure using code (TypeScript/Python/…) → easier modularization and reuse.
- **Containers on AWS:**
  - **ECR:** image registry with security scanning and lifecycle policies.  
  - **ECS/EKS:** two main ways to run containers (managed vs full Kubernetes).  
  - **App Runner:** suitable for simple web apps/containers with minimal configuration.
- **Lesson learned:**  
  Once infrastructure is treated as code, cloning environments (dev, staging, prod) becomes more consistent and less error-prone.

### 5. Monitoring & Observability

- **CloudWatch:**
  - Collects metrics (CPU, RAM, latency, …), logs, and custom metrics.  
  - Creates alarms and sends notifications via SNS.
- **X-Ray:**
  - Traces requests across multiple services, helping you “see through” microservices.
- **Observability:**
  - Combine metrics + logs + traces to reduce the time spent investigating incidents.

---

## Key Takeaways

### 1. DevOps Mindset

- Focus on the **value stream** rather than job titles.  
- Use data to measure and optimize the pipeline instead of relying on intuition.

### 2. Pipeline Design

- A good pipeline should:
  - Automate as many repetitive steps as possible.  
  - Include test stages and a staging environment before production.  
  - Have a clear rollback mechanism (especially with blue/green or canary deployments).

### 3. IaC & Containers

- Infrastructure should:
  - Be fully described with CloudFormation/CDK → version control, code review.  
  - Be validated (lint, template validation) before apply.
- Containers help:
  - Keep dev/test/prod environments similar, reducing “works on my machine” issues.

### 4. Monitoring is Mandatory

- Without monitoring, it’s very hard to practice DevOps properly.  
- Alerts should be designed around **user experience** (error rate, latency, downtime), not just “CPU is high/low”.

---

## Applying to Work

- Start building a **minimal CI/CD pipeline** for your project (even for personal projects).
- Try to describe your current infrastructure with **CloudFormation/CDK**, even at small scale.
- Set up basic CloudWatch Alarms:
  - API 5xx, latency, EC2 CPU usage, build pipeline failures, etc.
- Gradually standardize your deployment process:
  - Move away from “ssh into a server and run commands manually”.

---

## Event Experience

The “DevOps on AWS” event helped me:

- Connect fragmented knowledge about CI/CD, IaC, containers, and monitoring into **one coherent big picture**.  
- See clearly that DevOps on AWS is not just a bunch of separate services, but can be combined into a **single, continuous pipeline from code to production**.  
- Gain more motivation to apply proper DevOps practices to both my personal projects and my clickstream analytics project.

---

## Event Photos