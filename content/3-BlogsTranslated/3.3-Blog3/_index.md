---
title: "Blog 3"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Secure SAP HANA Cloud connectivity using AWS PrivateLink

by Sejun Kim, Arne Knoeller, Thuan Bui Thi, and Whye Loong Wong | on 26 AUG 2025 | in [AWS PrivateLink](https://aws.amazon.com/privatelink/), [SAP on AWS](https://aws.amazon.com/sap/), [Best Practices](https://aws.amazon.com/blogs/awsforsap/) | [Permalink](https://aws.amazon.com/blogs/awsforsap/secure-sap-hanacloud-connectivity-using-aws-privatelink/)

## Introduction

Modern enterprises need database platforms that can serve both **transactional** and **analytical** workloads while still meeting strict security and compliance requirements. **SAP HANA Cloud on AWS**, a key service in the SAP Business Technology Platform (SAP BTP), addresses these needs as a fully managed, cloud‑native database-as-a-service.

Organizations use SAP HANA Cloud to run core business applications (ERP, CRM, HR, logistics) and to power analytics solutions such as **SAP Analytics Cloud**, **SAP Datasphere**, and custom applications built with the SAP Cloud Application Programming Model (CAP).  

This post shows how to harden the **network path** to SAP HANA Cloud by integrating it with **AWS PrivateLink**, so that traffic flows over private connectivity within the AWS network instead of traversing the public internet.

## Why SAP HANA Cloud on AWS?

SAP HANA Cloud instances on AWS run on **AWS Graviton–based Amazon EC2** infrastructure. Compared to similar x86 instances, Graviton delivers better price–performance for analytical workloads, lowers compute cost and energy consumption, and helps reduce the carbon footprint of SAP HANA Cloud deployments. Customers running SAP HANA Cloud on AWS benefit directly from these performance and sustainability improvements.

## Connecting to SAP HANA Cloud on AWS

By default, applications connect to SAP HANA Cloud using an **internet‑accessible endpoint**. Clients from on‑premises networks or from other clouds send traffic over the public internet, secured using TLS, to reach the database.

![anh](/images/figure_01.png)

However, customers in highly regulated industries (such as financial services, healthcare, or public sector) often have policies that disallow direct access over public endpoints, even when encryption is used. These organizations want:

- Network paths that stay on private IP ranges (RFC 1918).
- A reduced attack surface with no public database endpoint.
- Clear separation between user traffic and administrative access.

To meet these requirements, SAP and AWS support connecting SAP HANA Cloud through **AWS PrivateLink**, which keeps the traffic on the AWS backbone and exposes the database to your VPC using **private interface endpoints**.

## What is AWS PrivateLink?

**AWS PrivateLink** provides private connectivity between VPCs, AWS services, and partner services – including SAP HANA Cloud – without exposing traffic to the public internet.

Key characteristics:

- Traffic flows through **interface VPC endpoints**, which are elastic network interfaces with private IP addresses in your VPC subnets.  
- You can attach **security groups** and **endpoint policies** to control which resources can reach the endpoint.  
- There is no need for VPC peering or AWS Transit Gateway, and there are no overlapping CIDR constraints between consumer and provider VPCs.  
- All packets stay on the global AWS network, improving security and latency characteristics.

When SAP HANA Cloud is published as an endpoint service and your VPC connects to it via an interface endpoint, clients inside that VPC (and connected environments such as on‑premises via AWS Direct Connect or VPN) can reach SAP HANA Cloud using private IP connectivity.

![anh](/images/figure_02.png)

## SAP HANA Cloud and AWS PrivateLink – example use cases

Below are two common scenarios where PrivateLink integration is especially valuable.

### Use case 1 – Secure administrative access to SAP HANA Cloud

Database administrators and developers often need broad privileges to perform schema changes, tuning, and maintenance on SAP HANA Cloud. Allowing these high‑privilege connections over a public endpoint increases risk.

With AWS PrivateLink:

- SAP HANA Cloud is reachable only from **approved VPCs or corporate networks**.  
- Administrative tools such as **SAP HANA Cockpit** or SQL clients connect through a **private interface endpoint**.  
- Access is controlled using **security groups**, **network ACLs**, and identity‑aware controls such as SSO or multi‑factor authentication.

This pattern enables enterprise‑grade security for administrator and developer traffic while keeping standard application traffic routes unchanged.

![anh](/images/figure_03.png)

### Use case 2 – Secure data platform integration on AWS

Many customers build data platforms on AWS and need to integrate **SAP data** from SAP HANA Cloud into their analytics stack. Typical patterns include:

- Using **AWS Glue** jobs to extract data from SAP HANA Cloud and load it into **Amazon S3** or downstream services such as Amazon Redshift.  
- Using **Amazon Athena** with an SAP HANA connector to run federated queries directly against SAP HANA Cloud from within AWS analytics tools.

In both patterns, AWS PrivateLink ensures that:

- Data moves between AWS services and SAP HANA Cloud over **private network paths**.  
- No public IPs or internet gateways are involved in the data‑in‑motion path.  
- Security and compliance requirements for sensitive business data can be met more easily.

![anh](/images/figure_04.png)

## High-level setup – SAP HANA Cloud with AWS PrivateLink

The blog walks through a configuration scenario for use case 1 (secure administrative access). At a high level, the steps are:

1. **Enable AWS PrivateLink for the SAP HANA Cloud instance** in the SAP HANA Cloud Central UI and obtain the **endpoint service IDs**.  
2. **Create an interface VPC endpoint** in your AWS account that points to the SAP HANA Cloud PrivateLink service.  
3. **Add the VPC endpoint ID to the allow list** in the SAP HANA Cloud instance configuration.  
4. **Configure a private hosted zone in Amazon Route 53** and create DNS records that map the SAP HANA Cloud hostname to the PrivateLink endpoint DNS name.  
5. **Test private routing** from an Amazon EC2 instance inside the VPC to ensure that HANA traffic uses the endpoint.  
6. (Optional, for hybrid scenarios) **Configure Route 53 Resolver inbound endpoints** so that on‑premises DNS queries can resolve the private HANA hostname through your VPC.

The original post also provides example screenshots from both SAP HANA Cloud Central and the AWS Management Console that illustrate each step in detail.

### Step 1 – Enable AWS PrivateLink in SAP HANA Cloud

From SAP HANA Cloud Central:

- Select the SAP HANA Cloud instance.  
- Open **View Configuration** and switch to the **Connections** tab.  
- Enable the option to expose **AWS PrivateLink endpoint IDs**.  
- Restrict allowed connections so that traffic must originate from Cloud Foundry in the BTP region and/or from specific IP ranges.

This configuration exposes one or more **PrivateLink endpoint service IDs** that you will reference when creating the VPC endpoint.

![anh](/images/figure_05.png)

### Step 2 – Create the interface VPC endpoint

In the AWS Management Console, under **Amazon VPC**:

1. Choose **Endpoints** → **Create endpoint**.  
2. Select *PrivateLink-ready partner services* as the service type.  
3. Paste the **service name** corresponding to the SAP HANA Cloud PrivateLink endpoint service and verify it.  
4. Choose the target **VPC**, **Availability Zones**, and **subnets** in which the endpoint should be created.  
5. Attach a **security group** that allows inbound HTTPS (TCP 443) from the CIDR ranges where administrators or applications reside.

Once the endpoint status is **Available**, note:

- The **endpoint ID** – used in the SAP HANA Cloud allow list.  
- The **DNS names** – later referenced by Route 53.  
- The **private IP addresses** – useful for network troubleshooting.

![anh](/images/figure_09.png)

### Step 3 – Allow the VPC endpoint in SAP HANA Cloud

Back in SAP HANA Cloud Central, add the VPC endpoint ID to the **allowed endpoint list** for the instance. After this change is applied, only traffic coming via the approved PrivateLink endpoints will reach the database using the private connectivity option.

Further steps (Route 53 DNS setup, testing, and hybrid DNS forwarding) follow standard AWS patterns and ensure that client applications seamlessly use the private hostname instead of the default public endpoint.

## Pricing considerations

When you enable PrivateLink for SAP HANA Cloud:

- There is **no extra charge on the SAP BTP side** for activating AWS PrivateLink; SAP manages the Network Load Balancer that fronts the service.  
- You pay only for the **AWS resources in your own account**, such as the interface VPC endpoint and any data processing charges for PrivateLink traffic.

The AWS Pricing Calculator can help you compare architectures **with and without** high availability (for example, endpoints in multiple Availability Zones) so you can estimate ongoing costs.

## What’s next?

To dive deeper:

- Review the AWS documentation **“What is AWS PrivateLink?”** and the guide **“Connect your VPC to services using AWS PrivateLink.”**  
- Explore SAP HANA Cloud documentation and the **SAP HANA Cloud Database Administration Guide** for PrivateLink and network configuration details.  
- Try the associated workshop lab to practice configuring SAP HANA Cloud with AWS PrivateLink in a sandbox environment.

By combining SAP HANA Cloud with AWS PrivateLink, you can build secure, performant data platforms that keep critical database traffic on private networks while still benefiting from the flexibility of the cloud.
