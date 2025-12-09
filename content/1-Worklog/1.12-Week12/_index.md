---
title: "Week 12 Worklog"
weight: 2
chapter: false
pre: " <b> 1.12. </b> "
---

### Goals for Week 12

- Deep test of the e-commerce web and deploy it to **Vercel**.
- Learn to use **LocalStack Pro** and **Terraform** in the simulated environment.
- Prepare for deploying Amplify on both LocalStack and real AWS.

### Tasks for this week

| Day | Task                                                                                                                                                                                                                                                                       | Start Date        | End Date        | References            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | --------------- | --------------------- |
| 1   | Meeting: test the e-commerce site (view, add to cart, checkout), try Clerk login but fail, decide to switch to Cognito; attempt to deploy the site to Vercel, fix multiple build errors before success.                                                                    | 23/11/2025 23:00  | 24/11/2025 01:00|  |
| 2   | Write down all Vercel errors: `next.config.ts`, `package.json` misconfig, duplicate Next configs, ESLint build issues, missing environment variables; keep this log for the “Challenges & Solutions” section later.                                                         | 24/11/2025        | 24/11/2025      |     |
| 3   | Meeting: learn to use **LocalStack Pro** (configure API key, run docker image, deploy Terraform to LocalStack), assign missions for each member (Amplify, CloudFront, local Cognito, PostgreSQL on EC2, S3 for images).                                                     | 24/11/2025 20:00  | 24/11/2025 22:00|  |
| 4   | Meeting: run Terraform, study which services LocalStack can host, create a small project that only runs Amplify on LocalStack, plan how to deploy Amplify on real AWS and integrate CloudFront and S3.                                | 30/11/2025 15:00  | 01/12/2025 01:30|  |
| 5   | Personal: run `terraform apply` on LocalStack, inspect logs, record common errors and how to fix them, prepare for deploying to real AWS.                                                                                           | 01/12/2025        | 01/12/2025      |   |

### Results achieved in Week 12

- E-commerce website:
  - **Deployed successfully to Vercel** after multiple fix attempts.
  - Authentication switched from Clerk to **Cognito**.
- LocalStack Pro:
  - Configured and running; I know which services can be tested locally.
- Terraform:
  - First runs on LocalStack completed; I understand `init → plan → apply` workflow.
- Gained real-world experience with:
  - Vercel build errors, Amplify deployment, and debugging Terraform in a simulated AWS environment.