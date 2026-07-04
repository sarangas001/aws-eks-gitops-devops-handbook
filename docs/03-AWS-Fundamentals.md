# Amazon Web Services (AWS) Fundamentals

> *"Amazon Web Services (AWS) is the world's leading cloud computing platform, providing scalable, secure, and highly available infrastructure and services that enable organizations to build and deploy applications without managing physical hardware."*

---

# 📖 Introduction

Cloud computing has transformed how modern applications are developed, deployed, and managed. Instead of purchasing and maintaining physical servers, organizations can provision computing resources on demand within minutes.

Amazon Web Services (AWS), launched by Amazon in 2006, is the world's largest and most widely adopted cloud platform. It offers more than **200 fully featured services**, including computing, networking, storage, databases, machine learning, analytics, security, and DevOps tools.

Throughout this handbook, AWS serves as the cloud platform where our complete production-ready DevOps infrastructure will be built.

---

# 🎯 Learning Objectives

By the end of this chapter, you will understand:

- What AWS is
- Why AWS is widely used
- Cloud Computing Fundamentals
- AWS Global Infrastructure
- AWS Regions
- Availability Zones
- Edge Locations
- AWS Services
- AWS Pricing Models
- AWS Shared Responsibility Model
- AWS IAM
- AWS Networking Basics
- AWS Services used in this project
- AWS Best Practices

---

# ☁ What is Cloud Computing?

Cloud Computing is the delivery of computing services such as servers, storage, databases, networking, and software over the internet instead of using physical infrastructure.

Instead of buying hardware, organizations rent resources only when needed.

### Traditional Infrastructure

```text
Company
   │
Purchase Servers
   │
Configure Hardware
   │
Maintain Infrastructure
   │
Deploy Application
```

### Cloud Infrastructure

```text
Developer
      │
Create Resources
      │
Deploy Application
      │
Scale Automatically
```

Cloud computing significantly reduces operational costs while increasing flexibility and scalability.

---

# 🌍 Why AWS?

AWS is trusted by startups, enterprises, governments, and Fortune 500 companies because it provides:

- High Availability
- Global Infrastructure
- Scalability
- Security
- Reliability
- Automation
- Cost Optimization
- Managed Services
- Continuous Innovation

Companies using AWS include:

- Netflix
- Airbnb
- Spotify
- Adobe
- NASA
- Samsung
- Coca-Cola
- BMW
- LinkedIn

---

# 🌎 AWS Global Infrastructure

AWS operates data centers across the globe.

Its infrastructure consists of:

- Regions
- Availability Zones (AZs)
- Edge Locations
- Local Zones
- Wavelength Zones

These components work together to provide highly available and low-latency cloud services.

---

# 📍 AWS Regions

A Region is a physical geographic area where AWS operates multiple data centers.

Examples:

| Region | Code |
|---------|------|
| N. Virginia | us-east-1 |
| Ohio | us-east-2 |
| Oregon | us-west-2 |
| Mumbai | ap-south-1 |
| Singapore | ap-southeast-1 |
| Tokyo | ap-northeast-1 |
| Frankfurt | eu-central-1 |
| London | eu-west-2 |

### Why Regions Matter

- Data Residency
- Lower Latency
- Disaster Recovery
- Compliance
- Cost Optimization

---

# 🏢 Availability Zones (AZs)

Each AWS Region contains multiple Availability Zones.

An Availability Zone is one or more physically separate data centers with independent:

- Power
- Cooling
- Networking
- Security

Example:

```text
AWS Region (ap-south-1)

├── AZ A
├── AZ B
└── AZ C
```

Deploying applications across multiple AZs improves fault tolerance.

---

# 🌐 Edge Locations

Edge Locations are AWS data centers used by services such as:

- CloudFront
- Route53
- AWS Shield

They reduce latency by serving content closer to users.

---

# ☁ Cloud Service Models

AWS supports three major cloud service models.

## Infrastructure as a Service (IaaS)

Provides virtual infrastructure.

Examples:

- EC2
- VPC
- EBS

---

## Platform as a Service (PaaS)

Provides a platform for application deployment.

Examples:

- Elastic Beanstalk
- Amazon EKS
- Amazon ECS

---

## Software as a Service (SaaS)

Provides complete software applications.

Examples:

- Gmail
- Microsoft 365
- Salesforce

---

# 📦 AWS Core Service Categories

AWS provides services across multiple categories.

| Category | Examples |
|-----------|----------|
| Compute | EC2, Lambda, ECS, EKS |
| Storage | S3, EBS, EFS |
| Database | RDS, DynamoDB |
| Networking | VPC, Route53 |
| Security | IAM, KMS, Secrets Manager |
| Monitoring | CloudWatch |
| Analytics | Athena |
| Containers | EKS, ECS |
| DevOps | CodeBuild, CodePipeline |

---

# 🔑 Identity and Access Management (IAM)

IAM controls who can access AWS resources.

It enables secure authentication and authorization.

Components include:

- Users
- Groups
- Roles
- Policies

Example:

```text
IAM User
      │
IAM Policy
      │
Access AWS Resources
```

Following the **Principle of Least Privilege** ensures users receive only the permissions they require.

---

# 🌐 Amazon VPC

Amazon Virtual Private Cloud (VPC) provides an isolated network inside AWS.

It allows you to control:

- IP Address Range
- Subnets
- Routing
- Firewalls
- Internet Access

Example:

```text
AWS Account
      │
      ▼
    VPC
 ├─────────────┐
 │             │
Public      Private
Subnet      Subnet
```

The VPC is the foundation of our DevOps infrastructure.

---

# 💻 Amazon EC2

Amazon Elastic Compute Cloud (EC2) provides virtual servers in the cloud.

Features:

- On-demand instances
- Auto Scaling
- Security Groups
- Multiple Operating Systems
- Flexible Instance Types

In this project, EC2 is used for the Bastion Host and EKS worker nodes.

---

# 💾 Amazon S3

Amazon Simple Storage Service (S3) provides highly durable object storage.

Use cases:

- File Storage
- Static Website Hosting
- Backups
- Terraform Remote Backend
- Logs

In our project, Terraform stores its remote state file in an S3 bucket.

---

# 🌐 Route 53

Amazon Route53 is AWS's managed DNS service.

Responsibilities:

- Domain Registration
- DNS Records
- Health Checks
- Traffic Routing

In this project, Route53 manages domain names for the application, ArgoCD, Grafana, and Kibana.

---

# 🔒 AWS Certificate Manager (ACM)

ACM provides SSL/TLS certificates.

Benefits:

- Free SSL Certificates
- Automatic Renewal
- Secure HTTPS

Our Application Load Balancer uses ACM certificates to terminate HTTPS traffic.

---

# ☸ Amazon Elastic Kubernetes Service (EKS)

Amazon EKS is a fully managed Kubernetes service.

AWS manages the Kubernetes control plane while users manage worker nodes and applications.

Benefits:

- High Availability
- Managed Control Plane
- Kubernetes Compatibility
- Automatic Updates
- Integrated AWS Security

This handbook focuses heavily on deploying applications to EKS.

---

# 📊 Amazon CloudWatch

CloudWatch is AWS's native monitoring service.

Capabilities:

- Metrics
- Logs
- Dashboards
- Alarms
- Events

Although we primarily use Prometheus and Grafana, CloudWatch remains important for AWS resource monitoring.

---

# 🛡 AWS Shared Responsibility Model

AWS security follows a shared responsibility model.

```text
AWS
│
├── Physical Security
├── Data Centers
├── Networking
├── Hardware
└── Managed Services

Customer
│
├── IAM
├── Applications
├── Data
├── Operating Systems
├── Security Groups
└── Configurations
```

AWS secures the cloud, while customers secure what they build inside the cloud.

---

# 💰 AWS Pricing Models

AWS offers several pricing options.

## On-Demand

Pay only for what you use.

Best for:

- Development
- Testing
- Short-term workloads

---

## Reserved Instances

Lower pricing in exchange for long-term commitments.

---

## Spot Instances

Use unused AWS capacity at significantly reduced prices.

Ideal for fault-tolerant workloads.

---

## Savings Plans

Flexible pricing discounts based on committed usage.

---

# 🏗 AWS Architecture Used in This Project

The project architecture includes the following AWS services:

```text
AWS
│
├── IAM
├── VPC
│   ├── Public Subnets
│   ├── Private Subnets
│   ├── Internet Gateway
│   ├── NAT Gateway
│   └── Route Tables
│
├── EC2 Bastion Host
├── Amazon EKS
├── Worker Nodes
├── Amazon S3
├── Route53
├── ACM
├── Application Load Balancer
└── Elastic Block Store (EBS)
```

---

# 🎯 AWS Services Used Throughout This Handbook

| Service | Purpose |
|----------|---------|
| IAM | Authentication & Authorization |
| EC2 | Bastion Host & Worker Nodes |
| EKS | Kubernetes Cluster |
| VPC | Networking |
| S3 | Terraform Remote State |
| Route53 | DNS Management |
| ACM | SSL Certificates |
| ALB | External Traffic |
| EBS | Persistent Storage |
| CloudWatch | AWS Monitoring |

---

# 🌟 Advantages of AWS

- Global Infrastructure
- High Availability
- Elastic Scalability
- Strong Security
- Managed Services
- Extensive Documentation
- Massive Community
- Cost Optimization
- Automation Support

---

# ⚠ AWS Best Practices

- Use IAM Roles instead of Access Keys
- Enable Multi-Factor Authentication (MFA)
- Follow Least Privilege
- Store Terraform State Remotely
- Use Multiple Availability Zones
- Enable Encryption
- Monitor Resources
- Tag Resources Properly
- Avoid Using the Root Account
- Regularly Review Security Policies

---

# 📝 Summary

Amazon Web Services provides the foundation for modern cloud-native applications. Throughout this handbook, AWS enables us to build a secure, scalable, and highly available DevOps platform using managed services and Infrastructure as Code.

Understanding AWS fundamentals—including Regions, Availability Zones, networking, IAM, and core services—is essential before exploring Terraform, VPC design, and Kubernetes deployment.

In the next chapter, we will dive into **Terraform**, the Infrastructure as Code (IaC) tool that provisions and manages all AWS resources used in this project.

---

# 🎯 Key Takeaways

- AWS is the world's leading cloud computing platform.
- Cloud computing eliminates the need to manage physical hardware.
- Regions and Availability Zones provide high availability and resilience.
- IAM secures access to AWS resources.
- VPC forms the network foundation for cloud applications.
- Services like S3, EKS, Route53, and ACM are essential building blocks of modern DevOps architectures.
- AWS follows a Shared Responsibility Model for security.

---

## 📚 Next Chapter

➡️ **04-Terraform.md**