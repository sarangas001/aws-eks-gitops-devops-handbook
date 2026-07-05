# AWS VPC & Networking

> *"Amazon Virtual Private Cloud (VPC) is the networking foundation of AWS that enables you to build secure, scalable, and isolated cloud environments. Every production application deployed on AWS begins with a well-designed network architecture."*

---

# 📖 Introduction

Networking is one of the most critical aspects of cloud infrastructure. Regardless of how powerful an application is, it cannot function properly without a secure and reliable network.

Before deploying Kubernetes, databases, or applications, cloud engineers must first design the network that connects these components.

In AWS, this network is called a **Virtual Private Cloud (VPC)**.

Throughout this project, Terraform provisions a complete networking environment including:

- Amazon VPC
- Public Subnets
- Private Subnets
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Bastion Host
- Elastic IP
- Amazon EKS Networking

This networking architecture ensures that the Kubernetes cluster remains private while still allowing applications to be securely accessible from the internet.

---

# 🎯 Learning Objectives

By the end of this chapter, you will understand:

- What a Virtual Private Cloud (VPC) is
- Public vs Private Networks
- CIDR Blocks
- Subnets
- Route Tables
- Internet Gateway
- NAT Gateway
- Elastic IP
- Security Groups
- Network ACLs
- Bastion Host
- DNS Resolution
- VPC Architecture
- Networking Design for Amazon EKS
- Networking Best Practices

---

# 🌐 What is a Virtual Private Cloud (VPC)?

A **Virtual Private Cloud (VPC)** is a logically isolated virtual network inside AWS where you launch and manage cloud resources.

Think of a VPC as your own private data center inside AWS.

Within a VPC, you control:

- IP Address Range
- Network Segmentation
- Internet Access
- Security
- Routing
- DNS
- Resource Communication

---

# 🏗 Why Do We Need a VPC?

Imagine deploying an application directly onto the internet.

Problems would include:

- No network isolation
- Poor security
- IP conflicts
- Difficult access control
- No private communication

A VPC solves these issues by providing an isolated networking environment.

Benefits include:

- Security
- High Availability
- Scalability
- Network Isolation
- Controlled Internet Access
- Private Communication

---

# 🌍 VPC Architecture

```text
                     AWS Region
                         │
                 ┌──────────────┐
                 │     VPC      │
                 │10.0.0.0/16   │
                 └──────────────┘
                      │
        ┌─────────────┴─────────────┐
        │                           │
 Public Subnet               Private Subnet
      │                            │
 Bastion Host                 EKS Worker Nodes
 Internet Gateway             Kubernetes Pods
      │                            │
      └────────── NAT Gateway ─────┘
```

---

# 📦 CIDR Blocks

Every VPC requires an IP address range.

Example:

```text
10.0.0.0/16
```

CIDR stands for:

**Classless Inter-Domain Routing**

CIDR defines how many IP addresses are available.

Examples:

| CIDR | Available IPs |
|------|---------------|
| /16 | 65,536 |
| /24 | 256 |
| /28 | 16 |

Example VPC:

```
10.0.0.0/16
```

Possible subnet allocation:

```
10.0.1.0/24
10.0.2.0/24
10.0.3.0/24
10.0.4.0/24
```

---

# 🏢 Subnets

A subnet is a subdivision of a VPC.

Each subnet belongs to a single Availability Zone.

Subnets help organize infrastructure and improve security.

There are two primary subnet types.

---

# 🌐 Public Subnet

A Public Subnet has a route to the Internet Gateway.

Resources inside a public subnet can communicate directly with the internet.

Examples:

- Bastion Host
- NAT Gateway
- Public Load Balancer

Architecture:

```text
Internet
     │
Internet Gateway
     │
Public Route Table
     │
Public Subnet
     │
EC2 Bastion Host
```

---

# 🔒 Private Subnet

A Private Subnet does **not** have direct internet access.

Resources in private subnets are protected from inbound internet traffic.

Examples:

- Kubernetes Worker Nodes
- Databases
- Internal APIs
- Backend Services

Architecture:

```text
Private Route Table
        │
Private Subnet
        │
EKS Nodes
        │
Pods
```

---

# 🌍 Availability Zones

Production systems should never rely on a single Availability Zone.

Example:

```text
Region (ap-south-1)

├── AZ-A
│      ├── Public Subnet
│      └── Private Subnet
│
├── AZ-B
│      ├── Public Subnet
│      └── Private Subnet
│
└── AZ-C
       ├── Public Subnet
       └── Private Subnet
```

Benefits:

- High Availability
- Fault Tolerance
- Disaster Recovery

---

# 🌐 Internet Gateway (IGW)

An **Internet Gateway (IGW)** enables communication between the VPC and the internet.

Without an Internet Gateway:

- No internet access
- No public websites
- No SSH from outside
- No external traffic

Architecture:

```text
Internet
      │
Internet Gateway
      │
Public Route Table
      │
Public Subnet
```

---

# 🚪 NAT Gateway

Private resources often need outbound internet access for:

- Downloading packages
- Pulling Docker images
- Installing updates

However, they should **not** accept inbound internet traffic.

This is the purpose of the NAT Gateway.

```text
Private Subnet
       │
Route Table
       │
NAT Gateway
       │
Internet Gateway
       │
Internet
```

The NAT Gateway allows **outbound-only** internet communication.

---

# 🌍 Route Tables

A Route Table determines where network traffic is sent.

Example Public Route Table:

| Destination | Target |
|-------------|---------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway |

Private Route Table:

| Destination | Target |
|-------------|---------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | NAT Gateway |

---

# 🔒 Security Groups

Security Groups act as virtual firewalls.

They control:

- Incoming Traffic (Inbound Rules)
- Outgoing Traffic (Outbound Rules)

Example:

```
SSH (22)

↓

Allowed

↓

Bastion Host
```

Application Example:

```
HTTPS (443)

↓

Application Load Balancer

↓

Gateway API
```

---

# 🚧 Network ACL (NACL)

Network ACLs provide subnet-level security.

Difference:

| Security Group | Network ACL |
|---------------|-------------|
| Instance Level | Subnet Level |
| Stateful | Stateless |
| Allow Rules | Allow & Deny Rules |

Most production environments rely primarily on Security Groups while using NACLs for additional protection.

---

# 🖥 Bastion Host

Our Kubernetes cluster is private.

This means it cannot be accessed directly from the internet.

Instead, administrators first connect to a Bastion Host.

Architecture:

```text
Developer

      │

SSH

      │

Bastion Host

      │

Private Network

      │

Amazon EKS
```

The Bastion Host acts as a secure entry point for cluster administration.

---

# 🌐 Elastic IP (EIP)

A Bastion Host requires a static public IP address.

AWS provides this through an **Elastic IP (EIP)**.

Benefits:

- Static Address
- Easy Firewall Configuration
- Reliable SSH Access

---

# 🔑 DNS Resolution

AWS provides internal DNS resolution inside every VPC.

Resources can communicate using:

- Private IP Addresses
- Internal DNS Names

This simplifies communication between services.

---

# ☸ Networking for Amazon EKS

Our EKS architecture uses:

```text
Internet

      │

Application Load Balancer

      │

Gateway API

      │

Kubernetes Services

      │

Pods

      │

Worker Nodes

      │

Private Subnets
```

Important points:

- Control Plane is Managed by AWS
- Worker Nodes remain in Private Subnets
- Applications are exposed through the Load Balancer
- Users never directly access worker nodes

---

# 🏗 Complete Project Network Architecture

```text
                         Internet
                              │
                       Internet Gateway
                              │
                ┌─────────────┴─────────────┐
                │                           │
        Public Route Table          Private Route Table
                │                           │
       Public Subnets              Private Subnets
                │                           │
      Bastion Host (SSH)        Amazon EKS Worker Nodes
                │                           │
           NAT Gateway                Kubernetes Pods
                │                           │
         Outbound Internet         Internal Communication
```

---

# 🚀 Why Private EKS?

Our project intentionally deploys Kubernetes nodes inside private subnets.

Advantages:

- Improved Security
- Reduced Attack Surface
- No Public Worker Nodes
- Controlled Access
- Better Compliance

Developers connect through the Bastion Host instead of directly accessing the cluster.

---

# 📋 Best Practices

- Use multiple Availability Zones
- Keep worker nodes private
- Use Bastion Host for administration
- Restrict Security Group rules
- Avoid exposing databases publicly
- Use NAT Gateway for outbound traffic
- Enable VPC Flow Logs
- Apply least privilege networking
- Use Infrastructure as Code (Terraform)

---

# ⚠ Common Mistakes

❌ Placing databases in public subnets

❌ Allowing SSH from anywhere (`0.0.0.0/0`)

❌ Using a single Availability Zone

❌ Hardcoding IP addresses

❌ Creating overly permissive Security Groups

❌ Forgetting Route Table associations

❌ Exposing Kubernetes worker nodes publicly

---

# 💼 Real-World Example

In this project:

1. Terraform creates the VPC.
2. Public and Private Subnets are provisioned across multiple Availability Zones.
3. An Internet Gateway provides internet connectivity for public resources.
4. A NAT Gateway enables private resources to access the internet securely.
5. A Bastion Host is deployed in the public subnet for secure administrative access.
6. Amazon EKS worker nodes are launched in private subnets.
7. Applications are exposed through an Application Load Balancer using the Gateway API.
8. Security Groups restrict traffic to only the required ports.

This architecture follows production best practices and provides a secure foundation for cloud-native applications.

---

# 📝 Summary

Networking is the backbone of every cloud environment. A properly designed VPC ensures that applications are secure, highly available, and scalable.

In this project, the VPC architecture isolates critical resources within private subnets while exposing only the necessary services to the internet through managed AWS networking components. This design minimizes security risks and supports reliable application deployment.

Understanding AWS networking is essential before provisioning infrastructure with Terraform and deploying workloads to Amazon EKS.

---

# 🎯 Key Takeaways

- A VPC is a private virtual network inside AWS.
- Public Subnets have direct internet access.
- Private Subnets are isolated from inbound internet traffic.
- Internet Gateways connect public resources to the internet.
- NAT Gateways provide secure outbound internet access for private resources.
- Route Tables determine how network traffic flows.
- Security Groups act as virtual firewalls.
- Bastion Hosts provide secure administrative access to private infrastructure.
- Production workloads should run in private subnets across multiple Availability Zones.

---

# 📚 Next Chapter

➡️ **06-Bastion-Host.md**