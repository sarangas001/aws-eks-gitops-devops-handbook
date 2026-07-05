# Bastion Host

> *"A Bastion Host is a secure entry point that allows administrators to access private cloud resources without exposing the entire infrastructure to the internet."*

---

# 📖 Introduction

One of the fundamental principles of cloud security is **minimizing the attack surface**. Production environments should never expose critical infrastructure directly to the public internet.

In this project, our **Amazon EKS worker nodes** are deployed inside **private subnets**, meaning they cannot be accessed directly via SSH or the internet. However, DevOps engineers still need a secure way to administer the Kubernetes cluster.

This is where the **Bastion Host** comes in.

A Bastion Host is a hardened EC2 instance deployed in a **public subnet** that acts as the single secure gateway into the private network.

Instead of opening SSH access to every server, administrators first connect to the Bastion Host and then securely access private resources from within the VPC.

---

# 🎯 Learning Objectives

After completing this chapter, you will understand:

- What a Bastion Host is
- Why Bastion Hosts are used
- How Bastion Hosts improve security
- Bastion Host architecture
- Bastion Host networking
- SSH connectivity
- Security Groups
- Key Pair authentication
- Bastion Host best practices
- Bastion Host implementation in this project

---

# 🤔 What is a Bastion Host?

A **Bastion Host** is a dedicated server that provides secure administrative access to resources located inside a private network.

Think of it as the **front gate** of a secure building.

Instead of allowing everyone to enter through multiple doors, there is only one monitored entrance.

```
Internet

    │

SSH

    │

Bastion Host

    │

Private Network

    │

Private Servers
```

Only authenticated users can access the Bastion Host.

---

# 🚨 Why Do We Need a Bastion Host?

Imagine an AWS environment without a Bastion Host.

```
Internet

│

├── SSH → Server A

├── SSH → Server B

├── SSH → Server C

└── SSH → Server D
```

Problems:

- Large attack surface
- Difficult to manage
- Higher security risks
- Multiple public IP addresses
- Increased firewall complexity

Instead, we use a single Bastion Host.

```
Internet

      │

SSH

      │

Bastion Host

      │

Private Servers
```

Benefits:

- One secure entry point
- Better auditing
- Simplified firewall rules
- Improved security
- Reduced public exposure

---

# 🏗 Bastion Host Architecture

Our project architecture looks like this.

```
                 Internet

                      │

                 Public IP

                      │

              Bastion Host (EC2)

                      │

             Private Subnet

                      │

             Amazon EKS Cluster

                      │

              Worker Nodes

                      │

               Kubernetes Pods
```

The Bastion Host is the **only EC2 instance** with a public IP address.

---

# 🌍 Bastion Host Placement

The Bastion Host is always deployed inside a **Public Subnet**.

```
VPC

├── Public Subnet
│      │
│      └── Bastion Host
│
└── Private Subnet
       │
       ├── EKS Nodes
       ├── Databases
       └── Internal Services
```

---

# 🔒 Why Private Subnets?

Private Subnets provide several advantages.

- No direct internet access
- Better security
- Reduced attack surface
- Protection against automated attacks
- Compliance with security standards

Only trusted administrators can access these resources.

---

# 🔑 SSH Authentication

Access to the Bastion Host is secured using **SSH Key Pairs**.

```
Developer

Private Key (.pem)

        │

SSH

        │

Bastion Host

Public Key
```

AWS stores the public key.

Only the matching private key can establish the connection.

---

# 🔥 Bastion Host Security Group

A typical Bastion Host Security Group allows:

| Type | Port | Source |
|------|------|--------|
| SSH | 22 | Your Public IP |

Example:

```
Inbound Rules

Port: 22

Source:

203.xxx.xxx.xxx/32
```

Never allow:

```
0.0.0.0/0
```

unless absolutely necessary.

---

# 🌐 Elastic IP

A Bastion Host normally uses an **Elastic IP (EIP)**.

Benefits:

- Static Public IP
- Easy DNS Mapping
- Reliable SSH Connection
- Firewall Whitelisting

Without an Elastic IP, the public IP changes whenever the instance restarts.

---

# 🖥 Connecting to the Bastion Host

Example:

```bash
ssh -i my-key.pem ec2-user@54.xx.xx.xx
```

After logging in:

```bash
kubectl get nodes
```

or

```bash
helm list
```

can be executed directly from the Bastion Host.

---

# 🛠 Tools Installed on the Bastion Host

In this project, the Bastion Host is configured with the following tools:

| Tool | Purpose |
|------|----------|
| AWS CLI | Manage AWS services |
| kubectl | Kubernetes CLI |
| Helm | Kubernetes Package Manager |
| eksctl | EKS Management |
| Git | Clone repositories |
| Docker CLI | Container operations |
| Terraform (Optional) | Infrastructure management |

This allows administrators to manage the Kubernetes cluster securely.

---

# ☸ Managing Amazon EKS

Since our EKS cluster is private:

```
Developer

      │

SSH

      │

Bastion Host

      │

kubectl

      │

Amazon EKS API

      │

Worker Nodes
```

All Kubernetes administration happens through the Bastion Host.

---

# 📂 Typical Administration Tasks

Using the Bastion Host, administrators can:

- Deploy applications
- Install Helm charts
- Access Kubernetes
- View Pods
- View Services
- Configure Ingress
- Install ArgoCD
- Deploy Monitoring
- Configure Logging
- Troubleshoot cluster issues

---

# 🔐 Bastion Host Best Practices

### Restrict SSH Access

Only allow trusted IP addresses.

---

### Use MFA

Require Multi-Factor Authentication where possible.

---

### Keep Software Updated

Regularly install security updates.

---

### Disable Password Authentication

Use SSH Keys instead.

---

### Rotate SSH Keys

Replace old SSH keys periodically.

---

### Monitor Login Activity

Enable:

- AWS CloudTrail
- CloudWatch Logs

---

### Use IAM Roles

Avoid storing AWS Access Keys on the Bastion Host.

---

### Remove Unused Software

Only install necessary administration tools.

---

# ⚠ Common Mistakes

❌ Public Kubernetes Nodes

❌ SSH Open to Everyone

❌ Using Password Authentication

❌ Multiple Bastion Hosts

❌ Shared SSH Keys

❌ Running Applications on Bastion Host

❌ Using Root User

---

# 🏢 Bastion Host in This Project

Terraform automatically provisions:

- EC2 Bastion Host
- Elastic IP
- Security Group
- IAM Role
- SSH Access

After deployment:

```
Local Machine

      │

SSH

      │

Bastion Host

      │

kubectl

      │

Amazon EKS
```

From the Bastion Host, we install:

- kubectl
- Helm
- AWS CLI
- Git
- ArgoCD CLI (optional)

All cluster management operations are performed from this secure environment.

---

# 🔄 Complete Access Flow

```
Developer Laptop

        │

SSH

        ▼

Bastion Host

        │

AWS CLI

        │

Amazon EKS

        │

kubectl

        │

Worker Nodes

        │

Pods

        │

Microservices
```

---

# 🚀 Advantages of Using a Bastion Host

- Secure administrative access
- Reduced attack surface
- Simplified firewall management
- Private infrastructure protection
- Centralized auditing
- Easy Kubernetes management
- Production-ready architecture
- Better compliance

---

# 💼 Real-World Example

Suppose a DevOps engineer needs to deploy a new version of the application.

Instead of exposing the Kubernetes cluster to the internet, the engineer:

1. Connects to the Bastion Host using SSH.
2. Uses `kubectl` to verify the cluster status.
3. Installs or upgrades applications with Helm.
4. Monitors deployments using ArgoCD.
5. Troubleshoots issues without exposing worker nodes publicly.

This approach significantly improves the security and manageability of the environment.

---

# 📝 Summary

A Bastion Host provides a secure gateway for managing private cloud infrastructure. By limiting administrative access to a single, hardened server, organizations reduce their attack surface and improve operational security.

In this project, the Bastion Host plays a critical role by enabling secure access to the private Amazon EKS cluster while keeping worker nodes and applications isolated from the public internet.

As we continue building the platform, the Bastion Host will serve as the primary administration point for deploying applications, managing Kubernetes resources, and maintaining the cluster.

---

# 🎯 Key Takeaways

- A Bastion Host is a secure entry point into a private network.
- It is deployed in a public subnet with a controlled public IP.
- Private resources remain inaccessible directly from the internet.
- SSH Key authentication is preferred over passwords.
- Security Groups should restrict SSH access to trusted IP addresses.
- The Bastion Host is used to administer the private Amazon EKS cluster.
- Production environments should expose only the Bastion Host—not internal servers.

---

## 📚 Next Chapter

➡️ **07-Amazon-EKS.md**