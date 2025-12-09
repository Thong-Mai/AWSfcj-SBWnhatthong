---
title: "Week 12 Worklog"
weight: 2
chapter: false
pre: " <b> 1.12. </b> "
---

### Goals for Week 13

- Deploy the web app to **AWS Amplify** (real environment), enable CloudFront, integrate Cognito.
- Design a large dataframe for clickstream data and split it into multiple tables.
- Deploy required services to AWS using Terraform.
- Finish the remaining parts of the project: data cleaning, charting, and preparing the Workshop (English/Vietnamese).

### Tasks for this week

| Day | Task                                                                                                                                                                                                                                                                                                        | Start Date        | End Date        | References            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------- | --------------- | --------------------- |
| 1   | Meeting: deploy the web app to **AWS Amplify**, enable CloudFront inside Amplify, check if Amplify has its own S3 for assets, integrate Cognito; design a large clickstream dataframe and split it into ~5 tables; run Terraform to deploy required services, create resources in AWS cloud.                 | 01/12/2025 22:30  | 02/12/2025 03:00|  |
| 2   | Personal assignment: **research CloudWatch & SNS**, decide which services to monitor and which metrics are important (5xx, 4xx, latency, CPU, billing, etc.), and how to send alerts via SNS.                                                                          | 02/12/2025        | 04/12/2025      |  |
| 3   | Meeting: finish remaining project parts – fix NaN issues in the data tables, draw analysis charts (traffic, user behavior, products), assign Workshop responsibilities , pushes Shiny code to Git.                              | 07/12/2025 00:00  | 08/12/2025 00:00|  |
| 4   | Personal: write the **Vietnamese Workshop** version, keep core technical terms in English where appropriate, emphasize architecture, data pipeline, challenges & solutions, and lessons learned.                                                                     | 07/12/2025        | 08/12/2025      |      |
| 5   | Personal: review the entire Worklog, highlight key points for later use in formal reports and self-assessment.                                                                                                                | 08/12/2025        | 08/12/2025      |       |

### Results achieved in Week 13

- Web app:
  - Running on **AWS Amplify** with CloudFront in front.
  - Successfully integrated **Cognito**, ready to associate users with clickstream events.
- Clickstream dataframe:
  - Designed and split into multiple well-structured tables, suitable for analysis.
- Terraform:
  - Used to deploy main services on real AWS, making the infrastructure reproducible.
- **CloudWatch & SNS**:
  - I understand how to monitor key metrics and send alerts on abnormal events (design & research level).
- Project:
  - NaN issues in data were fixed, major analysis charts were generated.
  - Shiny code was uploaded to Git.
- Workshop:
  - The Vietnamese Workshop version I wrote summarizes:
    - Context, architecture, and data pipeline.
    - Challenges (deploy, data, infrastructure, cost).
    - Possible improvements if we had more time.