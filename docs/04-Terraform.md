# Terraform

> *"Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp that enables you to define, provision, and manage cloud infrastructure using declarative configuration files."*

---

# 📖 Introduction

Modern cloud infrastructure consists of hundreds or even thousands of resources such as virtual networks, servers, databases, storage buckets, security groups, load balancers, and Kubernetes clusters.

Manually creating these resources through a cloud provider's web console is time-consuming, error-prone, and difficult to reproduce.

Infrastructure as Code (IaC) solves this problem by allowing infrastructure to be defined in code. Instead of manually provisioning resources, engineers write configuration files that describe the desired infrastructure. Terraform then automatically creates and manages those resources.

Throughout this project, Terraform is responsible for provisioning the entire AWS infrastructure, including the Virtual Private Cloud (VPC), networking components, Bastion Host, Amazon EKS cluster, IAM roles, and many other AWS resources.

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What Terraform is
- Infrastructure as Code (IaC)
- Why Terraform is used
- Terraform Architecture
- Terraform Workflow
- Terraform Configuration Language (HCL)
- Terraform Providers
- Resources
- Variables
- Outputs
- State Files
- Remote Backend
- Terraform Modules
- Terraform Best Practices

---

# 🏗 What is Infrastructure as Code (IaC)?

Infrastructure as Code (IaC) is the practice of managing infrastructure using code instead of manually configuring resources through graphical interfaces.

Instead of clicking buttons inside the AWS Console, we describe the infrastructure in configuration files.

Example:

Instead of manually creating:

- VPC
- Subnets
- Internet Gateway
- Route Tables
- NAT Gateway
- EKS Cluster

Terraform creates everything automatically.

---

# Traditional Infrastructure

```text
Login AWS Console
        │
Create VPC
        │
Create Subnets
        │
Create Route Tables
        │
Create Security Groups
        │
Launch EC2
        │
Create EKS
        │
Deploy Application
```

---

# Infrastructure as Code

```text
Write Terraform Code
        │
terraform init
        │
terraform plan
        │
terraform apply
        │
AWS Infrastructure Created
```

---

# 💡 Why Terraform?

Terraform provides several benefits:

- Infrastructure Automation
- Version Control
- Repeatability
- Team Collaboration
- Multi-Cloud Support
- Resource Dependency Management
- Easy Scaling
- Cost Efficiency
- Consistent Deployments

---

# 🌍 Why Terraform for This Project?

This DevOps project requires a large number of AWS resources.

Terraform automates the provisioning of:

- VPC
- Public Subnets
- Private Subnets
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Bastion Host
- Amazon EKS
- Node Groups
- IAM Roles
- EBS Resources
- S3 Backend

Without Terraform, creating these resources manually would take hours and increase the risk of configuration errors.

---

# ⚙ Terraform Architecture

Terraform interacts with cloud providers through **Providers**.

```text
Terraform Configuration
          │
          ▼
Terraform CLI
          │
          ▼
AWS Provider
          │
          ▼
AWS API
          │
          ▼
AWS Resources
```

Terraform itself does not create infrastructure directly. It communicates with the AWS API using the AWS Provider.

---

# 📝 Terraform Workflow

Terraform follows a simple workflow.

```text
Write Code
      │
      ▼
terraform init
      │
      ▼
terraform validate
      │
      ▼
terraform plan
      │
      ▼
terraform apply
      │
      ▼
Infrastructure Created
```

---

# 📂 Terraform Configuration Files

Terraform projects commonly include the following files:

| File | Purpose |
|------|----------|
| main.tf | Main infrastructure definitions |
| variables.tf | Input variables |
| outputs.tf | Output values |
| provider.tf | Provider configuration |
| versions.tf | Terraform version requirements |
| backend.tf | Remote backend configuration |
| terraform.tfvars | Variable values |

---

# 🌐 HashiCorp Configuration Language (HCL)

Terraform uses **HashiCorp Configuration Language (HCL)**.

Example:

```hcl
resource "aws_s3_bucket" "terraform_state" {
  bucket = "my-terraform-state"

  tags = {
    Name = "Terraform Backend"
  }
}
```

HCL is designed to be:

- Human readable
- Easy to learn
- Declarative
- Reusable

---

# 🔌 Providers

Providers allow Terraform to communicate with external platforms.

Popular providers include:

- AWS
- Azure
- Google Cloud
- Kubernetes
- Docker
- GitHub

AWS Provider Example:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

---

# 📦 Resources

Resources represent infrastructure components.

Examples:

- aws_vpc
- aws_subnet
- aws_instance
- aws_eks_cluster
- aws_security_group
- aws_route_table

Example:

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}
```

---

# 📥 Variables

Variables make Terraform reusable.

Instead of hardcoding values:

```hcl
variable "region" {
  default = "ap-south-1"
}
```

Use:

```hcl
provider "aws" {
  region = var.region
}
```

Benefits:

- Reusability
- Cleaner code
- Easier maintenance

---

# 📤 Outputs

Outputs display useful information after deployment.

Example:

```hcl
output "vpc_id" {
  value = aws_vpc.main.id
}
```

Common outputs:

- VPC ID
- Cluster Endpoint
- Load Balancer DNS
- Bastion Host IP

---

# 🗂 Terraform State

Terraform stores infrastructure information in a **State File**.

```
terraform.tfstate
```

The state file contains:

- Resource IDs
- Metadata
- Dependencies
- Current infrastructure state

Terraform compares the desired configuration with the current state to determine what changes need to be made.

---

# 🔄 Why Terraform State is Important

Without the state file, Terraform would not know:

- Which resources already exist
- Which resources need updating
- Which resources should be destroyed

The state file is the source of truth for your infrastructure.

---

# ☁ Remote Backend

In production environments, the state file should **never** be stored locally.

Instead, it is stored remotely.

In this project:

```text
Terraform
      │
      ▼
S3 Bucket
      │
Stores
terraform.tfstate
```

Benefits:

- Team Collaboration
- Version History
- Secure Storage
- Centralized Management

---

# 🔒 State Locking

When multiple engineers work on the same infrastructure, Terraform uses state locking to prevent conflicts.

Benefits:

- Prevents corruption
- Prevents simultaneous changes
- Ensures consistency

---

# 📚 Terraform Modules

Modules help organize Terraform code.

Example structure:

```text
terraform/

├── modules/
│     ├── vpc/
│     ├── eks/
│     ├── bastion/
│     ├── iam/
│     └── security-groups/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── backend.tf
└── provider.tf
```

Modules improve:

- Reusability
- Maintainability
- Scalability

---

# 🔄 Terraform Lifecycle

```text
Configuration

        │

terraform init

        │

terraform validate

        │

terraform plan

        │

terraform apply

        │

AWS Infrastructure

        │

terraform destroy
```

---

# ⚡ Important Terraform Commands

## Initialize

```bash
terraform init
```

Downloads providers and initializes the project.

---

## Validate

```bash
terraform validate
```

Checks configuration syntax.

---

## Format

```bash
terraform fmt
```

Formats Terraform files.

---

## Plan

```bash
terraform plan
```

Shows what changes Terraform will make.

---

## Apply

```bash
terraform apply
```

Creates or updates infrastructure.

---

## Destroy

```bash
terraform destroy
```

Deletes all managed resources.

---

# 🏗 Infrastructure Created in This Project

Terraform provisions the following AWS resources:

```text
AWS

├── VPC
├── Public Subnets
├── Private Subnets
├── Internet Gateway
├── NAT Gateway
├── Route Tables
├── Security Groups
├── Bastion Host
├── Amazon EKS
├── Node Groups
├── IAM Roles
├── S3 Backend
└── EBS Resources
```

Everything is created automatically from code.

---

# 🌟 Benefits of Terraform

- Infrastructure as Code
- Automated Provisioning
- Version Control
- Repeatable Deployments
- Easy Collaboration
- Resource Dependency Management
- Multi-Cloud Support
- Scalable Infrastructure

---

# ⚠ Terraform Best Practices

- Use Remote State
- Never commit `terraform.tfstate` to Git
- Use Modules
- Use Variables
- Keep configurations small and reusable
- Store secrets securely
- Use version control
- Run `terraform fmt`
- Review `terraform plan` before applying
- Tag AWS resources consistently

---

# 📝 Summary

Terraform is the backbone of modern Infrastructure as Code. It enables engineers to define cloud infrastructure using simple, reusable configuration files, making deployments predictable, repeatable, and scalable.

In this project, Terraform provisions the complete AWS environment, allowing us to focus on deploying and managing applications rather than manually configuring infrastructure.

In the next chapter, we will explore **AWS Virtual Private Cloud (VPC)**, the networking foundation upon which the entire DevOps platform is built.

---

# 🎯 Key Takeaways

- Terraform is an Infrastructure as Code tool.
- Infrastructure is managed through code rather than manual configuration.
- Terraform uses Providers to interact with cloud platforms.
- Resources represent infrastructure components.
- State files track deployed resources.
- Remote backends enable team collaboration.
- Modules improve code organization and reusability.
- Terraform is essential for building scalable, production-ready cloud infrastructure.

---

## 📚 Next Chapter

➡️ **05-VPC-Networking.md**