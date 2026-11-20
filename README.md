# AWS Architecture Labs

Hands-on AWS labs documented as clean PDF guides, each paired with an editable
[draw.io](https://app.diagrams.net) architecture diagram, plus one end-to-end
project: a **messaging application** deployed on AWS.

![AWS](https://img.shields.io/badge/AWS-Hands--on%20Labs-232F3E?logo=amazonaws&logoColor=white)
![Labs](https://img.shields.io/badge/Labs-7-5B7DB1)
![Diagrams](https://img.shields.io/badge/Diagrams-draw.io-F08705?logo=diagramsdotnet&logoColor=white)

---

## Contents

- [Labs](#labs)
- [Project: Messaging application on AWS](#project-messaging-application-on-aws)
- [Repository structure](#repository-structure)
- [How to use this repository](#how-to-use-this-repository)
- [Conventions](#conventions)
- [Disclaimer](#disclaimer)

---

## Labs

Seven guided labs covering storage, networking, compute, load balancing, databases
and content delivery. Each lab folder contains the reformatted guide (PDF) and the
architecture diagram (`architecture/*.drawio`).

| # | Lab | AWS services | What you build and learn | Guide | Diagram |
|---|-----|--------------|--------------------------|-------|---------|
| 01 | **Introduction to Amazon EBS**<br>`SPL-TF-100-STIEBS-3` | EBS, EC2 | Create and encrypt a gp3 volume, attach and mount it (ext3), resize 30 to 50 GiB, change gp2 to gp3 with higher IOPS and throughput, snapshot and restore | [PDF](01-SPL-TF-100-STIEBS-3-ebs/SPL-TF-100-STIEBS-3_Introduction-to-Amazon-EBS.pdf) | [draw.io](01-SPL-TF-100-STIEBS-3-ebs/architecture/ebs-ec2-lifecycle.drawio) |
| 02 | **Introduction to Amazon EFS**<br>`SPL-151` | EFS, EC2, VPC, CloudWatch | Security group for NFS (TCP 2049), file system with a mount target per Availability Zone, mount with the EFS mount helper, `fio` benchmark, `PermittedThroughput` metric | [PDF](02-SPL-151-efs/SPL-151_Introduction-to-Amazon-EFS.pdf) | [draw.io](02-SPL-151-efs/architecture/efs-shared-filesystem.drawio) |
| 03 | **Introduction to Elastic Load Balancing**<br>`SPL-68` | ELB (NLB), EC2 | Network Load Balancer and target group, health checks, cross-zone load balancing, failover test | [PDF](03-SPL-68-elb/SPL-68_Introduction-to-Elastic-Load-Balancing.pdf) | [draw.io](03-SPL-68-elb/architecture/nlb-ec2-web-servers.drawio) |
| 04 | **Introduction to Amazon EC2 Auto Scaling**<br>`SPL-29` | EC2 Auto Scaling, CloudWatch | Launch template, Auto Scaling group (min 1, max 2), self-healing: terminate an instance and watch the replacement | [PDF](04-SPL-29-ec2-auto-scaling/SPL-29_Introduction-to-Amazon-EC2-Auto-Scaling.pdf) | [draw.io](04-SPL-29-ec2-auto-scaling/architecture/ec2-auto-scaling.drawio) |
| 05 | **Building with Amazon RDS Databases**<br>`SPL-TF-200-DBWARD-1` | RDS (MySQL), Secrets Manager, Lambda, EC2 | Multi-AZ MySQL, credentials in Secrets Manager with 30-day rotation, SSL/TLS in transit, Multi-AZ failover and data synchronization test | [PDF](05-SPL-TF-200-DBWARD-1-rds/SPL-TF-200-DBWARD-1_Building-with-Amazon-RDS-Databases.pdf) | [draw.io](05-SPL-TF-200-DBWARD-1-rds/architecture/rds-multi-az-secrets.drawio) |
| 06 | **Network ACLs and Security Groups in Amazon VPC**<br>`SPL-TF-100-ITCF43-1` | VPC, EC2 | Stateless network ACLs vs stateful security groups, rule evaluation, troubleshooting connectivity with `ping` | [PDF](06-SPL-TF-100-ITCF43-1-nacl-security-groups/SPL-TF-100-ITCF43-1_Network-ACLs-and-Security-Groups.pdf) | [draw.io](06-SPL-TF-100-ITCF43-1-nacl-security-groups/architecture/vpc-nacl-security-groups.drawio) |
| 07 | **Introduction to Amazon CloudFront**<br>`SPL-85` | CloudFront, S3 | Private S3 origin served through a CloudFront distribution, edge caching, blocked direct access | [PDF](07-SPL-85-cloudfront/SPL-85_Introduction-to-Amazon-CloudFront.pdf) | [draw.io](07-SPL-85-cloudfront/architecture/cloudfront-s3.drawio) |

---

## Project: Messaging application on AWS

A chat platform architecture deployed and verified in an AWS Learner Lab (`us-east-1`).

| | |
|---|---|
| **Report** | [Messaging-App-Architecture-on-AWS_Report.pdf](messaging-app-aws/Messaging-App-Architecture-on-AWS_Report.pdf) |
| **Diagram** | [messaging-architecture-aws.drawio](messaging-app-aws/architecture/messaging-architecture-aws.drawio) |

**Architecture at a glance**

- **Network:** VPC with public and private subnets across two Availability Zones, internet gateway, NAT gateway, DynamoDB endpoint, one security group per tier
- **Entry point:** internet-facing Application Load Balancer and target group (HTTP 8080) with health checks
- **Compute:** Auto Scaling group (2 to 4 `t3.small` instances) across two Availability Zones
- **Data:** RDS MySQL (Multi-AZ), ElastiCache Redis (primary and replica), DynamoDB for message history, EFS for shared files
- **Verification:** both servers answer behind the same load balancer DNS name

The report walks through 16 verified components with console screenshots, and
documents the request flow (steps 1 to 7) of a message through the platform.

---

## Repository structure

```text
aws-architecture-labs/
├── README.md
├── 01-SPL-TF-100-STIEBS-3-ebs/
│   ├── SPL-TF-100-STIEBS-3_Introduction-to-Amazon-EBS.pdf
│   └── architecture/ebs-ec2-lifecycle.drawio
├── 02-SPL-151-efs/
├── 03-SPL-68-elb/
├── 04-SPL-29-ec2-auto-scaling/
├── 05-SPL-TF-200-DBWARD-1-rds/
├── 06-SPL-TF-100-ITCF43-1-nacl-security-groups/
├── 07-SPL-85-cloudfront/
└── messaging-app-aws/
    ├── Messaging-App-Architecture-on-AWS_Report.pdf
    └── architecture/messaging-architecture-aws.drawio
```

---

## How to use this repository

**Read a guide:** open the PDF in the lab folder. Each guide follows the same layout:
overview, architecture, step-by-step tasks with commands and expected output,
conclusion and key takeaways.

**Edit a diagram:** open the `.drawio` file with
[app.diagrams.net](https://app.diagrams.net) or the *Draw.io Integration* extension for VS Code.

**Clone the repository:**

```bash
git clone <repository-url>
cd aws-architecture-labs
```

---

## Conventions

- **Folder names:** `NN-<lab code>-<topic>`, numbered in the order the labs were completed.
- **File names:** `<lab code>_<Lab-title>.pdf` for the guide, a short descriptive name for the diagram.
- **Diagrams:** AWS architecture icons, colors by role (network, compute, data, security).
- **Lab codes** (for example `SPL-151`) are the identifiers printed on the original AWS lab guides.

---

## Disclaimer

This repository contains personal study notes. The lab guides are reformatted from
AWS Training and Certification material (&copy; Amazon Web Services, Inc. or its affiliates,
all rights reserved). AWS and the AWS service names are trademarks of Amazon Web Services.
This project is not affiliated with or endorsed by AWS.

Screenshots in the messaging project come from a temporary AWS Learner Lab account.

---

**Author:** 
<p align="center">
  <strong>Ons Ajmi</strong> — Engineering Student in Cloud Infrastructure Management @ TEK-UP University<br>
  GitHub : <a href="https://github.com/AjmiOns">AjmiOns</a> · 
  LinkedIn : <a href="https://www.linkedin.com/in/ons-ajmi-0ab2982a2/">Ons Ajmi</a>
</p>
