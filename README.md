# Cloud Engineering System Design

A hands-on collection of **Cloud Engineering System Design challenges** focused on designing scalable, secure, highly available, reliable, and cost-efficient cloud architectures.

Each system design problem is solved visually using **draw.io**, with explanations of the architecture, AWS services, security decisions, scalability, failure handling, monitoring, and cost considerations.

The goal of this repository is to practice real-world architecture thinking for roles such as:

- Cloud Engineer
- Cloud Security Engineer
- DevOps Engineer
- Platform Engineer
- Infrastructure Engineer
- Site Reliability Engineer
- Solutions Architect

---

## Architecture Platform

Most designs in this repository use **AWS**, but the architectural concepts can also be applied to Azure and Google Cloud.

Main technologies include:

- AWS
- Terraform
- Docker
- Kubernetes / EKS
- ECS / Fargate
- Lambda
- API Gateway
- S3
- CloudFront
- Route 53
- VPC
- IAM
- RDS / Aurora
- DynamoDB
- SQS
- SNS
- EventBridge
- CloudWatch
- CloudTrail
- AWS WAF
- KMS
- Secrets Manager
- GitHub Actions
- Jenkins

---

# System Design Challenges

## 1. Highly Available Web Application

**Question:**  
Design a highly available web application on AWS that can continue running if an EC2 instance or an entire Availability Zone fails.

**Topics:**

- Route 53
- Application Load Balancer
- Auto Scaling
- Multi-AZ architecture
- EC2
- RDS Multi-AZ
- Public and private subnets
- CloudWatch

---

## 2. Secure Three-Tier Web Architecture

**Question:**  
Design a secure three-tier application with separate web, application, and database layers.

**Topics:**

- VPC
- Public subnets
- Private subnets
- ALB
- EC2 / ECS
- RDS
- Security Groups
- NAT Gateway
- IAM
- Encryption

---

## 3. Global Static Website

**Question:**  
Design a secure and globally distributed static website.

**Topics:**

- Route 53
- CloudFront
- S3
- Origin Access Control
- ACM
- AWS WAF
- Caching

---

## 4. Serverless REST API

**Question:**  
Design a scalable serverless API that can handle unpredictable traffic.

**Topics:**

- API Gateway
- Lambda
- DynamoDB
- Cognito
- IAM
- CloudWatch
- WAF

---

## 5. Large File Upload System

**Question:**  
Design a system where users can securely upload very large files without sending the files through the application server.

**Topics:**

- S3
- Presigned URLs
- API Gateway
- Lambda
- IAM
- Multipart Upload

---

## 6. Event-Driven File Processing System

**Question:**  
Design a system that automatically processes files after users upload them to S3.

**Topics:**

- S3 Events
- EventBridge
- Lambda
- SQS
- DynamoDB
- SNS
- Dead Letter Queue

---

## 7. Asynchronous Job Processing System

**Question:**  
Design a system capable of accepting thousands of jobs while allowing backend workers to process them asynchronously.

**Topics:**

- API Gateway
- SQS
- Lambda
- ECS workers
- Dead Letter Queue
- Retry mechanisms
- CloudWatch

---

## 8. Highly Scalable Web Application

**Question:**  
Design a web application that normally receives low traffic but can suddenly scale to hundreds of thousands of users.

**Topics:**

- CloudFront
- ALB
- Auto Scaling
- EC2 / ECS
- ElastiCache
- RDS / Aurora
- DynamoDB

---

## 9. Highly Available Database Architecture

**Question:**  
Design a database architecture that minimizes downtime during infrastructure or Availability Zone failures.

**Topics:**

- RDS Multi-AZ
- Aurora
- Automated backups
- Failover
- Monitoring

---

## 10. Read-Heavy Database Architecture

**Question:**  
Design a database architecture for an application receiving extremely high numbers of read requests.

**Topics:**

- Read Replicas
- Aurora Replicas
- ElastiCache
- DynamoDB
- Caching

---

## 11. Multi-Region Application

**Question:**  
Design an application that serves users from multiple geographic regions with low latency.

**Topics:**

- Route 53
- CloudFront
- Multiple AWS Regions
- ALB
- Aurora Global Database
- DynamoDB Global Tables

---

## 12. Disaster Recovery Architecture

**Question:**  
Design a disaster recovery strategy for a critical application if an entire AWS Region becomes unavailable.

**Topics:**

- Backup and Restore
- Pilot Light
- Warm Standby
- Active-Passive
- Active-Active
- Route 53 failover
- Cross-region replication
- RTO
- RPO

---

## 13. Secure Private Application Architecture

**Question:**  
Design an application where backend servers and databases must not be publicly accessible.

**Topics:**

- Private subnets
- NAT Gateway
- VPC Endpoints
- ALB
- Security Groups
- IAM
- Systems Manager

---

## 14. VPC Internet Access Architecture

**Question:**  
Design networking that allows servers inside private subnets to access the internet while preventing inbound internet access.

**Topics:**

- VPC
- Public subnet
- Private subnet
- Internet Gateway
- NAT Gateway
- Route Tables

---

## 15. Private AWS Service Access

**Question:**  
Design an architecture where private EC2 instances access S3 and DynamoDB without sending traffic over the public internet.

**Topics:**

- Gateway VPC Endpoints
- Interface Endpoints
- Private networking
- IAM
- Endpoint policies

---

## 16. Secure IAM Architecture

**Question:**  
Design IAM access for applications running on EC2, Lambda, and containers without storing permanent AWS credentials.

**Topics:**

- IAM Roles
- Instance Profiles
- Lambda Execution Roles
- EKS Pod Identity / IRSA
- Least Privilege
- Temporary credentials

---

## 17. Centralized Logging System

**Question:**  
Design a centralized logging architecture for multiple applications and AWS services.

**Topics:**

- CloudWatch Logs
- CloudTrail
- S3
- Kinesis
- OpenSearch
- Log retention
- Security monitoring

---

## 18. Cloud Monitoring and Alerting System

**Question:**  
Design a monitoring system capable of detecting infrastructure and application failures.

**Topics:**

- CloudWatch Metrics
- CloudWatch Logs
- CloudWatch Alarms
- SNS
- Dashboards
- Application monitoring

---

## 19. Cloud Security Monitoring Architecture

**Question:**  
Design a system for detecting suspicious activity and security threats inside an AWS environment.

**Topics:**

- GuardDuty
- Security Hub
- CloudTrail
- AWS Config
- EventBridge
- Lambda
- SNS
- Detective

---

## 20. CI/CD Pipeline for Cloud Applications

**Question:**  
Design a CI/CD pipeline that automatically tests, scans, builds, and deploys an application after developers push code.

**Topics:**

- GitHub
- GitHub Actions
- Jenkins
- Docker
- ECR
- ECS / EKS
- Automated testing
- Security scanning

---

## 21. Infrastructure as Code CI/CD Pipeline

**Question:**  
Design a secure Terraform deployment pipeline for development, staging, and production environments.

**Topics:**

- Terraform
- GitHub Actions
- Terraform Plan
- Terraform Apply
- Remote State
- Approval gates
- Checkov
- tfsec
- IAM

---

## 22. Multi-Environment Terraform Architecture

**Question:**  
Design a Terraform repository structure for development, staging, and production infrastructure.

**Topics:**

- Terraform Modules
- Remote Backend
- State isolation
- Environment separation
- Variables
- Reusable infrastructure

---

## 23. Docker Container Deployment Architecture

**Question:**  
Design a cloud architecture for deploying Dockerized applications.

**Topics:**

- Docker
- ECR
- ECS
- Fargate
- ALB
- Auto Scaling
- CloudWatch

---

## 24. ECS Microservices Architecture

**Question:**  
Design a production architecture for multiple containerized microservices using Amazon ECS.

**Topics:**

- ECS
- Fargate
- ECR
- ALB
- Service discovery
- Auto Scaling
- CloudWatch
- IAM

---

## 25. Kubernetes / EKS Production Architecture

**Question:**  
Design a highly available Kubernetes architecture using Amazon EKS.

**Topics:**

- EKS
- Worker Nodes
- Multiple Availability Zones
- Ingress
- Services
- Pods
- HPA
- Karpenter
- EKS Pod Identity / IRSA
- ECR

---

## 26. Secure Kubernetes Architecture

**Question:**  
Design a secure Kubernetes environment for running production applications.

**Topics:**

- EKS
- RBAC
- Pod Identity
- Network Policies
- Secrets
- Private nodes
- Security Groups
- Container image scanning

---

## 27. Microservices Architecture

**Question:**  
Design a scalable cloud-native system consisting of multiple independent microservices.

**Topics:**

- API Gateway
- ALB
- ECS / EKS
- Service discovery
- SQS
- EventBridge
- DynamoDB
- Observability

---

## 28. Decoupled Microservices Architecture

**Question:**  
Design microservices so temporary failure of one service does not cause the entire application to fail.

**Topics:**

- SQS
- EventBridge
- SNS
- Retry
- Dead Letter Queue
- Circuit breaking
- Asynchronous communication

---

## 29. Notification System

**Question:**  
Design a system capable of sending email, SMS, and application notifications to large numbers of users.

**Topics:**

- API Gateway
- SQS
- SNS
- Lambda
- SES
- Retry mechanisms
- Dead Letter Queue

---

## 30. Image Processing Pipeline

**Question:**  
Design a scalable system that automatically processes and resizes images uploaded by users.

**Topics:**

- S3
- Lambda
- SQS
- EventBridge
- CloudFront
- DynamoDB

---

## 31. Video Processing Architecture

**Question:**  
Design an asynchronous system for uploading, processing, and delivering large video files.

**Topics:**

- S3
- Presigned URLs
- SQS
- MediaConvert
- Lambda
- CloudFront

---

## 32. Backup Architecture

**Question:**  
Design a centralized backup strategy for AWS infrastructure and data.

**Topics:**

- AWS Backup
- RDS snapshots
- EBS snapshots
- S3 versioning
- Cross-region backups
- Lifecycle policies

---

## 33. Secure Secrets Management Architecture

**Question:**  
Design a system where applications securely retrieve database passwords and API credentials.

**Topics:**

- AWS Secrets Manager
- Parameter Store
- IAM Roles
- KMS
- Secret rotation

---

## 34. Encryption and Key Management Architecture

**Question:**  
Design an encryption strategy for sensitive cloud data.

**Topics:**

- AWS KMS
- Encryption at rest
- Encryption in transit
- TLS
- Key rotation
- IAM key policies

---

## 35. Bastionless Server Management

**Question:**  
Design secure administrative access to private EC2 instances without exposing SSH to the internet.

**Topics:**

- AWS Systems Manager
- Session Manager
- IAM
- Private EC2
- VPC Endpoints
- CloudTrail

---

## 36. API Security Architecture

**Question:**  
Design a secure public API that protects against unauthorized access, abuse, and application-layer attacks.

**Topics:**

- API Gateway
- Cognito
- OAuth
- WAF
- Rate limiting
- IAM
- CloudWatch

---

## 37. DDoS-Resistant Web Architecture

**Question:**  
Design a public application capable of defending against large-scale DDoS and application-layer attacks.

**Topics:**

- CloudFront
- AWS Shield
- AWS WAF
- Route 53
- ALB
- Auto Scaling

---

## 38. Zero-Trust Cloud Architecture

**Question:**  
Design a cloud environment based on Zero Trust principles.

**Topics:**

- IAM
- Least privilege
- Identity-based access
- Network segmentation
- Private resources
- Encryption
- Continuous monitoring

---

## 39. Multi-Account AWS Architecture

**Question:**  
Design an AWS environment for a company with separate development, staging, production, security, and logging accounts.

**Topics:**

- AWS Organizations
- Organizational Units
- Service Control Policies
- IAM Identity Center
- Centralized logging
- Security account
- Shared services

---

## 40. Centralized Security Account Architecture

**Question:**  
Design an AWS multi-account security architecture where security logs and findings are managed centrally.

**Topics:**

- AWS Organizations
- Security Hub
- GuardDuty
- CloudTrail
- AWS Config
- Log Archive Account
- Security Account

---

## 41. Hybrid Cloud Connectivity

**Question:**  
Design secure connectivity between an on-premises data center and AWS.

**Topics:**

- Site-to-Site VPN
- Direct Connect
- Transit Gateway
- Route Tables
- BGP
- Redundant connections

---

## 42. Hub-and-Spoke Network Architecture

**Question:**  
Design networking for dozens of VPCs that need centralized connectivity.

**Topics:**

- Transit Gateway
- Multiple VPCs
- Shared Services
- Inspection VPC
- Route Tables
- Network Firewall

---

## 43. Centralized Egress Architecture

**Question:**  
Design a network where multiple VPCs send outbound internet traffic through centralized security inspection.

**Topics:**

- Transit Gateway
- Inspection VPC
- NAT Gateway
- AWS Network Firewall
- Route Tables

---

## 44. Data Processing Pipeline

**Question:**  
Design a cloud pipeline that receives large amounts of raw data, processes it, and stores the results for analysis.

**Topics:**

- S3
- Kinesis
- Lambda
- Glue
- Athena
- Redshift

---

## 45. Real-Time Streaming Architecture

**Question:**  
Design a system capable of processing millions of real-time events.

**Topics:**

- Kinesis
- Lambda
- DynamoDB
- S3
- CloudWatch
- Event-driven architecture

---

## 46. Cloud Data Lake Architecture

**Question:**  
Design a secure and scalable data lake.

**Topics:**

- S3
- Glue
- Athena
- Lake Formation
- IAM
- KMS
- Lifecycle policies

---

## 47. Serverless Event-Driven E-Commerce Backend

**Question:**  
Design a serverless backend for an e-commerce system handling users, orders, payments, and notifications.

**Topics:**

- API Gateway
- Lambda
- DynamoDB
- SQS
- SNS
- EventBridge
- Step Functions

---

## 48. Order Processing System

**Question:**  
Design an order processing workflow that safely handles failures between payment, inventory, and shipping services.

**Topics:**

- Step Functions
- SQS
- Lambda
- EventBridge
- DynamoDB
- Retry
- Compensation logic

---

## 49. Cloud Cost Optimization Architecture

**Question:**  
Design an AWS environment that automatically identifies and reduces unnecessary infrastructure costs.

**Topics:**

- Cost Explorer
- AWS Budgets
- Compute Optimizer
- CloudWatch
- Auto Scaling
- S3 Lifecycle
- Savings Plans

---

## 50. Production Cloud Platform

**Question:**  
Design a complete production cloud platform for a company starting from scratch.

The design should include:

- AWS accounts
- VPC architecture
- Public/private networking
- DNS
- Load balancing
- Compute
- Containers
- Databases
- IAM
- Secrets
- Encryption
- Logging
- Monitoring
- Security
- CI/CD
- Terraform
- Backup
- Disaster recovery
- Cost controls

This will be the final comprehensive system design challenge.

---

# Design Method

For every challenge, I follow the same architecture process:

1. Understand the requirements
2. Identify users and traffic flow
3. Design networking
4. Select compute services
5. Select storage and databases
6. Design scalability
7. Design high availability
8. Apply security controls
9. Add monitoring and logging
10. Design backup and disaster recovery
11. Consider cost optimization
12. Identify possible failure scenarios

---

# Repository Structure

Example:

```text
cloud-engineering-system-design/
├── README.md
├── 01-highly-available-web-app/
│   ├── architecture.drawio
│   ├── architecture.png
│   └── README.md
├── 02-secure-three-tier-app/
│   ├── architecture.drawio
│   ├── architecture.png
│   └── README.md
├── 03-static-website/
│   ├── architecture.drawio
│   ├── architecture.png
│   └── README.md
├── 04-serverless-api/
│   ├── architecture.drawio
│   ├── architecture.png
│   └── README.md
│
├── ...
│
└── 50-production-cloud-platform/
    ├── architecture.drawio
    ├── architecture.png
    └── README.md
```

---

# What Each Solution Contains

Each architecture will contain:

## Problem

The system design question and requirements.

## Requirements

Functional and infrastructure requirements.

## Architecture Diagram

Architecture created using **draw.io**.

## Request Flow

How traffic and data move through the system.

## AWS Services

Explanation of each AWS service used.

## Security

IAM, encryption, networking, secrets, and security controls.

## High Availability

How the architecture handles infrastructure failure.

## Scalability

How the architecture handles increased traffic.

## Monitoring

Metrics, logs, alarms, and observability.

## Disaster Recovery

Backup, recovery, RTO, and RPO considerations.

## Cost Optimization

Ways to reduce unnecessary cloud spending.

## Failure Scenarios

What happens when individual components fail.

---

# Goal

The purpose of this repository is not only to memorize AWS services.

The goal is to develop the ability to answer:

> **“Why was this architecture designed this way?”**

A Cloud Engineer should be able to understand the requirements, select appropriate cloud services, identify failure points, secure the architecture, and explain the trade-offs behind every design decision.

---

# Tools

- AWS
- draw.io
- Terraform
- Docker
- Kubernetes
- GitHub Actions
- Jenkins

---

# Progress

- [ ] 01 — Highly Available Web Application
- [ ] 02 — Secure Three-Tier Web Architecture
- [ ] 03 — Global Static Website
- [ ] 04 — Serverless REST API
- [ ] 05 — Large File Upload System
- [ ] 06 — Event-Driven File Processing
- [ ] 07 — Asynchronous Job Processing
- [ ] 08 — Highly Scalable Web Application
- [ ] 09 — Highly Available Database
- [ ] 10 — Read-Heavy Database Architecture
- [ ] 11 — Multi-Region Application
- [ ] 12 — Disaster Recovery Architecture
- [ ] 13 — Secure Private Application
- [ ] 14 — VPC Internet Access
- [ ] 15 — Private AWS Service Access
- [ ] 16 — Secure IAM Architecture
- [ ] 17 — Centralized Logging
- [ ] 18 — Monitoring and Alerting
- [ ] 19 — Cloud Security Monitoring
- [ ] 20 — Application CI/CD
- [ ] 21 — Terraform CI/CD
- [ ] 22 — Multi-Environment Terraform
- [ ] 23 — Docker Deployment
- [ ] 24 — ECS Microservices
- [ ] 25 — EKS Production Architecture
- [ ] 26 — Secure Kubernetes
- [ ] 27 — Microservices Architecture
- [ ] 28 — Decoupled Microservices
- [ ] 29 — Notification System
- [ ] 30 — Image Processing
- [ ] 31 — Video Processing
- [ ] 32 — Backup Architecture
- [ ] 33 — Secrets Management
- [ ] 34 — Encryption and Key Management
- [ ] 35 — Bastionless Server Management
- [ ] 36 — API Security
- [ ] 37 — DDoS-Resistant Architecture
- [ ] 38 — Zero-Trust Architecture
- [ ] 39 — Multi-Account AWS Architecture
- [ ] 40 — Centralized Security Account
- [ ] 41 — Hybrid Cloud Connectivity
- [ ] 42 — Hub-and-Spoke Networking
- [ ] 43 — Centralized Egress
- [ ] 44 — Data Processing Pipeline
- [ ] 45 — Real-Time Streaming
- [ ] 46 — Cloud Data Lake
- [ ] 47 — Serverless E-Commerce Backend
- [ ] 48 — Order Processing System
- [ ] 49 — Cloud Cost Optimization
- [ ] 50 — Production Cloud Platform
