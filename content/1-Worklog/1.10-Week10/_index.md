---
title: "Week 10 Worklog"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---

### Goals for Week 10

- Revisit **CloudFront and Amplify**, decide on using LocalStack.
- Update the architecture to fit the new design (Amplify, PostgreSQL, etc.).
- Start using **Docker** for environment packaging and prepare the **report template**.

### Tasks for this week

| Day | Task                                                                                                                                                                                                                        | Start Date        | End Date        | References            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | --------------- | --------------------- |
| 1   | Meeting: review CloudFront, decide to use **LocalStack Base** (to support more services, especially Cognito), switch web database to Supabase, adjust the architecture flow.                                              | 09/11/2025 20:30  | 09/11/2025      |  |
| 2   | Discussion: list required Python libraries, check versions, update proposal so it matches the new architecture.                                                                      | 09/11/2025        | 10/11/2025      |         |
| 3   | Meeting: study Docker, how to install libraries into the image, plan to use “Docker + LocalStack” as the standard environment; at the same time, refine architecture and Vietnamese version of documentation.              | 11/11/2025 20:00  | 11/11/2025 23:30|  |
| 4   | Short meeting: update the architecture to **version 9**, agree on which services must be included.                                                                                   | 12/11/2025 20:00  | 12/11/2025 23:00|  |
| 5   | Personal: help prepare the **docx template** (sections related to worklog and workshop), plan how to upload it to Hugo later.                                                        | 12/11/2025        | 13/11/2025      |           |

### Results achieved in Week 10

- CloudFront, Amplify, Supabase, and LocalStack:
  - Were all integrated more clearly into the updated architecture.
- Architecture **v9**:
  - Better reflects the data flow and roles of each service.
- Started using **Docker**:
  - Prepared for a standardized development environment (LocalStack, Terraform, etc.).
- The report docx template:
  - Basic structure created, which will make writing future reports and workshops easier.